# Services

## Overview

Services are objects in Kubernetes which enable communication inside and outside of your application.



## Types of Services

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*tnK94zrEwyNe1hL-PhJXOA.png)

1. __ClusterIP (default):__ This type exposes the serviceon an internal IP inside your cluster, making the service only reachable from within that cluster. Useful for interal apps.

2. __NodePort:__ This type exposes the service on the same port of each selected node in your cluster using NAT. It makes your service accessible from outside of the cluster by adding a layer of abstraction on top of ClusterIP services.

3. __LoadBalancer:__ This type exposes your service externally using a cloud provider's load balancer. It automatically creates a NodePort service and a ClusterIP service, making the service accessible from the internet.

4. __ExternalName:__ This type maps the service to an external DNS name and does not proxy or forward to the pods. It's a special case and less frequently used.

