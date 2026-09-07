---
name: safe-confluence-editing
description: Safely read and edit Confluence pages through MCP tools while preserving heading hierarchy, neighboring sections, macros, layouts, and concurrent user changes. Use for any Confluence page mutation, especially section updates, multi-step edit batches, or pages with nested headings.
---

# Safe Confluence Editing

Edit Confluence pages conservatively. A successful MCP response proves that Confluence accepted the request, not that the page structure and all neighboring content were preserved.

## Core Safety Model

Treat every edit as a checked transaction:

```text
fresh full read
-> identify exact structural boundaries
-> record version and invariants
-> perform one minimal write
-> read after the write
-> verify boundaries, hierarchy, and invariants
-> continue or stop
```

Never assume that a section-update operation is inherently local. Its effective range depends on heading recognition and hierarchy.

## Start of an Edit Session

Before one mutation or a related series of mutations, fetch the page again. Do this even if the page was read earlier in the conversation because another user may have changed it during the pause.

Use a fresh read to record:

- page ID;
- page title;
- current version;
- raw Confluence storage content;
- the exact target heading text;
- the target heading level;
- the nearest preceding and following sibling headings;
- nested headings inside the target section;
- stable text immediately before and after the intended replacement;
- macros, layouts, tables, anchors, and other Confluence-specific elements in or near the target.

Prefer `confluence_get_page` with `convert_to_markdown: false` for structural inspection. Markdown is useful for understanding prose, but raw storage is authoritative for heading levels and Confluence-specific elements.

One fresh full read is enough at the beginning of a continuous edit series. Do not fetch the entire page again before every write when each previous write has been followed by a successful verification read.

## Mandatory Backup

Before the first write of an edit session, persist the raw storage fetched at the start, together with the page ID, title, and version, to a local backup file. This is a required step, not an optimization.

- The backup file is the known-good copy for recovery: the last verified state, not the initial state, once the session has progressed through successful writes.
- Update or replace the backup after every verified write, so it always reflects the last known good version of the page.
- On failure, restore from this local backup rather than Confluence page history by default. Page history rolls back the whole page and would discard concurrent edits by other users; a restore from the local backup can be applied surgically to the damaged range only. Restoring by any method still requires explicit user approval.
- Keep the backup until the session is finished and the final verification has passed.

## Edit Session Continuity

A series remains continuous only while all of these conditions hold:

- every write is performed by the current workflow;
- every write is followed by a read;
- the returned version advances exactly as expected;
- the next write is based on the verified post-write page state;
- there is no meaningful pause or indication of concurrent editing;
- the page structure remains understood.

After each write, use the newly fetched page as the baseline for the next write.

If the version changed for any reason other than the immediately preceding write, stop the series. Fetch the full current page again and rediscover all boundaries before writing.

Also restart with a fresh full read after a significant pause, a failed write, an ambiguous tool result, a structural mismatch, or loss of confidence in the baseline.

## Determine Section Boundaries

Before calling a section update, establish the intended range explicitly.

For a target heading at level `N`, its section normally extends until the next heading of level `N` or a higher level. Lower-level headings belong to the target section.

Example:

```text
h3 Scenario 7          <- target starts
  content
  h4 Evidence          <- nested, belongs to Scenario 7
  content
h3 Scenario 8          <- target ends before this heading
```

Record these structural invariants before the write:

- the target is present exactly once;
- its level is known;
- all nested heading levels are known;
- the next sibling heading is known;
- the next sibling and later sections are outside the replacement;
- no macro or layout obscures the real boundary.

Do not proceed when the target heading is duplicated, its level is uncertain, the next sibling cannot be identified, or the section crosses an opaque macro or layout boundary.

## Editing Code Blocks

A code block in Confluence storage is not plain text. It is an `<ac:structured-macro ac:name="code">` element whose body is CDATA-wrapped and whose behavior is controlled by parameters such as `language`, `linenumbers`, `collapse`, and `title`.

```xml
<ac:structured-macro ac:name="code">
  <ac:parameter ac:name="language">yaml</ac:parameter>
  <ac:plain-text-body><![CDATA[kubectl get pods -n default]]></ac:plain-text-body>
</ac:structured-macro>
```

The `language` parameter is optional. A block may have no `language` parameter at all, or an empty value, which renders as plain unhighlighted code. Record this as part of the block's state. When replacing the body of such a block, keep it without `language`: do not add, infer, or "upgrade" the language on your own initiative, because that changes how the block renders. Add or change `language` only when the user explicitly asks for it.

