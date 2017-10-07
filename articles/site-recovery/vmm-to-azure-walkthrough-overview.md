---
title: "a VMM-ben a Hyper-V virtuális gépek aaaReplicate felhők tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Áttekintést nyújt a VMM-felhők tooAzure hello Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek replikálása"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a><span data-ttu-id="25eb9-103">A VMM-felhők tooAzure hello Azure-portálon a Site Recovery segítségével a Hyper-V virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="25eb9-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="25eb9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="25eb9-104">Azure portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="25eb9-105">Klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="25eb9-105">Azure classic</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="25eb9-106">PowerShell Resource Manager</span><span class="sxs-lookup"><span data-stu-id="25eb9-106">PowerShell Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="25eb9-107">Klasszikus PowerShell</span><span class="sxs-lookup"><span data-stu-id="25eb9-107">PowerShell classic</span></span>](site-recovery-deploy-with-powershell.md)


<span data-ttu-id="25eb9-108">Ez a cikk ismerteti a hello lépéseket szükséges tooreplicate áttekintését a helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="25eb9-108">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="25eb9-109">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="25eb9-109">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="25eb9-110">1. lépés: Hello kialakítandó architektúra áttekintése</span><span class="sxs-lookup"><span data-stu-id="25eb9-110">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="25eb9-111">Indítsa el a központi telepítés, tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy tudomásul veszi összetevők hello toodeploy kell.</span><span class="sxs-lookup"><span data-stu-id="25eb9-111">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="25eb9-112">Nyissa meg túl[1. lépés: hello architektúra áttekintése](vmm-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-112">Go too[Step 1: Review hello architecture](vmm-to-azure-walkthrough-architecture.md)</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="25eb9-113">2. lépés: Tekintse át az Előfeltételek és korlátozások</span><span class="sxs-lookup"><span data-stu-id="25eb9-113">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="25eb9-114">Győződjön meg arról, hogy tudomásul veszi hello telepítési előfeltételek és korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="25eb9-114">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="25eb9-115">**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.</span><span class="sxs-lookup"><span data-stu-id="25eb9-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
<span data-ttu-id="25eb9-116">**A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek**: Ellenőrizze, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek a szabályzatnak megfelelő és előkészített a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="25eb9-116">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>
<span data-ttu-id="25eb9-117">**Virtuális gépek replikálása**: Ellenőrizze, hogy a virtuális gépek tooreplicate kívánt felel meg az Azure követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="25eb9-117">**Replicated VMs**: Check that VMs you want tooreplicate comply with Azure requirements.</span></span>

<span data-ttu-id="25eb9-118">Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmm-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-118">Go too[Step 2: Verify prerequisites and limitations](vmm-to-azure-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="25eb9-119">3. lépés: Csomag kapacitás</span><span class="sxs-lookup"><span data-stu-id="25eb9-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="25eb9-120">Ha végzett teljes körű telepítésére, meg kell toofigure milyen replikációs erőforrásokat kell.</span><span class="sxs-lookup"><span data-stu-id="25eb9-120">If you're doing a full deployment, you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="25eb9-121">Néhány az eszközök elérhető toohelp ehhez.</span><span class="sxs-lookup"><span data-stu-id="25eb9-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="25eb9-122">Ha végzett a gyors tootest hello környezet kialakítása, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="25eb9-122">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="25eb9-123">Nyissa meg túl[3. lépés: kapacitástervezés](vmm-to-azure-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-123">Go too[Step 3: Plan capacity](vmm-to-azure-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="25eb9-124">4. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="25eb9-124">Step 4: Plan networking</span></span>

<span data-ttu-id="25eb9-125">Meg kell toodo bizonyos hálózati tervezési tooensure hálózatleképezés hello esetben telepítésekor konfigurálható, hogy Azure virtuális gépekhez csatlakoztatott tooAzure virtuális hálózatok után lesz feladatátvételt hajt végre, és, hogy hozzá vannak rendelve megfelelő IP-címek.</span><span class="sxs-lookup"><span data-stu-id="25eb9-125">You need toodo some network planning tooensure that you can configure network mapping when you deploy hello scenario, that Azure VMs will be connected tooAzure virtual networks after failover occurs, and that that they're assigned appropriate IP addresses.</span></span>

<span data-ttu-id="25eb9-126">Nyissa meg túl[4. lépés: hálózat tervezése](vmm-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-126">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md)</span></span>


## <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="25eb9-127">5. lépés: Felkészülés az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="25eb9-127">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="25eb9-128">Az Azure-fiók, a hálózatok és a tárolás beállítása.</span><span class="sxs-lookup"><span data-stu-id="25eb9-128">Set up an Azure account, networks, and storage.</span></span> <span data-ttu-id="25eb9-129">Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="25eb9-129">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="25eb9-130">Nyissa meg túl[5. lépés: Azure előkészítése](vmm-to-azure-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-130">Go too[Step 5: Prepare Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span></span>

## <a name="step-6-prepare-vmm-and-hyper-v"></a><span data-ttu-id="25eb9-131">6. lépés: Készítse elő a VMM és a Hyper-V</span><span class="sxs-lookup"><span data-stu-id="25eb9-131">Step 6: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="25eb9-132">Hello helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="25eb9-132">Prepare hello on-premises VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="25eb9-133">Nyissa meg túl[6. lépés: a helyszíni kiszolgálók előkészítése](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-133">Go too[Step 6: Prepare on-premises servers](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="25eb9-134">7. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="25eb9-134">Step 7: Set up a vault</span></span>

<span data-ttu-id="25eb9-135">Recovery Services-tároló beállítása.</span><span class="sxs-lookup"><span data-stu-id="25eb9-135">Set up a Recovery Services vault.</span></span> <span data-ttu-id="25eb9-136">hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.</span><span class="sxs-lookup"><span data-stu-id="25eb9-136">hello vault contains configuration settings, and orchestrates replication.</span></span>

[<span data-ttu-id="25eb9-137">7. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="25eb9-137">Step 7: Set up a vault</span></span>](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="25eb9-138">8. lépés: A forrás- és beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25eb9-138">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="25eb9-139">Hello forrás és cél replikációs helyek beállítása.</span><span class="sxs-lookup"><span data-stu-id="25eb9-139">Set up hello source and target replication locations.</span></span> <span data-ttu-id="25eb9-140">Hello VMM server toohello tároló hozzáadása, és a Site Recovery-összetevők hello fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="25eb9-140">Add hello VMM server toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="25eb9-141">Futtassa az Azure Site Recovery Provider telepítőjét hello VMM-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="25eb9-141">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="25eb9-142">A telepítő telepíti a hello szolgáltató hello VMM-kiszolgálón, valamint hello-kiszolgáló regisztrálása hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="25eb9-142">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="25eb9-143">Hello Microsoft Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="25eb9-143">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="25eb9-144">Nyissa meg túl[8. lépés: forrás és cél beállításainak konfigurálása](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-144">Go too[Step 8: Configure source and target settings](vmm-to-azure-walkthrough-source-target.md)</span></span>

## <a name="step-9-configure-network-mapping"></a><span data-ttu-id="25eb9-145">9. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25eb9-145">Step 9: Configure network mapping</span></span>

<span data-ttu-id="25eb9-146">Térkép helyszíni VMM-Virtuálisgép-hálózatok tooAzure virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="25eb9-146">Map on-premises VMM VM networks tooAzure virtual networks.</span></span> <span data-ttu-id="25eb9-147">A feladatátvétel után Azure virtuális gépek jönnek létre hello Azure-hálózatot, amely leképezhető toohello helyszíni Virtuálisgép-hálózat mely hello a Hyper-V adatforrás található.</span><span class="sxs-lookup"><span data-stu-id="25eb9-147">After failover, Azure VMs are created in hello Azure network that maps toohello on-premises VM network in which hello source Hyper-V is located.</span></span>

<span data-ttu-id="25eb9-148">Nyissa meg túl[9. lépés: hálózatleképezés konfigurálása](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-148">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>


## <a name="step-10-set-up-a-replication-policy"></a><span data-ttu-id="25eb9-149">10. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="25eb9-149">Step 10: Set up a replication policy</span></span>

<span data-ttu-id="25eb9-150">Adja meg, hogyan a helyszíni virtuális gépek replikált tooAzure lesz.</span><span class="sxs-lookup"><span data-stu-id="25eb9-150">Specify how on-premises VMs will be replicated tooAzure.</span></span>

<span data-ttu-id="25eb9-151">Nyissa meg túl[10. lépés: a replikációs házirend beállítása](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-151">Go too[Step 10: Set up a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>


## <a name="step-11-enable-replication-for-vms"></a><span data-ttu-id="25eb9-152">11. lépés: Virtuális gépek replikációjának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="25eb9-152">Step 11: Enable replication for VMs</span></span>

<span data-ttu-id="25eb9-153">Válassza ki a kívánt tooreplicate hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="25eb9-153">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="25eb9-154">A folyamatban lévő változások replikálása követ egy virtuális Gépet a replikálási eseményindítókat hello kezdeti replikálás tooAzure engedélyezése esetén.</span><span class="sxs-lookup"><span data-stu-id="25eb9-154">ENabling a VM for replication triggers hello initial replication tooAzure, followed by ongoing delta replication.</span></span>

<span data-ttu-id="25eb9-155">Nyissa meg túl[11. lépés: replikálás engedélyezése](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-155">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="25eb9-156">12. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="25eb9-156">Step 12: Run a test failover</span></span>

<span data-ttu-id="25eb9-157">Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="25eb9-157">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="25eb9-158">Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmm-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="25eb9-158">Go too[Step 12: Run a test failover](vmm-to-azure-walkthrough-test-failover.md)</span></span>


