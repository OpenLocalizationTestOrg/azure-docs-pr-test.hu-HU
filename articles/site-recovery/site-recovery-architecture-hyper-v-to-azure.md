---
title: "aaaHow nem Hyper-V replikáció tooAzure munka az Azure Site Recovery? | Microsoft Docs"
description: "Ez a cikk áttekintése összetevők és architektúra használható, ha replikálása a helyszíni Hyper-V virtuális gépek tooAzure hello Azure Site Recovery szolgáltatásban."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 3b64156307f37764a8315dec68cf297acf279530
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work-in-site-recovery"></a>Hogyan működik a Hyper-V replikáció tooAzure a Site Recovery?


Ez a cikk ismerteti a hello összetevők és a folyamatok replikálása esetén a helyszíni Hyper-V virtuális gépek hello segítségével tooAzure [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

A Site Recovery képes Hyper-V virtuális gépeket a System Center Virtual Machine Manager (VMM) szolgáltatással vagy anélkül felügyelt Hyper-V fürtökön vagy különálló gazdagépeken replikálni.

Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="architectural-components"></a>Az architektúra összetevői

Számos összetevők érintett tooAzure Hyper-V virtuális gépek replikálása esetén.

**Összetevő** | **Hely** | **Részletek**
--- | --- | ---
**Azure** | Az Azure-ban szüksége van egy Microsoft Azure-fiókra, és Azure Storage-fiókra és egy Azure-hálózatra. | Hello tárfiók tárolja a replikált adatok, és az Azure virtuális gépek replikálása hello adatokkal jönnek létre, ha a feladatátvétel a helyszíni helyről.<br/><br/> hello Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.
**VMM-kiszolgáló** | A Hyper-V-gazdagépek VMM-felhőkben helyezkednek el | Ha a VMM-felhőkben felügyelt Hyper-V-gazdagépek, Recovery Services-tároló hello hello VMM-kiszolgáló regisztrálása.<br/><br/> A VMM-kiszolgálón hello hello Site Recovery Provider tooorchestrate replikációs az Azure-ral telepítése.<br/><br/> Logikai van szüksége, és a Virtuálisgép-hálózatok tooconfigure hálózatleképezés beállításához. A Virtuálisgép-hálózat hello felhőhöz társított logikai hálózati csatolt tooa kell lennie.
**Hyper-V gazdagép** | A Hyper-V gazdagépek és fürtök VMM-kiszolgálóval vagy anélkül is üzembe helyezhetők. | Ha nem a VMM-kiszolgáló, a Site Recovery Provider telepítve van a hello állomás tooorchestrate replikálásához a Site Recovery keresztül hello hello internet. A VMM-kiszolgáló esetén hello szolgáltató telepítve van rajta, nem pedig hello állomás.<br/><br/> hello állomás toohandle adatreplikáció hello Recovery Services Agent ügynök van telepítve.<br/><br/> A szolgáltató hello és hello ügynök kommunikációit a biztonságos és titkosított. Ezenfelül az Azure-tárfiókba replikált adatok is titkosítást kapnak.
**Hyper-V virtuális gépek** | Legalább egy virtuális gépnek futnia kell egy Hyper-V-gazdakiszolgálón. | Virtuális gépekre telepített tooexplicitly kell semmi sem.

Hello telepítés előfeltételeit és ezeket az összetevőket a hello követelményeiről további [támogatási mátrix](site-recovery-support-matrix-to-azure.md).

**1. ábra: A Hyper-V helyek tooAzure replikációs**

![Összetevők](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**2. ábra: A Hyper-V a VMM-felhők tooAzure replikáció**

![Összetevők](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Replikációs folyamat

**3. ábra: A Hyper-V replikáció tooAzure replikáció és a helyreállítási folyamat**

![munkafolyamat](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Védelem engedélyezése

1. Miután engedélyezte a Hyper-V virtuális gépek védelmét, a hello Azure-portálon vagy a helyszíni hello **engedélyezni a védelmet** kezdődik.
2. hello feladat ellenőrzi, hogy hello a gép megfelel az előfeltételeknek, hello meghívása előtt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset hello-beállítások konfigurálása a replikáció.
3. hello feladat elindul a kezdeti replikáció hello figyelőn [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metódus tooinitialize teljes Virtuálisgép-replikációt és küldési hello VM virtuális lemezek tooAzure.
4. Figyelheti a feladat hello hello **feladatok** fülre.      ![Feladatok listája](media/site-recovery-hyper-v-azure-architecture/image1.png)![Védelem engedélyezésének részletei](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="replicate-hello-initial-data"></a>Hello kezdeti adatreplikációhoz

1. A kezdeti replikálás indításakor egy [pillanatfelvétel készül a Hyper-V-alapú virtuális gépről](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuális merevlemezek replikálását csak az összes másolt tooAzure fontosságúak. Sikerült az időigényes, attól függően, hogy hello Virtuálisgép-méretet, és a hálózati sávszélességre. toooptimize a hálózat használatát, lásd: [hogyan toomanage helyszíni tooAzure védelem hálózati sávszélesség használatának](https://support.microsoft.com/kb/3056159).
3. Ha a lemezen változások történnek, amíg a kezdeti replikáció folyamatban van, hello Hyper-V Replica Replication Tracker mint Hyper-V replikálási naplók (.hrl) nyomon követi ezeket a módosításokat. Ezek a fájlok találhatók hello hello lemezek ugyanabban a mappában. Minden lemezhez tartozik egy .hrl-fájl toosecondary tárolási elküldendő.
4. hello pillanatkép és a naplófájlok fájlok lemezerőforrásokat használnak, amíg a kezdeti replikáció folyamatban van.
5. Hello kezdeti replikáció befejezése után hello VM pillanatkép törlését. Hello naplóban rögzített változásokat szinkronizálja, és egyesített toohello szülőlemezt.


### <a name="finalize-protection"></a>Védelem véglegesítése

1. Hello kezdeti replikáció befejezését követően, hello **védelem véglegesítése a virtuális gép hello** feladat hálózati és a replikációt követő egyéb beállításokat konfigurálja, hogy hello virtuális gép védett.
    ![Védelem véglegesítése feladat](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Ha tooAzure replikál, akkor tootweak hello beállítások hello virtuális gép, hogy készen áll a feladatátvételt. Ezen a ponton futtatása a teszt feladatátvételi toocheck minden a várt módon működik.

### <a name="replicate-hello-delta"></a>Hello különbözeti replikáció

1. Hello kezdeti replikálás után elindul a változások szinkronizálása replikációs beállításoknak megfelelően.
2. Hyper-V Replica Replication Tracker hello hello módosítások tooa virtuális merevlemez .hrl-fájlok formájában követi nyomon. Minden replikációra konfigurált lemezhez tartozik egy .hrl fájl. Ez a napló toohello ügyfél tárfiókjával küldött kezdeti replikáció befejezése után. Amikor a napló az átvitel közben tooAzure, hello elsődleges lemez hello változásait követi egy másik naplófájlt, a hello ugyanabban a könyvtárban.
3. Kiindulási és különbözeti replikálás során hello VM hello Virtuálisgép-nézetet a figyelheti. [További információk](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Replikáció szinkronizálása

1. Ha nem sikerül a változások replikálása, és a teljes replikáció túl sok sávszélességet vagy időt venne igénybe, a rendszer a virtuális gépet megjelöli újraszinkronizálásra. Például ha hello .hrl-fájlok elérnék hello lemez méretének 50 %-át, majd hello VM lesz megjelölve az újraszinkronizálás.
2.  Az újraszinkronizálás minimálisra hello hello forrás és cél virtuális gépek ellenőrzőösszegeit, és csak a hello különbözeti adatokat küld által elküldött adatok mennyisége. Az újraszinkronizálás egy rögzített blokkméretű csonkoló algoritmust alkalmaz, amelyben a forrás- és a célfájlok rögzített méretű adattömbökre vannak osztva. Az egyes adattömbök ellenőrzőösszegeket akkor jönnek létre, és összehasonlítja a toodetermine, amely megakadályozza a hello forrás kell toobe alkalmazott toohello céltól.
3. Az újraszinkronizálás befejezését követően folytatódik a normál változásreplikálás. Alapértelmezés szerint az újraszinkronizálás automatikusan munkaidőn kívüli ütemezett toorun, de egy virtuális gépet manuálisan szinkronizálja újra. Például manuálisan folytathatja az újraszinkronizálást, ha hálózatkimaradás vagy egyéb kimaradás következik be. toodo e, jelölje be hello VM hello portálon > **újraszinkronizálása**.

    ![Manuális újraszinkronizálás](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a>Újrapróbálkozási logika

Ha hiba lép fel a replikáció során, a rendszer automatikusan újrapróbálkozik. A vonatkozó logika két kategóriába sorolható:

**Kategória** | **Részletek**
--- | ---
**Helyreállíthatatlan hibák** | A rendszer nem kísérli meg a helyreállításukat. A virtuális gép állapota **Kritikusra** vált, és rendszergazdai beavatkozás szükséges. Ezek a hibák például:; VHD-lánc megszakadt Érvénytelen állapot hello replika virtuális gép; Hálózati hitelesítési hibák: hitelesítési hibák; Virtuális gép nem található a hibák (az önálló Hyper-V kiszolgálók)
**Helyreállítható hibák** | A próbálkozások minden replikációs időköztől, használja az exponenciális vissza-ki, amely növeli a hello újrapróbálkozási időköz hello indítás hello első kísérlet 1, 2, 4, 8, és 10 perc. Ha a hiba nem szűnik meg, a rendszer 30 percenként újrapróbálkozik. Példák: hálózati hibák; nem elegendő lemezterületből fakadó hibák; alacsony memóriaállapot. |



## <a name="failover-and-failback-process"></a>Feladatátvételi és feladat-visszavételi folyamat

1. Futtathatja a tervezett vagy nem tervezett [feladatátvételi](site-recovery-failover.md) a helyszíni Hyper-V virtuális gépek tooAzure. Ha a tervezett feladatátvétel végrehajtása, majd a forrás virtuális gépeket állítsa le az tooensure adatvesztés nélküli.
2. Egyetlen gép feladatátvételt, vagy hozzon létre [helyreállítási tervek](site-recovery-create-recovery-plans.md) tooorchestrate több gép feladatátvétele.
4. Hello feladatátvétel futtatása után meg kell tudni toosee hello replika virtuális gép létrehozása az Azure-ban. A nyilvános IP-cím toohello VM rendelhet is, ha szükséges.
5. Majd véglegesíteni hello feladatátvételi toostart hello replika Azure virtuális gép hello alkalmazások és szolgáltatások eléréséhez.
6. Amint az elsődleges helyszíni hely megint elérhetővé válik, [visszaadhatja](site-recovery-failback-from-azure-to-hyper-v.md) a feladatokat. Egy tervezett feladatátvételt az Azure toohello elsődleges helyről indítsa el. A tervezett feladatátvételhez is válassza toofailback toohello azonos virtuális gép vagy tooan másik helyre, és szinkronizálja a módosítása Azure és a helyszíni, tooensure között adatvesztés nélküli Virtuális gépek létrehozásakor a helyszíni, hello feladatátvételi véglegesítést.




## <a name="next-steps"></a>Következő lépések

Felülvizsgálati hello [támogatási mátrix](site-recovery-support-matrix-to-azure.md)
