# K8s - Cloud orchestration tools

-----------------------

[K8s](#k8s)

[MicroK8s](#microk8s)

[Prometheus](#prometheus)

[NFS server & clients](#nfs-server--clients)

[LICENSE](#license)

-----------------------

## K8s




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

## Prometheus 

- [Prometheus with persistence on K8s & MicroK8s](https://github.com/rsucasas/k8s/tree/master/deploy/prometheus)

- Prometheus on K8s

- Prometheus on MicroK8s

-----------------------

## NFS server & clients

- [Install server and clients on Ubuntu 16, 18](https://github.com/rsucasas/k8s/tree/master/nfs)
