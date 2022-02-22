[TOC]

# Deploy a database from the Red Hat OpenShift service catalog

## Create a MongoDB template

Notes: Since OCP 4.6 all MongoDB-based samples have been replaced, deprecated, or removed. <https://access.redhat.com/documentation/en-us/openshift_container_platform/4.6/html/release_notes/ocp-4-6-release-notes#ocp-4-6-mongodb-removed>



Import `mongodb:3.6` image stream to `openshift` namespace via Cluster Admin:

```bash
oc import-image mongodb:3.6 \
  --from=quay.io/centos7/mongodb-36-centos7 \
  --confirm \
  -n openshift
```



Verify the mongodb imagestreamtag:

```bash
oc get imagestreamtag -n openshift | grep -i mongodb
```



Create MongoDB template in `will-demo` namespace:

```bash
oc create -n will-demo -f ./templates/mongodb-template.yaml
```

The `mongodb-template.yaml` is from <https://raw.githubusercontent.com/openshift-labs/starter-guides/ocp-4.6/mongodb-template.yaml>, and refer to <https://github.com/sclorg/mongodb-container/tree/master/latest> to use `quay.io/centos7/mongodb-36-centos7` container image.



Verify created tempalte in `wiil-demo` project.

On Developer perspective, click "+Add" button, and choose "Database", and open "Mongo".
You can find "MongoDB(Ephemeral)" template.





## Deploy MongoDB

On Developer perspective, click "+Add" button, and choose "Database", and open "Mongo", and click  "MongoDB(Ephemeral)" template, and then click "Instantiate Template".



Input values:

- Database Service Name: `mongodb-nationalparks`
- MongoDB Connection Username: `mongodb`
- MongoDB Connection Password: `mongodb`
- MongoDB Database Name: `mongodb`
- MongoDB Admin Password: `mongodb`



Click "Create" button to deploy MongoDB instance.

Verify the deployment cofnig `mongodb-nationalparks` roll out sucessfully.



## Inject MongoDB secret to deployment

Open Secrets, and search "mongodb", and find "mongoldb-ephemeral-parameters" secret, and open its details page.

Click "Add Secret to workload" button, and select workload as `nationalparks` deployment and check "Environment Variables", and then click "Save" button.



Or you can open the `nationalparks` deployment, and open "Environment", and click "Add all from ConfigMap or Secret" to add "mongoldb-ephemeral-parameters" secret.





## Add labels for MongoDB

```bash
# add labels for mongodb
oc label dc/mongodb-nationalparks svc/mongodb-nationalparks \
  app=workshop component=nationalparks role=database \
  --overwrite
```





## Deploy MongoDB via OpenShift CLI



```bash
# switch the current project
oc project will-test

# create template
oc create -n will-test -f ./templates/mongodb-template.yaml

# deploy mongodb via template
oc new-app --template=mongodb-ephemeral \
  --name="mongodb-nationalparks" \
  --param=DATABASE_SERVICE_NAME=mongodb-nationalparks \
  --param=MONGODB_USER=mongodb \
  --param=MONGODB_PASSWORD=mongodb \
  --param=MONGODB_DATABASE=mongodb \
  --param=MONGODB_ADMIN_PASSWORD=mongodb \
  -n will-test
  
 
# create mongodb parameter secret base on prameters name and value
cat > /tmp/mongodb.env << EOF
DATABASE_SERVICE_NAME=mongodb-nationalparks
MONGODB_USER=mongodb
MONGODB_PASSWORD=mongodb
MONGODB_DATABASE=mongodb
MONGODB_ADMIN_PASSWORD=mongodb
EOF
oc create secret generic mongodb-ephemeral-parameters --from-env-file=/tmp/mongodb.env

# inject mongodb parameter secret to deployment
oc set env --from=secret/mongodb-ephemeral-parameters deployment/nationalparks

# add labels for mongodb
oc label dc/mongodb-nationalparks svc/mongodb-nationalparks \
  app=workshop component=nationalparks role=database \
  --overwrite

```



Clean MongoDB:

```bash
export MONGODB_NAME=mongodb-nationalparks
oc delete dc ${MONGODB_NAME}
oc delete svc ${MONGODB_NAME}
oc delete secret${MONGODB_NAME}
```



## Load national parks data



Access nationalparks route <https://nationalparks-%PROJECT%.%CLUSTER_SUBDOMAIN%/ws/data/load> to load national parks data.

Access nationalparks route <https://nationalparks-%PROJECT%.%CLUSTER_SUBDOMAIN%/ws/data/all> to view national parks data with json format.

Access parksmap route <https://parksmap-%PROJECT%.%CLUSTER_SUBDOMAIN%> 

The main map **STILL** isn’t displaying the parks. That’s because the front end parks map only tries to talk to services that have the right **Label**.



Why the application can access MongoDB: <http://www.github.com/openshift-roadshow/nationalparks/blob/master/src/main/java/com/openshift/evg/roadshow/parks/db/MongoDBConnection.java#L44-l48>



## Test after adding required labels for backend route

Add label to `national-parks` route, so that the forntend can find the national parks data.

Frontend code: <https://github.com/openshift-roadshow/parksmap-web/blob/master/src/main/java/com/openshift/evg/roadshow/rest/RouteWatcher.java#L20>

```bash
oc label route nationalparks type=parksmap-backend
```



Verify labels:

```bash
oc describe route nationalparks
```



Access parksmap route <https://parksmap-%PROJECT%.%CLUSTER_SUBDOMAIN%>, it will show national parks mark in the map now.





