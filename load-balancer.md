# Load Balancer

- distribute user traffic across multiple instances of an application.
- load balancing reduces the risk of performance issues.
- provides cross-region load balancing, including automatic multi-region failover, which gently moves traffic in fractions if backends become unhealthy.

- External Load Balancing (for traffic coming into the Google network from the internet):
  - Global HTTP(S) load balancing: cross-regional load balancing for a web application.
  - Global SSL Proxy load balancer: for Secure Sockets Layer traffic that is not HTTP. only works for specific port numbers, and only works for TCP.
  - Global TCP Proxy load balancer: If it’s other TCP traffic that doesn’t use SSL. only works for specific port numbers, and only works for TCP.
  - Regional load balancer: load balance UDP traffic, or traffic on any port number across a Google Cloud region.
- Internal Load Balancing:
  - Regional internal load balancer: accepts traffic on a Google Cloud internal IP address and load balances it across Compute Engine VMs.
    - ex: between the presentation layer and business layer of your applications.
