---
name: dep-updater
description: >
  Analyze and update project dependencies, regardless of language, package manager, or execution harness.
  Trigger whenever the user asks to audit, check, review, or update dependencies/packages/libs in a project.
  The workflow covers dependency detection, available version analysis, changelog review, breaking-change detection,
  project-specific impact analysis, and controlled update application.
---

# dep-updater

A harness-agnostic skill for intelligently updating project dependencies.

## 1. Project detection

Automatically identify ecosystems and package managers from the files and configuration present in the project.

Possible ecosystems include npm, pnpm, Yarn, Bun, pip, Poetry, uv, Cargo, Go modules, Maven, Gradle, Composer, and others.

If multiple ecosystems coexist, handle each one separately.

Read direct dependencies and, when supported by the ecosystem, distinguish:

- runtime dependencies;
- development dependencies;
- peer/optional dependencies;
- pinned or locked dependencies;
- related lockfiles.

Do not assume that a dependency file implies a specific package manager. Infer the tool from lockfiles, project configuration, scripts, or existing usage.

## 2. Update detection

Use the native CLI of the detected package manager to identify outdated dependencies and available versions.

Do not use a generic command if the project clearly indicates a specific tool.

For JavaScript/TypeScript projects, support Bun when the project uses it.

Classify updates as:

- patch;
- minor;
- major;
- prerelease, if applicable;
- runtime, engine, or platform constraint change.

## 3. Changelog and documentation lookup

For each dependency to update, retrieve release notes covering the gap between the current version and the target version.

Prefer these sources:

1. GitHub Releases, using GitHub CLI if available;
2. `CHANGELOG.md` or release notes from the repository;
3. official documentation or migration guides;
4. Context7, if available;
5. the package registry;
6. web search, if needed.

When using Context7, always verify that the returned documentation matches the target package version. If Context7 points to outdated or ambiguous docs, do not rely on it; use official sources or web search instead.

If no reliable changelog is found, report it explicitly as: `changelog not found — manual verification recommended`.

Never invent changelog content.

## 4. Project impact analysis

For each package, inspect how it is actually used in the project:

- imports/usages;
- APIs called;
- options/configuration used;
- scripts, hooks, or integrations;
- usage in tests, CI, build, or tooling.

Classify the change as:

- breaking change;
- relevant bug fix;
- potentially useful new feature;
- transparent update;
- security fix;
- performance or compatibility change.

Clearly distinguish:

- theoretical breaking changes;
- breaking changes that actually affect this project.

## 5. Useful new features

If an update introduces a new feature that could be useful for the project, do not silently adopt it.

Instead:

- explain what the feature does;
- explain why it may be relevant in this specific codebase;
- point to the files or usage patterns that could benefit from it;
- estimate the implementation effort;
- ask for validation before changing existing working code to use it.

A dependency update and a feature adoption are two separate actions.

Updating the package may be safe to apply, but refactoring the project to use the new feature requires explicit approval.

## 6. Autonomy rules

### Apply directly

Apply without asking:

- patch/minor updates without breaking changes;
- transparent updates;
- low-risk bug fixes;
- security fixes without API changes, unless the context is sensitive.

### Apply and clearly report

Apply, then mention in the final report:

- potentially relevant bug fixes;
- security fixes;
- notable internal changes;
- behavior changes that should not break the public API.

### Ask before applying

Ask for validation before proceeding with:

- breaking changes;
- major updates;
- migrations requiring code changes;
- runtime or minimum version changes;
- dependency removal or replacement;
- adoption of a new feature;
- dependencies that appear intentionally pinned.

When validation is required, explain:

- what changes;
- whether the project is affected;
- likely impacted files;
- estimated effort;
- risk level.

## 7. Applying updates

Update dependencies only through the detected package manager CLI.

Do not manually edit dependency files to bump a package version, unless the ecosystem provides no reliable CLI-based way to do it.

Never edit lockfiles directly. Let the package manager regenerate them.

Respect the package manager actually used by the project:

- if the project uses Bun, use Bun;
- if it uses pnpm, use pnpm;
- if it uses uv, use uv;
- etc.

After updating:

- install/resolve dependencies with the native tool;
- adapt code only when validated or required by the autonomy rules;
- avoid unnecessary functional refactors;
- do not adopt new features without explicit approval.

## 8. Verification

After applying updates, run the relevant project checks when available:

- tests;
- typecheck;
- lint;
- build;
- format check;
- security audit;
- documented local CI commands.

Do not invent commands if the project exposes none clearly.

If a check fails:

- report the useful error;
- identify whether it is related to the update;
- fix it if it is in scope;
- otherwise clearly report the blocker.

## 9. Pre-update report

Before applying risky updates, produce a concise report:

```md
## Dependency update report

### Safe to apply directly
| Package | Current | Target | Type | Note |
|---|---:|---:|---|---|

### Requires validation
| Package | Current | Target | Reason | Estimated impact |
|---|---:|---:|---|---|

### Potentially useful new features
| Package | Feature | Why it may help | Effort |
|---|---|---|---|

### Missing changelogs
- package@version — manual verification recommended
````

Wait for validation only for items that require it.

## 10. Final report

After applying updates, produce a clear summary:

```md
## Dependency update applied

- X packages updated
- Y notable fixes
- Z migrations completed
- N packages skipped or awaiting validation

### Important changes
- package: short impact summary

### Potentially useful features not adopted
- package: feature summary and where it could help

### Modified files
- path/file

### Checks
- test: OK/KO
- build: OK/KO
- lint: OK/KO
```

## Important rules

* Do not edit lockfiles directly.
* Do not manually edit dependency files to update package versions: use the package manager CLI.
* Respect the package manager actually used by the project.
* Do not ignore development dependencies.
* Do not invent changelogs.
* Verify Context7 documentation freshness before relying on it.
* Do not adopt new features without approval.
* Ask for validation for majors, breaking changes, and migrations.
* Report pinned or very old dependencies before changing them.
* In case of dependency conflicts, explain the cause and propose the lowest-risk option.
