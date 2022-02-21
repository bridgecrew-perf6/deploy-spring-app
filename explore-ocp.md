[TOC]

# Explore OpenShift

References:
- [Explore OpenShift](https://redhat-scholars.github.io/openshift-starter-guides/rhs-openshift-starter-guides/4.7/common-explore.html)


## Login with OpenShift web console
Login in OpenShift web console with username and password.


## Install OpenShfit CLI
References:
- [Getting started with the OpenShift CLI](https://docs.openshift.com/container-platform/4.9/cli_reference/openshift_cli/getting-started-cli.html)

On Mac you can install OpenShift via:
- Use Homebrew to install
- Download from OpenShfit Web Console

Verify:
```bash
oc version
```

## Configure OpenShift CLI
References:
- [Configuring the OpenShift CLI: Enabling tab completion](https://docs.openshift.com/container-platform/4.9/cli_reference/openshift_cli/configuring-cli.html)

## Connect to the OpenShift Cluster from CLI

From Web Console overview, go to top-right menu bar and click to the dropdown menu containing your username, then click Copy Login Command.

Click on Display Token and copy the command under Login with this token.

Notes: The token will expire in 24 hours.

## Use OpenShift CLI
References:
- [Using the OpenShift CLI](https://docs.openshift.com/container-platform/4.9/cli_reference/openshift_cli/getting-started-cli.html#cli-using-cli_cli-developer-commands)

Some common tasks using the OpenShift CLI:

```bash
# login
oc login

# logout
oc logout

# create a project
oc new-project

# create a new app
oc new-app

# view pods
oc get pods

# view pod logs
oc logs -f

# view the current project
oc project

# view the status of the current project
oc status

# list supported api resources
oc api-resources

# get help
oc help

# get help for a command
oc <COMMAND> --help

# view the description and fields of a resource
oc explain <RESOURCE>

```