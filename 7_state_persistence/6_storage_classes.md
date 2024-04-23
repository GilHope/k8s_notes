# Storage Classes

## Overview

In Kubernetes, Storage Classes are a way to describe different types of storage that can be allocated to containers in a K8s cluster. They provide a mechanism to handle the storage requirements of pods and other data persisting operations with K8s.

Storage classes enable administrators to define different types of storage based on performance, capabilities, and costs. For example, you might have one storage class for fast SSD-based storage and another for cheaper, slower HDD-based storage.

Storage classes are particularly useful in multi-tenant environments where different applications or users require different types of storage. They also enable the use of advanced storage like snapshots, cloning, and resizing depending on the capabilities of the underlying storage system.

__Dynamic Provisioning:__ Storage classes are used in conjunction with dynamic volume provisioning. This allows storage volumes to be created on-demand, based on the storage class specified by a Persistent Volume Claim (PVC). This is particularly useful in cloud environments where storage can be dynamically provisioned without having to manually pre-provision storage.

Each storage class contains the attribute fields: provisioner, parameters, and reclaimPolicy. These are used when a Persistent Volume belonging to the class needs to be dynamically provisioned to satisfy a PVC.

1. __Provisioner:__ Each storage class specifies a provisioner that determines what volume plugin is used for provisioning the storage. For example, kubernetes.io/aws-ebs for AWS Elastic Block Store, kubernetes.io/gce-pd for Google Compute Engine Persistent Disk, etc.

2. __Parameters:__ Storage classes allow specifiying parameters that are passed to the volume provisioner, which can include details like performance tiers, replication data, or region.

3. __Reclaim Policy:__ Storage classes allow specifiying parameters that are passed to the volume provisioner, which can include details like performance tiers, replication data, or region.

Kubernetes allows defining __Default Storage Classes__. Any PVC created without a specific storage class will use the default. Only one storage class can be marked as default.

## Useful Resources

https://kubernetes.io/docs/concepts/storage/storage-classes/