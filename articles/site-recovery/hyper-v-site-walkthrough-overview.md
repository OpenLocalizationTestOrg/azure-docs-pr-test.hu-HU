---
title: "az Azure Site Recovery aaaReplicate Hyper-V virtuális gépek tooAzure |} Microsoft Docs"
description: "Ismerteti, hogyan tooorchestrate replikációjának, feladatátvételének és helyreállításának helyszíni Hyper-V virtuális gépek tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a><span data-ttu-id="7814e-103">Hyper-V virtuális gépek (VMM nélkül) tooAzure replikálása</span><span class="sxs-lookup"><span data-stu-id="7814e-103">Replicate Hyper-V virtual machines (without VMM) tooAzure</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7814e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7814e-104">Azure portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="7814e-105">Klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="7814e-105">Azure classic</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
> * [<span data-ttu-id="7814e-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7814e-106">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

<span data-ttu-id="7814e-107">Ez a cikk áttekintést hello szükséges lépések tooreplicate helyszíni Hyper-V virtuális gépek tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7814e-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span> <span data-ttu-id="7814e-108">A központi telepítés a Hyper-V virtuális gépek felügyelete nem a System Center Virtual Machine Manager (VMM).</span><span class="sxs-lookup"><span data-stu-id="7814e-108">In this deployment Hyper-V VMs aren't managed by System Center Virtual Machine Manager (VMM).</span></span>


<span data-ttu-id="7814e-109">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7814e-109">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="7814e-110">1. lépés: Tekintse át a architektúra és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7814e-110">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="7814e-111">Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy toodeploy szükséges összes hello összetevők</span><span class="sxs-lookup"><span data-stu-id="7814e-111">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="7814e-112">Nyissa meg túl[1. lépés: hello architektúra áttekintése](hyper-v-site-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-112">Go too[Step 1: Review hello architecture](hyper-v-site-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="7814e-113">2. lépés: Felülvizsgálati Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7814e-113">Step 2: Review prerequisites</span></span>

<span data-ttu-id="7814e-114">Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:</span><span class="sxs-lookup"><span data-stu-id="7814e-114">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="7814e-115">**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.</span><span class="sxs-lookup"><span data-stu-id="7814e-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="7814e-116">**Helyszíni Hyper-V előfeltételei**: Győződjön meg arról, hogy a Hyper-V-gazdagépek van előkészítve a Site Recovery üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="7814e-116">**On-premises Hyper-V prerequisites**: Make sure Hyper-V hosts are prepared for Site Recovery deployment.</span></span>
- <span data-ttu-id="7814e-117">**Virtuális gépek replikálása**: kívánt tooreplicate kell toocomply Azure-követelményekkel rendelkező virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="7814e-117">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements.</span></span>

<span data-ttu-id="7814e-118">Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-118">Go too[Step 2: Verify prerequisites and limitations](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="7814e-119">3. lépés: Csomag kapacitás</span><span class="sxs-lookup"><span data-stu-id="7814e-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="7814e-120">Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania.</span><span class="sxs-lookup"><span data-stu-id="7814e-120">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="7814e-121">Néhány az eszközök elérhető toohelp ehhez.</span><span class="sxs-lookup"><span data-stu-id="7814e-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="7814e-122">Nyissa meg tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="7814e-122">Go tooStep 2.</span></span> <span data-ttu-id="7814e-123">Ha végzett a gyors tootest hello környezet beállítása kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="7814e-123">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="7814e-124">Nyissa meg túl[3. lépés: kapacitástervezés](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-124">Go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="7814e-125">4. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="7814e-125">Step 4: Plan networking</span></span>

<span data-ttu-id="7814e-126">Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.</span><span class="sxs-lookup"><span data-stu-id="7814e-126">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="7814e-127">Nyissa meg túl[4. lépés: hálózat tervezése](hyper-v-site-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-127">Go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="7814e-128">5. lépés: Felkészülés az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="7814e-128">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="7814e-129">Azure-hálózatok és a tárolás beállítása az megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="7814e-129">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="7814e-130">Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="7814e-130">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="7814e-131">Nyissa meg túl[5. lépés: Azure előkészítése](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-131">Go too[Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-hyper-v"></a><span data-ttu-id="7814e-132">6. lépés: Felkészülés a Hyper-V</span><span class="sxs-lookup"><span data-stu-id="7814e-132">Step 6: Prepare Hyper-V</span></span>

<span data-ttu-id="7814e-133">Győződjön meg arról, hogy a Hyper-V kiszolgálók megfelelnek-e a Site Recovery üzembe helyezés feltételeit.</span><span class="sxs-lookup"><span data-stu-id="7814e-133">Make sure that Hyper-V servers meet Site Recovery deployment requirements.</span></span>

<span data-ttu-id="7814e-134">Nyissa meg túl[6. lépés: Felkészülés a Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-134">Go too[Step 6: Prepare Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="7814e-135">7. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7814e-135">Step 7: Set up a vault</span></span>

<span data-ttu-id="7814e-136">A Recovery Services-tároló tooorchestrate mentése tooset kell, és replikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="7814e-136">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="7814e-137">Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.</span><span class="sxs-lookup"><span data-stu-id="7814e-137">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="7814e-138">Nyissa meg túl[7. lépés: a tároló létrehozása](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-138">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="7814e-139">8. lépés: A forrás- és beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7814e-139">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="7814e-140">Hello forrás és cél-replikációhoz használt beállítása.</span><span class="sxs-lookup"><span data-stu-id="7814e-140">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="7814e-141">Adatforrás-beállítások beállítása magában foglalja hozzáadása Hyper-V gazdagépek tooa Hyper-V hely, hello Site Recovery Provider és Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen, és a Recovery Services-tároló hello hello hely regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="7814e-141">Setting up source settings includes adding Hyper-V hosts tooa Hyper-V site, installing hello Site Recovery Provider and Recovery Services agent on each Hyper-V host, and registering hello site in hello Recovery Services vault.</span></span>

<span data-ttu-id="7814e-142">Nyissa meg túl[8. lépés: állítson be hello forrása és célja](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-142">Go too[Step 8: Set up hello source and target](hyper-v-site-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="7814e-143">9. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="7814e-143">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="7814e-144">Egy házirend toospecify replikációs beállítások vonatkozóan beállított Hyper-V virtuális gépek hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="7814e-144">You set up a policy toospecify replication settings for Hyper-V VMs in hello vault.</span></span>

<span data-ttu-id="7814e-145">Nyissa meg túl[9. lépés: a replikációs házirend beállítása](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-145">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>


## <a name="step-10-enable-replication"></a><span data-ttu-id="7814e-146">10. lépés: A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7814e-146">Step 10: Enable replication</span></span>

<span data-ttu-id="7814e-147">Miután egy replikációs házirendet helyen, miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.</span><span class="sxs-lookup"><span data-stu-id="7814e-147">After you have a replication policy in place,  After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="7814e-148">Nyissa meg túl[10. lépés: replikálás engedélyezése](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-148">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="7814e-149">11. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="7814e-149">Step 11: Run a test failover</span></span>

<span data-ttu-id="7814e-150">Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="7814e-150">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="7814e-151">Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="7814e-151">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
