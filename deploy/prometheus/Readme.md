# Prometheus

## installation with persistence

```
sudo kubectl apply --kustomize github.com/rsucasas/ingress-nginx/deploy/prometheus/
```

## without persistence

#### INSTALLATION

1. NAMESPACE WHERE TO INSTALL ALL: ingress-nginx

2. PROMETHEUS INSTALLATION (https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/)

```
	> sudo kubectl apply --kustomize github.com/kubernetes/ingress-nginx/deploy/prometheus/

	> nohup kubectl port-forward -n ingress-nginx service/prometheus-server 9090:9090 --address 0.0.0.0 &
```

3. NODE-EXPORTER

```
	> sudo kubectl apply -f ./node-exporter.yaml
```

```yaml
#------------------------------------------------------------------------
#node-exporter.yaml:
#------------------------------------------------------------------------
apiVersion: apps/v1  #FAQ02
kind: DaemonSet  ##FAQ01
metadata:
  name: node-exporter
  namespace: ingress-nginx
  labels:
	k8s-app: node-exporter
spec:
  selector:  ##FAQ03
	matchLabels:
	  k8s-app: node-exporter
  template:
	metadata:
	  labels:
		k8s-app: node-exporter
	spec:
	  containers:
	  - image: prom/node-exporter
		name: node-exporter
		ports:
		- containerPort: 9100
		  protocol: TCP
		  name: http
	  tolerations:
	  - effect: NoSchedule
		operator: Exists
---
apiVersion: v1
kind: Service
metadata:
  labels:
	k8s-app: node-exporter
  name: node-exporter
  namespace: ingress-nginx
spec:
  ports:
  - name: http
	port: 9100
	nodePort: 31672
	protocol: TCP
  type: NodePort
  selector:
	k8s-app: node-exporter
#------------------------------------------------------------------------
```

4. NODE-EXPORTER CONFIGURATION (PROMETHEUS)

```yaml
#------------------------------------------------------------------------
- job_name: node-exporter
  scrape_interval: 30s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  kubernetes_sd_configs:
  - api_server: null
	role: endpoints
	namespaces:
	  names:
	  - ingress-nginx
  relabel_configs:
  - source_labels: [__meta_kubernetes_service_label_k8s_app]
	separator: ;
	regex: node-exporter
	replacement: $1
	action: keep
  - source_labels: [__meta_kubernetes_endpoint_port_name]
	separator: ;
	regex: http
	replacement: $1
	action: keep
  - source_labels: [__meta_kubernetes_namespace]
	separator: ;
	regex: (.*)
	target_label: namespace
	replacement: $1
	action: replace
  - source_labels: [__meta_kubernetes_pod_name]
	separator: ;
	regex: (.*)
	target_label: pod
	replacement: $1
	action: replace
  - source_labels: [__meta_kubernetes_service_name]
	separator: ;
	regex: (.*)
	target_label: service
	replacement: $1
	action: replace
  - source_labels: [__meta_kubernetes_service_name]
	separator: ;
	regex: (.*)
	target_label: job
	replacement: ${1}
	action: replace
  - source_labels: [__meta_kubernetes_service_label_k8s_app]
	separator: ;
	regex: (.+)
	target_label: job
	replacement: ${1}
	action: replace
  - separator: ;
	regex: (.*)
	target_label: endpoint
	replacement: http
	action: replace
#------------------------------------------------------------------------
  
  ```
