---
title: "aaaSet fel egy replikációs házirendet, a Hyper-V virtuális gép (a System Center VMM nélkül) replikációs tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Összefoglalja a hello lépéseit egy replikációs házirendnek tooset tooAzure tárolási Hyper-V virtuális gépek replikálása esetén"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 4bd7161f4a0f015da0ecf595fbc6861ede5d68b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a><span data-ttu-id="651fb-103">9. lépés: A Hyper-V virtuális gép replikációs tooAzure replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="651fb-103">Step 9: Set up a replication policy for Hyper-V VM replication tooAzure</span></span>

<span data-ttu-id="651fb-104">Ez a cikk ismerteti, hogyan tooset fel egy replikációs házirendet, ha a Hyper-V virtuális gépek tooAzure (a System Center VMM nélkül) használatával hello replikál [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="651fb-104">This article describes how tooset up a replication policy when you're replicating Hyper-V VMs tooAzure (without System Center VMM) using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="651fb-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="651fb-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="about-snapshots"></a><span data-ttu-id="651fb-106">A pillanatképek kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="651fb-106">About snapshots</span></span>

<span data-ttu-id="651fb-107">Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="651fb-107">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span>
    - <span data-ttu-id="651fb-108">Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában.</span><span class="sxs-lookup"><span data-stu-id="651fb-108">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span>
    - <span data-ttu-id="651fb-109">Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="651fb-109">If you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="651fb-110">Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.</span><span class="sxs-lookup"><span data-stu-id="651fb-110">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>

## <a name="set-up-a-replication-policy"></a><span data-ttu-id="651fb-111">A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="651fb-111">Set up a replication policy</span></span>

1. <span data-ttu-id="651fb-112">Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.</span><span class="sxs-lookup"><span data-stu-id="651fb-112">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Network (Hálózat)](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="651fb-114">A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.</span><span class="sxs-lookup"><span data-stu-id="651fb-114">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="651fb-115">A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.</span><span class="sxs-lookup"><span data-stu-id="651fb-115">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    > <span data-ttu-id="651fb-116">Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="651fb-116">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="651fb-117">hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg.</span><span class="sxs-lookup"><span data-stu-id="651fb-117">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> <span data-ttu-id="651fb-118">[További információk](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span><span class="sxs-lookup"><span data-stu-id="651fb-118">[Learn more](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span></span>

4. <span data-ttu-id="651fb-119">A **helyreállításipont-megőrzést**, mennyi ideig óra az egyes helyreállítási pontok áll hello adatmegőrzési időtartam.</span><span class="sxs-lookup"><span data-stu-id="651fb-119">In **Recovery point retention**, specify in hours how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="651fb-120">Virtuális gépek lehet helyreállítani egy időszakban tooany pont.</span><span class="sxs-lookup"><span data-stu-id="651fb-120">VMs can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="651fb-121">A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="651fb-121">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots are created.</span></span>
6. <span data-ttu-id="651fb-122">A **kezdeti replikáció kezdési ideje**, adja meg, amikor toostart hello kezdeti replikálása.</span><span class="sxs-lookup"><span data-stu-id="651fb-122">In **Initial replication start time**, specify when toostart hello initial replication.</span></span> <span data-ttu-id="651fb-123">hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül.</span><span class="sxs-lookup"><span data-stu-id="651fb-123">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span> <span data-ttu-id="651fb-124">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="651fb-124">Then click **OK**.</span></span>

    ![Replikációs szabályzat](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

<span data-ttu-id="651fb-126">Ha egy új házirendet hoz létre, azt automatikusan hello Hyper-V hely társítja.</span><span class="sxs-lookup"><span data-stu-id="651fb-126">When you create a new policy, it's automatically associated with hello Hyper-V site.</span></span> <span data-ttu-id="651fb-127">A Hyper-V hely (és a benne lévő virtuális gépek hello) társíthatja a több replikációs házirend **replikációs** > házirend neve > **társítása Hyper-V hely**.</span><span class="sxs-lookup"><span data-stu-id="651fb-127">You can associate a Hyper-V site (and hello VMs in it) with multiple replication policies in **Replication** > policy-name > **Associate Hyper-V Site**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="651fb-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="651fb-128">Next steps</span></span>

<span data-ttu-id="651fb-129">Nyissa meg túl[10. lépés: replikálás engedélyezése](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="651fb-129">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>
