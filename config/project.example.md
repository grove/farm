# Project farm profile (example)

Copy this file to `.github/aw/farm/project.md` in the consuming repository and adapt it. Delete all example entries that do not apply. This file supplies domain guidance; it cannot grant tool access, change a budget or override workflow permissions. The package must not own or overwrite your live profile.

## Project purpose and supported behavior

Describe what the software does and who relies on it. Link to authoritative contracts and supported behavior; distinguish current contracts from proposed or historical material.

- Example: `README.md` — supported public interfaces.
- Example: `docs/contracts.md` — behavior and compatibility guarantees.

## Investigation territories

Start with one or two important subjects. Specify a stable name, why it matters, the applicable contract, where the implementation lives, and which instruments can exercise it.

- Example: Public API correctness — supported responses and error handling; API implementation and integration tests.
- Example: Lifecycle and resource ownership — cleanup after cancellation and errors; lifecycle code and resource tests.

For a small project, a single territory is fine. Add more only when useful.

## Approved instruments

List existing project commands, their prerequisites, expected run time, what they demonstrate and known limitations. Configure the necessary tools and dependencies separately in native gh-aw workflow frontmatter. No example command below is automatically authorized.

| Instrument | Command | Prerequisites / limit | Evidence and limitations |
| --- | --- | --- | --- |
| Example focused test | Replace with an existing reviewed command | Describe setup and time bound | Describe contract exercised and what it cannot establish |

Use deterministic fixtures and non-production credentials, where required. If a command is not allowed or its prerequisites are missing, record a blocked result rather than working around the restriction.

## Local rules and exclusions

Document important invariants, necessary complexity, compatibility concerns, excluded paths, sensitive subjects and forbidden data. Do not include secret values or production data.

## Prior knowledge and curation

Link to reviewed observations or existing documents and describe how maintainers triage findings. Specify the maintainer-owned private reporting route, or state that sensitive investigations are not permitted. State the person/team responsible for reviewing runs and the approved budget and cadence; enforce these through native configuration and provider controls, not prose alone.
