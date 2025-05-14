## Pod manifest files

Four top-level fields:
- kind: type(Pod/Deployment)
- apiVersion 
- metaData: names, labels 
- spec: # of containers, image, port, resources 

## Inspecting
kubectl get pods -o yaml 

- spec: desired state
- status: observed state 

## Hostnames
metadata.name in YAMl is the hostname of every container in the pod 

## Resources
- requests: minimum values
- limits: maximum values 