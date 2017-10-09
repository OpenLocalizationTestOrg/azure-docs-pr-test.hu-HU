---
title: "Azure-erőforrások tooreplicate aaaPrepare helyszíni fizikai kiszolgálók tooAzure Azure Site Recovery segítségével |} Microsoft Docs"
description: "Ismerteti az Azure-beli megkezdése előtt a helyszíni kiszolgálók tooAzure, replikálása hello Azure Site Recovery szolgáltatással"
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
ms.openlocfilehash: b1d008dac278bc7797188a3c9c15f2a3b5fe12d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-tooazure"></a><span data-ttu-id="41dc6-103">5. lépés: Azure-erőforrások előkészítése a fizikai kiszolgáló replikációs tooAzure</span><span class="sxs-lookup"><span data-stu-id="41dc6-103">Step 5: Prepare Azure resources for physical server replication tooAzure</span></span>


<span data-ttu-id="41dc6-104">Ez a cikk tooprepare Azure hello utasításokat használható erőforrásokat, hogy a helyszíni kiszolgálók tooAzure hello segítségével replikálhatja [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="41dc6-104">Use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises servers tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="41dc6-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="41dc6-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="41dc6-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="41dc6-106">Before you start</span></span>

<span data-ttu-id="41dc6-107">Győződjön meg arról, hogy elolvasta a hello [Előfeltételek](physical-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="41dc6-107">Make sure you've read hello [prerequisites](physical-walkthrough-prerequisites.md).</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="41dc6-108">Az Azure-fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="41dc6-108">Set up an Azure account</span></span>

- <span data-ttu-id="41dc6-109">Első egy [Microsoft Azure-fiók](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="41dc6-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="41dc6-110">Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.</span><span class="sxs-lookup"><span data-stu-id="41dc6-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="41dc6-111">A Site Recovery ellenőrzése hello támogatott régiók **cikknek a földrajzi elérhetőséggel** a [Azure Site Recovery – díjszabás](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="41dc6-111">Check hello supported regions for Site Recovery, under **Geographic Availability** in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="41dc6-112">További tudnivalók [Site Recovery díjszabásáról](site-recovery-faq.md#pricing), és hello [díjszabása](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="41dc6-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="41dc6-113">Azure-hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="41dc6-113">Set up an Azure network</span></span>

- <span data-ttu-id="41dc6-114">Állítsa be az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="41dc6-114">Set up an Azure network.</span></span> <span data-ttu-id="41dc6-115">Azure virtuális gépek kerülnek ehhez a hálózathoz, ha feladatátvételt követően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="41dc6-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="41dc6-116">Site Recovery hello Azure-portálon állíthatja be hálózatokat fogja tudni használni [erőforrás-kezelő](../resource-manager-deployment-model.md), vagy a klasszikus mód.</span><span class="sxs-lookup"><span data-stu-id="41dc6-116">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="41dc6-117">hello hello hálózat legyen ugyanabban a régióban, hello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="41dc6-117">hello network should be in hello same region as hello Recovery Services vault.</span></span>
- <span data-ttu-id="41dc6-118">További tudnivalók [virtuális hálózati árképzési](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="41dc6-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="41dc6-119">További információ [Azure Virtuálisgép-kapcsolat](physical-walkthrough-network.md) feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="41dc6-119">Learn more about [Azure VM connectivity](physical-walkthrough-network.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="41dc6-120">Azure-tárfiók beállítása</span><span class="sxs-lookup"><span data-stu-id="41dc6-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="41dc6-121">A Site Recovery a helyszíni kiszolgálók tooAzure tárolási replikálja.</span><span class="sxs-lookup"><span data-stu-id="41dc6-121">Site Recovery replicates on-premises servers tooAzure storage.</span></span> <span data-ttu-id="41dc6-122">Azure virtuális gépek hello tárolási készített feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="41dc6-122">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="41dc6-123">Állítson be egy [Azure storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account) a replikált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="41dc6-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="41dc6-124">Site Recovery hello Azure-portálon használható storage-fiókok beállítása az erőforrás-kezelőben, vagy a klasszikus módban.</span><span class="sxs-lookup"><span data-stu-id="41dc6-124">Site Recovery in hello Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="41dc6-125">lehet, hogy a tárfiók hello standard vagy [prémium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="41dc6-125">hello storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="41dc6-126">Ha a prémium szintű fiók beállítása is szüksége lesz egy további standard fiók naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="41dc6-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="41dc6-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41dc6-127">Next steps</span></span>

<span data-ttu-id="41dc6-128">Nyissa meg túl[6. lépés: állítson be egy tárolóban.](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="41dc6-128">Go too[Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>
