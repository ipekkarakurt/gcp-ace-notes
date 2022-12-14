## Compute Engine

IaaS solution to create and run VMs.

- great option for quick migration of traditional apps. implement a solution in the cloud without changing your existing code.
- vCPU: affects throughput
  - each vCPU is implemented as a single hardware hyper-thread
  - network throughput scales 2 Gbps per vCPU
  - theoretical max:
    - 32 Gbps with 16 vCPU
    - 100 Gbps with T4 or V100 GPUs
- Machine types:
  - General purpose
  - Compute optimized
  - Memory optimized
  - Accelerator optimized (GPU intensive, parallelized for ML and high-performance computing)
- Boot disk:
  - When you create a virtual machine (VM) instance, you must also create a boot disk for the VM. You can use a public image, a custom image, or a snapshot that was taken from another boot disk.
  - **As a best practice, do not use regional persistent disks for boot disks. In a failover situation, they do not force-attach to a VM.**
- Snapshots: backup critical data into a durable storage solution
  - stored in Cloud Storage
  - used to migrate data between zones, transferring data to a different disk type
  - only available to persistent disks, not local SSDs.
  - difference to images:
    - images are used to create instances or configure instance templates
    - snapshots are useful for periodic backup of the data on your persistent disks
    - snapshots are incremental and automatically compressed, so you can create regular snapshots on a persistent disk faster and at lower cost than if you regularly created a full image of the disk
- creator of an instance has full root privileges on that instance.
  - Linux instance: creator has SSH capability and can use the Cloud Console to grant SSH capability to other users. firewall should allow tcp: 22.
  - Windows instance: the creator uses Cloud Console to generate a username and password. anyone who knows the username a password can connect to the instance using a RDP client. firewall should allow tcp: 3389.
- when a VM is terminated, you are not charged for memory or CPU resources but you are charged for attached disks and reserved IP addresses.
- guest attributes: a specific type of custom metadata that your applications can write to while running on your virtual machine (VM) instance. When guest attributes are enabled, Compute Engine will store your generated host keys as guest attributes
- Sole tenancy: physically isolate your workloads from other workloads for compliance requirements
- Shielded VM: verifiable integrity, defend against rootkits and bootkits.
  - Virtual Trusted Platform Module (vTPM): virtualized trusted platform module, which is a specialized computer chip you can use to protect objects, like keys and certificates.
  - Secure boot: ensure that the system only runs authentic software by verifying the digital signature of all boot components, and halting the boot process if signature verification fails
  - integrity monitoring
- Confidential VMs: encrypt data in use
- By default Compute Engine encrypts all data at rest
- images: includes the boot loader, the operating system, the file system structure, any pre configured software and any other customizations
  - public images:
    - premium images have per second charges after a one minute minimum
  - custom images:
    - create from VM: pre installing software that's been authorized for your org
    - importing images from your own premises
- Compute Engine can launch containers.
  - accepts the container image from you and launch a virtual machine instance that contains it.

### Pricing
- resources (GB of memory, vCPU, GPU) charged for a minimum of 1 minute, then 1 sec increments.
- discounts start to apply automatically, discount types cannot be combined:
  - sustained use discount: VM runs for **more than 25%** of a month => Compute Engine automatically applies a discount for every additional minute.
  - committed-use discounts: for stable and predictable workloads, purchase vCPUs and memory with up to 57% discount if you **commit to a usage term of one year or three years**.
  - Preemptible and Spot VMs: save up to 90%.
    - Spot VM is the latest version of preemptible VMs.
      - difference: preemptible VMs can only run for up to 24 hours at a time, but Spot VMs do not have a maximum runtime.
    - Compute Engine can terminate a job if its resources are needed elsewhere.
    - when preemption notice is received, you can run a shutdown script.
    - you'll need to ensure that your job can be stopped and restarted.
    - no live migrate or auto restarts (you can restart via external solutions, using monitoring and load balancers)
    - ex: a batch job analyzing a large dataset

### Storage
- By default, each Compute Engine instance has a single boot persistent disk (PD) that contains the operating system. When your apps require additional storage space, you can add one or more additional storage options to your instance.
- Persistent Disk: attached to the VM through the network interface
  - disk survives if the VM terminates (durable)
  - you can take snapshots: incremental backups
  - disks can be dynamically resized (even when the VM is running)
  - attach a disk in a read-only mode to multiple VMs => share static data between multiple instances (cheaper than replicating your data to unique disks for individual instances)
  - zonal or regional
    - zonal: efficient, reliable block storage
    - regional: durable storage that has synchronously replicated across zones
  - disk types:
    - pd-standard: backed by a standard HDD. Suitable for large data processing workloads that primarily use sequential I/Os.
    - pd-balanced: backed by SSD. Balance of performance and cost. suitable for most general-purpose applications.
    - pd-ssd: backed by SSD. enterprise applications and high-performance databases that require lower latency and more IOPS than standard persistent disks provide.
    - pd-extreme: zonal only. backed by SSD. designed for high end db workloads, high throughput
- RAM disks: faster than local disk, slower than memory
  - use tmpfs if you want to store data in memory
- Storage options for Compute Engine:
  - Regional persistent disk (SSD or HDD (standard/balanced))
  - Zonal persistent disk (SSD or HDD (standard/balanced))
  - Local SSD: data persists only until you stop/terminate the instance. can survive a reset. typically used as a swap disk/scratch space. very high IOPS. lower latency. high throughput.no snapshotting. no data redundancy.
  - Cloud Storage
  - Filestore
- SSD: higher IOPS per dollar
- HDD: higher capacity per dollar
- Disk limits:
  - shared core VM: 16
  - other VMs: 128

### Moving VMs
- moving zones within region: gcloud compute instances move
  - To move your VM, you must shut down the VM, move it to its destination zone or region, and then restart it. Update references to the VM.
- moving regions:
  - snapshot all attached persistent disks
  - create new persistent disks in destination zone from these snapshots
  - create new vm in destination zone and attach new disks
  - assign static IP
  - update all references to VM
  - delete snapshots, original disks & original VM

### States of a VM
- Provisioning: after defining all the properties of an instance and clicking Create
  - resources (CPU, memory, and disks) are being reserved for the instance, but the instance itself isn’t running yet
- Staging: resources have been acquired, instance is prepared for launch.
  - Compute Engine is adding IP addresses, booting up the system image, and booting up the system
- Running
  - startup scripts and enable SSH or RDP access.
  - you can live migrate your VM to another host in the same zone (instead of rebooting): for maintenance without interruption
  - you can also move your VM to a different zone, take a snapshot of the VM’s persistent disk, export the system image, or reconfigure metadata
- Stopping
  - an instance needs to be stopped when upgrading your machine by adding more CPU
  - runs shutdown script
  - takes about 90 seconds. so shutdown scripts must be completed within that time or VM will be forced to shut down.
  - preemptible VMs shutdown takes 30 seconds.
- Terminated

### When to choose Compute Engine
- complete control over your infrastructure. customize operating systems and even run applications that rely on a mix of operating systems.
- easily lift and shift your on premises workloads into GCP without rewriting your applications or making any changes.

gcloud compute instances set-service-account (assigns a service account to a VM instance)
gcloud compute disks snapshot DISK_NAME (creates snapshots of persistent disks)
