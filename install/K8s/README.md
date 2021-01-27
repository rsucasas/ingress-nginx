# K8s (Ubuntu 18.04)

-----------------------

[INSTALLATION](#Installation)

-----------------------

#### GENERATE SSH KEYs:

```
ssh-keygen

ssh-copy-id user@host
## or if your server uses custom port no:
ssh-copy-id "user@host -p 1234"
```

#### CHANGE PASSWORD:

passwd

-----------------------

## Steps to Install Kubernetes on Ubuntu

https://phoenixnap.com/kb/install-kubernetes-on-ubuntu


#### Step 1: Install Docker

```
sudo apt-get update

sudo apt-get install docker.io

docker version
```

```
sudo groupadd docker

sudo usermod -aG docker ${USER}
```

[log out and log in]


#### Step 2: Start and Enable Docker

```
sudo systemctl enable docker

sudo systemctl status docker
```

To start Docker if it’s not running: sudo systemctl start docker


#### Step 3: Add Kubernetes Signing Key

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```


#### Step 4: Add Software Repositories

```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```


#### Step 5: Kubernetes Installation Tools

```
sudo apt-get install kubeadm kubelet kubectl

sudo apt-mark hold kubeadm kubelet kubectl

kubeadm version
```


#### Step 6: Begin Kubernetes Deployment

```
sudo swapoff -a
```


#### Step 7: Assign Unique Hostname for Each Server Node 

```
sudo hostnamectl set-hostname worker01
```


#### Step 8: Initialize Kubernetes on Master Node

```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

```
====> Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

sudo kubeadm join 192.168.7.42:6443 --token ogsbbv.duqzrzohdvabboj7 \
    --discovery-token-ca-cert-hash sha256:d4a6f69caffaa773e65a44ae4d743a870bfa98081db9699968779934da13e584
	
	
sudo kubeadm join 192.168.7.42:6443 --token 96lwb5.1ff803s697k2201r     --discovery-token-ca-cert-hash sha256:d4a6f69caffaa773e65a44ae4d743a870bfa98081db9699968779934da13e584

TO GET THE TOKEN:
1. Master:

kubeadm token list

kubeadm token generate

kubeadm token create 96lwb5.1ff803s697k2201r --print-join-command --ttl=0

```	
	
```
mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### Step 9: Deploy Pod Network to Cluster

```
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl get pods --all-namespaces
```


#### Step 10: Join Worker Node to Cluster

```
kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443

kubectl get nodes
```


From master:

```
kubectl label node kube9 node-role.kubernetes.io/worker=worker
kubectl label node worker02 node-role.kubernetes.io/worker=worker
```


#### Deploying the Dashboard UI

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml

kubectl proxy --address='0.0.0.0' --port=8001 --accept-hosts= '.*' &
```

Example:

**URL:** http://192.168.1.2:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/



#### How to access Kubernetes Dashboard from outside network

https://stackoverflow.com/questions/53957413/how-to-access-kubernetes-dashboard-from-outside-network

Example:

```
kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8443:443 --address 0.0.0.0 &
```

**URL:** https://192.168.1.2:8443



------------------

#### PROMETHEUS

https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/

kubectl apply --kustomize github.com/kubernetes/ingress-nginx/deploy/prometheus/

kubectl get svc -n ingress-nginx




kubectl expose deployment prometheus-server  --type=LoadBalancer --name=prometheus-server 


nohup kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 8443:443 --address 0.0.0.0 &

nohup kubectl port-forward -n ingress-nginx service/prometheus-server 9090:9090 --address 0.0.0.0 &



###### How To Setup Kube-State-Metrics on Kubernetes

https://devopscube.com/setup-kube-state-metrics/



###### How To Setup Node-Exporter on Kubernetes
https://www.programmersought.com/article/22584628362/#%E4%BA%8C%E3%80%81Node%20Exporter%E9%83%A8%E7%BD%B2
