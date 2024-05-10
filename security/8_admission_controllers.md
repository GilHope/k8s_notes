# Admission Controllers

Admission Controllers help add additional security measures to enforce policies on objects during their creation, modification, or deletion phases.

They act as gatekeepers that intercept requests to the Kubernetes API Server before the request is processed and before an object's state is persisted to the Kubernetes datastore (etcd).

There are two types of Admisson Controllers: Validating and Mutating.

1. __Validating Admission Controllers:__ These controllers are responsible for validating the incoming request against a set of rules. If the request does not pass all validations, it is rejected, and an error is returned to the user. For example, a Validating Admission Controller might check whether all required fields in a resource (like a Pod) are filled out correctly.

2. __Mutating Admission Controllers:__ These controllers can modify the objects they admit; this could include adding, modifying, or deleting parts of the request object before it is saved. They are called before validating admission controllers so that any changes they make can be validated.

There a number of admission controllers which come by default inside Kubernetes, here are a few examples:

- __AlwaysPullImages:__ ensures that every time a pod is created that the images are pulled.
- __DefaultStorageClass:__ observes the creation of PVCs and automatically adds a default storage class to them if one is not specified.
- __EventRateLimit:__ helps set a limit on the requests the API Server can handle at a time to prevent the API Server from being flooded with requests.
- __NamespaceExists:__ rejects requests to namespaces that do not exist.


This is how the flow looks when implemented:

__Kubectl > Authentication > Authorization > Admission Controllers > Etcd Commit__

When a command iteracts with the cluster, it sends the HTTP request to the Kubernetes API Server. The API Server then verifies the identity of the requestor through its authentication mechanisms and once authenticated is checked for the necessary permissions for the requested action. After that is where the Admission Controllers may step in. They can modify the request (mutating admission controllers) and/or validate it (validating admission controllers) according to various configured policies. If the request passes all the admission controls, it is then processed by the API Server and committed to the cluster's database (etcd) and the new resource state is now persisted. 

Example,

Say we want to create a pod in a namespace called 'blue', but we forgot that there is no namespace named blue! If we ran a command to create a pod in the blue namespace, it would throw an error indicating that the blue namespace does not exist. 

What is really happening in this example is that the request is successfully getting authenticated and authorized, but once it hits the admission controllers it is getting stopped by the built-in default 'NamespaceExists' controller after it checks for the blue namespace's existence and realizes it in fact does not. So, the request is rejected.

But there is a configured admission controller you could enable for such a case. Perhaps you already know that the blue namespace does not exist but you want to create it automatically during the process of your original command. Configuring the 'NamespaceAutoProvision' admission controller would recieve our request, realize the blue namespace does not exist, and then would automatically create it for us.

__NOTE: The NamespaceExists and NamespaceAutoProvision admission controllers are now deprecated and now replaced by __NamespaceLifecycle__. 

## Useful Commands

To view a list of admission controllers enabled by default, use:
`kube-apiserver -h | grep enable-admission-plugins`


To add a new admission controller, all you need to do is update the 'enable-admission-plugins' flag on the kube-apiserver service.

Conversely, if you need to disable an admission controller, then you go to kube-apiserver service and add the flag.`--disable-admission-plugins=<admission-controller-to-disable>`

