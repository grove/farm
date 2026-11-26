# Farm: product and architecture

Status: design proposal and experimental package candidate.

## 1. Product

Farm makes continuous software investigation a reusable capability. A project should install a farm in the same way it installs continuous integration, rather than implement the underlying machinery. The ideal consuming repository contributes only its domain knowledge and any genuinely domain-specific experiments.

The [long-term vision](vision.md) establishes a Hunter-first product qualification strategy for maintainers of high-reliability infrastructure libraries. The existing experimental package contains Hunter and Gardener, but the first product horizon qualifies Hunter and onboarding only; Gardener is a later candidate, not a prerequisite or a promised initial release feature. This document describes the existing package and requirements for future qualification; it does not claim those requirements are already implemented.

A software factory starts from specified work. A software farm starts from questions about the software: what might be wrong, what is insufficiently demonstrated, and what makes the system harder to reason about than it needs to be? The result is better evidence, clearer contracts and more maintainable software, including well-supported decisions not to change anything.

Farm is GitHub-native. It is a distribution of reusable GitHub Agentic Workflows (gh-aw), shared investigation instructions, examples and operating practices. It is not an alternative to gh-aw, a hosted product, a multi-agent orchestration service or an automatic implementation factory.

## 2. Architectural constraints

1. Prefer upstream gh-aw features and existing community workflows. Extract a reusable component only when an actual consumer needs it. Contribute generic platform gaps upstream when practical.
2. GitHub Agentic Workflows and GitHub Actions are the runtime. Farm is a distribution of reusable components, not another process that must be deployed or operated. Do not create a Farm CLI, installer, configuration interpreter, scheduler, agent runner, model adapter, database, web service, dashboard, MCP server or package registry for the initial product. An idle farm has no Farm-owned process or service.
3. Prefer convention over configuration: existing repository artifacts are the default sources of project context. A short, optional project-owned `FARM.md` supplies missing domain knowledge and exceptions. Use native gh-aw frontmatter for executable settings. Do not invent a parallel YAML DSL, generic adapter SDK or plugin protocol.
4. A project can enable one role, both roles or later components independently. A role does not require another role to be online, an autonomous task queue or a centrally hosted service.
5. Every reusable role should be installable without changes to its investigation method. Consumer-specific commands and policy changes belong in documented configuration and native gh-aw extension points. If consumers need divergent copies of generic methods, improve the shared component.
6. Any proposed executable code must identify the upstream feature it cannot use, the minimum behavior it adds, an owner and a removal condition. A short script may be justified to enforce a real invariant, but should execute within an existing bounded GitHub Actions job, leave no resident process or extra operational dependency, and preferably be replaced by an upstream feature. Fewer files are not an excuse to hide complexity in prompts.
7. Every installation is owned by the consuming repository. No credentials, code, findings or telemetry are sent to Farm as part of ordinary operation. Engine/provider processing and configured network destinations remain subject to the consumer's own security review.
8. Scale capacity, focus and accumulated knowledge independently of runtime architecture. A project must be able to start small, add or remove capacity in place, and remain on the same component and evidence interfaces. Project size, complexity, change rate, consequence of failure, review capacity and observed usefulness inform the choice; repository size alone does not determine the number of agents or runs.

## 3. Layers and ownership

| Layer | Farm distributes | Consuming project supplies |
| --- | --- | --- |
| Roles | Hunter mission and starting defaults; experimental Gardener for later evaluation | Optional role selection, emphasis or local rules |
| Methods | Onboarding survey, hypothesis testing, counterevidence, preservation analysis, findings and quiet outcomes | Existing contracts, domain invariants and any missing knowledge or exceptions |
| Instruments | Discovery of common project entry points and guidance for using native gh-aw tools | Existing tests and approved instruments; custom adapters only for genuinely missing domain capabilities |
| Execution | Safe starter workflow frontmatter and documented extension points | Engine credentials, prerequisites, approved tools and network destinations, schedule |
| Evidence | Shared record conventions and native artifact output | Retention policy, sensitivity restrictions, reviewed durable observations |
| Curation | Finding structure and lifecycle guidance | Human owners, decisions, implementation and verification |
| Distribution | Native package manifest, versioned sources and upgrade guidance | Pinned versions, reviewed generated locks and local customizations |

