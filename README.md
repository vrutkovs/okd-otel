# OKD OTEL

Enable tracing of core components - etcd, crio, kubelet, kubeapi - in OKD.

WARNING: this uses several unsupported overrides and will make your cluster unupgradable. Use for testing only

Based on https://github.com/sallyom/otel-kubeadm

## HOWTO

### Grafana Alloy
* `oc apply -k manifests/grafana-alloy`
* Run `oc get svc -n otel` to get Cluster IP for `otel-collector` service
* Update `ClusterIP-otel-collector-svc` in `manifests/otel-collector/configmap.yaml` with the new IP, re-apply the configmap and delete all pods to make sure they mounted the new configmap

### CRI-O tracing

Apply the MachineConfig using `oc apply -f manifests/tracing-crio`

### Kubelet tracing

Apply the featuregate using `oc apply -f manifests/tracing-kubelet`

### Kube API tracing

Apply the necessary changes using `oc apply -f manifests/tracing-kubeapi`


### Etcd tracing

Unmanage `etcd` and run the script (update `instace-id`) on each master node
