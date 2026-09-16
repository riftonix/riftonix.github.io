# OpenSpec artifact structure

Use this rule when creating or updating artifacts in an OpenSpec `spec-driven` change. Keep `tasks.md` outside this content plan because tasks are derived from the approved design.

## Core responsibility

- `proposal.md` explains why the change is needed, what it includes, and what it affects.
- `specs/<capability>/spec.md` defines observable, normative, and verifiable system behavior.
- `design.md` explains how the requirements will be implemented.
- Do not copy the same material between artifacts. Summarize context where necessary and keep the authoritative detail in the artifact responsible for it.
- Treat plan sections and subsections as optional unless the active OpenSpec schema requires them.
- Omit optional sections that do not apply to the change instead of leaving empty headings or placeholders. Explicitly tell the user in chat which sections were omitted as not applicable.
- Never fill an artifact section merely because it appears in an outline. Do not invent facts, requirements, targets, examples, decisions, stakeholders, or technical details.
- If required information is unavailable, ask the user to investigate, confirm, or provide the missing context before writing that part of the artifact.

## Language

- Write OpenSpec artifacts in the language used by adjacent OpenSpec changes and specifications in the repository.
- If adjacent artifacts do not establish a language, continue in the language in which the current artifact or change was started.
- Use English by default when neither adjacent artifacts nor existing artifact content establish a language.
- An explicit language requested by the user overrides all defaults and neighboring-artifact conventions.
- Keep a change's `proposal.md`, capability specifications, and `design.md` in one language unless the user explicitly requests otherwise.

## Content mapping

| Specification content | OpenSpec file | Target section |
| --- | --- | --- |
| Glossary and useful links | `design.md` | `## Glossary and References` |
| Business context | `proposal.md` | `## Why` |
| Initial requirements and grounds for change | `proposal.md` | `## Why` / `### Initial Requirements and Drivers` |
| Current solution | `proposal.md` | `## Why` / `### Current Solution` |
| Business problems | `proposal.md` | `## Why` / `### Business Problems` |
| Stakeholders | `proposal.md` | `## Stakeholders` |
| Goals | `proposal.md` | `## What Changes` / `### Goals` |
| Scope | `proposal.md` | `## What Changes` / `### Scope` |
| Non-goals | `proposal.md` | `## What Changes` / `### Non-Goals` |
| General functional requirements | `specs/<capability>/spec.md` | Requirement blocks under the applicable requirements operation |
| Domain model | `specs/<capability>/spec.md` | Observable entities, attributes, relationships, states, and invariants as requirements |
| Business rules and constraints | `specs/<capability>/spec.md` | Validation, permission, prohibition, transition, and invariant requirements |
| Contracts and integrations | `specs/<capability>/spec.md` | Observable API, event, integration, and data exchange requirements |
| Functional error handling | `specs/<capability>/spec.md` | Observable behavior for invalid input, conflicts, unavailable dependencies, and failures |
| Performance | `specs/<capability>/spec.md` | Measurable latency, throughput, concurrency, volume, and resource requirements |
| Reliability and availability | `specs/<capability>/spec.md` | Availability, recovery, consistency, retry, idempotency, and failure-tolerance requirements |
| Security | `specs/<capability>/spec.md` | Authentication, authorization, data protection, audit, and boundary requirements |
| Compatibility | `specs/<capability>/spec.md` | Supported versions, formats, clients, and backward-compatibility guarantees |
| Observability and diagnostics | `specs/<capability>/spec.md` | Required logs, metrics, traces, audit records, and diagnostic information |
| Operational constraints | `specs/<capability>/spec.md` | Deployment, environment, infrastructure, configuration, and operational requirements |
| User scenarios | `specs/<capability>/spec.md` | `#### Scenario:` blocks directly under the requirement they verify |
| Architecture and component boundaries | `design.md` | `## Architecture and Component Boundaries` |
| Changes by component | `design.md` | `## Component Changes` |
| Technical data model | `design.md` | `## Data Model` |
| APIs and other technical contracts | `design.md` | `## Technical Contracts` |
| User, CLI, and internal interfaces | `design.md` | `## Interfaces` |
| Migration and backward compatibility implementation | `design.md` | `## Migration Plan` |
| Technical error handling | `design.md` | `## Error Handling` |
| Risks and trade-offs | `design.md` | `## Risks / Trade-offs` |

## Proposal structure

Preserve the standard OpenSpec proposal headings and place plan content under them by meaning:

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

- Keep `Current Solution` in `proposal.md`. Describe the current behavior and system context that establish the need for change.
- Keep implementation details, complete architecture diagrams, storage details, and technical solution choices out of `Current Solution`.
- Use `None` for a required capability subsection when it has no entries.
- Use `Impact` to identify affected areas, not to repeat requirements or design details.

## Design structure

Preserve the conventional OpenSpec design headings. Add detailed solution sections between `Goals / Non-Goals` and `Decisions`:

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

- Use `proposal.md` for current behavior that motivates the change.
- Use `design.md` context only for technical context and constraints required to understand the proposed implementation.
- Omit optional interface or migration subsections that do not apply instead of leaving empty placeholders.

## Capability specification structure

For a main specification under `openspec/specs/<capability>/spec.md`, use:

```markdown
# <Capability Name> Specification

## Purpose

<Stable purpose and responsibility of the capability.>

## Requirements

### Requirement: <Requirement Name>

The system SHALL ...

#### Scenario: <Scenario Name>

- **GIVEN** ...
- **WHEN** ...
- **THEN** ...
```

For a delta specification under `openspec/changes/<change-name>/specs/<capability>/spec.md`, use the applicable OpenSpec operation headings:

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

<Complete updated requirement with all retained scenarios.>

## REMOVED Requirements

### Requirement: <Removed Requirement Name>

**Reason:** <Reason for removal.>

**Migration:** <Consumer transition.>

## RENAMED Requirements

- `FROM: <Old Requirement Name>`
- `TO: <New Requirement Name>`
```

- Include only operation sections that contain actual changes.
- Put every user, integration, failure, and boundary scenario directly under the requirement it verifies.
- Use `SHALL` for normative requirements and make non-functional requirements measurable.
- Do not create a separate user-scenarios section detached from its requirements.
- Avoid category headings between a requirements operation heading and `### Requirement:` blocks unless the installed OpenSpec validator is known to accept them.

## Boundary rules

- Domain specification: state what entities, relationships, states, and invariants are observable and required.
- Data design: state how those concepts are persisted, indexed, owned, and migrated.
- Contract specification: state what consumers can observe and rely on.
- Contract design: state endpoint paths, DTOs, schemas, protocols, queues, and implementation details.
- Error specification: state the behavior visible to users and integrations.
- Error design: state detection, propagation, mapping, retry, recovery, logging, and diagnostic mechanisms.
- Compatibility specification: state the compatibility guarantee.
- Migration design: state how that guarantee is implemented during rollout and rollback.

## Validation

- Preserve all headings required by the active OpenSpec schema.
- Omit optional and inapplicable sections, and tell the user in chat which planned sections were omitted as not applicable.
- Confirm that no content was invented merely to complete an outline. Ask for missing context needed to produce a correct artifact.
- Before finalizing artifacts, run the repository's OpenSpec validation command for the change.
- Treat validation failures as schema issues to fix, not as a reason to remove required business or technical content.
