# riftonix.github.io
https://riftonix.github.io

## Local CI

Run the Dagger site verification job with `act` against the current working tree:

```bash
act -j verify
```

<<<<<<< HEAD
Run a pull request preview render. Local runs use preview id `0` unless an event
payload supplies a pull request number. Preview output is written to
`public-preview-<id>/`:
=======
The workflow uses the current working directory as `SITE_DIR` during local
`act` runs. Local render jobs write output under that directory.

Run a pull request preview render. Local runs use preview id `0` unless an event
payload supplies a pull request number:
>>>>>>> 6a1c1fb (fix: remove .nojekyll from preview envs)

```bash
act -j preview
```

Run the publish job locally to render with the canonical production base URL.
The `gh-pages` deployment step is skipped under `act`. Local publish output is
written to `public-local/`:

```bash
act -j publish
```
