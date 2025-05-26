# Deployment Hands on

## YMAL
- metadata.name: [Deployment name(must be valid DNS name)]
- spec.replicas: [# of pods]
- spec.selector: labels of pods to manage
- spec.revisionHistoryLimit: keep the previous # of ReplicaSet to roll back
- spec.minReadySeconds: throttles the rate of replacing replicas. Longer waits = time to catch problems
- spec.strategy: defines rolling update
- spec.template: Pod template

Applying YMAL overwrites the imperative changes, so it's best to make all changes via YMAL.

## Labels (of Pods)
- app: defined in YMAL
- pod-template-hash: All pods created by a Deployment(actually ReplicaSet) will be tagged this. 