# Onboarding an existing project

Status: proposed method and experimental gh-aw workflow candidate. The package and workflow must be qualified before use; see the [release gates](roadmap.md).

Farm should help maintainers introduce continuous software investigation into an existing repository. Most of the necessary knowledge already exists. The work is to make it easier to retrieve, distinguish supported behavior from implementation accidents, connect important promises to executable evidence, and resolve only the gaps that affect the first investigations.

## 1. Install and review the execution boundary

Install the tested Farm package using native gh-aw tooling, select an available engine and review authentication, tool permissions, network access, artifacts, budget and generated workflows. No role should run automatically during installation. Use a reviewed default-branch revision, not a fork or untrusted PR. Limit the onboarding survey to information that may be processed by the configured engine and exposed in its run records. Do not provide production data or credentials.

A project that already has adequate, discoverable domain guidance can skip the survey. Otherwise, manually dispatch **Farm Onboard**. This is an optional-use operation distributed with the package, not a recurring agent. It is intended to read the repository and its accessible history, not execute arbitrary project commands or make changes. It proposes one sanitized Markdown artifact. There is no issue/PR publication or automatic activation. The artifact is a real upload and may be visible to repository readers; review the native logging and retention behavior first. Review the final safe-output handler result and delivered artifact, not just the workflow status or staging artifact. A missing report is an incomplete onboarding run even if the Actions status is green.

## 2. Survey existing knowledge

The survey starts from normal repository conventions: README, contributor and agent instructions, linked contracts and architecture, decisions, test documentation, build manifests and CI, relevant code, issues and history. It also reads existing `FARM.md` and reviewed observations if present. These are discovery hints, not required files. Trace the important public promises into the production paths that appear responsible for them. Prefer one or two consequential territories to a broad inventory of files or components.

For each candidate knowledge statement, distinguish:

| Status | Meaning |
| --- | --- |
| Documented contract | A current source explicitly establishes a supported promise; reference the source and scope. |
| Observed implementation | Code or recorded behavior appears to do something; this is not automatically a promise. |
| Proposed or historical | A design, prior decision, issue or superseded behavior needs context before it can be relied upon. |
| Unresolved | Sources disagree, a contract is absent, or maintainer judgment is needed. |

Retain source paths/sections or issue/commit links, inspected revision, relevant context, and any limits on the observation. Do not declare one document authoritative just because it has a familiar name. A test demonstrates only what it exercises; reading a CI command does not show that it has passed or authorize execution. Do not silently turn observations into contracts or settle conflicts.

A larger repository may require several scoped surveys. Narrow the scope when the available context or run budget is insufficient; identify what was not examined rather than imply completeness.

## 3. Distill knowledge for investigation

Optimize for the questions the Hunter and Gardener need to answer, not for a comprehensive prose summary. A useful entry connects a claim to the place that defines it, the path that implements it and the evidence that could challenge it.

An illustrative small knowledge entry (not a required schema):

| Item | Example for a hypothetical storage library |
| --- | --- |
| Territory | Durability after acknowledgment |
| Contract | An acknowledged write survives a supported restart; see the current durability section of the specification. |
| Production path | Public write API → durable commit → restart/replay. Link to the owning code. |
| Candidate instrument | Existing process-restart integration test; link to its documented invocation and prerequisites. |
| Discriminating observation | Compare the complete acknowledged data set with recovered data after a real restart. |
| Evidence limit | An in-memory reset or row-count comparison alone cannot establish the promise. |
| Domain rule | Do not substitute a weaker model for the advertised real-process recovery guarantee. |
| Open question | Which failures are explicitly covered? Ask only if the source does not settle this. |

The survey should collect only the most useful entries for the chosen starting scope. Typical categories are:

- Purpose, vocabulary, public interfaces and authoritative contracts.
- Core invariants, responsibilities, boundaries and necessary complexity.
- Existing instruments, candidate commands, prerequisites, independent oracles and evidence limits.
- Important failure modes, unresolved disagreements and compatibility constraints.
- Domain-specific Hunter rules, Gardener preservation rules and shared exclusions not already discoverable.

