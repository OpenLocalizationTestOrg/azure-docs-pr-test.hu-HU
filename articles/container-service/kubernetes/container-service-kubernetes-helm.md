---
title: "az Azure Kubernetes Helm aaaDeploy tárolók |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban Kubernetes fürtön hello Helm csomagolás eszköz toodeploy tárolók használata"
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
ms.openlocfilehash: c7bd780afe00084ebe4e3a14873e1e340a29d144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="c8bbc-103">Kubernetes fürt Helm toodeploy tárolók használata</span><span class="sxs-lookup"><span data-stu-id="c8bbc-103">Use Helm toodeploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="c8bbc-104">[Helm](https://github.com/kubernetes/helm/) egy nyílt forráskódú csomagolás eszköz, amelynek segítségével telepítheti és kezelheti a Kubernetes alkalmazások hello életciklusát.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage hello lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="c8bbc-105">Hasonló tooLinux csomag kezelők például Apt-get és Yum Helm használt toomanage Kubernetes diagramok, amelyek az előre konfigurált Kubernetes erőforrások csomagok.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-105">Similar tooLinux package managers such as Apt-get and Yum, Helm is used toomanage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="c8bbc-106">Ez a cikk bemutatja, hogyan Kubernetes fürtön a Helm toowork Azure Tárolószolgáltatás telepítve.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-106">This article shows you how toowork with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="c8bbc-107">Helm két részből áll:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-107">Helm has two components:</span></span> 
* <span data-ttu-id="c8bbc-108">Hello **Helm CLI** helyileg vagy hello felhőben található a számítógépen futó ügyfél</span><span class="sxs-lookup"><span data-stu-id="c8bbc-108">hello **Helm CLI** is a client that runs on your machine locally or in hello cloud</span></span>  

* <span data-ttu-id="c8bbc-109">**Kormányrúd** , amely hello Kubernetes fürtön fut, és a Kubernetes alkalmazások hello életciklusát felügyeli</span><span class="sxs-lookup"><span data-stu-id="c8bbc-109">**Tiller** is a server that runs on hello Kubernetes cluster and manages hello lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="c8bbc-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8bbc-110">Prerequisites</span></span>

* <span data-ttu-id="c8bbc-111">[Hozzon létre egy Kubernetes fürtöt](container-service-kubernetes-walkthrough.md) az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="c8bbc-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="c8bbc-112">[Telepítse és konfigurálja `kubectl` ](../container-service-connect.md) a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="c8bbc-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="c8bbc-113">[Telepítse a Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) a helyi számítógépen</span><span class="sxs-lookup"><span data-stu-id="c8bbc-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="c8bbc-114">Helm alapjai</span><span class="sxs-lookup"><span data-stu-id="c8bbc-114">Helm basics</span></span> 

<span data-ttu-id="c8bbc-115">tooview információ hello Kubernetes fürt, hogy kormányrúd telepítése, és az alkalmazások telepítése, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-115">tooview information about hello Kubernetes cluster that you are installing Tiller and deploying your applications to, type hello following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl fürt-adatai](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="c8bbc-117">Miután telepítette a Helm, kormányrúd a fürtön telepíteni a Kubernetes hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing hello following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="c8bbc-118">Ha sikeres a végrehajtása, hasonló hello kimenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-118">When it completes successfully, you see output like hello following:</span></span>

![Kormányrúd telepítése](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="c8bbc-120">tooview hello Helm diagramjainak elérhető hello tárház, a következő típus hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-120">tooview all hello Helm charts available in hello repository, type hello following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="c8bbc-121">Hello következő kimenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-121">You see output like hello following:</span></span>

![Helm keresése](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="c8bbc-123">tooupdate hello diagramok tooget hello legújabb verziója, írja be:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-123">tooupdate hello charts tooget hello latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="c8bbc-124">Egy Nginx érkező vezérlő diagram telepítése</span><span class="sxs-lookup"><span data-stu-id="c8bbc-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="c8bbc-125">egy Nginx érkező vezérlő diagram toodeploy egy parancs írja be:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-125">toodeploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Érkező tartományvezérlő központi telepítése](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="c8bbc-127">Ha `kubectl get svc` tooview hello fürtön futó összes szolgáltatásokat, láthatja, hogy az IP-cím toohello érkező tartományvezérlő van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-127">If you type `kubectl get svc` tooview all services that are running on hello cluster, you see that an IP address is assigned toohello ingress controller.</span></span> <span data-ttu-id="c8bbc-128">(Hello hozzárendelés folyamatban van, amíg megjelenik `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-128">(While hello assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="c8bbc-129">Igénybe vehet néhány percet toocomplete.)</span><span class="sxs-lookup"><span data-stu-id="c8bbc-129">It takes a couple of minutes toocomplete.)</span></span> 

<span data-ttu-id="c8bbc-130">Után hello IP-cím van hozzárendelve, keresse meg a hello külső IP cím toosee hello Nginx háttér futtató toohello értékét.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-130">After hello IP address is assigned, navigate toohello value of hello external IP address toosee hello Nginx backend running.</span></span> 
 
![Érkező IP-cím](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="c8bbc-132">toosee diagramok listája telepítve a fürtben, típus:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-132">toosee a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="c8bbc-133">Hello parancs túl rövidíthető`helm ls`.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-133">You can abbreviate hello command too`helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="c8bbc-134">A MariaDB diagram és az ügyfél telepítése</span><span class="sxs-lookup"><span data-stu-id="c8bbc-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="c8bbc-135">Mostantól telepítheti MariaDB diagram és MariaDB ügyfél tooconnect toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-135">Now deploy a MariaDB chart and a MariaDB client tooconnect toohello database.</span></span>

<span data-ttu-id="c8bbc-136">toodeploy hello MariaDB diagram, a következő parancs típusa hello:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-136">toodeploy hello MariaDB chart, type hello following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="c8bbc-137">Ha `--name` kiadásokban használt címke.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="c8bbc-138">Ha hello telepítése sikertelen, futtassa `helm repo update` , és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-138">If hello deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="c8bbc-139">tooview a fürtben, típus regisztrálva diagramjainak hello:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-139">tooview all hello charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="c8bbc-140">tooview írja be a fürtben futó összes központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-140">tooview all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="c8bbc-141">Végezetül toorun pod tooaccess hello ügyfél, írja be:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-141">Finally, toorun a pod tooaccess hello client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="c8bbc-142">tooconnect toohello ügyfél, a következő parancsot, hogy a típus hello `v1-mariadb` hello nevet, a központi telepítés:</span><span class="sxs-lookup"><span data-stu-id="c8bbc-142">tooconnect toohello client, type hello following command, replacing `v1-mariadb` with hello name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="c8bbc-143">Most már használhatja a szokásos SQL-parancsok toocreate adatbázisok, táblák, stb. Például `Create DATABASE testdb1;` létrehoz egy üres adatbázist.</span><span class="sxs-lookup"><span data-stu-id="c8bbc-143">You can now use standard SQL commands toocreate databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="c8bbc-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8bbc-144">Next steps</span></span>

* <span data-ttu-id="c8bbc-145">Kubernetes diagramok kezelésével kapcsolatos további információkért lásd: hello [Helm dokumentáció](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="c8bbc-145">For more information about managing Kubernetes charts, see hello [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

