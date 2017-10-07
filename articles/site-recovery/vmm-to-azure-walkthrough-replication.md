---
title: "aaaSet fel egy replikációs házirendet, a Hyper-V virtuális gép (a VMM-mel) replikációs tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan tooset fel egy replikációs házirendet (VMM) szolgáltatással a Hyper-V virtuális gép replikációs tooAzure az Azure Site Recovery számára"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: e1579fde559ca34eca19a01e740fec28a0df2f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a><span data-ttu-id="144e8-103">10. lépés: A Hyper-V Virtuálisgép-replikációt (VMM) tooAzure replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="144e8-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) tooAzure</span></span>


<span data-ttu-id="144e8-104">Beállítása után [hálózatleképezés](vmm-to-azure-walkthrough-network-mapping.md), ez a cikk tooconfigure T\tooreplicate replikációs házirendet a helyszíni Hyper-V virtuális gépek kezelése a System Center Virtual Machine Manager (VMM) felhők tooAzure használja, a használatával hello [ Az Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="144e8-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article tooconfigure a replication policy T\tooreplicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="144e8-105">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="144e8-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="144e8-106">Házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="144e8-106">Create a policy</span></span>

1. <span data-ttu-id="144e8-107">Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.</span><span class="sxs-lookup"><span data-stu-id="144e8-107">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="144e8-109">A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.</span><span class="sxs-lookup"><span data-stu-id="144e8-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="144e8-110">A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.</span><span class="sxs-lookup"><span data-stu-id="144e8-110">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="144e8-111">Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="144e8-111">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="144e8-112">hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg.</span><span class="sxs-lookup"><span data-stu-id="144e8-112">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="144e8-113">További információ</span><span class="sxs-lookup"><span data-stu-id="144e8-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="144e8-114">A **helyreállításipont-megőrzést**, adja meg, órákban mennyi ideig hello adatmegőrzési időtartam fogja az egyes helyreállítási pontok lehet.</span><span class="sxs-lookup"><span data-stu-id="144e8-114">In **Recovery point retention**, specify in hours how long hello retention window will be for each recovery point.</span></span> <span data-ttu-id="144e8-115">Védett gépek lehet helyreállítani egy időszakban tooany pont.</span><span class="sxs-lookup"><span data-stu-id="144e8-115">Protected machines can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="144e8-116">Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke 1 és 12 óra között változhat).</span><span class="sxs-lookup"><span data-stu-id="144e8-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="144e8-117">Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül.</span><span class="sxs-lookup"><span data-stu-id="144e8-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span> <span data-ttu-id="144e8-118">Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában.</span><span class="sxs-lookup"><span data-stu-id="144e8-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span> <span data-ttu-id="144e8-119">Vegye figyelembe, hogy ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="144e8-119">Note that if you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="144e8-120">Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.</span><span class="sxs-lookup"><span data-stu-id="144e8-120">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="144e8-121">A **kezdeti replikáció kezdési ideje**, azt jelzi, ha toostart hello kezdeti replikálása.</span><span class="sxs-lookup"><span data-stu-id="144e8-121">In **Initial replication start time**, indicate when toostart hello initial replication.</span></span> <span data-ttu-id="144e8-122">hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül.</span><span class="sxs-lookup"><span data-stu-id="144e8-122">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span>
7. <span data-ttu-id="144e8-123">A **az Azure-on tárolt adatok titkosítása**, adja meg, hogy tooencrypt elhelyezett inaktív adatokat az Azure-tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="144e8-123">In **Encrypt data stored on Azure**, specify whether tooencrypt at rest data in Azure storage.</span></span> <span data-ttu-id="144e8-124">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="144e8-124">Then click **OK**.</span></span>

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="144e8-126">Ha egy új házirendet hoz létre, automatikusan rendelkezik hello VMM-felhő társítva.</span><span class="sxs-lookup"><span data-stu-id="144e8-126">When you create a new policy it's automatically associated with hello VMM cloud.</span></span> <span data-ttu-id="144e8-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="144e8-127">Click **OK**.</span></span> <span data-ttu-id="144e8-128">További VMM-felhőkben (és a bennük foglalt virtuális gépek hello) társíthatja a replikációs házirendet a **replikációs** > szabályzat neve > **VMM-felhő társítása**.</span><span class="sxs-lookup"><span data-stu-id="144e8-128">You can associate additional VMM clouds (and hello VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="144e8-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="144e8-130">Next steps</span></span>

<span data-ttu-id="144e8-131">Nyissa meg túl[11. lépés: replikálás engedélyezése](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="144e8-131">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
