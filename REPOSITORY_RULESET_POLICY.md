# Zeroth Repository Ruleset Policy

Status: Approved baseline  
Applies to: repositories under `zeroth-protocol`

## 1. Primary branch

The canonical protected branch is `main`.

Required controls:

- pull request required before merge;
- no direct pushes except explicitly designated break-glass maintainers;
- minimum 1 approving review for public documentation/interface repositories;
- minimum 2 approving reviews for protocol-critical, security-sensitive, settlement, consensus, cryptography, PoVW and infrastructure repositories;
- dismiss stale approvals after new reviewable commits;
- require resolution of review conversations;
- block force pushes;
- block branch deletion;
- require status checks once a repository has CI;
- require branches to be up to date when the required checks depend on current base state;
- preserve linear/reviewable history where compatible with repository release workflow.

## 2. Merge policy

Default merge method: **squash merge** for ordinary repository changes.

Protocol evidence, cryptographic vectors, formal-assurance artifacts or governance records whose commit identity is itself part of an evidence chain may use a non-squashed merge only when the applicable repository policy explicitly requires commit preservation.

## 3. Security-sensitive repositories

The following are treated as protocol-critical/security-sensitive by default:

- `zeroth-core`
- `zeroth-povw`
- `zeroth-settlement`
- `zeroth-consensus-assurance`
- `zeroth-security-lab`
- `zeroth-infra`
- `zeroth-pq` when executable cryptographic code is introduced
- `zeroth-test-vectors` for canonical release vectors

They require two independent approvals for material changes once more than one qualified maintainer is available.

## 4. Cross-review

Where Zeroth governance designates reciprocal AI review, an implementation must not self-clear its own independent-review requirement. A change authored by the OpenAI/Codex side that requires independent Claude Code review cannot be considered cleared by OpenAI/Codex review alone, and vice versa.

## 5. Break-glass

Emergency bypass authority must be restricted, auditable and used only for material operational/security emergencies. Any bypass must be followed by an explicit post-event review and permanent corrective action where applicable.

## 6. Release refs

Release tags used as protocol, cryptographic, audit or interoperability evidence must not be rewritten or reused. A superseding release receives a new version/tag and preserves the historical record.
