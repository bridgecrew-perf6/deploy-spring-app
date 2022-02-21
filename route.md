[TOC]

# Exposing your application to users outside of the cluster



## Create a route on OpenShift web console

1. Via the **Administrator Perspective**, just click **Networking → Routes** and then the **Create Route** button.
2. Insert **parksmap** in **Name** field.
3. From **Service** field, select **parksmap**. For **Target Port**, select **8080**.
4. In **Security** section, check **Secure route**. Select **Edge** from **TLS Termination** list. Choose Ins
5. Leave all other fields blank and click **Create**:



View routes:

1. Networking / Routes, and see locations.
2. Topoloy, click decorator icon to open route.



## Create a route via OpenShift CLI



```bash
# create an edge route
oc create route edge parksmap --service=parksmap

# check routes
oc get route
```





## Access parksmap

Acces parksmap application with route via browser.

Need grant permission to get your position.



