# Access Levels, Groups, and Permissions

[← Authentication and Entra ID](01-authentication-authorization-and-entra-id.md) · [Chapter 14](README.md) · [Next: Least Privilege →](03-least-privilege-and-separation-of-duties.md)

## Four related mechanisms

- **Access level:** Stakeholder, Basic, Basic + Test Plans, or eligible subscription benefits determine feature availability.
- **Security group:** collection of identities with permissions.
- **Permission/ACL:** allow, deny, or inherited/not-set action at organization, project, or object scope.
- **Role:** resource-specific responsibility such as environment or library role.

A permission cannot grant a feature excluded by the user's access level.

## Effective permissions

Azure DevOps combines direct assignments, inherited scopes, all direct/indirect groups, roles, and explicit denies. A deny generally overrides allows and can come from an unexpected group. “Not set” is not the same as deny; it permits inheritance.

Troubleshooting sequence:

1. Confirm exact sign-in identity.
2. Check organization membership and access level.
3. Enumerate direct/indirect Azure DevOps and Entra groups.
4. Inspect object and parent-scope assignments.
5. Look for explicit deny.
6. Check resource role and pipeline permissions.
7. Refresh Entra membership/sign-in if required.
8. Use permission trace/REST/CLI and audit history.

## Administration model

Grant access to purpose-specific groups, preferably managed through Microsoft Entra for workforce lifecycle. Use built-in Readers/Contributors/Project Administrators only when their breadth matches the job. Avoid direct user permissions because they are difficult to review/offboard.

Limit Project Collection Administrators severely. Project Administrators can still alter many powerful project settings. Review repository bypass, force-push, pipeline administration, agent-pool, service-hook, feed-owner, and protected-resource roles separately.

## Access reviews

Periodically inventory:

- Dormant/inactive identities and guests.
- Admin groups and nested membership.
- Direct grants and explicit denies.
- Broad “all pipelines” resource authorization.
- Application/service identities and access levels.
- Cross-project/organization access.
- Public project or external collaboration settings.
- Time-limited privilege that did not expire.

Tie review frequency to risk and preserve reviewer, scope, evidence, decision, and remediation.

## Interview preparation

**Why can a Basic user still fail to edit a repo?**  
Basic enables the feature; repository permissions still authorize the action.

**Why does deny surprise teams?**  
A user may inherit it through any group/scope, and it overrides otherwise valid allows.

**Groups versus direct grants?**  
Groups provide role-based lifecycle, consistent review, and simpler offboarding. Direct grants create invisible exceptions.

## Practical exercise

Create Reader, Developer, and Release Operator groups. Assign minimal scopes, add one user through two groups with a deliberate deny, and use effective-permission tools to explain the result. Remove every direct grant.

## Official references

- [About permissions and security groups](https://learn.microsoft.com/azure/devops/organizations/security/about-permissions)
- [View effective permissions](https://learn.microsoft.com/azure/devops/organizations/security/view-permissions)
- [Default permission reference](https://learn.microsoft.com/azure/devops/organizations/security/permissions-access)

[Next: Least Privilege and Separation of Duties →](03-least-privilege-and-separation-of-duties.md)
