---
title: "Egy Azure Kubernetes fürt CoScale figyelése |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban CoScale használatával Kubernetes fürt figyelése"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f894191baced710fc0f5a8c8692df98033341a48
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="3fbce-103">Egy Azure-tároló szolgáltatás Kubernetes fürt CoScale figyelése</span><span class="sxs-lookup"><span data-stu-id="3fbce-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="3fbce-104">Ebben a cikkben azt megmutatja, hogyan telepítheti a [CoScale](https://www.coscale.com/) agent figyelje az összes csomópontot és az Azure Tárolószolgáltatásban Kubernetes fürt a tárolók.</span><span class="sxs-lookup"><span data-stu-id="3fbce-104">In this article, we show you how to deploy the [CoScale](https://www.coscale.com/) agent to monitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="3fbce-105">Ez a konfiguráció CoScale rendelkező fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="3fbce-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="3fbce-106">CoScale kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="3fbce-106">About CoScale</span></span> 

<span data-ttu-id="3fbce-107">CoScale egy felügyeleti platform, metrikákkal és eseményekkel gyűjt az összes tároló több vezénylési platformokon.</span><span class="sxs-lookup"><span data-stu-id="3fbce-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="3fbce-108">CoScale kínál a teljes verem Kubernetes környezetek figyelése.</span><span class="sxs-lookup"><span data-stu-id="3fbce-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="3fbce-109">Képi megjelenítések és az elemzések biztosít az összes rétegének a veremben: az operációs rendszer, Kubernetes, Docker és a tárolók belül futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3fbce-109">It provides visualizations and analytics for all layers in the stack: the OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="3fbce-110">CoScale kínál számos beépített figyelési irányítópultok, és nem rendelkezik, ahhoz, hogy operátorok és a fejlesztők gyors található infrastruktúra és az alkalmazás számos beépített anomáliadetektálás.</span><span class="sxs-lookup"><span data-stu-id="3fbce-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection to allow operators and developers to find infrastructure and application issues fast.</span></span>

![Felhasználói felület coScale](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="3fbce-112">Amint ez a cikk, a ügynökök való futtatásra a Szolgáltatottszoftver-megoldás CoScale Kubernetes fürtön is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3fbce-112">As shown in this article, you can install agents on a Kubernetes cluster to run CoScale as a SaaS solution.</span></span> <span data-ttu-id="3fbce-113">Ha meg szeretné tartani az adatokat a helyszíni, CoScale is rendelkezésre áll a helyszíni telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="3fbce-113">If you want to keep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="3fbce-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3fbce-114">Prerequisites</span></span>

<span data-ttu-id="3fbce-115">Először [CoScale-fiók létrehozása](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="3fbce-115">You first need to [create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="3fbce-116">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3fbce-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="3fbce-117">Azt is feltételezi, hogy a `az` Azure CLI és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="3fbce-117">It also assumes that you have the `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="3fbce-118">Ha tesztelheti a `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="3fbce-118">You can test if you have the `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="3fbce-119">Ha nem rendelkezik a `az` eszköz telepítve, az e-mail utasításokat is [Itt](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="3fbce-119">If you don't have the `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="3fbce-120">Ha tesztelheti a `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="3fbce-120">You can test if you have the `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="3fbce-121">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="3fbce-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a><span data-ttu-id="3fbce-122">Egy DaemonSet a CoScale ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="3fbce-122">Installing the CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="3fbce-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) a tároló egyetlen példány futhat az a fürt minden egyes állomás Kubernetes használják.</span><span class="sxs-lookup"><span data-stu-id="3fbce-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="3fbce-124">Fontosságúak tökéletes választás, például a CoScale ügynök figyelőügynökök futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3fbce-124">They're perfect for running monitoring agents such as the CoScale agent.</span></span>

<span data-ttu-id="3fbce-125">Miután CoScale jelentkezik be, lépjen a [lap](https://app.coscale.com/) CoScale ügynökök telepítése a fürt egy DaemonSet használatával.</span><span class="sxs-lookup"><span data-stu-id="3fbce-125">After you log in to CoScale, go to the [agent page](https://app.coscale.com/) to install CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="3fbce-126">A CoScale felhasználói felület lépéseit az interaktív konfigurációs hozzon létre egy ügynök és a teljes Kubernetes fürt figyelni.</span><span class="sxs-lookup"><span data-stu-id="3fbce-126">The CoScale UI provides guided configuration steps to create an agent and start monitoring your complete Kubernetes cluster.</span></span>

![CoScale ügynök konfigurálása](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="3fbce-128">A megadott parancsot az ügynököt a fürt elindításához:</span><span class="sxs-lookup"><span data-stu-id="3fbce-128">To start the agent on the cluster, run the supplied command:</span></span>

![Indítsa el a CoScale ügynök](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="3fbce-130">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="3fbce-130">That's it!</span></span> <span data-ttu-id="3fbce-131">Ha az ügynök működik és elérhető, meg kell jelennie a konzol adatainak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3fbce-131">Once the agents are up and running, you should see data in the console in a few minutes.</span></span> <span data-ttu-id="3fbce-132">Látogasson el a [lap](https://app.coscale.com/) tekintse meg a fürt összegzését, további konfigurálásra, és tekintse meg az irányítópultok, mint a **Kubernetes fürt áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="3fbce-132">Visit the [agent page](https://app.coscale.com/) to see a summary of your cluster, perform additional configuration steps, and see dashboards such as the **Kubernetes cluster overview**.</span></span>

![Kubernetes fürtök – áttekintés](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="3fbce-134">A CoScale ügynök automatikusan új gépek a fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="3fbce-134">The CoScale agent is automatically deployed on new machines in the cluster.</span></span> <span data-ttu-id="3fbce-135">Az ügynök a frissítések automatikus verziójának kiadásakor.</span><span class="sxs-lookup"><span data-stu-id="3fbce-135">The agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3fbce-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fbce-136">Next steps</span></span>

<span data-ttu-id="3fbce-137">Tekintse meg a [dokumentáció CoScale](http://docs.coscale.com/) és [blog](https://www.coscale.com/blog) figyelési megoldásoknak CoScale további információt.</span><span class="sxs-lookup"><span data-stu-id="3fbce-137">See the [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

