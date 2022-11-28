## IP Addresses

- 2 types of IP addresses:
  - Internal IP: Every VM that starts up and any service that depends on virtual machines gets an internal IP address.
    - allocated from subnet range to VMs by DHCP
    - VM name + IP is registered with DNS (that is scoped to the network! can translate web URLs and VM names of hosts in the same network)
  - External IP (optional): You can assign an external IP address if your device or machine is externally facing
    - ephemeral: can be assigned from a pool
    - static: can be assigned from a reserved external IP
    - BYOIP: you can use your own publicly routable IP address prefixes as Google Cloud external IP addresses and advertise them on the Internet (must be a slash 24 block or larger)
    - the external address is unknown to the OS of the VM
