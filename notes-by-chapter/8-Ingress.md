# Ingress

## Purpose
LoadBalancer provides 1-to-1 mapping between cloud load balancer and Services
-> too many LBs

Ingress exposes multiple Services through 1 single cloud load balancer

## Ingress controller 
- Implements the rules(resources) to route traffic
- Not built-in 
- Operates at Application layer, so it can work with HTTP hostnames and paths

## Ingress resource/object
- spec.ingressClassName: do host-based and path-based rules
- Address: IP/DNS of the load balancer created by the Ingress. 
- Default backend: where the controller sends traffic if there is not a route.

## Ingress class
Allows running multiple Ingress controllers on a single cluster by organizing what controller handles which rules

## Configure DNS
Need to configure DNS to point hostnames to the Ingress load balander.