---
title: "az Azure Site Recovery aaaReplicate VMware virtuális gépek tooAzure |} Microsoft Docs"
description: "Hello lépéseket áttekintést nyújt a VMware virtuális gépek tooAzure munkaterheléseinek replikálására"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a><span data-ttu-id="ba33c-103">A Site Recovery VMware virtuális gépek tooAzure replikálása</span><span class="sxs-lookup"><span data-stu-id="ba33c-103">Replicate VMware VMs tooAzure with Site Recovery</span></span>

<span data-ttu-id="ba33c-104">Ez a cikk áttekintést hello lépéseket szükséges tooreplicate helyszíni VMware virtuális gépek tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ba33c-104">This article provides an overview of hello steps required tooreplicate on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


![A központi telepítési folyamat](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

<span data-ttu-id="ba33c-106">**1. ábra: Központi telepítési folyamat összegzése**</span><span class="sxs-lookup"><span data-stu-id="ba33c-106">**Figure 1: Deployment process summary**</span></span>

## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="ba33c-107">1. lépés: Tekintse át a architektúra és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ba33c-107">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="ba33c-108">Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy toodeploy szükséges összes hello összetevők</span><span class="sxs-lookup"><span data-stu-id="ba33c-108">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="ba33c-109">Nyissa meg túl[1. lépés: hello architektúra áttekintése](vmware-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-109">Go too[Step 1: Review hello architecture](vmware-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="ba33c-110">2. lépés: Felülvizsgálati Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ba33c-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="ba33c-111">Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:</span><span class="sxs-lookup"><span data-stu-id="ba33c-111">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="ba33c-112">**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.</span><span class="sxs-lookup"><span data-stu-id="ba33c-112">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="ba33c-113">**A helyszíni Site Recovery-összetevőkhöz**: egy a helyszíni Site Recovery összetevőt futtató gép van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ba33c-113">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="ba33c-114">**Helyszíni VMware Előfeltételek**: fiókok tooset kell, hogy a Site Recovery VMware-kiszolgálók és virtuális gépek hozzáférhessenek.</span><span class="sxs-lookup"><span data-stu-id="ba33c-114">**On-premises VMware prerequisites**: You need tooset up accounts so that Site Recovery can access VMware servers and VMs.</span></span>
- <span data-ttu-id="ba33c-115">**Virtuális gépek replikálása**: virtuális gépek szeretné, hogy tooreplicate kell toocomply az Azure-követelményeknek, és rendelkezik hello mobilitási szolgáltatás összetevőt.</span><span class="sxs-lookup"><span data-stu-id="ba33c-115">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements, and have hello Mobility service component installed.</span></span>

<span data-ttu-id="ba33c-116">Nyissa meg túl[2. lépés: tekintse át az Előfeltételek és korlátozások](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-116">Go too[Step 2: Review prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="ba33c-117">3. lépés: Csomag kapacitás</span><span class="sxs-lookup"><span data-stu-id="ba33c-117">Step 3: Plan capacity</span></span>

<span data-ttu-id="ba33c-118">Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania.</span><span class="sxs-lookup"><span data-stu-id="ba33c-118">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="ba33c-119">Néhány az eszközök elérhető toohelp ehhez.</span><span class="sxs-lookup"><span data-stu-id="ba33c-119">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="ba33c-120">Nyissa meg tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="ba33c-120">Go tooStep 2.</span></span> <span data-ttu-id="ba33c-121">Ha végzett a gyors tootest hello környezet beállítása kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="ba33c-121">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="ba33c-122">Nyissa meg túl[3. lépés: kapacitástervezés](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-122">Go too[Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="ba33c-123">4. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="ba33c-123">Step 4: Plan networking</span></span>

<span data-ttu-id="ba33c-124">Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.</span><span class="sxs-lookup"><span data-stu-id="ba33c-124">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="ba33c-125">Nyissa meg túl[4. lépés: hálózat tervezése](vmware-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-125">Go too[Step 4: Plan networking](vmware-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="ba33c-126">5. lépés: Felkészülés az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="ba33c-126">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="ba33c-127">Azure-hálózatok és a tárolás beállítása az megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ba33c-127">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="ba33c-128">Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ba33c-128">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="ba33c-129">Nyissa meg túl[5. lépés: Azure előkészítése](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-129">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-vmware"></a><span data-ttu-id="ba33c-130">6. lépés: A VMware előkészítése</span><span class="sxs-lookup"><span data-stu-id="ba33c-130">Step 6: Prepare VMware</span></span>

<span data-ttu-id="ba33c-131">Fiókok, amelyek a Site Recovery segítségével tooset lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="ba33c-131">You need tooset up accounts that Site Recovery will use to:</span></span>

- <span data-ttu-id="ba33c-132">Hozzáférés VMware virtualizálási kiszolgálók tooautomatically észleli a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ba33c-132">Access VMware virtualization servers tooautomatically detect VMs.</span></span>
- <span data-ttu-id="ba33c-133">Virtuális gépek tooinstall hello mobilitási szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ba33c-133">Access VMs tooinstall hello Mobility service.</span></span> <span data-ttu-id="ba33c-134">Minden virtuális gép kívánt tooreplicate rendelkeznie kell hello mobilitási szolgáltatás ügynöke telepítve előtt is engedélyezze a replikációját.</span><span class="sxs-lookup"><span data-stu-id="ba33c-134">Each VM you want tooreplicate must have hello Mobility service agent installed before you can enable replication for it.</span></span>

<span data-ttu-id="ba33c-135">Nyissa meg túl[6. lépés: Felkészülés VMware](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-135">Go too[Step 6: Prepare VMware](vmware-walkthrough-prepare-vmware.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="ba33c-136">7. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ba33c-136">Step 7: Set up a vault</span></span>

<span data-ttu-id="ba33c-137">A Recovery Services-tároló tooorchestrate mentése tooset kell, és replikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="ba33c-137">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="ba33c-138">Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.</span><span class="sxs-lookup"><span data-stu-id="ba33c-138">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="ba33c-139">Nyissa meg túl[7. lépés: állítson be egy tárolóban.](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-139">Go too[Step 7: Set up a vault](vmware-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="ba33c-140">8. lépés: A forrás- és beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ba33c-140">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="ba33c-141">Hello forrás és cél-replikációhoz használt beállítása.</span><span class="sxs-lookup"><span data-stu-id="ba33c-141">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="ba33c-142">Adatforrás-beállítások beállítása magában foglalja az egységes telepítő tooinstall hello helyszíni Site Recovery összetevőt futtató.</span><span class="sxs-lookup"><span data-stu-id="ba33c-142">Setting up source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="ba33c-143">Nyissa meg túl[8. lépés: állítson be hello forrása és célja](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-143">Go too[Step 8: Set up hello source and target](vmware-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="ba33c-144">9. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="ba33c-144">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="ba33c-145">Egy házirend toospecify replikációs beállítások vonatkozóan beállított VMware virtuális gépek hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="ba33c-145">You set up a policy toospecify replication settings for VMware VMs in hello vault.</span></span>

<span data-ttu-id="ba33c-146">Nyissa meg túl[9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-146">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>

## <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="ba33c-147">10. lépés: Hello mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="ba33c-147">Step 10: Install hello Mobility service</span></span>

<span data-ttu-id="ba33c-148">kell telepíteni a mobilitási szolgáltatás hello tooreplicate kívánt összes virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="ba33c-148">hello Mobility service must be installed on each VM you want tooreplicate.</span></span> <span data-ttu-id="ba33c-149">Nincsenek néhány módon tooset hello szolgáltatás a lekérés és küldés telepítést.</span><span class="sxs-lookup"><span data-stu-id="ba33c-149">There are a few ways tooset up hello service with push or pull installation.</span></span>

<span data-ttu-id="ba33c-150">Nyissa meg túl[10. lépés: hello mobilitási szolgáltatás telepítése](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-150">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>

## <a name="step-11-enable-replication"></a><span data-ttu-id="ba33c-151">11. lépés: A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ba33c-151">Step 11: Enable replication</span></span>

<span data-ttu-id="ba33c-152">Hello mobilitási szolgáltatás fut a virtuális gép után engedélyezze a replikációját.</span><span class="sxs-lookup"><span data-stu-id="ba33c-152">After hello Mobility service is running on a VM you can enable replication for it.</span></span> <span data-ttu-id="ba33c-153">Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.</span><span class="sxs-lookup"><span data-stu-id="ba33c-153">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="ba33c-154">Nyissa meg túl[11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-154">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>

## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="ba33c-155">12. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="ba33c-155">Step 12: Run a test failover</span></span>

<span data-ttu-id="ba33c-156">Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="ba33c-156">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="ba33c-157">Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="ba33c-157">Go too[Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
