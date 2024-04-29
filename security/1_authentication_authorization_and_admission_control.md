# Authentication, Authorization, and Admission Control

## Overview

Authentication, authorization, and admission control, collectively known as __Security Primatives__, are the fundamental building blocks of security inside Kubernetes.

They collectively include things like: Role-Based Access Control (RBAC), Network Policies, Pod Security Policies, Secrets Management, Service Accounts, Node Security, Mutual TLS (mTLS), and Admission Controllers.

Each of these play a vital role towards creating a robust security posture for K8s environments, helping to manage access, protect data, enfore policies, and secure communications within your clusters.

---------------------------------------------------------

Let's begin with the host the form the cluster itself...

Of course, all access to the host must be secured with password based authentication, root access disabled, and only SSH key based authentication to be made available...

As we know by now, the 'kube-apiserver' is the center of all operations within kubernetes and we are able to intereact with it through utilities like 'kubectl' or by accessing the API directly. Through those methods, you can interact with almost any control operation on your cluster...

This is the first line of defense... Controlling access to the API-server itself...

In order to secure this first line, we must ask two questions: Who can access the server? And what can they do?

Who can access the API server is defined through authentication mechanisms... And there are different ways you can authenticate... User IDs and passwords stored in static files, tokens, certificates, or through external authentication providers (like LDAP)...

For machines to access the API-server, we can create service accounts.

Once they gain access to the cluster... What they can do is defined by authorization mechanisms...

Authorization mechanisms is implemented using Role Based Access Control (RBAC), where user are associated to groups with specific permissions...

There are also other authorization modules like Attribute Based Access Control (ABAC), Node Authorizers, webhooks, and etc...

All communication with the cluster, between the various components, such as ETCD, kube-controller, manager, scheduler, API server, and as well as those running on the worker nodes, such as the kubelet and kubeproxy is secured using TLS Encryption...

What about communications between applications within the cluster?

As by default, all pods can access all other pods within the cluster, you would need to restrict access between them using Network Policies.