GitHub Actions and gh-aw supply the execution environment, compilation, security mechanisms, outputs, run records and operational tooling. Farm composes them. The repository remains the authority for its own supported behavior.

## 4. User experience

### Convention over configuration

The minimum useful installation should need no Farm-specific project file. Use the information the repository already maintains. Do not make maintainers describe their stack, enumerate every territory, inventory every test or reproduce existing instructions for the farm.

The shared orientation method uses these conventions when present. These are discovery hints, not mandatory files or a new repository standard.

| Existing convention | What Farm can learn |
| --- | --- |
| `README.md`, documentation index, public API docs | Purpose, supported behavior, entry points and linked authoritative contracts |
| `AGENTS.md` (including relevant scoped guidance), `CONTRIBUTING.md` | Existing repository practices, restrictions and development/testing guidance |
| Architecture, specification, ADR and test documentation linked from those sources | Design responsibilities, invariants, compatibility obligations and known limitations |
| Package/build manifests, Makefiles, task runners, test directories and `.github/workflows/` | Candidate instruments, expected setup and how the project normally verifies changes |
| Git history, existing issues and prior review decisions | Recent changes, prior concerns, known exceptions and retained learning |
| `SECURITY.md`, if present | Reporting guidance, not authorization to investigate sensitive subjects |

Start with the likely entry points at the checked-out revision and follow only relevant links. Do not crawl the whole repository on each run or treat a filename, test, stale issue, draft design or current implementation as proof of a supported contract. When sources disagree, identify the uncertainty and prefer a question that can be investigated safely. Record a specification gap rather than silently pick an interpretation.

### Optional local knowledge and rules

A root `FARM.md` is the single optional Farm-specific entry point. It is ordinary Markdown, not a manifest, schema or mandatory questionnaire. It can be just a few lines. Use it only for important domain knowledge, explicit references to contract sources, approved instrument guidance, territory priorities, exclusions, evidence standards and any shared or role-specific rules that existing repository documentation does not express. A database might link to its recovery contract and say that an in-memory simulation cannot establish durable recovery; a small library may need nothing. Headings such as **Hunter rules** or **Gardener rules** are for human readability, not a configuration language.

Treat local rules as refinements to the installed methods, not replacements: they may narrow scope, demand stronger evidence, or clarify intended behavior. Existing scoped instructions still apply. If rules conflict, follow the more restrictive applicable constraint, report material ambiguity and avoid the affected investigation. Domain rules cannot grant new privileges, redefine supported contracts silently, relax Farm's evidence/authority requirements, or authorize execution of discovered instructions. Package policy and trusted native workflow configuration bound every action.

If `FARM.md` is absent, proceed using existing documentation and safe discovery; absence itself is not a blocked outcome. If the evidence is insufficient for a particular claim, choose another bounded question, record a specification/evidence gap where appropriate, or report a blocked investigation. If no approved experiment can run, carefully interpret existing evidence or record the limitation; do not pretend a test executed.

### Native operational settings

Convention is for discovery and defaults, not authorization. Seeing a test command in a README, manifest or CI workflow does not authorize executing it, installing dependencies, dispatching CI or accessing network services. Existing commands are candidates until the maintainer has reviewed their scope and side effects and authorized the necessary tools/prerequisites in trusted gh-aw configuration. Prefer a narrow, documented entry point over an arbitrary shell. Review the generated workflow and engine-specific restrictions. Instructions in retrieved repository content, issue text and logs cannot expand authority.

