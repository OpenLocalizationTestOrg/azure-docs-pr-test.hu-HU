---
title: "Állítson be egy tárolót a Hyper-V replikáció (a System Center VMM nélkül) az Azure Site Recovery segítségével Azure-bA |} Microsoft Docs"
description: "Összefoglalja a lépéseket, akkor be kell állítania egy tárolót a Hyper-V replikát az Azure Site Recovery segítségével Azure-bA"
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
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="a60e3-103">7. lépés: Egy tárolót a Hyper-V replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="a60e3-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="a60e3-104">Ez a cikk ismerteti, hogyan állítson be egy tárolót, és adja meg, mi az Azure használatát a helyszíni helyről replikálja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a60e3-104">This article describes how to set up a vault, and specify what you want to replicate from your on-premises location, to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="a60e3-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="a60e3-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="a60e3-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a60e3-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="a60e3-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="a60e3-107">Select a protection goal</span></span>

<span data-ttu-id="a60e3-108">Válassza ki, hogy mit szeretne replikálni, és hová.</span><span class="sxs-lookup"><span data-stu-id="a60e3-108">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="a60e3-109">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a60e3-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="a60e3-110">Az erőforrás menüjében kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="a60e3-110">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="a60e3-111">A **védelmi cél**, jelölje be **az Azure-bA** > **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="a60e3-111">In **Protection goal**, select **To Azure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="a60e3-112">Válassza ki **nem** annak ellenőrzéséhez, hogy a VMM nem használja.</span><span class="sxs-lookup"><span data-stu-id="a60e3-112">Select **No** to confirm you're not using VMM.</span></span> 

    ![Célok megválasztása](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="a60e3-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a60e3-114">Next steps</span></span>

<span data-ttu-id="a60e3-115">Ugrás a [8. lépés: állítson be forrása és célja](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="a60e3-115">Go to [Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
