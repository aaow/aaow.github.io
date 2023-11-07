---
title: helm包管理器
date: 2023-11-08 13:35:26
banner_img:
index_img: 
https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060053084.jpg
categories: 云原生
tags: 
  - k3s

---

# helm包管理器

[Helm](http://helm.sh/) 是一个 Kubernetes 应用的包管理工具，用来管理 [chart](https://github.com/helm/charts)——预先配置好的安装包资源

Helm 和 chart 的主要作用是：

- 应用程序封装
- 版本管理
- 依赖检查
- 便于应用程序分发



# 安装helm

官方文档 https://helm.sh/docs/intro/install/

```bash
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

# 仓库

## 查看仓库

```bash
$ helm repo list
NAME    URL
stable  https://kubernetes-charts.storage.googleapis.com/
local   http://127.0.0.1:8879/charts
```

## 更新

```bash
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## 镜像

```bash
$ helm repo add stable  https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts/
# 阿里云
$ helm repo add stable http://mirror.azure.cn/kubernetes/charts/
$ helm repo add incubator http://mirror.azure.cn/kubernetes/charts-incubator/￥
```

## 删除仓库

```bash
$ helm repo remove stable
"stable" has been removed from your repositories
```

# chart

## 查找chart

```bash
$ helm search
NAME                                    CHART VERSION   APP VERSION                     DESCRIPTION
...
stable/minio                            1.6.3           RELEASE.2018-08-25T01-56-38Z    Minio is a high performance distributed object storage se...
stable/mission-control                  0.4.2           3.1.2                           A Helm chart for JFrog Mission Control
stable/mongodb                          4.2.2           4.0.2                           NoSQL document-oriented database that stores JSON-like do...
stable/mongodb-replicaset               3.5.6           3.6                             NoSQL document-oriented database that stores JSON-like do...
...
stable/zetcd                            0.1.9           0.0.3                           CoreOS zetcd Helm chart for Kubernetes
...
```

## 安装chart

```bash
$ helm install stable/mysql
# 指定文件
$ helm install -f myvalues.yaml myredis ./redis
# 指定变量
$ helm install --set name=prod myredis ./redis
# 指定变量的值为 string 类型
$ helm install --set-string long_int=1234567890 myredis ./redis
# 指定引用的文件地址
$ helm install --set-file my_script=dothings.sh myredis ./redis
# 同时指定多个变量
$ helm install --set foo=bar --set foo=newbar  myredis ./redis
```

## 升级和回滚 chart

要想升级 chart 可以修改本地的 chart 配置并执行

```bash
$ helm upgrade [RELEASE] [CHART] [flags]
```

使用 `helm ls` 的命令查看当前运行的 chart 的 release 版本，并使用下面的命令回滚到历史版本：

```bash
$ helm rollback <RELEASE> [REVISION] [flags]
```

## 卸载chart

```bash
$ helm uninstall RELEASE_NAME [...] [flags]
```

