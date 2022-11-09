
## VPC
- VPC is a secure individual private cloud computing model hosted within a public cloud, like Google Cloud.
- VPC networks connect Google Cloud resources to each other and to the internet.
- subnet: segmented piece of the larger network in any Google loud region worldwide.
  - subnets can span the zones that make up a region. resources can be in different zones on the same subnets.
- routing tables: forward traffic from one instance to another within the same network, across subnetworks, or even between Google Cloud zones, without requiring an external IP address. built-in so you don’t have to provision or manage a router.
- firewall: VPCs provide a global distributed firewall, which can be controlled to restrict access to instances through both incoming and outgoing traffic.
  - Firewall rules can be defined through network tags on Compute Engine instances.
    - ex: tag all your web servers with “WEB” and write a firewall rule saying that traffic on ports 80 or 443 is allowed into all VMs with the “WEB” tag, no matter what their IP address is.
- VPC Peering: if your company has several Google Cloud projects, and the VPCs need to talk to each other, a relationship between two VPCs can be established to exchange traffic
- Shared VPC: use the full power of Identity Access Management (IAM) to control who and what in one project can interact with a VPC in another.
- VPC Flow Logs: Flow Logs are used to track network related findings, tracks the network sent from and received by VM instances.


Google Cloud always reserves 4 IP addresses for every subnet you create. ex: you created a custom VPC with a subnet mask of 24 which provides 256 IP addresses but are only able to use 252 addresses out of it.
- First IP is a network address
- Second is reserved for the default gateway
- Second-to-last is reserved for future use
- Last address is the broadcast address
