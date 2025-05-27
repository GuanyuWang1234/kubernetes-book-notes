kubectl describe svc

## Endpoints
List of healthy Pods from the Service's EndpointSlice

## Session Affinity
session stickiness - whether client always connects to the same Pod

- None(default): forward to different Pods
- ClientIP: when app stores state, which is an anti-pattern

## External Traffic Policy
Whether traffic will be load balanced acroos Pods on all cluster nodes or just the Pods on the node the traffic arrives on.

- Cluster(default): obscures source IP
- Local: preserves source IP
