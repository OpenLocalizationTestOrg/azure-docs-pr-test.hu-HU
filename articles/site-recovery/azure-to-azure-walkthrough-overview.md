---
title: "aaaReplicate Azure virtuális gépek Azure-régiók közötti |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket szükséges tooreplicate Azure virtuális gépek közötti hello Azure Site Recovery szolgáltatás hello Azure-portálon az Azure-régiók"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="e5e76-103">Azure virtuális gépek replikálása az Azure Site Recovery régiók között</span><span class="sxs-lookup"><span data-stu-id="e5e76-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

><span data-ttu-id="e5e76-104">Ez a cikk hello lépéseket szükséges tooreplicate áttekintést nyújt az Azure virtuális gépek (VM) egy Azure-régiót tooAzure virtuális gépeket egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="e5e76-104">This article provides an overview of hello steps required tooreplicate Azure virtual machines (VMs) in one Azure region tooAzure VMs in a different region.</span></span> 

>[!NOTE]
>
> <span data-ttu-id="e5e76-105">Az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="e5e76-105">Azure VM replication is currently in preview.</span></span>

<span data-ttu-id="e5e76-106">Ez a cikk vagy hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="e5e76-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="step-1-review-architecture"></a><span data-ttu-id="e5e76-107">1. lépés: Felülvizsgálati architektúrája</span><span class="sxs-lookup"><span data-stu-id="e5e76-107">Step 1: Review architecture</span></span>

<span data-ttu-id="e5e76-108">Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra és hello összetevők toodeploy van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e5e76-108">Before you start deployment, review hello scenario architecture, and hello components you need toodeploy.</span></span>

<span data-ttu-id="e5e76-109">Nyissa meg túl[1. lépés: hello architektúra áttekintése](azure-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-109">Go too[Step 1: Review hello architecture](azure-to-azure-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="e5e76-110">2. lépés: Felülvizsgálati Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5e76-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="e5e76-111">Ellenőrizze, hogy rendelkezik-e hello helyen, beleértve az előfizetés, a virtuális hálózatok, a storage-fiókok és a virtuális gép Azure-előfeltételeknek.</span><span class="sxs-lookup"><span data-stu-id="e5e76-111">Check that you have hello Azure prerequisites in places, including a subscription, virtual networks, storage accounts, and VM requirements.</span></span>

<span data-ttu-id="e5e76-112">Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-112">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>


## <a name="step-3-plan-networking"></a><span data-ttu-id="e5e76-113">3. lépés: A hálózat megtervezése</span><span class="sxs-lookup"><span data-stu-id="e5e76-113">Step 3: Plan networking</span></span>

<span data-ttu-id="e5e76-114">Ellenőrizze, hogy a kimenő kapcsolat tooreplicate, valamint, hogy be vannak-e beállítva a helyszíni kapcsolatok Azure virtuális gépeken van beállítva.</span><span class="sxs-lookup"><span data-stu-id="e5e76-114">Check that outbound connectivity is set up on Azure VMs you want tooreplicate, and that connections from on-premises are set up.</span></span>

<span data-ttu-id="e5e76-115">Nyissa meg túl[4. lépés: hálózat tervezése](azure-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-115">Go too[Step 4: Plan networking](azure-to-azure-walkthrough-network.md)</span></span>



## <a name="step-4-create-a-vault"></a><span data-ttu-id="e5e76-116">4. lépés: A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5e76-116">Step 4: Create a vault</span></span> 

<span data-ttu-id="e5e76-117">Tooset a Recovery Services-tároló tooorchestrate fel kell és -replikáció kezelésére, és adja meg a hello forrás régió.</span><span class="sxs-lookup"><span data-stu-id="e5e76-117">You need tooset up a Recovery Services vault tooorchestrate and manage replication, and specify hello source region.</span></span>

<span data-ttu-id="e5e76-118">Nyissa meg túl[4. lépés: a tároló létrehozása](azure-to-azure-walkthrough-vault.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-118">Go too[Step 4: Create a vault](azure-to-azure-walkthrough-vault.md)</span></span>


## <a name="step-5-enable-replication"></a><span data-ttu-id="e5e76-119">5. lépés: A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e5e76-119">Step 5: Enable replication</span></span>


<span data-ttu-id="e5e76-120">tooenable replikációs cél hely beállításainak konfigurálása, replikációs házirend beállítása és válassza ki a megjeleníteni kívánt tooreplicate hello Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="e5e76-120">tooenable replication, you configure target location settings, set up a replication policy, and select hello Azure VMs that you want tooreplicate.</span></span> <span data-ttu-id="e5e76-121">Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.</span><span class="sxs-lookup"><span data-stu-id="e5e76-121">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="e5e76-122">Nyissa meg túl[5. lépés: replikálás engedélyezése](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-122">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-6-run-a-test-failover"></a><span data-ttu-id="e5e76-123">6. lépés: Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="e5e76-123">Step 6: Run a test failover</span></span>

<span data-ttu-id="e5e76-124">Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="e5e76-124">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="e5e76-125">Nyissa meg túl[6. lépés: feladatátvételi teszt futtatása](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="e5e76-125">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>


