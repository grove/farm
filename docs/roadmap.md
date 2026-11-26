# Implementation and release plan

The [long-term vision](vision.md) is continuous software cultivation: correctness, simplicity and understanding. This plan describes the narrower path to establishing a useful product, not the limits of that ambition. These work items are a proposed backlog, not completed work or created GitHub issues.

## Initial product strategy

**Prove Hunter first.** Focus qualification and early adoption on maintainers of high-reliability infrastructure libraries, such as storage engines, parsers and authentication libraries. They have consequential correctness requirements and existing verification instruments. Use small representative fixtures for controlled qualification and real independent consumers to measure incremental value.

Hunter's distinctive contribution should be identifying important properties, oracles, operation sequences and production paths that conventional verification has not yet exercised. Use deterministic instruments wherever possible instead of replacing linters, fuzzers or sanitizers with a model. The first release should demonstrate consequential, reproducible discoveries at a cost and review burden maintainers consider worthwhile.

Gardener remains an experimental package component, but is not required for first-release qualification. Reconsider it only when Hunter has established repeatable value and there is a distinct case for investigating conceptual complexity beyond deterministic analysis, with demonstrable benefit, behavioral preservation and affordable review.

### Illustrative target investigations

These are illustrative scenarios, not claims of defects Farm has already found.

#### Example 1: A parser loses the final bytes of a stream

A Go parser accepts an `io.Reader`. Its documentation is sparse, but its tests establish the expected parse result for valid input, and Go's reader contract permits a read to return both bytes and an end-of-file indication. CI exercises ordinary readers that return EOF separately from the final bytes.

Hunter notices that the streaming loop checks the error before consuming those bytes. It constructs a conforming reader that returns the final chunk and `io.EOF` together, then compares the result with the existing test fixture. The parser truncates the input or returns an incorrect result. The oracle comes from the standard interface and an existing expected result, not an invented business rule.

A small deterministic regression test reproduces the failure on the current revision. The maintainer can run it, accept the finding and fix the loop. The same test protects future changes. A conventional input fuzzer using the original reader harness might never reach this I/O boundary; Hunter's contribution is the new harness and the property it checks.

#### Example 2: An acknowledged transaction disappears after recovery

A storage engine promises that acknowledged commits survive a process interruption. Existing CI tests commits, checkpoints and restart separately; it does not combine them at a particular persistence boundary.

Hunter traces the commit and checkpoint paths and identifies a candidate interval where recovery might consult an outdated checkpoint. Using an approved fault-injection harness, it commits a known value, triggers checkpointing, interrupts the process at the selected point, reopens the store and checks that value. Suppose recovery returns the old value despite the commit having succeeded.

The finding includes the failing reproduction, baseline revision and relevant durability guarantee. A maintainer accepts the evidence, a developer fixes recovery, and the project keeps the experiment as a regression test. Hunter has added a meaningful state-transition sequence and a durability oracle to CI, not merely run a generic crash detector.

These scenarios define the kind of incremental value to seek, not actual Farm findings or a substitute for empirical qualification.

### Useful quiet outcomes

A completed quiet investigation should include a short factual coverage receipt in its ordinary run record, based on actual executed experiments. Report scope and revision, results, limits, and elapsed time and resource usage/cost when available. Keep blocked, failed and inconclusive outcomes distinct. An optional aggregate summary can show newly exercised scenarios and cumulative spend without creating notifications for each run.

For example, an illustrative receipt might read:

| | |
| --- | --- |
| Question | Does parsing depend on how a stream is chunked? |
| Scope | Streaming parser; valid fixture set at the tested revision |
| Experiment | Compare whole-buffer results with 24 chunking schedules, including bytes returned with EOF |
| Result | All 24 matched the expected results |
| Limits | Malformed inputs and cancellation not tested |
| Resources | 6 minutes; $0.70 in model and runner costs (illustrative) |

### Evaluation and expansion gates

Compare with the project's existing verification process, not with a world without tests. Record:

- **Incremental value:** consequential defects or new useful executable checks not already produced by existing CI and deterministic instruments.
- **Review economics:** precision, novelty, materiality, time to validate or reject findings, and value relative to review effort.
- **Compute economics:** total cost across all outcomes, cost per accepted useful finding or validated learning outcome, and marginal value of deeper experiments.
- **Confidence and learning:** distinct consequential scenarios exercised, limitations, executable evidence retained, and usefulness of quiet receipts.
- **Safety and coverage:** whether execution limits hold and important defects are missed; use known cases, comparable baselines and sampled quiet outcomes where practical.

Agree on project-specific budgets and thresholds before pilots. Reduce scope or stop if the evidence does not justify ongoing cost. No finding quotas and no inference of reliability from an absence of findings. Detailed evidence, publication and security requirements are in the [architecture](design.md).

After qualifying Hunter and onboarding, make cultivation routine through bounded recurring investigations and durable learning. Broaden into new methods, Gardener or optional factory integration only with evidence of incremental value. Explore deeper coordination or alternative runtimes only when concrete needs justify the additional complexity and risk.

## Milestone A — Validate the native package

### A1. Prove installation, compilation, update and removal

