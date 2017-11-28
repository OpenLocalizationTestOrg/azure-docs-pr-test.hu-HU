---
title: "A figyelő Azure DC/OS-fürtről - Dynatrace |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS fürtben Dynatrace a figyelheti. A DC/OS Irányítópult segítségével telepítheti a Dynatrace OneAgent."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 6fa23728680779e33eda7bb9aa8a01b9cad9a82b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="73f85-105">Egy Azure tároló szolgáltatás DC/OS fürtben a Dynatrace SaaS/felügyelt figyelése</span><span class="sxs-lookup"><span data-stu-id="73f85-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="73f85-106">Ebben a cikkben azt megmutatja, hogyan telepítheti a [Dynatrace](https://www.dynatrace.com/) OneAgent figyelése az Azure Tárolószolgáltatás-fürt minden ügynök csomópontján.</span><span class="sxs-lookup"><span data-stu-id="73f85-106">In this article, we show you how to deploy the [Dynatrace](https://www.dynatrace.com/) OneAgent to monitor all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="73f85-107">A Dynatrace SaaS/felügyelt fiók ehhez a konfigurációhoz szükség van.</span><span class="sxs-lookup"><span data-stu-id="73f85-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="73f85-108">Dynatrace SaaS/felügyelt</span><span class="sxs-lookup"><span data-stu-id="73f85-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="73f85-109">Dynatrace egy olyan felhőalapú natív figyelési megoldás magas dinamikus tároló és a fürt környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="73f85-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="73f85-110">Ez lehetővé teszi, hogy jobban optimalizálhatja a üzemelő tárolópéldányokat és memória-kiosztásokat valós idejű használati adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="73f85-110">It allows you to better optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="73f85-111">Is képes automatikusan felügyelő alkalmazás és az infrastruktúra-problémák automatikus viszonyítási probléma korrelációs és kiváltó okának észlelési megadásával.</span><span class="sxs-lookup"><span data-stu-id="73f85-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="73f85-112">Az alábbi ábrán láthatók a Dynatrace felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="73f85-112">The following figure shows the Dynatrace UI:</span></span>

![Dynatrace felhasználói felület](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="73f85-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73f85-114">Prerequisites</span></span> 
<span data-ttu-id="73f85-115">[Telepítése](container-service-deployment.md) és [csatlakozás](./../container-service-connect.md) állította be Azure Tárolószolgáltatási fürthöz.</span><span class="sxs-lookup"><span data-stu-id="73f85-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) to a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="73f85-116">Ismerkedjen meg a [Marathon felhasználói felülettel](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="73f85-116">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="73f85-117">Ugrás a [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) Dynatrace Szolgáltatottszoftver-fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="73f85-117">Go to [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) to set up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="73f85-118">A marathon segítségével Dynatrace telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="73f85-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="73f85-119">Ezeket a lépéseket megmutatja, hogyan konfigurálja és alkalmazza őket a marathon segítségével a fürt Dynatrace alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="73f85-119">These steps show you how to configure and deploy Dynatrace applications to your cluster with Marathon.</span></span>

1. <span data-ttu-id="73f85-120">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="73f85-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="73f85-121">Egyszer a DC/OS felhasználói felületének, keresse meg a **Universe** fülre, majd keresse meg a **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="73f85-121">Once in the DC/OS UI, navigate to the **Universe** tab and then search for **Dynatrace**.</span></span>

    ![A DC/OS Universe Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="73f85-123">A konfigurálás befejezéséhez szüksége egy Dynatrace Szolgáltatottszoftver-fiók vagy egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="73f85-123">To complete the configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="73f85-124">Miután bejelentkezik a Dynatrace irányítópultot, válassza ki a **telepítése Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="73f85-124">Once you log into the Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![A PaaS integrációs Dynatrace beállítása](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="73f85-126">A lapon válassza ki a **PaaS-integráció beállítása**.</span><span class="sxs-lookup"><span data-stu-id="73f85-126">On the page, select **Set up PaaS integration**.</span></span> 

    ![Dynatrace API jogkivonat](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="73f85-128">Adjon meg az API-token belül a DC/OS Universe Dynatrace OneAgent konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="73f85-128">Enter your API token into the Dynatrace OneAgent configuration within the DC/OS Universe.</span></span> 

    ![A DC/OS Universe Dynatrace OneAgent konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="73f85-130">A példányok állítsa a futtatni kívánt csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="73f85-130">Set the instances to the number of nodes you intend to run.</span></span> <span data-ttu-id="73f85-131">Nagyobb értékre is működik, de a DC/OS új példányok kereséséhez, amíg el nem, hogy a kívánt ténylegesen tovább próbálkozik.</span><span class="sxs-lookup"><span data-stu-id="73f85-131">Setting a higher number also works, but DC/OS will keep trying to find new instances until that number is actually reached.</span></span> <span data-ttu-id="73f85-132">Ha szeretné, akkor is állíthatja ezt például 1000000 értékre.</span><span class="sxs-lookup"><span data-stu-id="73f85-132">If you prefer, you can also set this to a value like 1000000.</span></span> <span data-ttu-id="73f85-133">Ebben az esetben ha új csomópontot a fürthöz hozzáadott, Dynatrace automatikusan telepíti a az ügynök új csomóponton, a DC/OS folyamatosan további példányok telepítésének megkísérlése áron.</span><span class="sxs-lookup"><span data-stu-id="73f85-133">In this case, whenever a new node is added to the cluster, Dynatrace automatically deploys an agent to that new node, at the price of DC/OS constantly trying to deploy further instances.</span></span>

    ![A DC/OS Universe-példányok Dynatrace konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="73f85-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73f85-135">Next steps</span></span>

<span data-ttu-id="73f85-136">Ha a csomag telepítése, lépjen vissza a Dynatrace irányítópult.</span><span class="sxs-lookup"><span data-stu-id="73f85-136">Once you've installed the package, navigate back to the Dynatrace dashboard.</span></span> <span data-ttu-id="73f85-137">Ismerje meg a másik a szoftverhasználati mérési adatok a tárolók a fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="73f85-137">You can explore the different usage metrics for the containers within your cluster.</span></span> 