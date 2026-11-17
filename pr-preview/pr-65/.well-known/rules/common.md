# Common Rules

## Contents

1. [Em Dashes](#1-em-dashes)
2. [Language](#2-language)
3. [OpenSpec Documentation Sync](#3-openspec-documentation-sync)
4. [OpenSpec Artifact Structure](#4-openspec-artifact-structure)
5. [Standalone Specification Structure](#5-standalone-specification-structure)
6. [Narrative Style](#6-narrative-style)
7. [Use Outline When Available](#7-use-outline-when-available)
8. [Daily Description Format](#8-daily-description-format)
9. [Issue Tracker Ticket Creation](#9-issue-tracker-ticket-creation)
10. [Do Not Make Unrequested Changes](#10-do-not-make-unrequested-changes)
11. [Do Not Run Heavy Operations](#11-do-not-run-heavy-operations)
12. [Kubernetes Operations Safety](#12-kubernetes-operations-safety)
13. [Secret Files](#13-secret-files)
14. [Prefer OpenTofu](#14-prefer-opentofu)
15. [Commit Messages](#15-commit-messages)
16. [Do Not Post to External Chats or Comments](#16-do-not-post-to-external-chats-or-comments)
17. [Explicit Overrides of Repository Instructions](#17-explicit-overrides-of-repository-instructions)

## 1. Em Dashes

Never use the em dash (U+2014) or the en dash (U+2013) in any output, including prose, code comments, commit messages, and documentation. Use the ASCII hyphen-minus (`-`, U+002D) instead. For asides, prefer commas, parentheses, or a spaced hyphen (` - `). This applies to every language and file type. This rule takes precedence over other rules, including requirements to preserve verbatim text or output; replace such dashes with a hyphen-minus.

## 2. Language

This rule applies to artifacts such as documentation, specifications, OpenSpec changes, daily updates, tracker tickets, and Outline documents. Reply in the current conversation in the language of the user's message. Commit messages are always written in English, see [Commit Messages](#15-commit-messages).

Write in English by default. When existing artifacts of the same kind already use another language, write in that language. Same-kind artifacts include adjacent documentation for the same audience, neighboring daily updates, adjacent OpenSpec changes and specs, related specifications, and tickets in the same tracker project. A project style guide that sets a language counts as an established language. For example, if neighboring daily updates are written in Russian, write new daily updates in Russian. An explicit user language request takes precedence. If the language cannot be determined reliably, for example when neighboring artifacts use different languages, ask the user.

Keep one primary language within a document and across closely related artifacts, such as the proposal, specs, and design of one OpenSpec change, unless multilingual content is requested.

Prefer established terms of the selected language; retain foreign terms when they are more precise, have no established equivalent, or are part of an exact interface. When both forms help, introduce them together once and use the preferred term consistently. Do not translate exact technical identifiers, including product and tool names, commands and options, paths and filenames, variables and configuration values, API fields, enums, and events, code, package, image, and repository names, external labels, and verbatim output. Keep headings in the selected language unless they are exact identifiers or a format prescribes fixed headings, such as the daily update format or headings required by the OpenSpec schema. Translate user-facing taxonomy when an established translation exists, but preserve machine-facing paths and keys. Preserve meaning when translating and do not add requirements or claims. Before finalizing, check language consistency, terminology, headings, and identifier fidelity.

## 3. OpenSpec Documentation Sync

When an implemented OpenSpec proposal is archived under `openspec/changes/archive/`, update repository documentation in `docs/` to reflect the implemented change. This update is part of the archive operation and does not need a separate request. Do not update docs for open, abandoned, or rejected proposals. One archive triggers one documentation update pass; do not re-edit pages for unrelated proposals.

Apply this rule only when `docs/` already exists and follows the Diataxis structure described below. If `docs/` is missing or uses another structure, skip this rule as if `docs/` did not exist, and do not create `docs/` or its sections. If it is unclear whether the existing `docs/` follows this structure, ask the user.

Documentation uses Diataxis sections: `tutorials/`, `how-to/`, `reference/`, and `explanation/`, plus `docs/README.md`. Each section has a `README.md` index. Map the change to its user-facing outcome: tutorials are guided workflows, how-to pages are operational tasks, reference pages are stable facts and contracts, and explanation pages describe architecture only when the system shape changes. Read the existing affected pages and indexes before editing. Extend existing pages where possible; create a new page only for a distinct topic. Update or remove outdated content and index entries, and do not leave placeholders, dangling links, or empty section directories.

Document how the component works now, not how it was built or why a design was chosen. Do not copy proposal, design, task, requirement, or scenario text verbatim into docs. Keep each page focused. Use the tone and link conventions of adjacent docs, and choose the language according to [Language](#2-language). After editing, verify all indexes and relative links.

## 4. OpenSpec Artifact Structure

Use this rule for artifacts in an OpenSpec `spec-driven` change. `tasks.md` is outside the content plan because tasks derive from the approved design. The OpenSpec format and the active schema take precedence over the structures below.

### Responsibilities and General Rules

- `proposal.md` explains why the change is needed, what it includes, and what it affects.
- `specs/<capability>/spec.md` defines observable, normative, verifiable behavior.
- `design.md` explains how requirements will be implemented.
- Do not duplicate material across artifacts; keep authoritative details in the responsible artifact.
- Treat sections as optional unless the active schema requires them. Omit inapplicable sections and tell the user in chat which planned sections were omitted.
- Do not invent facts, requirements, targets, examples, decisions, stakeholders, or technical details. If required information is unavailable, ask the user to investigate or confirm it.
- Choose the language according to [Language](#2-language). Keep a change's proposal, specs, and design in one language unless asked otherwise.

### Content Mapping

| Content | Artifact and location |
| --- | --- |
| Business context, drivers, current solution, problems | `proposal.md`, `## Why` |
| Stakeholders | `proposal.md`, `## Stakeholders` |
| Goals, scope, non-goals | `proposal.md`, `## What Changes` |
| Capabilities | `proposal.md`, `## Capabilities` |
| Affected users, systems, data, operations | `proposal.md`, `## Impact` |
| Functional behavior, domain model, constraints, external contracts, functional errors, quality requirements, scenarios | `specs/<capability>/spec.md` |
| Architecture, component changes, technical data model, technical contracts, interfaces, migration, technical errors, risks | `design.md` |
| Glossary and useful links | `design.md`, `## Glossary and References` |

### Proposal Structure

Use these headings for the sections you include and put each fact under the appropriate heading:

```markdown
# <Change Title>

## Why
### Initial Requirements and Drivers
### Current Solution
### Business Problems

## Stakeholders

## What Changes
### Goals
### Scope
### Non-Goals

## Capabilities
### New Capabilities
### Modified Capabilities

## Impact
### Users and Business Processes
### Systems and Components
### Data and Integrations
### Operational Impact
```

Keep current behavior that motivates the change in `Current Solution`; do not put architecture diagrams, storage details, or solution choices there. Use `None` for a required capability subsection without entries. Use Impact to identify affected areas, not repeat requirements or design details.

### Design Structure

Use these headings for the sections you include and keep detailed solution sections between `Goals / Non-Goals` and `Decisions`:

```markdown
# <Change Title> Design

## Glossary and References
### Glossary
### References

## Context
### Problem Context
### Existing Technical Constraints

## Goals / Non-Goals
### Goals
### Non-Goals

## Architecture and Component Boundaries
### Architecture Overview
### Component Responsibilities
### Component Boundaries
### Dependencies and Data Flows

## Component Changes
### <Component Name>

## Data Model
### Entities and Relationships
### Persistence Model
### Ownership and Lifecycle
### Indexes and Constraints

## Technical Contracts
### APIs
### Events and Messages
### External Integrations
### Internal Contracts

## Interfaces
### User Interface
### Command-Line Interface
### Internal Interfaces

## Error Handling
### Error Categories
### Error Propagation and Mapping
### Retry and Recovery
### Logging and Diagnostics

## Decisions
### Decision: <Title>

## Risks / Trade-offs

## Migration Plan
### Rollout
### Data Migration
### Backward Compatibility
### Rollback

## Open Questions
```

Use design context only for technical context and constraints needed to understand the solution.

### Capability Specifications

For a main spec under `openspec/specs/<capability>/spec.md`, use a purpose and requirements structure. Write requirements as `The system SHALL ...` and put each relevant user, integration, failure, and boundary scenario directly under its requirement, using `#### Scenario:` with GIVEN/WHEN/THEN statements.

For a delta spec under `openspec/changes/<change-name>/specs/<capability>/spec.md`, include only applicable operation sections:

```markdown
# <Capability Name> Delta Specification

## ADDED Requirements
### Requirement: <New Requirement>
The system SHALL ...
#### Scenario: <Scenario Name>
- **GIVEN** ...
- **WHEN** ...
- **THEN** ...

## MODIFIED Requirements
### Requirement: <Existing Requirement Name>
<Complete updated requirement with retained scenarios.>

## REMOVED Requirements
### Requirement: <Removed Requirement Name>
**Reason:** <Reason for removal.>
**Migration:** <Consumer transition.>

## RENAMED Requirements
- `FROM: <Old Requirement Name>`
- `TO: <New Requirement Name>`
```

Use stable normative behavior in specs, including measurable non-functional requirements. Keep domain entities, relationships, states, and invariants in specs; persistence, indexes, ownership, and migration mechanisms in design. Specs state what consumers observe, while design gives technical paths, schemas, protocols, and implementation details. Specs describe visible error behavior; design explains detection, mapping, retry, recovery, logging, and diagnostics. Compatibility guarantees belong in specs; rollout and rollback mechanisms belong in design. Do not add category headings between an operation heading and its Requirement blocks unless the installed validator accepts them. Before finalizing, preserve schema-required headings and run the repository's OpenSpec validation command.

## 5. Standalone Specification Structure

Use this guidance for product, business, system, or solution specifications outside OpenSpec. Do not apply it to OpenSpec artifacts.

### General Principles

- Include every section of the outline below. When a section does not apply to the current task, retain its heading and explicitly state that it is not applicable, for example `Not applicable to this task`, using the document's selected language. Do not omit sections or leave them empty or ambiguous.
- Never invent facts, targets, examples, decisions, or assumptions. Distinguish known facts, confirmed decisions, assumptions, and open questions. Ask for missing context required to write an accurate specification.
- Keep each fact authoritative in one place. Write testable requirements and use consistent identifiers when useful, such as `P-001`, `FR-001`, `NFR-001`, and `BR-001`.
- Choose the language according to [Language](#2-language) and keep one language throughout unless multilingual content is requested.

### Outline

```markdown
# <Specification Title>
## 1. Glossary and Useful Links
## 2. Business Context
### 2.1 Initial Requirements and Drivers
### 2.2 Current Solution
### 2.3 Business Problems
### 2.4 Stakeholders
### 2.5 Goals
### 2.6 Scope and Non-Goals
## 3. Functional Requirements
### 3.1 General Functional Requirements
### 3.2 Domain Model
### 3.3 Business Rules and Constraints
### 3.4 Contracts and Integrations
### 3.5 Error Handling
## 4. Non-Functional Requirements
### 4.1 Performance
### 4.2 Reliability and Availability
### 4.3 Security
### 4.4 Compatibility
### 4.5 Observability and Diagnostics
### 4.6 Operational Constraints
## 5. User Scenarios
## 6. Solution Specification
### 6.1 Architecture and Component Boundaries
### 6.2 Changes by Component
### 6.3 Data Model
### 6.4 APIs and Other Technical Contracts
### 6.5 Interfaces
### 6.6 Migration and Backward Compatibility
### 6.7 Error Handling
### 6.8 Risks and Trade-offs
```

### Business Context

Record confirmed drivers such as stakeholder requests, incidents, defects, obligations, research, or user feedback, with sources where available. Describe only verified current behavior, including relevant workflows, responsibilities, inputs, outputs, integrations, limitations, and workarounds. State problems and their consequences without embedding a preferred solution; include affected parties and evidence when available. Identify stakeholders by role and responsibility without assigning unconfirmed ownership or approval. Goals describe specific, objectively assessable outcomes that address the problems. Define scope and non-goals from confirmed boundaries only.

### Functional Requirements

Define observable, testable, unambiguous behavior independently of implementation unless a technical constraint is confirmed. Establish actor, trigger/precondition, required behavior, result, exceptions, and boundaries as relevant. Include domain meaning, attributes, relationships, lifecycle, and invariants. State validation, permissions, calculations, ordering, uniqueness, concurrency, and invalid transitions, including their outcomes. Describe contracts through accepted/returned/emitted information, semantics, triggers, consistency/idempotency, authoritative data, and dependency failure behavior. Define user- and integration-visible outcomes for invalid input, missing/conflicting data, permission problems, unavailable dependencies, partial completion, and retries.

### Non-Functional Requirements

Make quality requirements measurable, with workload/environment and verification conditions. Do not invent unagreed targets. Cover performance (latency, throughput, volume, resources, degradation), reliability (availability, recovery, consistency, durability, retries, backups), security (authentication, authorization, data protection, audit, privacy, retention, abuse prevention), compatibility (supported versions, formats, deprecation, legacy behavior), observability (logs, metrics, traces, diagnostics, alerts, retention/redaction), and operational constraints (deployment, infrastructure, configuration, release, residency, ownership) only when reliable context exists. Do not claim an SLA/SLO/RTO/RPO or security/compliance policy without confirmation.

### User Scenarios

Scenarios complement rather than replace requirements. Include actor, participating systems, scope, preconditions, trigger, main and failure flows, result, and related requirement identifiers when known. Do not invent user behavior or happy paths; ask for confirmation when the workflow is unknown.

### Solution Specification

Describe only the design that satisfies confirmed requirements. Include architecture, component responsibilities and boundaries, dependencies, and data flows when known. For each affected component, describe current responsibility, changes, new interfaces/configuration, unchanged behavior, and operational impact. Specify technical fields, types, keys, constraints, persistence, lifecycle, versioning, and migration only when established. Document concrete APIs/contracts including schemas, auth, validation, versioning, idempotency, ordering, timeout, and retry only when known. Describe UI/CLI/internal interfaces and relevant states/accessibility/localization. Explain migration rollout, data/config changes, compatibility, validation, rollback, and cleanup; do not claim migration is unnecessary without confirming persisted data, external consumers, and deployed behavior. Technical error handling may cover classification, propagation, mapping, retry/recovery, transactions, logging, redaction, and ownership. Record only material risks and trade-offs, with mitigation or accepted consequence when known.

### Completeness Review

Before finalizing, confirm that every section of the outline is either completed or explicitly marked as not applicable to the current task. Do not leave empty or ambiguous sections. Confirm no unsupported claims were invented, requirements are testable, goals address documented problems, scenarios trace to requirements, design meets requirements without silently adding scope, error handling is consistent, and compatibility aligns with migration and rollback. Identify missing information needed for implementation or acceptance.

## 6. Narrative Style

Use this style for documentation and other explanatory prose, unless a repository- or project-specific style guide defines another style.

Write for a practicing technical specialist rather than an academic or marketing audience. Use direct, natural engineering language and explain the subject as a sequential walkthrough. Use conversational transitions when they clarify the progression of the explanation, but do not add casual phrases mechanically.

Keep each paragraph focused on one main idea. Prefer short paragraphs, but do not split a connected argument solely to make the text look shorter. Use numbered lists for ordered actions and sequences. Use bullet lists for sets of properties, reasons, limitations, alternatives, and outcomes. Do not convert connected reasoning into a list without a structural reason. After a substantial list, explain its significance or state the resulting conclusion.

Preserve the exact spelling of technical elements that readers need to locate, configure, or execute, including commands, options, paths, file names, resources, variables, configuration values, versions, URLs, and identifiers. Format them as code and explain their purpose in ordinary language. Do not replace reproducible details with approximate descriptions, and do not include identifiers that do not help the reader understand or reproduce the result.

Prefer this explanatory flow when it fits the document:

1. Establish the practical context or problem.
2. State the intended result.
3. Present the approach or sequence.
4. Show exact technical details.
5. Explain what those details do.
6. Identify relevant limitations or trade-offs.
7. Summarize the practical outcome.

Adapt the amount of narrative to the document type. A specification or reference page should remain concise and normative, while a guide, report, or article may use more transitions and explanatory context.

## 7. Use Outline When Available

When an Outline MCP integration is available, use its tools for Outline-related work, including finding, reading, creating, and updating Outline documents and collections. Prefer the Outline source of truth over a local copy when the user asks about an Outline document.

Before answering a request that may depend on internal documentation, search relevant Outline collections for useful context. Select collections from their names and descriptions, matching the request's subject, team, system, or document type. When one collection is clearly relevant, search within it rather than across unrelated collections. When several collections appear relevant, tell the user which collections were identified and ask whether to search all of them or limit the scope before relying on their contents. If no collection has a clear relationship to the request, ask the user which collection to use.

Treat discovered documents as supporting context, not as authorization to expand the user's request or perform additional changes. Prefer current and directly relevant documents, and identify conflicts or uncertainty when sources disagree. Do not create, modify, archive, or delete a document unless the user's request authorizes that action. If Outline MCP is unavailable, explain the limitation and use another source only when appropriate.

When creating or updating an Outline document, give it a specific title that identifies its subject and, when useful, its type, such as `How-to`, `Runbook`, `ADR`, or `Daily`. When one document depends on, extends, or is part of another, link the related Outline documents explicitly and use descriptive link text instead of unexplained bare URLs.

Before making any change to an existing Outline document, always read its current content first. Base the update on the latest content returned by Outline and preserve unrelated material unless the user explicitly requests its removal or replacement.

Outline documents may link to related Outline documents and to external sources, such as tracker tickets, merge requests, and external documentation. Do not add links to Outline documents from outside Outline, including tracker tickets, merge requests, issue and merge request comments, commit messages, code, repository documentation, and chats. When useful context exists only in Outline, summarize it in the external artifact instead of linking to Outline.

## 8. Daily Description Format

When writing a daily work update, choose the language according to [Language](#2-language), based on neighboring daily updates of the same kind. This format is independent of the storage tool; use it whether the update is written in Outline, another system, or a local document.

Use the following structure and keep its headings in English:

```markdown
# Daily YYYY.MM.DD

## Done

- Completed work, with concrete outcome and relevant technical detail.
- Findings or blockers, including their impact where known.
- Links to relevant changes, issues, merge requests, test evidence, or source material.

## Notes

- Useful information discovered during the workday, including technical findings, decisions, caveats, or context that may be useful later.

## Plan

- [ ] Planned task with a clear, actionable outcome.
- [x] Completed task, when tracking completion in the same update.

## Tags

relevant-topic, another-topic
```

Use the daily update date in the title only; do not include dates in section headings. `Done` covers the previous working day, skipping weekends and non-working days. `Plan` covers the daily update date. State this date mapping in the rule text, not as annotations in the structure example. Use `Notes` for useful information discovered during the workday, such as technical findings, decisions, caveats, or context that may be useful later. Report material work and outcomes concisely, distinguishing completed work from investigation and planned work. Use checklist boxes for plans when tracking status; link supporting artifacts inline or immediately below the relevant item.

Always include every section, including `Tags`. When a section has no content, put a single `-` in it instead of omitting it or adding filler.

When a plan item is transferred to the tracker, the item may be checked only after a tracker ticket has been created for the remaining work and linked from the plan item. Checking the item records that the work was transferred to the tracker, not that the long-term work itself is complete.

## 9. Issue Tracker Ticket Creation

Create tracker tickets only when the user asks to create a ticket or to transfer work to the tracker. When you identify follow-up work without such a request, propose a ticket in the current conversation and do not create it.

Before creating a ticket, check whether an existing ticket already covers the work. If one exists, reference it instead of creating a duplicate, and edit it only when the user explicitly requests it. Otherwise create a new ticket and assign it to the current user by default. If the tracker supports an explicit assignee, set the current user as the assignee rather than leaving the ticket unassigned.

Structure the ticket description with these sections, in this order:

### Description

State the problem or need, relevant context, desired goal, and a concise summary of the work in scope. Keep implementation details out unless they are already known or required to explain the scope.

### DoD

List concrete deliverables and completion conditions. Include implementation, tests, documentation, rollout, migration, or operational verification when applicable. Do not mark a deliverable complete merely because investigation or ticket creation is complete.

### AC

Define specific, observable, and verifiable outcomes required for acceptance. Include relevant behavior, boundaries, failure cases, and edge cases when known. Criteria must describe what must be true, not only which tasks someone should perform.

### Related links

Link related tracker tickets and relevant technical artifacts, such as source files, pull requests, specifications, dashboards, or external documentation. Do not add links to Outline documents, see [Use Outline When Available](#7-use-outline-when-available). If useful context exists only in Outline, summarize the relevant information in the ticket instead.

## 10. Do Not Make Unrequested Changes

Do not write code, edit files, or take system-changing actions unless the user explicitly asks. Questions, discussions, and descriptions of problems are not implementation requests. If intent is ambiguous, ask before changing anything. Reading and searching are allowed.

Before asking the user to approve a change or state-changing operation, show the exact command you intend to execute, including its arguments, target, and working directory when relevant. If several commands require approval, list them in execution order. For changes made through tools rather than shell commands, show the tool or API operation and the relevant parameters or proposed diff. Never expose secret values; use variable references or secret placeholders instead. Ask for approval only after presenting these details. This requirement does not introduce additional approval steps for actions the user has already authorized.

An action that another rule defines as part of a requested operation counts as requested, for example the documentation update in [OpenSpec Documentation Sync](#3-openspec-documentation-sync) when an OpenSpec change is archived.

## 11. Do Not Run Heavy Operations

Do not run heavy or long-running operations, or operations that change external systems, unless the user explicitly requests the exact operation. This includes `tofu plan`, `tofu apply`, `terraform plan`, `terraform apply`, `dagger call` and other Dagger runs, including tests run through Dagger, end-to-end tests, commands that start services or containers, deployments, changes to clusters or cloud resources, and `git push`. Do not infer permission from a general request to implement or validate a change. Editing local files and creating local commits are governed by [Do Not Make Unrequested Changes](#10-do-not-make-unrequested-changes), not by this rule.

You may run read-only investigation and diagnostic commands by default, including commands that connect to external systems, when they are necessary to understand or troubleshoot the user's request. This includes Kubernetes inspection such as `kubectl get`, `kubectl describe`, `kubectl logs`, and `kubectl get events`, subject to the Kubernetes safety rules below. You may also run linters, unit tests, OpenSpec validation, and other fast, self-contained checks without a separate request; they are not heavy operations. When a requested workflow requires a prohibited operation, tell the user the exact command and explain that explicit approval is required before running it.

## 12. Kubernetes Operations Safety

These rules apply to all `kubectl` and Kubernetes MCP interactions, in local, staging, and production clusters.

- Before the first Kubernetes command or tool call in a task and before every mutation, run `kubectl config current-context` or the equivalent context tool and state the result to the user. Never switch context implicitly; get explicit approval for every context switch.
- Specify the namespace explicitly for every namespaced resource. Never rely on the current context's default namespace. Ask for a namespace if the user did not specify one. Do not pass a namespace for cluster-scoped resources, such as nodes, namespaces, CRDs, ClusterRoles, and PersistentVolumes. Use `--all-namespaces` (`-A`) only for read-only commands.
- Read a resource with `kubectl get`, `kubectl describe`, or an equivalent tool before modifying it. Prefer `kubectl diff -f <file>` or `kubectl apply --dry-run=server -f <file>` before apply. Show non-trivial diffs. Read and review generated manifests in full and get user confirmation before applying to production.
- Never run `kubectl delete` without explicit user intent. Never use `--force`, `--grace-period=0`, `--cascade=force`, or forced eviction without approval of that exact flag for that exact operation.
- Never automatically create, modify, read, or rotate Secrets. Each Secret operation requires explicit user instruction.
- After mutating a workload, such as a Deployment, StatefulSet, or DaemonSet, inspect `kubectl rollout status <resource>` or equivalent and do not report success until it is confirmed. After mutating other resources, confirm the result with `kubectl get` or `kubectl describe`. On failure, inspect events, resource details, and logs before any further mutation.

## 13. Secret Files

Secret files are `.envrc`, `.env*`, `*.tfvars` with credentials, `terraform.tfstate*`, everything under `.bitwarden/`, and any file that the user marks as holding secrets. Whether a `*.tfvars` or `.env*` file holds credentials cannot be checked without reading it, so treat every matching file as secret unless the user confirms that it holds none.

1. Never print a secret file or any part of it, with any tool: no `cat`, `head`, `tail`, `less`, `grep`, `sed`, `awk`, `diff`, `git diff`, a read or view tool, or an editor. Redaction filters do not make this allowed, because a filter can miss a form of a name or value.
2. Load a secret file only for the command that needs it, in a subshell: `( set -a; . ./.envrc; set +a; tofu plan ... )`. Loading secrets does not authorize the command itself; heavy operations still follow [Do Not Run Heavy Operations](#11-do-not-run-heavy-operations).
3. To learn which variables a file defines, print names only, from a clean environment: `env -i bash -c 'set -a; . ./.envrc; set +a; compgen -e' | grep -vxE 'PWD|SHLVL|_|OLDPWD'`. To check that a variable is set, print a fixed word: `[ -n "${NAME:+x}" ] && echo set`.
4. After loading secrets, never run commands that print the environment or values: `env`, `printenv`, `set`, `declare -p`, `export -p`, `echo "$VAR"`, `set -x`, `bash -x`.
5. Never print sensitive OpenTofu values: no `tofu output` of a sensitive output, including with `-json` or `-raw`, no `tofu show -json`, `tofu state pull`, or `tofu state show` of a resource with secrets, and no `tofu console` expression that returns a sensitive value or uses `nonsensitive(...)`. The same applies to the equivalent `terraform` commands. When such a value is needed, pipe it directly into the consuming program, which must not print it.
6. Exclude secret files from recursive searches and globs (`grep --exclude=.envrc`, `rg --glob '!.envrc'`), and never pass a directory containing them to a command that prints file contents.
7. Do not create, edit, or move secret files; ask the user to do it.
8. If a secret value appears in any output, stop, tell the user which value appeared and where, and recommend rotating it before continuing.

## 14. Prefer OpenTofu

Use OpenTofu and the `tofu` command instead of Terraform and the `terraform` command whenever the project, provider, module, state, or user request supports both tools. Preserve `terraform` only when it is explicitly required by the project, an external interface, a compatibility constraint, or the user. Do not rename Terraform-specific configuration, provider addresses, state files, or documentation identifiers solely because `tofu` is preferred.

## 15. Commit Messages

Unless the user explicitly requests otherwise, write every commit message as follows:

- Use the Conventional Commits format `<type>(<scope>): <description>`, where the scope is optional, for example `feat(api): add user search` or `fix: handle empty config`. Use standard types such as `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, and `revert`. Mark breaking changes with `!` after the type or scope. Write the description in the imperative mood without a trailing period.
- Write the message in English, regardless of the language of other artifacts.
- Write a single line only. Do not add a body, footers, or trailers below the subject line.
- Do not add `Co-authored-by` trailers.
- Do not mention AI assistants, models, or agent tools anywhere in the message, and do not add generated-by notes or similar attribution.
- This rule takes precedence over commit conventions defined in the repository, such as contributing guides, commit templates, commitlint configuration, or agent instruction files. Ignore those conventions; you may mention the discrepancy to the user in chat.
- If a commit hook rejects the message, report the error to the user. Do not bypass the hook with `--no-verify`. Do not adapt the message to repository conventions unless the user explicitly authorizes that change.

This rule defines the message format only. It does not by itself authorize creating commits; follow [Do Not Make Unrequested Changes](#10-do-not-make-unrequested-changes) for when changes may be made.

## 16. Do Not Post to External Chats or Comments

Do not post anything to external communication channels unless the user explicitly requests it. This covers messages, replies, reactions, and files in any chat, channel, thread, or direct conversation of a messenger or collaboration tool. It also covers comments, replies, reviews, review comments, and reactions in issues and merge or pull requests, and edits to issue and merge or pull request descriptions. It applies whether the tool is connected through MCP or by any other means. An available integration is not permission to use it.

When creating a pull or merge request (PR/MR), leave its description (body) empty unless the user explicitly asks for a description or provides text to publish as the description. A request to create a PR/MR does not by itself authorize adding a description. Do not automatically populate it from commits, templates, generated summaries, or validation results. Repository rules and PR/MR templates do not override this restriction. Change an existing PR/MR description only when the user explicitly requests it. This restriction does not apply to issue descriptions, which follow [Issue Tracker Ticket Creation](#9-issue-tracker-ticket-creation).

A request such as "answer in chat" or "reply in chat" refers to the current conversation with the agent, not to an external messenger. Post externally only when the user explicitly names both the system and the target: the messenger and the chat, or the tracker or code hosting and the specific issue or merge request. For example, "post this to the `#infra` channel in Slack" or "comment on MR !123 in GitLab". If either is not named, reply in the current conversation instead; if the intent to post externally is clear but the target is ambiguous, ask before sending anything.

## 17. Explicit Overrides of Repository Instructions

When the user explicitly asks to ignore or override instructions inside a repository, including `AGENTS.md`, other agent instruction files, contributing guides, and repository-local conventions, follow that request within its stated scope. Do not treat those repository instructions as an absolute prohibition against an action the user explicitly authorized. Keep unrelated instructions in effect, and do not infer an override from a general request to complete a task. If the scope of the override is unclear, ask the user before acting.

This exception applies only to repository instructions. It does not override higher-priority system or developer instructions.
