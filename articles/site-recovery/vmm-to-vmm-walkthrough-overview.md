---
title: "Hyper-V virtuális gépek replikálása az Azure Site Recovery VMM másodlagos hely |} Microsoft Docs"
description: "Áttekintést nyújt a Hyper-V virtuális gépek replikálása egy másodlagos VMM-hely, az Azure portál használatával."
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
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a><span data-ttu-id="b2c3a-103">Hyper-V virtuális gépek replikálása másodlagos VMM-hely VMM-felhőkben</span><span class="sxs-lookup"><span data-stu-id="b2c3a-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b2c3a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b2c3a-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="b2c3a-105">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="b2c3a-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="b2c3a-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b2c3a-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="b2c3a-107">Ez a cikk ismerteti a System Center Virtual Machine Manager (VMM) felügyelt helyszíni Hyper-V virtuális gépek (VM) replikálásához szükséges lépések áttekintését felhők, a másodlagos helyre VMM használatával [Azure Site Recovery](site-recovery-overview.md) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-107">This article provides an overview of the steps required to replicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, to a secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in the Azure portal.</span></span>

<span data-ttu-id="b2c3a-108">A cikk elolvasása után felmerülő megjegyzéseit alul vagy az [Azure Recovery Services fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti közzé.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-108">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-the-scenario-architecture"></a><span data-ttu-id="b2c3a-109">1. lépés:, Tekintse át a kialakítandó architektúra</span><span class="sxs-lookup"><span data-stu-id="b2c3a-109">Step 1: Review the scenario architecture</span></span>

<span data-ttu-id="b2c3a-110">Központi telepítés megkezdése előtt tekintse át a kialakítandó architektúra, és győződjön meg arról, hogy tudomásul veszi a telepítendő összetevők.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-110">Before you start deployment, review the scenario architecture, and make sure that you understand all the components you need to deploy.</span></span>

<span data-ttu-id="b2c3a-111">Ugrás a [1. lépés: az architektúra áttekintése](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-111">Go to [Step 1: Review the architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="b2c3a-112">2. lépés: Tekintse át az Előfeltételek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="b2c3a-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="b2c3a-113">Győződjön meg arról, hogy tudomásul veszi a telepítési előfeltételek és korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-113">Make sure that you understand the deployment prerequisites and limitations.</span></span>

<span data-ttu-id="b2c3a-114">**Azure-Előfeltételek**: a Microsoft Azure-előfizetés és Azure Recovery Services-tároló, összehangolására és-replikáció kezelésére van szükség.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, to orchestrate and manage replication.</span></span>
<span data-ttu-id="b2c3a-115">**A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek**: Ellenőrizze, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek a szabályzatnak megfelelő és előkészített a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="b2c3a-116">Ugrás a [2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-116">Go to [Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="b2c3a-117">3. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="b2c3a-117">Step 3: Plan networking</span></span>

<span data-ttu-id="b2c3a-118">Kell tennie az egyes hálózati biztosításához állíthatja be a hálózatra való leképezést a forgatókönyv központi telepítésekor a VMM Virtuálisgép-hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-118">You need to do some network planning to ensure that you can configure network mapping between VMM VM networks when you deploy the scenario.</span></span>

<span data-ttu-id="b2c3a-119">Ugrás a [3. lépés: hálózati terv](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-119">Go to [Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="b2c3a-120">4. lépés: A VMM és a Hyper-V előkészítése</span><span class="sxs-lookup"><span data-stu-id="b2c3a-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="b2c3a-121">A VMM-kiszolgáló és a Hyper-V gazdagépek előkészítése a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-121">Prepare the VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="b2c3a-122">Ugrás a [4. lépés: készítse elő a helyszíni kiszolgálók](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-122">Go to [Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="b2c3a-123">5. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="b2c3a-124">Recovery Services-tároló beállítása.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="b2c3a-125">A tároló konfigurációs beállításokat tartalmaz, és koordinálja a replikációt.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-125">The vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="b2c3a-126">[5. lépés: Állítson be egy tároló](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="b2c3a-127">6. lépés: A forrás és cél beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="b2c3a-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="b2c3a-128">A forrás és cél replikációs VMM helyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-128">Set up the source and target replication VMM locations.</span></span> <span data-ttu-id="b2c3a-129">A VMM-kiszolgáló hozzáadása a tárolóhoz, és töltse le a telepítőfájlokat, a Site Recovery-összetevők.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-129">Add the VMM servers to the vault, and download the installation files for Site Recovery components.</span></span> <span data-ttu-id="b2c3a-130">Azure Site Recovery Provider telepítőjének futtatása a VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-130">Run Azure Site Recovery Provider setup on the VMM server.</span></span> <span data-ttu-id="b2c3a-131">A telepítő telepíti a szolgáltatót a VMM-kiszolgálón, és regisztrálja a kiszolgálót a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-131">Setup installs the Provider on the VMM server, and registers the server in the vault.</span></span> <span data-ttu-id="b2c3a-132">A Microsoft Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-132">You install the Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="b2c3a-133">Ugrás a [6. lépés: a forrás és cél beállítások](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-133">Go to [Step 6: Set up the source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="b2c3a-134">7. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2c3a-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="b2c3a-135">Képezze le a forrás és cél helyek a VMM Virtuálisgép-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-135">Map VMM VM networks in the source and target locations.</span></span> <span data-ttu-id="b2c3a-136">A feladatátvétel után a célhálózat, amely leképezhető a forrás Virtuálisgép-hálózat, amelyben a forrás Hyper-V virtuális Gépen található virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-136">After failover, VMs are created in the target network that maps to the source VM network in which the source Hyper-V VM is located.</span></span>

<span data-ttu-id="b2c3a-137">Ugrás a [7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-137">Go to [Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="b2c3a-138">8. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="b2c3a-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="b2c3a-139">Adja meg, hogyan virtuális gépeket a rendszer replikálja a VMM-helyek között.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="b2c3a-140">Ugrás a [8. lépés: állítson be egy replikációs házirendet](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-140">Go to [Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="b2c3a-141">9. lépés: Virtuális gépek replikációjának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b2c3a-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="b2c3a-142">Válassza ki a replikálni kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-142">Select the VMs you want to replicate.</span></span> <span data-ttu-id="b2c3a-143">A másodlagos hely, a folyamatban lévő változások replikálása után a kezdeti replikáció engedélyezése egy virtuális Gépet a replikálási váltja ki.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-143">Enabling a VM for replication triggers the initial replication to the secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="b2c3a-144">Ugrás a [9. lépés: engedélyezze a replikálást](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-144">Go to [Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="b2c3a-145">10. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="b2c3a-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="b2c3a-146">Egy feladatátvételi teszt futtatásával ellenőrizze, hogy minden a vártnak megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="b2c3a-146">Run a test failover to make sure everything's working as expected.</span></span>

<span data-ttu-id="b2c3a-147">Ugrás a [10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="b2c3a-147">Go to [Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
