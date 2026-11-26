# Farm: Long-term vision

*Continuous cultivation of software*

**Status:** Proposed vision. This document describes a direction, not a commitment to features or a change to the current architecture.

## 1. The vision

Farm should make continuous cultivation a normal part of software development, just as continuous integration has made continuous verification a normal part of development.

Software is never finished in the sense that our understanding of it is complete. There are always assumptions that can be challenged, behaviors that can be better demonstrated, and designs that can be simplified.

Farm gives a project a way to pursue these questions continuously, systematically and at a cost proportionate to their importance. The aim is software that becomes **more correct, simpler, better understood and easier to evolve over time**.

Maintainer attention is Farm's scarcest resource. A plausible but incorrect finding can take far longer to disprove than it took an agent to produce. Farm must save more engineering attention than it consumes, including the time spent rejecting its mistakes. If maintainers have to investigate Farm's claims for it, Farm has failed.

Farm should be useful even when it finds nothing to change, and should be quiet when there is nothing worth a maintainer's attention.

## 2. Why a software farm?

AI is making it easier to implement software. But implementation is only one part of engineering. Knowing what to build, what to preserve, what to remove, and what evidence to trust is often the harder problem.

As the capacity to produce code grows, the capacity to exercise judgment must grow with it.

A software factory starts with work to be done. A software farm starts with a question worth investigating.

| Software factory | Software farm |
| --- | --- |
| Implements a chosen change | Discovers whether a change is worthwhile |
| Works toward a defined outcome | Investigates an uncertain outcome |
| Produces working software | Produces evidence, knowledge and recommendations |
| Measures successful delivery | Measures useful learning |

The two are complementary. A farm can inform a factory, and a factory can act on findings accepted by maintainers. The farm can then return to the changed software with fresh questions.

Farm should not become another software factory. Its distinctive contribution is improving the understanding on which engineering decisions depend.

## 3. The work of a farm

Farm starts with two complementary perspectives on the same evolving software system.

| Perspective | Central question | Contribution |
| --- | --- | --- |
| **Hunter — Correctness** | What might be wrong or insufficiently demonstrated? | Reproducible evidence of defects, violated contracts and material gaps in verification. |
| **Gardener — Simplicity** | What could be easier to understand, change or verify? | Demonstrated opportunities to reduce complexity while preserving important behavior. |

Hunter and Gardener should earn trust through their findings, not the breadth of their missions. Neither should produce speculative critiques or generic recommendations just to stay active.

Future directions may include knowledge curation, independent re-examination, performance research and system health, where useful questions can be answered with evidence. Explore these first as methods within the existing roles, and introduce a separate role only if experience demonstrates a distinct need.

## 4. Investigation as a discipline

All Farm investigations should share a simple method:

1. Select a bounded, consequential question and establish the relevant contract.
2. Use inexpensive checks and existing evidence to decide whether deeper inquiry is justified.
3. State a hypothesis, define what would falsify it, and set a resource budget.
4. Examine the actual system with authorized instruments; seek counterevidence.
5. Validate the result, record its limits and check for duplicates.
6. Publish a finding only if it meets the evidence and materiality requirements below; otherwise stop quietly.

### Proof before interruption

A finding is not ready simply because it sounds convincing. Before asking a maintainer to act, Farm must make the claim both consequential and inexpensive to check.

For Hunter, a finding must include an executable reproduction, a failing test, or a concrete, checkable counterexample to an identified contract or verification claim. It must identify the affected revision, relevant assumptions, expected and observed behavior, and a way to check the result. A failing test is not sufficient if it encodes an invented requirement or exercises only a path the project does not support. Textual arguments alone are not a sufficient basis for an unsolicited finding.

For Gardener, a simplification must be demonstrated rather than asserted: for example, through a bounded candidate diff or experiment, concrete evidence of reduced complexity, and verification that relevant behavior and constraints are preserved. A smaller line count or a preference for another design is not enough.

When evidence is incomplete, retain the uncertainty in the bounded run record if useful. Do not turn a suspicion into an issue or send a maintainer on a speculative investigation. Requested run summaries and operational failures are distinct from actionable findings; neither should be used to bypass the publication gate. Sensitive findings must follow the project's authorized disclosure process.

The cost of checking a finding matters as much as the cost of discovering it. Deduplicate against known issues, use staged publication until the approach is qualified, and keep unsolicited findings within a maintainer-approved attention budget. If a farm repeatedly produces costly false positives, narrow or suspend its activity.

### Spend compute where it matters

Deep investigation is expensive, particularly when it repeatedly concludes that no change is needed. A farm should spend resources in proportion to the consequence of the question, the likelihood that available instruments can resolve it, and the expected value of the information gained.

Prefer a tiered approach:

