[TOC]

# Access your application container and interacting with it

## Remtoe Shell Session to a Container Using the Web Console

Open the Pod details page, and open "Terminal" menu.

Try to execute some command in the container:
```bash
whoami
id
pwd
ls -l /
```

Run `exit` to quit the session.



## Remote Shell Session to a Container Using the CLI



```bash
# view pods
oc get pods

# connect the pod
# press <tab> key to auto-complete pod name
oc rsh <pod>

# try to execute some comands
ls -l /

# quit the session
exit
```



## Execute a Command in a Container



```bash
oc exec <pod> -- <command>

# example
oc exec parksmap-9f4dd6744-t7k2t -- ls -l /
oc exec parksmap-9f4dd6744-t7k2t -- whoami
```

