# OKD OTEL

Enable tracing of core components - etcd, crio, kubelet, kubeapi - in OKD.

WARNING: this uses several unsupported overrides and will make your cluster unupgradable. Use for testing only

Based on https://github.com/sallyom/otel-kubeadm

## HOWTO

### OTEL collector
* `oc apply -k manifests/otel-collector`
* Run `oc get svc -n otel` to get Cluster IP for `otel-collector` service
* Update `ClusterIP-otel-collector-svc` in `manifests/otel-collector/configmap.yaml` with the new IP, re-apply the configmap and delete all pods to make sure they mounted the new configmap

### Jaeger and cert-manager

* `oc apply -f manifests/cert-manager`
* `oc apply -f manifests/jaeger-operator`
* When CRDs have applied create a Jeager instance: `oc apply -f manifests/jaeger`

See `oc describe route/oteljaeger` for Jaeger URL

### CRI-O tracing

Apply the MachineConfig using `oc apply -f manifests/tracing-crio`

### Kubelet tracing

Apply the featuregate using `oc apply -f manifests/tracing-kubelet`