- **Cheap screening.** Use existing deterministic checks, change history, relevant prior evidence and, where useful, smaller models to narrow the search. Screening signals are leads, not findings.
- **Focused investigation.** Explore a specific lead with bounded context, approved tools and a modest allowance. Stop when the question is resolved or meaningful progress is unlikely.
- **Deep investigation.** Use more capable models, larger experiments or independent replication only when the risk, uncertainty and potential value justify the additional cost.

Projects should set per-run and cumulative budgets for model usage, compute and experiments, and track spending across all outcomes, including quiet and inconclusive runs. A quiet outcome can be valuable, but repeatedly spending heavily to reconfirm stable behavior is not a sustainable operating model. Reduce cadence, narrow the scope or stop when the marginal value no longer justifies the cost.

Silence is better than a weak finding. Uncertainty is better than false confidence. Neither requires unbounded investigation.

## 5. Learning that accumulates without going stale

The long-term value of Farm comes from what a project learns over many investigations, not from any individual agent run. But retained knowledge is a maintenance obligation. Stale conclusions can misdirect future investigations and create more work than they save.

Prefer **executable memory**: regression tests, property tests, assertions and reproducible experiments that preserve an important lesson in a form that can be checked against the evolving system. Where possible, promote reviewed findings into the project's existing verification suite rather than create another collection of agent-authored notes. Executable checks still need review; a passing test does not establish a contract it was never designed to exercise.

Some knowledge, such as design rationale and the limits of an experiment, is better expressed in prose. Use existing specifications, architecture decisions and scoped guidance sparingly; do not build a parallel knowledge base by default.

Every retained conclusion in prose must identify:

- The authoritative contract or assumption and the revision at which the evidence was obtained.
- The evidence or verification procedure that supports it.
- The exact paths, symbols, contract versions or other versioned dependencies whose change invalidates it, or the baseline commit if a narrower boundary cannot be established.

A change to a declared dependency makes the conclusion stale until it is checked again. If its validity cannot be established, treat it as historical context, not current truth. Prefer existing repository and CI mechanisms to detect changes and rerun checks; do not require a new memory service.

The project owns this knowledge. An agent's observation is not an authoritative contract until the relevant people have reviewed it. Promote only lessons that justify their ongoing maintenance cost, and retire those that no longer apply.

Over time, Farm should spend less effort rediscovering what is known without becoming overconfident in what used to be true.

## 6. How cultivation and delivery work together

The relationship can be expressed as a simple loop:

**Investigate → learn → decide → implement → verify → investigate again.**

Consider a database that promises to recover committed data after an interruption. Hunter identifies a consequential recovery scenario, runs an authorized, bounded experiment and produces a failing reproduction tied to the recovery contract and the tested revision. A maintainer can rerun the reproduction rather than reconstruct the agent's reasoning. If the finding is accepted, a developer or software factory implements a correction. A fresh investigation checks the original claim against the new version, and the project can preserve the lesson as a regression test.

Farm is valuable at the discovery, learning and re-examination stages. It does not need to own the implementation stage. GitHub issues and ordinary development processes can connect these activities; an external system such as Gas City could also implement accepted work.

This connection should remain optional. Findings must never become implementation instructions merely because an agent produced them. Maintainers retain authority over priorities, contracts, changes and release decisions.

## 7. A small foundation that can grow

Farm should remain a lightweight, reusable collection of investigation practices and native workflows, rather than a new infrastructure platform.

The initial architecture uses GitHub Agentic Workflows. A project should be able to install a farm, reuse its existing documentation and verification tools, and begin with a single manually triggered investigation. No Farm-operated service or database should be necessary.

Investigations that execute project code, builds, containers or fuzzers must run in isolated, ephemeral, network-restricted sandboxes with least-privilege credentials and only explicitly authorized commands and destinations. Experimental code must not receive model-provider credentials, repository write tokens, unrelated secrets or privileged access to the host or container daemon. Enforce hard wall-clock, token or provider-enforced model-usage, compute and concurrency limits outside the agent's instructions, with bounded output and controlled publication. Terminate experiments when limits are reached. If the selected workflow, runner or engine cannot enforce the required isolation and limits, disable that experiment or unattended mode until it can; do not substitute a prompt-based promise. These are requirements to qualify, not claims that the current package already satisfies every control.

As needs grow, a project may investigate more areas, add specialized methods or authorize deeper experiments. Capacity should increase only after the existing farm has demonstrated value within its review and cost budgets. Each capability should be useful on its own.

The investigation methodology should be expressed in terms that are not unnecessarily tied to a particular engine. But Farm should not build a generic runtime abstraction or maintain multiple execution platforms in anticipation of future needs. A native Gas City distribution, or any alternative runtime, would be a separate architectural decision justified by actual requirements and equivalent safeguards.

