# Compute Engine
IaaS solution
create and run VMs.
choose the machine properties of your instances, like the number of virtual CPUs and the amount of memory, by using a set of predefined machine types or by creating your own custom machine types.

Compute Engine bills by the second with a one-minute minimum, and sustained-use discounts start to apply automatically to virtual machines the longer they run.
- sustained use discount: VM runs for more than 25% of a month => Compute Engine automatically applies a discount for every additional minute.
- committed-use discounts: for stable and predictable workloads, a specific amount of vCPUs and memory can be purchased for up to a 57% discount off of normal prices in return for committing to a usage term of one year or three years.
- Preemptible and Spot VMs: save up to 90%.
  - Compute Engine can terminate a job if its resources are needed elsewhere.
  - you'll need to ensure that your job can be stopped and restarted.
  - preemptible VMs can only run for up to 24 hours at a time, but Spot VMs do not have a maximum runtime.
  - ex: a batch job analyzing a large dataset
