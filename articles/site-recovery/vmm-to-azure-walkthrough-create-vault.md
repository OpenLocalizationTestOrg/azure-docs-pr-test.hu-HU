---
title: "Állítson be egy tárolót a Hyper-V replikáció (a System Center VMM) az Azure Site Recovery segítségével Azure-bA |} Microsoft Docs"
description: "Összefoglalja a lépéseket, akkor be kell állítania egy tárolót az Azure Site Recovery segítségével Azure-ba (VMM) szolgáltatással a Hyper-V replikáció"
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
ms.openlocfilehash: af453ec27ba15ad8c59cf9f544584ad18dc0f74a
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="3ed9b-103">7. lépés: Egy tárolót a Hyper-V replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="3ed9b-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="3ed9b-104">Ez a cikk ismerteti, hogyan állítson be egy tárolót, és adja meg, mi az Azure használatát a helyszíni helyről replikálja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-104">This article describes how to set up a vault, and specify what you want to replicate from your on-premises location, to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="3ed9b-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3ed9b-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="3ed9b-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ed9b-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="3ed9b-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="3ed9b-107">Select a protection goal</span></span>

<span data-ttu-id="3ed9b-108">Válassza ki, hogy mit szeretne replikálni, és hová.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-108">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="3ed9b-109">Kattintson a **Recovery Services-tárolók** > tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="3ed9b-110">Az erőforrás menüjében kattintson **Site Recovery** > **infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-110">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="3ed9b-111">A **védelmi cél**, jelölje be **az Azure-bA** > **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-111">In **Protection goal**, select **To Azure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="3ed9b-112">Válassza ki **Igen** győződjön meg arról, hogy a VMM nhasználják van.</span><span class="sxs-lookup"><span data-stu-id="3ed9b-112">Select **Yes** to confirm you're nusing VMM.</span></span> 

     ![Célok megválasztása](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="3ed9b-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ed9b-114">Next steps</span></span>

<span data-ttu-id="3ed9b-115">Ugrás a [8. lépés: állítson be forrása és célja](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="3ed9b-115">Go to [Step 8: Set up source and target](vmm-to-azure-walkthrough-source-target.md)</span></span>
