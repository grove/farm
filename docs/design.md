# Farm: product and architecture

Status: design proposal and experimental package candidate.

## 1. Product

Farm makes continuous software investigation a reusable capability. A project should install a farm in the same way it installs continuous integration, rather than implement the underlying machinery. The ideal consuming repository contributes only its domain knowledge and any genuinely domain-specific experiments.

A software factory starts from specified work. A software farm starts from questions about the software: what might be wrong, what is insufficiently demonstrated, and what makes the system harder to reason about than it needs to be? The result is better evidence, clearer contracts and more maintainable software, including well-supported decisions not to change anything.

Farm is GitHub-native. It is a distribution of reusable GitHub Agentic Workflows (gh-aw), shared investigation instructions, examples and operating practices. It is not an alternative to gh-aw, a hosted product, a multi-agent orchestration service or an automatic implementation factory.

## 2. Architectural constraints

1. Prefer upstream gh-aw features and existing community workflows. Extract a reusable component only when an actual consumer needs it. Contribute generic platform gaps upstream when practical.
2. Do not create a Farm CLI, installer, configuration interpreter, scheduler, agent runner, model adapter, database, web service, dashboard, MCP server or package registry for the initial product.
3. Prefer convention over configuration: existing repository artifacts are the default sources of project context. A short, optional project-owned `FARM.md` supplies missing domain knowledge and exceptions. Use native gh-aw frontmatter for executable settings. Do not invent a parallel YAML DSL, generic adapter SDK or plugin protocol.
4. A project can enable one role, both roles or later components independently. A role does not require another role to be online, an autonomous task queue or a centrally hosted service.
5. Every reusable role should be installable without changes to its investigation method. Consumer-specific commands and policy changes belong in documented configuration and native gh-aw extension points. If consumers need divergent copies of generic methods, improve the shared component.
6. Any proposed executable code must identify the upstream feature it cannot use, the minimum behavior it adds, an owner and a removal condition. A short script may be justified to enforce a real invariant; fewer files are not an excuse to hide complexity in prompts.
7. Every installation is owned by the consuming repository. No credentials, code, findings or telemetry are sent to Farm as part of ordinary operation. Engine/provider processing and configured network destinations remain subject to the consumer's own security review.

## 3. Layers and ownership

| Layer | Farm distributes | Consuming project supplies |
| --- | --- | --- |
| Roles | Hunter and Gardener missions and sensible starting defaults | Optional role selection, emphasis or local rules |
| Methods | Hypothesis testing, counterevidence, preservation analysis, findings and quiet outcomes | Existing contracts, domain invariants and any missing knowledge or exceptions |
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

Native distribution is the starting point: a root `aw.yml`, two installable workflow sources outside the executable workflow directory, and shared Markdown copied as package resources. Consumers compile and review their lock files. Use native imports to share methods. Once package installation and updates have been exercised, consider pinned remote imports or parameterized components if they simplify customization further. Never load changing upstream instructions at runtime from an unpinned branch.

For installation, customization, update and removal, prefer the corresponding gh-aw commands. For a consumer that wants just one role, validate gh-aw's individual-workflow installation behavior with its resource dependencies or offer a small native subpackage. Do not add a custom installer to solve it.

## 5. Investigation loop

Each run:

1. Reads its role and the optional `FARM.md`, discovers relevant authoritative sources and candidate instruments using repository conventions, and checks prior findings. Missing optional guidance does not block orientation. Missing evidence, unresolved authority or required prerequisites limits the claim or blocks the affected question.
2. Chooses one bounded territory and records why: importance, recent change, uncertainty, evidence weakness or time since last investigation. Maintainers can suggest subjects; autonomous selection should remain possible.
3. States a testable question and traces the relevant contract into the actual production or public path.
4. Runs an approved instrument when practical, or carefully interprets directly relevant existing evidence. Records commands, revision, inputs, observations and limitations. Does not claim more than the instrument demonstrates.
5. Challenges its own interpretation with contrary evidence, a plausible alternative and, where appropriate, a discriminating or negative control.
6. Checks existing findings, fixes and reviewed observations for the underlying concern. Reopening a settled question requires changed circumstances or new evidence.
7. Produces an investigation record and at most one eligible finding. Quiet, duplicate, disproved, blocked and inconclusive outcomes remain valid and visible in the run record without opening issues.
8. A human reviews the result, decides its disposition, and promotes useful learning before artifacts expire. Accepted implementation work follows the project's ordinary process.

The first version needs no ranking service or semantic index. Repository history, GitHub search and reviewed observations are sufficient until experience shows otherwise. Cache memory may assist orientation later, but eviction must not erase authoritative knowledge.

### Hunter

The Hunter looks for incorrect behavior, unreachable public capabilities, weak evidence and unresolved specifications. It should find a falsifiable hypothesis, not produce a generic missing-test inventory. A confirmed defect requires a relevant contract and reproducible contradictory observation. A suspected defect records uncertainty. Evidence and specification gaps are not automatically defects.

Useful reusable investigation patterns include differential testing, property and metamorphic testing, mutation testing, state-machine exploration, fault injection, resource-lifetime analysis and public-path tracing. Start by describing these methods in shared Markdown when the role needs them; reuse existing tools and test harnesses rather than build a testing library.

### Gardener

The Gardener looks for fragmented responsibilities, contradictory representations, redundant decisions, obscure transitions, confusing tests and conflicting instructions. It reconstructs why the current structure exists, traces callers and important invariants, evaluates an alternative, and explains why the alternative reduces reasoning burden without weakening behavior.

It must identify necessary distinctions and verification obligations, not optimize line counts. Changing a contract, persistent format, or evidence standard is not simply cleanup. A local sketch is not evidence about the unchanged baseline. A well-supported recommendation to preserve a design is useful.

