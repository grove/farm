# Farm: Long-term vision

*Continuous cultivation of software*

**Status:** Proposed product direction. See [product and architecture](design.md) for operational requirements.

## 1. The vision

Farm should make continuous cultivation as natural as continuous integration. CI tells us whether software passes the checks we have written. Farm asks which important guarantees those checks have not yet tested.

The first users are maintainers of **high-reliability infrastructure libraries**, such as storage engines, parsers and authentication libraries. These projects have consequential correctness requirements, runnable test suites and relatively well-defined boundaries. A subtle logic bug that survives ordinary CI can be worth considerably more than the cost of finding it.

Imagine returning to a mature project after a weekend and finding one short, reproducible test that exposes a failure nobody thought to exercise. Or finding a receipt showing that a previously untested recovery scenario survived a deliberately challenging experiment. Over time, those investigations become better tests and stronger confidence in the software.

Maintainer attention is Farm's scarcest resource. Farm must save more engineering attention than it consumes, including the time spent rejecting its mistakes. If maintainers have to investigate Farm's claims for it, Farm has failed.

## 2. Why a software farm?

AI is making code cheaper to produce. Deciding what to build and establishing whether it works are becoming the harder problems.

| Software factory | Software farm |
| --- | --- |
| Starts from work to be done | Starts from a question worth investigating |
| Implements a chosen change | Tests whether a change is warranted |
| Produces working software | Produces evidence and reusable verification |

Farm complements a software factory rather than replacing one. Its initial product is not more code or more review comments, but discoveries that make existing software more trustworthy.

## 3. Start with Hunter

**Hunter — What important behavior has not been demonstrated, and can we break it?**

Hunter looks for consequential defects that the project's existing tests and tools miss. Its advantage should be the ability to connect context across APIs, tests, callers and implementation, formulate a new property or failure scenario, and turn it into an executable experiment.

A fuzzer explores the inputs and properties it is given. A linter or sanitizer checks the rules it knows. Hunter should use these instruments, not compete with them: the interesting work is choosing a new oracle, combining operations into a meaningful sequence or discovering which production path existing tests never exercise. Where a deterministic tool can already find a problem, use that tool.

**Gardener — What could be simpler?** remains a possible later direction, not part of the first product horizon. Routine mechanical cleanup is often better served by static analysis, while substantial refactoring is costly to verify and review. Reconsider Gardener only after Hunter demonstrates repeatable value and a separate case for simplification beyond existing tools can be made. Any eventual simplification finding needs measurable or concrete benefit and checkable preservation of relevant behavior, including edge cases; passing existing CI alone is insufficient.

## 4. What Hunter could discover

These are illustrative scenarios, not claims of defects Farm has already found.

### Example 1: A parser loses the final bytes of a stream

A Go parser accepts an `io.Reader`. Its documentation is sparse, but its tests establish the expected parse result for valid input, and Go's reader contract permits a read to return both bytes and an end-of-file indication. CI exercises ordinary readers that return EOF separately from the final bytes.

Hunter notices that the streaming loop checks the error before consuming those bytes. It constructs a conforming reader that returns the final chunk and `io.EOF` together, then compares the result with the existing test fixture. The parser truncates the input or returns an incorrect result. The oracle comes from the standard interface and an existing expected result, not an invented business rule.

A small deterministic regression test reproduces the failure on the current revision. The maintainer can run it, accept the finding and fix the loop. The same test protects future changes. A conventional input fuzzer using the original reader harness might never reach this I/O boundary; Hunter's contribution is the new harness and the property it checks.

### Example 2: An acknowledged transaction disappears after recovery

A storage engine promises that acknowledged commits survive a process interruption. Existing CI tests commits, checkpoints and restart separately; it does not combine them at a particular persistence boundary.

Hunter traces the commit and checkpoint paths and identifies a candidate interval where recovery might consult an outdated checkpoint. Using an approved fault-injection harness, it commits a known value, triggers checkpointing, interrupts the process at the selected point, reopens the store and checks that value. Suppose recovery returns the old value despite the commit having succeeded.

The finding includes the failing reproduction, baseline revision and relevant durability guarantee. A maintainer accepts the evidence, a developer fixes recovery, and the project keeps the experiment as a regression test. Hunter has added a meaningful state-transition sequence and a durability oracle to CI, not merely run a generic crash detector.

## 5. Investigation as a discipline

Hunter selects one consequential question, finds the best available evidence for the intended behavior, chooses an appropriate instrument, and tries to falsify its hypothesis. It records counterevidence and limitations before drawing a conclusion.

Explicit specifications are useful but not essential. Public interfaces, external standards, existing assertions and independently verified relationships between supported operations can establish testable expectations even in a poorly documented repository. Compare alternate paths, round trips, operation sequences or equivalent inputs when there is evidence that their results should agree. If the intended relationship is ambiguous, do not guess business logic: investigate an independently established invariant or record the uncertainty without reporting a defect.

**Proof before interruption.** An actionable defect needs an executable reproduction, failing test or concrete, independently checkable counterexample tied to the relevant contract or invariant, with expected and observed behavior. Farm reports novel, consequential results, not suspicions or generic missing-test inventories.

