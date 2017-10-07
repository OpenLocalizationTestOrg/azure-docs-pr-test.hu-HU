---
title: "teljesítmény és méretezés, a Hyper-V Virtuálisgép-replikációt (VMM) tooAzure az Azure Site Recovery aaaPlan |} Microsoft Docs"
description: "Ez a cikk tooplan kapacitás és a skála felhasználhatja a tooAzure, az Azure Site Recovery VMM-ben a Hyper-V virtuális gépek replikálása felhők"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 9818ada9bb21f60ac00b3894696201b06630cb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-tooazure-replication"></a><span data-ttu-id="e6adf-103">3. lépés: Teljesítmény és méretezés, a Hyper-V (a VMM-mel) tooAzure replikáció tervezése</span><span class="sxs-lookup"><span data-stu-id="e6adf-103">Step 3: Plan capacity and scaling for Hyper-V (with VMM) tooAzure replication</span></span>

<span data-ttu-id="e6adf-104">Miután már tekintse át a hello [telepítésének előfeltételei](vmm-to-azure-walkthrough-prerequisites.md), használja a cikk toofigure kimenő jelent a kapacitástervezés és skálázás, replikálása esetén a helyszíni Hyper-V virtuális gépek a System Center Virtual Machine Manager (VMM) felhők tooAzure, a [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e6adf-104">After you've reviewed hello [deployment prerequisites](vmm-to-azure-walkthrough-prerequisites.md), use this article toofigure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="e6adf-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e6adf-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="e6adf-106">Hogyan kezdhetem meg, hogy a kapacitástervezés?</span><span class="sxs-lookup"><span data-stu-id="e6adf-106">How do I start capacity planning?</span></span>


<span data-ttu-id="e6adf-107">A replikálási környezetet, és terv sebesség hello adatokról, és ez a cikk kiemelt hello szempontok információt gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="e6adf-107">You gather information about your replication environment, and then plan capacity using hello data, together with hello considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="e6adf-108">Adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="e6adf-108">Gather information</span></span>

1. <span data-ttu-id="e6adf-109">Gyűjtse össze a replikálási környezetre vonatkozó adatokat, többek között a virtuális gépeket, a virtuális gépekhez tartozó lemezeket, valamint a lemezenkénti tárterületeket.</span><span class="sxs-lookup"><span data-stu-id="e6adf-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="e6adf-110">Azonosítsa a replikált adatok napi adatváltozás (forgalom) sebessége.</span><span class="sxs-lookup"><span data-stu-id="e6adf-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="e6adf-111">Töltse le a hello [Hyper-V kapacitástervezési eszköz](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello változási sebessége.</span><span class="sxs-lookup"><span data-stu-id="e6adf-111">Download hello [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello change rate.</span></span> <span data-ttu-id="e6adf-112">Azt javasoljuk, hogy futtatja az eszközt egy hét toocapture keresztül átlagok.</span><span class="sxs-lookup"><span data-stu-id="e6adf-112">We recommend you run this tool over a week toocapture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="e6adf-113">Mérje fel, kapacitás</span><span class="sxs-lookup"><span data-stu-id="e6adf-113">Figure out capacity</span></span>

<span data-ttu-id="e6adf-114">Megismerte a összefog hello adatok alapján futtatja a hello [hely helyreállítási kapacitás Planner eszköz](http://aka.ms/asr-capacity-planner-excel) tooanalyze a forráskörnyezetét és a munkaterhelések, sávszélesség igényeknek és a kiszolgáló erőforrásait hello forráshely becsléséhez, és hello (virtuális gép és tárterület stb), szükséges erőforrások hello célként megadott helyen.</span><span class="sxs-lookup"><span data-stu-id="e6adf-114">Based on hello information you've gather, you run hello [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) tooanalyze your source environment and workloads, estimate bandwidth needs and server resources for hello source location, and hello resources (virtual machines and storage etc), that you need in hello target location.</span></span> <span data-ttu-id="e6adf-115">Hello eszköz módok néhány futtathatja:</span><span class="sxs-lookup"><span data-stu-id="e6adf-115">You can run hello tool in a couple of modes:</span></span>

- <span data-ttu-id="e6adf-116">Gyors tervezési: a hálózat- és átlagos száma virtuális gépeket, a lemezek, a tároló és a változási sebessége alapján leképezések tooget futtassa hello eszközt ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="e6adf-116">Quick planning: Run hello tool in this mode tooget network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="e6adf-117">Részletes tervezési: hello futtathatja ezt a módot, és adja meg a virtuális gép szinten minden munkaterhelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="e6adf-117">Detailed planning: Run hello tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="e6adf-118">Virtuális gép kompatibilitási elemzéséhez, és a hálózat- és leképezések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e6adf-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="e6adf-119">Most futtassa hello eszközt:</span><span class="sxs-lookup"><span data-stu-id="e6adf-119">Now run hello tool:</span></span>

1. <span data-ttu-id="e6adf-120">Töltse le a hello [eszköz](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="e6adf-120">Download hello [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="e6adf-121">toorun hello gyors planner, hajtsa végre a [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-quick-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="e6adf-121">toorun hello quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>
3. <span data-ttu-id="e6adf-122">toorun részletes planner hello hajtsa végre az [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-detailed-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="e6adf-122">toorun hello detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="e6adf-123">Hálózati sávszélesség szabályozására</span><span class="sxs-lookup"><span data-stu-id="e6adf-123">Control network bandwidth</span></span>

<span data-ttu-id="e6adf-124">Miután megismerte a számított hello sávszélesség van szüksége, a replikációhoz használt sávszélesség mennyiségét ellenőrző hello lehetőségek több lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="e6adf-124">After you've calculated hello bandwidth you need, you have a couple of options for controlling hello amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="e6adf-125">**A sávszélesség szabályozását**: tooAzure replikáló Hyper-V-forgalom halad át egy adott Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="e6adf-125">**Throttle bandwidth**: Hyper-V traffic that replicates tooAzure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="e6adf-126">Beállíthatja a gazdagép-kiszolgálón hello sávszélesség szabályozását.</span><span class="sxs-lookup"><span data-stu-id="e6adf-126">You can throttle bandwidth on hello host server.</span></span>
* <span data-ttu-id="e6adf-127">**Sávszélesség befolyásolja**: beállításkulcsok több replikációhoz használt hello sávszélességet is szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e6adf-127">**Influence bandwidth**: You can influence hello bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="e6adf-128">Sávszélesség szabályozása</span><span class="sxs-lookup"><span data-stu-id="e6adf-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="e6adf-129">Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="e6adf-129">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host server.</span></span> <span data-ttu-id="e6adf-130">Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="e6adf-130">By default a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="e6adf-131">Kattintson a beépülő modul hello **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="e6adf-131">In hello snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="e6adf-132">A hello **sávszélesség-szabályozási** lapon válassza **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**, és állítsa be a munkaórákra hello és a munkaórákon kívüli időre.</span><span class="sxs-lookup"><span data-stu-id="e6adf-132">On hello **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set hello limits for work and non-work hours.</span></span> <span data-ttu-id="e6adf-133">Érvényes tartomány: az 512 kbit/s too102 MB / s.</span><span class="sxs-lookup"><span data-stu-id="e6adf-133">Valid ranges are from 512 Kbps too102 Mbps per second.</span></span>

    ![Sávszélesség szabályozása](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

<span data-ttu-id="e6adf-135">Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="e6adf-135">You can also use hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset throttling.</span></span> <span data-ttu-id="e6adf-136">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="e6adf-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="e6adf-137">A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.</span><span class="sxs-lookup"><span data-stu-id="e6adf-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="e6adf-138">A hálózati sávszélesség szabályozása</span><span class="sxs-lookup"><span data-stu-id="e6adf-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="e6adf-139">Hello beállításjegyzék lépjen a túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="e6adf-139">In hello registry navigate too**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="e6adf-140">tooinfluence hello sávszélesség forgalom replikációs lemezen, módosítsa a hello érték hello **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e6adf-140">tooinfluence hello bandwidth traffic on a replicating disk, modify hello value hello **UploadThreadsPerVM**, or create hello key if it doesn't exist.</span></span>
   * <span data-ttu-id="e6adf-141">az Azure, a feladatátvételi forgalomhoz tooinfluence hello sávszélesség módosítható hello értéke **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="e6adf-141">tooinfluence hello bandwidth for failback traffic from Azure, modify hello value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="e6adf-142">hello alapértelmezett értéke 4.</span><span class="sxs-lookup"><span data-stu-id="e6adf-142">hello default value is 4.</span></span> <span data-ttu-id="e6adf-143">"A szükségesnél több erőforrással" hálózatban ezek a kulcsok hello alapértelmezett értékekhez módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="e6adf-143">In an “overprovisioned” network, these registry keys should be changed from hello default values.</span></span> <span data-ttu-id="e6adf-144">hello maximális 32.</span><span class="sxs-lookup"><span data-stu-id="e6adf-144">hello maximum is 32.</span></span> <span data-ttu-id="e6adf-145">Forgalom toooptimize hello érték figyelése.</span><span class="sxs-lookup"><span data-stu-id="e6adf-145">Monitor traffic toooptimize hello value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6adf-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6adf-146">Next steps</span></span>

<span data-ttu-id="e6adf-147">Nyissa meg túl[4. lépés: hálózati terv](vmm-to-azure-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="e6adf-147">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md).</span></span>
