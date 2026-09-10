# Zeroth Release Policy

Status: Approved baseline

## 1. Release classes

- **Research** — experimental and non-production; may change without compatibility guarantees.
- **Candidate** — release-candidate material under qualification; not production-final.
- **Public Testnet** — externally consumable testnet release with explicit limitations.
- **Mainnet** — production release that has cleared the applicable architecture, security, engineering and governance gates.

## 2. Required release metadata

Material releases must identify:

- version/tag;
- source commit SHA;
- applicable specification/ZIP versions;
- build or artifact hashes where relevant;
- compatibility/migration notes;
- security status and known limitations;
- audit/review status where applicable.

## 3. Immutability

Published release tags and canonical evidence hashes must not be rewritten. Corrections or supersessions require a new version and explicit linkage to the superseded release.

## 4. Protocol-critical releases

Consensus, settlement, cryptography, PoVW, identity, state-transition and protected-ordering releases require the applicable `ZEROTH-OSG-01` gates before production designation.

A successful CI run, research qualification result or independent review closes only the gate it actually evaluates; it does not implicitly authorize mainnet, value transfer or production freeze.

## 5. Public/open-source transition

Opening previously private implementation requires explicit publication review, including secret/history scanning, license/provenance checks, vulnerability disposition and removal of operational-security material.

## 6. Rollback and supersession

Rollback procedures must preserve forensic evidence and release history. A revoked or superseded release remains identifiable and must not be silently replaced under the same tag.
