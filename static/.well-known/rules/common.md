# Common Rules

## Contents

1. [Em Dashes](#1-em-dashes)
2. [OpenSpec Documentation Sync](#2-openspec-documentation-sync)
3. [OpenSpec Artifact Structure](#3-openspec-artifact-structure)
4. [Standalone Specification Structure](#4-standalone-specification-structure)
5. [Documentation Language Style](#5-documentation-language-style)
6. [Use Outline When Available](#6-use-outline-when-available)
7. [Daily Description Format](#7-daily-description-format)
8. [Issue Tracker Ticket Creation](#8-issue-tracker-ticket-creation)
9. [Do Not Make Unrequested Changes](#9-do-not-make-unrequested-changes)
10. [Do Not Run Heavy Operations](#10-do-not-run-heavy-operations)
11. [Kubernetes Operations Safety](#11-kubernetes-operations-safety)
12. [Prefer OpenTofu](#12-prefer-opentofu)

## 1. Em Dashes

Never use the em dash (U+2014) or the en dash (U+2013) in any output, including prose, code comments, commit messages, and documentation. Use the ASCII hyphen-minus (`-`, U+002D) instead. For asides, prefer commas, parentheses, or a spaced hyphen (` - `). This applies to every language and file type.

## 2. OpenSpec Documentation Sync

When an implemented OpenSpec proposal is archived under `openspec/changes/archive/`, update repository documentation in `docs/` to reflect the implemented change. Do not update docs for open, abandoned, or rejected proposals. One archive triggers one documentation update pass; do not re-edit pages for unrelated proposals.

Documentation uses Diataxis sections: `tutorials/`, `how-to/`, `reference/`, and `explanation/`, plus `docs/README.md`. Each section has a `README.md` index. Map the change to its user-facing outcome: tutorials are guided workflows, how-to pages are operational tasks, reference pages are stable facts and contracts, and explanation pages describe architecture only when the system shape changes. Read the existing affected pages and indexes before editing. Extend existing pages where possible; create a new page only for a distinct topic. Update or remove outdated content and index entries, and do not leave placeholders, dangling links, or empty section directories.

Document how the component works now, not how it was built or why a design was chosen. Do not copy proposal, design, task, requirement, or scenario text verbatim into docs. Keep each page focused. Use the language, tone, and link conventions of adjacent docs; use English by default if no docs exist. After editing, verify all indexes and relative links.

## 3. OpenSpec Artifact Structure

Use this rule for artifacts in an OpenSpec `spec-driven` change. `tasks.md` is outside the content plan because tasks derive from the approved design.

### Responsibilities and General Rules

- `proposal.md` explains why the change is needed, what it includes, and what it affects.
- `specs/<capability>/spec.md` defines observable, normative, verifiable behavior.
- `design.md` explains how requirements will be implemented.
- Do not duplicate material across artifacts; keep authoritative details in the responsible artifact.
- Treat sections as optional unless the active schema requires them. Omit inapplicable sections and tell the user in chat which planned sections were omitted.
- Do not invent facts, requirements, targets, examples, decisions, stakeholders, or technical details. If required information is unavailable, ask the user to investigate or confirm it.
- Use the language of adjacent OpenSpec changes/specs; otherwise continue the language already used, defaulting to English. An explicit user language request takes precedence. Keep a change's proposal, specs, and design in one language unless asked otherwise.

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

Preserve the standard headings and put each fact under the appropriate heading:

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

Preserve conventional headings and include detailed solution sections between `Goals / Non-Goals` and `Decisions` as applicable:

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

Omit optional interface or migration sections that do not apply. Use design context only for technical context and constraints needed to understand the solution.

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

## 4. Standalone Specification Structure

Use this guidance for product, business, system, or solution specifications outside OpenSpec. Do not apply it to OpenSpec artifacts.

### General Principles and Language

- Treat every section as optional. Include it only when reliable context exists and it helps explain or verify the change.
- Omit inapplicable sections without empty headings, and tell the user which planned sections were omitted.
- Never invent facts, targets, examples, decisions, or assumptions. Distinguish known facts, confirmed decisions, assumptions, and open questions. Ask for missing context required to write an accurate specification.
- Keep each fact authoritative in one place. Write testable requirements and use consistent identifiers when useful, such as `P-001`, `FR-001`, `NFR-001`, and `BR-001`.
- Use the language of adjacent specifications for the same audience. Otherwise continue the document's language, defaulting to English. An explicit user request takes precedence. Keep one language throughout unless multilingual content is requested.

### Recommended Outline

Use only relevant sections from this outline:

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

Before finalizing, remove empty sections and disclose inapplicable sections. Confirm no unsupported claims were invented, requirements are testable, goals address documented problems, scenarios trace to requirements, design meets requirements without silently adding scope, error handling is consistent, and compatibility aligns with migration and rollback. Identify missing information needed for implementation or acceptance.

## 5. Documentation Language Style

Follow a repository- or project-specific style guide first. Otherwise use the language of adjacent documentation for the same audience; if none establishes a language, continue the document's language, defaulting to English. Explicit user language requests override defaults. Keep one primary prose language unless multilingual content is requested.

Write explanatory prose in the selected language. Prefer established local terms; retain foreign terms when more precise, established without a clear equivalent, or part of an exact interface. When both forms help, introduce them together once and use the preferred term consistently. Preserve exact technical identifiers, including product/tool names, commands/options, paths, filenames, variables/configuration values, API fields/enums/events, code/package/image/repository names, external labels, and verbatim output. Do not translate commands, code, paths, values, schemas, or factual output. Keep headings in the document language unless an exact identifier is required. Translate user-facing taxonomy when an established translation exists but preserve machine-facing paths and keys. Preserve meaning when translating and do not add requirements or claims. Before finalizing, check language consistency, terminology, headings, and identifier fidelity.

### Narrative Style

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

## 6. Use Outline When Available

When an Outline MCP integration is available, use its tools for Outline-related work, including finding, reading, creating, and updating Outline documents and collections. Prefer the Outline source of truth over a local copy when the user asks about an Outline document.

Before answering a request that may depend on internal documentation, search relevant Outline collections for useful context. Select collections from their names and descriptions, matching the request's subject, team, system, or document type. When one collection is clearly relevant, search within it rather than across unrelated collections. When several collections appear relevant, tell the user which collections were identified and ask whether to search all of them or limit the scope before relying on their contents. If no collection has a clear relationship to the request, ask the user which collection to use.

Treat discovered documents as supporting context, not as authorization to expand the user's request or perform additional changes. Prefer current and directly relevant documents, and identify conflicts or uncertainty when sources disagree. Do not create, modify, archive, or delete a document unless the user's request authorizes that action. If Outline MCP is unavailable, explain the limitation and use another source only when appropriate.

When creating or updating an Outline document, give it a specific title that identifies its subject and, when useful, its type, such as `How-to`, `Runbook`, `ADR`, or `Daily`. When one document depends on, extends, or is part of another, link the related Outline documents explicitly and use descriptive link text instead of unexplained bare URLs.

When a tracker ticket is relevant to an Outline document, link to the ticket from the Outline document when useful. Do not add links from Outline documents to other Outline documents. This restriction applies to all Outline documents and all link types, not only tracker tickets.

## 7. Daily Description Format

When writing a daily work update, follow the language used by neighboring daily updates of the same kind. If no neighboring update establishes a language, use the language in which the request was made. An explicit language requested by the user takes precedence. This format is independent of the storage tool; use it whether the update is written in Outline, another system, or a local document.

Use the following structure and keep its headings in English:

```markdown
# Daily YYYY.MM.DD

## Done for YYYY.MM.DD

- Completed work, with concrete outcome and relevant technical detail.
- Findings or blockers, including their impact where known.
- Links to relevant changes, issues, merge requests, test evidence, or source material.

## Plan for YYYY.MM.DD

- [ ] Planned task with a clear, actionable outcome.
- [x] Completed task, when tracking completion in the same update.

## Tags

relevant-topic, another-topic
```

Use the daily update date in the title and `Plan for` heading. Use the date of the previous working day in the `Done for` heading, skipping weekends and non-working days. State this date mapping in the rule text, not as annotations in the structure example. Report material work and outcomes concisely, distinguishing completed work from investigation and planned work. Use checklist boxes for plans when tracking status; link supporting artifacts inline or immediately below the relevant item. Include tags only when useful or customary in neighboring updates. Omit empty sections rather than adding filler, unless the established daily template requires them.

When a plan item is transferred to the tracker, the item may be checked only after a tracker ticket has been created for the remaining work and linked from the plan item. Checking the item records that the work was transferred to the tracker, not that the long-term work itself is complete.

## 8. Issue Tracker Ticket Creation

When there is no existing suitable tracker ticket for requested or identified follow-up work, create a new ticket and assign it to the current user by default. If the tracker supports an explicit assignee, set the current user as the assignee rather than leaving the ticket unassigned. Do not create a duplicate when an existing ticket already covers the work; update or reference the existing ticket instead.

Structure the ticket description with these sections, in this order:

### Description

State the problem or need, relevant context, desired goal, and a concise summary of the work in scope. Keep implementation details out unless they are already known or required to explain the scope.

### DoD

List concrete deliverables and completion conditions. Include implementation, tests, documentation, rollout, migration, or operational verification when applicable. Do not mark a deliverable complete merely because investigation or ticket creation is complete.

### AC

Define specific, observable, and verifiable outcomes required for acceptance. Include relevant behavior, boundaries, failure cases, and edge cases when known. Criteria must describe what must be true, not only which tasks someone should perform.

### Related links

Link related tracker tickets and relevant technical artifacts, such as source files, pull requests, specifications, dashboards, or external documentation. Do not add links to Outline documents. If useful context exists only in Outline, summarize the relevant information in the ticket instead of linking back to Outline.

## 9. Do Not Make Unrequested Changes

Do not write code, edit files, or take system-changing actions unless the user explicitly asks. Questions, discussions, and descriptions of problems are not implementation requests. If intent is ambiguous, ask before changing anything. Reading and searching are allowed.

## 10. Do Not Run Heavy Operations

Do not run heavy, state-changing, or long-running operations yourself unless the user explicitly requests the exact operation. This includes `tofu plan`, `tofu apply`, `terraform plan`, `terraform apply`, `dagger call`, end-to-end tests, commands that start services or containers. Do not infer permission from a general request to implement or validate a change.

You may run read-only investigation and diagnostic commands by default, including commands that connect to external systems, when they are necessary to understand or troubleshoot the user's request. This includes Kubernetes inspection such as `kubectl get`, `kubectl describe`, `kubectl logs`, and `kubectl get events`, subject to the Kubernetes safety rules below. You may also run fast, self-contained checks that finish in a few seconds, produce a small amount of output, and have no external dependencies. When a requested workflow requires a prohibited operation, tell the user the exact command and explain that explicit approval is required before running it.

## 11. Kubernetes Operations Safety

These rules apply to all `kubectl` and Kubernetes MCP interactions, in local, staging, and production clusters.

- Before any command/tool call, run `kubectl config current-context` or the equivalent context tool and state the result to the user. Never switch context implicitly; get explicit approval for every context switch.
- Always specify the namespace explicitly. Never rely on the current context's default namespace. Ask for a namespace if the user did not specify one.
- Read a resource with `kubectl get`, `kubectl describe`, or an equivalent tool before modifying it. Prefer `kubectl diff -f <file>` or `kubectl apply --dry-run=server -f <file>` before apply. Show non-trivial diffs. Read and review generated manifests in full and get user confirmation before applying to production.
- Never run `kubectl delete` without explicit user intent. Never use `--force`, `--grace-period=0`, `--cascade=force`, or forced eviction without approval of that exact flag for that exact operation.
- Never automatically create, modify, read, or rotate Secrets. Each Secret operation requires explicit user instruction.
- After a mutation, inspect rollout status with `kubectl rollout status <resource>` or equivalent and do not report success until confirmed. On failure, inspect events, resource details, and logs before any further mutation.

## 12. Prefer OpenTofu

Use OpenTofu and the `tofu` command instead of Terraform and the `terraform` command whenever the project, provider, module, state, or user request supports both tools. Preserve `terraform` only when it is explicitly required by the project, an external interface, a compatibility constraint, or the user. Do not rename Terraform-specific configuration, provider addresses, state files, or documentation identifiers solely because `tofu` is preferred.