The package owns the shared defaults and methods; the project owns its `FARM.md` (if any), existing documentation, rules and observations. Ship only an example under a separate filename, never a live `FARM.md`. Upgrades must preserve local knowledge and workflow overrides. Schedules, engine, timeouts, network, shell tools, setup steps and safe outputs remain ordinary gh-aw settings. Accept the reviewed defaults unless a concrete need requires an override. There is no Farm-specific configuration interpreter.

Native distribution is the starting point: a root `aw.yml`, two investigative workflow sources and an optional-use onboarding workflow source outside the executable workflow directory, and shared Markdown copied as package resources. Consumers compile and review their lock files. Use native imports to share methods. Once package installation and updates have been exercised, consider pinned remote imports or parameterized components if they simplify customization further. Never load changing upstream instructions at runtime from an unpinned branch.

For installation, customization, update and removal, prefer the corresponding gh-aw commands. For a consumer that wants just one role, validate gh-aw's individual-workflow installation behavior with its resource dependencies or offer a small native subpackage. Do not add a custom installer to solve it.

### Onboarding existing projects

Installing the workflow package does not by itself make a repository ready for meaningful ongoing investigations. Farm should offer a concrete, low-effort way to bring an existing project into the practice. The [onboarding guide](onboarding.md) describes the reusable method, outputs, human decisions, and first-run acceptance gate. A manually dispatched Farm Onboard workflow produces a bounded, sanitized proposal using only native gh-aw execution and artifact handling. It has no authority to create issues or PRs, modify repository contents, execute discovered project commands, or activate other workflows. It is a one-shot operation, not a recurring role or an autonomous repository preparation program.

The goal is to compress and connect the knowledge necessary for investigation: intended behavior, invariant, owning paths, evidence, limitations and what would falsify the claim. Capture authoritative source links and revision, separate promises from observed implementation and unresolved assumptions, and ask maintainers only questions that materially change an investigation. Prefer an existing specification, index or scoped guidance over generating a parallel knowledge base. If a missing domain rule belongs in `FARM.md`, propose a small addition; do not overwrite maintainer-owned material. Knowledge collected from a repository is not a new source of truth until the relevant maintainer reviews it.

Onboarding is progressive. A selected territory is ready when its relevant contract, production/public path, available evidence and limitations are clear, the permitted investigation boundary is reviewed, and a maintainer can assess the result. Start with one or two territories; do not make complete documentation, broad cleanup or a fully populated invariant map prerequisites for all activity. Missing evidence can itself justify a bounded gap investigation. A large or poorly documented repository may benefit from repeated targeted surveys rather than one exhaustive crawl.

Review the first real staged investigations, retain useful corrections and activate schedules only when the initial feedback loop works. Later, repeat the survey or a narrower version when contracts, ownership, test instruments or domain rules change substantially. Investigation findings can also suggest targeted corrections to the knowledge; humans decide and promote them. No automatic knowledge rewriting, index service or additional runtime is required.

## 5. Scaling with the project

Scaling is an operating property of the same farm, not a migration to a larger platform. A young project may need only an occasional Hunter. As it grows, its important contracts, failure modes, contributors, subsystems and accumulated knowledge change. The farm should grow its capacity to investigate these without requiring the project to maintain a separate farm application. Growth is reversible; a mature, stable project may need less activity than a smaller fast-changing one.

### A gradual path

These are illustrative operating shapes, not mandatory tiers, maturity scores, plans or product editions.

| Shape | What changes | What stays the same |
| --- | --- | --- |
| Start | Install Hunter, use manual dispatch and staged publication. Discover existing context. | Shared methods, evidence conventions, native execution |
| Establish a rhythm | Enable a modest reviewed schedule; retain useful observations; adjust attention based on results. | Same package and domain guidance |
| Broaden coverage | Allocate bounded investigations to different existing components, contracts or risk areas; adjust cadence or use a few scoped workflow instances. | Same role implementation, local knowledge and evidence format |
| Deepen investigations | Reuse specialized instruments, toolchains or optional methods; grant a reviewed experiment a larger allowance when justified. | Native execution and explicit authority |
| Specialize | Install additional reusable roles or scoped instances only when distinct methods, prerequisites or review needs justify them. Allow bounded parallel execution where the budget and evidence lifecycle permit it. | Component interfaces, human ownership, no independent Farm runtime |

