---
title: "Az Azure-bA az Azure Site Recovery (a System Center VMM nélkül) a Hyper-V Virtuálisgép-replikációt a replikációs házirend beállítása |} Microsoft Docs"
description: "Összefoglalja a lépéseket kell beállítani a replikációs házirendet, ha a Hyper-V virtuális gépek replikálása az Azure storage"
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
ms.openlocfilehash: ca5bec5cf1152e6259b9fe7a869edd2d62b88e1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a><span data-ttu-id="9b5a8-103">9. lépés: Azure Hyper-V virtuális gép replikációs replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="9b5a8-103">Step 9: Set up a replication policy for Hyper-V VM replication to Azure</span></span>

<span data-ttu-id="9b5a8-104">Ez a cikk ismerteti, hogyan kell beállítani a replikációs házirendet, ha a Hyper-V virtuális gépeket replikál, az Azure (a System Center VMM nélkül) használatával a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-104">This article describes how to set up a replication policy when you're replicating Hyper-V VMs to Azure (without System Center VMM) using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="9b5a8-105">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9b5a8-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="about-snapshots"></a><span data-ttu-id="9b5a8-106">A pillanatképek kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="9b5a8-106">About snapshots</span></span>

<span data-ttu-id="9b5a8-107">A Hyper-V két különböző pillanatképtípust használ: az egyik a standard pillanatkép, amely a virtuális gép egészét lefedő növekményes pillanatképet, a másik pedig az alkalmazáskonzisztens pillanatkép, amely a virtuális gépen futó alkalmazások adatairól készült, időponthoz kötött pillanatképet jelent.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-107">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of the entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of the application data inside the virtual machine.</span></span>
    - <span data-ttu-id="9b5a8-108">Az alkalmazáskonzisztens pillanatképek a kötet árnyékmásolása szolgáltatás (VSS) segítségével garantálják, hogy az alkalmazások konzisztens állapotban legyenek a pillanatkép készítésének időpontjában.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-108">Application-consistent snapshots use Volume Shadow Copy Service (VSS) to ensure that applications are in a consistent state when the snapshot is taken.</span></span>
    - <span data-ttu-id="9b5a8-109">Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással a forrás virtuális gépeken futó alkalmazások teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-109">If you enable application-consistent snapshots, it will affect the performance of applications running on source virtual machines.</span></span> <span data-ttu-id="9b5a8-110">Ügyeljen rá, hogy az itt megadott érték kisebb legyen a további beállított helyreállítási pontok számánál.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-110">Ensure that the value you set is less than the number of additional recovery points you configure.</span></span>

## <a name="set-up-a-replication-policy"></a><span data-ttu-id="9b5a8-111">A replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="9b5a8-111">Set up a replication policy</span></span>

1. <span data-ttu-id="9b5a8-112">Új replikációs szabályzat létrehozásához kattintson az **Infrastruktúra előkészítése** > **Replikációs beállítások** > **+Létrehozás és társítás** elemre.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-112">To create a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Network (Hálózat)](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="9b5a8-114">A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-114">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="9b5a8-115">A **Másolás gyakorisága** elemmel meghatározhatja, hogy milyen gyakran szeretné replikálni a módosult adatokat a kezdeti replikációt követően (ez lehet 30 másodperc, 5 perc vagy 15 perc).</span><span class="sxs-lookup"><span data-stu-id="9b5a8-115">In **Copy frequency**, specify how often you want to replicate delta data after the initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b5a8-116">Prémium szintű tárterületre replikálás esetén nem támogatott a 30 másodperces gyakoriság.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-116">A 30 second frequency isn't supported when replicating to premium storage.</span></span> <span data-ttu-id="9b5a8-117">A korlátozás a Prémium szintű Storage által támogatott blobonkénti pillanatképek számától (100) függ.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-117">The limitation is determined by the number of snapshots per blob (100) supported by premium storage.</span></span> <span data-ttu-id="9b5a8-118">[További információk](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span><span class="sxs-lookup"><span data-stu-id="9b5a8-118">[Learn more](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span></span>

4. <span data-ttu-id="9b5a8-119">A **helyreállításipont-megőrzést**, adja meg az órában mennyi az adatmegőrzési időtartam legyen az egyes helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-119">In **Recovery point retention**, specify in hours how long the retention window is for each recovery point.</span></span> <span data-ttu-id="9b5a8-120">Virtuális gépek ablak belüli bármelyik pontra állíthatók helyre.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-120">VMs can be recovered to any point within a window.</span></span>
5. <span data-ttu-id="9b5a8-121">A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-121">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots are created.</span></span>
6. <span data-ttu-id="9b5a8-122">A **kezdeti replikáció kezdési ideje**, adja meg, mikor kell elkezdeni a kezdeti replikálást.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-122">In **Initial replication start time**, specify when to start the initial replication.</span></span> <span data-ttu-id="9b5a8-123">A replikálási folyamat az internetes sávszélességet használja, így érdemes a műveletet olyankorra ütemezni, amikor kevesen használják az internetet.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-123">The replication occurs over your internet bandwidth so you might want to schedule it outside your busy hours.</span></span> <span data-ttu-id="9b5a8-124">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-124">Then click **OK**.</span></span>

    ![Replikációs szabályzat](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

<span data-ttu-id="9b5a8-126">Ha egy új házirendet hoz létre, akkor automatikusan a Hyper-V hely társítja.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-126">When you create a new policy, it's automatically associated with the Hyper-V site.</span></span> <span data-ttu-id="9b5a8-127">A Hyper-V hely (és a benne lévő virtuális gépek) társíthatja a több replikációs házirend **replikációs** > házirend neve > **társítása Hyper-V hely**.</span><span class="sxs-lookup"><span data-stu-id="9b5a8-127">You can associate a Hyper-V site (and the VMs in it) with multiple replication policies in **Replication** > policy-name > **Associate Hyper-V Site**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="9b5a8-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b5a8-128">Next steps</span></span>

<span data-ttu-id="9b5a8-129">Ugrás a [10. lépés: replikálás engedélyezése](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="9b5a8-129">Go to [Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>
