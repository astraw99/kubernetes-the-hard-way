# Provisioning Pod Network

We chose to use CNI - [weave](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/) as our networking option.

### Install CNI plugins

Download the CNI Plugins required for weave on each of the worker nodes - `worker-1` and `worker-2`

`wget https://github.com/containernetworking/plugins/releases/download/v1.1.1/cni-plugins-linux-amd64-v1.1.1.tgz`

Extract it to /opt/cni/bin directory

```
sudo mkdir -p /opt/cni/bin
sudo tar -xzvf cni-plugins-linux-amd64-v1.1.1.tgz  --directory /opt/cni/bin/
```

Reference: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#cni

### Deploy Weave Network

Deploy weave network. Run only once on the `master` node.

```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

Weave uses POD CIDR of `10.32.0.0/12` by default.

## Verification

List the registered Kubernetes nodes from the master node:

```
master-1$ kubectl get pods -n kube-system
```

> output

```
NAME              READY   STATUS    RESTARTS   AGE
weave-net-58j2j   2/2     Running   0          89s
weave-net-rr5dk   2/2     Running   0          89s
```

```
kubectl get no

NAME       STATUS   ROLES    AGE    VERSION
worker-1   Ready    <none>   25h    v1.25.0
worker-2   Ready    <none>   100m   v1.25.0
```

Reference: https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/weave-network-policy/#install-the-weave-net-addon

Next: [Kube API Server to Kubelet Connectivity](13-kube-apiserver-to-kubelet.md)
