---
title: "egy replikációs házirendnek a fizikai kiszolgáló replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket végre kell tooset fel egy replikációs házirendet, ha replikálása a helyszíni fizikai kiszolgálók tooAzure tárolási hello Azure Site Recovery szolgáltatással"
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
ms.openlocfilehash: d6bdeeffc24ffc8eaba24311f7c76452edb65648
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-tooazure"></a><span data-ttu-id="1c798-103">8. lépés: A fizikai kiszolgáló replikációs tooAzure replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="1c798-103">Step 8: Set up a replication policy for physical server replication tooAzure</span></span>


<span data-ttu-id="1c798-104">Ez a cikk ismerteti, hogyan tooset fel egy replikációs házirendet, ha hello használata windowsos/Linuxos fizikai kiszolgálók tooAzure replikál [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1c798-104">This article describes how tooset up a replication policy when you're replicating Windows/Linux physical servers tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="1c798-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1c798-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="1c798-106">Házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1c798-106">Configure a policy</span></span>

1. <span data-ttu-id="1c798-107">Kattintson a **Site Recovery-infrastruktúra** > **replikációs házirendek** > **+ replikációs házirend**.</span><span class="sxs-lookup"><span data-stu-id="1c798-107">Click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="1c798-108">A **replikációs házirend létrehozása**, megadhatja a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="1c798-108">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="1c798-109">A **helyreállítási Időkorlát küszöbértéke**, adja meg a hello RPO korlátot.</span><span class="sxs-lookup"><span data-stu-id="1c798-109">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="1c798-110">Ez az érték azt jelzi, hogy milyen gyakran adatok helyreállítási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1c798-110">This value indicates how often data recovery points are created.</span></span> <span data-ttu-id="1c798-111">Riasztás akkor jön létre, ha a folyamatos replikálásra túllépi ezt a korlátozást.</span><span class="sxs-lookup"><span data-stu-id="1c798-111">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="1c798-112">A **helyreállításipont-megőrzést**, adja meg (órában) mennyi ideig áll hello adatmegőrzési időtartam az egyes helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="1c798-112">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="1c798-113">A replikált virtuális gépek lehet helyreállítani tooany pont ablakban.</span><span class="sxs-lookup"><span data-stu-id="1c798-113">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="1c798-114">Másolatot óra megőrzési gépek esetén támogatott too24 replikált toopremium tárolás és a standard szintű tárolást 72 órát.</span><span class="sxs-lookup"><span data-stu-id="1c798-114">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="1c798-115">A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, milyen gyakran (percben) alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1c798-115">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="1c798-116">Kattintson a **OK** toocreate hello házirend.</span><span class="sxs-lookup"><span data-stu-id="1c798-116">Click **OK** toocreate hello policy.</span></span>

    ![Replikációs szabályzat](./media/physical-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="1c798-118">Ha egy új házirendet hoz létre, automatikusan rendelkezik hello konfigurációs kiszolgáló társítva.</span><span class="sxs-lookup"><span data-stu-id="1c798-118">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="1c798-119">Alapértelmezés szerint a megfelelő házirend automatikusan létrejön a feladat-visszavételre.</span><span class="sxs-lookup"><span data-stu-id="1c798-119">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="1c798-120">Például ha hello replikációs házirend nem **rep-házirend** akkor hello feladatátvételi házirendhez lesz **rep-házirend-feladat-visszavétel**.</span><span class="sxs-lookup"><span data-stu-id="1c798-120">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="1c798-121">Ez a házirend nem használ, amíg nem indítja el a feladat-visszavétel az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="1c798-121">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c798-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c798-122">Next steps</span></span>

<span data-ttu-id="1c798-123">Nyissa meg túl[9. lépés: hello mobilitási szolgáltatás telepítése](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="1c798-123">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>
