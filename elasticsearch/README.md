# Pre-Requisites
1. The Kibana Dashboard should be using passthrough tls termination 
2. A SA metricbeat needs to be created in the `elasticsearch-test` namespace and provided with cluster-admin privileges (Granular roles needed yet to be determined)
3. Ensure that the user-workload has been enabled and you have created the user-defined workload monitoring config map
   Reference for Enabling User Workload Monitoring: https://docs.openshift.com/container-platform/4.9/monitoring/enabling-monitoring-for-user-defined-projects.html
   Reference for Creating User-Defined Workload Monitoring Config Map: https://docs.openshift.com/container-platform/4.10/monitoring/configuring-the-monitoring-stack.html#creating-user-defined-workload-monitoring-configmap_configuring-the-monitoring-stack
5. Expose the metricbeat svc once it has been created
6. ECK operator is installed on the app cluster that needs to be monitored

# Steps:
1. Create the ES instance in `elasticsearch-test` namespace
```
# oc create -f es.yaml
```
Note: The instance is using a persistent volume in the YML, but it can be circumvented using EmptyDir.
      Reference: https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-volume-claim-templates.html
2. Create the Kibana instance in `elasticsearch-test` namespace
```
# oc create -f kibana.yml
```
3. Create the metricbeat instance in the `elasticsearch-test` namespace
```
# oc create -f metricbeat.yml
```
4. Edit the user-defined workload monitoring config map and add the remote-write endpoint URL using the following notation `cluster-monitoring-config.yaml` as reference
```
# oc -n openshift-user-workload-monitoring edit configmap user-workload-monitoring-config
```
