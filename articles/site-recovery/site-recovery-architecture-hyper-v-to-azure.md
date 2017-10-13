---
title: "aaaHow nem Hyper-V replikáció tooAzure munka az Azure Site Recovery? | Microsoft Docs"
description: "Ez a cikk áttekintése összetevők és architektúra használható, ha replikálása a helyszíni Hyper-V virtuális gépek tooAzure hello Azure Site Recovery szolgáltatásban."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 3b64156307f37764a8315dec68cf297acf279530
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work-in-site-recovery"></a><span data-ttu-id="216da-104">Hogyan működik a Hyper-V replikáció tooAzure a Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="216da-104">How does Hyper-V replication tooAzure work in Site Recovery?</span></span>


<span data-ttu-id="216da-105">Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni Hyper-V virtuális gépek hello segítségével tooAzure [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="216da-105">This article describes hello components and processes involved when replicating on-premises Hyper-V virtual machines, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="216da-106">A Site Recovery képes Hyper-V virtuális gépeket a System Center Virtual Machine Manager (VMM) szolgáltatással vagy anélkül felügyelt Hyper-V fürtökön vagy különálló gazdagépeken replikálni.</span><span class="sxs-lookup"><span data-stu-id="216da-106">Site Recovery can replicate Hyper-V VMs on Hyper-V clusters and standalone hosts that are managed with or without System Center Virtual Machine Manager (VMM).</span></span>

<span data-ttu-id="216da-107">Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="216da-107">Post any comments at hello bottom of this article, or in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="architectural-components"></a><span data-ttu-id="216da-108">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="216da-108">Architectural components</span></span>

<span data-ttu-id="216da-109">Számos összetevők érintett tooAzure Hyper-V virtuális gépek replikálása esetén.</span><span class="sxs-lookup"><span data-stu-id="216da-109">There are a number of components involved when replicating Hyper-V VMs tooAzure.</span></span>

<span data-ttu-id="216da-110">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="216da-110">**Component**</span></span> | <span data-ttu-id="216da-111">**Hely**</span><span class="sxs-lookup"><span data-stu-id="216da-111">**Location**</span></span> | <span data-ttu-id="216da-112">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="216da-112">**Details**</span></span>
--- | --- | ---
<span data-ttu-id="216da-113">**Azure**</span><span class="sxs-lookup"><span data-stu-id="216da-113">**Azure**</span></span> | <span data-ttu-id="216da-114">Az Azure-ban szüksége van egy Microsoft Azure-fiókra, és Azure Storage-fiókra és egy Azure-hálózatra.</span><span class="sxs-lookup"><span data-stu-id="216da-114">In Azure you need a Microsoft Azure account, an Azure storage account, and a Azure network.</span></span> | <span data-ttu-id="216da-115">Hello tárfiók tárolja a replikált adatok, és az Azure virtuális gépek replikálása hello adatokkal jönnek létre, ha a feladatátvétel a helyszíni helyről.</span><span class="sxs-lookup"><span data-stu-id="216da-115">Replicated data is stored in hello storage account, and Azure VMs are created with hello replicated data when failover from your on-premises site occurs.</span></span><br/><br/> <span data-ttu-id="216da-116">hello Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="216da-116">hello Azure VMs connect toohello Azure virtual network when they're created.</span></span>
<span data-ttu-id="216da-117">**VMM-kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="216da-117">**VMM server**</span></span> | <span data-ttu-id="216da-118">A Hyper-V-gazdagépek VMM-felhőkben helyezkednek el</span><span class="sxs-lookup"><span data-stu-id="216da-118">Hyper-V hosts are located in VMM clouds</span></span> | <span data-ttu-id="216da-119">Ha a VMM-felhőkben felügyelt Hyper-V-gazdagépek, Recovery Services-tároló hello hello VMM-kiszolgáló regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="216da-119">If Hyper-V hosts are managed in VMM clouds, you register hello VMM server in hello Recovery Services vault.</span></span><br/><br/> <span data-ttu-id="216da-120">A VMM-kiszolgálón hello hello Site Recovery Provider tooorchestrate replikációs az Azure-ral telepítése.</span><span class="sxs-lookup"><span data-stu-id="216da-120">On hello VMM server you install hello Site Recovery Provider tooorchestrate replication with Azure.</span></span><br/><br/> <span data-ttu-id="216da-121">Logikai van szüksége, és a Virtuálisgép-hálózatok tooconfigure hálózatleképezés beállításához.</span><span class="sxs-lookup"><span data-stu-id="216da-121">You need logical and VM networks set up tooconfigure network mapping.</span></span> <span data-ttu-id="216da-122">A Virtuálisgép-hálózat hello felhőhöz társított logikai hálózati csatolt tooa kell lennie.</span><span class="sxs-lookup"><span data-stu-id="216da-122">A VM network should be linked tooa logical network that's associated with hello cloud.</span></span>
<span data-ttu-id="216da-123">**Hyper-V gazdagép**</span><span class="sxs-lookup"><span data-stu-id="216da-123">**Hyper-V host**</span></span> | <span data-ttu-id="216da-124">A Hyper-V gazdagépek és fürtök VMM-kiszolgálóval vagy anélkül is üzembe helyezhetők.</span><span class="sxs-lookup"><span data-stu-id="216da-124">Hyper-V hosts and clusters can be deployed with or without VMM server.</span></span> | <span data-ttu-id="216da-125">Ha nem a VMM-kiszolgáló, a Site Recovery Provider telepítve van a hello állomás tooorchestrate replikálásához a Site Recovery keresztül hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="216da-125">If there's no VMM server, hello Site Recovery Provider is installed on hello host tooorchestrate replication with Site Recovery over hello internet.</span></span> <span data-ttu-id="216da-126">A VMM-kiszolgáló esetén hello szolgáltató telepítve van rajta, nem pedig hello állomás.</span><span class="sxs-lookup"><span data-stu-id="216da-126">If there's a VMM server, hello Provider is installed on it, and not on hello host.</span></span><br/><br/> <span data-ttu-id="216da-127">hello állomás toohandle adatreplikáció hello Recovery Services Agent ügynök van telepítve.</span><span class="sxs-lookup"><span data-stu-id="216da-127">hello Recovery Services agent is installed on hello host toohandle data replication.</span></span><br/><br/> <span data-ttu-id="216da-128">A szolgáltató hello és hello ügynök kommunikációit a biztonságos és titkosított.</span><span class="sxs-lookup"><span data-stu-id="216da-128">Communications from both hello Provider and hello agent are secure and encrypted.</span></span> <span data-ttu-id="216da-129">Ezenfelül az Azure-tárfiókba replikált adatok is titkosítást kapnak.</span><span class="sxs-lookup"><span data-stu-id="216da-129">Replicated data in Azure storage is also encrypted.</span></span>
<span data-ttu-id="216da-130">**Hyper-V virtuális gépek**</span><span class="sxs-lookup"><span data-stu-id="216da-130">**Hyper-V VMs**</span></span> | <span data-ttu-id="216da-131">Legalább egy virtuális gépnek futnia kell egy Hyper-V-gazdakiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="216da-131">You need one or more VMs running on a Hyper-V host server.</span></span> | <span data-ttu-id="216da-132">Virtuális gépekre telepített tooexplicitly kell semmi sem.</span><span class="sxs-lookup"><span data-stu-id="216da-132">Nothing needs tooexplicitly installed on VMs.</span></span>

<span data-ttu-id="216da-133">Hello telepítés előfeltételeit és ezeket az összetevőket a hello követelményeiről további [támogatási mátrix](site-recovery-support-matrix-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="216da-133">Learn about hello deployment prerequisites and requirements for each of these components in hello [support matrix](site-recovery-support-matrix-to-azure.md).</span></span>

<span data-ttu-id="216da-134">**1. ábra: A Hyper-V helyek tooAzure replikációs**</span><span class="sxs-lookup"><span data-stu-id="216da-134">**Figure 1: Hyper-V site tooAzure replication**</span></span>

![Összetevők](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

<span data-ttu-id="216da-136">**2. ábra: A Hyper-V a VMM-felhők tooAzure replikáció**</span><span class="sxs-lookup"><span data-stu-id="216da-136">**Figure 2: Hyper-V in VMM clouds tooAzure replication**</span></span>

![Összetevők](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a><span data-ttu-id="216da-138">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="216da-138">Replication process</span></span>

<span data-ttu-id="216da-139">**3. ábra: A Hyper-V replikáció tooAzure replikáció és a helyreállítási folyamat**</span><span class="sxs-lookup"><span data-stu-id="216da-139">**Figure 3: Replication and recovery process for Hyper-V replication tooAzure**</span></span>

![munkafolyamat](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a><span data-ttu-id="216da-141">Védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="216da-141">Enable protection</span></span>

1. <span data-ttu-id="216da-142">Miután engedélyezte a Hyper-V virtuális gépek védelmét, a hello Azure-portálon vagy a helyszíni hello **engedélyezni a védelmet** kezdődik.</span><span class="sxs-lookup"><span data-stu-id="216da-142">After you enable protection for a Hyper-V VM, in hello Azure portal or on-premises, hello **Enable protection** starts.</span></span>
2. <span data-ttu-id="216da-143">hello feladat ellenőrzi, hogy hello a gép megfelel az előfeltételeknek, hello meghívása előtt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset hello-beállítások konfigurálása a replikáció.</span><span class="sxs-lookup"><span data-stu-id="216da-143">hello job checks that hello machine complies with prerequisites, before invoking hello [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset up replication with hello settings you've configured.</span></span>
3. <span data-ttu-id="216da-144">hello feladat elindul a kezdeti replikáció hello figyelőn [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metódus tooinitialize teljes Virtuálisgép-replikációt és küldési hello VM virtuális lemezek tooAzure.</span><span class="sxs-lookup"><span data-stu-id="216da-144">hello job starts initial replication by invoking hello [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) method, tooinitialize a full VM replication, and send hello VM's virtual disks tooAzure.</span></span>
4. <span data-ttu-id="216da-145">Figyelheti a feladat hello hello **feladatok** fülre.      ![Feladatok listája](media/site-recovery-hyper-v-azure-architecture/image1.png)![Védelem engedélyezésének részletei](media/site-recovery-hyper-v-azure-architecture/image2.png)</span><span class="sxs-lookup"><span data-stu-id="216da-145">You can monitor hello job in hello **Jobs** tab.      ![Jobs list](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Enable protection drill down](media/site-recovery-hyper-v-azure-architecture/image2.png)</span></span>

### <a name="replicate-hello-initial-data"></a><span data-ttu-id="216da-146">Hello kezdeti adatreplikációhoz</span><span class="sxs-lookup"><span data-stu-id="216da-146">Replicate hello initial data</span></span>

1. <span data-ttu-id="216da-147">A kezdeti replikálás indításakor egy [pillanatfelvétel készül a Hyper-V-alapú virtuális gépről](https://technet.microsoft.com/library/dd560637.aspx).</span><span class="sxs-lookup"><span data-stu-id="216da-147">A [Hyper-V VM snapshot](https://technet.microsoft.com/library/dd560637.aspx) snapshot is taken when initial replication is triggered.</span></span>
2. <span data-ttu-id="216da-148">Virtuális merevlemezek replikálását csak az összes másolt tooAzure fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="216da-148">Virtual hard disks are replicated one by one until they're all copied tooAzure.</span></span> <span data-ttu-id="216da-149">Sikerült az időigényes, attól függően, hogy hello Virtuálisgép-méretet, és a hálózati sávszélességre.</span><span class="sxs-lookup"><span data-stu-id="216da-149">It could take a while, depending on hello VM size, and network bandwidth.</span></span> <span data-ttu-id="216da-150">toooptimize a hálózat használatát, lásd: [hogyan toomanage helyszíni tooAzure védelem hálózati sávszélesség használatának](https://support.microsoft.com/kb/3056159).</span><span class="sxs-lookup"><span data-stu-id="216da-150">toooptimize your network usage, see [How toomanage on-premises tooAzure protection network bandwidth usage](https://support.microsoft.com/kb/3056159).</span></span>
3. <span data-ttu-id="216da-151">Ha a lemezen változások történnek, amíg a kezdeti replikáció folyamatban van, hello Hyper-V Replica Replication Tracker mint Hyper-V replikálási naplók (.hrl) nyomon követi ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="216da-151">If disk changes occur while initial replication is in progress, hello Hyper-V Replica Replication Tracker tracks those changes as Hyper-V Replication Logs (.hrl).</span></span> <span data-ttu-id="216da-152">Ezek a fájlok találhatók hello hello lemezek ugyanabban a mappában.</span><span class="sxs-lookup"><span data-stu-id="216da-152">These files are located in hello same folder as hello disks.</span></span> <span data-ttu-id="216da-153">Minden lemezhez tartozik egy .hrl-fájl toosecondary tárolási elküldendő.</span><span class="sxs-lookup"><span data-stu-id="216da-153">Each disk has an associated .hrl file that will be sent toosecondary storage.</span></span>
4. <span data-ttu-id="216da-154">hello pillanatkép és a naplófájlok fájlok lemezerőforrásokat használnak, amíg a kezdeti replikáció folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="216da-154">hello snapshot and log files consume disk resources while initial replication is in progress.</span></span>
5. <span data-ttu-id="216da-155">Hello kezdeti replikáció befejezése után hello VM pillanatkép törlését.</span><span class="sxs-lookup"><span data-stu-id="216da-155">When hello initial replication finishes, hello VM snapshot is deleted.</span></span> <span data-ttu-id="216da-156">Hello naplóban rögzített változásokat szinkronizálja, és egyesített toohello szülőlemezt.</span><span class="sxs-lookup"><span data-stu-id="216da-156">Delta disk changes in hello log are synchronized and merged toohello parent disk.</span></span>


### <a name="finalize-protection"></a><span data-ttu-id="216da-157">Védelem véglegesítése</span><span class="sxs-lookup"><span data-stu-id="216da-157">Finalize protection</span></span>

1. <span data-ttu-id="216da-158">Hello kezdeti replikáció befejezését követően, hello **védelem véglegesítése a virtuális gép hello** feladat hálózati és a replikációt követő egyéb beállításokat konfigurálja, hogy hello virtuális gép védett.</span><span class="sxs-lookup"><span data-stu-id="216da-158">After hello initial replication finishes, hello **Finalize protection on hello virtual machine** job configures network and other post-replication settings, so that hello virtual machine is protected.</span></span>
    <span data-ttu-id="216da-159">![Védelem véglegesítése feladat](media/site-recovery-hyper-v-azure-architecture/image3.png)</span><span class="sxs-lookup"><span data-stu-id="216da-159">![Finalize protection job](media/site-recovery-hyper-v-azure-architecture/image3.png)</span></span>
2. <span data-ttu-id="216da-160">Ha tooAzure replikál, akkor tootweak hello beállítások hello virtuális gép, hogy készen áll a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="216da-160">If you're replicating tooAzure, you might need tootweak hello settings for hello virtual machine so that it's ready for failover.</span></span> <span data-ttu-id="216da-161">Ezen a ponton futtatása a teszt feladatátvételi toocheck minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="216da-161">At this point you can run a test failover toocheck everything is working as expected.</span></span>

### <a name="replicate-hello-delta"></a><span data-ttu-id="216da-162">Hello különbözeti replikáció</span><span class="sxs-lookup"><span data-stu-id="216da-162">Replicate hello delta</span></span>

1. <span data-ttu-id="216da-163">Hello kezdeti replikálás után elindul a változások szinkronizálása replikációs beállításoknak megfelelően.</span><span class="sxs-lookup"><span data-stu-id="216da-163">After hello initial replication, delta synchronization begins, in accordance with replication settings.</span></span>
2. <span data-ttu-id="216da-164">Hyper-V Replica Replication Tracker hello hello módosítások tooa virtuális merevlemez .hrl-fájlok formájában követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="216da-164">hello Hyper-V Replica Replication Tracker tracks hello changes tooa virtual hard disk as .hrl files.</span></span> <span data-ttu-id="216da-165">Minden replikációra konfigurált lemezhez tartozik egy .hrl fájl.</span><span class="sxs-lookup"><span data-stu-id="216da-165">Each disk that's configured for replication has an associated .hrl file.</span></span> <span data-ttu-id="216da-166">Ez a napló toohello ügyfél tárfiókjával küldött kezdeti replikáció befejezése után.</span><span class="sxs-lookup"><span data-stu-id="216da-166">This log is sent toohello customer's storage account after initial replication is complete.</span></span> <span data-ttu-id="216da-167">Amikor a napló az átvitel közben tooAzure, hello elsődleges lemez hello változásait követi egy másik naplófájlt, a hello ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="216da-167">When a log is in transit tooAzure, hello changes in hello primary disk are tracked in another log file, in hello same directory.</span></span>
3. <span data-ttu-id="216da-168">Kiindulási és különbözeti replikálás során hello VM hello Virtuálisgép-nézetet a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="216da-168">During initial and delta replication, you can monitor hello VM in hello VM view.</span></span> <span data-ttu-id="216da-169">[További információk](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="216da-169">[Learn more](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).</span></span>  

### <a name="synchronize-replication"></a><span data-ttu-id="216da-170">Replikáció szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="216da-170">Synchronize replication</span></span>

1. <span data-ttu-id="216da-171">Ha nem sikerül a változások replikálása, és a teljes replikáció túl sok sávszélességet vagy időt venne igénybe, a rendszer a virtuális gépet megjelöli újraszinkronizálásra.</span><span class="sxs-lookup"><span data-stu-id="216da-171">If delta replication fails, and a full replication would be costly in terms of bandwidth or time, then a VM is marked for resynchronization.</span></span> <span data-ttu-id="216da-172">Például ha hello .hrl-fájlok elérnék hello lemez méretének 50 %-át, majd hello VM lesz megjelölve az újraszinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="216da-172">For example, if hello .hrl files reach 50% of hello disk size, then hello VM will be marked for resynchronization.</span></span>
2.  <span data-ttu-id="216da-173">Az újraszinkronizálás minimálisra hello hello forrás és cél virtuális gépek ellenőrzőösszegeit, és csak a hello különbözeti adatokat küld által elküldött adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="216da-173">Resynchronization minimizes hello amount of data sent by computing checksums of hello source and target virtual machines, and sending only hello delta data.</span></span> <span data-ttu-id="216da-174">Az újraszinkronizálás egy rögzített blokkméretű csonkoló algoritmust alkalmaz, amelyben a forrás- és a célfájlok rögzített méretű adattömbökre vannak osztva.</span><span class="sxs-lookup"><span data-stu-id="216da-174">Resynchronization uses a fixed-block chunking algorithm where source and target files are divided into fixed chunks.</span></span> <span data-ttu-id="216da-175">Az egyes adattömbök ellenőrzőösszegeket akkor jönnek létre, és összehasonlítja a toodetermine, amely megakadályozza a hello forrás kell toobe alkalmazott toohello céltól.</span><span class="sxs-lookup"><span data-stu-id="216da-175">Checksums for each chunk are generated and then compared toodetermine which blocks from hello source need toobe applied toohello target.</span></span>
3. <span data-ttu-id="216da-176">Az újraszinkronizálás befejezését követően folytatódik a normál változásreplikálás.</span><span class="sxs-lookup"><span data-stu-id="216da-176">After resynchronization finishes, normal delta replication should resume.</span></span> <span data-ttu-id="216da-177">Alapértelmezés szerint az újraszinkronizálás automatikusan munkaidőn kívüli ütemezett toorun, de egy virtuális gépet manuálisan szinkronizálja újra.</span><span class="sxs-lookup"><span data-stu-id="216da-177">By default resynchronization is scheduled toorun automatically outside office hours, but you can resynchronize a virtual machine manually.</span></span> <span data-ttu-id="216da-178">Például manuálisan folytathatja az újraszinkronizálást, ha hálózatkimaradás vagy egyéb kimaradás következik be.</span><span class="sxs-lookup"><span data-stu-id="216da-178">For example, you can resume resynchronization if a network outage or another outage occurs.</span></span> <span data-ttu-id="216da-179">toodo e, jelölje be hello VM hello portálon > **újraszinkronizálása**.</span><span class="sxs-lookup"><span data-stu-id="216da-179">toodo this, select hello VM in hello portal > **Resynchronize**.</span></span>

    ![Manuális újraszinkronizálás](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a><span data-ttu-id="216da-181">Újrapróbálkozási logika</span><span class="sxs-lookup"><span data-stu-id="216da-181">Retry logic</span></span>

<span data-ttu-id="216da-182">Ha hiba lép fel a replikáció során, a rendszer automatikusan újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="216da-182">If a replication error occurs, there's a built-in retry.</span></span> <span data-ttu-id="216da-183">A vonatkozó logika két kategóriába sorolható:</span><span class="sxs-lookup"><span data-stu-id="216da-183">This logic can be classified into two categories:</span></span>

<span data-ttu-id="216da-184">**Kategória**</span><span class="sxs-lookup"><span data-stu-id="216da-184">**Category**</span></span> | <span data-ttu-id="216da-185">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="216da-185">**Details**</span></span>
--- | ---
<span data-ttu-id="216da-186">**Helyreállíthatatlan hibák**</span><span class="sxs-lookup"><span data-stu-id="216da-186">**Non-recoverable errors**</span></span> | <span data-ttu-id="216da-187">A rendszer nem kísérli meg a helyreállításukat.</span><span class="sxs-lookup"><span data-stu-id="216da-187">No retry is attempted.</span></span> <span data-ttu-id="216da-188">A virtuális gép állapota **Kritikusra** vált, és rendszergazdai beavatkozás szükséges.</span><span class="sxs-lookup"><span data-stu-id="216da-188">VM status will be **Critical**, and administrator intervention is required.</span></span> <span data-ttu-id="216da-189">Ezek a hibák például:; VHD-lánc megszakadt Érvénytelen állapot hello replika virtuális gép; Hálózati hitelesítési hibák: hitelesítési hibák; Virtuális gép nem található a hibák (az önálló Hyper-V kiszolgálók)</span><span class="sxs-lookup"><span data-stu-id="216da-189">Examples of these errors include: broken VHD chain; Invalid state for hello replica VM; Network authentication errors: authorization errors; VM not found errors (for standalone Hyper-V servers)</span></span>
<span data-ttu-id="216da-190">**Helyreállítható hibák**</span><span class="sxs-lookup"><span data-stu-id="216da-190">**Recoverable errors**</span></span> | <span data-ttu-id="216da-191">A próbálkozások minden replikációs időköztől, használja az exponenciális vissza-ki, amely növeli a hello újrapróbálkozási időköz hello indítás hello első kísérlet 1, 2, 4, 8, és 10 perc.</span><span class="sxs-lookup"><span data-stu-id="216da-191">Retries occur every replication interval, using an exponential back-off that increases hello retry interval from hello start of hello first attempt by 1, 2, 4, 8, and 10 minutes.</span></span> <span data-ttu-id="216da-192">Ha a hiba nem szűnik meg, a rendszer 30 percenként újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="216da-192">If an error persists, retry every 30 minutes.</span></span> <span data-ttu-id="216da-193">Példák: hálózati hibák; nem elegendő lemezterületből fakadó hibák; alacsony memóriaállapot.</span><span class="sxs-lookup"><span data-stu-id="216da-193">Examples include: network errors; low disk  errors; low memory conditions</span></span> |



## <a name="failover-and-failback-process"></a><span data-ttu-id="216da-194">Feladatátvételi és feladat-visszavételi folyamat</span><span class="sxs-lookup"><span data-stu-id="216da-194">Failover and failback process</span></span>

1. <span data-ttu-id="216da-195">Futtathatja a tervezett vagy nem tervezett [feladatátvételi](site-recovery-failover.md) a helyszíni Hyper-V virtuális gépek tooAzure.</span><span class="sxs-lookup"><span data-stu-id="216da-195">You can run a planned or unplanned [failover](site-recovery-failover.md) from on-premises Hyper-V VMs tooAzure.</span></span> <span data-ttu-id="216da-196">Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.</span><span class="sxs-lookup"><span data-stu-id="216da-196">If you run a planned failover, then source VMs are shut down tooensure no data loss.</span></span>
2. <span data-ttu-id="216da-197">Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.</span><span class="sxs-lookup"><span data-stu-id="216da-197">You can fail over a single machine, or create [recovery plans](site-recovery-create-recovery-plans.md) tooorchestrate failover of multiple machines.</span></span>
4. <span data-ttu-id="216da-198">Hello feladatátvétel futtatása után meg kell tudni toosee hello replika virtuális gép létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="216da-198">After you run hello failover, you should be able toosee hello created replica VMs in Azure.</span></span> <span data-ttu-id="216da-199">A nyilvános IP-cím toohello VM rendelhet is, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="216da-199">You can assign a public IP address toohello VM if required.</span></span>
5. <span data-ttu-id="216da-200">Majd véglegesíteni hello feladatátvételi toostart hello replika Azure virtuális gép hello alkalmazások és szolgáltatások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="216da-200">You then commit hello failover toostart accessing hello workload from hello replica Azure VM.</span></span>
6. <span data-ttu-id="216da-201">Amint az elsődleges helyszíni hely megint elérhetővé válik, [visszaadhatja](site-recovery-failback-from-azure-to-hyper-v.md) a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="216da-201">When your primary on-premises site is available again, you can [fail back](site-recovery-failback-from-azure-to-hyper-v.md).</span></span> <span data-ttu-id="216da-202">Egy tervezett feladatátvételt az Azure toohello elsődleges helyről indítsa el.</span><span class="sxs-lookup"><span data-stu-id="216da-202">You kick off a planned failover from Azure toohello primary site.</span></span> <span data-ttu-id="216da-203">A tervezett feladatátvételhez is válassza toofailback toohello azonos virtuális gép vagy tooan másik helyre, és szinkronizálja a módosítása Azure és a helyszíni, tooensure között adatvesztés nélküli</span><span class="sxs-lookup"><span data-stu-id="216da-203">For a planned failover you can select toofailback toohello same VM or tooan alternate location, and synchronize changes between Azure and on-premises, tooensure no data loss.</span></span> <span data-ttu-id="216da-204">Virtuális gépek létrehozásakor a helyszíni, hello feladatátvételi véglegesítést.</span><span class="sxs-lookup"><span data-stu-id="216da-204">When VMs are created on-premises, you commit hello failover.</span></span>




## <a name="next-steps"></a><span data-ttu-id="216da-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="216da-205">Next steps</span></span>

<span data-ttu-id="216da-206">Felülvizsgálati hello [támogatási mátrix](site-recovery-support-matrix-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="216da-206">Review hello [support matrix](site-recovery-support-matrix-to-azure.md)</span></span>