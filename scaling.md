[TOC]

# Scale up your application in order to handle web traffic

## Deployments and ReplicatSets

While Services provide routing and load balancing for Pods, which may go in and out of existence, ReplicaSet (RS) and ReplicationController (RC) are used to specify and then ensure the desired number of Pods (replicas) are in existence. For example, if you always want your application server to be scaled to 3 Pods (instances), a ReplicaSet is needed. Without an RS, any Pods that are killed or somehow die/exit are not automatically restarted. ReplicaSets and ReplicationController are how OpenShift "self heals" and while Deployments control ReplicaSets, ReplicationController here are controlled by DeploymentConfigs.



## Explore scaling

### Explore scaling on OpenShift web console

On Adminstrator perspective
1. Open Workloads / Deployments
2. Open the deployment details, and check its Pods
- How many Pods ?
- How many Pods are running ?
- How many Pods are ready ?
- If any Pods restarted ?
3. Or you can open its ReplicaSets to check same Pods information



### Explore scalling via OpenShift CLI

```bash
# check deployment
oc get deployment

# check replicaset
oc get rs
```



## Scale application

### Scale application on OpenShift web console

Open the deployment details page, click arrow up/down to scale up/down the Pods in deployment.

Or click "Actions" dropdown menu, and choose "Edit Pod count" to set the Pods count.



### Scale application via OpenShift CLI

```bash
# scale replicas as 2
oc scale deployment parksmap --replicas 2

# check pods to see the Pod IP and which Node the Pods are running
oc get pods -o wide

# check endpoints
# the endpoints matches Pods IP and ports
oc get endpoints
```



## Application Self Healing

Watch the Pods in a terminal:

```bash
watch oc get pods
```



Delete a pod in aother terminal:

```bash
oc delete pod 
```



You can see the deleted Pod is terminating, and a new Pod is creating, and finaly the new Pod is running and ready.



## Scale Down

Scale down the application as 1 before proceed later exercises.

```bash
oc scale deployment parksmap --replicas 1 -n will-test
```

