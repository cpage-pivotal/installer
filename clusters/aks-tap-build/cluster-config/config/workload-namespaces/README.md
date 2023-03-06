# Workload Namespaces

Workload namespaces to be provisioned to the build cluster

Add label resource per this sample definition:

```
apiVersion: v1
kind: Namespace
metadata:
  labels:
    apps.tanzu.vmware.com/tap-workload-ns: ""
  name: [put your name here]
```
