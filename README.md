# Farm

**Continuous cultivation of software, powered by GitHub Agentic Workflows.**

Farm is an open-source project for installing a software farm into an existing GitHub repository. A farm continuously discovers correctness and completeness gaps, identifies opportunities to simplify a system, and accumulates useful knowledge about it. Unlike a software factory, which implements known work, a farm helps discover what work is worth doing.

Farm should be a small, reusable distribution of agentic workflows and investigation practices, not a new agent runtime. The consuming repository supplies its own contracts, priorities, existing test commands and domain-specific instruments. GitHub supplies execution, scheduling, authentication, artifacts, issues and constrained outputs.

> **Status: initial design and experimental package candidate.** The package is not released or qualified for unattended use. The workflows in this repository are distribution sources, not active GitHub Actions workflows.

## Start with two roles

| Role | Mission |
| --- | --- |
| Hunter | Challenge the project's confidence in correctness, completeness and evidence. |
| Gardener | Reduce the effort needed to understand, change and verify the project without weakening its contracts. |

Both investigate one bounded question at a time. They may find nothing new, disprove a hypothesis, or conclude that complexity is necessary. Useful findings go to human review. Neither role is required to generate issues or write production code.

## Intended installation experience

After a tested release is published, a maintainer should be able to install the package using the existing [gh-aw CLI](https://github.github.com/gh-aw/setup/quick-start/), rather than install a Farm-specific program.

```sh
# Intended release interface; this tag does not exist yet.
gh aw add grove/farm@v0.1.0
```

The installation should leave ordinary gh-aw Markdown workflows, generated lock files and a small project configuration example in the consuming repository. The maintainer copies the example to `.github/aw/farm/project.md`, describes the project's contracts and approved instruments, chooses an engine and configures authentication, reviews permissions and costs, and compiles the workflows with `gh aw compile`. Initially, runs are manually dispatched and issue creation is staged.

The first release must prove this flow in two independent repositories before we advertise it as working. See the [implementation plan](docs/roadmap.md).

## Design principles

- **Configuration over implementation:** install shared methods, adapt local rules, reuse existing tests.
- **Native over custom:** use gh-aw and GitHub features before adding scripts, services, formats or abstractions.
- **Evidence over activity:** useful learning matters; issue counts and agent activity do not.
- **Local ownership:** the consuming repository owns its data, credentials, contracts, budgets, findings and decisions.
- **Explicit authority:** proposals are not permission to change code or publish sensitive information.
- **Small by default:** start with ordinary Markdown and YAML. Add software only to close a demonstrated gap.

## Project documentation

- [Product and architecture](docs/design.md)
- [Implementation and release plan](docs/roadmap.md)
- [Project configuration example](config/project.example.md)
- [Shared investigation method](components/investigation.md)
- [Hunter](workflows/farm-hunter.md) and [Gardener](workflows/farm-gardener.md)
- [Native gh-aw package manifest](aw.yml)

Farm is licensed under the [Apache License 2.0](LICENSE).