A mature farm should also be able to scale down. A stable project may benefit more from an occasional well-chosen question than from constant activity.

## 8. Principles that should endure

- **Proof before interruption.** Maintainer attention is a scarce resource. Do not publish an actionable finding without concrete, checkable evidence that makes it economical to review. The farm bears the burden of demonstrating the claim.
- **Evidence before action.** Distinguish hypotheses, observations, supported conclusions and project decisions. Do not confuse a compelling explanation with a demonstrated result.
- **Executable memory before prose.** Preserve learning in checks that can run against changing software. Make retained prose evidence-linked, invalidatable and subject to revalidation.
- **Value before compute.** Use the cheapest adequate method, bound every investigation and escalate only when expected learning justifies additional resources.
- **Learning before output.** Findings are useful only if they improve understanding or inform worthwhile decisions. No issue or activity quotas.
- **Simplicity before machinery.** Prefer existing repository conventions, tools and platform capabilities to new abstractions and infrastructure.
- **Local ownership and enforced authority.** Projects control their contracts, data, credentials, retained knowledge, budgets and decisions. Investigation does not grant permission to publish sensitive findings, change production code or expand access. Enforce isolation, resource limits and permissions outside agent prompts.
- **Gradual, reversible growth.** Increase scope, depth and autonomy only after evidence of value and a review of risks, costs and maintainer effort. Keep meaningful human oversight.

## 9. A possible path forward

These are development horizons, not release dates. Progress depends on what can be demonstrated in real projects.

| Horizon | Focus | Evidence of readiness |
| --- | --- | --- |
| **Establish the foundation** | Qualify installation, isolation, hard resource limits and staged publication; validate Hunter, Gardener and onboarding. | Findings are reproducible and inexpensive to review; enforced limits hold; maintainers choose to continue using Farm. |
| **Make learning cumulative** | Improve evidence quality, deduplication, executable memory and invalidation of retained conclusions. | Reviewed lessons survive code changes without accumulating stale context or disproportionate maintenance work. |
| **Broaden cultivation** | Add specialized evidence-based methods, improve screening and escalation, and connect accepted findings to delivery systems. | New capabilities demonstrate additional value within review and compute budgets. |
| **Grow with complex systems** | Explore deeper and coordinated investigations, cross-project questions and optional alternative execution environments. | Observed needs justify the additional coordination, cost and security requirements. |

The first priority is earning the right to interrupt maintainers. Unattended publication and recurring execution should follow demonstrated evidence quality, affordable review and qualified execution boundaries, not precede them.

New capabilities should earn their place through experience, not simply because they fit the farm metaphor.

## 10. How we know Farm is succeeding

Farm is successful when maintainers can make better engineering decisions with less total effort because of what it discovers. Protecting maintainer attention is a condition of continued operation, not a secondary quality metric.

Evaluate Farm on a small set of outcomes:

- **Review economics.** Time spent validating, dismissing and deduplicating findings; the precision and materiality of published findings; and whether the value of accepted findings outweighs the review burden. A single expensive false positive can matter more than several cheap successes.
- **Compute economics.** Model and experiment cost across all runs, cost per accepted useful finding or other validated learning outcome, and the marginal benefit of deeper investigations. Include spending on quiet, blocked and inconclusive runs.
- **Software outcomes.** Reproducible findings, important guarantees that become better verified, and simplifications that preserve required behavior while reducing maintenance effort.
- **Knowledge health.** Retained executable checks that remain useful, the maintenance cost of retained prose, stale conclusions prevented from influencing new work, and less repeated investigation.
- **Safety and coverage.** Whether isolation, permission and resource limits hold, and whether screening or publication rules hide important problems. Evaluate against known cases and sampled quiet outcomes when practical.

Use these measures to set project-specific operating thresholds and to decide whether to increase, narrow, pause or stop activity. No universal cost per finding or false-positive threshold is appropriate for every project, and quality measures must not become quotas imposed on agents.

The ultimate test is not whether Farm appears busy. It is whether projects are better understood, more trustworthy and easier to evolve because Farm exists, without imposing an unsustainable burden on maintainers.

## 11. The ambition

Continuous integration helps us know whether a change satisfies the checks we have defined. Continuous cultivation should help us discover whether those checks are sufficient, whether our assumptions remain valid, and whether we are investing effort in the right things.

Farm should make this kind of inquiry accessible to ordinary software projects, from a small library to a complex long-lived system.

**A software factory helps us build software. A software farm helps us understand what software is worth building, what is worth preserving, and what can be trusted.**

---

Related documents: [Farm README](https://github.com/grove/farm), [product and architecture](https://github.com/grove/farm/blob/main/docs/design.md), and [shared investigation method](https://github.com/grove/farm/blob/main/workflows/shared/farm-investigation.md). These describe the existing design; this proposed vision extends it without superseding it.
