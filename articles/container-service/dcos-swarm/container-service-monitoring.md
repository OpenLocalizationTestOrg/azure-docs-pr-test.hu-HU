---
title: "A figyelő Azure DC/OS-fürtről - Datadog |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatás-fürt Datadog a figyelheti. A DC/OS webes felhasználói felület segítségével a Datadog ügynökök telepítésére a fürthöz."
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
ms.openlocfilehash: 9dd451f994940d7cc3a59bd7fd08a8f067345e34
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="92427-105">Egy Azure tároló szolgáltatás DC/OS fürt Datadog figyelése</span><span class="sxs-lookup"><span data-stu-id="92427-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="92427-106">Ebben a cikkben azt Datadog ügynökök telepíti az Azure Tárolószolgáltatás-fürt minden ügynök csomópontján.</span><span class="sxs-lookup"><span data-stu-id="92427-106">In this article we will deploy Datadog agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="92427-107">Ehhez a konfigurációhoz szüksége lesz egy fiókra Datadog.</span><span class="sxs-lookup"><span data-stu-id="92427-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="92427-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92427-108">Prerequisites</span></span>
<span data-ttu-id="92427-109">[Helyezzen üzembe](container-service-deployment.md) és [csatlakoztasson](../container-service-connect.md) egy, az Azure Container Service által konfigurált fürtöt.</span><span class="sxs-lookup"><span data-stu-id="92427-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="92427-110">Ismerkedjen meg a [Marathon felhasználói felülettel](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="92427-110">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="92427-111">Ugrás a [http://datadoghq.com](http://datadoghq.com) egy Datadog fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="92427-111">Go to [http://datadoghq.com](http://datadoghq.com) to set up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="92427-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="92427-112">Datadog</span></span>
<span data-ttu-id="92427-113">Datadog egy figyelési szolgáltatás, amely a figyelési adatokat gyűjt a tárolók skálázása az Azure Tárolószolgáltatás-fürt belül.</span><span class="sxs-lookup"><span data-stu-id="92427-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="92427-114">Datadog rendelkezik egy Docker integrációs irányítópultot, ahol láthatja adott mérőszámok belül a tárolók.</span><span class="sxs-lookup"><span data-stu-id="92427-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="92427-115">A tárolók gyűjtött metrikák Processzor, memória, hálózati és i/o szerint vannak rendszerezve.</span><span class="sxs-lookup"><span data-stu-id="92427-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="92427-116">Datadog metrikák felosztja a tárolók és a képeket.</span><span class="sxs-lookup"><span data-stu-id="92427-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="92427-117">Egy példa a felhasználói felület néz a CPU-használat nem éri el.</span><span class="sxs-lookup"><span data-stu-id="92427-117">An example of what the UI looks like for CPU usage is below.</span></span>

![Datadog felhasználói felület](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="92427-119">A marathon segítségével Datadog telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="92427-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="92427-120">Ezeket a lépéseket bemutatja, hogyan konfigurálhatja és telepítheti a fürtben a marathon segítségével Datadog alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="92427-120">These steps will show you how to configure and deploy Datadog applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="92427-121">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="92427-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="92427-122">Egyszer a DC/OS felhasználói felületének keresse meg a "Universe" amelyek a bal alsó részén, majd keresse meg a "Datadog", majd kattintson a "Telepítés".</span><span class="sxs-lookup"><span data-stu-id="92427-122">Once in the DC/OS UI navigate to the "Universe" which is on the bottom left and then search for "Datadog" and click "Install."</span></span>

![A DC/OS Universe belül Datadog csomag](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="92427-124">Most már a konfiguráció befejezéséhez szüksége lesz egy Datadog fiók vagy egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="92427-124">Now to complete the configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="92427-125">Amennyiben balra Datadog webhely megjelenését jelentkezett, és integrációk Ugrás -> majd [API-k](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="92427-125">Once you're logged in to the Datadog website look to the left and go to Integrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Datadog API-kulcs](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="92427-127">Ezután adjon meg az API-kulcs a DC/OS Universe belül Datadog konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="92427-127">Next enter your API key into the Datadog configuration within the DC/OS Universe.</span></span> 

![A DC/OS Universe Datadog konfiguráció](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="92427-129">A fenti konfigurációban példányok 10000000 értékre van beállítva, ha új csomópontot a fürthöz hozzáadott Datadog automatikusan telepíteni fogja az ügynök ahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="92427-129">In the above configuration instances are set to 10000000 so whenever a new node is added to the cluster Datadog will automatically deploy an agent to that node.</span></span> <span data-ttu-id="92427-130">Ez az egy átmeneti megoldás.</span><span class="sxs-lookup"><span data-stu-id="92427-130">This is an interim solution.</span></span> <span data-ttu-id="92427-131">Miután telepítette a csomag kell keresse meg a Datadog webhelyre, és keresse "[irányítópultok](https://app.datadoghq.com/dash/list)."</span><span class="sxs-lookup"><span data-stu-id="92427-131">Once you've installed the package you should navigate back to the Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="92427-132">Ott látni fogja a egyéni és integrációs irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="92427-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="92427-133">A [Docker irányítópult](https://app.datadoghq.com/screen/integration/docker) lesz szüksége a fürt figyelési tároló metrikákat.</span><span class="sxs-lookup"><span data-stu-id="92427-133">The [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all the container metrics you need for monitoring your cluster.</span></span> 