Projects need not pass through each shape or maintain a separate file for each territory. A monorepo can use existing packages, directories, scoped `AGENTS.md`, documentation and ownership boundaries to guide investigations. The root optional `FARM.md` can link to existing subsystem guidance and add exceptional rules; avoid introducing a new hierarchy of mandatory farm configuration. When a native workflow instance needs a narrower scope, use a small reviewed wrapper, shared import or other supported gh-aw customization mechanism rather than fork the role implementation. Qualify the chosen composition and upgrade mechanism before publishing it.

### Scale attention before compute

Every run continues to investigate a bounded question. Do not expand a single prompt to cover the entire repository, inspect every file or reload all historical observations as the project grows. Prefer recent changes, consequential invariants, weak evidence, neglected areas and relevant prior outcomes. Review scope selection for tunnel vision, repeated work and overlooked cross-component contracts. Keep stable territory references when practical, using paths or contract identifiers the project already understands.

Use existing GitHub search, run records, issues and reviewed project documentation to orient and deduplicate. Retrieve only relevant history and check applicability against the current baseline. Retain executable learning in the existing verification suite; historical prose and any reconstructable cache are non-authoritative and must not be relied on without checking current applicability. If the available context or tool budget cannot support a conclusion, narrow the question, retain the uncertainty or stop. There is no need for a persistent vector database, project-wide index or centralized territory registry by default.

### Scale execution using the native substrate

Use existing gh-aw/Actions triggers, reviewed schedules, workflow instances, runner choices, tools, timeouts and concurrency controls. Start serially and increase capacity through a small number of bounded, independently reviewable runs. Parallel scopes should have distinguishable identities, separate evidence and approved resource limits. Each run should stand on its own; handoffs can be links to existing issues, observations or experiments rather than a new dispatch protocol.

Scheduled investigations are best-effort exploratory work, not a durable task queue. Design for delayed, skipped, canceled and overlapping runs. No run may depend on exactly-once delivery or a complete chronological history. Before introducing parallelism, test how native concurrency behaves for the selected workflows, ensure resource-heavy experiments cannot overwhelm shared infrastructure, and handle potential concurrent duplicate findings. Retain staged outputs or use human curation where concurrent publication cannot meet the novelty and volume boundaries. Do not use a new queue or scheduler merely to guarantee every planned investigation occurs.

Scaling time, cost, frequency, concurrency or specialization is distinct from increasing authority. Larger farms do not gain code-writing, publication or credential privileges by virtue of size. Keep aggregate spend and review burden bounded across all installed roles and scoped instances; enforce the chosen limits with trusted native/provider controls, and reduce or pause activity when those limits cannot be maintained. A farm can suggest changes to its own cadence or scope based on recorded outcomes, but a maintainer reviews any operational change before it takes effect. The initial release does not require an autonomous scaling controller.

### The runtime boundary

Farm's steady-state operation consists of native gh-aw workflows executing in GitHub Actions, plus the already-approved project tools and dependencies necessary for the investigation. Distribution and configuration are primarily Markdown and native workflow frontmatter. No Farm-owned daemon, controller, API service, shared worker pool, message bus or persistent database is needed, even for a larger installation. GitHub and the configured engine provide the underlying services; their resource use and cost are real and remain the consumer's responsibility.

Prefer native facilities, then improvements contributed upstream, then the smallest reusable helper running within the existing job boundary. Any later proposal requiring a new runtime, durable coordination service or infrastructure to operate independently of gh-aw must demonstrate a concrete, observed limitation, alternatives tried, operational cost and an exit strategy. It is a separate architectural decision, not an automatic consequence of growth.

## 6. Investigation loop

Each run:

