# Native package qualification

Tracks [#2](https://github.com/grove/farm/issues/2). This records the reproducible qualification procedure and the observed results. The package remains experimental.

The first compiler candidate is [gh-aw v1.2.2](https://github.com/github/gh-aw/releases/tag/v1.2.2), the current stable release selected on 2026-11-24. The package declares that conservative minimum while compatibility is being established. The CI setup action is pinned to the same release's immutable commit. Do not claim older versions work without testing them.

## First deterministic check

The [qualification workflow](../.github/workflows/qualify-package.yml) uses the upstream setup action and native `gh aw add` and `gh aw compile`. It installs a specific Farm commit in a temporary Git checkout with only a README and a read-only GitHub token. It checks the three installed Markdown workflows, their nonempty generated locks, the shared import, the optional example, and the absence of a required `FARM.md`. It lists the installed files so the footprint is visible. It deliberately does not install an unrelated security-scanner workflow as part of the smoke test; compiler validation and strict mode remain enabled.

The temporary checkout uses a distinct synthetic GitHub origin solely for CLI repository context, with a non-existent local push target. gh-aw rejects installing a package from the current repository; the first run failed because the fixture identified itself as the source repository. No remote fixture is created, and nothing is pushed or dispatched. This exercises remote package retrieval and local compilation, not the permissions or metadata of a real consumer. It is not yet a separate GitHub repository or a full consumer acceptance test. Pull requests from forks do not run this first check; a maintainer can review and test a same-repository revision. There is no agent engine credential or additional runtime involved.

## Diagnostic publication boundary

The independent consumer's first generated locks exposed gh-aw v1.2.2 defaults for incomplete investigations, failed jobs and threat-detection tracking. Staging the Hunter/Gardener finding does not stage these independent issue paths. All three role sources explicitly disable these issue reports while retaining Actions diagnostics. Threat detection stays enabled and fails closed when no safe verdict is available. The qualification check rejects the observed unwanted settings in the compiled locks. This is a deterministic regression check for the pinned compiler, not proof of actual runtime behavior; the staged-output and failure cases must still be exercised under [#4](https://github.com/grove/farm/issues/4).

## First runtime evidence

The [initial Onboard run](https://github.com/grove/farm-example/actions/runs/45496710845) authenticated and executed successfully, with a clean detection verdict, but the intended artifact failed its final safe-output selection. The agent's edit tool could write a root-level file but could not create the absent `farm-output/` parent using the restricted shell. An alternate root filename reached gh-aw's staging artifact but was outside the configured allowlist. Native gh-aw treats `upload_artifact` handler failures as nonfatal, so a green run is not proof of delivery. Farm uses one exact root-level transient filename per report type and does not widen shell or output permissions. Verify the final safe-output handler outcome and expected delivered artifact (not merely the staging artifact) before accepting a trial; a missing record is incomplete even if Actions reports success. The [corrected Onboard run](https://github.com/grove/farm-example/actions/runs/45499924159) delivered the intended final artifact. Subsequent Hunter and Gardener runs also demonstrated delivered investigation records, quiet outcomes and a single staged candidate issue, with no observed repository mutation.

The initial activation also logged that daily AI-credit enforcement is skipped for manual dispatches. The per-invocation caps still apply; the maintainer must track manual aggregate usage separately. See the [pinned upstream cost-management documentation](https://github.com/github/gh-aw/blob/v1.2.2/docs/src/content/docs/reference/cost-management.md).

## Scoped execution evidence

The pinned Copilot CLI runtime denied exact approved Node commands in several reference-project trials. A [native SDK diagnostic](https://github.com/grove/farm-example/actions/runs/45508703225) with `engine.copilot-sdk: true` executed `node --version`, `npm test` and `node --test test/ledger.test.js` with exit 0 and denied an unlisted Node command. Both test invocations passed 5/5. The reference consumer enabled SDK mode for Hunter and Gardener, retaining existing permissions and budgets; the package default is unchanged. This does not establish the CLI denial's root cause or full sandbox qualification. An agent-generated focused probe remains to be demonstrated ([#15](https://github.com/grove/farm/issues/15)). The [next Hunter run](https://github.com/grove/farm-example/actions/runs/45510300492) used retained knowledge and suppressed a duplicate, but performed only static analysis; no approved test was attempted. Explicit focused dispatch is intended to address this distinction between a useful autonomous outcome and a requested experiment.

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

- Full and direct-role installation, upgrade and removal passed at the reviewed revision; see [lifecycle qualification](lifecycle.md). Continue qualifying additional consumers and compiler changes, including installation without the smoke-test scanner option.
- Inspect the generated permission and output paths before running anything. Authentication, a controlled manual agent run, evidence handling and effective security boundaries are covered by [#4](https://github.com/grove/farm/issues/4).
- Record the tested user-facing setup commands and limitations. Upgrade, customization and removal qualification continues in [#9](https://github.com/grove/farm/issues/9).

The distribution sources remain outside the executable workflow directory. The only added executable workflow is the ordinary deterministic CI check. No scheduled agents, repository mutation or issue publication are enabled.

## References

- [Pinned gh-aw releases](https://github.github.com/gh-aw/reference/releases/)
- [CLI commands](https://github.github.com/gh-aw/setup/cli/)
- [Package manifest](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/)
- [Shared imports](https://github.github.com/gh-aw/reference/imports/)
