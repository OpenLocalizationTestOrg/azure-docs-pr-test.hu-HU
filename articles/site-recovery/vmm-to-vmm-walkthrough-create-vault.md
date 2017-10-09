---
title: "a Hyper-V replikáció tooa másodlagos hely az Azure Site Recovery tárolójából aaaCreate |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy tároló tooa Hyper-V virtuális gépek replikálása esetén másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="8338f-103">5. lépés: Hyper-V replikáció tooa másodlagos hely tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="8338f-103">Step 5: Create a vault for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="8338f-104">A helyszíni előkészítését követően [System Center Virtual Machine Manager (VMM) kiszolgáló és Hyper-V gazdagépek/fürt](vmm-to-vmm-walkthrough-vmm-hyper-v.md) a Hyper-V replikáció tooa másodlagos hely [Azure Site Recovery](site-recovery-overview.md), létrehozhat egy Recovery Services-tároló, és jelölje be hello replikációs forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="8338f-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication tooa secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select hello replication scenario.</span></span>

<span data-ttu-id="8338f-105">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="8338f-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="8338f-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="8338f-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="8338f-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="8338f-107">Choose a protection goal</span></span>

<span data-ttu-id="8338f-108">Válassza ki a kívánt tooreplicate és a tooreplicate, ahová.</span><span class="sxs-lookup"><span data-stu-id="8338f-108">Select what you want tooreplicate and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="8338f-109">Kattintson a **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="8338f-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="8338f-110">Válassza ki **toorecovery hely**, és válassza ki **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="8338f-110">Select **toorecovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="8338f-111">Válassza ki **Igen** tooindicate VMM toomanage hello Hyper-V-gazdagépek használata.</span><span class="sxs-lookup"><span data-stu-id="8338f-111">Select **Yes** tooindicate you're using VMM toomanage hello Hyper-V hosts.</span></span>
4. <span data-ttu-id="8338f-112">Válassza ki **Igen** Ha egy másodlagos VMM-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8338f-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="8338f-113">Ha egy VMM-kiszolgálón felhők közötti replikáció telepít, kattintson a **nem**.</span><span class="sxs-lookup"><span data-stu-id="8338f-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="8338f-114">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8338f-114">Then click **OK**.</span></span>

    ![Célok megválasztása](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="8338f-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8338f-116">Next steps</span></span>

<span data-ttu-id="8338f-117">Nyissa meg túl[6. lépés: hello replikációs forrás és cél beállítása](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="8338f-117">Go too[Step 6: Set up hello replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>