When the target of an edit is a code block or a section containing code blocks:

1. Fetch raw storage, never a Markdown conversion, before planning the edit. Markdown rendering hides the macro, its parameters, and the CDATA boundary.
2. Record before the write: the macro parameters (`language`, `linenumbers`, `collapse`, `title`), whether the body is CDATA-wrapped, and the exact body content including leading and trailing whitespace and line breaks.
3. Match a code block structurally, not by remembered text. Locate the exact `<ac:structured-macro ac:name="code">` element by its surrounding context (section heading plus position among sibling blocks) when several blocks look similar.
4. When replacing the body only, preserve the existing macro element and its parameters; submit the new content wrapped the same way as the original (CDATA inside `<ac:plain-text-body>`). Do not convert the block to a fenced Markdown snippet.
5. When the requested change is a parameter change only (for example switching `language`), modify the parameter and keep the body untouched.
6. Do not feed a code block through a Markdown round trip. Converting storage to Markdown and back can drop the `language` parameter, flatten CDATA, alter whitespace inside the block, and replace the macro with a plain code fence that loses syntax highlighting and collapse behavior.
7. Never nest a `<![CDATA[` marker inside existing CDATA content. When the new body contains the `]]>` sequence, split it (for example `]]]]><![CDATA[>`) so the storage stays parseable, or ask the user to confirm a different representation.

Verify after the write:

- the block still renders as a code macro with the same `language` (or the same absence of `language`) and other parameters;
- the body contains exactly the intended content, with intended whitespace and line breaks;
- no extra escaping artifacts (`&lt;`, `&amp;`, duplicated CDATA wrappers) appear in the rendered block;
- neighboring code blocks and their parameters are unchanged.

If the edit cannot preserve the macro structure, stop and explain instead of replacing the code macro with plain Markdown.

## Large Pages

A large page (tens of kilobytes of raw storage or more) changes the cost of every operation but not the safety model. All rules above still apply; this section adjusts the workflow for scale.

- Measure before reading. Fetch page metadata first when the tool allows it (id, title, version, body size) and record the size. The size determines the strategy: what to cache, how to verify, and whether a full-page write is even feasible.
- One full raw read per session remains mandatory regardless of size. Cache it: save the raw storage to a local working file and treat that file as the planning baseline. Plan edits by slicing the file, not by re-fetching the page.
- Never fetch both representations. Do not pull a Markdown conversion of a large page on top of raw storage; it doubles the context cost and adds nothing authoritative.
- Detect truncation. MCP and API responses may silently truncate long bodies. After a full read, check completeness signals before trusting it: the storage parses, macro and section tags balance, and the known final section or footer text is present at the end. A truncated read is not a full read. Do not use it as a baseline; narrow the operation to a section-scoped read, or re-read in a way that returns the full body.
- Prefer section-scoped updates. A full-page write on a large page amplifies every risk in this skill and may exceed tool payload limits. Section updates bound both the blast radius and the payload size.
- Scale post-write verification. A full content diff after each write may be impractical. On a large page, verify: the version advanced as expected, the ordered heading inventory is unchanged outside the target, the target section content is exactly as intended, and control fragments before and after the target survived. On any mismatch or ambiguity, fall back to a full fresh read before writing again.
- When the page exceeds what a single call can return (tool truncation, payload limits), do not attempt a full-page write at all. Propose alternatives to the user: edit via a section-scoped tool with a locally narrowed read, split the page into child pages, or make the change manually. Never write against a baseline that could not be read completely.

## Preserve Heading Hierarchy

Heading levels are part of the page contract. Preserve them exactly unless the user explicitly requests restructuring.

Before and after each edit, verify that:

- every existing `h3` remains `h3`;
- every existing nested `h4` remains `h4` beneath the same parent;
- no nested heading is promoted to sibling level;
- no sibling heading is demoted into the edited section;
- heading order is unchanged outside the requested scope;
- the table of contents still reflects the intended hierarchy.

When replacing a section body, do not include the target heading itself unless the selected tool explicitly requires it. Recreate nested headings at their original levels.

Do not use Markdown heading syntax without first mapping each heading to its raw storage level. A Markdown round trip can flatten or shift hierarchy.

## Choose the Smallest Safe Operation

Prefer operations in this order:

1. Add a comment or inline comment when the requested change is discussion-only.
2. Update one existing section when its exact heading and safe boundary are known.
3. Use raw storage content for a section when macros or structure cannot be represented safely in Markdown.
4. Use a full-page update only when the user explicitly requests a whole-page rewrite or no safer operation can represent the change.

