# Rolling Updates and Rollbacks

## Overview

A __Rollout__ is the process of deploying updates to a Deployment, StatefulSet, Daemonset, or another resource that manages a set of pods in K8s. When you update the specification of these resources (for example, changing the image of a container to a new version), Kubernetes begins the rollout process. This process gradually replaces instances of the old version of your application with the new one, according to the deployment strategy defined.

A __Rollback__ is the process of reverting a Deployment or other managed sets of pods to a previous known state. This is particularly useful when the new version of the application is faulty or doesn't behave as expected, allowing operators to quickly restore the previous version of the application and minimize downtime or impact on the service.

Kubernetes supports automated rollbacks in the Deployment resource. When a deployment fails or is not stable, Kubernetes can automatically revert to a previous stable version of the application.

## Deployment Strategies

__Recreate Strategy (not recommended):__ This method relies on you destorying your old deployment and then recreating the deployment with your updates. This is not recommended as it will result in downtime to your application and does not take advantage of the power K8s provides.

__Rolling Update Strategy (RECOMMENDED):__ This is where K8s shines. This strategy instead takes your instances down one at a time and replaces them one at a time. It does not necessarilly need to be one at a time and instead can also be in small batches but the principle remains the same. This provides a seemless way to update your applications without any downtime, but does require that your application is able to handle running multiple versions simultaneously while this switch occurs. This is the default deployment strategy.

__Blue/Green Strategy:__ In this strategy, two identical environments are maintained where one (blue) hosts the current application version and the other (green) is the version you wish to update to. Once the green version is ready and tested, traffic is simply switched from the blue to the green. This strategy is best suited for more critical applicaitons where validation of the new version in a production-like environment is required before making a full transition.

__Canary Deployment Strategy:__ Canary deployments involve rolling out the new version to a small subset of users at first, before gradually increasing the rollout to all users. This strategy allows teams to monitor the performance and stability of the new version under a real workload without affecting all users. This strategy is best suited for applications where it is important to monitor the impact of new releases in a live environment. It allows the testing of new features or versions on real users to get feedback before officially making the decision to move forward. Once determined whether to move forward with the release, you would then choose one of the aforementioned strategies in which to make that full transition and remove your canary deployment.

## Useful Commands

Once updating your application instances, simply applying those changes will automatically trigger the default Rolling Update strategy unless otherwise specified. Simply make your changes and then use this command to perform this: `kubectl apply -f deployment-definition.yaml`

You can see the status of your rollouts with the command:
`kubectl rollout status deployment/myapp-deployment`

To see your deployments history of rollouts and revisions you can use this:
`kubectl rollout history deployment/myapp_deployment`

If you would like to check the status of a revision to a specific deployment you can use the '--revision' flag like this: `kubectl rollout history deployment nginx --revision=3`

If you need to rollback to the previous revision use something like this: `kubectl rollout undo deployment nginx`. This would rollback to revision 2.

If you ever need to rollback to a specific revision use the 'to-revision' flag with something like this: `kubectl rollout undo deployment nginx --to-revision=1`