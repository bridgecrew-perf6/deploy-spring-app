[TOC]

# Set up webhooks to enable automated application build

## Add Triggers to your Pipeline

```bash
oc create -f tekton/triggers/nationalparks-triggers.yaml
```

The trigger definiton is from <https://raw.githubusercontent.com/openshift-roadshow/nationalparks/master/pipeline/nationalparks-triggers.yaml>



This will create a new Pod with a Route that we can use to setup our Webhook on GitHub to trigger the automatic start of the Pipeline.


Create below resources:
- **TriggerTemplate**: a trigger template is a template for newly created resources. It supports parameters to create specific PipelineResources and PipelineRuns.
- **TriggerBinding**: validates events and extracts payload fields
- **EventListener**: connects TriggerBindings and TriggerTemplates into an addressable endpoint (the event sink). It uses the extracted event parameters from each TriggerBinding (and any supplied static parameters) to create the resources specified in the corresponding TriggerTemplate. It also optionally allows an external service to pre-process the event payload via the interceptor field.
- **Route**: webhook url



## Fork GitHub project and add web hook

Fork the GitHub project to <https://github.com/xdevops-caj-lab/nationalparks>

From your fork page top-right menu, click **Settings**. Then from result left-side menu, click **Webhook**, then from right side click **Add webhooks**.



Add Payload URL is `el-nationalparks` route url: 

```bash
http://el-nationalparks-will-test.apps.ocpdemo.sandbox956.opentlc.com/
```



Set "Content type" as "application/json", and check "Just the push event", and check "active".



## Try change some codes and trigger pipeline running



Modify <https://github.com/xdevops-caj-lab/nationalparks/blob/master/src/main/java/com/openshift/evg/roadshow/parks/rest/BackendController.java#L20> as below

```java
return new Backend("nationalparks","Amazing National Parks", new Coordinates("47.039304", "14.505178"), 4);
```



Commit to `master` branch.

Watch `nationalparks-pipeline` to see if the pipeline is triggered automatically.



