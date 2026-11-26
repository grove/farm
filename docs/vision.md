# Farm: Long-term vision

*Continuous cultivation of software*

**Status:** Proposed direction, not a commitment to features or a change to the current architecture.

## 1. The vision

Farm should make continuous cultivation a normal part of software development, just as continuous integration has made continuous verification a normal part of development.

Software may work well while our understanding of it remains incomplete. Farm systematically challenges assumptions, tests guarantees and explores simplifications so software becomes **more correct, simpler, better understood and easier to evolve**.

Maintainer attention is Farm's scarcest resource. A plausible but incorrect finding can take far longer to disprove than it took an agent to produce. Farm must save more engineering attention than it consumes, including the time spent rejecting its mistakes. If maintainers have to investigate Farm's claims for it, Farm has failed.

## 2. Why a software farm?

AI is making code cheaper to produce, but knowing what to build, what to preserve and what to trust remains hard. As implementation capacity grows, so must our capacity for judgment.

A software factory starts with work to be done. A software farm starts with a question worth investigating.

| Software factory | Software farm |
| --- | --- |
| Implements a chosen change | Discovers whether a change is worthwhile |
| Works toward a defined outcome | Investigates an uncertain outcome |
| Produces working software | Produces evidence, knowledge and recommendations |
| Measures successful delivery | Measures useful learning |

Farm should complement software factories, not become one.

## 3. The work of a farm

| Perspective | Central question | Contribution |
| --- | --- | --- |
| **Hunter — Correctness** | What might be wrong or insufficiently demonstrated? | Reproducible evidence of defects, violated contracts and material verification gaps. |
| **Gardener — Simplicity** | What could be easier to understand, change or verify? | Demonstrated opportunities to reduce complexity while preserving required behavior. |

Future directions include knowledge curation, independent re-examination, performance research and system health where questions can be answered with evidence. Explore these as methods within existing roles before considering any new role with a demonstrated, distinct need.

## 4. Investigation as a discipline

All investigations follow a common method:

1. Select a bounded, consequential question and identify the relevant contract or invariant.
2. Screen using cheap checks and existing evidence.
3. State a hypothesis, what would falsify it and a resource budget.
4. Examine the actual system with authorized instruments and seek counterevidence.
5. Validate the result, record its limits and check for duplicates.
6. Publish only if the evidence and materiality requirements are met; otherwise stop quietly.

### Proof before interruption

Farm bears the burden of making a consequential claim inexpensive to check.

**Hunter.** Require an executable reproduction, a failing test or a concrete, checkable counterexample to an identified contract or verification claim. Include the affected revision, assumptions, expected and observed behavior, and reproduction instructions. A test that invents a requirement or exercises only an unsupported path is insufficient; textual arguments alone do not justify unsolicited findings.

When a domain contract is missing or unclear, restrict defect claims to independently established invariants, such as unexpected panics on supported inputs, demonstrable resource leaks, data races that violate the language or runtime rules, or security boundary violations. Do not infer business rules from implementation details; record contract uncertainty in the run record instead of presenting it as a defect.

**Gardener.** Simplification is more subjective than correctness and generally carries a higher review risk. Initially restrict investigations to demonstrably safe mechanical wins (such as dead code, redundant state and unreachable branches) or well-tested boundaries where preservation can be established. Require a bounded candidate diff or experiment, concrete evidence of reduced complexity, and evidence that relevant behavior and constraints are preserved, including edge cases and indirect uses. Passing existing CI or reducing line count is not sufficient; use targeted characterization, differential or property tests, or a checkable equivalence argument as appropriate. Withhold findings when preservation cannot be demonstrated; broaden scope only after earning trust.

Retain useful uncertainty in bounded run records, not speculative issues. Requested run summaries and operational failures must not bypass the finding gate; sensitive findings follow the project's authorized disclosure process.

Deduplicate against known issues, stage publication until qualified, and bound unsolicited findings by a maintainer-approved attention budget. Narrow or suspend activity when false positives impose excessive review costs.

### Spend compute where it matters

An investigation is worth its cost when the consequence of the question, its tractability and the expected value of learning justify the resources required. Use tiered execution:

- **Cheap screening:** existing deterministic checks, change history, relevant prior evidence and, where useful, smaller models; signals are leads, not findings.
- **Focused investigation:** bounded context, approved tools and a modest allowance; stop when resolved or meaningful progress is unlikely.
- **Deep investigation:** more capable models, larger experiments or independent replication only when risk, uncertainty and expected value justify escalation.

Set per-run and cumulative budgets for model usage, compute and experiments. Quiet and inconclusive outcomes can be valuable, but repeated expensive reconfirmation of stable behavior is not sustainable. Reduce cadence, narrow scope or stop when marginal value no longer justifies cost.

## 5. Learning that accumulates without going stale

Retained knowledge is a maintenance obligation. Prefer **executable memory**: reviewed regression tests, property tests, assertions and reproducible experiments in the project's existing verification suite. A passing test does not establish a contract it was never designed to exercise.

When prose is necessary, for example to explain design rationale or experimental limitations, use existing specifications, architecture decisions and scoped guidance sparingly. Every retained prose conclusion must identify:

