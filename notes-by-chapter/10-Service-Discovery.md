# Service Discovery 

## High level
1. app-a asks Kubernetes for IP of app-b
2. Kubernetes returns IP 
3. app-a sends requests to app-b's IP

## The Service Registry

- is a built-in cluster DNS
- on control plane 
- has > 2 pods, deployment(coredns) and service(kube-dns).
- Maintains service names and their associated IP addresses

## Service Registration
Cluster DNS observes the new Service and registers the DNS records
- automatic

## Service Discovery
1. container sends to IP of cluster DNS configured in /etc/resolv.conf
2. DNS returns, and container sends to ClusterIP
3. ClusterIP is on service network(stable but virtual), so container sends to default gateway, which forwards to its node.
4. The node sends to its dafault gateway, and its kernel(kube-proxy) processes the request and redirects to the IP of the Pod.

## Namespaces
Fully qualified domain names(FQDN):
\[object name\]/.\[namespace\].svc.cluster.local

Object names must be unique within namespace but not across.