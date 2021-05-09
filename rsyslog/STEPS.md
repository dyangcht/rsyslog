# APIcast Policy customization

### create config map from files and mount it on apicast-staging
```
oc project threescale-<fdbb>
oc create configmap apicast-ver-module --from-file=verbose.lua --from-file=apicast-policy.json --from-file=init.lua
oc set volume dc/apicast-staging --add --name=apicast-custom-module --mount-path /opt/app-root/src/src/apicast/policy/verbose --type=configmap --configmap-name=apicast-ver-module --overwrite
oc set volume dc/apicast-staging --add --name=apicast-01-module --mount-path /opt/app-root/src/policies/verbose/0.1/ --type=configmap --configmap-name=apicast-ver-module --overwrite
```
### set APIcast module name and path
```
oc set env dc/apicast-staging APICAST_MODULE=apicast/policy/verbose/verbose
oc rollout latest apicast-staging
```

### OpenTracing and Jaeger
``` wget https://raw.githubusercontent.com/jaegertracing/jaeger-openshift/master/all-in-one/jaeger-all-in-one-template.yml ```

### Change some values like Deployment's version and selector
```
$ vi jaeger-all-in-one-template.yml
Original: apiVersion: extensions/v1beta1
New: apiVersion: apps/v1
Add New:
     selector:
       matchLabels:
         app: jaeger
         jaeger-infra: jaeger-pod
```



### Apply it
```
oc project threescale-1ff4
oc process -f jaeger-all-in-one-template.yml| oc create -f -
oc create configmap jaeger-config --from-file=jaeger_config.json
oc set volume dc/apicast-staging --add -m /tmp/jaeger/ --configmap-name jaeger-config
oc set env dc/apicast-staging OPENTRACING_TRACER=jaeger OPENTRACING_CONFIG=/tmp/jaeger/jaeger_config.json
```
### Reference

https://itnext.io/adding-opentracing-support-to-apicast-api-gateway-a8e0a38347d2 <br/>
http://jaeger-query-threescale-1ff4.apps.shared-na4.na4.openshift.opentlc.com/search
