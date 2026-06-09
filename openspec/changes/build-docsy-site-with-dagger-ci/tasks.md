## 1. Deployment Strategy

- [x] 1.1 Decision only: use `https://riftonix.io/` as the canonical production URL and `https://riftonix.io/pr-preview/pr-<pr-id>/` as the canonical preview URL.
- [x] 1.2 Decision only: use `master` as the production branch name for deploy triggers.
- [x] 1.3 Update the `ci-passed` job to require success of previous verification and preview jobs instead of being always green.

## 2. Hugo Docsy Site

- [x] 2.1 Add Hugo site structure, `hugo.yml`, `go.mod`, and `.gitignore` entries for generated output.
- [x] 2.2 Configure Docsy as a Hugo module and pin the initial Docsy version according to the selected visual target.
- [x] 2.3 Add baseline home page and primary content/navigation entry points for the RiftOnix site.
- [x] 2.4 Copy `/home/user/Nextcloud/riftonix/featured-background.jpg` into repository assets or static files.
- [x] 2.5 Configure the home page cover to use the repository copy of `featured-background.jpg`.
- [x] 2.6 Configure Docsy navbar settings and any minimal SCSS/layout overrides needed for transparent navbar behavior over the cover.
- [x] 2.7 Run a local Hugo/Dagger render and verify generated internal links use the requested base URL.
- [x] 2.8 Document the same-site preview/production path strategy in site content, such as `content/docs`, or workflow comments; do not add a separate top-level `docs` directory only for this note.

## 3. Dagger Build and Verification

- [x] 3.1 Add a local `act` invocation path for the Dagger static-site verification job so local checks use the same GitHub Actions job shape as CI.
- [x] 3.2 Add a production verification/render command with base URL `https://riftonix.io/`.
- [x] 3.3 Add a pull request preview verification/render command with base URL `https://riftonix.io/pr-preview/pr-<pr-id>/`.
- [x] 3.4 Ensure CI can run the Dagger commands without depending on local-only files outside the repository.
- [x] 3.5 Update local `act` workflow handling so publish and preview jobs derive `SITE_OUTPUT_DIR` from the detected checkout directory, allowing README commands to omit `--env SITE_OUTPUT_DIR=...`.

## 4. GitHub Actions Pipeline

- [x] 4.1 Add pull request workflow jobs for checkout, Dagger setup, site verification, and preview rendering.
- [x] 4.2 Configure `rossjrw/pr-preview-action` in `ci.yaml` to deploy previews to the `gh-pages` branch and handle auto-cleanup.
- [x] 4.3 Extract the production `publish` job into `.github/workflows/publish.yaml` and use `JamesIves/github-pages-deploy-action` to deploy to the root of `gh-pages`.
- [x] 4.4 Add production Pages publishing with the correct `riftonix.io` custom-domain handling.
- [x] 4.5 Add workflow permissions (`contents: write`, `pull-requests: write`) and concurrency groups.
- [x] 4.6 Update `ci-passed` to require success of `verify` and `preview`/`publish`.
- [x] 4.7 Update repository settings (manually or via `gh` if possible) to set Pages source to the `gh-pages` branch.

## 5. Renovate

- [ ] 5.1 Add Renovate configuration for GitHub Actions, Go modules, and repository policy defaults.
- [ ] 5.2 Add Renovate regex managers for standalone `DAGGER_VERSION`, Hugo pins, Docsy URL pins, and static-site scenario references not covered by standard managers.
- [ ] 5.3 Add grouping, labels, and schedules to keep dependency update pull requests manageable.
- [ ] 5.4 Validate Renovate configuration locally or with a dry-run command if credentials/tooling are available.

## 6. Validation

- [ ] 6.1 Run OpenSpec validation for `build-docsy-site-with-dagger-ci`.
- [ ] 6.2 Run local site verification through the Dagger static-site scenario.
- [ ] 6.3 Render production output and confirm `https://riftonix.io/` base URL behavior.
- [ ] 6.4 Render a sample preview output and confirm pull request path behavior.
- [ ] 6.5 Inspect the rendered home page to confirm the background image and transparent navbar behavior.
- [ ] 6.6 Check git status and summarize generated files and any follow-up operational steps.
- [ ] 6.7 Before archiving this proposal, remove machine-local absolute paths from proposal/design/spec/task artifacts or replace them with stable repository-relative paths, public repository links, or implementation notes.
