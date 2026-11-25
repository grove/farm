# Native lifecycle qualification

Tracks [#2](https://github.com/grove/farm/issues/2) and [#9](https://github.com/grove/farm/issues/9). The pinned toolchain is gh-aw v1.2.2. The [lifecycle CI](../.github/workflows/qualify-lifecycle.yml) exercises native direct-role installation and an older package revision upgraded to the observed default-branch revision, then role removal in a disposable checkout. Candidate installation and removal run in PR CI; the actual old-to-new native update runs on main after merge, because the native updater resolves the default branch rather than the PR candidate. It records failures rather than counting a missing import, an overwritten local setting, or a canceled removal as success. No model credentials, scheduled execution, remote writes or new Farm runtime are involved. A successful compile is not an execution qualification.

## Normal consumer workflow

In a clean checkout of a repository you own:

```sh
gh extension install github/gh-aw@v1.2.2
gh aw version
gh aw add grove/farm@REVIEWED_SHA --no-security-scanner
gh aw compile --validate --strict
git status --short
git diff
```

Replace REVIEWED_SHA with the full reviewed package revision. Commit the actual generated files through your normal branch and PR process. The reference consumer has demonstrated this path, including local recompilation after all shared resources are copied. Earlier revisions emitted import-order diagnostics because the shared investigation method was declared as a separate resource outside the conventional workflow import directory. The lifecycle trial confirmed that this prevented direct Hunter/Gardener installation and native update of those roles. The corrected package uses `workflows/shared/farm-investigation.md`, imported as `shared/farm-investigation.md`, so gh-aw can fetch it as a dependency before compilation. Full installation and individual role installation passed in [post-merge CI](https://github.com/grove/farm/actions/runs/45508579999) and [lifecycle CI](https://github.com/grove/farm/actions/runs/45508580012) at `d8fdd8197255b272c3b045e8be33ad166ed3410e`. Do not skip explicit compilation. The smoke test's `--no-security-scanner` is specific to the pinned qualification procedure, not advice to disable runtime security or skip review of an unfamiliar package.

The native CLI also exposes `gh aw add ... --create-pull-request` for authenticated local users. This option requires remote-write authority and has not yet been qualified by Farm. No extra installer is required to create an installation PR. The artifact transfer used during development was a constraint of the connected GitHub tools, which could write Git blobs but could not run the native CLI in the consumer checkout; it is not a normal consumer step.

## Update and removal

The upstream CLI's [update command](https://github.com/github/gh-aw/blob/v1.2.2/pkg/cli/update_command.go) normally performs a three-way merge. The package-target form is a repository URL. A previously SHA-pinned source tracks the latest default-branch revision on update; do not confuse that with selection of an explicit new SHA. Use a clean branch, check the resolved revision, inspect all permission, trigger, cost, publication and source changes, and recompile with the pinned toolchain before merging. Never apply an unreviewed `--force` or `--no-merge` to a customized consumer.

Native `gh aw remove farm-gardener --no-remove-orphans` previews matching roles and requires interactive confirmation. The filter is a substring match, not a package identity. Review its complete selection. For full removal, first stop scheduled/manual dispatch and review in-flight runs, then remove the selected role sources and locks through native tooling and a reviewed commit. Retain consumer-owned knowledge and unrelated workflows; inspect residual package assets and shared gh-aw initialization separately rather than deleting all of `.github`.

The [post-merge lifecycle run](https://github.com/grove/farm/actions/runs/45508580012) verified all three direct installs, full install, partial and complete role removal, and historical-package upgrade preserving project guidance and a local budget override. The [real consumer upgrade](https://github.com/grove/farm-example/pull/10) also preserved all three roles' scoped shell rules and budgets, moved the shared import, and passed strict compilation. The native updater refreshed container pins even with `--no-compile`; review the entire diff, including actions-lock.json. One import was rewritten to the equivalent pinned remote reference and normalized after byte comparison. Removal with `--no-remove-orphans` deliberately leaves shared imports and general gh-aw initialization. Broader qualification still includes cadence/scope, concurrency, cancellation, aggregate budget, and the native remote-PR flow. These are not unattended-operation qualifications.
