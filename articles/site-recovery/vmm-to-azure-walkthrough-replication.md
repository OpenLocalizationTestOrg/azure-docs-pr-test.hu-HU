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
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a>10. lépés: A Hyper-V Virtuálisgép-replikációt (VMM) tooAzure replikációs házirend beállítása


Beállítása után [hálózatleképezés](vmm-to-azure-walkthrough-network-mapping.md), ez a cikk tooconfigure T\tooreplicate replikációs házirendet a helyszíni Hyper-V virtuális gépek kezelése a System Center Virtual Machine Manager (VMM) felhők tooAzure használja, a használatával hello [ Az Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="create-a-policy"></a>Házirend létrehozása

1. Kattintson egy új replikációs házirendet toocreate **infrastruktúra előkészítése** > **replikációs beállítások** > **+ létrehozás és társítás**.

    ![Network (Hálózat)](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. A **Házirend létrehozása és társítása** beállításnál adja meg a szabályzat nevét.
3. A **másolás gyakorisága**, adja meg, milyen gyakran tooreplicate különbözeti adatokat hello kezdeti replikálás (30 másodperces, 5 vagy 15 perc) után.

    > [!NOTE]
    >  Az 30 második gyakoriságát toopremium tárolási replikálása esetén nem támogatott. hello korlátozás hello pillanatképek számát / (100) blob támogatja prémium szintű storage határozza meg. [További információ](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. A **helyreállításipont-megőrzést**, adja meg, órákban mennyi ideig hello adatmegőrzési időtartam fogja az egyes helyreállítási pontok lehet. Védett gépek lehet helyreállítani egy időszakban tooany pont.
5. Az **Alkalmazáskonzisztens pillanatkép gyakorisága** beállítás azt határozza meg, hogy milyen gyakran hozzon létre a rendszer alkalmazáskonzisztens pillanatképeket tartalmazó helyreállítási pontokat (a beállítás értéke 1 és 12 óra között változhat). Hyper-V két különböző használ – standard pillanatkép, amely lefedő növekményes pillanatképet hello teljes virtuális gép, és egy alkalmazáskonzisztens pillanatkép, amely időpontban pillanatképet készít a hello alkalmazásadatok hello virtuális gépen belül. Alkalmazáskonzisztens pillanatképek a kötet árnyékmásolata szolgáltatás (VSS) tooensure, amely az alkalmazások konzisztens állapotban legyenek, hello pillanatkép készítésének időpontjában. Vegye figyelembe, hogy ha engedélyezi az alkalmazáskonzisztens pillanatképeket, az hatással hello forrás virtuális gépeken futó alkalmazások teljesítményére. Győződjön meg arról, hogy hello beállított érték kisebb, mint hello további helyreállítási pontok száma.
6. A **kezdeti replikáció kezdési ideje**, azt jelzi, ha toostart hello kezdeti replikálása. hello replikálást, az internetes sávszélességet, ezért érdemes tooschedule azt a foglalt munkaidőn kívül.
7. A **az Azure-on tárolt adatok titkosítása**, adja meg, hogy tooencrypt elhelyezett inaktív adatokat az Azure-tárfiókba. Ezután kattintson az **OK** gombra.

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Ha egy új házirendet hoz létre, automatikusan rendelkezik hello VMM-felhő társítva. Kattintson az **OK** gombra. További VMM-felhőkben (és a bennük foglalt virtuális gépek hello) társíthatja a replikációs házirendet a **replikációs** > szabályzat neve > **VMM-felhő társítása**.

    ![Replikációs szabályzat](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[11. lépés: replikálás engedélyezése](vmm-to-azure-walkthrough-enable-replication.md)
