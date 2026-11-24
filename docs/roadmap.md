# Implementation and release plan

The goal is a useful product with as little custom software as possible. Start with the smallest end-to-end installation, not a general framework. These work items are a proposed backlog, not completed work or created GitHub issues.

## Milestone A — Validate the native package

### A1. Prove installation, compilation, update and removal

- Select and pin a supported gh-aw release; record the compatibility floor in the package.
- Inspect upstream Agentics examples for useful QA and simplification techniques and review license/attribution before reuse.
- Install the package at a candidate commit into a disposable repository. Verify manifest resources, shared imports, workflow names and generated locks.
- Test selecting one role, both roles, updating a package with consumer-owned configuration, and removing it without deleting project knowledge.
- Resolve native packaging limitations through documented gh-aw mechanisms or upstream fixes before adding a custom installer.
- Add a small CI check using native gh-aw validation/compilation and a fixture if needed. No fabricated lock files.

### A2. Qualify the execution and publication boundary

- Select an available engine and verify authentication with a least-privilege configuration.
- Inspect compiled workflow jobs, triggers, checkout/ref, credentials, network, tool permissions, output handlers and artifact behavior.
- Confirm no role starts automatically on installation, PRs or forks, and that default manual dispatch uses the reviewed branch.
- Exercise no-finding, duplicate, missing-profile, blocked, interrupted and invalid-output paths. Check that no ordinary diagnostics create issues or comments, including failure tracking.
- Verify one staged issue maximum and that staged issue creation makes no repository mutation. Verify approved evidence artifact retention separately, including path, size and sensitivity restrictions.
- Exercise attempted unauthorized actions using controlled fixtures. Test pause/disable, timeouts and concurrent runs. Document the selected engine's real spending limits and enforceable stop mechanism.
- Keep the package experimental until these checks pass. If native controls cannot meet a requirement, keep that capability disabled and document the gap.

## Milestone B — Demonstrate consumer value

### B1. Validate the Hunter and Gardener on a small independent project

Use a small repository with deterministic fixtures, known failures, a necessary complexity case, and a clean case. Both roles must install without changing their shared mission or method. Test a meaningful discovery and a justified quiet outcome. Ensure a missing prerequisite is never misreported as correctness.

### B2. Validate a demanding second consumer

Use a substantially different repository, ideally a database or distributed system such as RockStream if its maintainers choose to participate. Configure domain contracts, existing test commands, important territories and safe execution settings. Avoid introducing domain-specific logic into Farm. Exercise a real bounded experiment and a structural investigation, and retain their review decisions.

### B3. Prove the learning loop

A human independently follows an investigation record, reviews a finding or negative result, and promotes the useful lesson before artifacts expire. A later run uses that lesson without repeating a settled finding. Verify that missing memory does not break a run. Use ordinary GitHub issues, reviewed docs and native artifacts; no new storage service.

## Milestone C — Publish a small first release

### C1. Make installation and adaptation easy

- Publish a pinned experimental or stable version only after the above gates.
- Document the exact tested installation, engine setup, configuration, compile, dispatch, audit, upgrade and removal commands.
- Supply a minimal starter profile and explain how to extend it with local test tools without increasing authority accidentally.
- Document how to change cadence and how to stop both roles. Explain what consumes credits and what native controls can enforce.
- Preserve upstream package-owned files and consumer-owned domain rules through updates. No new CLI or daemon.

### C2. Pilot in staged mode

A named maintainer runs a small staged pilot in each consumer, with a deliberately low cadence and explicit budget. Observe quality, novelty, evidence limits, review effort and costs. No finding quota. Pause on repeated infrastructure failure or excessive review burden.

### C3. Consider opt-in live issue publication

Review all output paths and retained evidence; prove same-repository and maximum-output controls, novelty checks, quiet outcomes and sensible human curation. Only then document an explicit live-publication opt-in using native gh-aw controls. A safe way to stay staged indefinitely remains supported. No automatic code changes in the first release.

## Definition of done for the first useful release

- Two different projects install the same shared Hunter and Gardener methods, with only domain guidance and native execution configuration differing.
- Installation, compilation and upgrades are reproducible against a recorded gh-aw version.
- At least one real learning cycle is reviewed and retained; a negative result counts.
- Findings are bounded, attributable, appropriately scoped and quiet when there is nothing to publish.
- No project-specific runtime, central service, custom package manager, bespoke configuration interpreter or general agent framework is needed.
- Maintainers can explain permissions, limits, costs, artifact exposure and the stop procedure.

## Later, only with demonstrated demand

More reusable methods and technology-specific guidance; independent verification and draft changes under separate authority; improved novelty checks and curation; native policy and evaluation components. Evaluate existing gh-aw and third-party capabilities first. Additional runtime code needs a specific unsolved problem and a narrow reviewable boundary.
