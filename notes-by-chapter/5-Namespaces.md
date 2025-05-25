# Virtual clusters with Namespaces

K8s namespaces partition Kubernetes clusters into virtual clusters called namespaces.

A way for multiple tenants to share a cluster

- Soft isolation(separate tenants to strongly isolate)
- Nodes/PersistentVolumes are cluster-scoped

## Pre-created Namespaces

- default: where objects go if you don't specifiy namespaces
- kube-system: control plane components
- kube-public: objects readable by anyone
- kube-node-lease: node heartbeats/leases



