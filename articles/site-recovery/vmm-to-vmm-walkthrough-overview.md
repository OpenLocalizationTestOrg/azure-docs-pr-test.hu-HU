---
title: "aaaReplicate Hyper-V virtuális gépek tooa VMM másodlagos hely az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Áttekintést nyújt a Hyper-V virtuális gépek tooa VMM másodlagos hely hello Azure-portál használatával replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a><span data-ttu-id="23c58-103">Hyper-V virtuális gépek VMM felhők tooa másodlagos VMM-hely replikálása</span><span class="sxs-lookup"><span data-stu-id="23c58-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="23c58-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="23c58-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="23c58-105">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="23c58-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="23c58-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="23c58-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="23c58-107">Ez a cikk áttekintést lépéseket szükséges tooreplicate helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők, tooa másodlagos VMM helyre hello segítségével [Azure Site Recovery](site-recovery-overview.md)a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="23c58-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span>

<span data-ttu-id="23c58-108">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="23c58-108">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="23c58-109">1. lépés: Hello kialakítandó architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="23c58-109">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="23c58-110">Indítsa el a központi telepítés, tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy tudomásul veszi összetevők hello toodeploy kell.</span><span class="sxs-lookup"><span data-stu-id="23c58-110">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="23c58-111">Nyissa meg túl[1. lépés: tekintse át a hello architektúra](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-111">Go too[Step 1: Review hello architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="23c58-112">2. lépés: Tekintse át az Előfeltételek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="23c58-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="23c58-113">Győződjön meg arról, hogy tudomásul veszi hello telepítési előfeltételek és korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="23c58-113">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="23c58-114">**Azure-Előfeltételek**: egy Microsoft Azure-előfizetés szükséges, és az Azure Recovery Services használó, tooorchestrate és -replikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="23c58-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, tooorchestrate and manage replication.</span></span>
<span data-ttu-id="23c58-115">**A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek**: Ellenőrizze, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek a szabályzatnak megfelelő és előkészített a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="23c58-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="23c58-116">Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-116">Go too[Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="23c58-117">3. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="23c58-117">Step 3: Plan networking</span></span>

<span data-ttu-id="23c58-118">Toodo kell bizonyos hálózati tervezési tooensure állíthatja be a hálózatra való leképezés hello forgatókönyv központi telepítésekor a VMM Virtuálisgép-hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="23c58-118">You need toodo some network planning tooensure that you can configure network mapping between VMM VM networks when you deploy hello scenario.</span></span>

<span data-ttu-id="23c58-119">Nyissa meg túl[3. lépés: hálózati terv](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-119">Go too[Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="23c58-120">4. lépés: A VMM és a Hyper-V előkészítése</span><span class="sxs-lookup"><span data-stu-id="23c58-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="23c58-121">Hello VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="23c58-121">Prepare hello VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="23c58-122">Nyissa meg túl[4. lépés: készítse elő a helyszíni kiszolgálók](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-122">Go too[Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="23c58-123">5. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="23c58-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="23c58-124">Recovery Services-tároló beállítása.</span><span class="sxs-lookup"><span data-stu-id="23c58-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="23c58-125">hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.</span><span class="sxs-lookup"><span data-stu-id="23c58-125">hello vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="23c58-126">[5. lépés: Állítson be egy tároló](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="23c58-127">6. lépés: A forrás és cél beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="23c58-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="23c58-128">Hello forrás és cél replikációs VMM helyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="23c58-128">Set up hello source and target replication VMM locations.</span></span> <span data-ttu-id="23c58-129">Hello VMM kiszolgálók toohello tároló hozzáadása, és a Site Recovery-összetevők hello fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="23c58-129">Add hello VMM servers toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="23c58-130">Futtassa az Azure Site Recovery Provider telepítőjét hello VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="23c58-130">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="23c58-131">A telepítő telepíti a hello szolgáltató hello VMM-kiszolgálón, valamint hello-kiszolgáló regisztrálása hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="23c58-131">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="23c58-132">Hello Microsoft Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="23c58-132">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="23c58-133">Nyissa meg túl[6. lépés: hello forrása és célja beállítások](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-133">Go too[Step 6: Set up hello source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="23c58-134">7. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23c58-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="23c58-135">Képezze le a VMM Virtuálisgép-hálózatok hello forrás és cél helyeket.</span><span class="sxs-lookup"><span data-stu-id="23c58-135">Map VMM VM networks in hello source and target locations.</span></span> <span data-ttu-id="23c58-136">A feladatátvétel után virtuális gépek jönnek létre hello célhálózat, hogy a maps toohello forrás Virtuálisgép-hálózat mely hello a Hyper-V virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="23c58-136">After failover, VMs are created in hello target network that maps toohello source VM network in which hello source Hyper-V VM is located.</span></span>

<span data-ttu-id="23c58-137">Nyissa meg túl[7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-137">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="23c58-138">8. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="23c58-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="23c58-139">Adja meg, hogyan virtuális gépeket a rendszer replikálja a VMM-helyek között.</span><span class="sxs-lookup"><span data-stu-id="23c58-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="23c58-140">Nyissa meg túl[8. lépés: állítson be egy replikációs házirendet](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-140">Go too[Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="23c58-141">9. lépés: Virtuális gépek replikációjának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="23c58-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="23c58-142">Válassza ki a kívánt tooreplicate hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="23c58-142">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="23c58-143">A folyamatban lévő változások replikálása engedélyezése egy virtuális Gépet a replikálási eseményindítókat hello kezdeti replikálás toohello másodlagos helyet, majd.</span><span class="sxs-lookup"><span data-stu-id="23c58-143">Enabling a VM for replication triggers hello initial replication toohello secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="23c58-144">Nyissa meg túl[9. lépés: engedélyezze a replikálást](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-144">Go too[Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="23c58-145">10. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="23c58-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="23c58-146">Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="23c58-146">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="23c58-147">Nyissa meg túl[10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="23c58-147">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
