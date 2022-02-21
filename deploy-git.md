[TOC]

# Build and deploy an application from source code in a Git repository

## Create a Java application on OpenShift web console

On Developer perspective, click "+Add", and choose "Import from Git".

Input the Git repo URL:
```bash
https://github.com/openshift-roadshow/nationalparks.git
```

Choose import strategy as "Builder Image" to use OpenShift S2I build.
Choose Builder image as "Java", and choose builder image vesion as `openjdk-11-ubi8`.

Input "Application Name" as `workshop`.
Input "Name" as `nationalparks`.

Check resources type as "Deployment".

Check "Create a route to the application" to expose external access via OpenShift route.

Click "Labels" to add below labels:

```bash
app=workshop
component=nationalparks
role=backend
```

Click "Create" button to create the application.



## Create a Java application via OpenShift CLI

```bash
oc project will-test

# create application
# `java:openjdk-11-ubi8` is an imagestreamtag in `openshift` namespace
oc new-app -n will-test \
  java:openjdk-11-ubi8~https://github.com/openshift-roadshow/nationalparks.git \
  --name=nationalparks \
  --labels=app=workshop,component=nationalparks,role=backend
  
# add labels for "Application name" and "Runtime icon"
oc label deployment nationalparks "app.kubernetes.io/part-of"=workshop "app.openshift.io/runtime"=java

# expose as https route
oc create route edge nationalparks --service=nationalparks
```



## Verify application

### Verify builds

On OpenShift web console, on Adminstrator perspective, open Builds / Builds, to check build logs.

Because we don't have private Maven repository now, so it takes some time to download dependencies from the central Maven repository.

Once the build completed, it will push the container image to OpenShift internal image registry.

Or you can check build logs via OpenShift CLI:

```bash
oc logs -f bc/nationalparks
```



### Verify deployment

Open the `nationalparks` deployment details page, check all Pods are running and ready.

Or you can check deployment and Pods via OpenShift CLI:

```bash
oc get deployment
oc get pods
```



### Verify route

Open Networking / Routes, to check the `nationalparks` route.

Or check the route via OpenShift CLI:

```bash
oc get route
```



We don't configure health check yet, so wait for a few minutes and refresh if the application is not avaiable.



Access <http://{ROUTE}/ws/info> via browser.

Notes: It is `http` not `https`, because when we check"Create a Route to the Applicaiton", we haven't chosen "Secure Route" and choose "TlS termiantion type".



You can re-create a `https` route:

```bash
oc delete route nationalparks
oc create route edge nationalparks --service=nationalparks
```



Now, access <https://{ROUTE}/ws/info> via browser.



## What is next

We will deploy a MongoDB database in the next section.



