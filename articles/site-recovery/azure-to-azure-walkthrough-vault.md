---
title: "aaaSet be egy Azure virtuális gép repliction tárolót az Azure Site Recovery régiók közötti |} Microsoft Docs"
description: "Azure Site Recovery segítségével Azure-régiók közötti Azure replikáció tooset mentése a tárolóba kell hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a><span data-ttu-id="f1340-103">4. lépés: Azure tooAzure replikációra tárolókba beállítása</span><span class="sxs-lookup"><span data-stu-id="f1340-103">Step 4: Set up a vault for Azure tooAzure replication</span></span>

<span data-ttu-id="f1340-104">Után [hálózatok tervezési](azure-to-azure-walkthrough-network.md), használja a cikk tooset mentése a tárolóba, az Azure virtuális gépek (VM) replikálása az Azure-régió, tooanother hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f1340-104">After [planning networks](azure-to-azure-walkthrough-network.md), use this article tooset up a vault, for Azure virtual machines (VMs) replicating tooanother Azure region, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

- <span data-ttu-id="f1340-105">Hello cikk befejezése után állítsa be a Recovery Services-tárolónak kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f1340-105">When you finish hello article, you should have a Recovery Services vault set up.</span></span>
- <span data-ttu-id="f1340-106">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f1340-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



>[!NOTE]
>
> <span data-ttu-id="f1340-107">Az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="f1340-107">Azure VM replication is currently in preview.</span></span>




## <a name="create-a-vault"></a><span data-ttu-id="f1340-108">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1340-108">Create a vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="f1340-109">Azt javasoljuk, hogy a hello helyre, ahol a virtuális gépek tooreplicate hozzon létre hello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="f1340-109">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="f1340-110">Például ha a célhely van hello központi VELÜNK, hozzon létre hello tárolót a **USA középső RÉGIÓJA**.</span><span class="sxs-lookup"><span data-stu-id="f1340-110">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f1340-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1340-111">Next steps</span></span>

<span data-ttu-id="f1340-112">Nyissa meg túl[5. lépés: replikálás engedélyezése](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="f1340-112">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>