**Value before compute.** Start with existing deterministic tools and inexpensive screening; use bounded model-assisted investigation only where context and experiment design can add value. Escalate to deeper work when risk, tractability and expected learning justify it. Set per-run and cumulative budgets and reduce cadence or stop when marginal value falls. Details of qualification and publication are in [the architecture](design.md).

## 6. Make even quiet runs useful

A run that finds no defect should leave a short, factual **coverage receipt**. It shows what was attempted, what was actually exercised, the result, its limits and what it cost. It is not a claim that the software is bug-free.

For example, an illustrative receipt might read:

| | |
| --- | --- |
| Question | Does parsing depend on how a stream is chunked? |
| Scope | Streaming parser; valid fixture set at the tested revision |
| Experiment | Compare whole-buffer results with 24 chunking schedules, including bytes returned with EOF |
| Result | All 24 matched the expected results |
| Limits | Malformed inputs and cancellation not tested |
| Resources | 6 minutes; $0.70 in model and runner costs (illustrative) |

Receipts belong in ordinary run records, with an optional compact aggregate summary showing newly exercised scenarios, outcomes and total spend. They should be based on actual experiment records, not model guesses about coverage. Distinguish a completed experiment from a blocked, failed or inconclusive run.

This makes weeks without findings intelligible without forcing maintainers to read every report. If repeated runs add no new useful evidence, spend less or stop. No activity or finding quotas.

## 7. Learning that survives

Farm's durable memory should be executable: reviewed regression tests, property tests, assertions and benchmarks that encode important lessons and can be rerun as the software changes. Promote those lessons through the project's ordinary development process.

Design rationale and experiment notes can remain in normal issues, PR descriptions, commit messages or existing project documentation. Treat them as historical context unless confirmed against the current system; do not build a Farm prose knowledge base, dependency-tracking scheme or revalidation notification queue.

A successful investigation is especially valuable when its result can become an inexpensive deterministic check, so future runs do not need to pay a model to rediscover it.

## 8. A small foundation

Farm distributes reusable investigation methods and native GitHub Agentic Workflows. Start with a single manually triggered Hunter in an existing repository, a narrow scope and its approved instruments. No Farm-owned service, database or generic orchestration platform is needed.

Investigations must run within isolated, network-restricted, least-privilege environments with enforced resource and publication limits. Where those controls cannot be qualified, do not enable the affected experiment or unattended operation. The detailed requirements belong in [the architecture and security boundaries](design.md#8-authority-safety-and-defaults).

Maintainers own contracts, credentials, findings and decisions. Accepted findings can enter ordinary development or an optional factory such as Gas City; implementation remains outside Farm's authority. Alternative runtimes require a separate decision justified by demonstrated need.

## 9. A possible path forward

These horizons describe evidence of readiness, not promised features or release dates.

| Horizon | Focus | Evidence of readiness |
| --- | --- | --- |
| **Prove Hunter** | Qualify Hunter and onboarding in a few high-reliability infrastructure libraries; start with targeted manual investigations. | Novel, reproducible defects missed by existing CI and tools, with low review burden and qualified execution safeguards. |
| **Make cultivation routine** | Improve experiment selection, receipts and executable memory; enable bounded recurring investigations. | Repeatable incremental value, useful quiet runs and sustainable total cost. |
| **Broaden carefully** | Expand proven methods and factory integration; evaluate whether Gardener or other capabilities offer distinct value. | Each addition outperforms simpler alternatives within review and compute budgets. |
| **Grow with complex systems** | Explore coordinated investigations and other execution environments only as needed. | Concrete limitations and benefits justify the additional complexity. |

## 10. How we know Farm is succeeding

Compare Hunter with the project's existing verification process, not with a world without tests.

- **Incremental value:** consequential defects and new useful executable checks that existing CI, linters, fuzzers or sanitizers did not already produce.
- **Review economics:** time spent verifying, rejecting and deduplicating findings, their precision and materiality, and the value delivered relative to that effort.
- **Compute economics:** total model and experiment cost across all runs, including quiet and unsuccessful runs, relative to accepted findings or validated learning.
- **Confidence and learning:** new consequential scenarios actually exercised, limitations of that evidence, and lessons retained as executable checks.
- **Safety and coverage:** whether execution limits hold and important problems are missed; use known cases, comparable baselines and sampled quiet outcomes where practical.

Use project-specific thresholds to decide whether to continue, narrow or stop. Do not infer reliability from an absence of findings.

## 11. The ambition

A mature infrastructure library is not finished simply because its tests are green. There are still questions worth asking about how it behaves under unusual combinations of otherwise valid operations.

Start by proving that Hunter can ask and answer those questions better than the existing toolchain alone. If it succeeds, continuous cultivation can become a routine complement to CI: a way to discover the next important test before a user discovers the next important bug.

**A factory builds software. A farm grows the evidence that it can be trusted.**

---

Related documents: [Farm README](../README.md), [product and architecture](design.md), and [shared investigation method](../workflows/shared/farm-investigation.md).
