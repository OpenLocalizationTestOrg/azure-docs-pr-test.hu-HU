---
title: "Az Azure Kubernetes Helm a tároló üzembe helyezése |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban Kubernetes fürtön tároló üzembe helyezése a Helm csomagolás eszközzel"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3cfcc5abbee03ca8fbbec4e4eae711e7c2d9deae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="d3a56-103">Helm segítségével tároló Kubernetes fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d3a56-103">Use Helm to deploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="d3a56-104">[Helm](https://github.com/kubernetes/helm/) nyílt forráskódú csomagolás eszköz, amely segít telepíteni, és Kubernetes alkalmazások életciklusának kezelését.</span><span class="sxs-lookup"><span data-stu-id="d3a56-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage the lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="d3a56-105">Linux-csomag kezelői például Apt-get és Yum hasonló, Helm segítségével Kubernetes diagramok, amelyek az előre konfigurált Kubernetes erőforrások-csomagok kezelése.</span><span class="sxs-lookup"><span data-stu-id="d3a56-105">Similar to Linux package managers such as Apt-get and Yum, Helm is used to manage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="d3a56-106">Ez a cikk bemutatja, hogyan Helm dolgozni az Azure Tárolószolgáltatásban telepített Kubernetes fürtön.</span><span class="sxs-lookup"><span data-stu-id="d3a56-106">This article shows you how to work with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="d3a56-107">Helm két részből áll:</span><span class="sxs-lookup"><span data-stu-id="d3a56-107">Helm has two components:</span></span> 
* <span data-ttu-id="d3a56-108">A **Helm CLI** helyileg vagy a felhőben a számítógépen futó ügyfél</span><span class="sxs-lookup"><span data-stu-id="d3a56-108">The **Helm CLI** is a client that runs on your machine locally or in the cloud</span></span>  

* <span data-ttu-id="d3a56-109">**Kormányrúd** , amely a Kubernetes fürtön fut, és a Kubernetes alkalmazások életciklusát felügyeli</span><span class="sxs-lookup"><span data-stu-id="d3a56-109">**Tiller** is a server that runs on the Kubernetes cluster and manages the lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="d3a56-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d3a56-110">Prerequisites</span></span>

