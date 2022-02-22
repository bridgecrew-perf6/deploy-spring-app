[TOC]

# Continuous Integration and Pipelines

## Install OpenShift Pipelines from OperatorHub

Install "Red Hat OpenShift Pipelines" operator (which base on Tekton) from OperatorHub via Cluster Admin with default values.



## Create Your Pipeline

```bash
oc create -f tekton/pipelines/nationalparks-pipeline-new.yaml 
```

The pipeline is from <https://raw.githubusercontent.com/openshift-roadshow/nationalparks/master/pipeline/nationalparks-pipeline-new.yaml>

On OpenShift web console, switch to Adminstrator perspective, and open Pipelines / Pipelines.

Or use Tekton CLI (`tkn`) to show the pipeline:
```bash
tkn pipeline describe nationalparks-pipeline
```



The pipeline include below **tasks**:

- **git-clone**: this is a *ClusterTask* that will clone our source repository for nationalparks and store it to a Workspace app-source which will use the PVC created for it app-source-workspace
- **build-and-test**: will build and test our Java application using maven *ClusterTask*
- **build-image**: this is a buildah *ClusterTask* that will build an image using a binary file as input in OpenShift, in our case a JAR artifact generated in the previous Task.
- **redeploy**: it will use an openshift-client *ClusterTask* to deploy the created image on OpenShift using the Deployment named nationalparks we created in the previous lab, using the



The pipeline has below **parameters**:

- `APP_NAME`: default value is `nationalparks`
- `APP_GIT_URL`: default value is `https://github.com/openshift-roadshow/nationalparks.git`
- `API_GIT_VERSION`: default value is `master`



The pipeline has below **workspaces**:

- `app-source`: linked to a PersistentVolumeClaim app-source-pvc will becreated. This will be used to store the artifact to be used in different Task

- `maven-settings`: an EmptyDir volume for the maven cache, this can be extended also with a PVC to make subsequent Maven builds faster

  

## Add Storage for your Pipeline

References:
- <https://docs.openshift.com/container-platform/4.8/storage/understanding-persistent-storage.html>
- <https://docs.openshift.com/container-platform/4.9/storage/persistent_storage/persistent-storage-aws.html>


The Lab env use aws-ebs storage, once you create PVC, it will create the persistent volume claim and generate a persistent volume..



### Add PVC for app-source workspace

From **Administrator Perspective**, go to **Storage**→ **Persistent Volume Claims**.

Inside **Persistent Volume Claim name** insert **app-source-pvc**.

In **Size** section, insert **1** as we are going to create 1 GiB Persistent Volume for our Pipeline, using RWO Single User access mode.

Leave all other default settings, and click **Create**.


The PVC is "Pending" state and the PV doesn't exist until the Pod use it (here means start the pipeline).

The PVC defintion yaml references are in `tekton/workspaces`.





## Troubleshooting

Build image failed error x509:

```bash
+ buildah --storage-driver=vfs push --tls-verify=true --digestfile /workspace/source/image-digest image-registry.openshift-image-registry.svc:5000/will-test/nationalparks:latest docker://image-registry.openshift-image-registry.svc:5000/will-test/nationalparks:latest
Getting image source signatures
Getting image source signatures
Getting image source signatures
Getting image source signatures
Get https://image-registry.openshift-image-registry.svc:5000/v2/: x509: certificate signed by unknown authority
level=error msg="exit status 1"
```

Modify `build-image` task paramter `TLSVERIFY` as `fasle`.



## Speedup Maven build

References:

- <https://hub.tekton.dev/tekton/task/maven>

- <https://developers.redhat.com/blog/2020/02/26/speed-up-maven-builds-in-tekton-pipelines#build_a_pipeline_with_a_workspace>
- <https://blog.csdn.net/weixin_43902588/article/details/105535794>
- [Tekton Catalog](https://github.com/tektoncd/catalog/)



### Check ClusterTask definition on OpenShift web console

On Adminstrator perspective, open Pipelines / Tasks, and open ClusterTasks.

### Add PVC for Maven Local cache

If each pipeline need cache Maven repo, it will cause much disk usage.

Suggest to use intenal Nexus repo to act as local central cache ?

### Share Maven settings

Use a PVC for maven-settings or a ConfigMap for Maven settings.xml ?