1. Reads its role and the optional `FARM.md`, discovers relevant authoritative sources and candidate instruments using repository conventions, and checks prior findings. Missing optional guidance does not block orientation. Missing evidence, unresolved authority or required prerequisites limits the claim or blocks the affected question.
2. Chooses one bounded territory and records why: importance, recent change, uncertainty, evidence weakness or time since last investigation. Maintainers can suggest subjects; autonomous selection should remain possible.
3. States a testable question and traces the relevant contract or independently established invariant into the actual production or public path. Uses existing deterministic checks, change history and relevant evidence (or a smaller model when useful) to screen; escalates to deeper model-assisted investigation only when consequence, tractability and expected incremental learning justify it.
4. Runs an approved instrument when practical, or carefully interprets directly relevant existing evidence. Records commands, revision, inputs, observations and limitations. Does not claim more than the instrument demonstrates.
5. Challenges its own interpretation with contrary evidence, a plausible alternative and, where appropriate, a discriminating or negative control.
6. Checks existing findings, fixes and reviewed observations for the underlying concern. Reopening a settled question requires changed circumstances or new evidence.
7. Produces an investigation record and at most one eligible finding. Quiet, duplicate, disproved, blocked and inconclusive outcomes remain valid and visible in the run record without opening issues. Completed quiet investigations include the coverage receipt described below.
8. A human reviews staged eligible findings, decides their disposition, and promotes useful learning before artifacts expire. Quiet receipts are available on demand or in an optional aggregate summary rather than requiring per-run review. Accepted implementation work follows the project's ordinary process.

The first version needs no ranking service or semantic index. Repository history, GitHub search and reviewed observations are the intended basis for bounded attention and history retrieval. Cache memory may assist orientation later, but eviction must not erase authoritative knowledge.

### Hunter

The Hunter looks for consequential incorrect behavior, unreachable public capabilities and weak verification that existing deterministic tools or CI have missed. Its contribution is the contextual choice of a property, production path, oracle, experiment or operation sequence, not running an expensive model in place of an existing linter, fuzzer or sanitizer. It should find a falsifiable hypothesis, not produce a generic missing-test inventory.

A confirmed defect requires a relevant contract or independently established invariant and an executable reproduction, failing test or concrete independently checkable counterexample. In a poorly documented project, establish the expectation from public interface semantics, external standards, existing supported-behavior tests or independently supported relationships between operations. Current implementation alone is not proof of intended behavior. If semantics remain ambiguous, investigate an independently established invariant or record uncertainty without publishing a defect. Evidence and specification gaps are not automatically defects.

Useful reusable investigation patterns include differential testing, property and metamorphic testing, mutation testing, state-machine exploration, fault injection, resource-lifetime analysis and public-path tracing. Start by describing these methods in shared Markdown when the role needs them; reuse existing tools and test harnesses rather than build a testing library.

### Gardener

Gardener is experimental and outside first-horizon product qualification. Reconsider it only after Hunter has demonstrated repeatable incremental value and a separate cost and review case beyond deterministic static analysis has been established. If pursued, use existing tools for mechanical checks and focus model-assisted work on demonstrated improvements they cannot supply. Initially limit scope to demonstrably safe transformations or well-tested boundaries where preservation can be established; do not treat generic style suggestions as findings.

The Gardener looks for fragmented responsibilities, contradictory representations, redundant decisions, obscure transitions, confusing tests and conflicting instructions. It reconstructs why the current structure exists, traces callers and important invariants, evaluates an alternative, and explains why the alternative reduces reasoning burden without weakening behavior.

It must identify necessary distinctions and verification obligations, not optimize line counts. Changing a contract, persistent format, or evidence standard is not simply cleanup. A local sketch is not evidence about the unchanged baseline. Require a bounded candidate diff or experiment, concrete benefit and checkable preservation of required behavior, including edge cases and indirect uses; existing CI passing is insufficient by itself. Use targeted characterization, differential or property tests, or a checkable equivalence argument as appropriate. Withhold proposals where preservation cannot be demonstrated. A well-supported recommendation to preserve a design is useful.

