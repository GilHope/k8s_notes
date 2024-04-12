# Storage In Docker

## File Structure Overview

When you install Docker on a system it creates a file structure which looks like this:

/var/lib/docker/
    aufs/
    containers/
    image/
    volumes/

This is where data will be stored in Docker by default.

This data will be files related to images and containers running on the Docker host.

To understand how Docker stores data, first you need to understand Docker's layered architecture...

## Layered Architecture

__Image Layers:__

Key points to understand about Docker's Layered approach:

- Image Layers
- Layer Caching
- Shared Layers
- Layer Isolation
- Snapshotting

When Docker builds images it does so in a layered architecture.

These layers are read-only and isolated from one another. They are essentially snapshots of the filesystem at a certain point in time which are created during this image building process.

Every time a Dockerfile instruction modifies the Docker image, a new layer is created. Only layers that have been changed need to be pushed or pulled, this is called 'layer caching' and it is a major benefit of Docker as it speeds up subsequent image builds which share common layers and it also therefore saves bandwidth.

For example, if multiple images are built with the same base image then only the differences between them need to be built and stored. 

Each layer is isolated from the others. This means that changes in one layer do not affect previous layers.

__Container Layer:__

When you start a container from an image, Dokcer uses the image layers as a read only base and then adds a writable layer on top.

So, your container layer is read/write.

This writable layer is specific to the container and is known as the contianer layer.

Unlike the image layers below it, the container layer is writable so this means any changes made during the runtime of the container, such as files modified by the application and new files created or deleted are written to this layer.

The lifecycle of a container layer are tied to the container itself. When the container is deleted, the container layer is also deleted, and the changes stored in this layer are lost unless explicitly saved into a new image layer.

Keep in mind that excessive writes to this container layer can hinder performance as each write operation can involve copying data from the underlying read-only image layers to the writable layer (known as copy-on write).

The container layer is primarily useful for temporary changes that only need persist as long as the container is running or experimentation and debugging operations where you might do so to make observations before committing a new image.

Because the container layer is ephemeral and coupled to the lifecycle of the container itself you should not consider it ideal for persisting data needs. For data that needs to be persistent Docker provides Volumes and Bind Mounts.
- __Volumes:__ are managed by Docker and are stored outside of the container's filesystem and are not removed when a container is deleted. Best when data needs be stored independent of the container lifecycle, managed securely, and portable across different environments.
- __Bind Mounts:__ involve mapping a host file or directory to a container, allowing the container to access and modify the host's filesystem directly. Best when needing immediate performance within the container and a native filesystem.

So, what is responsible for doing all of these operations? 
Maintaining the layered architecture, creating a writable layer, moving files across layers to enable copy and write, and etc?

__Storage Drivers:__

Storage Drivers are an integral component to containerization process. They allow Docker to manage the details of the file system, handle how images are built, how layers are merged, and how changes are written. Each type of storage driver will handle these performantly and efficiently slightly different, so you need to consider this depending on your workload and specifics of your system.

Common Docker Storage Drivers are:
- AUFS
- ZFS
- BTRFS
- Device Mapper
- Overlay
- Overlay2

Consider system compatibility when picking a driver as not all drivers are supported by all systems. For example, Overlay2 requires newer Linux kernels, while Devicemapper might prefer older systems than do not support Overlay2.

Some drivers, like Overlay2, generally perform better and have fewer overheads compared to others like Devicemapper or Btrfs, depending on the workload.

Some storage drivers, like Btrfs and ZFS, offer advanced features such as native snapshotting and file compression.

Keep in mind that Docker automatically will select a default storage driver for you based on your OS and its configuration and capabilities. Docker will aim to balance compatibility and performance in most common scenarios but if you determine the need of a more specifically suitable driver for your needs then that is always an option to manually configure.