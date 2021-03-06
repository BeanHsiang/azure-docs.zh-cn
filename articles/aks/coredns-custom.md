---
title: 为 Azure Kubernetes 服务 (AKS) 自定义 CoreDNS
description: 了解如何自定义 CoreDNS 添加子域或扩展 Azure Kubernetes 服务 (AKS) 中使用的自定义 DNS 终结点
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jnoller
ms.openlocfilehash: 9186c5ff7c6fbc68487a1ccff0fc1d2d1478df79
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466438"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>自定义 CoreDNS 与 Azure Kubernetes 服务

Azure Kubernetes 服务 (AKS) 使用[CoreDNS] [ coredns]适用于群集 DNS 管理和解析所有项目*1.12.x*和更高版本的群集。 以前，kube dns 项目已使用。 此 kube dns 项目现已弃用。 有关 CoreDNS 自定义和 Kubernetes 的详细信息，请参阅[上游的官方文档][corednsk8s]。

由于 AKS 是一种托管的服务，不能修改 CoreDNS 的主要配置 ( *CoreFile*)。 相反，使用 Kubernetes *configmap 装载*重写默认设置。 若要查看默认 AKS CoreDNS ConfigMaps，使用`kubectl get configmaps coredns -o yaml`命令。

本文介绍如何使用 ConfigMaps CoreDNS 在 AKS 中的基本的自定义选项。

> [!NOTE]
> `kube-dns` 提供不同[自定义选项][ kubednsblog]通过 Kubernetes 配置映射。 是 CoreDNS**不**kube dns 与向后兼容。 必须为用于 CoreDNS 更新以前使用的任何自定义。

## <a name="before-you-begin"></a>开始之前

本文假定你拥有现有的 AKS 群集。 如果需要 AKS 群集，请参阅 [使用 Azure CLI] AKS 快速入门 [aks-快速入门-cli] 或 [使用 Azure 门户] [aks 快速入门门户]。

## <a name="change-the-dns-ttl"></a>更改 DNS TTL

您可能想要在 CoreDNS 中配置的一种方案是为较低，或者引发时间 (TTL) 设置 DNS 名称缓存。 在此示例中，让我们更改了 TTL 值。 默认情况下，此值为 30 秒。 有关 DNS 缓存选项的详细信息，请参阅[官方 CoreDNS docs][dnscache]。

在下面的示例 configmap 装载，请注意`name`值。 默认情况下，CoreDNS 不支持此类型的自定义项，修改 CoreFile 本身一样。 使用 AKS *coredns 自定义*configmap 装载以合并您自己的配置和主要 CoreFile 后加载。

下面的示例指示 CoreDNS 的所有域 (所示`.`中`.:53`)，在端口 53 （默认 DNS 端口），将缓存 TTL 设置为 15 (`cache 15`)。 创建名为`coredns-custom.json`并粘贴下面的示例配置：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        cache 15  # this is our new cache value
    }
```

创建使用 ConfigMap [kubectl 应用 configmap 装载][ kubectl-apply]命令并指定 YAML 清单的名称：

```console
kubectl apply configmap coredns-custom.json
```

若要验证是否已应用自定义项，请使用[kubectl 获取 configmaps] [ kubectl-get]并指定你*coredns 自定义*configmap 装载：

```
kubectl get configmaps coredns-custom -o yaml
```

现在强制 CoreDNS 重新加载 configmap 装载。 [Kubectl 删除 pod] [ kubectl delete]命令不是破坏性，并不会导致的停机时间。 `kube-dns` Pod 被删除，并且 Kubernetes 计划程序然后重新创建它们。 这些新 pod 包含 TTL 值更改。

```console
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="rewrite-dns"></a>重写 DNS

您必须一种情况是执行实时上的 DNS 名称重写操作。 在以下示例中，将为`<domain to be written>`自己完全限定的域名。 创建名为`coredns-custom.json`并粘贴下面的示例配置：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

在上一示例中，使用，创建 configmap 装载[kubectl 应用 configmap 装载][ kubectl-apply]命令并指定 YAML 清单的名称。 然后，强制重新加载 configmap 装载使用 CoreDNS [kubectl 删除 pod] [ kubectl delete] Kubernetes 计划程序来重新创建它们：

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="custom-proxy-server"></a>自定义代理服务器

如果需要指定代理服务器的网络流量，可以创建 ConfigMap 自定义 DNS。 在以下示例中，更新`proxy`名称和地址替换为你自己的环境的值。 创建名为`coredns-custom.json`并粘贴下面的示例配置：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    .:53 {
        proxy foo.com 1.1.1.1
    }
```

与前面的示例中，使用，创建 ConfigMap [kubectl 应用 configmap 装载][ kubectl-apply]命令并指定 YAML 清单的名称。 然后，强制重新加载 configmap 装载使用 CoreDNS [kubectl 删除 pod] [ kubectl delete] Kubernetes 计划程序来重新创建它们：

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>使用自定义域

您可能想要配置自定义域仅在内部解析的。 例如，你可能想要解析自定义域*puglife.local*，这不是有效的顶级域。 如果自定义域 configmap 装载，没有 AKS 群集无法解析的地址。

在以下示例中，替换为你自己的环境的值流量定向到更新的自定义域和 IP 地址。 创建名为`coredns-custom.json`并粘贴下面的示例配置：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

与前面的示例中，使用，创建 ConfigMap [kubectl 应用 configmap 装载][ kubectl-apply]命令并指定 YAML 清单的名称。 然后，强制重新加载 configmap 装载使用 CoreDNS [kubectl 删除 pod] [ kubectl delete] Kubernetes 计划程序来重新创建它们：

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>存根 （stub） 的域

此外可以使用 CoreDNS 存根 （stub） 域配置。 在以下示例中，更新的自定义域和 IP 地址替换为你自己的环境的值。 创建名为`coredns-custom.json`并粘贴下面的示例配置：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }
```

与前面的示例中，使用，创建 ConfigMap [kubectl 应用 configmap 装载][ kubectl-apply]命令并指定 YAML 清单的名称。 然后，强制重新加载 configmap 装载使用 CoreDNS [kubectl 删除 pod] [ kubectl delete] Kubernetes 计划程序来重新创建它们：

```console
kubectl apply configmap coredns-custom.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="next-steps"></a>后续步骤

本文介绍了 CoreDNS 自定义的一些示例方案。 CoreDNS 项目的信息，请参阅[CoreDNS 上游项目页][coredns]。

若要了解有关核心网络概念的详细信息，请参阅[网络 AKS 中的应用程序的概念][concepts-network]。

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete

<!-- LINKS - external -->
[concepts-network]: concepts-network.md