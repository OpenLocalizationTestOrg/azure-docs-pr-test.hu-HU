---
title: "Azure virtuális gép replikációs az Azure Site Recovery feladatátvételi tesztet aaaRun |} Microsoft Docs"
description: "Meg kell a feladatátvételi tesztet futtató Azure virtuális gépek replikálását tooanother az Azure-régió használatával hello Azure Site Recovery szolgáltatás hello lépéseket foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: c1f765aa94c59dd70b33317ebbcd04beb7977969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>6. lépés: Az Azure virtuális gép replikációs feladatátvételi teszt futtatása

Miután engedélyezte az Azure virtuális gép (VM) replikálása, lépésekkel hello ebben a cikkben toorun feladatátvételi teszt Azure-régió, egy tooanother hello segítségével a [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

- Hello cikk befejezése után kell ellenőrizte a feladatátvételi tesztet, hogy legalább egy Azure virtuális gép tooyour másodlagos Azure régióra átveheti. 
- Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Az Azure virtuális gép replikációs jelenleg előzetes verzió.


## <a name="before-you-start"></a>Előkészületek

- Feladatátvételi teszt futtatása előtt javasoljuk, hogy ellenőrizze hello virtuális gép tulajdonságait, és hajtsa végre a módosításokat kell. van-e hozzáférési hello Virtuálisgép-tulajdonságokat **replikált elemek**. Hello **Essentials** panel gépek beállítások és állapotával kapcsolatos adatokat jeleníti meg.
- Javasoljuk, hogy egy külön Azure Virtuálisgép-hálózat hello feladatátvételi teszthez, és nem hello hálózati használja (alapértelmezett vagy egyéni), amely be lett állítva a termelési feladatátvételre.
- hello teszt feladatátvételi Azure virtuális gépek (és azok tárolási) feladatátvételt hajt végre toohello másodlagos Azure-régiót. Azt nem replikálja az függő alkalmazásokhoz és erőforrásokhoz. Ha sikertelen az állomásokon futó virtuális gépek alkalmazások függenek más erőforrások, például az Active Directory vagy a DNS, az kell tooreplicate ezek is, ha azok már nem érhető el a másodlagos régióban. [További információ](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Ha azt szeretné, hogy tooaccess replikált virtuális gépek egy helyszíni hely a feladatátvétel után, akkor túl kell[tooconnect előkészítése](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) toothese virtuális gépeket.

## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

1. A **beállítások** > **replikált elemek**, kattintson a virtuális gép hello **+ feladatátvételi teszt** ikonra. 

2. A **feladatátvételi teszt**, válassza ki a helyreállítási pont toouse hello feladatátvételi:

    - **Legújabb feldolgozott**: sikertelen a virtuális gép hello toohello legújabb helyreállítási pont hello Site Recovery szolgáltatás által feldolgozott keresztül. hello időbélyeg jelenik meg. Ezzel a beállítással nem van feldolgozásával töltött idő adatokat, így egy alacsony RTO (helyreállítási idő célkitűzése) biztosít.
    - **Legutóbbi alkalmazáskonzisztens**: Ez a beállítás átadja a feladatokat az összes virtuális gépek toohello legutóbbi alkalmazáskonzisztens helyreállítási pontnak. hello időbélyeg jelenik meg. 
    - **Egyéni**: válassza ki a helyreállítási pontot.
 
3. Jelölje be hello cél Azure-beli virtuális hálózat toowhich hello másodlagos régióban Azure virtuális gépek csatlakoznak, hello feladatátvételt követően.
4. toostart hello feladatátvételi, kattintson a **OK**. tootrack előrehaladás, kattintson a hello VM tooopen tulajdonságát. És kattintson a hello **feladatátvételi teszt** feladat hello tároló neve > **beállítások** > **feladatok** > **SiteRecovery-feladatok**.
5. Feladatátvétel befejezése után hello, hello replika Azure virtuális gép hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy hello VM hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.
6. toodelete hello hello feladatátvételi teszt során létrehozott virtuális gépek kattintson **karbantartása a feladatátvételi teszt** hello a replikált elemek vagy hello helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. 

[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.

## <a name="next-steps"></a>Következő lépések

Miután feladatátvételi tesztelését, ez a forgatókönyv számára befejeződött. Most lásd még: feladatátvételek éles környezetben:

- [További](site-recovery-failover.md) kapcsolatos különféle a feladatátvételeket, és hogyan toorun őket.
- További tudnivalók hibás át több virtuális gép [segítségével a helyreállítási terv](site-recovery-create-recovery-plans.md).
- További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md).
- További információ [újbóli védelméhez az Azure virtuális gépek](site-recovery-how-to-reprotect.md) feladatátvételt követően.

