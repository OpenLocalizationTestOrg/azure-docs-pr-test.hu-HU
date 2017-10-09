---
title: "aaaMonitor Azure DC/OS-fürtről - Datadog |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatás-fürt Datadog a figyelheti. Hello DC/OS webes felhasználói felület toodeploy hello Datadog ügynökök tooyour-fürt használatára."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, a DC/OS, a Docker Swarm Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="cfdfa-105">Egy Azure tároló szolgáltatás DC/OS fürt Datadog figyelése</span><span class="sxs-lookup"><span data-stu-id="cfdfa-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="cfdfa-106">Ebben a cikkben fogjuk üzembe helyezni Datadog ügynökök tooall hello ügynök a Azure Tárolószolgáltatási fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-106">In this article we will deploy Datadog agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="cfdfa-107">Ehhez a konfigurációhoz szüksége lesz egy fiókra Datadog.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cfdfa-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cfdfa-108">Prerequisites</span></span>
<span data-ttu-id="cfdfa-109">[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="cfdfa-110">Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="cfdfa-110">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="cfdfa-111">Nyissa meg túl[http://datadoghq.com](http://datadoghq.com) tooset Datadog fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-111">Go too[http://datadoghq.com](http://datadoghq.com) tooset up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="cfdfa-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="cfdfa-112">Datadog</span></span>
<span data-ttu-id="cfdfa-113">Datadog egy figyelési szolgáltatás, amely a figyelési adatokat gyűjt a tárolók skálázása az Azure Tárolószolgáltatás-fürt belül.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="cfdfa-114">Datadog rendelkezik egy Docker integrációs irányítópultot, ahol láthatja adott mérőszámok belül a tárolók.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="cfdfa-115">A tárolók gyűjtött metrikák Processzor, memória, hálózati és i/o szerint vannak rendszerezve.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="cfdfa-116">Datadog metrikák felosztja a tárolók és a képeket.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="cfdfa-117">Milyen hello például felhasználói felület tűnik processzor kihasználtsága nem éri el.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-117">An example of what hello UI looks like for CPU usage is below.</span></span>

![Datadog felhasználói felület](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="cfdfa-119">A marathon segítségével Datadog telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cfdfa-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="cfdfa-120">Ezeket a lépéseket bemutatja, hogyan tooconfigure és Datadog alkalmazások tooyour fürtben a marathon segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-120">These steps will show you how tooconfigure and deploy Datadog applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="cfdfa-121">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="cfdfa-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="cfdfa-122">Egyszer hello DC/OS felhasználói felületének keresse meg a "Universe", amelyek a hello toohello bal alsó, majd keresse meg a "Datadog", majd kattintson "Telepítés".</span><span class="sxs-lookup"><span data-stu-id="cfdfa-122">Once in hello DC/OS UI navigate toohello "Universe" which is on hello bottom left and then search for "Datadog" and click "Install."</span></span>

![A DC/OS Universe hello belül Datadog csomag](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="cfdfa-124">Most toocomplete hello konfigurációs szüksége lesz egy Datadog fiók vagy egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-124">Now toocomplete hello configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="cfdfa-125">Miután jelentkezett toohello Datadog webhelyen keresse meg a toohello balra, és nyissa meg tooIntegrations majd -> [API-k](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="cfdfa-125">Once you're logged in toohello Datadog website look toohello left and go tooIntegrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Datadog API-kulcs](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="cfdfa-127">Ezután adjon meg az API-kulcs hello Datadog konfigurációs hello DC/OS Universe belül.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-127">Next enter your API key into hello Datadog configuration within hello DC/OS Universe.</span></span> 

![A DC/OS Universe hello Datadog konfiguráció](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="cfdfa-129">A fenti konfigurációs hello példányok úgy van beállítva, too10000000 amikor egy új csomópontot hozzáadják toohello fürt Datadog automatikusan telepíteni fogja az ügynök toothat csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-129">In hello above configuration instances are set too10000000 so whenever a new node is added toohello cluster Datadog will automatically deploy an agent toothat node.</span></span> <span data-ttu-id="cfdfa-130">Ez az egy átmeneti megoldás.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-130">This is an interim solution.</span></span> <span data-ttu-id="cfdfa-131">Hello csomag telepítése után, nyissa meg a visszafelé toohello Datadog webhelyet, és található "[irányítópultok](https://app.datadoghq.com/dash/list)."</span><span class="sxs-lookup"><span data-stu-id="cfdfa-131">Once you've installed hello package you should navigate back toohello Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="cfdfa-132">Ott látni fogja a egyéni és integrációs irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="cfdfa-133">Hello [Docker irányítópult](https://app.datadoghq.com/screen/integration/docker) lesz szüksége figyelését a fürt összes hello tároló metrikát.</span><span class="sxs-lookup"><span data-stu-id="cfdfa-133">hello [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all hello container metrics you need for monitoring your cluster.</span></span> 

