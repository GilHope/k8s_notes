# Volumes In Kubernetes

## Overview

Docker containers are meant to be transient in nature, designed to be ephemeral. So they only last for a short period of time, are called upon to process data, and then are destroyed. In order to persist data across your containers, we can attach volumes. Now, even if the container is deleted, the volume and its stored data will remain.

Just like in Docker, Kubernetes pods are also transient in nature. Pods are created to process data and then deleted. And likewise with Docker, we may attach volumes in order to persist the data.

In K8s, doing so at the pod level allows multiple containers within the same pod to share the same set of volumes and thusly allowed more straightforward methods of data sharing and communication among those containers.

In K8s, these volumes can be local to a node or a remote resource. 

As Docker volumes are managed at the container level and exist independantly of the containers themselves, they are able to persist. In K8s however, as volumes are integrated at the pod level, allowing multiple containers within a pod to share the same storage, K8s introduces a bit more flexible complexity.

Kubernetes offers Persistent and Ephemeral Volume types.

- __Emphemeral Volumes:__ exist as long as the pod that created them continues to exist and are used for temporary storage needs, such as `emptyDir`.
- __Persistent Volumes (PVs) and Persistent Volume Claims (PVCs):__ PVs are a resource in a cluster that outlive the individual pods. Their lifecycle is managed completely independently from the pods and can be shared across many pods and survive restarts/failures. PVCs are what the pods use to access the PVs external to themselves.

## Types of Volumes in Kubernetes

Kubernetes supports many types of volumes and a pod can utilize any number of volume types simultaneously. 

Here are few popular options:

1. __emptyDir:__ A simple empty directory used for storing transient data that persists only as long as the pod is running. Useful for temporary storage that provides a common volume for all containers in a pod.
2. __hostPath:__ Similar to Docker's bind mounts, 'hostPath' mounts a file or directory from the host node's filesystem into your pod. This is often used for system services that need to access specific files or directories on the host.
3. __NFS:__ Mounts an Network File System share into your pod, allowing multiple pods to read and write to the same filesystem simultaneously.
4. __persistentVolumeClaim (PVC):__ Unlike 'emptyDir' and 'hostPath', which are typically tied to the lifecycle of a pod. PVC is a request for storage by a user. PVCs use a Persisten Volume (PV) that the cluster admin has provisioned beforehand. This abstraction separates the specific details of how storage is provided from how is is consumed.
5. __Cloud based solutions:__ Such as, AWS Elastic Block Storage, Azure Disk Storage, Azure File Storage, Google Persistent Disk, and so on.

## Useful Resources

https://kubernetes.io/docs/concepts/storage/volumes/