Do not update a large parent section when only one nested subsection needs modification.

Do not use a full-page Markdown update for a local prose change. Converting storage to Markdown and back may lose or alter:

- heading levels;
- macro IDs and parameters;
- layouts and columns;
- anchors;
- table attributes;
- code-block settings;
- unsupported storage elements.

If a safe local mutation cannot be expressed by available tools, stop and explain the risk instead of attempting a destructive approximation.

## Section Update Rules

Before `confluence_update_page_section`:

1. Fetch raw storage from the current baseline.
2. Match the heading text exactly, including case, punctuation, spacing, and capitalization.
3. Confirm the heading occurs once.
4. Confirm its raw level.
5. Identify the next sibling heading that must survive.
6. Record at least one invariant from the section before the target and one from the section after it.
7. Select `markdown` only for plain content that round-trips safely; otherwise use `storage`.
8. Submit only the body beneath the heading, not the heading itself.

Never infer a heading from rendered appearance alone. Two headings may look similar while having different storage levels.

## Multi-Edit Series

For a related series of changes:

1. Perform one fresh full read at the start.
2. Plan edits from deepest or most local sections outward when sections are nested.
3. Apply one mutation at a time.
4. Read the page after every mutation.
5. Verify the write before starting the next mutation.
6. Base the next mutation on the verified new version.

Do not send multiple dependent section updates in parallel. Parallel writes can race on page versions and invalidate section boundaries.

Independent reads may run in parallel. Writes to the same page must be sequential.

## Mandatory Post-Write Verification

After every mutation, fetch the page and verify all of the following:

- the version increased as expected;
- the target content changed exactly as intended;
- the target heading still exists at the original level;
- every nested heading retains its original level and parent;
- the next sibling heading still exists;
- the section after the target is unchanged;
- later headings have not disappeared;
- macros, layouts, tables, anchors, and code blocks near the edit remain intact;
- no duplicate target section was introduced;
- no unrelated content was added, removed, or reformatted.

For high-risk pages, compare an ordered heading inventory before and after the write:

```text
level | exact heading text | occurrence index
```

Also compare control fragments immediately before and after the replaced range.

Do not report success before this verification passes.

## Detecting Concurrent Changes

Track the expected page version throughout the edit session.

Stop and refresh the full baseline when:

- the pre-write version differs from the last verified version;
- the post-write version advances by more than expected;
- content outside the target changed unexpectedly;
- a heading, macro, or neighboring section differs from the recorded baseline;
- Confluence reports a version conflict.

Do not overwrite concurrent changes. Re-read, identify the external modifications, and reapply only the user's requested change against the new state.

## Failure Handling

If a write removes later sections, changes heading levels, or damages macros:

1. Stop all further writes immediately.
2. Record the bad version and the last known good version.
3. Re-fetch raw storage and inspect the exact damage.
4. Do not attempt another speculative section update.
5. Restore only with explicit user approval, using the session backup file (the known-good raw body, see Mandatory Backup) applied to the damaged range; use Confluence page history only when no valid backup exists or the user explicitly prefers it.
6. After restoration, verify the complete heading inventory and neighboring content.

A second write is not a safe automatic response to a failed first write.

## Prohibited Practices

- Do not rely on a page read performed before a break in work.
- Do not treat Markdown as the authoritative representation of page structure.
- Do not assume an `h4` remains nested after conversion.
- Do not assume a section update stops at the visually next heading.
- Do not update a parent section when a child section is the real target.
- Do not perform dependent writes to the same page in parallel.
- Do not continue a series after an unexpected version change.
- Do not trust a successful MCP response without a post-write read.
- Do not report success when neighboring headings or content were not checked.
- Do not silently repair unrelated page content.

## Minimal Safe Workflow

```text
1. Freshly fetch raw page and metadata.
2. Record version and ordered heading inventory.
3. Locate the unique exact target heading.
4. Determine its level, nested headings, and next sibling.
5. Record neighboring control fragments.
6. Choose the smallest safe mutation and content format.
7. Apply one write.
8. Fetch the page again.
9. Verify version, target, hierarchy, next sibling, later sections, and macros.
10. Continue from the verified state or stop on any mismatch.
11. After the final write, perform a final structural and content verification.
```

## Completion Report

State:

- which page and sections changed;
- the starting and ending page versions;
- that the heading hierarchy was checked;
- that following sections and nearby Confluence elements were preserved;
- any content that could not be changed safely.

Keep the report factual. Do not claim that the page was preserved unless the post-write checks were actually performed.
