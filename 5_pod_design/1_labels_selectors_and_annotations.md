# Labels, Selectors, and Annotations

## Overview

Labels and selectors are the standard method to group things together.

Labels are key-value pairs attached to objects. 

Selectors are used to filter objects based on their labels.

Annotations are another form of metadata, similar to labels, but instead are used to hold non-identifying data, such as build info, pointers to logging or monitoring systems, or any other info that might be useful.

## Example

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
  labels:
    app: web
    env: production
  annotations:
    description: "The web application deployment in the production environment."
    version: "v1.0"
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
      env: production
  template:
    metadata:
      labels:
        app: web
        env: production
    annotations:
      buildVersion: "1.0.0"
      maintainer: "web-team@example.com"
    spec:
      containers:
      - name: web-app-container
        image: web-app:1.0
        ports:
        - containerPort: 80
```

Breakdown of the Example

1. Labels: We use labels in three places:
- On the Deployment itself (app: web, env: production) to organize our deployments.
- In the selector.matchLabels to specify which pods this deployment should manage.
- In the pod template (template.metadata.labels) to ensure that the created pods match the selector criteria of the Deployment.

 2. Selector: The selector field in the deployment spec uses matchLabels to filter pods by the labels app: web and env: production. This ensures the deployment manages only the pods intended for the web application in the production environment.

3. Annotations:
- The Deployment metadata includes annotations description and version to provide additional context about the deployment. This could be useful for other developers or automation tools interacting with this deployment.
- The pod template metadata includes annotations buildVersion and maintainer. These could be used by monitoring tools or when troubleshooting to quickly identify build versions and responsible teams.

## Useful Commands

If you would like to filter a particular object by its selectors you can use something like this example: `kubectl get pods --selector env=dev`

If you would like to simply get a count of how many objects are using a given selector then use something like this: `kubectl get pods --selector bu=finance --no-headers | wc -l`

If you would to filter by multiple selectors, try something like this: `kubectl get pods --selector env=prod,bu=finance,tier=frontend`