- Its authoritative contract or assumption and the revision at which evidence was obtained.
- The supporting evidence or verification procedure.
- Exact paths, symbols, contract versions or other versioned dependencies whose change invalidates it, or the baseline commit if a narrower boundary cannot be established.

Changes to these dependencies invalidate the conclusion until revalidated. If validity cannot be established, treat it as historical context rather than current truth. Prefer existing repository and CI mechanisms to detect changes and rerun checks, not a parallel knowledge base or memory service.

The project owns its knowledge; agent observations become authoritative only through maintainer review. Retain only lessons worth maintaining, and retire those that no longer apply.

## 6. How cultivation and delivery work together

**Investigate → learn → decide → implement → verify → investigate again.**

Suppose a database promises to recover committed data after an interruption. Hunter produces a failing reproduction against that guarantee. A maintainer checks and accepts the finding; a developer or factory fixes it. A fresh investigation checks the original claim against the new version, and the project preserves the lesson as a regression test.

GitHub issues can connect cultivation to ordinary development or an optional delivery system such as Gas City. An agent's finding must not automatically become an implementation instruction. Maintainers control priorities, contracts, changes and releases.

## 7. A small foundation that can grow

Farm distributes reusable investigation practices and native GitHub Agentic Workflows. Projects can start with one manually triggered investigation using existing documentation and tools, without a Farm-operated service or database.

Investigations executing project code, builds, containers or fuzzers must use isolated, ephemeral, network-restricted sandboxes with least-privilege credentials and explicitly authorized commands and destinations. Experimental code must not receive model-provider credentials, repository write tokens, unrelated secrets or privileged host or container-daemon access. Enforce hard wall-clock, token or equivalent provider-enforced model-usage, compute and concurrency limits outside agent prompts; bound output, control publication and terminate experiments at their limits. If the workflow, runner or engine cannot enforce these controls, disable the experiment or unattended mode until it can. These are qualification requirements, not claims about current capabilities.

Keep capabilities independently useful and scale up or down in response to demonstrated need. Express methods without unnecessary engine dependencies, but avoid generic runtime abstractions or multiple distributions before they are needed. A native Gas City distribution or other runtime would require a separate decision and equivalent safeguards.

## 8. Principles that should endure

- **Proof before interruption.** Make consequential findings cheap to verify.
- **Evidence before action.** Distinguish hypotheses, observations, supported conclusions and decisions.
- **Executable memory before prose.** Retain learning that can be checked against changing software.
- **Value before compute.** Use the cheapest adequate method within explicit budgets.
- **Learning before output.** Accept quiet outcomes and reject issue or activity quotas.
- **Simplicity before machinery.** Prefer existing conventions, tools and native capabilities.
- **Local ownership and enforced authority.** Projects control contracts, data, credentials, knowledge and budgets; investigations grant no implicit write, disclosure or access privileges.
- **Gradual, reversible growth.** Expand scope and autonomy only with demonstrated value, reviewed risks and meaningful human oversight.

## 9. A possible path forward

These are development horizons, not release dates.

| Horizon | Focus | Evidence of readiness |
| --- | --- | --- |
| **Establish the foundation** | Qualify installation, execution safeguards, Hunter, Gardener and onboarding. | Reproducible, economical findings; enforced controls hold. |
| **Make learning cumulative** | Improve evidence quality, deduplication and knowledge retention. | Reviewed lessons remain useful as code evolves. |
| **Broaden cultivation** | Add proven methods, better screening and optional delivery integration. | Distinct value within review and compute budgets. |
| **Grow with complex systems** | Explore coordinated and cross-project investigations or alternative runtimes. | Observed needs justify added complexity, cost and risk. |

Qualify the evidence, review economics and execution boundaries before enabling recurring unattended operation or publication.

## 10. How we know Farm is succeeding

Measure outcomes, not activity:

- **Review economics:** time validating, dismissing and deduplicating findings; precision, materiality and value relative to review effort.
- **Compute economics:** model and experiment costs across all runs (including quiet, blocked and inconclusive runs), cost per accepted useful finding or validated learning outcome, and marginal benefit of deeper investigations.
- **Software outcomes:** reproducible findings, stronger verification of important guarantees and demonstrably simpler software.
- **Knowledge health:** continuing usefulness of executable checks, prose maintenance costs, stale conclusions prevented from influencing work, and avoided repeat investigations.
- **Safety and coverage:** whether execution controls hold and whether screening or publication misses important problems; evaluate against known cases and sampled quiet outcomes when practical.

Use project-specific thresholds to increase, narrow, pause or stop activity. There is no universal acceptable cost per finding or false-positive rate.

## 11. The ambition

Continuous integration helps us know whether a change satisfies the checks we have defined. Continuous cultivation helps us discover whether those checks are sufficient, whether our assumptions remain valid, and whether we are investing effort in the right things.

Farm should make this inquiry accessible to ordinary software projects, from a small library to a complex long-lived system.

**A software factory helps us build software. A software farm helps us understand what software is worth building, what is worth preserving, and what can be trusted.**

---

Related documents: [Farm README](https://github.com/grove/farm), [product and architecture](https://github.com/grove/farm/blob/main/docs/design.md), and [shared investigation method](https://github.com/grove/farm/blob/main/workflows/shared/farm-investigation.md). These describe the existing design; this proposed vision extends it without superseding it.
