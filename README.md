# K8s - Cloud orchestration tools

Installation guides of orchestrators like K8s, MicroK8s, K3s, and also tools like Grafana, NFS o Prometheus.

-----------------------

[K8s](#k8s)

[MicroK8s](#microk8s)

[Prometheus](#prometheus)

[NFS server & clients](#nfs-server--clients)

[LICENSE](https://github.com/rsucasas/k8s/blob/master/LICENSE)

-----------------------

## K8s

- [Install a K8s cluster](https://github.com/rsucasas/k8s/blob/master/install/K8s) (_Ubuntu 16, 18_)


### MicroK8s

#### Dashboard

To expose Dashboard:

```
nohup microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard --address 0.0.0.0 8443:443 > dashboard_log.txt &
```

Token used to login

```
microk8s kubectl -n kube-system describe secret default-token
```

Dashboard:

```
https://192.168.1.131:8443/#/login  
```

--------------------

## Prometheus on K8s

- [Prometheus with persistence](https://github.com/rsucasas/k8s/tree/master/deploy/prometheus)

- [Prometheus with persistence (NFS)](https://github.com/rsucasas/k8s/tree/master/deploy/prometheus-NFS)

- Prometheus (MicroK8s)

-----------------------

## NFS server & clients

- [Install server and clients](https://github.com/rsucasas/k8s/tree/master/nfs) (_Ubuntu 16, 18_)