### Beyond the first two roles

Independent verification, performance, dependencies, compatibility, operational learning and architecture research are potential components. Treat them as territories or methods first. Introduce separate roles only when their questions, instruments, authority or review requirements differ materially. Increasing write authority requires its own design and release gate.

## 7. Evidence and learning

The shared record has a small human-readable structure: role, question, territory, baseline revision, workflow and run reference, contract, inspected paths, actual commands and inputs, observations, counterevidence, conclusion, limitations, related findings and disposition. Local modifications and their results are separate from the baseline. Engine and usage metadata can be linked from native gh-aw run artifacts rather than duplicated.

Run outcomes and finding kinds are distinct. Outcomes include actionable, disproved within scope, no new result, duplicate, inconclusive, blocked and failed. Recorded investigation topics may include suspected defects, evidence gaps, specification gaps and simplification proposals, but recording a topic does not make it eligible for unsolicited publication. A failed or interrupted workflow must not masquerade as a completed investigation.

A completed quiet run has a concise coverage receipt in the existing sanitized investigation record, derived from actual instrument results: question and scope (including baseline revision and paths or operations), experiments attempted and actually executed, relevant cases or bounds exercised, outcomes and limitations, and elapsed time and usage/cost where available. Do not equate experiment scope with code coverage or evidence of absence beyond that scope. Missing or estimated cost is labeled as such. Blocked, failed and inconclusive records identify what did not execute or could not be established. An optional low-noise aggregate view can use existing Actions run records to summarize distinct scenarios, results and spending; no new dashboard or per-run notification is required.

Use existing GitHub Actions artifacts for short-lived sanitized records. A staged finding previews an issue; it is not a published issue. The initial candidate stages issue creation separately from its bounded artifact upload. That upload is a real operation, not a preview, and needs review. No raw logs, patches, production data or sensitive findings should be uploaded by default. The consumer reviews actual log and artifact behavior before enabling sensitive territories.

Use ordinary issues for novel, consequential actionable findings after explicit approval of live publication. Hunter findings must include the baseline revision, assumptions, expected and observed behavior and reproduction instructions; an invented requirement, unsupported path or textual argument alone does not satisfy the gate. Deduplicate and cap unsolicited findings within a maintainer-approved attention budget; narrow or suspend activity if false positives impose excessive review costs. Requested run summaries and operational failures do not bypass the gate. Humans record a disposition: accept, investigate further, settle contract, reject or revisit on change.

Promote durable lessons as reviewed regression tests, property tests, assertions, benchmarks or reproducible experiments through the existing project verification suite. Ordinary issue, PR, commit and project documentation may retain rationale and experimental context, but do not promote Farm-generated prose into an authoritative memory store or create a dependency-tracking and revalidation notification scheme. Consult historical evidence only after checking its applicability to the current revision. Artifacts expire; cache memory is optional and non-authoritative. Do not create a second issue tracker or a Farm database.

The durable learning loop matters more than how many findings are published. Evaluate incremental value against existing CI and deterministic instruments, usefulness, reproducibility, duplication, distinct consequential scenarios exercised, cost across all outcomes, review burden and whether executable lessons survive later changes. Compare known cases and sample quiet outcomes when practical to check for missed problems. Use project-specific thresholds to change the distribution and depth of attention, narrow or pause activity before adding more workflows. Metrics diagnose the process; they are not agent quotas.

## 8. Authority, safety and defaults

The investigation process is not a security boundary. Trusted gh-aw configuration, its generated workflows, tool permissions, credential scopes and execution environment determine authority. Repository files, issues, logs and tool output can be adversarial; text discovered during investigation does not grant permissions. Never rely on an instruction to redact or refrain as the only control.

