# Documentation language style

Use this rule when writing or editing documentation in any natural language. Apply it according to the language of each document, not according to the repository language.

## Precedence and language selection

- Follow a repository-specific or project-specific style guide first when one exists.
- Otherwise, write in the language used by adjacent documentation of the same type and audience.
- If adjacent documentation does not establish a language, continue in the language in which the document was started.
- Use English by default when no language precedent exists.
- An explicit language requested by the user overrides these defaults.
- Keep one primary prose language throughout a document unless the user explicitly requests multilingual content.

## Core rule

Write explanatory prose in the document's selected language. Use foreign-language terms only when they are more precise, are established terms without a clear local equivalent, or are exact parts of a technical interface.

- Prefer a clear and established term in the document language when one exists.
- Do not mix languages merely because source material uses a different language.
- Do not translate a term when translation would obscure its exact technical meaning.
- When both a localized term and the source term aid understanding, introduce them together on first use, then use the chosen form consistently.

Example pattern:

```text
<localized term> (<source term>)
```

Russian examples of this language-independent pattern:

```text
задание CI (job)
среда выполнения (runtime)
```

These examples do not make Russian the default language. Apply the same principle to any document language.

## Preserve exact technical identifiers

Do not translate or alter exact technical identifiers, including:

- Product, technology, service, library, and tool names.
- Commands and command-line options.
- File and directory paths.
- File names and extensions.
- Environment variables and configuration keys.
- Configuration values when their exact spelling is part of the contract.
- API field names, object kinds, enum values, event names, and protocol tokens.
- Code identifiers, package names, image names, and repository names.
- Exact section, entity, or feature names in an external system.
- Verbatim command output, error messages, and externally defined labels when accuracy is required.

Examples:

```text
GitLab CI
git push
--set-upstream
helmwave.yml.tpl
DEPLOYMENT_RUNTIME
production
Component
```

Write the surrounding explanation in the document language while preserving these values exactly.

## Terminology

- Establish one preferred term for each concept and use it consistently throughout the document.
- Do not alternate between localized and source-language terms for the same concept without a meaningful distinction.
- Preserve official capitalization and spelling, for example `GitLab`, `GitLab CI`, and `GitHub`.
- Do not use different branch names, state names, or identifiers interchangeably when they refer to one concrete scenario.
- If an external schema or metadata format uses source-language field names, preserve those names when listing or documenting the schema.
- If a term is ambiguous, define it in the glossary or at first use instead of silently choosing a meaning.

Russian examples of terminology localization:

| Prefer in Russian prose | Avoid without a specific reason |
| --- | --- |
| сборка | build |
| развертывание | deployment |
| сценарий | scenario |
| переменная | variable |
| зависимость | dependency |

For another document language, use established equivalents in that language rather than copying these Russian terms.

## Headings

- Write descriptive headings in the document language.
- Keep heading grammar and capitalization consistent with the conventions of that language and the surrounding documentation.
- Avoid mixing languages in one heading unless an exact product, command, file, API, or external entity name requires it.
- Preserve exact identifiers when the heading documents that identifier directly.

Examples of acceptable identifier-focused headings:

```text
post-build.py
helmwave.yml.tpl
GitLab CI
```

## Commands and examples

- Never translate executable commands, options, code, paths, variable names, or configuration values.
- Put executable content in an appropriate code block and use a language tag when possible.
- Write introductions, explanations, expected results, and troubleshooting guidance in the document language.
- Preserve factual output exactly when documenting command output or an external response.
- Localize illustrative placeholder values only when they are not contractually significant and localization improves comprehension.

Example:

```bash
git push -u origin main
helmwave yml --templater gomplate
python3 scripts/post-build.py 0.0.0
```

The prose before and after this block should use the selected document language.

## External taxonomies and translated navigation

- Translate user-facing taxonomy and navigation labels when established translations exist for the document language.
- Preserve machine-facing directory names, routes, keys, and identifiers unless the implementation itself localizes them.
- When documenting both forms, clearly distinguish the user-facing label from the machine-facing identifier.

Example pattern:

```text
User-facing label: <localized label>
Directory: `reference/`
```

## Editing and translation

- Preserve technical meaning, requirements, and interface identifiers when translating or synchronizing documents.
- Do not translate source-language prose word for word when that produces unnatural text. Use clear technical language in the target language.
- Do not introduce new requirements, claims, examples, or decisions during translation.
- Keep links, commands, paths, identifiers, and contractual values synchronized with the source unless the target environment differs intentionally.
- If a source term has no reliable equivalent, retain it and explain it at first use when necessary.

## Final consistency check

Before finalizing documentation:

- Confirm that prose consistently uses the selected document language.
- Confirm that exact technical identifiers retain their official spelling and capitalization.
- Confirm that one concept is not named with multiple unexplained terms.
- Confirm that headings follow the language and style of adjacent documentation.
- Confirm that commands, paths, schemas, and output were not translated or altered.
- Confirm that foreign-language terms remain only where they improve precision or preserve an interface contract.
