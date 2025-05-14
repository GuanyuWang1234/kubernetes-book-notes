Pods are 
## An abstraction layer 
K8s wouldn't know the difference of containers or VMs in pods 

Benefits 
- K8s can focus on deploying and managing pods 
- Heterogenous workloads can run side-by-side 

Knative/KubeVirt extend the K8s API to run serverless functions/VMs 

## Pods augment workloads
Resource sharing, Advanced scheduling, Health probes, Etc

## Pods enable resource sharing
All containers in the same pod share the pods execution environment

This include shared file system and volumes, net, IPC, PID, uts.

## Scheduling
Only put containers in the same pod if they need to share resources

 NodeSelectors: The scheduler will only assign pods to a node with all the labels
 Affinity and anti-affinity: Affinity rules attract; anti-affinity rules repel; hard rules must be obeyed; soft rules are suggestions

## Deploying pods
1. Define the pod in a manifest file
2. Post the Manifest to the API server
3. The scheduler filters nodes based on node selectors/affinity rules/etc
4. The pod is assigned to a healthy node meeting all requirements
5. The kubelet on the Node watches the API server and notices the Pod assignment
6. The kubelet downloads the pod spec and asks the local runtime to start it.
7. The kubelet monitors the pod status and reports status changes to the API server

## Pod lifecycle
Pods are immutable and mortal whereas VMs are immortal and mutable

- pending:  when the scheduler finds a node to run it.
- running: Keeps running if it's a long-lived pod like a web server (always restartpolicy)
- succeeded: if it's a short-lived pod and the containers complete their tasks(onFailure/never restartpolicy)

## Deploy by static pods vs controllers
- pod manifest (rare because limited to this node)
- workload resources and controller(common because can restart, rolling updates, etc)

## The pod network
Every cluster runs a pod network that connects all pods to it.
Implemented by a third party plugin that interfaces k8s' Container Network Interface.
Nodes do not connect directly to the pod network.

## Multi-container Pods
Every container should have a single responsibility.

- Init containers: start and complete before the main app starts.
- Sidecar containers: add functionality to an application without adding directly to the application container.


