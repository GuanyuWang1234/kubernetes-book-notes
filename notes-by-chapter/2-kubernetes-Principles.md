Kubernetes is both

- a cluster
- an orchestrator

## Cluster

A Kubernetes Cluster is one or more nodes, providing CPU, memory, and other resources for applications

# Control plane nodes
- Implements the Kubernetes Intelligence. 
- Must be Linux
- Should have 3/5 for HA
- Don't run apps here
- Includes the API server, the scheduler, and the controllers

## The API server
The front end of Kubernetes. All commands go through it

## The Cluster store
- Holds the desired state of all applications
- Based on etcd(need odd number of replicas to avoid split brain)

## Controllers and the controller manager
- Implement cluster intelligence
- Reconciling observed state with desired state
- Controller manager spawns and manages individual controllers

## The Scheduler
Watches the API server for new work tasks And assign them to the best worker nodes

## Cloud Controller manager
Integrates the cluster with cloud services

+----------------------------------------------------------+
|                                                          |
|    [ctl]                      [{API}]                    |
|     ctl                        api    API server         |
|                                  ▲                       |
|                                 ▲|▲                      |
|                                / | \                     |
|    [sched]            ◄-------  |  ------►    [ctrl]    |
|     sched  Scheduler            |           ctrl Controllers
|                                 |                        |
|                                 ▼                        |
|                                                          |
|                              [etcd]                      |
|                               etcd   Cluster             |
|                                      store               |
|                                                          |
+----------------------------------------------------------+
|    [tux]                      Linux                      |
+----------------------------------------------------------+
|    [cloud]                    VM Fe                      |
+----------------------------------------------------------+


# Worker nodes
- Where you run your business applications.
- Can be Linux or Windows
- Contains kubelet, runtime and network proxy

## kubelet
- Watches API server for new tasks
- Instructs the appropriate runtime to execute tasks
- Reports task status to the API server

## Runtime
Execute tasks: 
- Pulls container images
- Starts and stops containers

Usually use the Pre-installed Containerd run time

## Kube-proxy
Implements cluster Networking and load balances traffic to tasks running on the node.

# Packaging apps
We wrap containers in pod so kubernetes can run it and do the complex logistics
Pods also get wrapped in high-level controllers for advanced features

```
+-------------------------------------------------------------+
|  [↻]                                                         |
|  dpty  Adds scaling, self-healing, rollouts...              |
|                                                             |
|       +---------------------------------------------------+ |
|       |  [□]                                              | |
|       |  pod  Enable running on Kubernetes                | |
|       |                                                   | |
|       |        +------------------------------------+     | |
|       |        |  ||||||||                          |     | |
|       |        |         App and                    |     | |
|       |        |         dependencies               |     | |
|       |        |                                    |     | |
|       |        +------------------------------------+     | |
|       |                                                   | |
|       +---------------------------------------------------+ |
|                                                             |
+-------------------------------------------------------------+
```

# Declarative model and desired state
Describe the desired state in YAML, post it to the API server, and the controller will make necessary changes to reconcile the differences

# Pods 
The atomic unit of scheduling. 
Can have multiple containers(for tightly cupboard components).

## Pod anatomy
Each pod is a shared execution environment(network stack volumes, memory) for one or more containers.
```
+--------------------------------------------------------------+
|  [□]                                                          |
|  pod                                                          |
|                                                               |
|       +-------------+                                         |
|       |  Localhost  |                                         |
|       +------+------+                                         |
|              |                                                |
|     +--------+--------+                                       |
|     |                 |                                       |
|  +--+-----+       +---+----+                                  |
|  |||||||||       |||||||||                                    |
|  Main           Sidecar                                       |
|  container       container                                    |
|     |                |                                        |
|  +--+--+          +--+--+                                     |
|  |8080 |          |5005 |                                     |
|  +-----+          +-----+                                     |
|       \              /                                        |
|        \            /                                         |
|         +----------+                                          |
|         |10.0.10.15|                                          |
|         +----------+                                          |
|               |                                               |
+---------------+-----------------------------------------------+
```
(Multi contained Pod sharing Pod IP)

## Pod scheduling
- kubernetes schedules pods, not containers
- A pod is only ready when all its containers are running

## Scaling
Scale up by adding pods instead of adding containers; Scale down by deleting pods

## Lifecycle
Pods dies and gets replaced with a new one that has a new ID and a new IP.
Applications should be immune to individual pod failure

## Immutability
Pods cannot be changed once they're running
Updating a pod is the same as replacing it with a new one

## Deployment
Pods are deployed with higher level controllers like deployment

# Services
Services provide reliable networking(reliable name and IP and load balance) for groups of pods

```
                  [□]           [□]                    [Device]
                  pod           pod                   /Laptop &
                   |             |                   /  Mobile
                   |             |                  /
                   v             v                 v
                 +-------------------------------+
                 |      [⚙]                     |
                 |      svc   Name: back-end    |
                 |            IP: 172.16.1.43   |
                 |            Port: 4434        |
                 +-------------------------------+
                  /|\ |           /|\
                 / | \|           / \
                /  |  \          /   \
               /   |   \        /     \
  ............/.....|....\....../.....\.........................
  :           v     v     v    v       v                       :
  :     +---------+ +---------+ +---------+                    :
  :     |  [□]    | |  [□]    | |  [□]    |                    :
  : [↻] | pod     | | pod     | | pod     |                    :
  : dpty||||||||||| ||||||||||| |||||||||||                    :
  :     +---------+ +---------+ +---------+                    :
  :                                                            :
  :............................................................:

```