* <span data-ttu-id="d3a56-111">[Hozzon létre egy Kubernetes fürtöt](container-service-kubernetes-walkthrough.md) az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="d3a56-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="d3a56-112">[Telepítse és konfigurálja `kubectl` ](../container-service-connect.md) a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="d3a56-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="d3a56-113">[Telepítse a Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="d3a56-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="d3a56-114">Helm alapjai</span><span class="sxs-lookup"><span data-stu-id="d3a56-114">Helm basics</span></span> 

<span data-ttu-id="d3a56-115">A Kubernetes fürt, hogy kormányrúd telepítése, és az alkalmazások telepítésével kapcsolatos információk megtekintéséhez írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3a56-115">To view information about the Kubernetes cluster that you are installing Tiller and deploying your applications to, type the following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl fürt-adatai](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="d3a56-117">Miután telepítette a Helm, kormányrúd a fürtön telepíteni a Kubernetes a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="d3a56-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing the following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="d3a56-118">Sikeresen befejeződött, a következőhöz hasonló kimenetet láthat:</span><span class="sxs-lookup"><span data-stu-id="d3a56-118">When it completes successfully, you see output like the following:</span></span>

![Kormányrúd telepítése](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="d3a56-120">A tárházban összes rendelkezésre álló Helm diagramok megtekintéséhez írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3a56-120">To view all the Helm charts available in the repository, type the following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="d3a56-121">A következőhöz hasonló kimenetnek jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d3a56-121">You see output like the following:</span></span>

![Helm keresése](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="d3a56-123">A diagramok első a legújabb verziók frissítése, írja be:</span><span class="sxs-lookup"><span data-stu-id="d3a56-123">To update the charts to get the latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="d3a56-124">Egy Nginx érkező vezérlő diagram telepítése</span><span class="sxs-lookup"><span data-stu-id="d3a56-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="d3a56-125">Egy Nginx érkező vezérlő diagram telepítéséhez írja be egy parancs:</span><span class="sxs-lookup"><span data-stu-id="d3a56-125">To deploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Érkező tartományvezérlő központi telepítése](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="d3a56-127">Ha `kubectl get svc` minden, a fürtön futó szolgáltatások megtekintése, láthatja, hogy egy IP-címet a érkező tartományvezérlő van rendelve.</span><span class="sxs-lookup"><span data-stu-id="d3a56-127">If you type `kubectl get svc` to view all services that are running on the cluster, you see that an IP address is assigned to the ingress controller.</span></span> <span data-ttu-id="d3a56-128">(A hozzárendelés folyamatban van, amíg megjelenik `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="d3a56-128">(While the assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="d3a56-129">Igénybe vehet néhány percet.)</span><span class="sxs-lookup"><span data-stu-id="d3a56-129">It takes a couple of minutes to complete.)</span></span> 

<span data-ttu-id="d3a56-130">Az IP-cím után cím hozzá van rendelve, keresse meg a futó Nginx háttér megtekintéséhez a külső IP-cím értékét.</span><span class="sxs-lookup"><span data-stu-id="d3a56-130">After the IP address is assigned, navigate to the value of the external IP address to see the Nginx backend running.</span></span> 
 
![Érkező IP-cím](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="d3a56-132">A fürtön telepítve diagramok listájának megtekintéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="d3a56-132">To see a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="d3a56-133">A parancs futtatásával rövidíthető `helm ls`.</span><span class="sxs-lookup"><span data-stu-id="d3a56-133">You can abbreviate the command to `helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="d3a56-134">A MariaDB diagram és az ügyfél telepítése</span><span class="sxs-lookup"><span data-stu-id="d3a56-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="d3a56-135">Mostantól telepítheti MariaDB diagram és az adatbázishoz való kapcsolódáshoz MariaDB ügyfél.</span><span class="sxs-lookup"><span data-stu-id="d3a56-135">Now deploy a MariaDB chart and a MariaDB client to connect to the database.</span></span>

<span data-ttu-id="d3a56-136">A MariaDB diagram telepítéséhez írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3a56-136">To deploy the MariaDB chart, type the following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="d3a56-137">Ha `--name` kiadásokban használt címke.</span><span class="sxs-lookup"><span data-stu-id="d3a56-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="d3a56-138">Ha nem sikerül a telepítés, futtassa `helm repo update` , és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="d3a56-138">If the deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="d3a56-139">A fürt telepített összes diagramok megtekintéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="d3a56-139">To view all the charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="d3a56-140">A fürtben futó összes központi telepítést, írja be:</span><span class="sxs-lookup"><span data-stu-id="d3a56-140">To view all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="d3a56-141">Végezetül futtatásához egy pod az ügyfél eléréséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="d3a56-141">Finally, to run a pod to access the client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="d3a56-142">Az ügyfél csatlakozik, írja be a következő parancsot, cseréje `v1-mariadb` nevű, a központi telepítés:</span><span class="sxs-lookup"><span data-stu-id="d3a56-142">To connect to the client, type the following command, replacing `v1-mariadb` with the name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="d3a56-143">Használhatja a szokásos SQL-parancsok létrehozása az adatbázisok, táblák, stb. Például `Create DATABASE testdb1;` létrehoz egy üres adatbázist.</span><span class="sxs-lookup"><span data-stu-id="d3a56-143">You can now use standard SQL commands to create databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="d3a56-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3a56-144">Next steps</span></span>

* <span data-ttu-id="d3a56-145">Kubernetes diagramok kezelésével kapcsolatos további információkért tekintse meg a [Helm dokumentáció](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="d3a56-145">For more information about managing Kubernetes charts, see the [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

