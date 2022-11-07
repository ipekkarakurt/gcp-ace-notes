
# GCP Notes

## Storage
- Bigtable provides high-speed reads and writes, accommodates a simple schema, and is cost-effective. Good for IoT.
-  BigQuery does not provide the high-speed reads and writes required by IoT.

## GKE
- _Autopilot_: designed to reduce the operational cost of managing clusters and optimize your clusters for production.

## Billing
- You can define budgets either per billing account or per GCP project. A budget can be a fixed limit or you can tie it to another metric.
- _Alerts_: notifies when costs approach your budget limit.
- _Billing export_: store detailed billing information for more detailed analysis, in a BigQuery dataset or a Cloud storage bucket.
- _Quotas_: prevent the over-consumption of resources (due to error/malicious attack).
  - two types of quotas: rate quotas and allocation quotas.
    - Rate quotas reset after a specific time.
    - Allocation quotas control the # of resources you can have in your projects.

## Google recommended practices
- Assign IAM roles to groups, instead of assigning roles to individuals.
  - Groups are easier to manage than individual users.
  - Groups provide high level visibility into roles and permissions.
- Cloud Foundation Toolkit (CFT) provides ready-made templates that reflect Google Cloud recommended practices and can be used to automate creation of dev/staging/prod environments.
- Attach labels to resources to reflect the owner and purpose: when analysing billing data, these labels can be propagated into billing items.
