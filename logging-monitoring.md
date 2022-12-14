## Audit Logs

- "who did what, where, and when?" within your Google Cloud resources
- types of audit logs:
  - Admin Activity:
    - API calls or other actions that modify the configuration or metadata of resources
    - can't be disabled. Even if you disable the Cloud Logging API, Admin Activity audit logs are still generated.
  - Data Access:
    - API calls that read the configuration or metadata of resources, as well as user-driven API calls that create, modify, or read user-provided resource data.
    - disabled by default because audit logs can be quite large. must be enabled manually.
  - System Event:
    - Google Cloud actions that modify the configuration of resources
    - can't be disabled.
  - Policy Denied
    - Google Cloud service denies access to a user or service account because of a security policy violation.
- Logs Viewer ( roles/logging.viewer ): read-only access to all features of Logging, except Access Transparency logs and Data Access audit logs.
- Private Logs Viewer ( roles/logging.privateLogViewer ) – includes roles/logging.viewer, plus the ability to read Access Transparency logs and Data Access audit logs.
