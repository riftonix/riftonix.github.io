## ADDED Requirements

### Requirement: Hugo Docsy site skeleton

The repository SHALL contain a Hugo Docsy site that can be built from repository files without relying on generated `public/` output committed to source control.

#### Scenario: Site source exists

- **WHEN** a developer checks out the repository
- **THEN** the repository contains Hugo configuration, module dependency files, content roots, assets, and any project-specific style/layout overrides needed to build the site

#### Scenario: Generated output is excluded

- **WHEN** the site is built locally or in CI
- **THEN** generated output directories such as `public/` are not required as source inputs

### Requirement: Docsy theme dependency

The site SHALL use Hugo modules to import the Docsy theme and SHALL keep the Docsy version visible in a Renovate-managed dependency file.

#### Scenario: Theme is resolvable

- **WHEN** Hugo resolves modules for the site
- **THEN** Docsy is imported from `github.com/google/docsy`

#### Scenario: Version is trackable

- **WHEN** dependency automation scans the repository
- **THEN** the Docsy version is discoverable from tracked configuration or module files

### Requirement: Home page cover background

The home page SHALL use the provided `featured-background.jpg` image as the first-viewport cover background.

#### Scenario: Home page renders with cover image

- **WHEN** the production site is rendered
- **THEN** the home page references the repository copy of `featured-background.jpg` in its cover or equivalent first-viewport background

#### Scenario: CI build is self-contained

- **WHEN** CI builds the site
- **THEN** the build succeeds without reading `/home/user/Nextcloud/riftonix/featured-background.jpg`

### Requirement: Transparent navbar over cover

The top navigation bar SHALL be transparent or translucent while it overlays the home page cover background.

#### Scenario: Navbar overlays cover

- **WHEN** a user opens the home page at the top scroll position
- **THEN** the navbar does not render as an opaque solid bar over the cover background

#### Scenario: Navbar behavior follows Docsy cover mode

- **WHEN** Docsy cover mode is active on the home page
- **THEN** site configuration keeps `navbar_translucent_over_cover_disable` disabled or applies an equivalent override

### Requirement: Baseline site navigation

The site SHALL include baseline navigation and content entry points suitable for a public RiftOnix site.

#### Scenario: User opens production root

- **WHEN** a user opens `https://riftonix.io/`
- **THEN** the rendered page presents the RiftOnix site home page and navigable links to primary sections

#### Scenario: User opens generated pages

- **WHEN** Hugo renders the site
- **THEN** generated internal links resolve under the active base URL
