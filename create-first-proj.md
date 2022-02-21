[TOC]

# Create your first project

## Create project in OpenShift web console

**Create first project:**
On OpenShift web console, click "Project" (or "Add" or "Topology"), and then click "Create new project".

Example: 
Create a `will-demo` project.

**Create another project:**
If you want to create another project, click project dropdown menu, and click "Create Project" button to create another project.

**Swich project:**
Click project dropdown menu to switch the current project.

## Create project via OpenShift CLI

Example:
```bash
# create a project
oc new-project will-test

# switch a project
oc project will-test

# view current project
oc project
```

You can use [kube-ps1](https://github.com/jonmosco/kube-ps1) to show current project in comman dline.
