# Standalone specification structure

Use this rule when writing a product, business, system, or solution specification outside OpenSpec. Do not apply it to OpenSpec artifacts, which follow the separate OpenSpec artifact structure rule.

## General principles

- Treat every section and subsection in this rule as optional.
- Include a section only when reliable context exists and the section helps explain or verify the proposed change.
- Omit sections that do not apply to the task. Do not leave empty headings or placeholders. Explicitly tell the user in chat which sections were omitted as not applicable.
- Never fill a section merely because it exists in the outline. Do not add invented facts, targets, examples, decisions, or unsupported assumptions.
- If information is required but unavailable, do not write that section as if it were complete. Ask the user to investigate, confirm, or provide the missing context before writing it.
- Clearly distinguish known facts, confirmed decisions, assumptions, and open questions. Do not present an assumption as a requirement or decision.
- Keep each fact authoritative in one place. Reference it elsewhere instead of duplicating it across sections.
- Write requirements as testable statements. Prefer stable behavior and constraints over implementation details.
- Use consistent identifiers when traceability is useful, for example `P-001` for problems, `FR-001` for functional requirements, `NFR-001` for non-functional requirements, and `BR-001` for business rules.

## Language

- Write the specification in the language used by adjacent specifications in the same repository or documentation set.
- If no adjacent specification establishes a language, continue in the language in which the document was started.
- Use English by default when neither adjacent specifications nor existing document content establish a language.
- An explicit language requested by the user overrides all defaults and neighboring-document conventions.
- Keep one language consistently throughout the document unless the user explicitly requests a multilingual specification.

## Recommended outline

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

### 5.1 <Scenario Name>

### 5.2 <Scenario Name>

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

## 1. Glossary and Useful Links

Define only terms, abbreviations, roles, systems, and domain concepts that readers need to interpret the specification consistently.

Include when available:

- Domain terms whose meaning is specific to the organization or product.
- Abbreviations and acronyms used in the document.
- Names and responsibilities of relevant systems or components.
- Links to source requests, issue trackers, policies, prior decisions, diagrams, API documentation, and related specifications.

Do not turn the glossary into a general encyclopedia. If a term is standard and unambiguous for the intended audience, it does not need an entry.

## 2. Business Context

Explain why the specification exists, what situation led to it, and what outcomes are expected. Keep implementation design out of this section unless a current technical limitation is necessary to explain the problem.

### 2.1 Initial Requirements and Drivers

Record the confirmed inputs that initiated the work.

Include when available:

- Customer or stakeholder requests.
- Incidents, defects, support cases, or operational findings.
- Regulatory, contractual, security, or compliance obligations.
- Research results, metrics, or user feedback.
- Links to source materials and their relevant status.

Do not infer requirements from an issue title or incomplete request. Ask for clarification when the source does not establish the required outcome.

### 2.2 Current Solution

Describe the current behavior and workflow that are relevant to the proposed change.

Include when available:

- Current user or business process.
- Existing system behavior and responsibilities.
- Current inputs, outputs, and integrations.
- Relevant limitations, manual steps, or known workarounds.
- A concise current-state diagram when it improves understanding.

Describe only verified current behavior. Do not guess how an undocumented system works.

### 2.3 Business Problems

Describe the problems caused by the current state and why they matter.

For each problem, include when available:

- A stable identifier such as `P-001`.
- The affected users, teams, processes, or business capabilities.
- The observable problem and its consequences.
- Evidence, frequency, scale, cost, or severity.
- A link to the originating requirement or current-state behavior.

State problems without embedding the preferred solution. For example, describe missing visibility rather than requiring a specific database field.

### 2.4 Stakeholders

Identify parties who own, use, approve, operate, secure, or are affected by the change.

Include when available:

- Role or team.
- Responsibility in the change.
- Decision or approval authority.
- Consultation or notification expectations.
- Approval status when formally tracked.

Do not assign named individuals, ownership, or approval status without confirmation.

### 2.5 Goals

Define the outcomes the change is intended to achieve.

Goals should:

- Address the documented business problems.
- Describe outcomes rather than implementation tasks.
- Be measurable or objectively assessable where possible.
- Be specific enough to guide scope and acceptance decisions.

Do not write vague goals such as "improve the system" without a defined improvement.

### 2.6 Scope and Non-Goals

Define the boundaries of the specification.

Include when available:

- Products, users, processes, components, and environments in scope.
- Deliverables or behavior included in the change.
- Explicit exclusions and deferred capabilities.
- Assumed dependencies owned by other teams or changes.
- Boundaries between this work and adjacent initiatives.

Do not invent non-goals to fill the section. Record only exclusions that have been discussed or are needed to prevent scope ambiguity.

## 3. Functional Requirements

Define what the system must do. Requirements must be observable, testable, unambiguous, and independent of a specific implementation unless the implementation itself is a confirmed constraint.

Use normative wording such as `The system SHALL ...`. Give each requirement a stable identifier when traceability is needed.

