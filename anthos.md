## Anthos

hybrid and multi cloud solution
rests on Kubernetes and GKE On-Prem (both have access to Cloud Marketplace)

- use the same configurations on both sides of the network, which reduces time spent maintaining conformity between your clusters.
- spend less time developing applications because of a write once replicates anywhere approach.
- Anthos + Istio service mesh layers communicate across the hybrid network using cloud Interconnect to sync and pass their data.
- Cloud Logging watches all sides of your hybrid or multi cloud network.
- Anthos provides a single authoritative source of truth for your cluster's configuration (kept in the policy repository, which is actually a git repository) The repository can be located on premises or hosted in the cloud.

## Migrate for Anthos

tool for getting workloads into containerized deployments on Google Cloud. moves your existing applications into a Kubernetes environment through an automated process.

You can migrate from VMware, AWS, Azure or Google cloud

- migctl command
  - create source migrating from Compute Engine: migtcl source create ce [SOURCE-NAME] -- project [PROJECT] --zone [ZONE]
  - create migration plan: migctl migration create...
  - generate artifacts for migration: migctl migration generate-artifacts [MIG-NAME] 
