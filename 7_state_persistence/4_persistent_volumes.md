# Persistent Volumes

## Overview

Persistent Volumes (PVs) are a cluster-level resource in K8s. They represent a piece of storage that has been provisioned by an administrator or dynamically provisioned using storage classes. 

PVs are designed to outlive any individual pod that uses them, providing a durable and consistent storage solution across multiple pods and even regardless of pod restarts and rescheduling.

Once you have deployed your PVs, you can then configure your pods to be able to access them by using Persistent Volume Claims (PVCs).

## Configuration Files

### Spec: 

- __Capacity:__ Specifies the size of the volume.
- __Access Modes:__ Defines how the volume can be accessed. Common modes include:
    - __ReadWriteOnce (RWO):__ the volume can be mounted as read-write by a single node.
    - __ReadOnlyMany (ROX):__ the volume can be mounted as read-only by many nodes.
    - __ReadWriteMany (RWX):__ the volume can be mounted as read-write by many nodes.
- __PersistentVolumeReclaimPolicy:__ Determines what happens to the volume after the assocaited PVC is deleted. Options include: __Retain__, __Delete__, and __Recycle__ (deprecated).
- __StorageClassName:__ (optional) Links to the PV to a storage class.
- __VolumeMode:__ (optional) Defines if the volume is presented as a Block or Filesystem type.
- __Storage:__ Details about the physical storage media to use:
    - __Type:__ Could be NFS, iSCSI, local, cloud provided, etc.
    - __Configuration details:__ These differ based on the storage type. For example, NFS would require server and path details.

Example:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
  labels:
    type: local
spec:
  capacity:
    storage: 10Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  nfs:
    path: /path/to/directory
    server: nfs-server.example.com

```

## Useful Resources

https://kubernetes.io/docs/concepts/storage/persistent-volumes/