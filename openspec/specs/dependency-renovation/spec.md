# dependency-renovation Specification

## Purpose
TBD - created by archiving change build-docsy-site-with-dagger-ci. Update Purpose after archive.
## Requirements
### Requirement: Renovate configuration

The repository SHALL include Renovate configuration for automated dependency updates.

#### Scenario: Renovate scans repository

- **WHEN** Renovate runs for the repository
- **THEN** it discovers repository package managers and applies project labels, grouping, and schedule rules from the repository configuration

### Requirement: GitHub Actions updates

Renovate SHALL track GitHub Actions versions used by repository workflows.

#### Scenario: Workflow action update exists

- **WHEN** an action such as `actions/checkout` or `dagger/dagger-for-github` has a newer allowed version
- **THEN** Renovate proposes an update according to the repository rules

### Requirement: Go and Hugo module updates

Renovate SHALL track Go module dependencies used by the Hugo site.

#### Scenario: Docsy module update exists

- **WHEN** `github.com/google/docsy` has a newer allowed version
- **THEN** Renovate proposes an update for the tracked Go module dependency

### Requirement: Dagger and static-site version updates

Renovate SHALL track Dagger CLI/action versions and static-site scenario references that are not covered by standard package managers.

#### Scenario: Dagger version changes

- **WHEN** a newer allowed Dagger version is available
- **THEN** Renovate proposes an update for workflow environment variables, action inputs, or other tracked Dagger version references

#### Scenario: Scenario reference changes

- **WHEN** the site pins a Dagger module or static-site scenario reference in a tracked file
- **THEN** Renovate can detect and propose updates for that reference

### Requirement: Hugo and Docsy build pins

Renovate SHALL track Hugo and Docsy build pins wherever they are represented outside standard package managers.

#### Scenario: Hugo version is pinned

- **WHEN** a Hugo version is declared in workflow, container, config, or Dagger settings
- **THEN** Renovate proposes updates for that declaration

#### Scenario: Docsy URL pin is used

- **WHEN** a Docsy theme URL pin such as `github.com/google/docsy@v0.15.0` appears in tracked files
- **THEN** Renovate proposes updates for that pin

