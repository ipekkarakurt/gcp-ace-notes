## VPC

secure individual private cloud computing model hosted within GCP

- VPC networks connect Google Cloud resources to each other and to the internet.
- projects contain up to 5 networks by default (additional quota can be requested)
- subnet: segmented piece of the larger network in any Google Cloud region worldwide.
  - subnets can span the zones that make up a region. resources can be in different zones on the same subnets.
- routes: Routes tell VM instances and the VPC network how to send traffic from an instance to a destination.
  - each VPC network comes with some default routes to route traffic among its subnets and send traffic from eligible instances to the internet.
  - you can create custom static routes to direct some packets to specific destinations. ex: you can create a route that sends all outbound traffic to an instance configured as a NAT gateway.
- routing tables: forward traffic from one instance to another within the same network, across subnetworks, or even between Google Cloud zones, without requiring an external IP address. built-in so you don’t have to provision or manage a router.
- VPC Peering: if your company has several Google Cloud projects, and the VPCs need to talk to each other, a relationship between two VPCs can be established to exchange traffic
- Shared VPC: use the full power of Identity Access Management (IAM) to control who and what in one project can interact with a VPC in another.
- VPC Flow Logs: Flow Logs are used to track network related findings, tracks the network sent from and received by VM instances.
- Cloud NAT: Cloud NAT is Google's managed network address translation service. provides internet access to private instances (for updates, patching, configuration management etc).
- VM instances that only have internal IP addresses (no external IP addresses) can use Private Google Access to reach external IP addresses of Google APIs and services (through the default route (0.0.0.0/0) with a next hop to the default internet gateway). By default, Private Google Access is disabled on a VPC network. Private Google Access is enabled at the subnet level.
- When instances do not have external IP addresses, they can only be reached by other instances on the network via a managed VPN gateway or via a Cloud IAP tunnel. Cloud IAP enables context-aware access to VMs via SSH and RDP without bastion hosts

### Firewall
- Each VPC network implements a distributed virtual firewall that you can configure. Firewall rules allow you to control which packets are allowed to travel to which destinations. Every VPC network has two implied firewall rules that block all incoming connections and allow all outgoing connections.
- VPCs provide a global distributed firewall, which can be controlled to restrict access to instances through both incoming and outgoing traffic.
  - Firewall rules can be defined through network tags on Compute Engine instances.
    - ex: tag all your web servers with “WEB” and write a firewall rule saying that traffic on ports 80 or 443 is allowed into all VMs with the “WEB” tag, no matter what their IP address is.
- There are 4 Ingress firewall rules for the default network (prio 65534):
  - default-allow-icmp
  - default-allow-rdp
  - default-allow-ssh
  - default-allow-internal
These firewall rules allow ICMP, RDP, and SSH ingress traffic from anywhere (0.0.0.0/0) and all TCP, UDP, and ICMP traffic within the network (10.128.0.0/9).
- By default, incoming traffic from outside your network is blocked

### Network Types:
- There are 3 VPC Network types:
  - default:
    - every project
    - preset subnets: 1 subnet per region, non overlapping CIDR blocks
    - default firewall rules: allow ingress for traffic for ICMP, RDP and SSH traffic from anywhere, as well as ingress traffic from within the default network for all protocols and ports
  - auto mode:
    - default network
    - one subnet per region: subnets use a set of predefined IP ranges with a /20 mask that can expanded to a /16 mask
    - regional IP allocation
  - custom mode:
   - no automatic subnet creation
   - full control over subnets & IP ranges
- you can convert auto mode networks to custom mode networks, but not vice versa

### Reserved IPs in Subnets
Google Cloud always reserves 4 IP addresses for every subnet you create. ex: you created a custom VPC with a subnet mask of 24 which provides 256 IP addresses but are only able to use 252 addresses out of it.
- First IP is a network address
- Second is reserved for the default gateway
- Second-to-last is reserved for future use
- Last address is the broadcast address

### Expanding Subnet IP Ranges
you can increase the IP address space of any subnet without any workload shutdown or downtime
- new subnet mask cannot overlap with other subnets in the same VPC network in any region
- Each IP range for all subnets in a VPC network must be a unique valid CIDR block.
- new subnet IP ranges are regional internal IP addresses and have to fall within valid IP ranges.
- can expand but cannot shrink a subnet range
- Subnet ranges cannot span a valid RFC range in a privately used public IP address range. Subnet ranges cannot span multiple RFC ranges.
- auto mode subnets start with a /20 IP range. They can be expanded to a /16 IP range, but no larger. you can convert the auto mode subnetwork to a custom mode subnetwork to increase the IP range further.
- avoid creating large subnets. Overly large subnets are more likely to cause CIDR range collisions

####How can you connect your Google VPC to other networks? (on-premise/other clouds)
- Start a VPN connection **over the Internet** and use the IPSec VPN protocol to create a tunnel connection. To make the connection dynamic => Cloud Router
  - Cloud Router: lets other networks and Google VPC exchange route information over the VPN using the border gateway protocol. If you add a new sub net to your google VPC, your on premises network will automatically get roots to it.
- Direct Peering: not over the internet
  - peering: putting a router in the same public data center as a google point of presence, and using it to exchange traffic between networks.
- Carrier Peering: used if customer is not already in a point of presence. Gives you direct access from your on premises network, through a service provider's network to google workspace and to google cloud products that can be exposed through one or more public IP addresses.
  - downside: not covered by a Google SLA.
- Dedicated Interconnect: if getting the highest up times for interconnection is important. allows one or more direct & private connections to Google.
  - can be backed up by VPN for better reliability.
  - 99.9% SLA
- Partner Interconnect: provides connectivity between an on premises network and a VPC network through a supported service provider. useful if a data center is in a physical location that can't reach a Dedicated Interconnect colocation facility or if the data needs don't warrant an entire 10 GB per second connection.
  - 99.9% SLA
  - Google isn't responsible for any aspects of Partner Interconnect provided by the third party service provider nor any issues outside of Google's network

### DNS Resolution for Internal Addresses
- Each instance has a host name that can be resolved to an internal IP address.
  - hostname is the same as the instance name
  - FQDN (fully qualified domain name): [hostname].[zone].c.[projectid].internal
- Each instance has a metadata server that also acts as a DNS resolver for that instance. The metadata server handles all DNS queries for local network resources and routes all other queries to Google's public DNS servers for public name resolution.

### DNS Resolution for External Addresses
- Instances with external IP addresses can allow connections from hosts outside of the project.
- Public DNS records pointing to instances are not published automatically (admins can publish these using existing DNS servers)

### Common Commands
gcloud compute networks create <NETWORK_NAME> --project=<PROJ_ID> --subnet-mode=custom --mtu=1460 --bgp-routing-mode=regional

gcloud compute networks subnets create <SUBNET_NAME> --project=<PROJ_ID> --range=10.130.0.0/20 --stack-type=IPV4_ONLY --network=<NETWORK_NAME> --region=us-central1

gcloud compute firewall-rules create <FIREWALL_NAME> --network <NETWORK_NAME> --allow tcp,udp,icmp --source-ranges <IP_RANGE>

gcloud compute --project=<PROJ_ID> firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=<NETWORK_NAME> --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0

gcloud compute networks list

gcloud compute networks subnets list --sort-by=NETWORK

gcloud compute firewall-rules list --sort-by=NETWORK

gcloud compute instances create <VM_NAME> --zone=us-central1-c --machine-type=f1-micro --subnet=privatesubnet-us --image-family=debian-10 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=privatenet-us-vm

gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
