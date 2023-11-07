---
title: helm包管理器
abbrlink: 50d83b7d
date: 2023-11-08 12:29:45
banner_img:
index_img: https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060053084.jpg
categories: 云原生
tags: 
  - k3s
---



# helm

是 Kubernetes 的包管理工具，Helm Chart 为 Kubernetes YAML 清单文件提供了模板语法。



## helm安装

https://github.com/helm/helm/releases

脚本安装：

```bash
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```



## helm命令

### 查看仓库

```bash
$ helm repo list
Error： no repositories to show
# 首次安装helm3 是没有指定默认仓库的。需要手动疯狂添加仓库才可以获取程序包
```

### 仓库添加

```bash
$ helm repo add stable http://mirror.azure.cn/kubernetes/charts
$ helm repo add aliyun  https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update  # 更新仓库
```

### 仓库删除

```bash
$ helm repo remove stable
$ helm repo remove aliyun
$ helm repo remove jetstack
```

### 搜索chart

```
$ helm search
$ helm list --filter 'ara[a-z]+'
```

### 安装chart

```bash
$ helm install -f myvalues.yaml myredis ./redis
# 安装参数必须是chart的引用，一个打包后的chart路径，未打包的chart目录或者是一个URL。
```

### helm卸载

```bash
$ helm uninstall
```