### 3.1 General Functional Requirements

Define user-visible or system-visible capabilities that are not better classified under the more specific subsections.

Each requirement should establish:

- Actor or initiating system when relevant.
- Trigger or precondition.
- Required behavior.
- Expected result or externally visible state change.
- Applicable exceptions or boundaries.

Avoid combining multiple independently testable obligations into one requirement.

### 3.2 Domain Model

Define the business concepts the system must represent and the rules visible to consumers.

Include when available:

- Entities and their business meaning.
- Required and optional attributes.
- Relationships, ownership, and cardinality.
- Lifecycle states and valid transitions.
- Domain invariants and identity rules.

Keep physical tables, indexes, serialization details, and storage technology in section 6.3.

### 3.3 Business Rules and Constraints

Define rules that govern valid operations and state.

Include when available:

- Validation rules.
- Preconditions and prohibitions.
- Permission and eligibility rules.
- Calculations and derivation rules.
- Ordering, uniqueness, and concurrency rules visible at the business level.
- Valid and invalid state transitions.

Describe the required outcome when a rule is violated. Do not rely on examples as the only definition of a rule.

### 3.4 Contracts and Integrations

Define externally observable obligations between the system and its consumers or dependencies.

Include when available:

- Information that must be accepted, returned, emitted, or consumed.
- Required semantics and validation of exchanged data.
- Triggering conditions and expected outcomes.
- Idempotency, ordering, and consistency guarantees.
- Dependency failure behavior.
- Ownership of the authoritative data source.

Keep endpoint paths, DTO layouts, transport protocols, queue names, and concrete schemas in section 6.4 unless they are externally mandated requirements.

### 3.5 Error Handling

Define behavior visible to users and integrating systems when an operation cannot complete normally.

Include when available:

- Invalid input and validation failures.
- Missing or conflicting data.
- Permission failures.
- Unavailable or inconsistent dependencies.
- Partial completion and retry behavior visible to consumers.
- Required error category, user-facing outcome, and preserved state.

Keep internal exception types, retry implementation, logging mechanics, and error mapping in section 6.7.

## 4. Non-Functional Requirements

Define measurable quality attributes and operating guarantees. Do not use this section for architecture responsibilities or implementation decisions.

Each requirement should specify a measurable target, applicable workload or environment, and verification conditions. If no target has been agreed, ask the user to establish one rather than inventing a number.

### 4.1 Performance

Include when available:

- Response-time percentiles and maximum latency.
- Throughput and concurrency targets.
- Expected and maximum data volumes.
- Batch completion windows.
- Resource consumption limits.
- Degradation behavior under load.

State the workload, environment, and measurement boundary for every numeric target.

### 4.2 Reliability and Availability

Include when available:

- Availability objectives and measurement windows.
- Recovery time and recovery point objectives.
- Consistency and durability guarantees.
- Retry, idempotency, and duplicate-handling expectations.
- Failure isolation and graceful degradation requirements.
- Backup, restoration, and disaster recovery expectations.

Do not claim an SLA, SLO, RTO, or RPO without confirmation.

### 4.3 Security

Include when available:

- Authentication and authorization requirements.
- Roles, permissions, and separation of duties.
- Data classification and protection in transit and at rest.
- Secret and credential handling requirements.
- Audit and non-repudiation requirements.
- Retention, deletion, privacy, and compliance obligations.
- Abuse prevention and security boundary expectations.

Do not infer security classification or compliance obligations from general industry practice. Ask for the applicable policy or owner decision.

### 4.4 Compatibility

Include when available:

- Supported clients, platforms, environments, and versions.
- Backward and forward compatibility guarantees.
- Accepted data formats and protocol versions.
- Deprecation periods and consumer transition expectations.
- Behavior for legacy data or clients.

Distinguish required compatibility from a migration mechanism. Put the mechanism in section 6.6.

### 4.5 Observability and Diagnostics

Include when available:

- Required metrics, logs, traces, and audit records.
- Correlation identifiers and diagnostic context.
- Health, readiness, and dependency status signals.
- Alerting conditions and ownership.
- Retention, access, redaction, and cardinality constraints.
- Information required to distinguish failure stages and root causes.

Specify observable outcomes, not a particular monitoring vendor, unless the technology is a confirmed constraint.

### 4.6 Operational Constraints

Include when available:

- Supported deployment and execution environments.
- Infrastructure, network, storage, and resource constraints.
- Configuration and secret-management requirements.
- Release, maintenance-window, and support constraints.
- Geographic, tenancy, or data-residency restrictions.
- External dependencies and operational ownership boundaries.

Do not convert an implementation preference into an operational requirement without confirmation.

## 5. User Scenarios

Describe end-to-end interactions that show how requirements work together from an actor's perspective. Scenarios complement requirements but do not replace them.

For each scenario, include when available:

