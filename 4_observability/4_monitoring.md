# Monitoring

## Overview

Kubernetes does not come with its own full featured monitoring solution. However, there are a number of open-source options to resolve this available, such as Prometheus, Elastic Stack, Datadog, and Dynatrace.

Heapster was one of the original monitoring tools designed for Kubernetes but is since deprected.

A slimmed down version of Heapster, known as Metrics Server, has replaced it.

You can have one Metric Server per K8s cluster.

The Metric Server retrieves metrics form eahc of the kubernetes nodes and pods and stores them in memory. 

Note that Metric Server is only an in-memory solution so these collected metrics are not stored on disk and as result you cannot see historical performance data. For that, you might try one of the more advanced monitoring solutions mentioned earlier above.

For Metrics Server this is made possible by the 'cAdvisor', apart of the kubelet, which exposes the kubelet API to be availble for the Metrics Server to access.

## Metric Server Usage

If using minikube, use the command: `minikube addons enable metrics-server`

Otherwise, you will need to clone the Metric Server deployment files from GitHub. Do this by running: `git clone https://github.com/kubernetes-incubator/metrics-serve` and followed by `kubectl create -f deploy/1.8+/` which will deploy everything necessary to support your MS monitoring.

## Useful 

`kubectl top node`

`kubectl top pod`