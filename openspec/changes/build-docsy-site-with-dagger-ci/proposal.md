## Why

The repository currently only serves a placeholder GitHub Pages site, while the desired target is a maintainable Hugo Docsy website with repeatable Dagger-based verification and publishing. Adding the site and pipeline now creates a clear path for MR previews, production releases on `riftonix.io`, and automated version maintenance.

## What Changes

- Add a Hugo Docsy site to this repository, modeled after `/home/user/code/riftonix/kb` and using the prepared Dagger static-site scenario from `/home/user/code/riftonix/daggerverse/scenarios/static-site`.
- Add a GitHub Actions pipeline that verifies the site on pull requests and deploys rendered artifacts.
- Add pull request preview publishing at `https://riftonix.io/<mr-id>/`.
- Add production publishing for merged changes at `https://riftonix.io/`.
- Add a `ci-passed` job that is always successful on merge requests, matching the requested behavior for branch protection/status checks.
- Use `/home/user/Nextcloud/riftonix/featured-background.jpg` as the home page cover background.
- Keep the Docsy top navbar transparent while it overlays the home page cover, preserving the behavior available in Docsy `v0.13.0`.
- Add Renovate coverage for all versioned dependencies and workflow/tooling versions used by the repository.

## Capabilities

### New Capabilities

- `docsy-site`: Defines the Hugo Docsy website structure, content baseline, homepage cover image, and navbar behavior.
- `dagger-site-deployment`: Defines Dagger-based site verification, preview deployment for pull requests, and production deployment for merged changes.
- `dependency-renovation`: Defines Renovate automation for repository dependency and tooling versions.

### Modified Capabilities

- None.

## Impact

- New Hugo files such as `hugo.yml`, `go.mod`, content, assets, layouts/partials or SCSS overrides, and static metadata.
- New or updated GitHub Actions workflows under `.github/workflows/`.
- New Dagger module wiring that calls the `static-site` scenario for validation and rendering.
- GitHub Pages configuration for preview and production publishing on `riftonix.io`, including custom-domain preservation.
- Renovate configuration for Go modules, GitHub Actions, Dagger version references, Hugo/Docsy versions, and other versioned project files.
- Read-only cross-repository dependency on the existing `daggerverse` static-site scenario; `ci-passed` changes are scoped to this website repository only.
