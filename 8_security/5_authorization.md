# Authorization

## Why?

Once authentitation is approved what now determines what you or another user can do?

As an admin you can do all sorts of operations on your cluster but what about if you need to bring other users to run certain operations for you?

Other admins, developers, testers, other applications for things like monitoring like Prometheus or continuous delivery like Jenkins?

But each of these types of users should really only need to perform certain tasks... 

For example, you wouldn't need nor want developers being able to add or delete clusters or networking configurations... They would only need be able to view them and not to modify. But they should be allowed to deploy applications.

Best practice is to allows allow only the minimal level of access to perform all their necessary operations and thats it.

# Overview

There are several types of authorization mechanisms inside Kubernetes, such as node authorization, attribute-based authorization (ABAC), role-based authorization (RBAC), and webhooks.

__Node Authorization:__ As the Kube API Server and Kubelets are what is accessed by users for management purposes within the cluster, the Kubelet accesses the Kube API Server to read information about services, endpoints, nodes, and pods, and as well report information about their statuses and events.

These requests are handled by the Node Authorizer.

__Attribute-Based Access Controls (ABAC):__ For external access to the API, ABAC is where you associate a user or group with a set of permissions. You do this by creating a policy file with a set of policies defined in JSON format. This file is then passed to the API server. Within this document you must define the users and their levels of access to the policy. To make any changes or updates to these policies it must be done manually within this file.

__Role-Based Access Controls (RBAC):__ Like ABAC, but make it much easier to manage. Instead of defining users/groups with a defined set of permissions, you instead define a role with a set of permissions and associate the necessary users to that role. So now whenever any updates or changes need to be made to the policy the users of the role will automatically be redefined in their access level instead of having to be manually reconfigured individually like in ABAC.

__Webhooks:__ Say you want to outsource your authorization mechanisms externally and not through the built-in K8s native mechanisms above, there are third-party tools like Open Policy that assists with admission control and authorization.

In this example, K8s will make an API call to the Open Policy agent with the info about the user and his access requirements and have the Open Policy agent decide whether the user should be permitted or not.

__AlwaysAllow and AlwaysDeny:__ Beyond the aforementioned, these are modes which are set on the Kube API Server. If you do not specify this option then it is set to __Always Allow by default__.