# Helm Introduction

## Overview

Helm changes the paradigm by which we can deploy our applications inside Kubernetes...

Helm is sometimes referred to as a "Kubernetes package manager".

Helm helps you managed K8s apps through 'Helm Charts', which are packages of pre-config K8s resources.

These charts are analogous to packages in package management systems (apt or yum).

Instead of having to declare numerous yaml configuration files, there is a single location with Helm in which you may declare every custom setting called a 'values.yaml' file.

## Useful Commands

Using wordpress for an example...

`helm install wordpress`

`helm upgrade wordpress`

`helm rollback wordpress`

`helm uninstall wordpress`