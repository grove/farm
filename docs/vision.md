# Farm: Long-term vision

*Continuous cultivation of software*

**Status:** Proposed vision. This document describes a direction, not a commitment to features or a change to the current architecture.

## 1. The vision

Farm should make continuous cultivation a normal part of software development, just as continuous integration has made continuous verification a normal part of development.

Software is never finished in the sense that our understanding of it is complete. There are always assumptions that can be challenged, behaviors that can be better demonstrated, designs that can be simplified, and possibilities that have not yet been explored.

Farm gives a project a way to pursue these questions continuously, systematically and at a sustainable cost.

The aim is software that becomes **more correct, simpler, better understood and easier to evolve over time**.

Farm should be useful even when it finds nothing to change.

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

A farm brings different perspectives to the same evolving software system. Each perspective asks a different kind of question.

| Perspective | Central question | Contribution |
| --- | --- | --- |
| **Hunter — Correctness** | What might be wrong or insufficiently demonstrated? | Evidence about defects, unsupported assumptions and gaps in verification. |
| **Gardener — Simplicity** | What could be easier to understand, change or verify? | Opportunities to reduce complexity while preserving important behavior. |
| **Librarian — Knowledge** | What have we learned, and what are we forgetting? | Better-connected, current and durable project knowledge. |
| **Scout — Opportunity** | What valuable possibilities have we overlooked? | Evidence-informed options for improving capabilities, architecture or technology choices. |
| **Auditor — Confidence** | Can we independently trust what we believe? | Reproduction, challenges to evidence and verification of accepted changes. |

Hunter and Gardener are Farm's initial roles. Librarian, Scout and Auditor are candidate perspectives, not promised workflows. Each should begin as a focused practice or investigation method; a dedicated role should be introduced only when it proves useful and needs a distinct mission, tools or review process.

Other capabilities may become valuable as projects grow: experimental research, observation of long-term system health, or recommendations about where to investigate next. These need not become separate agents.

The goal is not to assemble a large population of agents. It is to cultivate the few perspectives that help a project learn.

## 4. Investigation as a discipline

All Farm investigations should share a simple method:

1. Understand the relevant project context and what the software is intended to do.
2. Choose a specific, consequential question.
3. State a hypothesis or define what needs to be learned.
4. Examine the actual system and use suitable, authorized instruments.
5. Seek evidence that challenges the initial interpretation.
6. Record the findings, uncertainty and limits of the investigation.
7. Propose an action only when the evidence justifies it.

A farm should distinguish an observed defect from an untested suspicion, an unclear requirement from a broken implementation, and a successful experiment from proof that a proposed change is ready for production.

It should be equally willing to discover that a design is sound, that a suspected problem cannot be reproduced, or that further evidence is needed.

Silence is better than a weak finding. Uncertainty is better than false confidence.

## 5. Learning that accumulates

The long-term value of Farm comes from what a project learns over many investigations, not from any individual agent run.

An investigation may expose a hidden assumption, clarify a contract, reveal a better experiment or explain why a seemingly complicated design is necessary. Useful lessons should survive the run that produced them.

Farm should help maintainers preserve this learning in the places they already use: specifications, architecture decisions, tests, contributor guidance and reviewed observations. Evidence should remain traceable to the version of the software examined, and old conclusions should be reconsidered when their assumptions change.

The project owns this knowledge. An agent's observation is not an authoritative contract until the relevant people have reviewed it.

Over time, a farm should spend less effort rediscovering what is known and more effort exploring what is still uncertain.

## 6. How cultivation and delivery work together

The relationship can be expressed as a simple loop:

**Investigate → learn → decide → implement → verify → investigate again.**

Consider a database that promises to recover committed data after an interruption. Hunter investigates whether the recovery guarantee holds and produces reproducible evidence of a gap. A maintainer reviews the evidence and accepts the issue. A developer or software factory implements a correction. An independent investigation tests the original claim against the new version. The project retains the reviewed lesson in its recovery documentation or verification suite.

Farm is valuable at the discovery, learning and re-examination stages. It does not need to own the implementation stage. GitHub issues and ordinary development processes can connect these activities; an external system such as Gas City could also implement accepted work.

This connection should remain optional. Findings must never become implementation instructions merely because an agent produced them. Maintainers retain authority over priorities, contracts, changes and release decisions.

