---
title: "egy Azure Site Recovery segítségével VMware-replikáció tooAzure tárolót fel aaaSet |} Microsoft Docs"
description: "Tooset mentése a tárolóba van szüksége az Azure Site Recovery segítségével VMware-replikáció tooAzure hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 8a7755a6c9a3f55f241c615e425285bc4b782493
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-tooazure"></a><span data-ttu-id="7c483-103">7. lépés: Állítson be egy VMware-replikáció tooAzure tárolót</span><span class="sxs-lookup"><span data-stu-id="7c483-103">Step 7: Set up a vault for VMware replication tooAzure</span></span>


<span data-ttu-id="7c483-104">Ez a cikk ismerteti, hogyan tooset legfeljebb egy tárolót, és határozza meg, hogy a helyszíni helyről, hello segítségével tooAzure tooreplicate [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7c483-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="7c483-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7c483-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="7c483-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c483-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="7c483-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="7c483-107">Select a protection goal</span></span>

<span data-ttu-id="7c483-108">Jelölje ki a tooreplicate, és a tooreplicate, ahová.</span><span class="sxs-lookup"><span data-stu-id="7c483-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="7c483-109">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7c483-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="7c483-110">Hello erőforrás menüben, kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="7c483-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="7c483-111">A **védelmi cél**, jelölje be **tooAzure** > **Igen, amelyen a VMware vSphere Hipervizorra**.</span><span class="sxs-lookup"><span data-stu-id="7c483-111">In **Protection goal**, select **tooAzure** > **Yes, with VMware vSphere Hypervisor**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="7c483-112">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c483-112">Next steps</span></span>

<span data-ttu-id="7c483-113">Nyissa meg túl[8. lépés: állítson be forrása és célja](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="7c483-113">Go too[Step 8: Set up source and target](vmware-walkthrough-source-target.md)</span></span>
