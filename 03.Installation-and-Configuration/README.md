# 03. INSTALLATION AND CONFIGURATION

## Introduction
그냥 동영상으로 소개 (재미없음)


## Installation and Configuration

### Installation Tools
- GKE, Minikube, MicroK8s 같은거 있음
- kubectl 중요함
- kubeadm, Kubespray, hyperkube 등의 방법도 있음


### kubectl 설치
- kubectl은 `$HOME / .kube / config`를 구성 파일로 사용
  - cluster definitions (i.e. IP endpoints), credentials, and contexts
- switch the shell

```
> kubectl config use-context foobar
```


### GKE (Google Kubernetes Engine)
- gcloud 설치하면 kubectl 같이 설치 됨
- [Google Cloud SDK 설치](https://cloud.google.com/sdk/install#linux)

```
> gcloud container clusters create linuxfoundation

> gcloud container clusters list

> kubectl get nodes

> gcloud container clusters delete linuxfoundation
```


### Minikube
- [Minikube GitHub](https://github.com/kubernetes/minikube)

```
> curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64

> chmod +x minikube

> sudo mv minikube /usr/local/bin

> minikube start

> kubectl get nodes
```


### kubeadm
- [kubeadm 가이드](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

```
head node> kubeadm init

[ Create a network for IP-per-Pod criteria ]

worker nodes> kubeadm join --token token head-node-IP

# You can also create the network with kubectl by using a resource manifest of the network.
# For example, to use the Weave network, you would do the following: 

> kubectl create -f https://git.io/weave-kube
```


### kubeadm upgrade
- [Upgrading kubeadm clusters](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

- `plan`
  - This will check the installed version against the newest found in the repository, and verify if the cluster can be upgraded.
- `apply`
  - Upgrades the first control plane node of the cluster to the specified version.
- `diff`
  - Similar to an apply --dry-run, this command will show the differences applied during an upgrade.
- `node`
  - This allows for updating the local kubelet configuration on worker nodes, or the control planes of other master nodes if there is more than one. Also, it will access a phase command to step through the upgrade process.

- General upgrade process:
  - Update the software
  - Check the software version
  - Drain the control plane
  - View the planned upgrade
  - Apply the upgrade
  - Uncordon the control plane to allow pods to be scheduled.


### Installing a Pod Network

- [Calico](https://www.projectcalico.org/)
- [Flannel](https://github.com/flannel-io/flannel)
- [Kube-Router](https://www.kube-router.io/)
- [Romana](https://github.com/romana/romana)
- [Weave Net](https://www.weave.works/oss/net/)


### More Installation Tools

- [kubernetes-the-hard-way](https://github.com/kelseyhightower/kubernetes-the-hard-way)

- [kubesplay](https://github.com/kubernetes-sigs/kubespray)
- [kops](https://github.com/kubernetes/kops)
- [kube-aws](https://github.com/kubernetes-retired/kube-aws)
- [kubicorn](http://kubicorn.io/)


### Installation Considerations
강의는 GCE (Google Compute Engine) 노드를 사용하여 작성



### Main Deployment Configurations
4가지 기본 배포 구성

1. `Single-node`
    - With a single-node deployment, all the components run on the same server. This is great for testing, learning, and developing around Kubernetes.
    - 모든 구성 요소가 동일한 서버에서 실행
1. `Single head node, multiple workers`
    - Adding more workers, a single head node and multiple workers typically will consist of a single node etcd instance running on the head node with the API, the scheduler, and the controller-manager.
1. `Multiple head nodes with HA, multiple workers`
    - Multiple head nodes in an HA configuration and multiple workers add more durability to the cluster. The API server will be fronted by a load balancer, the scheduler and the controller-manager will elect a leader (which is configured via flags). The etcd setup can still be single node.
    - 내구성 확보
1. `HA etcd, HA head nodes, multiple workers`
    - The most advanced and resilient setup would be an HA etcd cluster, with HA head nodes and multiple workers. Also, etcd would run as a true cluster, which would provide HA and would run on nodes separate from the Kubernetes head nodes.


### systemd Unit File for Kubernetes
이게 뭔지 설명을 봐도 잘 모르겠네요.


### Using Hyperkube
api server, scheduler, controller 등을 하나로 묶은 이미지를 만들어서 이용한다는 것 같은데...


### Compiling from Source

- [Golang 설치](https://golang.org/doc/install)

```
> cd $GOPATH

> git clone https://github.com/kubernetes/kubernetes

> cd kubernetes

> make
```

