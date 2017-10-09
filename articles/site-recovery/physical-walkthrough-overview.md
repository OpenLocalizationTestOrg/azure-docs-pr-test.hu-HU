---
title: "fizikai aaaReplicate a helyszíni kiszolgálók tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Hello lépéseket áttekintést nyújt a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure az Azure Site Recovery szolgáltatás hello munkaterheléseinek replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a><span data-ttu-id="72f2d-103">A Site Recovery fizikai kiszolgálók tooAzure replikálása</span><span class="sxs-lookup"><span data-stu-id="72f2d-103">Replicate physical servers tooAzure with Site Recovery</span></span>

<span data-ttu-id="72f2d-104">Ez a cikk áttekintést hello lépéseket szükséges tooreplicate helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="72f2d-104">This article provides an overview of hello steps required tooreplicate on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="72f2d-105">1. lépés: Tekintse át a architektúra és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72f2d-105">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="72f2d-106">Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és így megértheti, hogy minden hello összetevőket tooset hello központi telepítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="72f2d-106">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need tooset up hello deployment.</span></span>

<span data-ttu-id="72f2d-107">Nyissa meg túl[1. lépés: hello architektúra áttekintése](physical-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-107">Go too[Step 1: Review hello architecture](physical-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="72f2d-108">2. lépés: Felülvizsgálati Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72f2d-108">Step 2: Review prerequisites</span></span>

<span data-ttu-id="72f2d-109">Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:</span><span class="sxs-lookup"><span data-stu-id="72f2d-109">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="72f2d-110">**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.</span><span class="sxs-lookup"><span data-stu-id="72f2d-110">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="72f2d-111">**A helyszíni Site Recovery-összetevőkhöz**: egy a helyszíni Site Recovery összetevőt futtató gép van szüksége.</span><span class="sxs-lookup"><span data-stu-id="72f2d-111">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="72f2d-112">**A replikált gép**: kiszolgálókat szeretne tooreplicate kell toocomply a helyszíni és az Azure-követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="72f2d-112">**Replicated machines**: Servers you want tooreplicate need toocomply with on-premises and Azure requirements.</span></span>

<span data-ttu-id="72f2d-113">Nyissa meg túl[2. lépés: tekintse át az Előfeltételek és korlátozások](physical-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-113">Go too[Step 2: Review prerequisites and limitations](physical-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="72f2d-114">3. lépés: Csomag kapacitás</span><span class="sxs-lookup"><span data-stu-id="72f2d-114">Step 3: Plan capacity</span></span>

<span data-ttu-id="72f2d-115">Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania.</span><span class="sxs-lookup"><span data-stu-id="72f2d-115">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="72f2d-116">Ha végzett a gyors tootest hello környezet kialakítása, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="72f2d-116">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="72f2d-117">Nyissa meg túl[3. lépés: kapacitástervezés](physical-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-117">Go too[Step 3: Plan capacity](physical-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="72f2d-118">4. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="72f2d-118">Step 4: Plan networking</span></span>

<span data-ttu-id="72f2d-119">Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.</span><span class="sxs-lookup"><span data-stu-id="72f2d-119">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="72f2d-120">Nyissa meg túl[4. lépés: hálózat tervezése](physical-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-120">Go too[Step 4: Plan networking](physical-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="72f2d-121">5. lépés: Felkészülés az Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="72f2d-121">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="72f2d-122">Azure-hálózatok és a tárolás beállítása az megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="72f2d-122">Set up Azure networks and storage before you start.</span></span> 

<span data-ttu-id="72f2d-123">Nyissa meg túl[5. lépés: Azure előkészítése](physical-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-123">Go too[Step 5: Prepare Azure](physical-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-set-up-a-vault"></a><span data-ttu-id="72f2d-124">6. lépés: Állítson be egy tárolóban.</span><span class="sxs-lookup"><span data-stu-id="72f2d-124">Step 6: Set up a vault</span></span>

<span data-ttu-id="72f2d-125">A Recovery Services-tároló tooorchestrate beállításában és -replikáció kezelésére.</span><span class="sxs-lookup"><span data-stu-id="72f2d-125">You set up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="72f2d-126">Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.</span><span class="sxs-lookup"><span data-stu-id="72f2d-126">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="72f2d-127">Nyissa meg túl[6. lépés: állítson be egy tárolóban.](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-127">Go too[Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>

## <a name="step-7-configure-source-and-target-settings"></a><span data-ttu-id="72f2d-128">7. lépés: A forrás- és beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72f2d-128">Step 7: Configure source and target settings</span></span>

<span data-ttu-id="72f2d-129">Hello forrás beállításainak konfigurálása, és a célhely (Azure).</span><span class="sxs-lookup"><span data-stu-id="72f2d-129">Configure settings for hello source and target (Azure) site.</span></span> <span data-ttu-id="72f2d-130">Az adatforrás-beállítások magában foglalja az egységes telepítő tooinstall hello helyszíni Site Recovery összetevőt futtató.</span><span class="sxs-lookup"><span data-stu-id="72f2d-130">Source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="72f2d-131">Nyissa meg túl[7. lépés: állítson be hello forrása és célja](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-131">Go too[Step 7: Set up hello source and target](physical-walkthrough-source-target.md)</span></span>

## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="72f2d-132">8. lépés: A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="72f2d-132">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="72f2d-133">Állít be a házirend toospecify hogyan fizikai kiszolgálók replikálása kell.</span><span class="sxs-lookup"><span data-stu-id="72f2d-133">You set up a policy toospecify how physical servers should replicate.</span></span>

<span data-ttu-id="72f2d-134">Nyissa meg túl[8. lépés: a replikációs házirend beállítása](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>

## <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="72f2d-135">9. lépés: Hello mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="72f2d-135">Step 9: Install hello Mobility service</span></span>

<span data-ttu-id="72f2d-136">hello mobilitási szolgáltatást telepíteni kell minden kiszolgálón tooreplicate szeretné.</span><span class="sxs-lookup"><span data-stu-id="72f2d-136">hello Mobility service must be installed on each server you want tooreplicate.</span></span> <span data-ttu-id="72f2d-137">Nincsenek néhány módon tooset hello szolgáltatás, a lekérés és küldés telepítést.</span><span class="sxs-lookup"><span data-stu-id="72f2d-137">There are a few ways tooset up hello service, with push or pull installation.</span></span>

<span data-ttu-id="72f2d-138">Nyissa meg túl[9. lépés: hello mobilitási szolgáltatás telepítése](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-138">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>

## <a name="step-10-enable-replication"></a><span data-ttu-id="72f2d-139">10. lépés: A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="72f2d-139">Step 10: Enable replication</span></span>

<span data-ttu-id="72f2d-140">Miután hello mobilitási szolgáltatás egy kiszolgálón fut, engedélyezze a replikációját.</span><span class="sxs-lookup"><span data-stu-id="72f2d-140">After hello Mobility service is running on a server, you can enable replication for it.</span></span> <span data-ttu-id="72f2d-141">Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.</span><span class="sxs-lookup"><span data-stu-id="72f2d-141">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="72f2d-142">Nyissa meg túl[10. lépés: replikálás engedélyezése](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-142">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="72f2d-143">11. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="72f2d-143">Step 11: Run a test failover</span></span>

<span data-ttu-id="72f2d-144">Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="72f2d-144">After initial replication finishes and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="72f2d-145">Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="72f2d-145">Go too[Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>

