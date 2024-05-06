# API Groups

## Overview

What is the Kubernetes API?

In discussing the Kubernetes API Server we understand already that whatever operations we perform on the cluster is through interactions involving the API server either through the kubectl utility or directly while at rest.

Say we want to check the version, we can access the api server at the master nodes address followed by the port (6443 by default) and the API "version", like so: `curl https://kube-master:6443/version`... This will return information about the version.

Similarly, to get a list of pods, you would use `curl https://kube-master:6443/api/v1/pods`.

The focus here will be on these API paths. The /version and /api.

The K8s API is grouped into multiple such groups by their purpose, such as /api, /apis, /version, /healthz, /metrics, and /logs.

The /version API is used for viewing the version of the cluster, as used demonstrated above.

The /metrics and /healthz APIs are used to monitor the health of the cluster.

The /logs are for integrating with third party logging applications.

__For right now we will only focus on the APIs responsible for the functionality of the clusters.__

__These APIs are categorized into two, the (core group) /api and the (named group) /apis.__

The Core Group is where all the core functionality exists, such as namespaces, pods, replication controllers, events, endpoints, nodes, bindings, PVs, PVCs, secrets, etc etc.

The Named Group are more organized and where all of the newer features are. In it are the 'API Groups', such as /apps, /extensions, /networking.k8s.io, /storage.k8s.io, /authentication.k8s.io, /certificates.k8s.io, etc etc.

Within these API Groups, you have resources...

For example, within the API Group '/apps', you have its resources of deployments, replicasets, statefulsets.

Within /networking.k8s.io, you have /networkingpolicies.

Within /certificates.k8s.io, you have /certificatessigningrequests.

Each resource within an API Groups has a set of actions associated with them, known as 'verbs'...

Example, inside of the deployments resource inside the /apps API group you have 'verbs' like list, get, create, delete, update, etc.

## How to view these on the cluster

Like shown earlier, you can view these things via accessing the kube-api-server at port 6443 without any path and it will list you the available api groups, you could use:

`curl http://localhost:6443 -k`

If you wanted to look a level deeper to for example see a list of resource groups within /apis, you could use:

`curl http://localhost:6443/apis -k | grep "name"`

Do note, that to use commands like above you are going to need to authenticate first to see the full listed response. Or alternatively, you could start a kubectl proxy client to launch a proxy service locally on port 8001 and uses the credentials and certificates from your kubeconfig file to access the cluster.

Keep in mind, the kubectl proxy and the kube proxy are not the same thing.