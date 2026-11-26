# Farm: Long-term vision

*Continuous cultivation of software*

## Software deserves continuous care

We have learned to build, test and deliver software continuously. We have not yet made it normal to continuously question the software we already have.

Does it really behave as we believe? Are its guarantees adequately demonstrated? Is all of its complexity necessary? What could we learn that would change our understanding of the system?

Farm exists to make that kind of inquiry a natural part of software engineering. Its vision is software that becomes **more correct, simpler, better understood and easier to evolve throughout its lifetime**.

Imagine a project that grows more trustworthy as it ages, not merely larger. Its guarantees become clearer, its tests more revealing, and its unnecessary machinery gradually disappears. The people maintaining it inherit not just code, but a better understanding of why that code exists and what can be trusted about it.

That is the promise of continuous cultivation.

## From continuous integration to continuous cultivation

Continuous integration asks whether a change passes the checks we have defined. Continuous delivery helps turn a chosen change into working software. Continuous cultivation asks whether we are checking the right things, whether our assumptions still hold and whether a change is worth making in the first place.

| Practice | Central question |
| --- | --- |
| Continuous integration | Does this change satisfy our existing checks? |
| Continuous delivery | Can we deliver this change reliably? |
| Continuous cultivation | What should we investigate, understand or improve next? |

Even a stable project with no feature backlog has questions worth asking. An unexpected combination of valid operations might expose a defect. A complicated abstraction might outlive the reason it was introduced. A passing test suite might provide less confidence than we think.

Farm helps discover these opportunities before they become incidents, maintenance burdens or forgotten assumptions. It can also discover that the current design is sound and that changing it would be a mistake.

## Farms and factories

AI is making it easier to turn an intention into code. It is not making every intention worth pursuing. As our capacity to implement grows, the ability to decide what to implement, what to preserve and what to remove becomes more valuable.

A software factory starts with work to be done. A software farm starts with a question worth investigating.

| Software factory | Software farm |
| --- | --- |
| Implements a chosen change | Discovers whether a change is worthwhile |
| Works toward a specified outcome | Investigates an uncertain outcome |
| Produces working software | Produces evidence, understanding and worthwhile options |
| Measures delivery | Measures useful learning |

Both need sound engineering judgment. The distinction is their starting point and purpose, not a prohibition on discovery in factories or experimentation in farms.

A farm may write a small test, build a prototype or try an alternative design to learn something. Production changes remain the responsibility of the project's ordinary delivery process. Maintainers decide which discoveries to act on, and the farm can later re-examine the result.

**Investigate → learn → decide → implement → verify → investigate again.**

Farm should strengthen this loop without becoming another software factory.

## What a farm cultivates

### Correctness

*What might be wrong, and what have we not yet demonstrated?*

Hunter challenges confidence in existing software. It connects contracts, callers, tests and implementation to discover questions that conventional verification has not yet answered.

Imagine a storage engine whose tests exercise commits, checkpoints and recovery independently. Hunter identifies a meaningful combination of those operations, constructs a controlled experiment and exposes a lost acknowledged transaction. A maintainer can reproduce the failure rather than audit pages of reasoning. The fix becomes a lasting regression test.

The greatest value is not running more tests. It is discovering the next important test before a user discovers the next important bug.

### Simplicity

*What could be easier to understand, change or verify without losing what matters?*

Gardener explores the complexity that accumulates as software evolves. Its ambition goes beyond cleanup that a linter can already perform. It asks whether an abstraction remains necessary, whether two representations of the same state can become one, or whether a simpler model could eliminate an entire class of failure.

Imagine a service that maintains two overlapping sources of truth. Rather than suggesting a stylistic refactor, Gardener investigates whether a single representation can preserve the required behavior, demonstrates the benefit and checks the difficult cases. It might find that the two representations are essential. Either result can be useful.

Simplification is valuable when it reduces the reasoning burden of future engineers, not merely the number of lines of code.

## Learning that compounds

*What do we know, why do we believe it, and what remains uncertain?*

The lasting product of a farm is not its latest report. It is the improvement in what a project knows and can verify.

An investigation can clarify why a recovery guarantee needs a particular ordering constraint, reveal the limits of a benchmark, or establish that a seemingly redundant check protects an important edge case. A useful discovery can become a regression test, a property, an assertion or a repeatable benchmark. These lessons can be checked against later changes without paying an agent to rediscover them.

Executable evidence is preferred where possible, but it is not the only valuable form of engineering knowledge. Reviewed rationale and decisions belong in the project's existing documentation and engineering records. Farm should strengthen the project's own knowledge, not create a parallel memory system that must be maintained.

A well-cultivated system should become easier to investigate as it grows. More of its important behavior is explicit, more of its assumptions can be tested and less effort is wasted rediscovering old lessons.

Farm should also make room for new questions: opportunities to improve performance, compare designs or reconsider assumptions in light of new evidence. New perspectives should earn their place through demonstrated value, not through an expanding cast of agent personas.

## A farm that earns trust

Maintainer attention is Farm's scarcest resource. A farm should give engineers back more time and confidence than it consumes. It should bear the burden of demonstrating its findings rather than ask people to disprove persuasive speculation.

A few principles should endure even as models, tools and platforms change:

- **Proof before interruption.** Make consequential findings inexpensive to verify.
- **Evidence over activity.** A useful negative result is better than an unnecessary issue; there are no finding quotas.
- **Learning that lasts.** Prefer repeatable evidence and preserve reviewed knowledge where the project already maintains it.
- **Value before compute.** Use existing deterministic instruments whenever they suffice; spend deeper investigation effort where it adds value.
- **Simplicity and local ownership.** Reuse existing tools and conventions, respect project authority, and enforce safe execution boundaries.
- **Grow empirically.** Increase scope and autonomy only when evidence justifies the additional cost and risk.

Quiet cultivation should be visible without being noisy. A short record of the questions actually tested, the results and their limits can make an uneventful investigation worthwhile. Repeated activity without new learning is a reason to scale back.

## Start small, think long-term

Farm does not need a large society of agents or a new orchestration platform to begin. A project should be able to install a farm, start with one bounded investigation, and retain whatever useful evidence it discovers.

The first product strategy is deliberately narrower than this vision: prove Hunter on real, high-reliability infrastructure libraries, using the tools those projects already have. Then establish whether additional methods and perspectives, including Gardener, deliver enough incremental value to justify themselves.

The [roadmap](roadmap.md) defines how we will test that strategy. The [architecture](design.md) defines the operating boundaries, evidence standards and lightweight GitHub-native implementation. Neither is the limit of the long-term ambition.

## The ambition

Every software project has a backlog of things it knows it wants to do. It also has a much larger, mostly invisible landscape of questions it has not thought to ask.

A mature software engineering practice should cultivate both.

Farm should make systematic investigation accessible enough to become a habit, rigorous enough to earn trust, and economical enough to continue for the life of a project.

**Build continuously. Learn continuously. Cultivate software for the long term.**

---

Related documents: [Farm README](../README.md), [implementation and release plan](roadmap.md), [product and architecture](design.md), and [shared investigation method](../workflows/shared/farm-investigation.md).
