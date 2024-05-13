# KubeConfig

## Overview

KubeConfig is a file used to configure access to Kuberenetes clusters.

It containts information such as cluster details, authentication mechanisms, and other settings necessary to communicate with a Kubernetes cluster.

It includes:
- cluster information, such as the address of the API server and the cluster's certificate authority (CA) certificate.
- user information, such as detials about the user accessing the cluster, such as their username, client certificate, and client key.
- context information, such as contexts. contexts in a kubeconfig file refer to a combination of a cluster, a user, and a nampespace. It specifies the cluster to interact with, the user to authenticate as, and the default namespace to use.

## How Does It Work?

So far we have already discussed how to generate a certificate for a user elsewhere...

__This is the tedious way to do it__

Once the user has their certificate file and private key pair they can use it alongside their query to the Kubernetes REST API using the 'curl' command (used for making HTTP requests) to view, for example, a list of pods within the cluster...

Say the cluster is named 'my-kube-playground', that query would look something like this:
```
curl https://my-kube-playground:6443/api/v1/pods \
--key admin.key
--cert admin.crt
--cacert ca.crt
```

This is then validated by the K8s API Server to authenticate the user so they could then view the list of pods...

Now, you could also do the same thing using the 'kubectl' command, like this:
```
kubectl get pods
    --server my-kube-playground:6443
    --client-key admin.key
    --client-certificate admin.crt
    --certificate-authority ca.crt
```

You specify the same info using the options: server, client-key, client-certificate, adn certificate-authority.

__As this is tedious, this is where the KubeConfig file comes in handy!__

How KubeConfig works is you simply move all of these require informations into its own configuration file, the kubeconfig file...

You then specify this file as 'kubeconfig'...

Now, all you have to do is use it in your 'kubectl' commands along with the 'kubeconfig' option, like so:
```
kubectl get pods --kubeconfig=config
```

By default, 'kubectl' knows to expect 'kube-config' files named as 'config'... so really unless you decide to give your file its own custom name then you do not even need include the kube-config option and can simply enter in this case:
```
kubectl get pods
```

## KubeConfig Format

__The config file consists of 3 sections: Clusters, Users, and Contexts.__

The cluster sections are for the various K8s clusters of which you need access to... For example, you might have multiple clusters for development, testing, and/or production environments.

The user section is for the user accounts which will have access to your clusters... For example, an admin user, dev user, prod user, etc... These users may have different privledges on different clusters.

The context section is how the previous are married together. This section determines which user accounts will be used to access which cluster... For example, you might create a context named 'admin@production' that will use admin account to access a production cluster.

Here is an example of what a kubeconfig file might actually look like:
```
apiVersion: v1
kind: Config

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority:
    server: https://my-kube-playground:6443

contexts:
- name: my-kube-admin@my-kube-playground
  context:
    cluster: my-kube-playground
    user: my-kube-admin

users:
- name: my-kube-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

## Useful Commands

To list the clusters, contexts, and users, and as well the current-contet set, use:
`kubectl config view`

Alternatively, if you want to list a specific kubeconfig file then you can pass the kubeconfig option, like so:
`kubectl config view --kubeconfig=/path/to/my-custom-config`

To change the default context to use a different user to access a cluster, use:
`kubectl config use-context <different-user>@<cluster-name>`

To do the same for a custom context, try:
`kubectl config use-context --kubeconfig=/path/to/my-custom-config`

To view you default current context, use:
`kubectl config current-context`

For a specific non-default current context, use:
`kubectl config current-context --kubeconfig=/path/to/my-custom-config`

To set a specific kubeconfig file to become the new default, use:
`cp my-custom-config ~/.kube/config`

There are many other commands you can execute, view the list with:
`kubectl config -h`