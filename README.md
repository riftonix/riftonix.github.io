# riftonix.github.io
https://riftonix.github.io

## Local CI

Run the Dagger site verification job with `act` against the current working tree:

```bash
act -j verify
```

Run a pull request preview render. Local runs use preview id `0` unless an event
payload supplies a pull request number. Preview output is written to
`public-preview-<id>/`:

```bash
act -j preview
```

Run the publish job locally to render with the canonical production base URL.
The `gh-pages` deployment step is skipped under `act`. Local publish output is
written to `public-local/`:

```bash
act -j publish
```
