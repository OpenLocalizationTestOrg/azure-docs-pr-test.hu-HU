---
title: "VMware virtuális gépek replikálása az Azure-bA az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "A lépések áttekintést nyújt a replikálása az Azure-bA VMware virtuális gépeken futó számítási feladatok"
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
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a><span data-ttu-id="c3134-103">VMware virtuális gépek replikálása az Azure Site Recovery szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="c3134-103">Replicate VMware VMs to Azure with Site Recovery</span></span>

<span data-ttu-id="c3134-104">Ez a cikk áttekintést lépéseit a helyszíni VMware virtuális gépek replikálása az Azure-ba, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c3134-104">This article provides an overview of the steps required to replicate on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


![A központi telepítési folyamat](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

<span data-ttu-id="c3134-106">**1. ábra: Központi telepítési folyamat összegzése**</span><span class="sxs-lookup"><span data-stu-id="c3134-106">**Figure 1: Deployment process summary**</span></span>

## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="c3134-107">1. lépés: Tekintse át a architektúra és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c3134-107">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="c3134-108">Központi telepítés megkezdése előtt tekintse át a kialakítandó architektúra, és győződjön meg arról is telepíteni kell minden összetevők megismerése</span><span class="sxs-lookup"><span data-stu-id="c3134-108">Before you start deployment, review the scenario architecture, and make sure you understand all the components you need to deploy</span></span>

<span data-ttu-id="c3134-109">Ugrás a [1. lépés: az architektúra áttekintése](vmware-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-109">Go to [Step 1: Review the architecture](vmware-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="c3134-110">2. lépés: Felülvizsgálati Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c3134-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="c3134-111">Győződjön meg arról, hogy az előfeltételek teljesülnek az egyes telepítési összetevők:</span><span class="sxs-lookup"><span data-stu-id="c3134-111">Make sure you have the prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="c3134-112">**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.</span><span class="sxs-lookup"><span data-stu-id="c3134-112">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="c3134-113">**A helyszíni Site Recovery-összetevőkhöz**: egy a helyszíni Site Recovery összetevőt futtató gép van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c3134-113">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="c3134-114">**Helyszíni VMware Előfeltételek**: kell, hogy a Site Recovery férhetnek hozzá VMware-kiszolgálók és virtuális gépek fiókok beállítása.</span><span class="sxs-lookup"><span data-stu-id="c3134-114">**On-premises VMware prerequisites**: You need to set up accounts so that Site Recovery can access VMware servers and VMs.</span></span>
- <span data-ttu-id="c3134-115">**Virtuális gépek replikálása**: replikálni kívánt virtuális gépeket kell ahhoz az Azure-követelményeknek, és rendelkeznie kell a mobilitási szolgáltatás összetevőt.</span><span class="sxs-lookup"><span data-stu-id="c3134-115">**Replicated VMs**: VMs you want to replicate need to comply with Azure requirements, and have the Mobility service component installed.</span></span>

<span data-ttu-id="c3134-116">Ugrás a [2. lépés: tekintse át az Előfeltételek és korlátozások](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-116">Go to [Step 2: Review prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="c3134-117">3. lépés: Csomag kapacitás</span><span class="sxs-lookup"><span data-stu-id="c3134-117">Step 3: Plan capacity</span></span>

<span data-ttu-id="c3134-118">Ha végzett teljes körű telepítésére, ki kell deríteni kell replikációs erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c3134-118">If you're doing a full deployment you need to figure out what replication resources you need.</span></span> <span data-ttu-id="c3134-119">Nincsenek elérhető segítséget nyújthat eszközöket néhány.</span><span class="sxs-lookup"><span data-stu-id="c3134-119">There are a couple of tools available to help you do this.</span></span> <span data-ttu-id="c3134-120">Folytassa a 2.</span><span class="sxs-lookup"><span data-stu-id="c3134-120">Go to Step 2.</span></span> <span data-ttu-id="c3134-121">Ha végzett a gyors másolatot a környezet teszteléséhez kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="c3134-121">If you're doing a quick set up to test the environment you can skip this step.</span></span>

<span data-ttu-id="c3134-122">Folytassa a [3. lépés: Kapacitás megtervezése](vmware-walkthrough-capacity.md) szakasszal.</span><span class="sxs-lookup"><span data-stu-id="c3134-122">Go to [Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="c3134-123">4. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="c3134-123">Step 4: Plan networking</span></span>

<span data-ttu-id="c3134-124">Néhány tervezi, győződjön meg arról, hogy Azure virtuális gépek csatlakoznak feladatátvételt hajt végre, valamint az, hogy rendelkeznek-e a megfelelő IP-címek hálózati elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c3134-124">You need to do some network planning to ensure that Azure VMs are connected to networks after failover occurs, and  that that they have the right IP addresses.</span></span>

<span data-ttu-id="c3134-125">Ugrás a [4. lépés: hálózat tervezése](vmware-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-125">Go to [Step 4: Plan networking](vmware-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="c3134-126">5. lépés: Felkészülés az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="c3134-126">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="c3134-127">Azure-hálózatok és a tárolás beállítása az megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c3134-127">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="c3134-128">Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c3134-128">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="c3134-129">Ugrás a [5. lépés: Azure előkészítése](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-129">Go to [Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-vmware"></a><span data-ttu-id="c3134-130">6. lépés: A VMware előkészítése</span><span class="sxs-lookup"><span data-stu-id="c3134-130">Step 6: Prepare VMware</span></span>

<span data-ttu-id="c3134-131">Meg kell beállítania a fiókokat, amelyek a Site Recovery segítségével:</span><span class="sxs-lookup"><span data-stu-id="c3134-131">You need to set up accounts that Site Recovery will use to:</span></span>

- <span data-ttu-id="c3134-132">VMware virtualizálási kiszolgálók automatikus észlelése a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c3134-132">Access VMware virtualization servers to automatically detect VMs.</span></span>
- <span data-ttu-id="c3134-133">Hozzáférés virtuális gépeket telepíteni a mobilitási szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c3134-133">Access VMs to install the Mobility service.</span></span> <span data-ttu-id="c3134-134">Minden replikálni kívánt virtuális gép rendelkeznie kell a mobilitási szolgáltatás ügynöke telepítve előtt is engedélyezze a replikációját.</span><span class="sxs-lookup"><span data-stu-id="c3134-134">Each VM you want to replicate must have the Mobility service agent installed before you can enable replication for it.</span></span>

<span data-ttu-id="c3134-135">Ugrás a [6. lépés: készítse elő a VMware](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-135">Go to [Step 6: Prepare VMware](vmware-walkthrough-prepare-vmware.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="c3134-136">7. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c3134-136">Step 7: Set up a vault</span></span>

<span data-ttu-id="c3134-137">Meg kell állítania a Recovery Services-tároló összehangolására és-replikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="c3134-137">You need to set up a Recovery Services vault to orchestrate and manage replication.</span></span> <span data-ttu-id="c3134-138">A tároló beállításakor szeretne replikálni, és amennyiben szeretne replikálni úgy, hogy adja meg.</span><span class="sxs-lookup"><span data-stu-id="c3134-138">When you set up the vault, you specify what you want to replicate, and where you want to replicate it to.</span></span>

<span data-ttu-id="c3134-139">Ugrás a [7. lépés: állítson be egy tárolóban.](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-139">Go to [Step 7: Set up a vault](vmware-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="c3134-140">8. lépés: A forrás- és beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c3134-140">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="c3134-141">A forrás és cél-replikációhoz használt beállítása.</span><span class="sxs-lookup"><span data-stu-id="c3134-141">Set up the source and target that's used for replication.</span></span> <span data-ttu-id="c3134-142">Adatforrás-beállítások beállítása magában foglalja az egységes telepítő a helyszíni Site Recovery-összetevők telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c3134-142">Setting up source settings includes running Unified Setup to install the on-premises Site Recovery components.</span></span>

<span data-ttu-id="c3134-143">Ugrás a [8. lépés: a forrás és cél beállítása](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-143">Go to [Step 8: Set up the source and target](vmware-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="c3134-144">9. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="c3134-144">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="c3134-145">Egy házirend beállítása határozza meg a replikációs beállításokat VMware virtuális gépek esetén a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="c3134-145">You set up a policy to specify replication settings for VMware VMs in the vault.</span></span>

<span data-ttu-id="c3134-146">Ugrás a [9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-146">Go to [Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>

## <a name="step-10-install-the-mobility-service"></a><span data-ttu-id="c3134-147">10. lépés: A mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="c3134-147">Step 10: Install the Mobility service</span></span>

<span data-ttu-id="c3134-148">A mobilitási szolgáltatás minden replikálni kívánt virtuális gép telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c3134-148">The Mobility service must be installed on each VM you want to replicate.</span></span> <span data-ttu-id="c3134-149">Néhány módon állítsa be a szolgáltatás a lekérés és küldés telepítést.</span><span class="sxs-lookup"><span data-stu-id="c3134-149">There are a few ways to set up the service with push or pull installation.</span></span>

<span data-ttu-id="c3134-150">Ugrás a [10. lépés: a mobilitási szolgáltatás telepítése](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-150">Go to [Step 10: Install the Mobility service](vmware-walkthrough-install-mobility.md)</span></span>

## <a name="step-11-enable-replication"></a><span data-ttu-id="c3134-151">11. lépés: A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c3134-151">Step 11: Enable replication</span></span>

<span data-ttu-id="c3134-152">Miután a virtuális gép fut a mobilitási szolgáltatás engedélyezze a replikációját.</span><span class="sxs-lookup"><span data-stu-id="c3134-152">After the Mobility service is running on a VM you can enable replication for it.</span></span> <span data-ttu-id="c3134-153">Miután engedélyezte a, a virtuális gép kezdeti replikációs következik be.</span><span class="sxs-lookup"><span data-stu-id="c3134-153">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="c3134-154">Ugrás a [11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-154">Go to [Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>

## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="c3134-155">12. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="c3134-155">Step 12: Run a test failover</span></span>

<span data-ttu-id="c3134-156">Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, győződjön meg arról, hogy minden megfelelően működik-e a teszt feladatátvételt is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="c3134-156">After initial replication finishes, and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="c3134-157">Ugrás a [12. lépés: feladatátvételi teszt futtatása](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="c3134-157">Go to [Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
