# Persistent Volume Claims (PVCs)

## Overview

Persistent Volume Claims, or PVCs, are a what provides a way for pods to request and access Persistent Volume (PVs) storage.

A PVC is essentially a request for storage by a pod which specifies size, access modes, and other possible parameters.

## How PVCs Work

1. __Creation:__ Users define PVCs in their deployments in YAML format and specify their requirements.
2. __Matching:__ Once created, the K8s master looks for available PVs that match the criteria in the PVC.
3. __Binding:__ When a suitable PV is found, the PVC is binded to it. This binding is exclusive, meaning that each PV can only be bound to one PVC at a time in order to ensure that storage resources cannot be accidently overwritten or accessed by unauthorized pods.
4. __Usage:__ After binding, the now mounted PV can be read and wrote to according to whatever permissions and capacities were defined.
5. __Release:__ When a pod or PVC is deleted, the PV becomes unbound but typically remains within the cluster. What happens next depends on the PV's reclaim policy - it could be retained, recycled, or deleted.

## PVC Properties

__Access Modes:__ Defines how the PVC can be mounted on a host.
    - __ReadWriteOnce:__ - The volume can be mounted as read-write by a single node.
    - __ReadOnlyMany:__ - The volume can be mounted read-only by many nodes.
    - __ReadWriteMany:__ - The volume can be mounted as read-write by many nodes.
__Storage Requests:__ The amount of storage requested. K8s ensures that the PV matched and bound to the PVc meets or exceeds this requirement.
__Volume Mode:__ Can be either 'Filesystem' (where the volume is mounted into a pod as a directory) or 'Block" (where the volume is attached to a pod as a block device, without any filesystem).
__Selector:__ A PVC can define a 'selector' to further specify which PV is should bind to, based on the labels set on the PV.

## Example Config File

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard

```

This file will create a PVC named 'mypvc' that requests 1 GB of storage, with the access mode set to 'ReadWriteOnce', meaning it can be mounted as read-write by a single node.

How to reference PVCs within your pod definitions:

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    volumeMounts:
    - mountPath: "/var/www/html"
      name: myvolume
  volumes:
  - name: myvolume
    persistentVolumeClaim:
      claimName: mypvc

```

In this example, in the 'volumeMounts' section in the container specifies where the volume should be mounted inside the container and in this case '/var/www/html'.

The 'volumes' section names the volume and specifies that is should use a PVC, then states which PVC to attach by 'claimName'. The name of the volume ('myvolume') is linked with the 'volumeMounts' by the name attribute.
