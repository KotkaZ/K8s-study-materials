yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

dnf install -y containerd.io

systemctl start containerd
systemctl enable containerd

dnf install -y nfs-utils
dnf install -y iscsi-initiator-utils
dnf install -y jq

dnf install -y iptables
curl -L https://github.com/containernetworking/plugins/releases/download/v1.3.0/cni-plugins-linux-amd64-v1.3.0.tgz --output cni-plugins-linux-amd64-v1.3.0.tgz
mkdir -p /opt/cni/bin/
tar xvfx cni-plugins-linux-amd64-v1.3.0.tgz -C /opt/cni/bin/

containerd config default | sudo tee /etc/containerd/config.toml
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

systemctl restart containerd

timedatectl set-timezone UTC

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
iscsi_tcp
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
sudo modprobe iscsi_tcp

systemctl start iscsid
systemctl enable iscsid

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system

sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.27/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.27/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF

dnf makecache

dnf install -y kubelet-1.28.4-150500.1.1.x86_64 kubeadm-1.28.4-150500.1.1.x86_64 kubectl-1.28.4-150500.1.1.x86_64 --disableexcludes=kubernetes

systemctl start kubelet
systemctl enable kubelet
