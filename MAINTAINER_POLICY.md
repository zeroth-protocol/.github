# Zeroth Maintainer and Review Policy

Status: Approved baseline

## 1. Principle

Repository permissions follow least privilege. Maintainer authority is scoped by repository sensitivity, technical domain and demonstrated review competence.

## 2. Roles

### Owner
Organization-level administration, security controls, repository lifecycle and emergency access. Owners should be kept to the minimum practical number.

### Maintainer
Repository administration and merge authority within a defined technical domain. Maintainers do not automatically receive organization-owner privileges.

### Reviewer
Qualified to review specific classes of changes. Review authority may be narrower than write authority.

### Contributor
May propose changes but has no implicit merge or release authority.

## 3. Review thresholds

Public documentation and non-critical interface changes: minimum 1 qualified approval.

Protocol-critical, security-sensitive, settlement, consensus, cryptography, PoVW, canonical test-vector and infrastructure changes: minimum 2 qualified approvals once staffing permits, including at least one reviewer independent from the implementation author.

## 4. Independence

Authors must not self-approve their own pull requests.

Where Zeroth governance requires reciprocal AI review, the model/system that authored the implementation cannot satisfy the independent-review requirement for that change. Cross-review evidence must be attributable to the designated independent reviewer.

## 5. Security-sensitive changes

Exploit details, unreleased vulnerabilities, credentials, incident evidence and private operational topology must not be moved into public review surfaces. Use private security workflows and coordinated disclosure.

## 6. Merge authority

A maintainer may merge only after required checks and approvals are satisfied and no unresolved blocking finding remains. Passing CI is necessary where required but does not substitute for semantic/security review.

## 7. Release authority

Production designation is separate from merge authority. A merged change becomes part of a production release only after the applicable release and `ZEROTH-OSG-01` gates are cleared.

## 8. Access review

Organization owners should review maintainers, teams, installed apps and repository permissions periodically and immediately after material staffing or security events.
