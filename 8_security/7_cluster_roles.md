# Cluster Roles

## Overview

Roles and Rolebindings are namespaced, meaning that they are created within namespaces. If you do not specify a namespace when creating then they are created in the default namespace.

Namespaces are useful for grouping and isolating resources like pods, deployments, and services, but what about other resources, such as nodes? You can't do so with resources like nodes. This is because they are cluster wide resources and therefore cannot be associated to any particular namespace.

So, resources can be categorized as either namespaced or cluster scoped.

__Namespaced Resources__ encompass things such as, pods, replica sets, jobs, deployments, services, secrets, roles, rolebindings, PVC, configmaps, and etc.

__Cluster Scoped Resources__ include nodes, PV, cluster roles, cluster role bindings, certificate signing requests, namespaces themselves, and etc.

If you want to view a comprehensive list of either, then run:
`kubectl api-resources --namespaced=true` or
`kubectl api-resources --namespaced=false`

So the question here now arises, how we authorize users to cluster wide resources?

This is where Cluster Roles and Cluster Role Bindings come into play.

These ar ejust like regular roles and role bindings except for the cluster scoped resources.

For example, a cluster role could be created to provide a cluster admin the permissions to view, create, or delete nodes in a cluster. This would allow the new admin permission to these resources across all namespaces.