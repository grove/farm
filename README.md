# Farm

**Continuous cultivation of software, powered by GitHub Agentic Workflows.**

Farm is an open-source project for installing a software farm into an existing GitHub repository. A farm continuously discovers correctness and completeness gaps, identifies opportunities to simplify a system, and accumulates useful knowledge about it. Unlike a software factory, which implements known work, a farm helps discover what work is worth doing.

Farm should be a small, reusable distribution of agentic workflows and investigation practices, not a new agent runtime. It should grow with the project, from a single occasional investigation to multiple focused workflows, without requiring new infrastructure or a different installation model. The consuming repository supplies its own contracts, priorities, existing test commands and domain-specific instruments. GitHub supplies execution, scheduling, authentication, artifacts, issues and constrained outputs. An idle farm should require no running Farm-owned process or service.

> **Status: initial design and experimental package candidate.** The package is not released or qualified for unattended use. The workflows in this repository are distribution sources, not active GitHub Actions workflows.

## Start with two roles

| Role | Mission |
| --- | --- |
| Hunter | Challenge the project's confidence in correctness, completeness and evidence. |
| Gardener | Reduce the effort needed to understand, change and verify the project without weakening its contracts. |

Both investigate one bounded question at a time. They may find nothing new, disprove a hypothesis, or conclude that complexity is necessary. Useful findings go to human review. Neither role is required to generate issues or write production code.

## Intended installation and onboarding experience

After a tested release is published, a maintainer should be able to install the package using the existing [gh-aw CLI](https://github.github.com/gh-aw/setup/quick-start/), rather than install a Farm-specific program.

```sh
# Intended release interface; this tag does not exist yet.
gh aw add grove/farm@v0.1.0
```

The installation should leave ordinary gh-aw Markdown workflows, generated lock files and an optional example in the consuming repository. Farm discovers the existing project documentation, contributor instructions, build/test entry points and CI conventions. A project with sufficient existing guidance does not need a Farm-specific configuration file.

For an existing project, start with the optional **Farm Onboard** workflow, a manually dispatched, read-only survey that collects and distills repository knowledge into a small reviewable proposal. It identifies current contracts, important invariants, candidate experiments, evidence limits and unresolved questions. A maintainer reviews the proposal, clarifies only what matters and incorporates useful guidance into existing documentation or an optional root `FARM.md`. A well-documented project may not need a new file.

Review [the onboarding guide](docs/onboarding.md) for the concrete process. Link to existing contracts and instruments rather than restating them. No required fields or schema. The example is available at `.github/aw/farm/project.example.md` after installation.

Review the native gh-aw engine, authentication, permissions, tools and costs, and compile with `gh aw compile`. Initially, runs are manually dispatched and issue creation is staged. Discovery of a command does not grant permission to execute it; approve any additional instruments through trusted native workflow settings.

After reviewing native execution settings, run the survey (if useful), review its proposal, and manually calibrate the selected roles on real questions before scheduling anything. The first release must prove this flow in two independent repositories before we advertise it as working. See the [implementation plan](docs/roadmap.md).

## Design principles

- **Convention over configuration:** discover ordinary repository conventions; ask maintainers only for domain knowledge or exceptions that cannot be inferred responsibly.
- **Configuration over implementation:** install shared methods, adapt local rules, reuse existing tests.
- **Native over custom:** use gh-aw and GitHub features before adding scripts, services, formats or abstractions.
- **Evidence over activity:** useful learning matters; issue counts and agent activity do not.
- **Local ownership:** the consuming repository owns its data, credentials, contracts, budgets, findings and decisions.
- **Explicit authority:** proposals are not permission to change code or publish sensitive information.
- **Scale with the project:** begin with one or two roles and bounded runs. Grow attention, frequency, depth or specialization only as project needs and useful evidence justify it; scale back when they do not.
- **Near-zero additional runtime:** GitHub Agentic Workflows is the runtime. Prefer ordinary Markdown and YAML and existing project instruments; no Farm service, worker or database. If a small helper is unavoidable, run it ephemerally inside an existing GitHub Actions job.

## Project documentation

- [Product and architecture](docs/design.md)
- [Onboarding existing projects](docs/onboarding.md)
- [Implementation and release plan](docs/roadmap.md)
- [Optional `FARM.md` example](config/project.example.md)
- [Shared investigation method](components/investigation.md)
- [Onboard](workflows/farm-onboard.md), [Hunter](workflows/farm-hunter.md) and [Gardener](workflows/farm-gardener.md)
- [Native gh-aw package manifest](aw.yml)

Farm is licensed under the [Apache License 2.0](LICENSE).
