[TOC]

# Configure Application Healthchecks

## Configure Applicaiton Healthchecks on OpenShift web console

Open `nationalparks` deployment details page, and click actions dropdown menu, and choose "Add Health Checks".

Click to **Add Readiness Probe** and add in Path field:
```bash
/ws/healthz/
```

Leave other values as defeault.
Repeat the same procedure for Liveness Probe, click to **Add Liveness Probe**.

Watch until the deployment rolledout successfully.

## Configure Application Healthchecks via OpenShift CLI

```bash
oc set probe deployment/nationalparks --readiness --get-url=http://:8080/ws/healthz
oc set probe deployment/nationalparks --liveness --get-url=http://:8080/ws/healthz
```

Veiry deployment rollout:
```bash
watch oc get pods
```

