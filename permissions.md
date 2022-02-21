[TOC]

# Give access to other users to collaborate on your project

## Permissions

### ServiceAccount

References:
- [Understanding and creating service accounts](https://docs.openshift.com/container-platform/4.7/authentication/understanding-and-creating-service-accounts.html)

On Adminstrator perspective, open User Management / ServiceAccounts.

OpenShift automatically creates a few special service accounts in every project.:

- `defautl` run pods
- `builder` build and push image
- `deployer`

Check ServiceAccount via OpenShift CLI:
```bash
oc get sa
```

### RoleBindings

Open a project details page, and check "Role Bindings".

Check RoleBindings via OpenShift CLI:
```bash
oc get rolebinding
```



## Grant Service Account View Permissions

Grant `view` permissions to `default` service account.

Open project details page, and open "Role Bindings", and create below role bindings:

- Name: `view`
- Namespace:
- Role name: ClusterRole `view`
- Subject: `Service Account`, and choose Subject namespace as curent project, and input Subject Name as `default`



Or create the rolebinding via OpenShift CLI:

```bash
oc project will-test
# -z means short name of service account in the curent project
# the long name of service account is `system:serviceaccount:will-test:default`
oc policy add-role-to-user view -z default
```



## Redeploy Application

```bash
oc rollout restart deployment/parksmap
```

