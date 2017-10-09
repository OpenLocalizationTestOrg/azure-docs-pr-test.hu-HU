---
title: "egy Azure Site Recovery segítségével a Hyper-V replikáció (a System Center VMM nélkül) tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Mentése a tárolóba tooset van szüksége az Azure Site Recovery segítségével a Hyper-V replikáció tooAzure hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: e3ef8758faab36d19d0968d98a23105bed7830f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="950d3-103">7. lépés: Egy tárolót a Hyper-V replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="950d3-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="950d3-104">Ez a cikk ismerteti, hogyan tooset legfeljebb egy tárolót, és határozza meg, hogy a helyszíni helyről, hello segítségével tooAzure tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="950d3-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="950d3-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="950d3-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="950d3-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="950d3-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="950d3-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="950d3-107">Select a protection goal</span></span>

<span data-ttu-id="950d3-108">Jelölje ki a tooreplicate, és a tooreplicate, ahová.</span><span class="sxs-lookup"><span data-stu-id="950d3-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="950d3-109">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="950d3-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="950d3-110">Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="950d3-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="950d3-111">A **védelmi cél**, jelölje be **tooAzure** > **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="950d3-111">In **Protection goal**, select **tooAzure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="950d3-112">Válassza ki **nem** tooconfirm a VMM nem használja.</span><span class="sxs-lookup"><span data-stu-id="950d3-112">Select **No** tooconfirm you're not using VMM.</span></span> 

    ![Célok megválasztása](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="950d3-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="950d3-114">Next steps</span></span>

<span data-ttu-id="950d3-115">Nyissa meg túl[8. lépés: állítson be forrása és célja](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="950d3-115">Go too[Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
