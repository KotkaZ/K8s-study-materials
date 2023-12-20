# 06 Sep -- Setting up and configuring personal learning environment

# 03 Sep -- Containerization technologies

# 20 Sep -- Setting up and configuring Kubernetes

## Terminology

There is a set of terms that Kubernetes basically redefines, and some new ones. As a way to prevent confusion, here's a list of the worst offenders together with most used definitions in Kubernetes:

- Pod - group of one or more containers that share a network. Lowest tier of workloads in Kubernetes.
- CP or control plane - administrative Kubernetes layer that runs the most important Kubernetes components.
- worker - a compute unit that doesn't run any control plane components.
- Node - in Kubernetes, a node is a physical or virtual machine that can run a workload. It doesn't matter if it's control plane or worker.
- cluster - in Kubernetes context, this means a set of nodes (at least 1 CP), that have been aggregated into the same failure and trust domain by the Kubernetes components.
- CNI - Container Network Interface. A standard that facilitates communication between container run-times and network plugins. Often also used to mean the networking software that uses the standard to provide networking for the Kubernetes cluster.
- Service - A Kubernetes resource object that defines a logical set of Pods and a policy to access them.
- Namespace - A Kubernetes resource object that allows for the segregation of cluster resources between multiple users or environments.
- Ingress - in Kubernetes context, usually means the Kubernetes resource object Ingress, which defines IngressController routes.
- API server - in Kubernetes context, this usually means the Kubernetes control plane component called kube-apiserver.
- Labels and Selectors - in Kubernetes context, allow for organizing and selecting subsets of objects based on key-value pairs.
- kubectl - a command-line tool for interacting with Kubernetes clusters.
- scheduler - in Kubernetes context, this usually means the Kubernetes control plane component called scheduler.

To start using your cluster, you need to run the following as a regular user:

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Alternatively, if you are the root user, you can run:

```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

The basic operations include, but are not limited by:

- kubectl get - get an object in the API, for example kubectl get nodes.
- kubectl describe - describe an object and their metadata in the API, for example kubectl describe nodes.
- kubectl apply - apply a resource manifest into the API, for example kubectl apply -f manifest.yaml.
- kubectl delete - delete a resource from the API, for example kubectl delete -f manifest.yaml.
- kubectl edit - edit an existing resource in the API, for example kubectl edit service/kubernetes.

--token f4bxay.80plbcfuemz38f00 --discovery-token-ca-cert-hash sha256:9313b21c7eb01bef11ae9e5c3cb57734d95bf61f6429259fc45f42a46fee1687

```
---
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: "systemd"
shutdownGracePeriod: 5m0s
shutdownGracePeriodCriticalPods: 5m0s
---
apiVersion: kubeadm.k8s.io/v1beta3
caCertPath: /etc/kubernetes/pki/ca.crt
discovery:
  bootstrapToken:
    caCertHashes:
    - sha256:9313b21c7eb01bef11ae9e5c3cb57734d95bf61f6429259fc45f42a46fee1687
    apiServerEndpoint: 172.17.89.65:6443
    token: nlg4b7.m1cdmddqf1j4ecie
  timeout: 5m0s
kind: JoinConfiguration
nodeRegistration:
  criSocket: unix:///run/containerd/containerd.sock
  imagePullPolicy: IfNotPresent
  name: rapur-worker.cloud.ut.ee
  taints: null
```

```
kubectl get pods -o wide
```

```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

# 27 Sep Workloads, Configuration and Secrets management

# 04 Oct Services, Ingress, LB, Networking.

# 11 Oct Storage (& database management in Kubernetes)

```
5e4b7370abd92a6ed6b441a3edccfe0dcd319790f9a7c9d1e38a8936793c0b70b015c01c145cf47e3765cd36d7a784c29b8aca07a227f61639f6bc8869b483a2
```

# 18 Oct Monitoring, logging, tracing

# 25 Oct Consultation session. You can ask for help with previous labs

# 01 Nov Container lifecycle managment, Helm

# 08 Nov Continuous Integration & Deployment, CI/CD in Kubernetes

# 15 Nov Designing cloud native applications

# 22 Nov Cluster administration

# 29 Nov Security

# 6 Dec HPA
