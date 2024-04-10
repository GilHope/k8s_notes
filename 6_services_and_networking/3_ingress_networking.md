# Ingress Networking

## Overview

In Kubernetes, an Ingress is an API object that can manage external access to services within your cluster.

__You would use Ingress when you need to make your web services available to external users, for example, exposing a website or API to the public.__

Ingress can provide external access, load balancing, HTTP/HTTPS routing, SSL/TLS termination, and name-based virtual hosting.

To utilize Ingress in K8s, you will need an Ingress Controller.Ingress Controllers are the components responsible for fulfilling the Ingress, usually with a load balancer. 

K8s supports several Ingress Controllers, such as NGINX, Traefik, and HAProxy.

An Ingress resource is defined in YAML just like other K8s objects.

## Example Config

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80

```

## Useful Commands

Now, in k8s version 1.20+ we can create an Ingress resource from the imperative way like this:-

Format - `kubectl create ingress <ingress-name> --rule="host/path=service:port"`

Example - `kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"`