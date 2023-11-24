# pipelines-demo

## How-to-reproduce
1. Create the needed resources  (`Namespace`, `Task`, `Pipeline`, `EventListener`, `TriggerBinding`, `TriggerTemplate`, `Trigger`):
   ```sh
   oc apply -k https://github.com/gmeghnag/pipelines-demo
   ```

2. Expose the `EventListener` to create a `Route`:
   ```sh
   oc expose svc/el-test-el -n pipelines-demo
   ```

3. Execute an `HTTP` `POST` request to trigger a new `PipelineRun`:
   ```sh
   curl -X POST -H "Content-Type: application/json" --data '{"repository": {"url": "test-url", "name": "test-reponame"}, "head_commit": {"id": "test-commit"}}' "http://$(oc get route el-test-el -n pipelines-demo -o jsonpath='{.spec.host}')"
   ```

## Environment
```
$ tkn version
Client version: 0.31.0
Chains version: v0.16.0
Pipeline version: v0.47.4
Triggers version: v0.24.1
Operator version: v0.67.0

$ oc get csv,sub -n openshift-operators 
NAME                                                                                 DISPLAY                       VERSION   REPLACES   PHASE
clusterserviceversion.operators.coreos.com/openshift-pipelines-operator-rh.v1.11.1   Red Hat OpenShift Pipelines   1.11.1               Succeeded

NAME                                                                PACKAGE                           SOURCE             CHANNEL
subscription.operators.coreos.com/openshift-pipelines-operator-rh   openshift-pipelines-operator-rh   redhat-operators   pipelines-1.11
```

