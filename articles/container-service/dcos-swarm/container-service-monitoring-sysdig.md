---
title: "Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével | Microsoft Docs"
description: "Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: e61001161e632a5d2e513107e30f1eaf06103989
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="2eaab-104">Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével</span><span class="sxs-lookup"><span data-stu-id="2eaab-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="2eaab-105">Ebben a cikkben Sysdig-ügynököket telepítünk az Azure Container Service-fürt összes ügynökcsomópontjára.</span><span class="sxs-lookup"><span data-stu-id="2eaab-105">In this article, we will deploy Sysdig agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="2eaab-106">Ehhez a konfiguráláshoz Sysdig-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2eaab-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2eaab-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2eaab-107">Prerequisites</span></span>
<span data-ttu-id="2eaab-108">[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt.</span><span class="sxs-lookup"><span data-stu-id="2eaab-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="2eaab-109">Ismerkedjen meg a [Marathon felhasználói felülettel](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="2eaab-109">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="2eaab-110">Látogasson el a [http://app.sysdigcloud.com](http://app.sysdigcloud.com) címre egy Sysdig-felhőfiók beállításához.</span><span class="sxs-lookup"><span data-stu-id="2eaab-110">Go to [http://app.sysdigcloud.com](http://app.sysdigcloud.com) to set up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="2eaab-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="2eaab-111">Sysdig</span></span>
<span data-ttu-id="2eaab-112">A Sysdig egy olyan megfigyelő szolgáltatás, amelynek a segítségével megfigyelheti a fürtben található tárolókat.</span><span class="sxs-lookup"><span data-stu-id="2eaab-112">Sysdig is a monitoring service that allows you to monitor your containers within your cluster.</span></span> <span data-ttu-id="2eaab-113">A Sysdig a hibaelhárításban nyújtott segítségről ismert, de a processzor, a hálózatkezelés, a memória és az I/O megfigyelésére alkalmas alapvető mérőszámokkal is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2eaab-113">Sysdig is known to help with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="2eaab-114">A Sysdig megkönnyíti annak áttekintését, hogy mely tárolók működnek a legtöbbet, és melyek használják leginkább a processzort, illetve a legtöbb memóriát.</span><span class="sxs-lookup"><span data-stu-id="2eaab-114">Sysdig makes it easy to see which containers are working the hardest or essentially using the most memory and CPU.</span></span> <span data-ttu-id="2eaab-115">Ez a nézet az „Overview” (Áttekintés) részben található, amelynek jelenleg még csak a bétaverziója érhető el.</span><span class="sxs-lookup"><span data-stu-id="2eaab-115">This view is in the “Overview” section, which is currently in beta.</span></span> 

![Sysdig felhasználói felület](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="2eaab-117">Üzemelő Sysdig-példány konfigurálása a Marathonnal</span><span class="sxs-lookup"><span data-stu-id="2eaab-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="2eaab-118">Ezek a lépések bemutatják, hogyan konfigurálhat és helyezhet üzembe Sysdig-alkalmazásokat a fürtön a Marathonnal.</span><span class="sxs-lookup"><span data-stu-id="2eaab-118">These steps will show you how to configure and deploy Sysdig applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="2eaab-119">Nyissa meg a DC/OS felhasználói felületét a [http://localhost:80/](http://localhost:80/) címen keresztül. A DC/OS felhasználói felületen navigáljon a „Universe” elemre, amely a bal alsó részen található, majd keressen rá a „Sysdig” kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="2eaab-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in the DC/OS UI navigate to the "Universe", which is on the bottom left and then search for "Sysdig."</span></span>

![Sysdig a DC/OS Universe rendszerben](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="2eaab-121">A konfiguráció befejezéséhez Sysdig-felhőfiókra vagy ingyenes próbafiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2eaab-121">Now to complete the configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="2eaab-122">Miután bejelentkezett a Sysdig felhő webhelyére, kattintson a felhasználónevére, és az oldalon meg kell jelennie a hívóbetűjének („Access Key”).</span><span class="sxs-lookup"><span data-stu-id="2eaab-122">Once you're logged in to the Sysdig cloud website, click on your user name, and on the page you should see your "Access Key."</span></span> 

![Sysdig API-kulcs](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="2eaab-124">Ezután írja be a hívóbetűt a DC/OS Universe rendszerben található Sysdig-konfigurációba.</span><span class="sxs-lookup"><span data-stu-id="2eaab-124">Next enter your Access Key into the Sysdig configuration within the DC/OS Universe.</span></span> 

![Sysdig-konfiguráció a DC/OS Universe rendszerben](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="2eaab-126">Most állítsa a példányszámot 10000000 értékre, hogy amikor új csomópontot ad a fürthöz, a Sysdig mindig automatikusan üzembe helyezzen egy ügynököt az új csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="2eaab-126">Now set the instances to 10000000 so whenever a new node is added to the cluster Sysdig will automatically deploy an agent to that new node.</span></span> <span data-ttu-id="2eaab-127">Ez egy ideiglenes megoldás, hogy a Sysdig a fürtben található összes új ügynöknél üzembe legyen helyezve.</span><span class="sxs-lookup"><span data-stu-id="2eaab-127">This is an interim solution to make sure Sysdig will deploy to all new agents within the cluster.</span></span> 

![Sysdig-konfiguráció a DC/OS Universe-példányokban](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="2eaab-129">Miután telepítette a csomagot, lépjen vissza a Sysdig felhasználói felületére, ahol áttekintheti a fürtben lévő tárolók különböző használati mérőszámait.</span><span class="sxs-lookup"><span data-stu-id="2eaab-129">Once you've installed the package navigate back to the Sysdig UI and you'll be able to explore the different usage metrics for the containers within your cluster.</span></span> 

<span data-ttu-id="2eaab-130">Telepíthet kimondottan a Mesoshoz és a Marathonhoz készült irányítópultokat is az [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new) segítségével.</span><span class="sxs-lookup"><span data-stu-id="2eaab-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
