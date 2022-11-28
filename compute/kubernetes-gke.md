## Kubernetes
open source platform for managing containerized workloads and services

- uses container-optimized operating system.
- supports declarative configurations.
  - describe the desired state you want to achieve instead of issuing a series of commands to achieve that desired state.
  - also allows imperative configuration but not recommended at scale
- primary components = control plane + a set of nodes that run containers.
  - Kubernetes control plane: the various system processes that collaborate to make a Kubernetes cluster work
  - node: virtual machines that host your containers inside of a GKE cluster
- pod: **the smallest unit** in Kubernetes that you can create or deploy. It represents a running process on your cluster as either a component of replication or an entire app.
  - generally you only have one container per pod, but if you have multiple containers that are tightly coupled, you can package them into a single pod and share networking and storage resources between them
    - every container within a pod shares the network namespace including IP address and network ports
    - containers within the same pod can communicate through localhost 127.0.0.1
  - pods are not self healing.
  - designed to be ephemeral and disposable
- deployment: represents a group of replicas of the same pod and keeps your pods running even when the node they run on fail.
  - for long lived software components
  - in deployment object spec, specify:
    - how many replica pods you want
    - how pods should run
    - which containers should run within these pods
    - which volumes should be mounted
  - allows a rolling upgrade of the pods it manages -> To perform the upgrade, the deployment object will create a second ReplicaSet object, and then increase the number of (upgraded) pods in the second ReplicaSet while it decreases the number in the first ReplicaSet.
  - deployment config file: Instead of issuing commands, you provide a configuration file that tells Kubernetes what you want.
- namespace: provide scope for naming resources such as Pods, Deployments, and controllers.
  - let you implement **resource quotas** across the cluster.
  - initial 3 namespaces in a cluster:
    - default: workload resources will use this namespace by default.
    - kube-system: namespace for objects created by the Kubernetes system itself.
    - kube-public: namespace for objects that are publicly readable to all users.
- A **service** is an abstraction which defines a logical set of pods and a policy by which to access them.
- what happens when you want to update a new version of your app?
  - kubectl rollout: New pods will then be created according to your new update strategy

### Kubernetes Control Plane
- kube-apiserver: accept commands that view or change the state of the cluster. authenticates incoming requests, determines whether they are authorized and valid, and manages admission control.
  - kubectl command: connects to the kube-apiserver and communicates with it using the Kubernetes API
- etcd: cluster's database, stores state of the cluster. ex:  all of the cluster configuration data and more dynamic information, such as what nodes are part of the cluster, what pods should be running and where they should be running
  - You'll never interact directly with etcd, instead kube-apiserver interacts with the database on behalf of the rest of the system.
- kube-scheduler: schedules pods onto nodes
  - doesn't do the work of actually launching the pods on the nodes. chooses a node without an assignment and simply writes the name of that node into the pod object
  - knows the state of all of the nodes and it will obey constraints that you define on where a pod may run
- kube-controller-manager: continuously monitors the state of the cluster through kube-apiserver. Whenever the current state of the cluster doesn't match the desired state, kube-controller-manager will attempt to make changes to achieve the desired state
- kube-cloud-manager: manages controllers that interact with the underlying cloud providers
- Additionally, each node runs kubelet (a Kubernetes agent) and kube-proxy
  - When the kube-apiserver wants to start a pod on a node, it connects to that nodes kubelet.
  - kubelet uses the container runtime to start the pod and monitors its life cycle, including readiness and liveliness probes and reports back to kube-apiserver.
  - kube-proxy's job is to maintain the network connectivity among the pods in a cluster.

### Kubernetes Services
- Services provide load-balanced access to specified Pods. There are three primary types of Services:
  - ClusterIP: Exposes the service on an IP address that is only accessible from within this cluster.
  - NodePort: Exposes the service on the IP address of each node in the cluster, at a specific port number.
  - LoadBalancer: Exposes the service externally, using a load balancing service provided by a cloud provider.

### Kubernetes Controller Objects
- ReplicaSet: ensures that a population of pods, all identical to one another, are running at the same time.
- Deployments: let you create, update, roll back, and scale pods using ReplicaSets
  - when you perform a rolling upgrade of a deployment, the deployment object creates a second ReplicaSet, and then increases the number of Pods in the new ReplicaSet as it decreases the number of Pods in its original ReplicaSet.
- StatefulSet: similar to a deployment, but pods created using StatefulSet have unique **persistent** identities with stable network identity and persistent disk storage.
- DaemonSet: ensures that a specific Pod is always running on all or some subset of the nodes.
- Job controller: creates one or more Pods required to run a task
- CronJob controller: runs Pods on a time-based schedule.

## Google Kubernetes Engine
GKE is a Google-hosted managed Kubernetes service in the Cloud
consists of Compute Engine instances grouped together to form a cluster
- GKE manages all of the control plane components, exposes an IP address to which we send all of our Kubernetes API requests.
- launches Compute Engine virtual machine instances and registers them as nodes
  - nodes run on Compute Engine so machine type, # cores and memory can be customized
  - GKE clusters can be customized and they support different machine types, number of nodes, and network settings.
- By default:
  - a cluster launches on a single Google Cloud Compute Zone with three identical nodes, all in one node pool.
  - a regional cluster is spread across 3 zones, each containing 1 control plane and 3 nodes.
- overhead: some of each node's CPU and memory are needed to run the GKE and Kubernetes components that let it work as part of your cluster.
- advantages:
  - Google Cloud's load balancing for Compute Engine instances
  - node pools to designate subsets of nodes within the cluster
  - automatic scaling
  - automatic upgrades for your clusters node software
  - node auto repair to maintain note health and availability
  - logging and monitoring with Google Cloud's operation suite for visibility into your cluster
- node pool: subset of nodes within a cluster that share a configuration
  - ensures that workloads run on the right hardware within your cluster -> label them with a desired node pool.
- pay per hour of life of your nodes (not counting the control plane)
- once you build a zonal cluster, you cannot convert it into a regional cluster or vice versa
- private cluster: entire cluster is hidden from the public internet
  - accessed by authorized networks through external IP
  - accessed by Google Cloud products through internal IP
  - limited outbound access through Private Google Access, which allows them to communicate with other Google Cloud services (nodes can pull container images from Google Container Registry without needing external IP addresses.)
- In GKE, LoadBalancers give you access to a regional Network Load Balancing configuration by default. To get access to a global HTTP(S) Load Balancing configuration, you can use an Ingress object.

### When to use GKE
-  containerized applications, cloud-native distributed systems, and hybrid applications.

- to see a list of the running pods in your project: kubectl get pods.
- to scale the deployment: kubectl scale
- create a cluster: gcloud container clusters create k1
- get pods by the label: kubectl get pods --selector=key=value
