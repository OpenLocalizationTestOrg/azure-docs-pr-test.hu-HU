---
title: "A Hyper-V replikáció egy másodlagos helyre tároló létrehozása az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Hozzon létre egy tárolót, ha a Hyper-V virtuális gépek replikálása az Azure Site Recovery System Center VMM másodlagos hely ismerteti."
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
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="b52be-103">5. lépés:, Hozzon létre egy tárolót a Hyper-V replikáció egy másodlagos helyre</span><span class="sxs-lookup"><span data-stu-id="b52be-103">Step 5: Create a vault for Hyper-V replication to a secondary site</span></span>

<span data-ttu-id="b52be-104">A helyszíni előkészítését követően [System Center Virtual Machine Manager (VMM) kiszolgáló és a Hyper-V gazdagép vagy fürt](vmm-to-vmm-walkthrough-vmm-hyper-v.md) a Hyper-V replikáció egy másodlagos hely használatával [Azure Site Recovery](site-recovery-overview.md), Recovery Services-tároló létrehozása, és válassza ki a replikációs környezet.</span><span class="sxs-lookup"><span data-stu-id="b52be-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication to a secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select the replication scenario.</span></span>

<span data-ttu-id="b52be-105">A cikk elolvasása után felmerülő megjegyzéseit alul vagy az [Azure Recovery Services fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti közzé.</span><span class="sxs-lookup"><span data-stu-id="b52be-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="b52be-106">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="b52be-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="b52be-107">Válassza ki a védelmi cél</span><span class="sxs-lookup"><span data-stu-id="b52be-107">Choose a protection goal</span></span>

<span data-ttu-id="b52be-108">Válassza ki, hogy mit szeretne replikálni, és hova.</span><span class="sxs-lookup"><span data-stu-id="b52be-108">Select what you want to replicate and where you want to replicate to.</span></span>

1. <span data-ttu-id="b52be-109">Kattintson a **Site Recovery** > **1. lépés: az infrastruktúra előkészítése** > **védelmi cél**.</span><span class="sxs-lookup"><span data-stu-id="b52be-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="b52be-110">Válassza ki **helyreállítási hely**, és válassza ki **Igen, a Hyper-V-vel**.</span><span class="sxs-lookup"><span data-stu-id="b52be-110">Select **To recovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="b52be-111">Válassza ki **Igen** annak jelzésére, hogy a VMM segítségével a Hyper-V-gazdagépek kezelése.</span><span class="sxs-lookup"><span data-stu-id="b52be-111">Select **Yes** to indicate you're using VMM to manage the Hyper-V hosts.</span></span>
4. <span data-ttu-id="b52be-112">Válassza ki **Igen** Ha egy másodlagos VMM-kiszolgálóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b52be-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="b52be-113">Ha egy VMM-kiszolgálón felhők közötti replikáció telepít, kattintson a **nem**.</span><span class="sxs-lookup"><span data-stu-id="b52be-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="b52be-114">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b52be-114">Then click **OK**.</span></span>

    ![Célok megválasztása](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="b52be-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b52be-116">Next steps</span></span>

<span data-ttu-id="b52be-117">Ugrás a [6. lépés: a replikáció forrás- és a cél beállítása](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="b52be-117">Go to [Step 6: Set up the replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>