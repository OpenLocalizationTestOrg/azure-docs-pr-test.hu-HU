---
title: "aaaMonitor Azure DC/OS-fürtről - műveletek kezelése |} Microsoft Docs"
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
ms.openlocfilehash: 0ebfa3ba3cef8f0205b15731b0e91f5b304bc8fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="14e6e-103">Egy Azure tároló szolgáltatás DC/OS fürtben, az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="14e6e-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="14e6e-104">A Microsoft Operations Management Suite (OMS) a Microsoft felhőalapú informatikai felügyeleti megoldása, amely segít a helyszíni és a felhőalapú infrastruktúra kezelésében és védelmében.</span><span class="sxs-lookup"><span data-stu-id="14e6e-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="14e6e-105">Tároló egy olyan megoldás, az OMS szolgáltatáshoz, így a nézet hello tároló szoftverleltár, a teljesítmény és a naplók egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="14e6e-105">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="14e6e-106">Naplózási, tárolók hibaelhárításáról hello naplók megtekintése központi helyen, és zajos fel felesleges tároló egy gazdagépen található.</span><span class="sxs-lookup"><span data-stu-id="14e6e-106">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="14e6e-107">A tároló megoldásról további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="14e6e-107">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-hello-dcos-universe"></a><span data-ttu-id="14e6e-108">A DC/OS universe hello OMS beállítása</span><span class="sxs-lookup"><span data-stu-id="14e6e-108">Setting up OMS from hello DC/OS universe</span></span>


