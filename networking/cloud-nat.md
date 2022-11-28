## Cloud NAT (Network Address Translation)
managed network address translation service. provides internet access to private instances (for updates, patching, configuration management etc).

- VM instances without external IP addresses are isolated from external networks. Using Cloud NAT, these instances can access the internet for updates and patches.
- Cloud NAT is a regional resource. You can configure it to allow traffic from all ranges of all subnets in a region, from specific subnets in the region only, or from specific primary and secondary CIDR ranges only.
- The Cloud NAT gateway implements outbound NAT, but not inbound NAT (hosts outside of your VPC network can only respond to connections initiated by your instances; they cannot initiate their own connections to your instances via NAT)
- Cloud NAT flow logs provides two types of logs:
  - Translation: a VM instance initiates a connection that is successfully allocated to a Cloud NAT IP and port and traverses to the internet
  - Error: a VM instance attempts to connect to the internet by sending a packet over the connection, but the Cloud NAT gateway can't allocate a Cloud NAT IP and port due to port exhaustion
