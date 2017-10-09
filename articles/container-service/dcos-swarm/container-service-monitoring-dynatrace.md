---
title: "aaaMonitor Azure DC/OS-fürtről - Dynatrace |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS fürtben Dynatrace a figyelheti. Hello Dynatrace OneAgent hello DC/OS Irányítópult segítségével telepítheti."
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
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="85ce1-105">Egy Azure tároló szolgáltatás DC/OS fürtben a Dynatrace SaaS/felügyelt figyelése</span><span class="sxs-lookup"><span data-stu-id="85ce1-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="85ce1-106">Ez a cikk azt mutatja be toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor összes hello ügynök az Azure Tárolószolgáltatás-fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="85ce1-106">In this article, we show you how toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor all hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="85ce1-107">A Dynatrace SaaS/felügyelt fiók ehhez a konfigurációhoz szükség van.</span><span class="sxs-lookup"><span data-stu-id="85ce1-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="85ce1-108">Dynatrace SaaS/felügyelt</span><span class="sxs-lookup"><span data-stu-id="85ce1-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="85ce1-109">Dynatrace egy olyan felhőalapú natív figyelési megoldás magas dinamikus tároló és a fürt környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="85ce1-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="85ce1-110">Lehetővé teszi toobetter optimalizálhatja a üzemelő tárolópéldányokat és memória-kiosztásokat valós idejű használati adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="85ce1-110">It allows you toobetter optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="85ce1-111">Is képes automatikusan felügyelő alkalmazás és az infrastruktúra-problémák automatikus viszonyítási probléma korrelációs és kiváltó okának észlelési megadásával.</span><span class="sxs-lookup"><span data-stu-id="85ce1-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="85ce1-112">hello következő ábra azt mutatja be hello Dynatrace felhasználói felület:</span><span class="sxs-lookup"><span data-stu-id="85ce1-112">hello following figure shows hello Dynatrace UI:</span></span>

![Dynatrace felhasználói felület](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="85ce1-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="85ce1-114">Prerequisites</span></span> 
<span data-ttu-id="85ce1-115">[Telepítése](container-service-deployment.md) és [csatlakozás](./../container-service-connect.md) állította be az Azure Tárolószolgáltatás tooa fürt.</span><span class="sxs-lookup"><span data-stu-id="85ce1-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) tooa cluster configured by Azure Container Service.</span></span> <span data-ttu-id="85ce1-116">Fedezze fel hello [Marathon felhasználói felület](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="85ce1-116">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="85ce1-117">Nyissa meg túl[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset Dynatrace Szolgáltatottszoftver-fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="85ce1-117">Go too[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="85ce1-118">A marathon segítségével Dynatrace telepítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85ce1-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="85ce1-119">Ezeket a lépéseket mutatja be tooconfigure és Dynatrace alkalmazások tooyour fürtben a marathon segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="85ce1-119">These steps show you how tooconfigure and deploy Dynatrace applications tooyour cluster with Marathon.</span></span>

1. <span data-ttu-id="85ce1-120">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="85ce1-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="85ce1-121">Egyszer hello DC/OS felhasználói felületének, lépjen a toohello **Universe** fülre, majd keresse meg a **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="85ce1-121">Once in hello DC/OS UI, navigate toohello **Universe** tab and then search for **Dynatrace**.</span></span>

    ![A DC/OS Universe Dynatrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="85ce1-123">toocomplete hello konfigurálása van szüksége egy Dynatrace Szolgáltatottszoftver-fiók vagy egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="85ce1-123">toocomplete hello configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="85ce1-124">Után jelentkezzen be a hello Dynatrace irányítópultot, válassza a **telepítése Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="85ce1-124">Once you log into hello Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![A PaaS integrációs Dynatrace beállítása](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="85ce1-126">Hello lapon jelölje be **PaaS-integráció beállítása**.</span><span class="sxs-lookup"><span data-stu-id="85ce1-126">On hello page, select **Set up PaaS integration**.</span></span> 

    ![Dynatrace API jogkivonat](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="85ce1-128">Adjon meg az API-token hello Dynatrace OneAgent konfigurációs hello DC/OS Universe belül.</span><span class="sxs-lookup"><span data-stu-id="85ce1-128">Enter your API token into hello Dynatrace OneAgent configuration within hello DC/OS Universe.</span></span> 

    ![A DC/OS Universe hello Dynatrace OneAgent konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="85ce1-130">Beállítása hello példányok toohello csomópontok toorun szeretné.</span><span class="sxs-lookup"><span data-stu-id="85ce1-130">Set hello instances toohello number of nodes you intend toorun.</span></span> <span data-ttu-id="85ce1-131">Nagyobb értékre is működik, de a DC/OS tovább próbálkozik toofind új példányok eléréséig, hogy a kívánt ténylegesen.</span><span class="sxs-lookup"><span data-stu-id="85ce1-131">Setting a higher number also works, but DC/OS will keep trying toofind new instances until that number is actually reached.</span></span> <span data-ttu-id="85ce1-132">Ha szeretné, például 1000000 tooa érték is beállíthat.</span><span class="sxs-lookup"><span data-stu-id="85ce1-132">If you prefer, you can also set this tooa value like 1000000.</span></span> <span data-ttu-id="85ce1-133">Ebben az esetben amikor egy új csomópont hozzáadása toohello fürt Dynatrace automatikusan telepíti a az ügynök toothat új csomópont, a DC/OS folyamatosan próbált toodeploy további példányokat hello áron.</span><span class="sxs-lookup"><span data-stu-id="85ce1-133">In this case, whenever a new node is added toohello cluster, Dynatrace automatically deploys an agent toothat new node, at hello price of DC/OS constantly trying toodeploy further instances.</span></span>

    ![A DC/OS Universe-példányokat hello Dynatrace konfiguráció](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="85ce1-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85ce1-135">Next steps</span></span>

<span data-ttu-id="85ce1-136">Hello csomag telepítése után nyissa meg a visszafelé toohello Dynatrace irányítópult.</span><span class="sxs-lookup"><span data-stu-id="85ce1-136">Once you've installed hello package, navigate back toohello Dynatrace dashboard.</span></span> <span data-ttu-id="85ce1-137">Ismerje meg a különböző a szoftverhasználati mérési adatok hello az hello tárolókat a fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="85ce1-137">You can explore hello different usage metrics for hello containers within your cluster.</span></span> 