<span data-ttu-id="14e6e-109">Ez a cikk feltételezi, hogy a DC/OS beállítását, és egyszerű webes tároló alkalmazások hello fürtön telepített.</span><span class="sxs-lookup"><span data-stu-id="14e6e-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on hello cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="14e6e-110">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="14e6e-110">Pre-requisite</span></span>
- <span data-ttu-id="14e6e-111">[A Microsoft Azure-előfizetés](https://azure.microsoft.com/free/) -szabad szerezni ez.</span><span class="sxs-lookup"><span data-stu-id="14e6e-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="14e6e-112">Microsoft OMS-munkaterület beállítása - lásd a "3. lépés" alatt</span><span class="sxs-lookup"><span data-stu-id="14e6e-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="14e6e-113">[DC/OS parancssori felület](https://dcos.io/docs/1.8/usage/cli/install/) telepítve.</span><span class="sxs-lookup"><span data-stu-id="14e6e-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="14e6e-114">Hello DC/OS-irányítópultot kattintson a Universe, és keressen a "OMS" alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="14e6e-114">In hello DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="14e6e-115">Kattintson az **Install** (Telepítés) gombra.</span><span class="sxs-lookup"><span data-stu-id="14e6e-115">Click **Install**.</span></span> <span data-ttu-id="14e6e-116">Látni fogja a pop mentése hello OMS fájlverzió-információkat, és egy **csomagtelepítés** vagy **speciális telepítési** gombra.</span><span class="sxs-lookup"><span data-stu-id="14e6e-116">You will see a pop up with hello OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="14e6e-117">Amikor rákattint **speciális telepítési**, amely részletes útmutatást toohello **OMS konfigurációs tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="14e6e-117">When you click **Advanced Installation**, which leads you toohello **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="14e6e-118">Itt, meg kell adnia tooenter hello `wsid` (hello OMS-munkaterület azonosítója) és `wskey` (hello OMS elsődleges kulcsát hello munkaterület azonosítója).</span><span class="sxs-lookup"><span data-stu-id="14e6e-118">Here, you will be asked tooenter hello `wsid` (hello OMS workspace ID) and `wskey` (hello OMS primary key for hello workspace id).</span></span> <span data-ttu-id="14e6e-119">mindkét tooget `wsid` és `wskey` OMS fiók kell toocreate <https://mms.microsoft.com>. Kövesse az hello lépéseket toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="14e6e-119">tooget both `wsid` and `wskey` you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="14e6e-120">Miután létrehozása hello fiókja, szüksége tooobtain a `wsid` és `wskey` kattintva **beállítások**, majd **csatlakoztatott források**, majd **Linux-kiszolgálókon** lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="14e6e-120">Once you are done creating hello account, you need tooobtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="14e6e-121">Hello szám, OMS-példányok kiválasztása, hogy szeretné, hogy és hello áttekintése és telepítése gombra.</span><span class="sxs-lookup"><span data-stu-id="14e6e-121">Select hello number you OMS instances that you want and click hello ‘Review and Install’ button.</span></span> <span data-ttu-id="14e6e-122">Általában érdemes OMS példányok egyenlő toohello számát a virtuális gép van az ügynök fürt toohave hello száma.</span><span class="sxs-lookup"><span data-stu-id="14e6e-122">Typically, you will want toohave hello number of OMS instances equal toohello number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="14e6e-123">Linux OMS-ügynököt telepíti minden egyes virtuális gépen, hogy szeretnének toocollect figyelés és naplózás adatokat egyes tárolóként.</span><span class="sxs-lookup"><span data-stu-id="14e6e-123">OMS Agent for Linux is installs as individual containers on each VM that it wants toocollect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="14e6e-124">Egy egyszerű OMS irányítópult beállítása</span><span class="sxs-lookup"><span data-stu-id="14e6e-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="14e6e-125">Hello virtuális gépeken Linux hello OMS-ügynök telepítése után a következő lépésre tooset hello OMS irányítópult mentése.</span><span class="sxs-lookup"><span data-stu-id="14e6e-125">Once you have installed hello OMS Agent for Linux on hello VMs, next step is tooset up hello OMS dashboard.</span></span> <span data-ttu-id="14e6e-126">Nincsenek két módon toodo ez: OMS-portálon vagy az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="14e6e-126">There are two ways toodo this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="14e6e-127">OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="14e6e-127">OMS Portal</span></span> 

<span data-ttu-id="14e6e-128">Jelentkezzen be az OMS-portálon toohello (<https://mms.microsoft.com>), és toohello **megoldás gyűjteménye**.</span><span class="sxs-lookup"><span data-stu-id="14e6e-128">Log in toohello OMS portal (<https://mms.microsoft.com>) and go toohello **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="14e6e-129">Miután belépett hello **megoldás gyűjtemény**, jelölje be **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="14e6e-129">Once you are in hello **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="14e6e-130">Hello tároló megoldás kijelölt hello OMS áttekintése irányítópult-oldalon csempét hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="14e6e-130">Once you’ve selected hello Container Solution, you will see hello tile on hello OMS Overview Dashboard page.</span></span> <span data-ttu-id="14e6e-131">Miután egy indexelt okozhatnak hello lévő adatokhoz, megjelenik a megoldás nézet csempék adatokkal feltöltve hello csempe.</span><span class="sxs-lookup"><span data-stu-id="14e6e-131">Once hello ingested container data is indexed, you will see hello tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="14e6e-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="14e6e-132">Azure Portal</span></span> 

<span data-ttu-id="14e6e-133">Bejelentkezési tooAzure portálon, a <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="14e6e-133">Login tooAzure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="14e6e-134">Nyissa meg a **piactér**, jelölje be **figyelés + felügyeleti** kattintson **tekintse meg az összes**.</span><span class="sxs-lookup"><span data-stu-id="14e6e-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="14e6e-135">Írja be `containers` keresésben.</span><span class="sxs-lookup"><span data-stu-id="14e6e-135">Then Type `containers` in search.</span></span> <span data-ttu-id="14e6e-136">Látni fogja a "tárolók" hello keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="14e6e-136">You will see "containers" in hello search results.</span></span> <span data-ttu-id="14e6e-137">Válassza ki **tárolók** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="14e6e-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="14e6e-138">Miután rákattintott **létrehozása**, akkor kérni fogja a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="14e6e-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="14e6e-139">A munkaterület kiválasztása, vagy ha nem rendelkezik ilyennel, hozzon létre egy új munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="14e6e-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="14e6e-140">A munkaterület kijelölése után kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="14e6e-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="14e6e-141">A hello OMS tároló megoldás kapcsolatos további információkért tekintse meg az toothe [tároló megoldás Naplóelemzési](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="14e6e-141">For more information about hello OMS Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a><span data-ttu-id="14e6e-142">Hogyan tooscale OMS-ügynököt a DC/OS ACS</span><span class="sxs-lookup"><span data-stu-id="14e6e-142">How tooscale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="14e6e-143">Abban az esetben telepítve toohave OMS-ügynököt hello tényleges csomópontok száma kevés van szüksége, vagy adja hozzá a további VM skálázás be VMSS, ehhez hello skálázással `msoms` szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="14e6e-143">In case you need toohave installed OMS agent short of hello actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling hello `msoms` service.</span></span>

<span data-ttu-id="14e6e-144">Nyissa meg tooMarathon vagy hello DC/OS felhasználói felületének (szolgáltatások) lapján, és növelheti a csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="14e6e-144">You can either go tooMarathon or hello DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="14e6e-145">Ezzel telepít tooother csomópontokat, amelyeknek hello OMS-ügynök még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="14e6e-145">This will deploy tooother nodes which have not yet deployed hello OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="14e6e-146">MS OMS eltávolítása</span><span class="sxs-lookup"><span data-stu-id="14e6e-146">Uninstall MS OMS</span></span>

<span data-ttu-id="14e6e-147">MS OMS toouninstall adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="14e6e-147">toouninstall MS OMS enter hello following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="14e6e-148">Ossza meg velünk!!!</span><span class="sxs-lookup"><span data-stu-id="14e6e-148">Let us know!!!</span></span>
<span data-ttu-id="14e6e-149">Mi működik?</span><span class="sxs-lookup"><span data-stu-id="14e6e-149">What works?</span></span> <span data-ttu-id="14e6e-150">Mi az a hiányzó?</span><span class="sxs-lookup"><span data-stu-id="14e6e-150">What is missing?</span></span> <span data-ttu-id="14e6e-151">Milyen hiba van szüksége a toobe a hasznos meg?</span><span class="sxs-lookup"><span data-stu-id="14e6e-151">What else do you need for this toobe useful for you?</span></span> <span data-ttu-id="14e6e-152">Ossza meg velünk <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="14e6e-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14e6e-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14e6e-153">Next steps</span></span>

 <span data-ttu-id="14e6e-154">Most, hogy meg van adva OMS toomonitor a tárolók[tekintse meg a tároló irányítópult](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="14e6e-154">Now that you have set up OMS toomonitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
