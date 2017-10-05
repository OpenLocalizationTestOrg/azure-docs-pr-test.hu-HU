---
title: "A figyelő Azure DC/OS-fürtről - Operations Management |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS fürtben, a Microsoft Operations Management Suite figyelése."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="3cb9b-103">Egy Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="3cb9b-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="3cb9b-104">A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="3cb9b-105">Tároló egy olyan megoldás az OMS Naplóelemzési, amely segít a tároló szoftverleltár, a teljesítmény és a naplók megtekintéséhez egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-105">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="3cb9b-106">Naplózási, tárolók hibaelhárítás a naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-106">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="3cb9b-107">A tároló megoldásról további információkért tekintse meg a [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="3cb9b-107">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-the-dcos-universe"></a><span data-ttu-id="3cb9b-108">A DC/OS universe az OMS beállítása</span><span class="sxs-lookup"><span data-stu-id="3cb9b-108">Setting up OMS from the DC/OS universe</span></span>


<span data-ttu-id="3cb9b-109">Ez a cikk feltételezi, hogy a DC/OS beállítását, és egyszerű webes tároló alkalmazást a fürtön telepített.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on the cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="3cb9b-110">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="3cb9b-110">Pre-requisite</span></span>
- <span data-ttu-id="3cb9b-111">[A Microsoft Azure-előfizetés](https://azure.microsoft.com/free/) -szabad szerezni ez.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="3cb9b-112">Microsoft OMS-munkaterület beállítása - lásd a "3. lépés" alatt</span><span class="sxs-lookup"><span data-stu-id="3cb9b-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="3cb9b-113">[DC/OS parancssori felület](https://dcos.io/docs/1.8/usage/cli/install/) telepítve.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="3cb9b-114">A DC/OS-irányítópultot kattintson a Universe, és keressen a "OMS" alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-114">In the DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="3cb9b-115">Kattintson az **Install** (Telepítés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-115">Click **Install**.</span></span> <span data-ttu-id="3cb9b-116">Látni fogja a pop be az OMS fájlverzió-információkat és egy **csomagtelepítés** vagy **speciális telepítési** gombra.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-116">You will see a pop up with the OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="3cb9b-117">Elemre **speciális telepítési**, amely vezet, hogy a **OMS konfigurációs tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-117">When you click **Advanced Installation**, which leads you to the **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="3cb9b-118">Itt kérni fogja írni a `wsid` (OMS munkaterület azonosítója) és `wskey` (OMS elsődleges kulcsát a munkaterület azonosítója).</span><span class="sxs-lookup"><span data-stu-id="3cb9b-118">Here, you will be asked to enter the `wsid` (the OMS workspace ID) and `wskey` (the OMS primary key for the workspace id).</span></span> <span data-ttu-id="3cb9b-119">Mindkét beolvasandó `wsid` és `wskey` OMS fiók létrehozásához szükséges <https://mms.microsoft.com>. Kérjük, kövesse a lépéseket egy fiók létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-119">To get both `wsid` and `wskey` you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="3cb9b-120">Ha elkészült a fiók létrehozását, be kell szereznie a `wsid` és `wskey` kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux kiszolgálók**lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-120">Once you are done creating the account, you need to obtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="3cb9b-121">A szám, OMS-példányok kiválasztása szeretne, és kattintson a "Áttekintése és telepítése" gombra.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-121">Select the number you OMS instances that you want and click the ‘Review and Install’ button.</span></span> <span data-ttu-id="3cb9b-122">Általában érdemes a virtuális gép van az ügynök fürt száma egyenlő OMS-példányok számát.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-122">Typically, you will want to have the number of OMS instances equal to the number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="3cb9b-123">Linux OMS-ügynököt telepíti minden egyes virtuális gépen, amely a figyelés és naplózás adatokat gyűjthet kíván egyedi tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-123">OMS Agent for Linux is installs as individual containers on each VM that it wants to collect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="3cb9b-124">Egy egyszerű OMS irányítópult beállítása</span><span class="sxs-lookup"><span data-stu-id="3cb9b-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="3cb9b-125">Miután telepítette az OMS-ügynököt a virtuális gépeken Linux, tovább állíthatja be az OMS-irányítópult.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-125">Once you have installed the OMS Agent for Linux on the VMs, next step is to set up the OMS dashboard.</span></span> <span data-ttu-id="3cb9b-126">Ehhez két módja van: OMS-portálon vagy az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-126">There are two ways to do this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="3cb9b-127">OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="3cb9b-127">OMS Portal</span></span> 

<span data-ttu-id="3cb9b-128">Jelentkezzen be az OMS-portálon (<https://mms.microsoft.com>), és navigáljon a **megoldás gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-128">Log in to the OMS portal (<https://mms.microsoft.com>) and go to the **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="3cb9b-129">Miután belépett a **megoldás gyűjtemény**, jelölje be **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-129">Once you are in the **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="3cb9b-130">Miután a tároló megoldás kijelölt, látni fogja a csempe az OMS áttekintése irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-130">Once you’ve selected the Container Solution, you will see the tile on the OMS Overview Dashboard page.</span></span> <span data-ttu-id="3cb9b-131">Miután a feldolgozott adatai egy indexelt, megjelenik a csempe a megoldás nézet csempék adatokkal feltöltve.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-131">Once the ingested container data is indexed, you will see the tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="3cb9b-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3cb9b-132">Azure Portal</span></span> 

<span data-ttu-id="3cb9b-133">Jelentkezzen be az Azure portálon, a <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-133">Login to Azure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="3cb9b-134">Nyissa meg a **piactér**, jelölje be **figyelés + felügyeleti** kattintson **tekintse meg az összes**.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="3cb9b-135">Írja be `containers` keresésben.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-135">Then Type `containers` in search.</span></span> <span data-ttu-id="3cb9b-136">A keresési eredmények "tárolók" jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-136">You will see "containers" in the search results.</span></span> <span data-ttu-id="3cb9b-137">Válassza ki **tárolók** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="3cb9b-138">Miután rákattintott **létrehozása**, akkor kérni fogja a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="3cb9b-139">A munkaterület kiválasztása, vagy ha nem rendelkezik ilyennel, hozzon létre egy új munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="3cb9b-140">A munkaterület kijelölése után kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="3cb9b-141">Az OMS-tároló megoldás kapcsolatos további információkért tekintse meg a [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="3cb9b-141">For more information about the OMS Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a><span data-ttu-id="3cb9b-142">Az ACS a DC/OS OMS-ügynököt méretezése</span><span class="sxs-lookup"><span data-stu-id="3cb9b-142">How to scale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="3cb9b-143">Abban az esetben kell a tényleges csomópontok száma kevés OMS-ügynököt telepítette, vagy adja hozzá a további VM skálázás be VMSS, ehhez skálázással a `msoms` szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-143">In case you need to have installed OMS agent short of the actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling the `msoms` service.</span></span>

<span data-ttu-id="3cb9b-144">Nyissa meg a Marathon vagy a DC/OS felhasználói felületének Services lapra, és növelheti a csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-144">You can either go to Marathon or the DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="3cb9b-145">Ez fog üzembe helyezni, más csomópontok, amelyek még nem telepítették az OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-145">This will deploy to other nodes which have not yet deployed the OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="3cb9b-146">MS OMS eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3cb9b-146">Uninstall MS OMS</span></span>

<span data-ttu-id="3cb9b-147">MS OMS eltávolításához adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3cb9b-147">To uninstall MS OMS enter the following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="3cb9b-148">Ossza meg velünk!!!</span><span class="sxs-lookup"><span data-stu-id="3cb9b-148">Let us know!!!</span></span>
<span data-ttu-id="3cb9b-149">Mi működik?</span><span class="sxs-lookup"><span data-stu-id="3cb9b-149">What works?</span></span> <span data-ttu-id="3cb9b-150">Mi az a hiányzó?</span><span class="sxs-lookup"><span data-stu-id="3cb9b-150">What is missing?</span></span> <span data-ttu-id="3cb9b-151">Milyen hiba van szüksége a lehet hasznos, ha Ön?</span><span class="sxs-lookup"><span data-stu-id="3cb9b-151">What else do you need for this to be useful for you?</span></span> <span data-ttu-id="3cb9b-152">Ossza meg velünk <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="3cb9b-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cb9b-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3cb9b-153">Next steps</span></span>

 <span data-ttu-id="3cb9b-154">Most, hogy beállítása OMS a tárolók figyelése[tekintse meg a tároló irányítópult](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="3cb9b-154">Now that you have set up OMS to monitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
