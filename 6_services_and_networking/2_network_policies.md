# Network Policies

## Overview

Network Policies apply to pods within a Kubernetes namespace and allow you to specify both ingress (incoming) and egress (outgoing) rules. These rules can specify which pods are allowed to communicate and on what ports, essentially determining the allowed traffic flow.

K8s __does not__ enforce any network policy by default. This means that unless specified in a policy document, pods can communicate freely with each other within the cluster.

## Components of Network Policies

- __Pod Selector:__ Specifies the group of pods to which the policy applies. It uses labels to select the pods.

- __Policy Types:__ Defines whether the policy is for ingress, egress, or both.

- __Ingress/Egress Rules:__ Define the rules for the selected policy type. Rules can specify:
    - __Ports:__ Which ports traffic can be received on or sent from.
    - __From/To:__ The sources and destinations of the traffic. This can be defined by pod selectors, namespace selectors, or IP blocks.