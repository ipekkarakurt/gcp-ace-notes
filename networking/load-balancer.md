## Load Balancer

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

- Global load balancers: when your users and instances are globally distributed, your users need access to the same application and content, and you want to provide access using a single anycast IP address.
  - HTTP(S)
  - SSL Proxy
  - TCP Proxy
- Regional load balancers: internal and network load balancers, distribute traffic to instances that are in a single GCP region.
  - Internal TCP/UDP
  - Network TCP/UDP
  - Internal HTTP(S)

###HTTP(S) Load Balancing
Layer 7 of OSI model.
- global load Balancing
- single anycast IP address: simplified DNS setup
- HTTP: load balanced on port 80/8080
- HTTPS: load balanced on port 443
- IPv4 or IPv6
- URL maps: route some URLs to one set of instances and route other URLs to other instances.
- cross-region load balancing: if there are no healthy instances, with available capacity in a given region, the load balancer instead sends the request to the next closest region with available capacity.
- HTTPS -> Backend buckets allow you to use Google Cloud Storage buckets with HTTPS Load Balancing. An external HTTPS load balancer uses a URL map to direct traffic from specified URLs to either a backend service or a backend bucket.
- network endpoint group(NEG): a configuration object that specifies a group of backend endpoints or services.

#### Cloud CDN
uses Google's globally distributed edge points of presence to cache HTTP(S) load-balanced content close to your users.

cache mode: control the factors that determine whether or not Cloud CDN caches your content
- USE_ORIGIN_HEADERS: requires origin responses to set valid cache directives and valid caching headers.
- CACHE_ALL_STATIC: automatically caches static content that doesn't have the no-store, private, or no-cache directive
- FORCE_CACHE_ALL: unconditionally caches responses, overriding any cache directives set by the origin

### SSL Proxy Load balancing
- global load balancing for encrypted non HTTP traffic
- terminates client SSL connection at load balancer layer
- IPv4/IPv6
- Intelligent routing: routes traffic to instances that have capacity
- Certificate management: you only need to update your customer-facing certificate in one place when you need to switch those certificates. reduce the management overhead for your virtual machine instances by using self-signed certificates on your instances.
- Security patching

### TCP Proxy Load Balancing
- global load balancing for encrypted non HTTP traffic
- terminates client TCP session at load balancer layer, then forwards the traffic to your virtual machine instances using TCP or SSL
- IPv4/IPv6
- Intelligent routing: routes traffic to instances that have capacity
- Security patching

### Network Load Balancing
- regional, non proxied load balancer
- forwarding rules -> to balance the load of your systems based on the incoming IP protocol data (address, port and protocol type.)
- You can use it to load balance UDP traffic and to load balance TCP and SSL traffic on ports that are not supported with the TCP proxy and SSL proxy load balancers.

### Internal Load Balancing
- Internal TCP/UDP
  - regional, private load balancer: run and scale your services behind a private load balancing IP address
    - it's only accessible through internal IP addresses or virtual machine instances in the same region
  - low latency: your internal client requests stay internal to your VPC network and region
- Internal HTTPS:
  - proxy-based regional layer 7 load balancer
