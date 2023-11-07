---
title: k3s安装
date: 2023-10-27 19:06:04
banner_img: https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060053084.jpg
index_img:
categories: 云原生
tags:
- k3s

---

# K3S安装

{% note success %}

Kubernetes 是一个可移植、可扩展的开源平台，用于管理容器化的工作负载和服务，方便进行声明式配置和自动化。因为ks间长度为8，也叫k8s。

{% endnote %}

## k3s是轻量级的 Kubernetes

精简了很多 K8s 组件，非常适合在性能较弱的机器上运行，支持 IoT 设备，支持 arm 架构，很适合在 vps 上轻量部署。

![](https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060048731.svg)

> 我们希望安装的 Kubernetes 只占用一半的内存。Kubernetes 是一个 10 个字母的单词，简写为 K8s。Kubernetes 的一半就是一个 5 个字母的单词，因此简写为 K3s。K3s 没有全称，也没有官方的发音。

- Server 节点指的是运行 `k3s server` 命令的主机，control plane 和数据存储组件由 K3s 管理。

- Agent 节点指的是运行 `k3s agent` 命令的主机，不具有任何数据存储或 control plane 组件。

- Server 和 Agent 都运行 kubelet、容器运行时和 CNI。有关运行无 Agent 的 Server 的更多信息，请参阅[高级选项](https://docs.k3s.io/zh/advanced#运行无-agent-的-server实验性)。

  



## 安装脚本

国内安装：

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
```

默认配置文件`/etc/rancher/k3s/k3s.yaml`，由 K3s 安装的 kubectl 将自动使用该文件。

添加Agent节点，在节点机器安装：

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -
```

`K3S_TOKEN`在server节点`var/lib/rancher/k3s/server/node-token`中。

## 仓库镜像配置

在`/etc/rancher/k3s/` 中是创建 `registries.yaml`

```yaml
mirrors:
  mycustomreg.com:
    endpoint:
      - "https://mycustomreg.com:5000"
```

如果没有配置安全证书改用`http`，每个节点都要配置。



## 检查节点

`kubectl`是Kubernetes 提供使用 Kubernetes API 与 Kubernetes 集群的[控制面](https://kubernetes.io/zh-cn/docs/reference/glossary/?all=true#term-control-plane)进行通信的命令行工具。

```bash
$ kubectl get nodes
NAME           STATUS   ROLES                       AGE     VERSION
k3s-server-1   Ready    etcd                        5h39m   v1.20.4+k3s1
k3s-server-2   Ready    control-plane,master        5h39m   v1.20.4+k3s1
$ kubectl delete node <node-name>
```

## 如果找不到节点

```bash
couldn't get current server API group list: Get "https://127.0.0.1:6443/api?timeout=32s": tls: failed to verify certificate: x509: certificate signed by unknown authority

```

给配置文件权限，或者复制出来：

```bash
$ sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```

修改`k3s.yaml`文件远程访问

```bash
server: https://10.10.10.10:6443
```

设置`.bashrc`默认启动：

```bash
export KUBECONFIG=~/.kube/config/k3s.yaml
```

## 分配work角色

```bash
kubectl label node ${node} node-role.kubernetes.io/worker=worker

```



## 更改启动参数

修改`/etc/systemd/system/k3s.service`

```bash
ExecStart=/usr/local/bin/k3s \
    server \
        '--cluster-init' \
        '--tls-san=192.168.10.10' \
```

## 重启命令

重启Server，

```bash
systemctl restart k3s 
```

重启Agent，

```bash
systemctl restart k3s-agent
```



## 高可用集群

{% note success %}

具有嵌入式 etcd 的 HA K3s 集群由以下部分组成：

- 三个或多个 **Server 节点**为 Kubernetes API 提供服务并运行其他 control plane 服务，以及托管嵌入式 etcd 数据存储。
- 可选：零个或多个 **Agent 节点**，用于运行你的应用和服务
- 可选：**固定注册地址**，供 Agent 节点注册到集群

{% endsuccess %}

首先启动一个带有 `cluster-init` 标志的 Server 节点来启用集群和一个令牌

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --cluster-init \
    --tls-san=<FIXED_IP> #固定注册地址
```

使用共享 secret 将第二台和第三台服务器加入集群：

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --server https://<ip or hostname of server1>:6443 \
    --tls-san=<FIXED_IP> #固定注册地址
```

加入两个Agent：

```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - agent --server https://<ip or hostname of server>:6443
```



## 卸载k3s

卸载Server，运行

```bash
sh /usr/local/bin/k3s-uninstall.sh
```

卸载Agent，运行

```bash
sh /usr/local/bin/k3s-agent-uninstall.sh
```

## 参考

> [K3S](https://docs.k3s.io/zh/)
