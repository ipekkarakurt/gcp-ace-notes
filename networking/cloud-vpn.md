## Cloud VPN (Classic)

securely connects your on-premises network to your Google Cloud VPC network through an IPsec VPN tunnel

- Traffic traveling between the two networks is encrypted by one VPN gateway, then decrypted by the other VPN gateway.
  - protects your data as it travels over the public internet
  - Cloud VPN is useful for low-volume data connections.
- in order to connect to your on-premises network and its resources, you need to configure your Cloud VPN gateway, on-premises VPN gateway, and two VPN tunnels
  - The Cloud VPN gateway is a regional resource that uses a regional external IP address.
  - Your on-premises VPN gateway can be a physical device in your data center or a physical or software-based VPN offering in another cloud provider's network. This VPN gateway also has an external IP address.
  - A VPN tunnel then connects your VPN gateways and serves as the virtual medium through which encrypted traffic is passed. In order to create a connection between two VPN gateways, you must establish two VPN tunnels
- when using Cloud VPN, the maximum transmission unit (MTU), for your on-premises VPN gateway cannot be greater than 1460 bytes. (due to encryption and encapsulation of packets)
-  Cloud Router can manage routes for a Cloud VPN tunnel using **Border Gateway Protocol (BGP)**. This routing method allows for routes to be updated and exchanged without changing the tunnel configuration.

### HA VPN
high availability Cloud VPN solution that lets you securely connect your on-premises network to your Virtual Private Cloud (VPC) network through an IPsec VPN connection in a single region.
- When you create an HA VPN gateway, Google Cloud automatically chooses two external IP addresses, one for each of its fixed number of two interfaces.
  - Each of the HA VPN gateway interfaces supports multiple tunnels. You can also create multiple HA VPN gateways.
- VPN tunnels connected to HA VPN gateways must use dynamic (BGP) routing
- A VPN supports site-to-site VPN in one of the following recommended topologies or configuration scenarios:
  - An HA VPN gateway to peer VPN devices
  - An HA VPN gateway to an Amazon Web Services (AWS) virtual private gateway
  - Two HA VPN gateways connected to each other

https://cloud.google.com/network-connectivity/docs/vpn/concepts/topologies
https://cloud.google.com/network-connectivity/docs/vpn/how-to/moving-to-ha-vpn

###Commands
gcloud compute networks create vpc-demo --subnet-mode custom

gcloud compute networks subnets create vpc-demo-subnet1 \
--network vpc-demo --range 10.1.1.0/24 --region us-central1

gcloud compute firewall-rules create vpc-demo-allow-custom \
  --network vpc-demo \
  --allow tcp:0-65535,udp:0-65535,icmp \
  --source-ranges 10.0.0.0/8
