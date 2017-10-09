---
title: "egy Azure Site Recovery segítségével a fizikai kiszolgáló replikációs tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket be Azure Site Recovery segítségével tároló tooreplicate fizikai kiszolgálók tooAzure tooset van szüksége"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a><span data-ttu-id="31d11-103">6. lépés: Állítson be egy fizikai kiszolgáló replikációs tooAzure tárolót</span><span class="sxs-lookup"><span data-stu-id="31d11-103">Step 6: Set up a vault for physical server replication tooAzure</span></span>


<span data-ttu-id="31d11-104">Ez a cikk ismerteti, hogyan tooset mentése a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="31d11-104">This article describes how tooset up a vault.</span></span> <span data-ttu-id="31d11-105">Hozzon létre hello tárolót, és határozza meg, hogy a helyszíni hely tooAzure hello segítségével a tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="31d11-105">You create hello vault, and specify what you want tooreplicate from your on-premises location tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="31d11-106">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="31d11-106">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="31d11-107">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="31d11-107">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="31d11-108">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="31d11-108">Select a protection goal</span></span>

<span data-ttu-id="31d11-109">Jelölje ki a tooreplicate, és a tooreplicate, ahová.</span><span class="sxs-lookup"><span data-stu-id="31d11-109">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="31d11-110">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="31d11-110">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="31d11-111">Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="31d11-111">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="31d11-112">A **védelmi cél**, jelölje be **tooAzure** > **nem virtualizált vagy egyéb**.</span><span class="sxs-lookup"><span data-stu-id="31d11-112">In **Protection goal**, select **tooAzure** > **Not virtualized/Other**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="31d11-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31d11-113">Next steps</span></span>

<span data-ttu-id="31d11-114">Nyissa meg túl[7. lépés: állítson be forrása és célja](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="31d11-114">Go too[Step 7: Set up source and target](physical-walkthrough-source-target.md)</span></span>
