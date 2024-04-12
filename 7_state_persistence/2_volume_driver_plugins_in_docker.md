# Volume Driver Plugins In Docker

## Overview

Not to be confused with Storage Drivers, in Docker there are also Volume Drivers.

Volume Drivers are plugins that allow Docker to store volumes on remote hosts or within cloud services and thusly extend the capabilities of the basic local volume system.

These drivers provide additional options for how and where data is stored, managed, and accessed, enabling more complex and scalable storage solutions.

Particularly useful in multi-host Docker setups or in environments where data needs to be resilient, highly available, or integrated with external storage systems.

## How Volume Drivers Work

When you specify a volume in Docker, you can select which driver to use. This driver then manages the lifecycle of the volume, including its creation, deletion, mounting, unmounting, and potentially other operations like snapshotting or replication, depending on the driver's capabilities.

## Common Types of Volume Drivers

- __Local:__  The default volume driver which manages volumes on the local filesystem.
- __RexRay:__ A popular third-party driver that provides high availability and automation capabilities. It supports multiple cloud storage providers and platforms, including Amazon EBS, Google Compute Engine, and VMWare vSphere.
- __Convoy:__ An open-spurce Dokcer volume driver that can manage volumes across a cluster of hosts. It supports snapshot, backup, and recovery of Docker volumes and works with various backend like NFS, AWS EBS, DigitalOcean Block Sorage, and more.
- __Ceph RBD:__ Integrates Docker with the Ceph distributed storage system, allowing Docker containers to interface directly with Ceph for highly scalable and resilient storage.
- __NFS:__ Allows Dokcer containers to mount volumes stored on NFS servers, which is useful for shared storage in cluster environments.
- __GlusterFS:__ Another option for clustered environments, GLusterFS allows volumes to be distributed across multiple hosts in a trasnparent and scalable manner.
- __Portworx:__ Provides a cloud-native storage solution for Kubernetes and Docker, supporting features like HA, data protection, data sec, and multi-cloud migrations.
- __VMware vSphere Storage for Docker (vDVS):__ Enables Docker containers to interface with VMware's enterprise-class storage solutions like VMFS and vSAN. This driver is essential for enterprising using VMware infrastructure that want to integrate Docker seamlessly.
- __Flocker:__ Although no longer actively developed, Flocker wa sone fo the first to offer an application-centric approach to managing Dokcer volumes, allowing users to manage their Docker volumes along with the container, supporting data migration between servers as container are moved.