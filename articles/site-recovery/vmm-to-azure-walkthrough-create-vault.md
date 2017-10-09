---
title: "egy Azure Site Recovery segítségével a Hyper-V replikáció (a System Center VMM) tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket az Azure Site Recovery segítségével a Hyper-V-replikációt (VMM) tooAzure szükséges tooset mentése a tárolóba"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: f2c90f3c8b0a48db1e57fefd9829d29cffff8d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="970af-103">7. lépés: Egy tárolót a Hyper-V replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="970af-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="970af-104">Ez a cikk ismerteti, hogyan tooset legfeljebb egy tárolót, és határozza meg, hogy a helyszíni helyről, hello segítségével tooAzure tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="970af-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="970af-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="970af-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="970af-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="970af-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="970af-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="970af-107">Select a protection goal</span></span>

<span data-ttu-id="970af-108">Jelölje ki a tooreplicate, és a tooreplicate, ahová.</span><span class="sxs-lookup"><span data-stu-id="970af-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="970af-109">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="970af-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="970af-110">Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="970af-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="970af-111">A **védelmi cél**, jelölje be **tooAzure** > **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="970af-111">In **Protection goal**, select **tooAzure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="970af-112">Válassza ki **Igen** tooconfirm Ön nhasználják VMM.</span><span class="sxs-lookup"><span data-stu-id="970af-112">Select **Yes** tooconfirm you're nusing VMM.</span></span> 

     ![Célok megválasztása](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="970af-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="970af-114">Next steps</span></span>

<span data-ttu-id="970af-115">Nyissa meg túl[8. lépés: állítson be forrása és célja](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="970af-115">Go too[Step 8: Set up source and target](vmm-to-azure-walkthrough-source-target.md)</span></span>
