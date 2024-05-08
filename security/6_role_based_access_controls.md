# Role-Based Access Controls (RBAC)

## Overview

How do we create a Role?

We do that by creating a Role Object.

This means we create a role definition file with the API version set to rbac.authorization.k8s.io/v1 and the kind set to role.

For example, if the role is being created for your developers you would name them in the metadata and then assign them some rules.

Each rule has three sections: apiGroups, resources, and verbs.

For you core group you can leave the apiGroup as blank, but for any other group you would name them.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata: 
    name: Developer
rules:rbac.authorization.k8s.io/v1
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]

- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

After making this file you can then create it using:
`kubectl create -f <role-file-name.yaml>`

After this you will then need to link your users, the developers in this instance, to that role.

For that, we need to create another object, known as the Role Binding.

The apiVersion is is also set to "rbac.authorization.k8s.io/v1".

Kind is set to RoleBinding.

Further, it has two sections, subjects and roleRef.

Subjects is where we specify user details and the roleRef section is where the details of the role are provided.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata: 
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

After this, create the  Role Binding using:
`kubectl create -f <role-binding-name.yaml>`

__Note: Roles and Role-Bindings are subject to the Namespace in which they are in unless specified within the metadata of defintion file.__

## How to Check Access

What If You A Using A Role Yourself?

To check what you have access to when using a role, try:
`kubectl auth can-i <verb> <object>`
For example,
`kubectl auth can-i create deployment`
`kubectl auth can-i delete nodes`

You will recieve a 'yes' or 'no' response.

-----------------------------------------------------------

What if you have created a role for another user and want to check to see if you did so successfully, run:
`kubectl auth can-i <verb> <object> --as <user>`
For example,
`kubectl auth can-i create deployment --as dev-user`
`kubectl auth can-i create pods --as dev-user`
 
You will recieve a 'yes' or 'no' response.

-----------------------------------------------------------

If you would like to find out if you have access to a particular namespace, then you can do so like this:
`kubectl auth can-i <verb> <object> --as <user> --namespace <ns-name>`
For example,
`kubectl auth can-i create pods --as dev-user --namespace test`