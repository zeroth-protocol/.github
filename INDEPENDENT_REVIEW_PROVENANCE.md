# ZEROTH-RPV-01 — Independent Review Provenance Architecture

Status: **Draft / implementation required**  
Owner issue: `zeroth-protocol/.github#4`  
Security classification: **Public governance architecture; credentials and private source remain private**

## 1. Purpose

This policy defines how The Zeroth Protocol obtains genuinely independent, artifact-bound review of private repositories and private pull requests without weakening reviewer isolation or relying on self-asserted transport copies.

A review is not independent merely because a different model or reviewer reads a copied file. The reviewer must be able to rely on evidence whose provenance has been independently verified against the authoritative source repository.

## 2. Triggering failure

The first qualification use of the foreign-review transport against `zeroth-protocol/zeroth-consensus-assurance#4` correctly failed closed.

The reviewer found no demonstrated defect in the governance prose itself. The blocking defect was evidence provenance: the carrier repository contained copied material and asserted hashes, but the reviewer had no independent source-repository read path with which to verify the external PR, commits, Git blobs, or referenced assurance objects.

That failure is treated as a successful security property of the fail-closed review system, not as permission to weaken evidence requirements.

## 3. Core invariant

**No reviewer may clear a private Zeroth artifact when the only evidence binding that artifact to its source was authored by the same side requesting review.**

Independent review requires two distinct stages:

1. **Source verification** — deterministic machinery fetches and verifies the authoritative source directly.
2. **Semantic review** — an independent reviewer examines only evidence that passed source verification.

Neither stage may silently substitute the review-carrier commit for the actual Zeroth subject.

## 4. Preferred architecture

### 4.1 Dedicated read-only GitHub App

Create a dedicated GitHub App provisionally named **Zeroth Review Verifier**.

The app should be installed only on repositories that require private reciprocal review and should receive the minimum permissions necessary to verify source artifacts.

Baseline repository permissions:

- Metadata: read
- Contents: read
- Pull requests: read

Additional permissions require a documented need and separate approval.

The app must not receive:

- repository administration;
- repository contents write;
- pull-request write;
- Actions write;
- issues write;
- secrets write;
- organization administration.

Repository access should use an explicit allowlist rather than organization-wide access by default.

### 4.2 Credential boundary

The private review runner may hold the app identifier and private credential needed to obtain short-lived installation tokens.

Requirements:

- no personal access token;
- no reusable owner credential;
- no long-lived repository write token;
- short-lived installation tokens only;
- private key stored only in an approved private secret boundary;
- token and key values never written to logs, artifacts, prompts, or review-visible files;
- documented rotation and emergency revocation path.

### 4.3 Deterministic source verifier

Before Claude or any other semantic reviewer runs, a deterministic verifier must independently fetch the external subject from GitHub using the dedicated read-only installation token.

Inputs must include at least:

- owner/repository;
- pull-request number where applicable;
- expected base SHA;
- expected head SHA;
- explicitly required source paths / Git objects;
- any expected external manifest or root identifiers required by the review scope.

The verifier must fail closed if:

- the repository is outside the app allowlist;
- the PR does not exist;
- the base or head differs from the expected value;
- a requested path is unavailable at the expected ref;
- a Git object or blob SHA differs;
- a required referenced manifest cannot be obtained;
- GitHub returns an ambiguous, partial, or unauthorized response.

## 5. Verified evidence envelope

The verifier should materialize a private, ephemeral review workspace containing only the evidence necessary for the requested scope.

It should emit `VERIFIED_EVIDENCE.json` with a schema equivalent to:

```json
{
  "schema": "ZEROTH-RPV-01/v1",
  "source": {
    "repository": "owner/repo",
    "pull_request": 0,
    "base_sha": "...",
    "head_sha": "..."
  },
  "objects": [
    {
      "path": "path/to/file",
      "ref": "head sha",
      "git_blob_sha": "...",
      "content_sha256": "...",
      "fetch_result": "VERIFIED"
    }
  ],
  "verification": {
    "source_access": "VERIFIED",
    "base_head_binding": "VERIFIED",
    "objects": "VERIFIED"
  },
  "verifier": {
    "version": "...",
    "workflow_run_id": "..."
  }
}
```

The envelope may contain GitHub object identifiers and cryptographic digests. It must not leak secrets or private source beyond the minimum review scope.

## 6. Semantic reviewer isolation

The semantic reviewer remains read-only.

Default reviewer capabilities:

- Read
- Grep
- Glob

Default prohibited capabilities:

- Edit
- Write
- unrestricted Bash
- unrestricted network fetch
- merge or repository mutation

The reviewer receives:

- the exact independently fetched source material required by scope;
- `VERIFIED_EVIDENCE.json`;
- review instructions;
- public governance/policy references required to interpret the subject.

The reviewer must refuse a PASS if source verification is missing, failed, incomplete, or bound to a different head.

## 7. Verdict binding

Every structured verdict must bind at least:

- authoritative source repository;
- PR number or subject identifier;
- base SHA;
- head SHA;
- reviewed source-object Git blob SHAs;
- verifier schema/version;
- verifier workflow-run identifier;
- relevant assurance root / manifest identifiers when required by the review scope.

A carrier repository SHA may be recorded for audit purposes, but it must be identified explicitly as transport provenance and never substituted for the external subject.

## 8. Privacy and logging

Private source must not be published merely to make independent review easier.

The pipeline must:

- keep source-fetch and semantic-review jobs in private repositories/runners;
- avoid echoing private file contents into ordinary Actions logs;
- prevent credentials from entering evidence envelopes;
- use private workflow artifacts only when an artifact is necessary;
- define artifact retention deliberately;
- delete ephemeral source workspaces after review;
- preserve only the minimum verdict and provenance metadata needed for audit.

## 9. Negative-control qualification

The architecture is not qualified until deliberate negative controls demonstrate fail-closed behavior.

Required tests include:

1. correct repository + correct head → verifier succeeds;
2. wrong head SHA → verifier fails before semantic review;
3. wrong changed-file blob SHA → verifier fails;
4. changed manifest after expected head → verifier fails or binds the correct immutable ref;
5. unauthorized repository → access fails closed;
6. missing source object → verifier fails;
7. transport copy altered while source remains correct → source verification prevents the copy from becoming authoritative;
8. claimed assurance root differs from source object → verifier fails;
9. reviewer attempts to PASS with failed provenance → final gate rejects verdict;
10. private source does not appear in public logs/artifacts.

## 10. Separation from GitHub hardening

`ZEROTH-RPV-01` does not replace organization security hardening tracked by `.github#2`.

Both gates are required:

- `.github#2` establishes repository and organization administrative controls.
- `.github#4` / this policy establishes independently verifiable reciprocal-review provenance.

A green review cannot compensate for unprotected repositories, and protected repositories cannot compensate for unverifiable independent-review evidence.

## 11. Current release consequence

Until an implementation of this architecture passes its negative controls:

- private Zeroth PRs requiring reciprocal independent review must remain blocked when the reviewer cannot independently verify source provenance;
- a self-asserted transport bundle is insufficient for PASS;
- a provenance-related FAIL must not be manually reclassified as PASS;
- reviewer isolation must not be weakened solely to obtain a favorable verdict.

## 12. Promotion criteria

This document may move from Draft to Active only when:

- the dedicated read-only source-verification credential/app exists;
- the verifier implementation is reviewed;
- exact source/base/head/blob verification is demonstrated;
- semantic reviewer isolation remains intact;
- negative controls pass;
- private data/logging behavior is verified;
- at least one real private Zeroth PR receives an artifact-bound independent verdict using the qualified path.
