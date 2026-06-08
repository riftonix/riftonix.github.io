## ADDED Requirements

### Requirement: Dagger site verification

The CI pipeline SHALL verify the Hugo site through the prepared Dagger static-site scenario.

#### Scenario: Pull request verification

- **WHEN** a pull request workflow runs
- **THEN** the workflow calls the Dagger static-site verification path for the repository site source

#### Scenario: Verification uses target base URL

- **WHEN** the site is verified for a deployment target
- **THEN** the verification command receives the base URL that matches that target

### Requirement: Local workflow execution

Local verification SHALL run through `act` against the GitHub Actions job that performs Dagger site verification.

#### Scenario: Developer runs local site verification

- **WHEN** a developer validates the site locally
- **THEN** the documented command runs the `site-dagger` GitHub Actions job through `act`
- **AND** the command passes the current repository checkout as the Dagger source directory

#### Scenario: Local execution follows CI shape

- **WHEN** the Dagger verification command changes in the GitHub Actions workflow
- **THEN** local `act` execution uses the same workflow step instead of a separate Makefile or script implementation

### Requirement: Pull request preview rendering

The pipeline SHALL render a feature preview for each pull request using a `riftonix.io` base URL that includes the pull request number.

#### Scenario: Preview base URL is calculated

- **WHEN** pull request number `123` is rendered for preview
- **THEN** the site is built with base URL `https://riftonix.io/123/`

#### Scenario: Preview artifact is published

- **WHEN** the pull request preview workflow completes successfully
- **THEN** the rendered site artifact is published to a preview path corresponding to the pull request number

### Requirement: Production rendering

The pipeline SHALL render the production site for `https://riftonix.io/` after changes are merged to `master`.

#### Scenario: Merge triggers production build

- **WHEN** changes are merged to `master`
- **THEN** the workflow renders the site with base URL `https://riftonix.io/`

#### Scenario: Production artifact contains custom domain marker when needed

- **WHEN** the selected GitHub Pages publishing mode requires a `CNAME` file in the deployed artifact
- **THEN** the production artifact contains `CNAME` with `riftonix.io`

### Requirement: Preview and production path strategy

The implementation SHALL publish production and preview builds under the same `riftonix.io` site without requiring a separate preview domain.

#### Scenario: Same Pages site is used

- **WHEN** the implementation uses one GitHub Pages site for both production and previews
- **THEN** production is available at `https://riftonix.io/` and previews are available under `https://riftonix.io/<mr-id>/`

#### Scenario: Preview update preserves production

- **WHEN** a pull request preview is updated
- **THEN** the deployment does not remove the production root site from the published artifact

### Requirement: Always-success ci-passed job

The pull request workflow in this repository SHALL include a `ci-passed` job that always finishes successfully.

#### Scenario: Upstream checks fail

- **WHEN** one or more diagnostic CI jobs fail in a pull request
- **THEN** the `ci-passed` job still reports success

#### Scenario: Diagnostic jobs remain visible

- **WHEN** the workflow run is inspected
- **THEN** real verification, build, and deploy-preparation jobs remain visible separately from `ci-passed`

### Requirement: Deployment permissions and concurrency

Deployment workflows SHALL use least-privilege GitHub token permissions and prevent conflicting deployments to the same target.

#### Scenario: Production deployments overlap

- **WHEN** two production workflow runs target the same environment
- **THEN** workflow concurrency prevents both from publishing at the same time

#### Scenario: Pages deployment runs

- **WHEN** a workflow deploys to GitHub Pages
- **THEN** it declares only the permissions required for Pages publishing and repository checkout
