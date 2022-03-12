Om Kubernetes metrics deployment fallerar så kan det bero på en liten konfiguration som saknas i args. Lägg till detta i args under deployment. Kör `kubectl edit deployments <namn-på-metrics-deployment>`.

Lägg till:

```
spec:
containers:
- args:
- --secure-port=4443
- --cert-dir=/tmp
command:
- /metrics-server
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
- --kubelet-use-node-status-port
- --metric-resolution=15s
image: k8s.gcr.io/metrics-server/metrics-server:v0.6.1
imagePullPolicy: IfNotPresent
```