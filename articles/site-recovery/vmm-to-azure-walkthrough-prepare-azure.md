---
title: "Azure-ban az Azure Site Recovery (a System Center VMM) a Hyper-V virtuális gépek replikálása Azure-erőforrások előkészítése |} Microsoft Docs"
description: "Ismerteti az Azure-beli (VMM) szolgáltatással a Hyper-V virtuális gépek replikálása az Azure-bA az Azure Site Recovery elindítása előtt"
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
ms.openlocfilehash: 63b005f37ab5e15e8a1b4645446d65f1529f1bbd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-to-azure"></a><span data-ttu-id="dbb1c-103">5. lépés: Felkészülés az Azure-ba (VMM) szolgáltatással a Hyper-V replikáció Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="dbb1c-103">Step 5: Prepare Azure resources for Hyper-V replication (with VMM) to Azure</span></span>

<span data-ttu-id="dbb1c-104">Annak ellenőrzése után [hálózati vonatkozó követelmények](vmm-to-azure-walkthrough-network.md), kövesse az utasításokat ebben a cikkben az Azure-erőforrások készíti elő az, hogy az Azure-bA a replikálhatjaahelyszíniHyper-Vvirtuálisgépek,aSystemCenterVirtualMachineManager(VMM)felhők[ Az Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-104">After verifying [network requirements](vmm-to-azure-walkthrough-network.md), use the instructions in this article to prepare Azure resources so that you can replicate on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="dbb1c-105">A cikk elolvasása után bármely fűzhetnek alsó, vagy a műszaki kérdései a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-an-azure-account"></a><span data-ttu-id="dbb1c-106">Az Azure-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="dbb1c-106">Set up an Azure account</span></span>

- <span data-ttu-id="dbb1c-107">Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-107">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="dbb1c-108">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-108">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="dbb1c-109">Ellenőrizze a támogatott régiók a helyreállítás alatt a cikknek a földrajzi elérhetőséggel [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-109">Check the supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="dbb1c-110">További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és a [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-110">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="dbb1c-111">Győződjön meg arról, hogy az Azure-fiókjával a megfelelő [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)Azure virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-111">Make sure your Azure account has the correct [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)to create Azure VMs.</span></span> <span data-ttu-id="dbb1c-112">[További](../active-directory/role-based-access-built-in-roles.md) Azure szerepköralapú hozzáférés-vezérléssel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-112">[Learn more](../active-directory/role-based-access-built-in-roles.md) about Azure role-based access control.</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="dbb1c-113">Azure-hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="dbb1c-113">Set up an Azure network</span></span>

- <span data-ttu-id="dbb1c-114">Állítson be egy [Azure hálózati](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-114">Set up an [Azure network](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span></span> <span data-ttu-id="dbb1c-115">Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="dbb1c-116">A hálózati és a Recovery Services-tárolónak ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-116">The network should be in the same region as the Recovery Services vault</span></span>
- <span data-ttu-id="dbb1c-117">Helyreállítás az Azure portálon állítsa be a hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-117">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="dbb1c-118">Javasoljuk, hogy a kezdés előtt hozza létre a hálózatot.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="dbb1c-119">Ha most kihagyja ezt a lépést, a Site Recovery üzembe helyezése közben kell majd elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-119">If you don't, you need to do it during Site Recovery deployment.</span></span>
- <span data-ttu-id="dbb1c-120">További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="dbb1c-121">Azure-tárfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="dbb1c-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="dbb1c-122">A Site Recovery a helyszíni gépeket replikál az Azure storage.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-122">Site Recovery replicates on-premises machines to Azure storage.</span></span> <span data-ttu-id="dbb1c-123">Azure virtuális gépek jönnek létre a tárból, feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-123">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="dbb1c-124">Állítsa be a standard vagy prémium [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) az Azure-bA replikált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to hold data replicated to Azure.</span></span>
- <span data-ttu-id="dbb1c-125">[Prémium szintű storage](../storage/common/storage-premium-storage.md) általában azoknak a virtuális gépekhez, amelyek folyamatosan magas I/O teljesítménye, és a gazdagép IO-igényes munkaterhelések kis késleltetésű van szükség.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency to host IO intensive workloads.</span></span>
- <span data-ttu-id="dbb1c-126">Ha Prémium szintű fiókot kíván használni a replikált adatok tárolására, a helyszíni adatokban bekövetkező változásokat rögzítő replikálási naplók tárolására be kell állítania egy standard szintű Storage-fiókot is.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-126">If you want to use a premium account to store replicated data, you also need a standard storage account to store replication logs that capture ongoing changes to on-premises data.</span></span>
- <span data-ttu-id="dbb1c-127">Attól függően, hogy a használni kívánt erőforrás-modellje az Azure virtuális gépek a feladatátvételt egy fiók beállítása a [Resource Manager módra](../storage/common/storage-create-storage-account.md), vagy [klasszikus módban](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="dbb1c-127">Depending on the resource model you want to use for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="dbb1c-128">Azt javasoljuk, hogy állítsa be a tárfiók megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="dbb1c-129">Ha most kihagyja ezt kell tennie azt a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-129">If you don't you need to do it during Site Recovery deployment.</span></span> <span data-ttu-id="dbb1c-130">A fiókokat és a Recovery Services-tárolónak ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-130">The accounts must be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="dbb1c-131">Erőforráscsoportok egyazon előfizetésen belül, vagy az eltérő előfizetések a Site Recovery által használt tárfiókok nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="dbb1c-131">You can't move storage accounts used by Site Recovery across resource groups within the same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="dbb1c-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dbb1c-132">Next steps</span></span>

<span data-ttu-id="dbb1c-133">Ugrás a [6. lépés: készítse elő a VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="dbb1c-133">Go to [Step 6: Prepare VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>
