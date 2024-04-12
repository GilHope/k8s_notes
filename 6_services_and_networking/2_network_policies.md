# Network Policies

## Overview

Network Policies apply to pods within a Kubernetes namespace and allow you to specify both ingress (incoming) and egress (outgoing) rules. These rules can specify which pods are allowed to communicate and on what ports, essentially determining the allowed traffic flow.

K8s __does not__ enforce any network policy by default. This means that unless specified in a policy document, pods can communicate freely with each other within the cluster.

__Kubernetes BY DEFAULT is configured with an ALL-ALLOW rule within a cluster__

In order to make use of K8s network policies you must use a Container Network Interface (CNI) plugin which supports them. Not all do, such as Calico, Cilium, Weave Net, Kube-router, and Antrea. Flannel nor the Kubernetes default (kubenet) support network policies. Also, keep in mind that depending on which CNI you choose to use this can impact things such as network performance, scalability, and the complexity of your deployment. You can utilize multiple CNIs if necessary to your specific needs.

## Components of Network Policies

- __Pod Selector:__ Specifies the group of pods to which the policy applies. It uses labels to select the pods.

- __Policy Types:__ Defines whether the policy is for ingress, egress, or both.

- __Ingress/Egress Rules:__ Define the rules for the selected policy type. Rules can specify:
    - __Ports:__ Which ports traffic can be received on or sent from.
    - __From/To:__ The sources and destinations of the traffic. This can be defined by pod selectors, namespace selectors, or IP blocks.

## Configuration Files

In order to make a Network Policy config file, the following are required...

- Network Polcies utilize the `networking.k8s.io/v1` apiVersion.

- kind is set to `NetworkPolicy`

- Under the spec section you will need to set your `podSelector`. This selects the group of pods which the policy applies to. It can match all pods in a namespace or a specific subset, based on their labels.

- `policyTypes` will specify whether the policy is enforced on ingress or egress or both. You must include atleast one to define what the policy controls.

Here is a minimal example of what this might look like:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-inbound
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

So, this example would work like this:

- It applies to ALL pods in the namespace where it is applied.  (podSelector: {}).
- It applies to inbound traffic. (policyTypes: - Ingress).

- By NOT specifying any ingress rules, it has effectively DENIED ALL inbound traffic.

__Here is another example that is a bit more specific:__

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: block-specific-outbound
spec:
  podSelector:
    matchLabels:
      role: api
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 192.168.100.0/24

```

- Here it targets pods which have the label `role: api`. Only these pods will be affected by the policy.

- `policyTypes` is set to egress so it is only controlling outbound traffic from the selected pods.

- The first rule under `egress` specifies `to` block an `ipBlock` selector. By specifying the `cidr` of `0.0.0.0/0` with the `except` of `192.168.100.0/24` what it is doing is allowing traffic to ALL destinations EXCEPT for that specific range.

## Useful Commands

To view the network policies within a namespace use:
`kubectl get networkpolicy` or `kubectl get netpol`

