---
title: "Azure virtuális gépek replikálása Azure-régiók közötti, a vész helyreállítási igényeket: Azure tooAzure |} Microsoft Docs"
description: "Tooreplicate Azure virtuális gépek Azure-régiók (Azure-Azure) hello Azure Site Recovery szolgáltatásban a vész-helyreállítási igényekre között kell hello lépéseket foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Azure virtuális gépek replikálása az Azure Site Recovery régiók között

>[!NOTE]
>
> Az Azure virtuális gépek (VM) az Azure Site Recovery replikációs jelenleg előzetes verzió.

Ez a cikk ismerteti, hogyan tooreplicate Azure virtuális gépek használatával Azure-régiók közötti hello [Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Az Azure-ban katasztrófa utáni helyreállítás

Beépített Azure infrastruktúra-szolgáltatásait és funkcióit hozzájárul az Azure virtuális gépeken futó alkalmazások és szolgáltatások tooa hatékony és rugalmas rendelkezésre állási stratégiát. Vannak azonban számos oka lehet, miért van szüksége tooplan vész-helyreállítási Azure-régiók közötti saját kezűleg is:

- Bizonyos alkalmazások és munkafolyamatok, amelyek üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia kérése toomeet megfelelőségi irányelveinek van szüksége.
- Szeretné, hogy hello képességét tooprotect és helyreállítása Azure virtuális gépeken, az üzleti döntések alapján, és nem csak a beépített Azure funkciókon alapulnak.
- Az üzleti és megfelelőségi igényeinek érintő forgalomkiesés nélkül éles megfelelően kell tootest feladatátvételét és helyreállítását.
- Toofail toohello helyreállítási régió egy olyan vészhelyzet esetén a hello eseményben van szükségük, és hátsó toohello eredeti forrás régió zökkenőmentesen sikertelen.

A Site Recovery használata az Azure-Azure virtuális gép replikációs toohelp végre ezeket a feladatokat.


## <a name="why-use-site-recovery"></a>Miért előnyös a Site Recovery használata?      

A Site Recovery Azure virtuális gépek biztosít egy egyszerű módon tooreplicate régiók között:

- **Automatikus központi telepítési**. Az aktív-aktív replikációs modell eltérően nincs szükség egy összetett és költséges infrastruktúra hello másodlagos régióban. Ha engedélyezi a replikációt, Site Recovery automatikusan létrehoz hello szükséges erőforrások hello cél régióban, forrás régió beállításai alapján.
- **Szabályozza a régiók**. A Site Recovery replikálhatja a kontinensen belül bármely régióban tooany régióban. Hasonlítsa ezt össze írásvédett georedundáns tárolás, amelyek szabványos közötti aszinkron módon replikál [régiók párosítva](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) csak. Írásvédett georedundáns tárolás adja meg a csak olvasási hozzáféréssel toohello adatait hello cél régióban.
- **Replikációs automatikus**. A Site Recovery automatizált folyamatos replikálásra biztosít. A feladatátvételi és a feladat-visszavétel egyetlen kattintással is elindítható.
- **RTO és a helyreállítási Időkorlát**. A Site Recovery kihasználja hello Azure hálózati infrastruktúra, amely a régiók tookeep RTO és a helyreállítási Időkorlát nagyon alacsony.
- **Tesztelési**. Az igény szerinti feladatátvételi teszteket, szükséges, hogy az nem befolyásolja a termelési számítási feladatokhoz vagy a folyamatban lévő replikáció vész-helyreállítási gyakorlatokat is futtathatja.
- **A helyreállítási terv**. Helyreállítási tervek tooorchestrate feladatátvétel és a feladat-visszavétel hello teljes alkalmazás több virtuális gépeken futó is használhatja. hello helyreállítási terv szolgáltatás gazdag első osztályú integrálása az Azure automation-runbook rendelkezik.


## <a name="deployment-summary"></a>Központi telepítési összefoglaló

Mire van szüksége a összegzése toodo tooset virtuális gépek Azure-régiók közötti replikáció:

1. Recovery Services-tároló létrehozása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.
2. Engedélyezze a hello Azure virtuális gépek replikációját.
3. Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

>[!IMPORTANT]
>
> Ellenőrizheti a hello [támogatási mátrixot az Azure virtuális gép replikációs](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Hogyan tooconfigure hello virtuális gépekhez szükséges hálózati kimenő kapcsolat Azure Site Recovery-replikációhoz információkért lásd: hello [hálózati útmutató](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Előkészületek

* Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable Azure virtuális gép replikációját.
* Az Azure-előfizetéssel kell engedélyezett toocreate virtuális gépe, amelyet az hello vész-helyreállítási régió szerint toouse hello célhelyet. Forduljon a támogatási szolgálathoz tooenable hello szükséges kvótát.

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Azt javasoljuk, hogy a hello helyre, ahol a virtuális gépek tooreplicate hozzon létre hello Recovery Services-tároló. Például ha a célhely van hello központi VELÜNK, hozzon létre hello tárolót a **USA középső RÉGIÓJA**.

## <a name="enable-replication"></a>A replikáció engedélyezése

A **Recovery Services-tárolók**, kattintson a hello tároló nevére. Hello tárolóban, kattintson a hello **+ replikálás** gombra.

### <a name="step-1-configure-hello-source"></a>1. lépés Állítson be hello forrás
1. A **forrás**, jelölje be **Azure - előzetes**.
2. A **adatforrásról**, jelölje be hello forrás Azure-régió, ahol a virtuális gépek futnak.
3. A virtuális gépek válassza hello telepítési modell: **erőforrás-kezelő** vagy **klasszikus**.
4. Jelölje be hello **forrás-erőforráscsoporton** erőforrás-kezelő virtuális gépek vagy **felhőalapú szolgáltatás** klasszikus virtuális gépekhez.

    ![Állítson be hello forrás](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>2. lépés Válassza ki a virtuális gépek

* Válassza ki a hello virtuális gépek tooreplicate szeretne, és kattintson a **OK**.

    ![Válassza ki a virtuális gépek](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>3. lépés Beállítások konfigurálása

1. toooverride hello alapértelmezett beállítások célozza, és hello beállításainak megadása az Ön által választott, kattintson a **Testreszabás**. További információkért lásd: [célerőforrások testreszabása](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Beállítások konfigurálása](./media/site-recovery-azure-to-azure/settings.png)


2. Alapértelmezés szerint a Site Recovery veszi alkalmazáskonzisztens pillanatképek 4 óránként, és megtartja a helyreállítási pont 24 órán át replikációs házirendet hoz létre. egy házirend különböző beállításokkal toocreate kattintson **Testreszabás** következő túl**replikációs házirend**.

    ![Házirend testreszabása](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Kattintson a toostart létesítési hello tároló erőforrásait, **célerőforrások létrehozása**. Kiépítés egy percet vesz igénybe. Ne zárja be a panelt hello kiépítése során, vagy toostart keresztül kell.

4. hello tootrigger replikálása kijelölt virtuális gép, kattintson a **engedélyezze a replikálást**.

5. Hello állapotának nyomon követheti **engedélyezni a védelmet** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.

6. A **beállítások** > **replikált elemek**, virtuális gépek hello állapotát tekintheti meg és hello a kezdeti replikáció folyamatban van. Kattintson a hello VM toodrill le azokat a beállításokat.

## <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása

Telepítése után minden, futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik:

1. toofail keresztül egyetlen számítógépen, a **beállítások** > **replikált elemek**, kattintson a virtuális gép hello **+ feladatátvételi teszt** ikonra.

2. a helyreállítás alatt toofail tervez, a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal hello terv **feladatátvételi teszt**. a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md). 

3. A **feladatátvételi teszt**, jelölje be hello cél Azure-beli virtuális hálózat toowhich Azure virtuális gépek csatlakoznak hello feladatátvételt követően.

4. toostart hello feladatátvételi, kattintson a **OK**. tootrack előrehaladás, kattintson a hello VM tooopen tulajdonságát. Hello kattintva **feladatátvételi teszt** feladat hello tároló neve > **beállítások** > **feladatok** > **SiteRecovery-feladatok**.

5. Feladatátvétel befejezését követően hello, hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**. Győződjön meg arról, hogy hello VM hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.

6. toodelete hello hello feladatátvételi teszt során létrehozott virtuális gépek kattintson **karbantartása a feladatátvételi teszt** hello a replikált elemek vagy hello helyreállítási terv. A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez. 

[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.


## <a name="next-steps"></a>Következő lépések

Miután hello központi telepítés tesztelése:

- [További](site-recovery-failover.md) feladatátvételek különböző típusú, és hogyan toorun őket.
- További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md) tooreduce RTO.