Executing project code, builds, containers or fuzzers requires an isolated, ephemeral, network-restricted sandbox with least-privilege credentials and only explicitly approved commands and destinations. Experimental processes must not receive model-provider credentials, repository write tokens, unrelated secrets or privileged access to the host or container daemon. Qualify hard wall-clock, token or equivalent provider-enforced model-usage, compute and concurrency limits outside the agent prompt; bound output, control publication and terminate experiments when a limit is reached. Use per-run and aggregate spend controls. If any required isolation or limit cannot be enforced by the chosen workflow, runner or engine, disable the affected experiment or unattended mode until it can. These are qualification requirements, not guarantees provided by the present package.

The experimental package starts with manual dispatch, staged issue creation, one finding per run, a 30-minute target workflow timeout and a shared concurrency group. It declares read permissions and a small shell command set; no general-purpose shell wildcard, production credentials, external connector or code-writing safe output is requested. Local evidence-file writes are necessary; they are not permission to persist repository changes. The consumer must verify the selected engine's actual tool enforcement and review the compiled jobs. Install or run on a reviewed default-branch revision, not an untrusted PR or fork. Add only the approved toolchain, dependencies and test commands a project needs. A discovered CI or build command is not an approved command; local Markdown cannot enlarge tool permissions. Sensitive investigations remain excluded unless explicitly and safely enabled.

The package requests a bounded run artifact (one Markdown record) independently of the staged issue. Native gh-aw may also retain prompts, execution and audit artifacts. Neither staged previews nor Actions logs/artifacts are guaranteed private. Do not run sensitive investigations without an approved private reporting path and verified log/data isolation. For suspected sensitive findings, stop and report only a safe indication that a maintainer needs to review privately. Never send raw evidence to an issue or output channel merely because a tool accepts it.

Explicitly suppress ordinary no-op and failure tracking issues; validate all diagnostic behavior against the selected gh-aw release. No auto-closing, grouping, assignment, implementation dispatch, code PR creation, branch pushes or merges. Live issue publication is a separate opt-in after staged validation. A prompt-based record template and native structural validation do not prove evidence quality; initial curation is human. If a hard publication gate cannot be expressed with native features, keep publication staged rather than introduce an unrestricted writer.

A run timeout and concurrency do not establish an aggregate monetary ceiling. Use native gh-aw credit controls where applicable and provider/repository spending and admission controls for the selected engine. Document what is enforced, what is estimated, and how to stop both workflows. No recurring unattended execution or live publication before budgets, ownership, reproducibility, review economics and the applicable execution and publication controls have been qualified.

## 9. Distribution and maintenance

Version the package and test it against a named compatible gh-aw release. Pin package references to reviewed releases or immutable commits. Do not hand-edit generated lock files. Compare install/compile output and permissions on upgrades, and preserve consumer-owned configuration. A consumer can remove the workflows and package-owned resources while keeping project knowledge and decisions.

The package should stay small: only shared methods and native workflow configuration until a proven gap requires more. Tests and reference installations are essential even when runtime code is absent. A new role, instrument or component should show reuse in more than one project, or a concrete reason it is broadly useful. Avoid an extensive catalog of untested templates.

## 10. References

- [gh-aw package manifest](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/)
- [gh-aw imports](https://github.github.com/gh-aw/reference/imports/)
- [Workflow structure](https://github.github.com/gh-aw/reference/workflow-structure/) and [frontmatter](https://github.github.com/gh-aw/reference/frontmatter/)
- [gh-aw tools](https://github.github.com/gh-aw/reference/tools/)
- [Safe outputs](https://github.github.com/gh-aw/reference/safe-outputs/) and [staged mode](https://github.github.com/gh-aw/reference/staged-mode/)
- [Artifacts](https://github.github.com/gh-aw/reference/artifacts/) and [cost management](https://github.github.com/gh-aw/reference/cost-management/)
- [GitHub Agentics](https://github.com/githubnext/agentics): evaluate existing workflows and reuse applicable methods instead of copying them wholesale.

The specific initial engine, compatibility floor, output enforcement and upgrade behavior are implementation decisions to be validated, not claims established by this design. See the [roadmap](roadmap.md).
