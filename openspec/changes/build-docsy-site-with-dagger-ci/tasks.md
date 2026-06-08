## 1. Deployment Strategy

- [x] 1.1 Decision only: use `https://riftonix.io/` as the canonical production URL and `https://riftonix.io/<mr-id>/` as the canonical preview URL.
- [x] 1.2 Decision only: use `master` as the production branch name for deploy triggers.
- [x] 1.3 Add the always-successful `ci-passed` pull request job in this repository before implementing the rest of the pipeline.

## 2. Hugo Docsy Site

- [ ] 2.1 Add Hugo site structure, `hugo.yml`, `go.mod`, and `.gitignore` entries for generated output.
- [ ] 2.2 Configure Docsy as a Hugo module and pin the initial Docsy version according to the selected visual target.
- [ ] 2.3 Add baseline home page and primary content/navigation entry points for the RiftOnix site.
- [ ] 2.4 Copy `/home/user/Nextcloud/riftonix/featured-background.jpg` into repository assets or static files.
- [ ] 2.5 Configure the home page cover to use the repository copy of `featured-background.jpg`.
- [ ] 2.6 Configure Docsy navbar settings and any minimal SCSS/layout overrides needed for transparent navbar behavior over the cover.
- [ ] 2.7 Run a local Hugo/Dagger render and verify generated internal links use the requested base URL.
- [ ] 2.8 Document the same-site preview/production path strategy in site content, such as `content/docs`, or workflow comments; do not add a separate top-level `docs` directory only for this note.

## 3. Dagger Build and Verification

- [ ] 3.1 Add repository commands or scripts that call the Dagger static-site scenario from `/home/user/code/riftonix/daggerverse/scenarios/static-site`.
- [ ] 3.2 Add a production verification/render command with base URL `https://riftonix.io/`.
- [ ] 3.3 Add a pull request preview verification/render command with base URL `https://riftonix.io/<mr-id>/`.
- [ ] 3.4 Ensure CI can run the Dagger commands without depending on local-only files outside the repository.

## 4. GitHub Actions Pipeline

- [ ] 4.1 Add pull request workflow jobs for checkout, Dagger setup, site verification, and preview rendering.
- [ ] 4.2 Add preview publishing for the selected strategy, including path cleanup or overwrite behavior for repeated pull request updates.
- [ ] 4.3 Add production workflow trigger for merged changes on `master`.
- [ ] 4.4 Add production Pages publishing with the correct `riftonix.io` custom-domain handling for the selected Pages publishing mode.
- [ ] 4.5 Add workflow permissions and concurrency groups for preview and production deployments.
- [ ] 4.6 Keep diagnostic jobs visible separately from the always-successful `ci-passed` job.

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
