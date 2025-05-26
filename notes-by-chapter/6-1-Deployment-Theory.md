# Deployment Theory 

- Most popular way of running stateless apps on Kubernetes.
- Add self-healing, scaling, rollouts/rollbacks.


## Deployment and Pods

Every Deployment manages one more more identical Pods.
Pod is defined in a template in Deployment YAML

## ReplicaSet

User manages via Deployment. 
Deployment manages ReplicatSet.
ReplicaSet manages Pods.

┌─────────────────────────────────────┐
                  │        ⬡ Deployment                │
                  │         (dply)                     │
                  │    Rollouts and rollbacks          │
                  └─────────────┬───────────────────────┘
                                │
                  ┌─────────────▼───────────────────────┐
                  │        📚 ReplicaSet               │
                  │         (rs)                       │
                  │    Scaling, self-healing           │
                  └─────────────┬───────────────────────┘
                                │
                  ┌─────────────▼─────────────────────┐
                  │                                    │
                  │  ┌──────────────┬──────────────┐  │
                  │  │   📦 Pod     │   📦 Pod     │  │
                  │  │ Co-lo, shar..│ Co-lo, shar..│  │
                  │  │              │              │  │
                  │  │ ┌──────────┐ │ ┌──────────┐ │  │
                  │  │ │ 🐳       │ │ │ 🐳       │ │  │
                  │  │ │App&deps  │ │ │App&deps  │ │  │
                  │  │ └──────────┘ │ └──────────┘ │  │
                  │  └──────────────┴──────────────┘  │
                  └─────────────────────────────────────┘

## Autoscalers

### Horizontal Pod Autoscaler(HPA)
- Adds and removes **pods** to meet current demand.
- Automatically installed

### Cluster Autoscaler(CA)
- Adds and removes **nodes** to have enough to run all pods.
- Automatically installed

### Vertical Pod Autoscaler(VPA)
- Increases and decreases CPU/memory to run pods.
- Not auto installed

Multi-dimensional autoscaling: HPA and CA work together to add/remove nodes/pods. When one node can't fit all pods, another node is added dynamically. 

## Rolling Updates

Deployments make zero-downtime rolling updates: 
Keeps creating a new replica running the new version and terminate a replica running the old version

Works best when apps are 
- loosely coupled via APIs: update without having to worry about others
- Backwork/Forward compatible: perform independent updates without caring the version of clients

When updating YAMl to increase # of pods, Deployment controller will create a new ReplicaSet, and systematically increment # of pods in new ReplicaSet and decrement # of pods in old ReplicaSet.

## Rollbacks

Even though older ReplicaSets don't manage any pods, their configs still exist to roll back to eariler versions. Rollback process is the opposite of Rollout.


