# ZEROTH-OSG-01 — Open-Source Release Gate

## Principle

The Zeroth Protocol is **open by design** while implementation is **private by default until hardened**.

## Public now / when stable

- specifications and ZIPs
- research suitable for publication
- documentation
- public cryptographic formats and interoperability vectors
- stable SDK interfaces and examples

## Private until release gate

- production core
- consensus implementation
- PoVW scoring / anti-gaming internals
- protected-ordering implementation
- bridge / settlement implementation under active hardening
- unreleased vulnerability research
- production infrastructure and IAM

## Always private

- credentials, keys and secrets
- privileged endpoints
- incident-response evidence under embargo
- exploit details for unresolved vulnerabilities

## Core implementation release gate

Architecture:
- release-candidate semantics frozen
- settlement interfaces frozen
- cryptographic formats frozen
- PQ migration architecture frozen
- PoVW / consensus boundaries documented

Security:
- threat model complete
- at least two independent reviews for consensus-critical components
- critical/high findings remediated or formally risk-accepted
- fuzzing and adversarial campaigns completed
- dependency/supply-chain review completed
- history and artifacts scrubbed for secrets

Engineering:
- reproducible release process
- protected primary branch
- mandatory PR review
- CI security controls
- dependency pinning
- SBOM generation
- signed/tagged releases
- release hashes published

Governance:
- licensing ratified
- SECURITY.md
- CONTRIBUTING.md
- maintainer/review policy
- ZIP process
- release policy
- coordinated vulnerability disclosure

## Target sequencing

- Current R&D / Seed: public specs and standards; core private.
- Protocol stabilization: public SDK and vetted components.
- ~8–12 weeks before mainnet: publish audited release-candidate reference implementation.
- ~6–8 weeks before mainnet: start public bug bounty.
- Mainnet: protocol source, specs, contracts, audits, SDK and test vectors publicly verifiable; operational infrastructure remains private.
