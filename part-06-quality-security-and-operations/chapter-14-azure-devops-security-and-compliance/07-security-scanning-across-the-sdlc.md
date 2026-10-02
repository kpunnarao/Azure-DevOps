# Security Scanning Across the SDLC

[← Secret Lifecycle](06-key-vault-secure-files-and-secret-rotation.md) · [Chapter 14](README.md) · [Next: Auditing and Compliance →](08-auditing-compliance-and-evidence.md)

## Defense in depth

Security analysis belongs at multiple points:

| Control | Finds |
|---|---|
| Threat modeling/design review | architectural trust and abuse cases |
| Secret push protection/history scan | exposed credentials |
| SAST/code scanning | source-level vulnerability patterns |
| SCA/dependency scanning | vulnerable/licensing components |
| IaC/config scanning | cloud/Kubernetes misconfiguration |
| Container scanning | OS/application packages in final image |
| DAST/API testing | behavior of a running system |
| Fuzzing | unexpected inputs/state |
| SBOM/provenance/signature policy | components and origin evidence |
| Runtime/cloud posture | drift and deployed exposure |

No single scanner proves security.

## Azure Repos capabilities

GitHub Advanced Security for Azure DevOps can provide secret protection, dependency scanning, and CodeQL code scanning for supported Azure Repos Git scenarios/licensing. Product packaging and availability evolve; verify current organization/repository prerequisites.

Push protection prevents supported newly detected secrets; repository scanning also examines existing history. On a secret alert, revoke/rotate first—closing an alert or rewriting history does not neutralize the credential.

## Gate design

Define severity/exploitability, new-versus-baseline findings, protected branches, fix availability, confidence, environment, and exception policy. Start legacy adoption by blocking new critical risk rather than suppressing thousands of findings.

Scanner unavailable, timed out, or skipped is **unknown**, not passed. Decide fail-closed behavior according to release risk and ensure later promotion cannot bypass required evidence.

## Triage

Validate finding and affected version/path, reachability/exposure, compensating controls, exploit intelligence, owner, remediation, and deadline. A false-positive dismissal needs rationale and review; a risk acceptance needs accountable authority and expiry.

Track findings by immutable artifact/digest and deployed inventory. Source branch results alone do not identify runtime exposure.

## Supply-chain considerations

Pin trusted tasks/templates/tools, isolate scanners that execute builds, protect SARIF/results, limit third-party data egress, verify scanner binaries, and review extension permissions. Security tools themselves can access sensitive source and credentials.

## Interview preparation

**SAST versus DAST?**  
SAST analyzes source/build artifacts without exercising deployed behavior; DAST probes a running application externally. They find different classes.

**How adopt scanning in legacy code?**  
Baseline existing debt, block new/worsened high-risk findings, prioritize remediation by exposure, and ratchet policy without mass suppression.

**What if the scanner fails?**  
Report unavailable and apply the predefined risk policy—never report success.

## Practical exercise

Introduce one disposable secret, vulnerable dependency, insecure IaC rule, source flaw, and image package. Run corresponding scans, triage results, revoke the secret, fix three, and document one expiring exception. Verify scan outage handling.

## Official references

- [Configure Advanced Security for Azure DevOps](https://learn.microsoft.com/azure/devops/repos/security/configure-github-advanced-security-features)
- [Secret scanning](https://learn.microsoft.com/azure/devops/repos/security/github-advanced-security-secret-scanning)
- [Microsoft SDL practices](https://www.microsoft.com/securityengineering/sdl/practices)

[Next: Auditing, Compliance, and Evidence →](08-auditing-compliance-and-evidence.md)
