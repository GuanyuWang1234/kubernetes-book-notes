# Storage

## Storage providers
supply their CSI plugins via Helm chart/YAML installer
CSI plugins run as a set of pods in kube-system

## The Container Storage Interface(CSI)
interface where container orchestrators leverage external storage

## The Persistent Volume Subsystem

### PersistentVolumes(PV)
map to external volumes

### PersistentVolumesClaims(PVC)
grant access for Pods to use PV

### StorageClasses(SC)
templates for provisioning storage
- allows dynamic provisioning

## Workflow
1. Pod requests volume via PVC
2. PVC asks SC to create PV
3. SC calls provisioner via CSI
4. CSI creates volume on provisioner
5. SC creates PV and maps it to volume
6. The Pod mounts the PV

## Volume settings
SC controlls how volumes are managed

### Access Mode
- ReadWriteOnce 
- ReadWriteMany: support File/Object, not block
- ReadOnlyMany

### Reclaim policy
what to do with PV/external storage when PVC is released

- Delete(default): deletes the PV and external storage 
- Retain: keep them

### VolumnBindingMode 
- Immediate: create PV as soon as PVC references SC
- WaitForFirstConsumer: wait to create PV/Volume until deployed a Pod tht used them

