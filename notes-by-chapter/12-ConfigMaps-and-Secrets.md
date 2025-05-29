# ConfigMaps and Secrets

## The Pattern
Packaging apps and configs as a single unit is anti pattern.
We should decouple apps from configurations:
- Reuse
- Simpler changes.

## ConfigMap(CM)
store configuration data outside of Pods, and inject at runtime.
- non-sensitive
- key-value pairs 

methods to inject:
- env vars in Pod YAML: becomes standard Linux env vars
- arguments to the containers' startup command
- files in volume: can push updates to live containers

### Kubernetes-native apps 
Kubernetes-native apps can talk to API server and can read CM directly, but create Kubernetes lock-in where apps only work on k8s

## Secrets
- works the same as ConfigMaps
- part of secrets management solution for storing sensitive data 
- not encrypted in cluster store or in network
