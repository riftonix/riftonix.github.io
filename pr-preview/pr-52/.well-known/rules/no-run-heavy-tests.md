# No running heavy tests

Do not run heavy or verbose tests yourself. Instead, instruct the user to run them and provide the exact command (or commands) they should use.

## What you may run

You may run fast, self-contained unit tests that complete quickly and produce a small amount of output. These are typically pure tests with no external dependencies (no network, no database, no containers, no long-running services) that finish in a few seconds and print only a short summary.

## What you must not run

Never run these yourself:

- Heavy integration, end-to-end, or system tests, including tests that start servers, spin up containers, connect to databases or external services, or run browser sessions.
- Tests that produce a large amount of output (for example verbose logs, large diffs, or very long stack traces), even if they are fast. A large output pollutes the session and should be left to the user.
- Long-running test suites that take more than a few seconds to complete.

For example, do not run the dagger-ci test suite (`dagger call --progress=plain all`). It spawns containers and runs integration tests, which is both heavy and verbose.

When in doubt about whether a test is heavy or produces large output, treat it as off-limits and ask the user to run it.

## What to do instead

For tests you must not run, after finishing your work, tell the user to run the tests themselves and provide the exact command (or commands). Prefer the command defined in the project's `AGENTS.md`, `README.md`, or package manifests. If multiple test commands are relevant, list them in the order the user should run them.

## Linters and typecheckers

This restriction applies to tests only. Linters, formatters, typecheckers, and other non-test verification commands (for example `npm run lint`, `npm run typecheck`, `ruff`, `mypy`, `tsc --noEmit`) may be run as normal and remain your responsibility to run as part of completing a task.

This applies to every language, framework, and project type.