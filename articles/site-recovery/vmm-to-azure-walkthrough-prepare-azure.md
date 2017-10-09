---
title: "aaaPrepare Azure-erőforrások tooreplicate Hyper-V virtuális gépek (a System Center VMM-mel) tooAzure Azure Site Recovery segítségével |} Microsoft Docs"
description: "Ismerteti az Azure-beli megkezdése előtt (VMM) szolgáltatással a Hyper-V virtuális gépek tooAzure replikálása Azure Site Recovery segítségével"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 86bfbab7722fe5bd5b93b92e398d1d441505d3b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="acff0-103">5. lépés: Hyper-V-replikációt (VMM) tooAzure Azure-erőforrások előkészítése</span><span class="sxs-lookup"><span data-stu-id="acff0-103">Step 5: Prepare Azure resources for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="acff0-104">Annak ellenőrzése után [hálózati vonatkozó követelmények](vmm-to-azure-walkthrough-network.md), ez a cikk tooprepare Azure hello utasítások használata erőforrások, hogy a helyszíni Hyper-V virtuális gépek, a System Center Virtual Machine Manager (VMM) felhők tooAzure képes replikálni, használatával Hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="acff0-104">After verifying [network requirements](vmm-to-azure-walkthrough-network.md), use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="acff0-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="acff0-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-an-azure-account"></a><span data-ttu-id="acff0-106">Az Azure-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="acff0-106">Set up an Azure account</span></span>

- <span data-ttu-id="acff0-107">Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="acff0-107">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="acff0-108">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="acff0-108">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="acff0-109">Hello támogatott régiók ellenőrizze a hely helyreállítását követően a cikknek a földrajzi elérhetőséggel a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="acff0-109">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="acff0-110">További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és hello [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="acff0-110">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="acff0-111">Ellenőrizze, hogy az Azure-fiókjával rendelkezik a megfelelő hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="acff0-111">Make sure your Azure account has hello correct [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate Azure VMs.</span></span> <span data-ttu-id="acff0-112">[További](../active-directory/role-based-access-built-in-roles.md) Azure szerepköralapú hozzáférés-vezérléssel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="acff0-112">[Learn more](../active-directory/role-based-access-built-in-roles.md) about Azure role-based access control.</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="acff0-113">Azure-hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="acff0-113">Set up an Azure network</span></span>

- <span data-ttu-id="acff0-114">Állítson be egy [Azure hálózati](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="acff0-114">Set up an [Azure network](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span></span> <span data-ttu-id="acff0-115">Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="acff0-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="acff0-116">hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="acff0-116">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="acff0-117">Site Recovery hello Azure-portálon állíthatja be hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.</span><span class="sxs-lookup"><span data-stu-id="acff0-117">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="acff0-118">Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot.</span><span class="sxs-lookup"><span data-stu-id="acff0-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="acff0-119">Ha ezt elmulasztja, toodo kell azt a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="acff0-119">If you don't, you need toodo it during Site Recovery deployment.</span></span>
- <span data-ttu-id="acff0-120">További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="acff0-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="acff0-121">Azure-tárfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="acff0-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="acff0-122">A Site Recovery a helyszíni gépeket tooAzure tárolási replikálja.</span><span class="sxs-lookup"><span data-stu-id="acff0-122">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="acff0-123">Azure virtuális gépek hello tárolási készített feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="acff0-123">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="acff0-124">Állítsa be a standard vagy prémium [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold adatait tooAzure replikálja.</span><span class="sxs-lookup"><span data-stu-id="acff0-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replicated tooAzure.</span></span>
- <span data-ttu-id="acff0-125">[Prémium szintű storage](../storage/common/storage-premium-storage.md) jellemzően egy folyamatosan magas I/O teljesítménye és kis késleltetésű toohost IO-igényes munkaterhelések igénylő virtuális gépektől.</span><span class="sxs-lookup"><span data-stu-id="acff0-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency toohost IO intensive workloads.</span></span>
- <span data-ttu-id="acff0-126">Ha azt szeretné, hogy toouse egy prémium szintű fiók toostore replikált adatokból, is szükség van egy standard tárolási fiók toostore replikálási naplókhoz, hogy a folyamatban lévő rögzítési tooon helyszíni adatokhoz módosítja.</span><span class="sxs-lookup"><span data-stu-id="acff0-126">If you want toouse a premium account toostore replicated data, you also need a standard storage account toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
- <span data-ttu-id="acff0-127">Attól függően, hogy az erőforrás-modellje hello meg toouse az Azure virtuális gépek a feladatátvételt, a fiók beállítása [Resource Manager módra](../storage/common/storage-create-storage-account.md), vagy [klasszikus módban](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="acff0-127">Depending on hello resource model you want toouse for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="acff0-128">Azt javasoljuk, hogy állítsa be a tárfiók megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="acff0-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="acff0-129">Ha most kihagyja ezt toodo kell azt a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="acff0-129">If you don't you need toodo it during Site Recovery deployment.</span></span> <span data-ttu-id="acff0-130">hello fiókoknak kell lenniük a hello hello és ugyanabban a régióban Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="acff0-130">hello accounts must be in hello same region as hello Recovery Services vault.</span></span>
- <span data-ttu-id="acff0-131">Nem lehet áthelyezni a storage-fiókok keresztül által használt Site Recovery erőforráscsoportok hello belül azonos előfizetés, vagy különböző előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="acff0-131">You can't move storage accounts used by Site Recovery across resource groups within hello same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="acff0-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="acff0-132">Next steps</span></span>

<span data-ttu-id="acff0-133">Nyissa meg túl[6. lépés: a VMM előkészítése](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="acff0-133">Go too[Step 6: Prepare VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>
