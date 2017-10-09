---
title: "aaaPrepare Azure-erőforrások tooreplicate (a System Center VMM nélkül) a Hyper-V virtuális gépek tooAzure Azure Site Recovery segítségével |} Microsoft Docs"
description: "Ismerteti az Azure-beli replikálása az Azure Site Recovery segítségével a Hyper-V virtuális gépek (a VMM nélkül) tooAzure elindítása előtt"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f659e300c39253b0eaf7218bee9d39b11682edb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-tooazure"></a><span data-ttu-id="5ea3c-103">5. lépés: Hyper-V replikáció tooAzure Azure-erőforrások előkészítése</span><span class="sxs-lookup"><span data-stu-id="5ea3c-103">Step 5: Prepare Azure resources for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="5ea3c-104">Hello utasítások alapján ez a cikk tooprepare Azure az erőforrásokat, hogy replikálhatja a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) használatával hello tooAzure [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-104">Use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises Hyper-V VMs (without System Center VMM) tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="5ea3c-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="5ea3c-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5ea3c-106">Before you start</span></span>

<span data-ttu-id="5ea3c-107">Győződjön meg arról, hogy elolvasta a hello [Előfeltételek](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="5ea3c-107">Make sure you've read hello [prerequisites](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="5ea3c-108">Az Azure-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="5ea3c-108">Set up an Azure account</span></span>

- <span data-ttu-id="5ea3c-109">Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="5ea3c-110">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="5ea3c-111">Hello támogatott régiók ellenőrizze a hely helyreállítását követően a cikknek a földrajzi elérhetőséggel a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-111">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="5ea3c-112">További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és hello [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="5ea3c-113">Azure-hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="5ea3c-113">Set up an Azure network</span></span>

- <span data-ttu-id="5ea3c-114">Állítsa be az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-114">Set up an Azure network.</span></span> <span data-ttu-id="5ea3c-115">Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="5ea3c-116">hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="5ea3c-116">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="5ea3c-117">Site Recovery hello Azure-portálon állíthatja be hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-117">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="5ea3c-118">Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="5ea3c-119">Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-119">If you don't, you need toodo it during Site Recovery deployment.</span></span>
- <span data-ttu-id="5ea3c-120">További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="5ea3c-121">Azure-tárfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="5ea3c-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="5ea3c-122">A Site Recovery a helyszíni gépeket tooAzure tárolási replikálja.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-122">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="5ea3c-123">Azure virtuális gépek hello tárolási készített feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-123">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="5ea3c-124">Állítsa be a standard vagy prémium [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold adatait tooAzure replikálja.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replicated tooAzure.</span></span>
- <span data-ttu-id="5ea3c-125">[Prémium szintű storage](../storage/common/storage-premium-storage.md) jellemzően egy folyamatosan magas I/O teljesítménye és kis késleltetésű toohost IO-igényes munkaterhelések igénylő virtuális gépektől.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency toohost IO intensive workloads.</span></span>
- <span data-ttu-id="5ea3c-126">Ha azt szeretné, hogy toouse egy prémium szintű fiók toostore replikált adatokból, is szükség van egy standard tárolási fiók toostore replikálási naplókhoz, hogy a folyamatban lévő rögzítési tooon helyszíni adatokhoz módosítja.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-126">If you want toouse a premium account toostore replicated data, you also need a standard storage account toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
- <span data-ttu-id="5ea3c-127">Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, a fiók beállítása [Resource Manager módra](../storage/common/storage-create-storage-account.md), vagy [klasszikus módban](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="5ea3c-127">Depending on hello resource model you want toouse for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="5ea3c-128">Azt javasoljuk, hogy állítsa be a tárfiók megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="5ea3c-129">Ha most kihagyja ezt toodo kell azt a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-129">If you don't you need toodo it during Site Recovery deployment.</span></span> <span data-ttu-id="5ea3c-130">hello fiókoknak kell lenniük a hello hello és ugyanabban a régióban Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-130">hello accounts must be in hello same region as hello Recovery Services vault.</span></span>
- <span data-ttu-id="5ea3c-131">Nem lehet áthelyezni a storage-fiókok keresztül által használt Site Recovery erőforráscsoportok hello belül azonos előfizetés, vagy különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="5ea3c-131">You can't move storage accounts used by Site Recovery across resource groups within hello same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5ea3c-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ea3c-132">Next steps</span></span>

<span data-ttu-id="5ea3c-133">Nyissa meg túl[6. lépés: Felkészülés a Hyper-V-erőforrások](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="5ea3c-133">Go too[Step 6: Prepare Hyper-V resources](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>
