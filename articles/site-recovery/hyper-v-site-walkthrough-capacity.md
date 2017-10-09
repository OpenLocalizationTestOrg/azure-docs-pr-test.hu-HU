---
title: "teljesítmény és méretezés, a Hyper-V virtuális gép replikációs (VMM nélkül) tooAzure az Azure Site Recovery aaaPlan |} Microsoft Docs"
description: "Ez a cikk tooplan kapacitás és a skála használja, az Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek tooAzure replikálása esetén"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f5b029f273e51c01c7d918d176832f6d1735b5f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-tooazure-replication"></a><span data-ttu-id="c296b-103">3. lépés: A kapacitás és a Hyper-V tooAzure replikációs csoportok skálázási módjának megtervezése</span><span class="sxs-lookup"><span data-stu-id="c296b-103">Step 3: Plan capacity and scaling for Hyper-V tooAzure replication</span></span>

<span data-ttu-id="c296b-104">Ez a cikk toofigure jelent a kapacitástervezés és a méretezés, a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) tooAzure replikálása esetén használjon [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c296b-104">Use this article toofigure out planning for capacity and scaling, when replicating on-premises Hyper-V VMs (without System Center VMM) tooAzure with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="c296b-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c296b-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="c296b-106">Hogyan kezdhetem meg, hogy a kapacitástervezés?</span><span class="sxs-lookup"><span data-stu-id="c296b-106">How do I start capacity planning?</span></span>


<span data-ttu-id="c296b-107">A replikálási környezetre vonatkozó információkat gyűjt, és majd a kapacitástervezés ezeket az adatokat, a cikkben a kijelölt hello szempontok használatával.</span><span class="sxs-lookup"><span data-stu-id="c296b-107">You gather information about your replication environment, and then plan capacity using this information, together with hello considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="c296b-108">Adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="c296b-108">Gather information</span></span>

1. <span data-ttu-id="c296b-109">Gyűjtse össze a replikálási környezetre vonatkozó adatokat, többek között a virtuális gépeket, a virtuális gépekhez tartozó lemezeket, valamint a lemezenkénti tárterületeket.</span><span class="sxs-lookup"><span data-stu-id="c296b-109">Gather information about your replication environment, including VMs, disks per VMs, and storage per disk.</span></span>
2. <span data-ttu-id="c296b-110">Azonosítsa a replikált adatok napi adatváltozás (forgalom) sebessége.</span><span class="sxs-lookup"><span data-stu-id="c296b-110">Identify your daily change (churn) rate for replicated data.</span></span> <span data-ttu-id="c296b-111">Töltse le a hello [Hyper-V kapacitástervezési eszköz](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello változási sebessége.</span><span class="sxs-lookup"><span data-stu-id="c296b-111">Download hello [Hyper-V capacity planning tool](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello change rate.</span></span> <span data-ttu-id="c296b-112">Azt javasoljuk, hogy futtatja az eszközt egy hét toocapture keresztül átlagok.</span><span class="sxs-lookup"><span data-stu-id="c296b-112">We recommend you run this tool over a week toocapture averages.</span></span>
 

## <a name="figure-out-capacity"></a><span data-ttu-id="c296b-113">Mérje fel, kapacitás</span><span class="sxs-lookup"><span data-stu-id="c296b-113">Figure out capacity</span></span>

<span data-ttu-id="c296b-114">Megismerte a összefog hello adatok alapján futtatja a hello [hely helyreállítási kapacitás Planner eszköz](http://aka.ms/asr-capacity-planner-excel) tooanalyze a forráskörnyezetét és a munkaterhelések, sávszélesség igényeknek és a kiszolgáló erőforrásait hello forráshely becsléséhez, és hello (virtuális gép és tárterület stb), szükséges erőforrások hello célként megadott helyen.</span><span class="sxs-lookup"><span data-stu-id="c296b-114">Based on hello information you've gather, you run hello [Site Recovery Capacity Planner Tool](http://aka.ms/asr-capacity-planner-excel) tooanalyze your source environment and workloads, estimate bandwidth needs and server resources for hello source location, and hello resources (virtual machines and storage etc), that you need in hello target location.</span></span> <span data-ttu-id="c296b-115">Hello eszköz módok néhány futtathatja:</span><span class="sxs-lookup"><span data-stu-id="c296b-115">You can run hello tool in a couple of modes:</span></span>

- <span data-ttu-id="c296b-116">Gyors tervezési: a hálózat- és átlagos száma virtuális gépeket, a lemezek, a tároló és a változási sebessége alapján leképezések tooget futtassa hello eszközt ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="c296b-116">Quick planning: Run hello tool in this mode tooget network and server projections based on an average number of VMs, disks, storage, and change rate.</span></span>
- <span data-ttu-id="c296b-117">Részletes tervezési: hello futtathatja ezt a módot, és adja meg a virtuális gép szinten minden munkaterhelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="c296b-117">Detailed planning: Run hello tool in this mode, and provide details of each workload at VM level.</span></span> <span data-ttu-id="c296b-118">Virtuális gép kompatibilitási elemzéséhez, és a hálózat- és leképezések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c296b-118">Analyze VM compatibility and get network and server projections.</span></span>

<span data-ttu-id="c296b-119">Most futtassa hello eszközt:</span><span class="sxs-lookup"><span data-stu-id="c296b-119">Now run hello tool:</span></span>

1. <span data-ttu-id="c296b-120">Töltse le a hello [eszköz](http://aka.ms/asr-capacity-planner-excel)</span><span class="sxs-lookup"><span data-stu-id="c296b-120">Download hello [tool](http://aka.ms/asr-capacity-planner-excel)</span></span>
2. <span data-ttu-id="c296b-121">toorun hello gyors planner, hajtsa végre a [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-quick-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="c296b-121">toorun hello quick planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-quick-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>
3. <span data-ttu-id="c296b-122">toorun részletes planner hello hajtsa végre az [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-detailed-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="c296b-122">toorun hello detailed planner, follow [these instructions](site-recovery-capacity-planner.md#run-the-detailed-planner), and select hello scenario **Hyper-V tooAzure**.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="c296b-123">Hálózati sávszélesség szabályozására</span><span class="sxs-lookup"><span data-stu-id="c296b-123">Control network bandwidth</span></span>

<span data-ttu-id="c296b-124">Miután megismerte a számított hello sávszélesség van szüksége, a replikációhoz használt sávszélesség mennyiségét ellenőrző hello lehetőségek több lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="c296b-124">After you've calculated hello bandwidth you need, you have a couple of options for controlling hello amount of bandwidth used for replication:</span></span>

* <span data-ttu-id="c296b-125">**A sávszélesség szabályozását**: tooAzure replikáló Hyper-V-forgalom halad át egy adott Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="c296b-125">**Throttle bandwidth**: Hyper-V traffic that replicates tooAzure goes through a specific Hyper-V host.</span></span> <span data-ttu-id="c296b-126">Beállíthatja a gazdagép-kiszolgálón hello sávszélesség szabályozását.</span><span class="sxs-lookup"><span data-stu-id="c296b-126">You can throttle bandwidth on hello host server.</span></span>
* <span data-ttu-id="c296b-127">**Sávszélesség befolyásolja**: beállításkulcsok több replikációhoz használt hello sávszélességet is szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="c296b-127">**Influence bandwidth**: You can influence hello bandwidth used for replication using a couple of registry keys.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="c296b-128">Sávszélesség szabályozása</span><span class="sxs-lookup"><span data-stu-id="c296b-128">Throttle bandwidth</span></span>
1. <span data-ttu-id="c296b-129">Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c296b-129">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host server.</span></span> <span data-ttu-id="c296b-130">Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="c296b-130">By default a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="c296b-131">Kattintson a beépülő modul hello **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="c296b-131">In hello snap-in click **Change Properties**.</span></span>
3. <span data-ttu-id="c296b-132">A hello **sávszélesség-szabályozási** lapon válassza **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**, és állítsa be a munkaórákra hello és a munkaórákon kívüli időre.</span><span class="sxs-lookup"><span data-stu-id="c296b-132">On hello **Throttling** tab select **Enable internet bandwidth usage throttling for backup operations**, and set hello limits for work and non-work hours.</span></span> <span data-ttu-id="c296b-133">Érvényes tartomány: az 512 kbit/s too102 MB / s.</span><span class="sxs-lookup"><span data-stu-id="c296b-133">Valid ranges are from 512 Kbps too102 Mbps per second.</span></span>

    ![Sávszélesség szabályozása](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

<span data-ttu-id="c296b-135">Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="c296b-135">You can also use hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset throttling.</span></span> <span data-ttu-id="c296b-136">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="c296b-136">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="c296b-137">A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.</span><span class="sxs-lookup"><span data-stu-id="c296b-137">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth"></a><span data-ttu-id="c296b-138">A hálózati sávszélesség szabályozása</span><span class="sxs-lookup"><span data-stu-id="c296b-138">Influence network bandwidth</span></span>
1. <span data-ttu-id="c296b-139">Hello beállításjegyzék lépjen a túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="c296b-139">In hello registry navigate too**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="c296b-140">tooinfluence hello sávszélesség forgalom replikációs lemezen, módosítsa a hello érték hello **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="c296b-140">tooinfluence hello bandwidth traffic on a replicating disk, modify hello value hello **UploadThreadsPerVM**, or create hello key if it doesn't exist.</span></span>
   * <span data-ttu-id="c296b-141">az Azure, a feladatátvételi forgalomhoz tooinfluence hello sávszélesség módosítható hello értéke **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="c296b-141">tooinfluence hello bandwidth for failback traffic from Azure, modify hello value **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="c296b-142">hello alapértelmezett értéke 4.</span><span class="sxs-lookup"><span data-stu-id="c296b-142">hello default value is 4.</span></span> <span data-ttu-id="c296b-143">"A szükségesnél több erőforrással" hálózatban ezek a kulcsok hello alapértelmezett értékekhez módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="c296b-143">In an “overprovisioned” network, these registry keys should be changed from hello default values.</span></span> <span data-ttu-id="c296b-144">hello maximális 32.</span><span class="sxs-lookup"><span data-stu-id="c296b-144">hello maximum is 32.</span></span> <span data-ttu-id="c296b-145">Forgalom toooptimize hello érték figyelése.</span><span class="sxs-lookup"><span data-stu-id="c296b-145">Monitor traffic toooptimize hello value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c296b-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c296b-146">Next steps</span></span>

<span data-ttu-id="c296b-147">Nyissa meg túl[4. lépés: hálózati terv](hyper-v-site-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="c296b-147">Go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md).</span></span>
