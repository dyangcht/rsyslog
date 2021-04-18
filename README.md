# Docker images for OpenShift

This project provides docker images of useful software, tailored to run on
OpenShift.

## Available images

Currently, the following software are available: 
- [OpenLDAP](openldap)
- [rsyslog](rsyslog)
- [Grafana](grafana)

### Set env var for apicast
```
$ oc set env dc/apicast-staging APICAST_MODULE=apicast/policy/verbose/verbose
```

### Create a config map including application and policy files
```
$ oc create configmap apicast-ver-module --from-file=verbose.lua --from-file=apicast-policy.json --from-file=init.lua
```

### Mounting

```
$ oc set volume dc/apicast-staging --add --name=apicast-custom-module --mount-path /opt/app-root/src/src/apicast/policy/verbose/ --type=configmap --configmap-name=apicast-ver-module --overwrite
```

### Mounting
The version number has to mount to a path
```
$ oc set volume dc/apicast-staging --add --name=apicast-01-module --mount-path /opt/app-root/src/policies/verbose/0.1/ --type=configmap --configmap-name=apicast-ver-module --overwrite
```
