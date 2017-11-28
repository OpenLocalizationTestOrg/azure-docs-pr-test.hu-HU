---
title: "A helyszíni fizikai kiszolgálók replikálása az Azure-bA az Azure Site Recovery segítségével az Azure erőforrás előkészítése |} Microsoft Docs"
description: "Ismerteti az Azure-beli a helyszíni kiszolgálók replikálása az Azure-bA az Azure Site Recovery szolgáltatás elindítása előtt"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a><span data-ttu-id="cf4a8-103">5. lépés: Azure-erőforrások előkészítése a fizikai kiszolgáló replikációs az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="cf4a8-103">Step 5: Prepare Azure resources for physical server replication to Azure</span></span>


<span data-ttu-id="cf4a8-104">Ez a cikk az utasításokat követve előkészítése az Azure-erőforrások, úgy, hogy a helyszíni kiszolgálók Azure használatával replikálhatja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-104">Use the instructions in this article to prepare Azure resources so that you can replicate on-premises servers to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="cf4a8-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="cf4a8-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="cf4a8-106">Before you start</span></span>

<span data-ttu-id="cf4a8-107">Győződjön meg arról, hogy elolvasta a [Előfeltételek](physical-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-107">Make sure you've read the [prerequisites](physical-walkthrough-prerequisites.md).</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="cf4a8-108">Az Azure-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="cf4a8-108">Set up an Azure account</span></span>

- <span data-ttu-id="cf4a8-109">Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="cf4a8-110">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="cf4a8-111">A támogatott régiók a Site Recovery ellenőrizze **cikknek a földrajzi elérhetőséggel** a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-111">Check the supported regions for Site Recovery, under **Geographic Availability** in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="cf4a8-112">További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és a [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="cf4a8-113">Azure-hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="cf4a8-113">Set up an Azure network</span></span>

- <span data-ttu-id="cf4a8-114">Állítsa be az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-114">Set up an Azure network.</span></span> <span data-ttu-id="cf4a8-115">Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="cf4a8-116">Helyreállítás az Azure portálon állítsa be a hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-116">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="cf4a8-117">A hálózatnak és a Recovery Services-tárolónak ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-117">The network should be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="cf4a8-118">További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="cf4a8-119">További információ [Azure Virtuálisgép-kapcsolat](physical-walkthrough-network.md) feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-119">Learn more about [Azure VM connectivity](physical-walkthrough-network.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="cf4a8-120">Azure-tárfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="cf4a8-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="cf4a8-121">A Site Recovery a helyszíni kiszolgálók az Azure storage replikálja.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-121">Site Recovery replicates on-premises servers to Azure storage.</span></span> <span data-ttu-id="cf4a8-122">Azure virtuális gépek jönnek létre a tárból, feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-122">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="cf4a8-123">Állítson be egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="cf4a8-124">Helyreállítás az Azure portálon használható storage-fiókok beállítása az erőforrás-kezelőben, vagy a klasszikus módban.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-124">Site Recovery in the Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="cf4a8-125">A tárfiók lehet standard vagy [prémium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="cf4a8-125">The storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="cf4a8-126">Ha a prémium szintű fiók beállítása is szüksége lesz egy további standard fiók naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="cf4a8-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="cf4a8-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf4a8-127">Next steps</span></span>

<span data-ttu-id="cf4a8-128">Ugrás a [6. lépés: állítson be egy tárolóban.](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="cf4a8-128">Go to [Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>
