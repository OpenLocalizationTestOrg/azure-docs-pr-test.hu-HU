---
title: "egy Azure Kubernetes aaaMonitor fürt CoScale |} Microsoft Docs"
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
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="c8d08-103">Egy Azure-tároló szolgáltatás Kubernetes fürt CoScale figyelése</span><span class="sxs-lookup"><span data-stu-id="c8d08-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="c8d08-104">Ez a cikk azt mutatja be toodeploy hello [CoScale](https://www.coscale.com/) az Azure Tárolószolgáltatásban fürt minden csomópontján és a tárolók a Kubernetes ügynök toomonitor.</span><span class="sxs-lookup"><span data-stu-id="c8d08-104">In this article, we show you how toodeploy hello [CoScale](https://www.coscale.com/) agent toomonitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="c8d08-105">Ez a konfiguráció CoScale rendelkező fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8d08-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="c8d08-106">CoScale kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="c8d08-106">About CoScale</span></span> 

<span data-ttu-id="c8d08-107">CoScale egy felügyeleti platform, metrikákkal és eseményekkel gyűjt az összes tároló több vezénylési platformokon.</span><span class="sxs-lookup"><span data-stu-id="c8d08-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="c8d08-108">CoScale kínál a teljes verem Kubernetes környezetek figyelése.</span><span class="sxs-lookup"><span data-stu-id="c8d08-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="c8d08-109">Képi megjelenítések és az elemzések biztosít az összes rétegének hello verem: hello az operációs rendszer, Kubernetes, Docker és a tárolók belül futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c8d08-109">It provides visualizations and analytics for all layers in hello stack: hello OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="c8d08-110">CoScale kínál számos beépített figyelési irányítópultok, és beépített anomáliadetektálási észlelési tooallow operátorok rendelkezik, és a fejlesztők toofind infrastruktúra és az alkalmazás problémák gyors.</span><span class="sxs-lookup"><span data-stu-id="c8d08-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection tooallow operators and developers toofind infrastructure and application issues fast.</span></span>

![Felhasználói felület coScale](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="c8d08-112">Amint ez a cikk, ügynökök telepíthető egy Kubernetes fürt toorun CoScale, mint a Szolgáltatottszoftver-megoldás.</span><span class="sxs-lookup"><span data-stu-id="c8d08-112">As shown in this article, you can install agents on a Kubernetes cluster toorun CoScale as a SaaS solution.</span></span> <span data-ttu-id="c8d08-113">Ha azt szeretné, tookeep az adatok helyszíni, CoScale is rendelkezésre áll a helyszíni telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="c8d08-113">If you want tookeep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c8d08-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8d08-114">Prerequisites</span></span>

<span data-ttu-id="c8d08-115">Először túl[CoScale-fiók létrehozása](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="c8d08-115">You first need too[create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="c8d08-116">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="c8d08-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="c8d08-117">Azt is feltételezi, hogy rendelkezik-e hello `az` Azure CLI és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="c8d08-117">It also assumes that you have hello `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="c8d08-118">Ha hello tesztelheti `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="c8d08-118">You can test if you have hello `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="c8d08-119">Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c8d08-119">If you don't have hello `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="c8d08-120">Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="c8d08-120">You can test if you have hello `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="c8d08-121">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="c8d08-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a><span data-ttu-id="c8d08-122">Egy DaemonSet hello CoScale ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="c8d08-122">Installing hello CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="c8d08-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) rendszer által használt Kubernetes toorun a tároló egyetlen példány hello fürt minden állomáson.</span><span class="sxs-lookup"><span data-stu-id="c8d08-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="c8d08-124">Fontosságúak tökéletes megoldás például hello CoScale ügynök figyelőügynökök futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c8d08-124">They're perfect for running monitoring agents such as hello CoScale agent.</span></span>

<span data-ttu-id="c8d08-125">Miután tooCoScale a bejelentkezést, lépjen a toohello [lap](https://app.coscale.com/) tooinstall CoScale ügynököt a fürt egy DaemonSet használatával.</span><span class="sxs-lookup"><span data-stu-id="c8d08-125">After you log in tooCoScale, go toohello [agent page](https://app.coscale.com/) tooinstall CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="c8d08-126">hello CoScale felhasználói felület interaktív konfigurációs lépéseket toocreate biztosít, az ügynök és a figyelő a teljes Kubernetes fürt indításának.</span><span class="sxs-lookup"><span data-stu-id="c8d08-126">hello CoScale UI provides guided configuration steps toocreate an agent and start monitoring your complete Kubernetes cluster.</span></span>

![CoScale ügynök konfigurálása](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="c8d08-128">toostart hello ügynök hello fürtön, futtassa a megadott hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="c8d08-128">toostart hello agent on hello cluster, run hello supplied command:</span></span>

![Indítsa el a hello CoScale ügynök](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="c8d08-130">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="c8d08-130">That's it!</span></span> <span data-ttu-id="c8d08-131">Ha hello ügynökök megfelelően működik, akkor jelenik meg: hello adatai néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="c8d08-131">Once hello agents are up and running, you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="c8d08-132">A Microsoft hello [lap](https://app.coscale.com/) toosee összegzését a fürt további konfigurációs lépések, és ellenőrizze, például hello irányítópultok **Kubernetes fürt áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="c8d08-132">Visit hello [agent page](https://app.coscale.com/) toosee a summary of your cluster, perform additional configuration steps, and see dashboards such as hello **Kubernetes cluster overview**.</span></span>

![Kubernetes fürtök – áttekintés](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="c8d08-134">hello CoScale ügynök automatikusan új gépek hello fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="c8d08-134">hello CoScale agent is automatically deployed on new machines in hello cluster.</span></span> <span data-ttu-id="c8d08-135">hello ügynök frissítések automatikus verziójának kiadásakor.</span><span class="sxs-lookup"><span data-stu-id="c8d08-135">hello agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c8d08-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8d08-136">Next steps</span></span>

<span data-ttu-id="c8d08-137">Lásd: hello [dokumentáció CoScale](http://docs.coscale.com/) és [blog](https://www.coscale.com/blog) figyelési megoldásoknak CoScale további információt.</span><span class="sxs-lookup"><span data-stu-id="c8d08-137">See hello [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

