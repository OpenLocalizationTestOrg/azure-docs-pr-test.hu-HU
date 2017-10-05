---
title: "A Hyper-V virtuális gép (a VMM-mel) replikáció az Azure szolgáltatásban az Azure Site Recovery replikációs házirend beállítása |} Microsoft Docs"
description: "Ismerteti, hogyan lehet a Hyper-V virtuális gép (a VMM-mel) replikáció az Azure szolgáltatásban az Azure Site Recovery replikációs házirend beállítása"
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
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a><span data-ttu-id="d6b77-103">10. lépés: Az Azure-ba (VMM) szolgáltatással Hyper-V Virtuálisgép-replikációt a replikációs házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="d6b77-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) to Azure</span></span>


<span data-ttu-id="d6b77-104">Beállítása után [hálózatleképezés](vmm-to-azure-walkthrough-network-mapping.md), használja a cikk egy replikációs házirend T\to replikálás konfigurálása a helyszíni Hyper-V virtuális gépek kezelése a System Center Virtual Machine Manager (VMM) felhők az Azure-bA a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d6b77-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article to configure a replication policy T\to replicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="d6b77-105">A cikk elolvasása után felmerülő megjegyzéseit alul vagy az [Azure Recovery Services fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti közzé.</span><span class="sxs-lookup"><span data-stu-id="d6b77-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="d6b77-106">Házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6b77-106">Create a policy</span></span>

1. <span data-ttu-id="d6b77-107">Új replikációs szabályzat létrehozásához kattintson az **Infrastruktúra előkészítése** > **Replikációs beállítások** > **+Létrehozás és társítás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d6b77-107">To create a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="d6b77-109">A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.</span><span class="sxs-lookup"><span data-stu-id="d6b77-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="d6b77-110">A **Másolás gyakorisága** elemmel meghatározhatja, hogy milyen gyakran szeretné replikálni a módosult adatokat a kezdeti replikációt követően (ez lehet 30 másodperc, 5 perc vagy 15 perc).</span><span class="sxs-lookup"><span data-stu-id="d6b77-110">In **Copy frequency**, specify how often you want to replicate delta data after the initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="d6b77-111">Prémium szintű tárterületre replikálás esetén nem támogatott a 30 másodperces gyakoriság.</span><span class="sxs-lookup"><span data-stu-id="d6b77-111">A 30 second frequency isn't supported when replicating to premium storage.</span></span> <span data-ttu-id="d6b77-112">A korlátozás a Prémium szintű Storage által támogatott blobonkénti pillanatképek számától (100) függ.</span><span class="sxs-lookup"><span data-stu-id="d6b77-112">The limitation is determined by the number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="d6b77-113">További információ</span><span class="sxs-lookup"><span data-stu-id="d6b77-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="d6b77-114">A **Helyreállítási pont megőrzése** beállításnál azt adhatja meg, hogy hány órás legyen az egyes helyreállítási pontok adatmegőrzési időtartama.</span><span class="sxs-lookup"><span data-stu-id="d6b77-114">In **Recovery point retention**, specify in hours how long the retention window will be for each recovery point.</span></span> <span data-ttu-id="d6b77-115">A védelemmel ellátott gépeket az időtartamon belüli bármelyik pontra visszaállíthatja.</span><span class="sxs-lookup"><span data-stu-id="d6b77-115">Protected machines can be recovered to any point within a window.</span></span>
5. <span data-ttu-id="d6b77-116">Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke 1 és 12 óra között változhat).</span><span class="sxs-lookup"><span data-stu-id="d6b77-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="d6b77-117">A Hyper-V két különböző pillanatképtípust használ: az egyik a standard pillanatkép, amely a virtuális gép egészét lefedő növekményes pillanatképet, a másik pedig az alkalmazáskonzisztens pillanatkép, amely a virtuális gépen futó alkalmazások adatairól készült, időponthoz kötött pillanatképet jelent.</span><span class="sxs-lookup"><span data-stu-id="d6b77-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of the entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of the application data inside the virtual machine.</span></span> <span data-ttu-id="d6b77-118">Az alkalmazáskonzisztens pillanatképek a kötet árnyékmásolása szolgáltatás (VSS) segítségével garantálják, hogy az alkalmazások konzisztens állapotban legyenek a pillanatkép készítésének időpontjában.</span><span class="sxs-lookup"><span data-stu-id="d6b77-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) to ensure that applications are in a consistent state when the snapshot is taken.</span></span> <span data-ttu-id="d6b77-119">Ne feledje, hogy az alkalmazáskonzisztens pillanatképek bekapcsolása negatív hatással lesz a forrás virtuális gépeken futó alkalmazások teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="d6b77-119">Note that if you enable application-consistent snapshots, it will affect the performance of applications running on source virtual machines.</span></span> <span data-ttu-id="d6b77-120">Ügyeljen rá, hogy az itt megadott érték kisebb legyen a további beállított helyreállítási pontok számánál.</span><span class="sxs-lookup"><span data-stu-id="d6b77-120">Ensure that the value you set is less than the number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="d6b77-121">A **Kezdeti replikáció kezdési ideje** a kezdeti replikáció kezdésének időpontját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d6b77-121">In **Initial replication start time**, indicate when to start the initial replication.</span></span> <span data-ttu-id="d6b77-122">A replikálási folyamat az internetes sávszélességet használja, így érdemes a műveletet olyankorra ütemezni, amikor kevesen használják az internetet.</span><span class="sxs-lookup"><span data-stu-id="d6b77-122">The replication occurs over your internet bandwidth so you might want to schedule it outside your busy hours.</span></span>
7. <span data-ttu-id="d6b77-123">**Az Azure-on tárolt adatok titkosítása** beállításnál adhatja meg, hogy szeretné-e titkosítani az Azure-tárfiókban elhelyezett inaktív adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6b77-123">In **Encrypt data stored on Azure**, specify whether to encrypt at rest data in Azure storage.</span></span> <span data-ttu-id="d6b77-124">Végül kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b77-124">Then click **OK**.</span></span>

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="d6b77-126">Az újonnan létrehozott szabályzatokat a rendszer automatikusan társítja a VMM-felhővel.</span><span class="sxs-lookup"><span data-stu-id="d6b77-126">When you create a new policy it's automatically associated with the VMM cloud.</span></span> <span data-ttu-id="d6b77-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b77-127">Click **OK**.</span></span> <span data-ttu-id="d6b77-128">A további VMM-felhőket (és a rajtuk futó virtuális gépeket) a **Replikáció** > szabályzat neve > **VMM-felhő társítása** menüpontban társíthatja az adott replikációs szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="d6b77-128">You can associate additional VMM clouds (and the VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="d6b77-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6b77-130">Next steps</span></span>

<span data-ttu-id="d6b77-131">Ugrás a [11. lépés: replikálás engedélyezése](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="d6b77-131">Go to [Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
