# Implementation and release plan

The goal is a useful product with as little custom software as possible. Start with the smallest end-to-end installation, not a general framework. These work items are a proposed backlog, not completed work or created GitHub issues.

## Milestone A — Validate the native package

### A1. Prove installation, compilation, update and removal

- Select and pin a supported gh-aw release; record the compatibility floor in the package.
- Inspect upstream Agentics examples for useful QA and simplification techniques and review license/attribution before reuse.
- Install the package at a candidate commit into a disposable repository. Verify manifest resources, shared imports, workflow names and generated locks.
- Test selecting one role, both roles, installing without `FARM.md`, updating a package with a consumer-owned `FARM.md` and native overrides, and removing it without deleting project knowledge.
- Resolve native packaging limitations through documented gh-aw mechanisms or upstream fixes before adding a custom installer.
- Add a small CI check using native gh-aw validation/compilation and a fixture if needed. No fabricated lock files.

### A2. Qualify the execution and publication boundary

- Select an available engine and verify authentication with a least-privilege configuration.
- Inspect compiled workflow jobs, triggers, checkout/ref, credentials, network, tool permissions, output handlers and artifact behavior.
- Confirm no role starts automatically on installation, PRs or forks, and that default manual dispatch uses the reviewed branch.
- Exercise no-finding, duplicate, absent optional `FARM.md`, missing authoritative guidance, conflicting local rules, blocked, interrupted and invalid-output paths. Confirm an absent `FARM.md` alone does not block a run. Check that no ordinary diagnostics create issues or comments, including failure tracking.
- Verify that discovery of commands in README files, manifests and CI does not grant execution authority, and that local rules and untrusted instructions cannot expand permissions or weaken mandatory evidence and publication boundaries.
- Verify one staged issue maximum and that staged issue creation makes no repository mutation. Verify approved evidence artifact retention separately, including path, size and sensitivity restrictions.
- Exercise attempted unauthorized actions using controlled fixtures. Test pause/disable, timeouts and concurrent runs. Document the selected engine's real spending limits and enforceable stop mechanism.
- Keep the package experimental until these checks pass. If native controls cannot meet a requirement, keep that capability disabled and document the gap.

## Milestone B — Demonstrate consumer value

### B1. Validate the Hunter and Gardener on a small independent project

Use a small repository with deterministic fixtures, known failures, a necessary complexity case, and a clean case. Both roles must install without changing their shared mission or method. Start without a `FARM.md`: discover its purpose, supported contracts and candidate tests from ordinary repository files. Where native configuration authorizes the necessary instruments, show a meaningful discovery and a justified quiet outcome. Repeat with a minimal local rule that narrows the investigation. Exercise ambiguous documentation and an undiscovered/unauthorized test; neither should lead to an unsupported correctness claim.

### B2. Validate a demanding second consumer

Use a substantially different repository, ideally a database or distributed system such as RockStream if its maintainers choose to participate. Discover ordinary repository context and add a short `FARM.md` only for domain-specific gaps and constraints, such as explicit recovery or oracle requirements. Configure safe native execution settings only where necessary. Avoid restating existing contracts, maintaining a parallel instrument inventory or introducing domain-specific logic into Farm. Exercise a real bounded experiment and a structural investigation, and retain their review decisions.

### B3. Prove the learning loop

A human independently follows an investigation record, reviews a finding or negative result, and promotes the useful lesson before artifacts expire. A later run uses that lesson without repeating a settled finding. Verify that missing memory does not break a run. Use ordinary GitHub issues, reviewed docs and native artifacts; no new storage service.

### B4. Prove incremental scaling without a new runtime

- Start with a single manually dispatched role and no Farm-specific profile in a conventional project; add the second role without replacing the installation or losing prior learning.
- In the larger consumer, use existing component boundaries and contract references to exercise more than one territory. Demonstrate bounded context and targeted history lookup rather than loading the entire repository or all prior results.
- Exercise native cadence/scope customization or a small scoped workflow wrapper using the same shared role and method, then scale back. Verify that consumer guidance, retained evidence and upgrades remain compatible.
- Test the relevant native concurrency behavior, delayed/skipped/canceled runs, interruption and overlapping duplicate proposals before allowing parallel execution. Verify aggregate budget, shared experiment resource limits and review capacity; keep outputs staged if a boundary is not enforceable.
- Establish that the scaled installation uses only gh-aw/Actions, existing approved instruments and any strictly necessary ephemeral helpers. No Farm-owned process, database, service, durable queue or independent control plane. Document remaining native platform limits rather than build a general runtime.
- Evaluate whether extra scope or frequency produces useful learning without proportionally increasing configuration effort or curation burden. A large project does not need more agents by default.

## Milestone C — Publish a small first release

### C1. Make installation and adaptation easy

- Publish a pinned experimental or stable version only after the above gates.
- Document the exact tested installation, engine setup, configuration, compile, dispatch, audit, upgrade and removal commands.
- Demonstrate installation without a profile on a well-documented repository. Supply a tiny optional `FARM.md` example, examples of domain and role-specific rules, and instructions for approving local test tools without increasing authority accidentally.
- Record how many Farm-specific project files, manual choices and workflow edits each installation actually requires. Treat unnecessary setup and duplicated domain knowledge as product defects, not documentation problems.
- Document how to start with one role, add/remove roles, focus on a subsystem, change cadence and stop all enabled instances using native gh-aw settings. Explain what consumes credits and what native controls can enforce; no automated scaling controller is required.
- Preserve upstream package-owned files and consumer-owned domain rules through updates. No new CLI or daemon.

### C2. Pilot in staged mode

A named maintainer runs a small staged pilot in each consumer, with a deliberately low cadence and explicit budget. Observe quality, novelty, evidence limits, review effort and costs. No finding quota. Pause on repeated infrastructure failure or excessive review burden.

### C3. Consider opt-in live issue publication

Review all output paths and retained evidence; prove same-repository and maximum-output controls, novelty checks, quiet outcomes and sensible human curation. Only then document an explicit live-publication opt-in using native gh-aw controls. A safe way to stay staged indefinitely remains supported. No automatic code changes in the first release.

## Definition of done for the first useful release

- Two different projects install the same shared Hunter and Gardener methods, with only domain guidance and native execution configuration differing.
- A conventionally organized project can orient and investigate without a `FARM.md`. Local knowledge can be supplied in one optional Markdown file, with no required fields, duplicate inventory or custom configuration language.
- Installation, compilation and upgrades are reproducible against a recorded gh-aw version.
- At least one real learning cycle is reviewed and retained; a negative result counts.
- Findings are bounded, attributable, appropriately scoped and quiet when there is nothing to publish.
- No project-specific runtime, central service, custom package manager, bespoke configuration interpreter or general agent framework is needed.
- Demonstrate a path from one bounded workflow to broader or deeper investigations, and back, through native composition and settings without changing the shared method or introducing a Farm-owned runtime. No Farm-owned service runs while idle.
- Maintainers can explain permissions, limits, costs, artifact exposure and the stop procedure.

## Later, only with demonstrated demand

More reusable methods and technology-specific guidance; independent verification and draft changes under separate authority; improved novelty checks and curation; native policy and evaluation components. Evaluate existing gh-aw and third-party capabilities first. Additional executable helpers need a specific unsolved problem and a narrow reviewable boundary, preferably inside an existing job. A separate runtime or always-on service is outside the roadmap and would require a new architectural decision grounded in measured limits.
