---
title: "egy replikációs házirendnek a VMware virtuális gép replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseit egy replikációs házirendnek tooset tooAzure tárolási VMware virtuális gépek replikálása esetén"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 12870f3f98a4dd412221b817581403cd4bf7a76d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-tooazure"></a><span data-ttu-id="b67ef-103">9. lépés: A VMware virtuális gép replikációs tooAzure replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="b67ef-103">Step 9: Set up a replication policy for VMware VM replication tooAzure</span></span>


<span data-ttu-id="b67ef-104">Ez a cikk ismerteti, hogyan tooset fel egy replikációs házirendet, ha VMware virtuális gépek tooAzure hello segítségével replikál [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b67ef-104">This article describes how tooset up a replication policy when you're replicating VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="b67ef-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="b67ef-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="b67ef-106">Házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b67ef-106">Configure a policy</span></span>

<span data-ttu-id="b67ef-107">Gyorsan áttekintheti videó elindítása előtt:</span><span class="sxs-lookup"><span data-stu-id="b67ef-107">Get a quick video overview before you start:</span></span>
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. <span data-ttu-id="b67ef-108">egy új replikációs házirendet toocreate kattintson **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.</span><span class="sxs-lookup"><span data-stu-id="b67ef-108">toocreate a new replication policy, click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="b67ef-109">A **replikációs házirend létrehozása**, megadhatja a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="b67ef-109">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="b67ef-110">A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot.</span><span class="sxs-lookup"><span data-stu-id="b67ef-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="b67ef-111">Ez az érték határozza meg, milyen gyakran adatok helyreállítási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b67ef-111">This value specifies how often data recovery points are created.</span></span> <span data-ttu-id="b67ef-112">Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.</span><span class="sxs-lookup"><span data-stu-id="b67ef-112">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="b67ef-113">A **helyreállításipont-megőrzést**, adja meg (órában) mennyi ideig áll hello adatmegőrzési időtartam az egyes helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="b67ef-113">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="b67ef-114">A replikált virtuális gépek lehet helyreállítani tooany pont ablakban.</span><span class="sxs-lookup"><span data-stu-id="b67ef-114">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="b67ef-115">Másolatot óra megőrzési gépek esetén támogatott too24 replikált toopremium tárolás és a standard szintű tárolást 72 órát.</span><span class="sxs-lookup"><span data-stu-id="b67ef-115">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="b67ef-116">A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b67ef-116">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="b67ef-117">Kattintson a **OK** toocreate hello házirend.</span><span class="sxs-lookup"><span data-stu-id="b67ef-117">Click **OK** toocreate hello policy.</span></span>

    ![Replikációs szabályzat](./media/vmware-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="b67ef-119">Ha egy új házirendet hoz létre, automatikusan rendelkezik hello konfigurációs kiszolgáló társítva.</span><span class="sxs-lookup"><span data-stu-id="b67ef-119">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="b67ef-120">Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre.</span><span class="sxs-lookup"><span data-stu-id="b67ef-120">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="b67ef-121">Például ha hello replikációs házirend nem **rep-házirend** akkor hello feladatátvételi házirendhez lesz **rep-házirend-feladat-visszavétel**.</span><span class="sxs-lookup"><span data-stu-id="b67ef-121">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="b67ef-122">Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="b67ef-122">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b67ef-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b67ef-123">Next steps</span></span>

<span data-ttu-id="b67ef-124">Nyissa meg túl[10. lépés: hello mobilitási szolgáltatás telepítése](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="b67ef-124">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>