Use concise Markdown, meaningful headings and links rather than repeat entire documents or code. Preserve source provenance. Link to existing artifacts first; improve existing documentation when possible. A complex project may benefit from a compact territory or invariant index that points to scoped documentation. Do not invent a mandatory catalog, ontology, graph, vector database or separate configuration schema.

## 4. Produce a small onboarding proposal

The [Farm Onboard workflow](../workflows/farm-onboard.md) proposes one run-scoped Markdown record. It should contain:

- Survey scope, baseline revision and inspected sources; unexamined areas and limitations.
- A short map of the relevant authoritative entry points and uncertainties.
- One or two proposed starter territories, with contract/production path/candidate evidence and limitations.
- A small proposed set of documentation or `FARM.md` additions only where useful; separate supported statements from questions. If existing material is sufficient, say so and propose no new file.
- A prioritized list of questions that prevent a useful investigation or require domain judgment. Ask specific questions with evidence and why the answer matters.
- Candidate instrument authorization needs, readiness blockers and suggested first Hunter and/or Gardener questions. Mark discovered but unapproved or untested commands accordingly.

Do not produce a large repository refactoring plan, a new feature backlog or a giant automatically generated knowledge file. Contradictions and missing instruments are gaps to be reviewed, not automatic product defects. Cleanup should be proportionate to the first investigations.

## 5. Review and encode only what matters

The maintainer reviews the proposal against current contracts and corrects assumptions. Start with the following review questions:

- Which promises are actually supported, and what are the important exceptions?
- Are the initial territories and production paths correct?
- Which evidence is independent, discriminating and safe to reproduce? What does it not establish?
- What necessary complexity must be preserved? Are there excluded areas or data?
- Which ambiguous claims are blockers, and which can remain open?
- Who reviews the first investigations, which tools are explicitly approved, and what are the cost and exposure boundaries?

Prefer reviewing a proposed answer to writing a long questionnaire from scratch. If a key answer is not in the repository, put it in the existing authoritative specification, ADR, test documentation or relevant instructions when appropriate. Use a short optional root `FARM.md` for cross-cutting links and farm-specific rules that do not naturally belong elsewhere. For larger projects, link to existing component-level knowledge rather than copy it into the root file. The [example](../config/project.example.md) is a convenience, not a required format.

Commit accepted changes through the project's normal review. Keep proposals, inferences and open questions visibly distinct from accepted facts. Package updates must preserve the project's knowledge. A maintainer may decline every proposed addition if the repository already supplies enough guidance.

A candidate instrument only becomes executable when the maintainer reviews its side effects, prerequisites, data and resources and authorizes it through native gh-aw configuration. Markdown rules and an onboarding artifact cannot increase permissions, schedule workflows or grant access to credentials.

## 6. Calibrate and activate gradually

A selected territory is ready for the first investigation when a maintainer can explain the relevant contract or explicitly unresolved question, identify its production path, choose an approved instrument or directly relevant existing evidence, and describe what the evidence can and cannot establish. The authority boundary, review owner and limits must be reviewed. Readiness is scoped, not a whole-repository certification.

Manually run the enabled Hunter and/or Gardener with findings staged. Choose real questions from the proposal. An investigation that disproves a hypothesis or justifies retaining a design is a useful result. Review whether the workflow followed the accepted knowledge and reached a conclusion within the evidence. Fix material misunderstandings, gaps or unsafe settings before scheduling. Retain useful lessons using the existing review process; no discovery quota and no requirement to fix every historical defect.

Only enable a modest schedule after the operational and evidence gates pass. Live issue publication is a separate, explicit decision. Larger projects can expand territories, frequency or specialized roles without a different runtime or a fresh onboarding from scratch.

## 7. Keep knowledge current

Onboarding establishes a starting point, not a static context dump. The roles should check applicable sources at their current baseline, use reviewed observations, and flag a material contradiction or stale reference. Re-run the survey manually after a significant contract, ownership or verification change, or focus a follow-up on one territory. Compare to current guidance, retain only useful updates, and never overwrite accepted domain rules automatically.

Measure onboarding by maintainer effort, duplicated information, accuracy and usefulness in subsequent investigations, not by how many pages of knowledge it creates. A farm that adapts to existing knowledge is better than a project that must maintain a second version of itself for its agents.
