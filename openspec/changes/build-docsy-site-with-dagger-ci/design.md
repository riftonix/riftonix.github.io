## Context

This repository is currently a minimal GitHub Pages repository with `README.md`, `CNAME` set to `riftonix.io`, and OpenSpec scaffolding. The target site should use Hugo Docsy, following the implementation style in `/home/user/code/riftonix/kb`, while build and validation should use the prepared Dagger scenario in `/home/user/code/riftonix/daggerverse/scenarios/static-site`.

The static-site scenario already supports `verify_site` and `render_site` for Hugo and currently defaults to `github.com/google/docsy@v0.13.0`. The example `kb` site uses Hugo modules, Docsy config, Russian/English language config, Docsy navbar settings, and project SCSS overrides.

GitHub Pages custom domain handling is repository/site-level. `riftonix.github.io` is the GitHub-provided Pages domain and `riftonix.io` is the custom domain for the same site. To avoid ambiguity, this change treats `riftonix.io` as the canonical host for both production and preview URLs.

## Goals / Non-Goals

**Goals:**

- Build a Hugo Docsy site in this repository with a functional home page, navigation, base content, and project styling.
- Use `/home/user/Nextcloud/riftonix/featured-background.jpg` as the home page cover background.
- Keep the navbar translucent over the home page cover by using Docsy-compatible configuration and only minimal overrides when necessary.
- Use Dagger as the build/verification entrypoint for CI, preview rendering, and release rendering.
- Provide pull request preview artifacts addressable by MR/PR number under `riftonix.io`.
- Publish merged changes to `riftonix.io`.
- Add a merge-request `ci-passed` job that is intentionally always successful.
- Add Renovate automation for all tracked version declarations.

**Non-Goals:**

- Rebuild the Dagger `static-site` scenario unless implementation reveals a missing capability.
- Replace GitHub Pages with another hosting provider as part of the default implementation.
- Build a full content taxonomy or production copywriting set beyond the minimal site skeleton.
- Solve DNS registration or registrar-side configuration inside the repository.

## Decisions

1. Use Hugo modules for Docsy instead of vendoring the theme.

   This matches the `kb` example, keeps Docsy versioning visible in `go.mod`, and gives Renovate a normal dependency file to maintain. The site config will import `github.com/google/docsy`; the Dagger scenario can still pin the build-time theme URL for reproducible scenario validation.

   Alternative considered: vendoring Docsy into `themes/`. That would make local builds less dependent on module resolution but increases repository size and makes upgrades noisier.

2. Keep Docsy `v0.13.0` behavior as the initial visual target.

   The request specifically calls out transparent navbar behavior like Docsy `v0.13.0`, and the prepared static-site scenario also defaults to `github.com/google/docsy@v0.13.0`. The implementation should start with Docsy `v0.13.0` unless there is a compatibility issue that forces a newer version.

   Alternative considered: start from the newer `kb` dependency (`v0.15.0`). That may work, but it changes navbar defaults and increases visual drift from the requested behavior.

3. Publish previews as path-scoped builds on the canonical custom domain.

   Production is rendered at `https://riftonix.io/`. MR previews are rendered at `https://riftonix.io/<mr-id>/`. The implementation should publish one Pages tree that contains production at `/` and previews under `/<mr-id>/`, preserving the custom domain for the site.

   Complexity assessment:

   - Same published Pages tree, production plus `/<mr-id>/` on `riftonix.io`: low to medium complexity.
   - Separate preview Pages repository/site or external preview hosting: not needed for the current requirement.

   The default implementation should use the same-tree approach.

4. Keep `ci-passed` intentionally green and make real checks separate.

   The requested `ci-passed` job should always succeed on merge requests in this website repository only. Real quality gates should remain visible as separate jobs so failures are not hidden from reviewers. If branch protection requires only `ci-passed`, this intentionally allows merges while preserving diagnostic feedback.

   Alternative considered: aggregate required jobs, as `/home/user/code/riftonix/daggerverse/.github/workflows/ci.yaml` currently does. That is safer as a required gate but contradicts the requested always-success behavior.

5. Use Renovate with explicit managers and custom regex where needed.

   Standard managers should cover GitHub Actions and Go modules. Regex managers should cover standalone versions such as `DAGGER_VERSION`, Docsy theme URL pins, Hugo image/version strings, and any Dagger module references that are not visible to standard managers.

## Risks / Trade-offs

- Preview and production share one published Pages tree -> guard against preview publishes deleting production output by merging preview output into the existing deployed tree or using an equivalent preserve/restore step.
- Always-green `ci-passed` can weaken branch protection -> keep real check jobs visible and name the behavior explicitly in workflow comments.
- Hugo `baseURL` differs between preview and production path -> render preview with `https://riftonix.io/<mr-id>/` and production with `https://riftonix.io/`, and validate generated links for each mode.
- Docsy navbar behavior can drift across versions -> pin Docsy initially and add a visual/style check for home page navbar classes or rendered CSS.
- The background image lives outside the repository -> copy it into repository assets during implementation so CI can build without local Nextcloud state.
- Renovate can create noisy updates across Actions, Go, Dagger, and Hugo -> group related updates and use schedule/labels.

## Migration Plan

1. Add the Hugo Docsy site skeleton and local build commands.
2. Add Dagger verification/render commands for production and preview base URLs.
3. Add GitHub Actions workflows for pull request checks, preview deploy, and production deploy; document preview/production path behavior in site content such as `content/docs`, or in workflow comments, without adding a separate top-level `docs` directory only for that note.
4. Configure GitHub Pages production custom domain for `riftonix.io` and preserve custom-domain behavior in the deployed artifact/settings.
5. Add Renovate config and verify it detects every versioned dependency.
6. Run local validation and CI-equivalent commands before enabling branch protection changes.

Rollback is straightforward: disable the new deploy workflows, restore the previous Pages artifact/source, and remove preview paths from the published artifact if same-tree preview publishing was used.

## Open Questions

- None.
