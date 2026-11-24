# Native package qualification

Tracks [#2](https://github.com/grove/farm/issues/2). This is a reproducible qualification procedure, not a record of a passing run.

The first compiler candidate is [gh-aw v1.2.2](https://github.com/github/gh-aw/releases/tag/v1.2.2), the current stable release selected on 2026-11-24. The package declares that conservative minimum while compatibility is being established. The CI setup action is pinned to the same release's immutable commit. Do not claim older versions work without testing them.

## First deterministic check

The [qualification workflow](../.github/workflows/qualify-package.yml) uses the upstream setup action and native `gh aw add` and `gh aw compile`. It installs a specific Farm commit in a temporary Git checkout with only a README and a read-only GitHub token. It checks the three installed Markdown workflows, their nonempty generated locks, the shared import, the optional example, and the absence of a required `FARM.md`. It lists the installed files so the footprint is visible. It deliberately does not install an unrelated security-scanner workflow as part of the smoke test; compiler validation and strict mode remain enabled.

The temporary checkout uses the source repository as its origin solely for CLI repository context. Nothing is pushed or dispatched. It is not yet a separate GitHub repository or a full consumer acceptance test. Pull requests from forks do not run this first check; a maintainer can review and test a same-repository revision. There is no agent engine credential or additional runtime involved.

## Repeat locally

Use a disposable checkout of a GitHub repository you own, with working GitHub CLI authentication and no existing Farm files. Do not push or dispatch an unqualified workflow.

```sh
# On a clean gh installation; use the pinned version for qualification.
gh extension install github/gh-aw@v1.2.2
gh aw version
# Replace FARM_SHA with the full, reviewed Farm commit SHA.
gh aw add grove/farm@FARM_SHA --no-security-scanner
gh aw compile --validate --strict
```

Inspect the resulting files, resource ownership, imports, compiler metadata and generated jobs. Record the exact Farm revision, compiler version, actual command output, hands-on steps, and any extra files or prerequisites. Do not hand-create generated locks or substitute static YAML parsing for compiler success.

## Remaining qualification

- Obtain the first successful installation/compilation check, fixing only demonstrated packaging or compiler incompatibilities.
- Repeat in an independent consumer repository, including selecting one role through supported native mechanisms where possible.
- Inspect the generated permission and output paths before running anything. Authentication, a controlled manual agent run, evidence handling and effective security boundaries are covered by [#4](https://github.com/grove/farm/issues/4).
- Record the tested user-facing setup commands and limitations. Upgrade, customization and removal qualification continues in [#9](https://github.com/grove/farm/issues/9).

The distribution sources remain outside the executable workflow directory. The only added executable workflow is the ordinary deterministic CI check. No scheduled agents, repository mutation or issue publication are enabled.

## References

- [Pinned gh-aw releases](https://github.github.com/gh-aw/reference/releases/)
- [CLI commands](https://github.github.com/gh-aw/setup/cli/)
- [Package manifest](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/)
- [Shared imports](https://github.github.com/gh-aw/reference/imports/)
