# Services

Service objects sit in front of pods that match labels and expose them via a **reliable** DNS name, IP address and port.

## EndpointSlice

When a Service is created, the EndpointSlice controller creates an EndpointSlice to track healthy Pods that match labels.

## Service Types

### ClusterIP
Provides an endpoint(name, IP, port) that's only accessible from the internal network.

### NodePort
Adds a port on every cluster node that external clients can use, and forward to ClusterIP.

Limitations
- high-numbered ports
- clients need to know the IP/names/health of the nodes

### Load balancer
Puts a cloud load balancer in front of Nodeport
Built on top of Nodeport
