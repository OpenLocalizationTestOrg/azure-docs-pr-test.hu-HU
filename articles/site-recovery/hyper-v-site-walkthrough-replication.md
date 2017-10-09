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
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a>9. lépés: A Hyper-V virtuális gép replikációs tooAzure replikációs házirend beállítása

Ez a cikk ismerteti, hogyan tooset fel egy replikációs házirendet, ha a Hyper-V virtuális gépek tooAzure (a System Center VMM nélkül) használatával hello replikál [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>A pillanatképek kapcsolatos

Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül.
    - Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában.
    - Ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.

## <a name="set-up-a-replication-policy"></a>A replikációs házirend beállítása

1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.
3. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.

    > [!NOTE]
    > Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott. hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg. [További információk](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. A **helyreállításipont-megőrzést**, mennyi ideig óra az egyes helyreállítási pontok áll hello adatmegőrzési időtartam. Virtuális gépek lehet helyreállítani egy időszakban tooany pont.
5. A **alkalmazáskonzisztens pillanatkép gyakorisága**, adja meg, hogy milyen gyakran (1 – 12 órába) helyreállítási pontokat tartalmazó alkalmazáskonzisztens pillanatképeket jönnek létre.
6. A **kezdeti replikáció kezdési ideje**, adja meg, amikor toostart hello kezdeti replikálása. hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül. Ezután kattintson az **OK** gombra.

    ![Replikációs szabályzat](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

Ha egy új házirendet hoz létre, azt automatikusan hello Hyper-V hely társítja. A Hyper-V hely (és a benne lévő virtuális gépek hello) társíthatja a több replikációs házirend **replikációs** > házirend neve > **társítása Hyper-V hely**.



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[10. lépés: replikálás engedélyezése](hyper-v-site-walkthrough-enable-replication.md)
