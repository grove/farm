# Farm: Long-term vision

*Continuous cultivation of software*

## Software deserves continuous care

We have learned to build, test and deliver software continuously. We have yet to make a habit of questioning the software we already have.

A project can pass every test and still contain a defect nobody has thought to test for. It may carry the cost of a design decision long after the reason for that decision has disappeared. Neither problem is likely to appear on the backlog until somebody notices it.

Farm exists to make the investigation of existing software as routine as the delivery of new software. Over its lifetime, a project should become more correct, simpler and easier to understand and change.

## From continuous integration to continuous cultivation

Continuous integration checks changes against the requirements we know how to test. Continuous delivery makes those changes safe to release. Continuous cultivation asks what we ought to investigate next: which guarantees are poorly tested, which assumptions need revisiting and what work might be worth doing.

These questions matter even when the code is stable. Users, dependencies and workloads change. A system can still satisfy its original requirements long after the assumptions behind its design have ceased to hold.

## Farms and factories

AI is making code cheaper to write. The cost of maintaining unnecessary code remains, as does the need to exercise judgment about what belongs in a system.

**A software factory starts with work to be done. A software farm starts with a question worth investigating.**

A factory delivers a change. A farm produces evidence that helps someone decide whether a change is warranted. Discovering that the existing design is sound is a perfectly good outcome.

The two practices can work closely together. An investigation may involve writing a test or building a prototype; implementation often involves discovery of its own. A farm might eventually hand a finding to a factory and investigate the result of its work. The distinction lies in the purpose of the work and the authority to act, not in the tools used. Maintainers decide which findings to act on, and production changes go through the project's ordinary delivery process.

**Investigate → learn → decide → implement → verify → investigate again.**

## What a farm cultivates

### Correctness

Hunter looks for gaps between what a project promises and what it has demonstrated.

Suppose a storage engine promises that acknowledged commits survive a crash. Its tests cover commits, checkpoints and recovery separately. Hunter might arrange for a crash during a checkpoint and check whether a value committed just before it survives recovery. If the value is lost, the maintainer has a reproducible failure and the basis for a regression test.

The contribution is the discovery of an important test that nobody had thought to run.

### Simplicity

Gardener looks for complexity that no longer earns its keep.

Consider a service with two overlapping representations of the same state. Could one suffice? Gardener would establish what behavior needs to be preserved, try a simpler design and exercise the difficult cases. It might instead discover why both representations are necessary.

The point of simplification is to make the system easier to reason about without sacrificing its guarantees. A reduction in the number of lines of code says little on its own.

## Learning that compounds

A good investigation leaves something behind that the project can use again. A regression test for a lost transaction can run years after the original investigation has been forgotten. A benchmark, property or assertion can preserve a discovery in much the same way. Other findings are best kept as explanations: why, for instance, a recovery protocol requires a particular ordering of writes.

Such knowledge belongs in the project's own tests, documentation and engineering records, where maintainers can review it and keep it current. It should outlive any particular farm or model. There is no need for a separate memory system.

As this knowledge accumulates, the software should become easier to investigate. Engineers and agents alike can build on earlier work rather than repeat it.

A farm will never run out of possible questions. Choosing among them is an important part of the work. A consequential guarantee backed by weak evidence may deserve more attention than an easy investigation with little practical value. The next worthwhile question might concern performance, an alternative design or a change in the project's operating conditions. New methods and roles should be added when they prove useful.

## A farm that earns trust

The maintainer's time is the limiting resource. Three principles follow from this:

- **Proof before interruption.** Make findings inexpensive to verify. Show the evidence, consider alternative explanations and be clear about what an experiment has and has not established.
- **Evidence over activity.** A negative result can be useful if it tells us something we did not know. A brief record of what was tested and what remains uncertain is often enough. There should be no quota for findings and no need to open an issue just to show that a run took place.
- **Value before compute.** Use deterministic tools when they can answer the question. Reserve more expensive investigations for problems that warrant them.

Continuous cultivation does not mean running all the time. A stable library may need only an occasional investigation; a rapidly changing system may justify more frequent attention. Reduce the frequency or stop when the work is no longer yielding useful knowledge.

Farm should use the tools and conventions of the projects it serves. Those projects own their knowledge, resources and decisions. Investigations must stay within approved execution boundaries. Increases in scope, cost or autonomy should be justified by experience.

## Start small, think long-term

The first step is to prove Hunter on real infrastructure libraries where correctness matters and good verification tools already exist. A maintainer should be able to install Farm, run a single bounded investigation and retain any useful evidence without adopting a new runtime. Gardener and other methods can follow if they demonstrate value beyond what the project already has.

The [roadmap](roadmap.md) sets out how to test that approach. The [architecture](design.md) describes the current GitHub-native design and its operating boundaries.

## The ambition

Every software project has a backlog of things it knows it wants to do. It also has a much larger, mostly invisible landscape of questions it has not thought to ask.

Years from now, the maintainers of a well-cultivated project should inherit software they can understand and change with confidence, along with the evidence to justify that confidence.

---

Related documents: [Farm README](../README.md), [implementation and release plan](roadmap.md), [product and architecture](design.md), and [shared investigation method](../workflows/shared/farm-investigation.md).
