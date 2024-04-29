# Authentication

## Overview

As we already know, the Kubernetes cluster consists of multiple nodes physical/virtual and the other various components that work together...

You may have users, like administrators, that may need to access the cluster to perform admin tasks... The developers that access the cluster to test or deploy applications... And end users who access the applications deployed on the cluster... And third part applications that may access the cluster for integrations purposes...

The focus of this overview will be to discuss how to secure the management access to the cluster by way of authentication mechanisms... So, the focus will be here on the users who access your clusters for administrative purposes, such as admins/developers and the processes/services or applications...

K8s does not manage user accounts natively. It relies on external sources like from a file containing user accounts or certificates or from third party identity services like LDAP to manage users...

So, you cant create or list users inside of kubernetes itself... 

However, in the case of Service Accounts, Kubernetes can manage them. You can create and manage service accounts using the Kubernetes API...

Right now, lets focus on users in Kubernetes...

All user access is managed by the API server... Whether you are accessing the cluster through kubectl tool or the API directly... All of these requests go through the api server... And the api server has to authenticate requests before it can process any of those requests...

How does the api server authenticate?

There are multiple methods it can utilize to do this...

You can have a list of username and passwords in a static password file... You can have usernames and tokens inside a static token file... Or, you can authenticate using certificates... And you can also utilize third party auth protocols like LDAP or Kerberos...

__Static Password and Token Files__

These are the simplest forms of authentication... __But is not the recommended approach as it is insecure...__

You can create a list of users and their passwords in 'csv file' and use that as the source for user info...

This file will be comprised of three columns consisting of "password, username, ID, and (optional) group", ex - `password123,user1,u0001,group1`...

You would then pass the file name as an option to the kube-apiserver static pod in the 'kube-apiserver.service' and then restart the kube-api server for the effects to take place...

As for token files, the process is very much the same with some minor adjustments.

## Example Static Password Config Process

1. Create a file with user details locally at /tmp/users/user-details.csv.

```
# User File Contents
password123,user1,u0001
password123,user2,u0002
password123,user3,u0003
password123,user4,u0004
password123,user5,u0005
```

2. Edit the kube-apiserver static pod configured by kubeadm to pass in the user details. The file is located at `/etc/kubernetes/manifests/kube-apiserver.yaml`.

```
apiVersion: v1
kind: Pod
metadata:
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
      <content-hidden>
    image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
    name: kube-apiserver
    volumeMounts:
    - mountPath: /tmp/users
      name: usr-details
      readOnly: true
  volumes:
  - hostPath:
      path: /tmp/users
      type: DirectoryOrCreate
    name: usr-details
```

