# OpenSpec documentation sync

When an OpenSpec proposal is archived (moved under `openspec/changes/archive/`), update the repository documentation in the `docs/` directory to reflect the implemented change.

## When to update

- Update `docs/` as part of archiving a proposal, after the change has been implemented and the proposal is moved to `openspec/changes/archive/`.
- Only update documentation for proposals that were implemented. Do not update `docs/` for proposals that are still open, abandoned, or rejected.
- One archive triggers one documentation update pass. Do not re-edit existing `docs/` pages for unrelated proposals.

## Where to update

- Documentation lives in the `docs/` directory at the repository root.
- Documentation follows the Diataxis framework, with four top-level sections: `tutorials/`, `how-to/`, `reference/`, and `explanation/`, plus a root `docs/README.md` index.
- Each section has a `README.md` index that lists its pages. Add new pages to the relevant section index and link them from `docs/README.md` when appropriate.
- Map a change to the section that matches its user-facing outcome:
  - `tutorials/`: guided end-to-end learning material for a new workflow introduced by the change.
  - `how-to/`: concrete operational tasks that the change enables or changes.
  - `reference/`: stable facts, contracts, paths, command shapes, configuration, and module or component listings introduced or changed by the proposal.
  - `explanation/`: only when a proposal changes the system shape enough that the existing architecture pages need a refreshed description of how parts fit together. Do not duplicate design rationale.

## How to update

- Before writing, re-read the existing `docs/` content. Read the affected section's pages, the section `README.md`, and `docs/README.md` to understand what is already documented.
- Integrate the change into existing pages when the proposal modifies or extends an already-documented component. Edit in place, keep the page title and scope stable, and update the description, commands, paths, and facts to match the implemented state.
- Do not create a new page for every proposal. A new page is justified only when the change introduces a distinct capability or topic that does not fit any existing page. When in doubt, extend or refine an existing page instead of adding a new one.
- When a new page is added, link it from the relevant section `README.md` and, if appropriate, from `docs/README.md`.
- Remove or rewrite outdated documentation as part of the same update pass:
  - Delete or rewrite sections, pages, or index entries that describe components, commands, paths, or behaviors that a proposal removed, replaced, or renamed so `docs/` never describes code that no longer exists.
  - Do not leave placeholder pages or stub sections behind after removal. If a page becomes empty after cleanup, remove the file and drop it from the section `README.md` index.
  - If a proposal deprecates a workflow but does not remove it, update the page to mark the current behavior and remove any guidance that no longer applies.

## What to write

- Document the current state of the component after the change is implemented. Documentation describes how the system works now, not how it was built or why a choice was made.
- Do not copy proposal, design, or task content into `docs/`. Reasoning, technology choices, alternatives, and migration notes belong in the OpenSpec artifacts under `openspec/`, not in `docs/`.
- Do not copy requirement/scenario blocks verbatim. Rewrite them as documentation prose that describes the implemented behavior.
- Keep pages focused: one page per concrete topic.

## Language

- Use the same language that the existing `docs/` content is already written in.
- If no `docs/` content exists yet, use English by default unless the user asks for another language.
- Match the tone, heading style, and link conventions of the existing pages

## Verification

- After updating `docs/`, confirm that section `README.md` indexes and `docs/README.md` still list every page that exists and link to pages using the correct relative paths.
- Do not leave dangling links, empty index entries, or pages that describe removed or renamed components.
- Do not create empty section directories. If a section has no pages after the change, do not add or keep a placeholder `README.md` for it unless one already exists.
