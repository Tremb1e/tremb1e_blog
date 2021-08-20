# 如何部署 k8s 

环境: Centos8 , docker-18.09.0 , k8s-1.15.1

## 配置 k8s 的 yum 源

在 master 节点和 node 节点都要执行

```
$ cat << EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

```

```
[root@tremb1e ~]# cat << EOF > /etc/yum.repos.d/kubernetes.repo
> [kubernetes]
> name=Kubernetes
> baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
> enabled=1
> gpgcheck=0
> repo_gpgcheck=0
> gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
>        http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
> EOF
```

## 关闭防火墙、 selinux 、 swap 

```
$ setenforce 0
$ sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
$ swapoff -a
$ yes | cp /etc/fstab /etc/fstab_bak
$ cat /etc/fstab_bak |grep -v swap > /etc/fstab
```

## 修改 /etc/sysctl.conf 

在 master 节点和 node 节点都要执行

```
$ vim /etc/sysctl.conf
```

在其中添加

```
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
```

在 master 节点和 node 节点都要执行

```
$ sysctl -p
```

## 安装 kubelet 、 kubeadm 、 kubectl 

在 master 节点和 node 节点都要执行

```
$ yum install -y kubelet-1.15.1 kubeadm-1.15.1 kubectl-1.15.1
```

## 修改 docker Cgroup Driver 为 systemd 

如果不修改，在添加 node 节点时可能会碰到如下错误

```
[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". 
Please follow the guide at https://kubernetes.io/docs/setup/cri/
```

在 master 节点和 node 节点都要执行

```
$ vim /usr/lib/systemd/system/docker.service
```

在其中添加

```
--exec-opt native.cgroupdriver=system
```

## 重启 docker ，并启动 kubelet 

在 master 节点和 node 节点都要执行

```
$ systemctl daemon-reload
$ systemctl restart docker
$ systemctl enable kubelet && systemctl start kubelet
```

## 初始化 master 节点

### 配置域名（根据自身的公网或内网IP修改）

```
$ echo "192.168.159.146  master.k8s.com" >> /etc/hosts
$ echo "192.168.159.146  k8s-master" >> /etc/hosts
```

```
hostnamectl set-hostname k8s-master
```

### 创建 kubeadm-config.yaml

```
$ cat << EOF > ./kubeadm-config.yaml
apiVersion: kubeadm.k8s.io/v1beta1
kind: ClusterConfiguration
kubernetesVersion: v1.15.1
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers
controlPlaneEndpoint: "master.k8s.com:6443"
networking:
  podSubnet: "10.100.0.1/20"
EOF
```

### 初始化 k8s 

只在 master 节点运行

```
$ kubeadm init --config=kubeadm-config.yaml --upload-certs
```

```
You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join master.k8s.com:6443 --token 637lys.e9bjoo33sw4apn27 \
    --discovery-token-ca-cert-hash sha256:65e8525f1e109eced9a020ff80dc0ca95e18aa88ed4c1dd3907327e81be37868 \
    --control-plane --certificate-key 480d8c578b0be2d446d4e653f6933ededef3bc6978cc4e43990f2c5204d1d87e

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use 
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join master.k8s.com:6443 --token 637lys.e9bjoo33sw4apn27 \
    --discovery-token-ca-cert-hash sha256:65e8525f1e109eced9a020ff80dc0ca95e18aa88ed4c1dd3907327e81be37868
```

node 节点复制 join 命令即可加入 k8s 集群中

### 初始化 root 用户的 kubectl 配置

只在 master 节点运行

```
$ rm -rf /root/.kube/
$ mkdir /root/.kube/
$ cp -i /etc/kubernetes/admin.conf /root/.kube/config
```

### 安装 flannel 

在 master 节点和 node 节点都要执行

#### 新建 flannel 配置文件

```
$ vim kube-flannel.yml
```

#### 将此文件内容复制进去

[https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml](https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml)

#### 下载 flannel 镜像

```
$ cat kube-flannel.yml | grep image
```

```
[root@k8s-master ]# cat kube-flannel.yml | grep image
        image: quay.io/coreos/flannel:v0.14.0-rc1
        image: quay.io/coreos/flannel:v0.14.0-rc1
```

```
$ docker pull quay.io/coreos/flannel:v0.14.0-rc1
```

## k8s 重启

### 关闭 k8s 

```
$ kubeadm reset
```

### 再启动 k8s 

```
$ kubeadm init --config=kubeadm-config.yaml --upload-certs
```

### 清理旧文件

```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

不清理旧文件会报错

```
[root@k8s-master ~]# kubectl get nodes
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
```
