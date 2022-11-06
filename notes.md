# GCP Notes

## Google recommended practices
- Assign IAM roles to groups, instead of assigning roles to individuals.
  - Groups are easier to manage than individual users.
  - Groups provide high level visibility into roles and permissions.
- Cloud Foundation Toolkit (CFT) provides ready-made templates that reflect Google Cloud recommended practices and can be used to automate creation of dev/staging/prod environments.
- Attach labels to resources to reflect the owner and purpose: when analysing billing data, these labels can be propagated into billing items.

## IAM
- Basic roles do not have enough granularity to account for access to sensitive data.
- Predefined roles are fine-grained enough to set permissions for specific roles requiring sensitive data access.

# Storage
- Bigtable provides high-speed reads and writes, accommodates a simple schema, and is cost-effective. Good for IoT.
-  BigQuery does not provide the high-speed reads and writes required by IoT.

# GKE
- Autopilot: designed to reduce the operational cost of managing clusters and optimize your clusters for production.

# TODO
- Cloud Interconnect vs Cloud VPN
- Bigtable: HBase does not allow ad-hoc queries.