## 7. A small foundation that can grow

Farm should remain a lightweight, reusable collection of investigation practices and native workflows, rather than a new infrastructure platform.

The initial architecture uses GitHub Agentic Workflows. A project should be able to install a farm, reuse its existing documentation and verification tools, and begin with a single manually triggered investigation. No Farm-operated service or database should be necessary.

As needs grow, a project may investigate more areas, run more frequently, add specialized methods or conduct deeper experiments. Each capability should be useful on its own, without requiring every other role to be installed.

The investigation methodology should be expressed in terms that are not unnecessarily tied to a particular engine. But Farm should not build a generic runtime abstraction or maintain multiple execution platforms in anticipation of future needs. A native Gas City distribution, or any alternative runtime, would be a separate architectural decision justified by actual requirements and equivalent safeguards.

A mature farm should also be able to scale down. A stable project may benefit more from an occasional well-chosen question than from constant activity.

## 8. Principles that should endure

- **Evidence before action.** Conclusions must be supported by observations, not by persuasive language or pressure to generate work.
- **Learning before output.** Findings are useful only if they improve understanding or inform worthwhile decisions. No issue or activity quotas.
- **Simplicity before machinery.** Prefer existing repository conventions, tools and platform capabilities to new abstractions and infrastructure.
- **Local ownership.** Each project controls its contracts, data, credentials, retained knowledge, budget and decisions.
- **Explicit authority.** Investigation does not imply permission to publish sensitive findings, change production code or expand access. Enforce these limits through the execution environment, not prompts alone.
- **Gradual, reversible growth.** Increase scope, depth and autonomy only after evidence of value and a review of risks and costs. Keep meaningful human oversight.

## 9. A possible path forward

These are development horizons, not release dates. Progress depends on what can be demonstrated in real projects.

| Horizon | Focus | Evidence of readiness |
| --- | --- | --- |
| **Establish the foundation** | Qualify installation and safe operation; validate Hunter, Gardener and onboarding. | Independent projects obtain useful, reviewable results with manageable effort. |
| **Make learning cumulative** | Improve evidence quality, deduplication, knowledge curation and investigation evaluation. Explore the Librarian perspective. | Reviewed lessons survive across runs, and repeated investigations become more informed. |
| **Broaden cultivation** | Add specialized methods; explore Scout and Auditor; connect accepted findings to delivery systems. | New capabilities contribute distinct value without adding disproportionate complexity or review burden. |
| **Grow with complex systems** | Explore deeper and coordinated investigations, cross-project questions and optional alternative execution environments. | Observed needs justify the additional coordination, cost and security requirements. |

New capabilities should earn their place through experience, not simply because they fit the farm metaphor.

## 10. How we know Farm is succeeding

Farm is successful when maintainers can make better engineering decisions because of what it discovers.

Useful signs include:

- Findings that are reproducible, relevant and accepted as valuable by maintainers.
- Important assumptions that become explicit and better verified.
- Simplifications that reduce reasoning and maintenance effort without sacrificing guarantees.
- Knowledge that remains useful as contributors, agents and code change.
- Less repeated investigation, manageable review effort and sustainable cost.

Farm should also evaluate its own limitations: false findings, missed concerns, shallow experiments, unsafe output, repeated work and unnecessary consumption of maintainer attention. Quality measures should guide improvement, not become quotas imposed on agents.

The ultimate test is not whether Farm appears busy. It is whether projects are better understood, more trustworthy and easier to evolve because Farm exists.

## 11. The ambition

Continuous integration helps us know whether a change satisfies the checks we have defined. Continuous cultivation should help us discover whether those checks are sufficient, whether our assumptions remain valid, and whether we are investing effort in the right things.

Farm should make this kind of inquiry accessible to ordinary software projects, from a small library to a complex long-lived system.

**A software factory helps us build software. A software farm helps us understand what software is worth building, what is worth preserving, and what can be trusted.**

---

Related documents: [Farm README](https://github.com/grove/farm), [product and architecture](https://github.com/grove/farm/blob/main/docs/design.md), and [shared investigation method](https://github.com/grove/farm/blob/main/workflows/shared/farm-investigation.md). These describe the existing design; this proposed vision extends it without superseding it.
