[TOC]

# Deploy an application using a pre-existing container image

Image:
```bash
quay.io/openshiftroadshow/parksmap:latest
```

## Deploy a container image on OpenShift web console

### Deploying your first image

On Developer perspective, choose `will-demo` project and then:
1. Open "+Add" menu
2. Choose "Container images"
3. Check "Image name from external registry", and input image as:
```bash
quay.io/openshiftroadshow/parksmap:latest
```
4. Choose "Runtime icon" as "spring-boot"
5. Input "Applicaiton name" as `workshop`
6. Input "Name" as `parksmap`
7. Check "Deployment" as resource type
8. Uncheck "Create a route to the Applicaiton" (For learning purposes, we will create a Route for the application later in the workshop.) 
9. Click "Labels" to add below labels:
```properties
app=workshop
component=parksmap
role=frontend
```
10. Click "Create" button to create and deploy the application

Once the application is created, go to the "Topology" menu to check the new created application.

Notes:
- Project: `will-demo`
- Application (A): `workshop`
- Deployment (D): `parksmap`
- Runtime icon: Spring Boot logo
- Status: Dark blues means Pods are running and ready

Click the deployment to see the details.


### Backround: Container and Pods

Please see reference for detils.

> Each Pod has its own IP address, therefore owning its entire port space, and containers within pods can share storage. Pods can be "tagged" with one or more labels, which are then used to select and manage groups of pods in a single operation.

### Examing the Pod

Switch to Adminstrator perspective:
1. Open Workloads / Pods, viw all pods under the project
2. Open a Pod to check its details, example:
- Pod details
    - Name
    - Labels
    - Pod IP
    - Node (Which nod the Pod is running)
- Container
    - Container name
    - Container image
- YAML (Pod defintion)
- Metrics (monitor the Pod metrics)
- Environment (environment vars)
- Logs (view Pod logs)
- Events (view Pod lifecycle events)
- Terminal (enter the Pod)


### Services

The way that a Service maps to a set of Pods is via a system of Labels and Selectors. Services are assigned a fixed IP address and many ports and protocols can be mapped.

1. Open Networking / Services
2. Open the service details page, example:
- Hostname: `<name>.<project>.svc.cluster.local`
- Service address
- Service port mapping
3. Open YAML to view service definiton
4. Open Pods to view Pods links with the service

How a service link a set of Pods ?

Service `selector` yaml:
```yaml
selector:
    app: parksmap
    deploymentconfig: parksmap
```

Pod `labels` yaml:
```yaml
labels:
    app: parksmap
    component: parksmap
    deploymentconfig: parksmap
    pod-template-hash: 7655f98b6f
    role: frontend
```



## Deploy a container image via OpenShift CLI

### Deploy a pre-built image

Use OpenShift CLI to deploy the same container image in another project `will-test`:

```bash
# create project
oc new-project will-test

# create application
oc new-app -n will-test \
  --image=quay.io/openshiftroadshow/parksmap:latest \
  --name=parksmap \
  --labels=app=workshop,component=parksmap,role=frontend

# add labels for "Application name" and "Runtime icon"
oc label deployment parksmap "app.kubernetes.io/part-of"=workshop "app.openshift.io/runtime"=spring-boot

```



### View resources

```bash
# view all resource in the project
oc get all -n will-test

# view deployment
oc get deployment
oc get deployment --show-labels
oc get deployment parksmap -o yaml

# view pods
oc get pods
oc get pods --show-labels
oc get pods -o wide
oc get pods <pod> -o yaml

# view service
oc get svc
oc get svc -o wide
oc get svc parksmap -o yaml
```





### Clean up resource (dangerous)

```bash
# delete project
oc delete project will-test
```



