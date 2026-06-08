# riftonix.github.io
https://riftonix.github.io

## Local CI

Run the Dagger site verification job with `act` against the current working tree:

```bash
act -j verify
```

The workflow uses the current working directory as `SITE_DIR` during local
`act` runs.

Run the publish job locally to render with the canonical production base URL.
The `gh-pages` deployment step is skipped under `act`:

```bash
act -j publish --env SITE_OUTPUT_DIR="$PWD/public-production"
```

Run a pull request preview render. Local runs use preview id `0` unless an event
payload supplies a pull request number:

```bash
act -j preview --env SITE_OUTPUT_DIR="$PWD/public-preview-0"
```