- Name and stable identifier when traceability is needed.
- Primary actor and participating systems.
- Scope or system boundary.
- Preconditions.
- Trigger.
- Main flow.
- Alternative and failure flows.
- Result and postconditions.
- Related requirement identifiers.

Use this structure when appropriate:

```markdown
### 5.1 <Scenario Name>

**Primary actor:** <Actor>

**Scope:** <System or process>

**Preconditions:**

1. ...

**Main flow:**

1. ...

**Alternative and failure flows:**

1. ...

**Result:**

...

**Related requirements:** `FR-001`, `BR-002`
```

Do not invent user behavior or happy paths when the workflow has not been confirmed. Ask the user or relevant stakeholder to establish the scenario.

## 6. Solution Specification

Describe how the confirmed requirements will be implemented. Keep this section aligned with sections 3 through 5 and do not introduce new business requirements implicitly through design choices.

### 6.1 Architecture and Component Boundaries

Include when available:

- Architecture overview and relevant diagrams.
- Components and their responsibilities.
- Ownership and trust boundaries.
- Dependencies and communication paths.
- Data flows and authoritative data sources.
- Synchronous and asynchronous boundaries.

If the architecture is not known, record the missing decision outside the final specification and ask the user to resolve it.

### 6.2 Changes by Component

For each affected component, include when available:

- Current responsibility relevant to the change.
- Required modifications.
- New inputs, outputs, dependencies, and configuration.
- Behavior that remains unchanged.
- Deployment or ownership impact.

Do not list components as affected without evidence from the current solution or an approved design.

### 6.3 Data Model

Describe the technical representation and persistence of domain concepts.

Include when available:

- Tables, collections, records, and schemas.
- Fields, types, required values, and defaults.
- Keys, relationships, indexes, and constraints.
- Ownership, lifecycle, retention, and deletion.
- Versioning and concurrency control.
- Data migration implications.

Do not invent field names, types, defaults, or indexes when implementation context is unavailable.

### 6.4 APIs and Other Technical Contracts

Describe concrete technical interfaces.

Include when available:

- Endpoints, methods, commands, events, topics, or queues.
- Request, response, message, and error schemas.
- Authentication and authorization mechanisms.
- Validation, versioning, idempotency, and ordering behavior.
- Timeouts, retry boundaries, and compatibility rules.
- Examples that illustrate, but do not replace, the normative contract.

Do not fabricate payloads or protocol details. Ask for the existing contract or an explicit design decision.

### 6.5 Interfaces

Describe user-facing and internal interaction surfaces.

Include when available:

- User interface screens, states, fields, actions, and navigation.
- Command-line commands, options, outputs, and exit behavior.
- Internal interfaces and extension points.
- Accessibility, localization, and responsive behavior when required.
- Empty, loading, success, validation, permission, and failure states.

Use mockups as supporting material, not as the sole definition of required behavior.

### 6.6 Migration and Backward Compatibility

Describe how the system moves from the current solution to the proposed solution safely.

Include when available:

- Data and configuration migration.
- Deployment order and rollout stages.
- Feature flags, dual-read, dual-write, or coexistence periods.
- Compatibility with existing consumers and persisted data.
- Validation and success criteria for each stage.
- Rollback triggers, procedure, and data implications.
- Deprecation and cleanup steps.

Do not state that migration is unnecessary without confirming the absence of persisted data, external consumers, and deployed behavior.

### 6.7 Error Handling

Describe the technical mechanisms used to satisfy functional and non-functional error requirements.

Include when available:

- Error detection and classification.
- Internal exception or result propagation.
- Mapping to API, event, UI, and CLI errors.
- Retry, timeout, backoff, and circuit-breaker behavior.
- Transaction boundaries, compensation, and recovery.
- Logging, tracing, metrics, and redaction.
- Ownership of operational response.

Do not introduce retry or recovery behavior that could violate confirmed business rules, consistency guarantees, or idempotency requirements.

### 6.8 Risks and Trade-offs

Record material uncertainty and accepted consequences of the solution.

For each item, include when available:

- Risk or trade-off.
- Cause and possible impact.
- Likelihood or severity when known.
- Mitigation, contingency, or accepted limitation.
- Owner or decision authority.
- Alternatives considered and why they were not selected.

Do not invent risks merely to populate the section. Omit it when no material risk or trade-off has been identified.

## Completeness review

Before finalizing a standalone specification:

- Remove every empty or inapplicable section.
- Tell the user in chat which planned sections were omitted because they were not applicable.
- Confirm that no statement was invented to complete the template.
- Identify missing information required for implementation or acceptance and ask the user to investigate or provide it.
- Confirm that goals address documented problems.
- Confirm that every requirement is testable and does not silently contain an implementation decision.
- Confirm that user scenarios trace to requirements.
- Confirm that solution details satisfy requirements without introducing undocumented scope.
- Confirm that functional and technical error handling are consistent.
- Confirm that compatibility requirements align with the migration and rollback plan.