- Select and pin a supported gh-aw release; record the compatibility floor in the package.
- Inspect upstream Agentics examples for useful QA and simplification techniques and review license/attribution before reuse.
- Install the package at a candidate commit into a disposable repository. Verify manifest resources, shared imports, workflow names and generated locks.
- Test selecting Hunter and the optional-use onboarding workflow. Check selection and resource dependencies for the existing experimental Gardener as a packaging compatibility check, not a first-release role qualification gate. Install without `FARM.md`, update a package with a consumer-owned `FARM.md` and native overrides, and remove it without deleting project knowledge.
- Resolve native packaging limitations through documented gh-aw mechanisms or upstream fixes before adding a custom installer.
- Add a small CI check using native gh-aw validation/compilation and a fixture if needed. No fabricated lock files.

### A2. Qualify the execution and publication boundary

- Select an available engine and verify authentication with a least-privilege configuration.
- Inspect compiled workflow jobs, triggers, checkout/ref, credentials, network, tool permissions, output handlers and artifact behavior.
- Confirm no role starts automatically on installation, PRs or forks, and that default manual dispatch uses the reviewed branch.
- Exercise no-finding, duplicate, absent optional `FARM.md`, missing authoritative guidance, conflicting local rules, blocked, interrupted and invalid-output paths. Confirm an absent `FARM.md` alone does not block a run. Check that no ordinary diagnostics create issues or comments, including failure tracking.
- Verify that discovery of commands in README files, manifests and CI does not grant execution authority, and that local rules and untrusted instructions cannot expand permissions or weaken mandatory evidence and publication boundaries.
- Verify one staged issue maximum and that staged issue creation makes no repository mutation. Verify approved evidence artifact retention separately, including path, size and sensitivity restrictions.
- Exercise attempted unauthorized actions using controlled fixtures. Test pause/disable, timeouts and concurrent runs. Verify experimental code isolation, protected credentials, restricted network access and hard per-run and aggregate resource controls against the requirements in [architecture](design.md#8-authority-safety-and-defaults). Document the selected engine's real spending limits and enforceable stop mechanism. Keep affected experiments or unattended operation disabled when controls cannot be enforced.
- Keep the package experimental until these checks pass. If native controls cannot meet a requirement, keep that capability disabled and document the gap.

- Qualify the onboarding workflow independently: manual-only, no issue/PR/dispatch or repository-writing outputs, no execution of discovered project commands, one bounded sanitized artifact, reviewed log exposure and quiet failures.

## Milestone B — Demonstrate consumer value

### B0. Prove knowledge onboarding and preparation

- Run the reusable [onboarding method](onboarding.md) on an existing conventional repository without Farm-specific documentation and on a more complex repository with scattered or conflicting knowledge. Include fixtures for a stale design, a behavior inferred only from code, a misleading test, and an unauthorized candidate command.
- Require traceable source references and revision, clear distinction between supported contract, observed implementation, historical/proposed design and unresolved question, and a bounded set of useful starter territories and evidence limitations.
- Verify the proposed knowledge changes are minimal, preserve existing docs and any local `FARM.md`, distinguish candidate instruments from approved commands, and pose only material questions for the maintainer. No automatic repository changes or requirement to create a new knowledge file.
- Have a maintainer review the proposal, retain only needed guidance and choose an initial territory. Demonstrate that the subsequent Hunter can use the reviewed knowledge without rediscovering the entire project.
- Repeat a targeted survey after a relevant contract or instrument change; identify stale guidance and propose a small reviewed update rather than rewrite the project knowledge. Record onboarding effort, evidence quality and any unresolved blockers.

### B1. Validate Hunter on a small independent project

Use a representative infrastructure library with deterministic fixtures, known logic failures missed by its initial verification, and a clean case. Hunter must install without changing its shared mission or method. Start without a `FARM.md`: discover its purpose, supported contracts and candidate tests from ordinary repository files. Where native configuration authorizes the necessary instruments, show a meaningful discovery beyond existing CI, an independently checkable reproduction and a justified quiet outcome with a coverage receipt. Repeat with a minimal local rule that narrows the investigation. Exercise ambiguous or missing documentation and an undiscovered/unauthorized test; neither should lead to an unsupported correctness claim. Compare results and cost with the existing deterministic instruments and record maintainer review time.

### B2. Validate a demanding second consumer

Use a substantially different repository, ideally a database or distributed system such as RockStream if its maintainers choose to participate. Discover ordinary repository context and add a short `FARM.md` only for domain-specific gaps and constraints, such as explicit recovery or oracle requirements. Configure safe native execution settings only where necessary. Avoid restating existing contracts, maintaining a parallel instrument inventory or introducing domain-specific logic into Farm. Exercise real bounded correctness investigations that probe different properties or operation sequences and retain the review decisions. Compare against the project's existing CI and deterministic tools; prioritize genuine new defects rather than only rediscovering seeded examples.

### B3. Prove the learning loop

A human independently follows an investigation record, reviews a finding or negative result, and promotes useful executable evidence as a regression test, property, assertion, benchmark or repeatable experiment before artifacts expire. A later run uses that lesson without repeating a settled finding. Reviewed rationale can remain in ordinary project documentation, issues, PRs and commits; no parallel prose memory store, dependency tracking or revalidation queue. Verify that missing historical evidence does not break a run. Use native artifacts and the existing verification suite; no new storage service.

### B4. Prove incremental scaling without a new runtime

- Start with a single manually dispatched Hunter and no Farm-specific profile in a conventional project; broaden or deepen Hunter investigations without replacing the installation or losing prior learning. Adding Gardener is not required for initial qualification.
- In the larger consumer, use existing component boundaries and contract references to exercise more than one territory. Demonstrate bounded context and targeted history lookup rather than loading the entire repository or all prior results.
- Exercise native cadence/scope customization or a small scoped workflow wrapper using the same shared role and method, then scale back. Verify that consumer guidance, retained evidence and upgrades remain compatible.
- Test the relevant native concurrency behavior, delayed/skipped/canceled runs, interruption and overlapping duplicate proposals before allowing parallel execution. Verify aggregate budget, shared experiment resource limits and review capacity; keep outputs staged if a boundary is not enforceable.
- Establish that the scaled installation uses only gh-aw/Actions, existing approved instruments and any strictly necessary ephemeral helpers. No Farm-owned process, database, service, durable queue or independent control plane. Document remaining native platform limits rather than build a general runtime.
- Evaluate whether extra scope or frequency produces useful learning without proportionally increasing configuration effort or curation burden. A large project does not need more agents by default.

## Milestone C — Publish a small first release

### C1. Make installation and adaptation easy

- Publish a pinned experimental or stable version only after the above gates.
- Document the exact tested installation, engine setup, configuration, compile, dispatch, audit, upgrade and removal commands.
- Demonstrate installation without a profile on a well-documented repository. Supply a tiny optional `FARM.md` example, a tested onboarding workflow and concrete maintainer review checklist, examples of domain and role-specific rules, and instructions for approving local test tools without increasing authority accidentally.
- Record how many Farm-specific project files, manual choices and workflow edits each installation actually requires. Treat unnecessary setup and duplicated domain knowledge as product defects, not documentation problems.
- Document how to start with Hunter, focus on a subsystem, change cadence and stop all enabled instances using native gh-aw settings. Distinguish the experimental Gardener from the qualified first-release scope. Explain what consumes credits and what native controls can enforce; no automated scaling controller is required.
- Preserve upstream package-owned files and consumer-owned domain rules through updates. No new CLI or daemon.

### C2. Pilot in staged mode

A named maintainer runs a small staged Hunter pilot in each consumer, with a deliberately low cadence and explicit budget. Evaluate results against the initial strategy and agreed thresholds above; include all run costs, verify independent reproductions, and sample quiet receipts. No finding quota. Pause on repeated infrastructure failure, excessive review burden or insufficient incremental value.

### C3. Consider opt-in live issue publication

Review all output paths and retained evidence; prove same-repository and maximum-output controls, novelty checks, quiet outcomes and sensible human curation. Only then document an explicit live-publication opt-in using native gh-aw controls. A safe way to stay staged indefinitely remains supported. No automatic code changes in the first release.

## Definition of done for the first useful release

- Two different representative projects install the same shared Hunter methods, with only domain guidance and native execution configuration differing. Evidence from real investigations supports incremental value over their existing verification; Gardener qualification is deferred.
- A conventionally organized project can orient and investigate without a `FARM.md`. Local knowledge can be supplied in one optional Markdown file, with no required fields, duplicate inventory or custom configuration language.
- An existing project can use the same on-demand onboarding workflow to collect and distill relevant knowledge, review the gaps and start a real investigation with a minimal amount of maintainer input. Knowledge stays source-linked and locally owned; onboarding adds no required recurring workflow or independent runtime.
- Installation, compilation and upgrades are reproducible against a recorded gh-aw version.
- At least one real learning cycle is reviewed and retained as executable evidence; a useful negative result counts for learning, but does not substitute for demonstrating novel reproducible defects missed by existing verification.
- Findings are bounded, attributable, novel, independently checkable and appropriately scoped. Completed quiet investigations have factual coverage receipts without issue publication. Review and compute costs, including quiet runs, satisfy the pilot's agreed thresholds.
- No project-specific runtime, central service, custom package manager, bespoke configuration interpreter or general agent framework is needed.
- Demonstrate a path from one bounded workflow to broader or deeper investigations, and back, through native composition and settings without changing the shared method or introducing a Farm-owned runtime. No Farm-owned service runs while idle.
- Maintainers can explain permissions, limits, costs, artifact exposure and the stop procedure.

## Later, only with demonstrated demand

Gardener qualification, if distinct value beyond deterministic tools and checkable preservation of behavior can be demonstrated; more reusable methods and technology-specific guidance; independent verification and draft changes under separate authority; improved novelty checks and curation; native policy and evaluation components. Evaluate existing gh-aw and third-party capabilities first. Additional executable helpers need a specific unsolved problem and a narrow reviewable boundary, preferably inside an existing job. A separate runtime or always-on service is outside the roadmap and would require a new architectural decision grounded in measured limits.
