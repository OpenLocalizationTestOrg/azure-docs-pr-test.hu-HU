---
title: "Állítson be egy Azure virtuális gép repliction tárolót az Azure Site Recovery régiók közötti |} Microsoft Docs"
description: "Összefoglalja a lépéseket, akkor be kell állítania egy Azure Site Recovery segítségével Azure-régiók közötti Azure replikációs tárolót"
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
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a><span data-ttu-id="535c0-103">4. lépés: Állítson be egy Azure az Azure-bA replikációs tárolót</span><span class="sxs-lookup"><span data-stu-id="535c0-103">Step 4: Set up a vault for Azure to Azure replication</span></span>

<span data-ttu-id="535c0-104">Után [hálózatok tervezési](azure-to-azure-walkthrough-network.md), ez a cikk az használatával állítson be egy tárolót az Azure virtuális gépek (VM) esetében, amelyek egy másik Azure-régió, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="535c0-104">After [planning networks](azure-to-azure-walkthrough-network.md), use this article to set up a vault, for Azure virtual machines (VMs) replicating to another Azure region, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

- <span data-ttu-id="535c0-105">A cikk befejezése után állítsa be a Recovery Services-tárolónak kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="535c0-105">When you finish the article, you should have a Recovery Services vault set up.</span></span>
- <span data-ttu-id="535c0-106">Megjegyzéseket a cikk alján tehet, kérdéseket pedig az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.</span><span class="sxs-lookup"><span data-stu-id="535c0-106">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



>[!NOTE]
>
> <span data-ttu-id="535c0-107">Az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="535c0-107">Azure VM replication is currently in preview.</span></span>




## <a name="create-a-vault"></a><span data-ttu-id="535c0-108">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="535c0-108">Create a vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="535c0-109">Azt javasoljuk, hogy a helyen, ahol azt szeretné, hogy a virtuális gépek replikálásához hozzon létre a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="535c0-109">We recommend that you create the Recovery Services vault in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="535c0-110">Például, ha a cél elérési útja a központi USA, hozzon létre a tárolót, a **USA középső RÉGIÓJA**.</span><span class="sxs-lookup"><span data-stu-id="535c0-110">For example, if your target location is the central US, create the vault in **Central US**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="535c0-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="535c0-111">Next steps</span></span>

<span data-ttu-id="535c0-112">Ugrás a [5. lépés: replikálás engedélyezése](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="535c0-112">Go to [Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>
