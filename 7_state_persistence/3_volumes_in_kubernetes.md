# Volumes In Kubernetes

## Oveview

Unlike Docker volumess which are managed at the container level, Kubernetes volumes integrates volumes at the pod level, allowing multiple containers within the same pod to share data and storage resources.

In K8s, these volumes can be local to a node or a remote resource. Unlike Dokcer volumes, K8s volumes have a defined lifecycle that is tied to the lifecycle of the pod. Depending on the type of volume used, when a pod is deleted then typically so is the volume.

## Types of Volumes in Kubernetes

1. __emptyDir:__ A simple empty directory used for storing transient data that persists only as long as the pod is running. Useful for temporary storage that provides a common volume for all containers in a pod.
2. __hostPath:__ Similar to Docker's bind mounts, 'hostPath' mounts a file or directory from the host node's filesystem into your pod. This is often used for system services that need to access specific files or directories on the host.
3. __NFS:__ Mounts an Network File System share into your pod, allowing multiple pods to read and write to the same filesystem simultaneously.
4. __persistentVolumeClaim (PVC):__ Unlike 'emptyDir' and 'hostPath', which are typically tied to the lifecycle of a pod. PVC is a request for storage by a user. PVCs use a Persisten Volume (PV) that the cluster admin has provisioned beforehand. This abstraction separates the specific details of how storage is provided from how is is consumed.