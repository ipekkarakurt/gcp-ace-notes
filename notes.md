
# GCP Notes

## Storage
- Bigtable provides high-speed reads and writes, accommodates a simple schema, and is cost-effective. Good for IoT.
-  BigQuery does not provide the high-speed reads and writes required by IoT.

## GKE
- _Autopilot_: designed to reduce the operational cost of managing clusters and optimize your clusters for production.

## Billing
- You can define budgets either per billing account or per GCP project. A budget can be a fixed limit or you can tie it to another metric.
- _Alerts_: notifies when costs approach your budget limit. notification or triggered webhook.
- _Billing export_: store detailed billing information for more detailed analysis, in a BigQuery dataset or a Cloud storage bucket (as CSV or JSON).
- _Quotas_: prevent the over-consumption of resources (due to error/malicious attack).
  - applies at project level
  - two types of quotas: rate quotas and allocation quotas.
    - Rate quotas reset after a specific time.
    - Allocation quotas control the # of resources you can have in your projects.
- When you create a budget, notifications go to:
  - Billing admins & users, by default
  - Email notification channels (max 5, when configured)
  - PubSub topic (when configured)
- Billing can be exported to BigQuery only. File export is deprecated.

## Google recommended practices
- Assign IAM roles to groups, instead of assigning roles to individuals.
  - Groups are easier to manage than individual users.
  - Groups provide high level visibility into roles and permissions.
- Cloud Foundation Toolkit (CFT) provides ready-made templates that reflect Google Cloud recommended practices and can be used to automate creation of dev/staging/prod environments.
- Attach labels to resources to reflect the owner and purpose: when analysing billing data, these labels can be propagated into billing items.

- SLI (Service Level Indicator) = # good events / # all events
- SLO (Service Level Objective): set SMART
- SLA (Service Level Agreement): commitment made to customers & what happens when you break that promise

- Cloud Shell is a temporary Virtual Machine with five gigabytes of persistent disk storage that has the Cloud SDK pre-installed. After 1 hour of inactivity, the Cloud Shell instance is recycled. Only the /home directory persists. Any changes made to the system configuration, including environment variables, are lost between sessions.
- If you use Cloud Shell frequently, you may want to set common values in environment variables and use them instead of typing the actual values. -> set the env variables in a file & source that file in .profile
- Google Cloud Marketplace lets you quickly deploy functional software packages by providing pre-defined templates with Deploment Manager. Deployment Manager is a Google Cloud service that uses templates written in a combination of YAML, python, and Jinja2 to automate the allocation of Google Cloud resources and perform setup tasks.
- **Images, snapshots, and networks, are global resources. External IP addresses are regional resources. Instances and disks are zonal resources.**
- Tags vs labels: Labels are user-defined strings and key-value formats that are used to organize resources and they can propagate true billing. Tags are user-defined strings that are applied to instances only and are mainly used for networking, such as applying firewall rules.
- Cloud SDK Configuration: default Google Compute Engine zone, verbosity level, usage reporting, project ID, and an active user or service account
