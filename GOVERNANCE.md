# Governance for ApexDemo

This governance model covers decisions and changes made in the `rjensenNV/ApexDemo` repository. It does not assign authority over the upstream `NVIDIA/apex` repository.

## Scope

Governance covers repository scope, public policy, contribution review, issue triage, documentation, and changes to the Apex code maintained in this repository.

## Roles and responsibilities

| Role | Responsibilities | Selection and removal | Current holder |
| --- | --- | --- | --- |
| Maintainer | Set repository direction, review and merge changes, triage issues, keep policy consistent, and coordinate security reports through the private reporting path. | The repository owner appoints or removes maintainers through a reviewed change to [MAINTAINERS.md](MAINTAINERS.md). | [@rjensenNV](https://github.com/rjensenNV) |

No separate reviewer, steering committee, or formal role-progression process is currently published.

## Decision process

- Open proposals as GitHub issues in this repository.
- Discuss substantial features, public API changes, new dependencies, compatibility changes, performance-sensitive designs, repository policy, and governance changes before implementation.
- The maintainer makes routine decisions and has final approval and merge authority.
- With one maintainer, that maintainer resolves competing proposals and deadlocks after considering documented technical evidence and community feedback.
- This repository does not publish a separate release procedure; existing tags and documentation are preserved without creating a new release commitment.
- Decisions and their rationale are recorded in the relevant public issue or pull request whenever they can be discussed publicly. Security-sensitive decisions remain in the NVIDIA PSIRT process.

## Changes to governance

Propose governance changes through an issue followed by a pull request to this file. A change takes effect after approval and merge by the maintainer.
