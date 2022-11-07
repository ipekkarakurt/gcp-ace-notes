## Resource Hierarchy
- resources > projects > folders > organization node
- resources: VMs, Cloud Storage buckets, BigQuery tables etc.
- projects can have different owners and users because they are **billed and managed separately**.
- Each Google Cloud project has 3 identifying attributes: project ID, project name & project number.
  - Project ID: globally unique, assigned by Google, immutable.
  - Project name: don’t have to be unique, user-created, mutable.
  - Project number: globally unique, assigned by Google, used internally by Google.
- organization node creation:
  - Google workspace customer => projects automatically belong to your organization
  - not a Google workspace customer => use Cloud Identity -> create organization node -> anyone in the domain can create projects and billing accounts
- policies can be defined at project, folder and org node levels. some GCP services allow policies to be applied to individual resources. inherited downward.
  - example to folder level policy: if you have one department managing two projects, put the policy in a folder containing these projects -> easier to manage.
- Resource Manager Tool: programmatically manage projects.
  - API that helps you list/create/update/delete/recover projects. Accessed through RPC API & REST API.

## IAM
- who can do what on which resources
  - who : Google account / Google group / Service Account / Cloud Identity domain
  - can do what: role -> collection of permissions
- When a user, group, or service account is given a role on a specific element of the resource hierarchy, the resulting policy applies to both the chosen element and all the elements below it in the hierarchy.
- 3 role types: basic / predefined / custom.
  - Basic roles: broad in scope. do not have enough granularity to account for access to *sensitive data*.
    - owner / editor / viewer / billing admin
  - Predefined roles: fine-grained enough to set permissions for specific roles requiring *sensitive data access*.
  - Custom roles: for applying least privileged model (each person in your organization is given the minimal amount of privilege needed to do their job and no more).
    - you'll need to manage the permissions that define the Custom role you've created.
    - custom roles can only be applied to either the project level or organization level. they can't be applied to the folder level.

### Service accounts
- named with an email address, but instead of passwords they use cryptographic keys to access resources.
- needs to be managed
- in addition to being an identity, a service account is also a resource, so it can have IAM policies of its own attached to it
  - one person can have the editor role on a service account, and another can have the viewer role.
