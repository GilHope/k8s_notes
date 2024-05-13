# Validating and Mutating Admission Controllers

## Validating Admission Controllers

Validating Admission Controllers check the requests to ensure they meet certain criteria before allowing them to proceed. If a request does not meet the specified requirements, the validating controller will reject it adn return an error to the user.

These controllers __do not__ modify the object; they only perform validation.

Examples of Validating Admission Controllers include:

- __NamespaceLifecycle:__ Ensures that requests to a namespace that is being deleted or that has been deleted are denied.
__Note -__ Discussed previously in example. Replaced the now deprecated NamespaceExists validating admission controller.
- __AlwaysPullImages:__ Ensures that requests to a namespace that is being deleted or that has been deleted are denied.
- __ValidatingAdmissionWebhook:__ Calls external webhooks that validate the creation, deletion, and updates to objects without modifying them.
- __DenyEscalatingExec:__ Disallows 'exec' and similar commands that could escalate priviledge inside containers.



## Mutating Admission Controllers

Modifying Admission Controllers can modify requests to enforce default values, inject configuration, or otherwise transform incoming objects before they are validated. After mutation, the request is passed to the validating admission controllers.

Examples of Mutating Admission Controllers include:

- __DefaultStorageClass:__ Automatically assigns a default storage class to Persistent Volume Claims (PVCs) that do not require any specifc storage class.

- __PodPreset:__ Injects certain fields into pods at creation time based on preset configurations.

- __MutatingAdmissionWebhook:__ Calls external webhooks to modify objects before they are created or updated.

- __DefaultTolerationSeconds:__ Sets the default number of seconds that a toleration lasts when not specified in a pod.