### Beyond the first two roles

Independent verification, performance, dependencies, compatibility, operational learning and architecture research are potential components. Treat them as territories or methods first. Introduce separate roles only when their questions, instruments, authority or review requirements differ materially. Increasing write authority requires its own design and release gate.

## 6. Evidence and learning

The shared record has a small human-readable structure: role, question, territory, baseline revision, workflow and run reference, contract, inspected paths, actual commands and inputs, observations, counterevidence, conclusion, limitations, related findings and disposition. Local modifications and their results are separate from the baseline. Engine and usage metadata can be linked from native gh-aw run artifacts rather than duplicated.

Run outcomes and finding kinds are distinct. Outcomes include actionable, disproved within scope, no new result, duplicate, inconclusive, blocked and failed. Finding kinds include confirmed defect, suspected defect, evidence gap, specification gap and simplification proposal. A failed or interrupted workflow must not masquerade as a completed investigation.

Use existing GitHub Actions artifacts for short-lived sanitized records. A staged finding previews an issue; it is not a published issue. The initial candidate stages issue creation separately from its bounded artifact upload. That upload is a real operation, not a preview, and needs review. No raw logs, patches, production data or sensitive findings should be uploaded by default. The consumer reviews actual log and artifact behavior before enabling sensitive territories.

Use ordinary issues for actionable findings after explicit approval of live publication. Humans record a disposition: accept, investigate further, settle contract, reject or revisit on change. Promote durable lessons into a maintainer-owned observations file or existing documentation. Artifacts expire; cache memory is optional and non-authoritative. A record's revision and scope determine whether it still applies. Do not create a second issue tracker or a Farm database.

The durable learning loop matters more than how many findings are published. Review usefulness, reproducibility, duplication, coverage of important territories, cost, review burden and whether lessons survive later changes. Metrics diagnose the process; they are not agent quotas.

## 7. Authority, safety and defaults

The investigation process is not a security boundary. Trusted gh-aw configuration, its generated workflows, tool permissions, credential scopes and execution environment determine authority. Repository files, issues, logs and tool output can be adversarial; text discovered during investigation does not grant permissions. Never rely on an instruction to redact or refrain as the only control.

The experimental package starts with manual dispatch, staged issue creation, one finding per run, a 30-minute target workflow timeout and a shared concurrency group. It declares read permissions and a small shell command set; no general-purpose shell wildcard, production credentials, external connector or code-writing safe output is requested. Local evidence-file writes are necessary; they are not permission to persist repository changes. The consumer must verify the selected engine's actual tool enforcement and review the compiled jobs. Install or run on a reviewed default-branch revision, not an untrusted PR or fork. Add only the approved toolchain, dependencies and test commands a project needs. A discovered CI or build command is not an approved command; local Markdown cannot enlarge tool permissions. Sensitive investigations remain excluded unless explicitly and safely enabled.

The package requests a bounded run artifact (one Markdown record) independently of the staged issue. Native gh-aw may also retain prompts, execution and audit artifacts. Neither staged previews nor Actions logs/artifacts are guaranteed private. Do not run sensitive investigations without an approved private reporting path and verified log/data isolation. For suspected sensitive findings, stop and report only a safe indication that a maintainer needs to review privately. Never send raw evidence to an issue or output channel merely because a tool accepts it.

Explicitly suppress ordinary no-op and failure tracking issues; validate all diagnostic behavior against the selected gh-aw release. No auto-closing, grouping, assignment, implementation dispatch, code PR creation, branch pushes or merges. Live issue publication is a separate opt-in after staged validation. A prompt-based record template and native structural validation do not prove evidence quality; initial curation is human. If a hard publication gate cannot be expressed with native features, keep publication staged rather than introduce an unrestricted writer.

A run timeout and concurrency do not establish an aggregate monetary ceiling. Use native gh-aw credit controls where applicable and provider/repository spending and admission controls for the selected engine. Document what is enforced, what is estimated, and how to stop both workflows. No unattended operation before the budget and ownership decisions are in place.

## 8. Distribution and maintenance

Version the package and test it against a named compatible gh-aw release. Pin package references to reviewed releases or immutable commits. Do not hand-edit generated lock files. Compare install/compile output and permissions on upgrades, and preserve consumer-owned configuration. A consumer can remove the workflows and package-owned resources while keeping project knowledge and decisions.

The package should stay small: only shared methods and native workflow configuration until a proven gap requires more. Tests and reference installations are essential even when runtime code is absent. A new role, instrument or component should show reuse in more than one project, or a concrete reason it is broadly useful. Avoid an extensive catalog of untested templates.

## 9. References

- [gh-aw package manifest](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/)
- [gh-aw imports](https://github.github.com/gh-aw/reference/imports/)
- [Workflow structure](https://github.github.com/gh-aw/reference/workflow-structure/) and [frontmatter](https://github.github.com/gh-aw/reference/frontmatter/)
- [gh-aw tools](https://github.github.com/gh-aw/reference/tools/)
- [Safe outputs](https://github.github.com/gh-aw/reference/safe-outputs/) and [staged mode](https://github.github.com/gh-aw/reference/staged-mode/)
- [Artifacts](https://github.github.com/gh-aw/reference/artifacts/) and [cost management](https://github.github.com/gh-aw/reference/cost-management/)
- [GitHub Agentics](https://github.com/githubnext/agentics): evaluate existing workflows and reuse applicable methods instead of copying them wholesale.

The specific initial engine, compatibility floor, output enforcement and upgrade behavior are implementation decisions to be validated, not claims established by this design. See the [roadmap](roadmap.md).
