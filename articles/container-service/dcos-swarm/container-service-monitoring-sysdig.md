---
title: "aaaMonitor egy Azure Tárolószolgáltatás-fürtöt Sysdig |} Microsoft Docs"
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
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="954d0-104">Azure tárolószolgáltatás-fürt megfigyelése a Sysdig segítségével</span><span class="sxs-lookup"><span data-stu-id="954d0-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="954d0-105">Ebben a cikkben fogjuk üzembe helyezni Sysdig ügynökök tooall hello ügynök a Azure Tárolószolgáltatási fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="954d0-105">In this article, we will deploy Sysdig agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="954d0-106">Ehhez a konfiguráláshoz Sysdig-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="954d0-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="954d0-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="954d0-107">Prerequisites</span></span>
<span data-ttu-id="954d0-108">[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt.</span><span class="sxs-lookup"><span data-stu-id="954d0-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="954d0-109">Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="954d0-109">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="954d0-110">Nyissa meg túl[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset Sysdig felhő fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="954d0-110">Go too[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="954d0-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="954d0-111">Sysdig</span></span>
<span data-ttu-id="954d0-112">Sysdig egy figyelési szolgáltatása lehetővé teszi toomonitor a tárolók a fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="954d0-112">Sysdig is a monitoring service that allows you toomonitor your containers within your cluster.</span></span> <span data-ttu-id="954d0-113">Sysdig ismert toohelp kaptak, de a alapvető figyelési metrikákat, a CPU, a hálózat, a memória és az i/o is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="954d0-113">Sysdig is known toohelp with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="954d0-114">Sysdig teszi, hogy mely tárolók dolgozik könnyen toosee hello segítségével hardest vagy lényegében hello legtöbb memóriát és CPU.</span><span class="sxs-lookup"><span data-stu-id="954d0-114">Sysdig makes it easy toosee which containers are working hello hardest or essentially using hello most memory and CPU.</span></span> <span data-ttu-id="954d0-115">Ez a nézet hello "Overview" szakaszban, amely jelenleg bétaverziójú van.</span><span class="sxs-lookup"><span data-stu-id="954d0-115">This view is in hello “Overview” section, which is currently in beta.</span></span> 

![Sysdig felhasználói felület](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="954d0-117">Üzemelő Sysdig-példány konfigurálása a Marathonnal</span><span class="sxs-lookup"><span data-stu-id="954d0-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="954d0-118">Ezeket a lépéseket bemutatja, hogyan tooconfigure és Sysdig alkalmazások tooyour fürtben a marathon segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="954d0-118">These steps will show you how tooconfigure and deploy Sysdig applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="954d0-119">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/) egyszer hello DC/OS felhasználói felületének lépjen a toohello "Universe", amelyek a hello bal alsó, és keressen a "Sysdig."</span><span class="sxs-lookup"><span data-stu-id="954d0-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate toohello "Universe", which is on hello bottom left and then search for "Sysdig."</span></span>

![Sysdig a DC/OS Universe rendszerben](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="954d0-121">Most toocomplete hello konfigurációs egy Sysdig van szüksége a fiók vagy egy ingyenes próbafiók felhő.</span><span class="sxs-lookup"><span data-stu-id="954d0-121">Now toocomplete hello configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="954d0-122">Miután toohello Sysdig felhő webhelyen jelentkezett, kattintson a felhasználónevére, és hello oldalon kell megjelennie a "hozzáférési kulcsot."</span><span class="sxs-lookup"><span data-stu-id="954d0-122">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![Sysdig API-kulcs](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="954d0-124">A hozzáférési kulcsot következő kezdenek hello Sysdig konfigurációs hello DC/OS Universe belül.</span><span class="sxs-lookup"><span data-stu-id="954d0-124">Next enter your Access Key into hello Sysdig configuration within hello DC/OS Universe.</span></span> 

![A DC/OS Universe hello Sysdig konfiguráció](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="954d0-126">Most már készen hello példányok too10000000, amikor egy új csomópontot hozzáadják toohello fürt Sysdig automatikusan telepíteni fogja az ügynök toothat új csomópont.</span><span class="sxs-lookup"><span data-stu-id="954d0-126">Now set hello instances too10000000 so whenever a new node is added toohello cluster Sysdig will automatically deploy an agent toothat new node.</span></span> <span data-ttu-id="954d0-127">Ez az egy átmeneti megoldás toomake meg arról, hogy Sysdig telepíti az új ügynökök tooall hello fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="954d0-127">This is an interim solution toomake sure Sysdig will deploy tooall new agents within hello cluster.</span></span> 

![A DC/OS Universe-példányokat hello Sysdig konfiguráció](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="954d0-129">Hello csomag telepítése után nyissa meg a visszafelé toohello Sysdig felhasználói felület, és be fog tudni tooexplore hello különböző a szoftverhasználati mérési adatok az hello tárolókat a fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="954d0-129">Once you've installed hello package navigate back toohello Sysdig UI and you'll be able tooexplore hello different usage metrics for hello containers within your cluster.</span></span> 

<span data-ttu-id="954d0-130">Telepíthet kimondottan a Mesoshoz és a Marathonhoz készült irányítópultokat is az [új irányítópult varázsló](https://app.sysdigcloud.com/#/dashboards/new) segítségével.</span><span class="sxs-lookup"><span data-stu-id="954d0-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
