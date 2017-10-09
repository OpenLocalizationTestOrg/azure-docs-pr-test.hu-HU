---
title: "aaaHow Hyper-V replikáció tooAzure munka a Site Recovery szolgáltatásban működik? | Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan működik a Hyper-V-replikáció az Azure Site Recoveryben"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-architecture-hyper-v-to-azure
ms.openlocfilehash: e982806b4d6cdec2f71f82d8c73c17cc50ad3c33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-hyper-v-replication-tooazure-work"></a>Hogyan működik a Hyper-V replikáció tooAzure?

Ez a cikk toounderstand hello architektúra és a munkafolyamatokat a hello segítségével a Hyper-V replikáció tooAzure olvasási [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Ez a cikk vagy hello hello alsó megjegyzések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

A következő tooAzure hello replikálhatja:
- **Hyper-V VMM-mel**: System Center Virtual Machine Manager-felhőkben (VMM-felhőkben) felügyelt helyszíni Hyper-V-gazdagépeken található virtuális gépek. A gazdagépek bármilyen [támogatott operációs rendszert](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) futtathatnak. A [Hyper-V és az Azure által támogatott](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bármilyen vendég operációs rendszert futtató Hyper-V-alapú virtuális gépet replikálhat.
- **Hyper-V VMM nélkül**: VMM-felhőkben nem felügyelt Hyper-V-gazdagépeken található helyszíni virtuális gépek. Gazdagépek futtatható bármely hello [támogatott operációs rendszerek](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions). A [Hyper-V és az Azure által támogatott](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bármilyen vendég operációs rendszert futtató Hyper-V-alapú virtuális gépet replikálhat.


## <a name="architectural-components"></a>Az architektúra összetevői

**Terület** | **Összetevő** | **Részletek**
--- | --- | ---
**Azure** | Az Azure-ban szüksége van egy Microsoft Azure-fiókra, és Azure Storage-fiókra és egy Azure-hálózatra. | A Storage és a hálózat lehet Resource Manager-alapú vagy klasszikus fiók.<br/><br/> Hello tárfiók tárolja a replikált adatok, és az Azure virtuális gépek replikálása hello adatokkal jönnek létre, ha a feladatátvétel a helyszíni helyről.<br/><br/> hello Azure virtuális gépek Azure-beli virtuális hálózat toohello csatlakozás, ha a jönnek létre.
**VMM-kiszolgáló** | VMM-felhőkben lévő Hyper-V-gazdagépek | Ha a VMM-felhőkben felügyelt Hyper-V-gazdagépek, Recovery Services-tároló hello hello VMM-kiszolgáló regisztrálása.<br/><br/> A VMM-kiszolgálón hello hello Site Recovery Provider tooorchestrate replikációs az Azure-ral telepítése.<br/><br/> Logikai van szüksége, és a Virtuálisgép-hálózatok tooconfigure hálózatleképezés beállításához. A Virtuálisgép-hálózat hello felhőhöz társított logikai hálózati csatolt tooa kell lennie.
**Hyper-V gazdagép** | A Hyper-V-kiszolgálók VMM-kiszolgálóval vagy anélkül is üzembe helyezhetők. | Ha nem a VMM-kiszolgáló, a Site Recovery Provider telepítve van a hello állomás tooorchestrate replikálásához a Site Recovery keresztül hello hello internet. A VMM-kiszolgáló esetén hello szolgáltató telepítve van rajta, nem pedig hello állomás.<br/><br/> hello állomás toohandle adatreplikáció hello Recovery Services Agent ügynök van telepítve.<br/><br/> A szolgáltató hello és hello ügynök kommunikációit a biztonságos és titkosított. Ezenfelül az Azure-tárfiókba replikált adatok is titkosítást kapnak.
**Hyper-V virtuális gépek** | Egy vagy több hello Hyper-V gazdakiszolgálón futó virtuális gépek van szüksége. | Mit kell virtuális gépekre telepített tooexplicitly

## <a name="deployment-steps"></a>A központi telepítés lépései

1. **Azure**: beállítása hello Azure összetevőket. Javasoljuk, hogy a Site Recovery üzembe helyezésének megkezdése előtt hozza létre a Storage- és hálózati fiókokat.
2. **Tároló**: Hozzon létre egy Recovery Services-tárolót a Site Recoveryhez, és konfigurálja a tároló beállításait, például konfigurálja a forrás- és célbeállításokat, állítson be egy replikációs szabályzatot és engedélyezze a replikációt.
3. **Forrás és cél**:
    - **Hyper-V gazdagépek a VMM-felhőkben**: az adatforrás-beállítások megadása a részeként, töltse le és hello Azure Site Recovery Provider telepítése hello VMM-kiszolgálón, és minden Hyper-V gazdagépen hello Azure Recovery Services Agent ügynököt. hello lesznek hello VMM-kiszolgálón. hello Azure célja.
    - Hyper-V gazdagépeket VMM nélkül: az adatforrás-beállítások megadása esetén töltse le, és hello szolgáltató és az ügynök telepítése minden Hyper-V gazdagépen. Központi telepítése során hello állomások gyűjt be a Hyper-V hely, és adja meg a hely hello forrásaként. hello Azure célja.

    ![A VMM/Hyper-V replikáció tooAzure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png) ![Hyper-V hely replikációs tooAzure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Replikációs szabályzat**: hello Hyper-V hely vagy a VMM-felhő replikációs házirend létrehozása. hello házirend alkalmazott tooall virtuális gépek gazdagépén hello hely vagy a felhőben található.
5. **Replikáció engedélyezése**: Engedélyezi a replikációt a Hyper-V-alapú virtuális gépek számára. Kezdeti replikáció hello replikációs házirend-beállításoknak megfelelően. Adatok változások nyomon követése, és a különbözeti módosítások tooAzure replikálása hello kezdeti replikáció befejezését követően megkezdődik. Az elemek nyomon követett módosításait a rendszer .hrl fájlokban tárolja.
6. **Feladatátvételi teszt**: futtatja, a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

További információk az üzembe helyezésről:
- [Hyper-V virtuális gép replikációs tooAzure - és a VMM az első lépései](site-recovery-vmm-to-azure.md)
- [Ismerkedés a Hyper-V virtuális gép replikációs tooAzure - VMM nélkül](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>A Hyper-V replikálás munkafolyamata

### <a name="enable-protection"></a>Védelem engedélyezése

1. Miután engedélyezte a Hyper-V virtuális gépek védelmét, a hello Azure-portálon vagy a helyszíni hello **engedélyezni a védelmet** kezdődik.
2. hello feladat ellenőrzi, hogy hello a gép megfelel az előfeltételeknek, hello meghívása előtt [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), tooset hello-beállítások konfigurálása a replikáció.
3. hello feladat elindul a kezdeti replikáció hello figyelőn [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metódus tooinitialize teljes Virtuálisgép-replikációt és küldési hello VM virtuális lemezek tooAzure.
4. Figyelheti a feladat hello hello **feladatok** fülre.      ![Feladatok listája](media/site-recovery-hyper-v-azure-architecture/image1.png)![Védelem engedélyezésének részletei](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Kezdeti replikálás

1. A kezdeti replikálás indításakor egy [pillanatfelvétel készül a Hyper-V-alapú virtuális gépről](https://technet.microsoft.com/library/dd560637.aspx).
2. Virtuális merevlemezek replikálását csak az összes másolt tooAzure fontosságúak. Sikerült az időigényes, attól függően, hogy hello Virtuálisgép-méretet, és a hálózati sávszélességre. toooptimize a hálózat használatát, lásd: [hogyan toomanage helyszíni tooAzure védelem hálózati sávszélesség használatának](https://support.microsoft.com/kb/3056159).
3. Ha a lemezen változások történnek, amíg a kezdeti replikáció folyamatban van, hello Hyper-V Replica Replication Tracker mint Hyper-V replikálási naplók (.hrl) nyomon követi ezeket a módosításokat. Ezek a fájlok találhatók hello hello lemezek ugyanabban a mappában. Minden lemezhez tartozik egy .hrl-fájl toosecondary tárolási elküldendő.
4. hello pillanatkép és a naplófájlok fájlok lemezerőforrásokat használnak, amíg a kezdeti replikáció folyamatban van.
5. Hello kezdeti replikáció befejezése után hello VM pillanatkép törlését. Hello naplóban rögzített változásokat szinkronizálja, és egyesített toohello szülőlemezt.


### <a name="finalize-protection"></a>Védelem véglegesítése

1. Hello kezdeti replikáció befejezését követően, hello **védelem véglegesítése a virtuális gép hello** feladat hálózati és a replikációt követő egyéb beállításokat konfigurálja, hogy hello virtuális gép védett.
    ![Védelem véglegesítése feladat](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Ha tooAzure replikál, akkor tootweak hello beállítások hello virtuális gép, hogy készen áll a feladatátvételt. Ezen a ponton futtatása a teszt feladatátvételi toocheck minden a várt módon működik.

### <a name="delta-replication"></a>Változásreplikáció

1. Hello kezdeti replikálás után elindul a változások szinkronizálása replikációs beállításoknak megfelelően.
2. Hyper-V Replica Replication Tracker hello hello módosítások tooa virtuális merevlemez .hrl-fájlok formájában követi nyomon. Minden replikációra konfigurált lemezhez tartozik egy .hrl fájl. Ez a napló toohello ügyfél tárfiókjával küldött kezdeti replikáció befejezése után. Amikor a napló az átvitel közben tooAzure, hello elsődleges lemez hello változásait követi egy másik naplófájlt, a hello ugyanabban a könyvtárban.
3. Kiindulási és különbözeti replikálás során hello VM hello Virtuálisgép-nézetet a figyelheti. [További információk](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Replikációszinkronizálás

1. Ha nem sikerül a változások replikálása, és a teljes replikáció túl sok sávszélességet vagy időt venne igénybe, a rendszer a virtuális gépet megjelöli újraszinkronizálásra. Például ha hello .hrl-fájlok elérnék hello lemez méretének 50 %-át, majd hello VM lesz megjelölve az újraszinkronizálás.
2.  Az újraszinkronizálás minimálisra hello hello forrás és cél virtuális gépek ellenőrzőösszegeit, és csak a hello különbözeti adatokat küld által elküldött adatok mennyisége. Az újraszinkronizálás egy rögzített blokkméretű csonkoló algoritmust alkalmaz, amelyben a forrás- és a célfájlok rögzített méretű adattömbökre vannak osztva. Az egyes adattömbök ellenőrzőösszegeket akkor jönnek létre, és összehasonlítja a toodetermine, amely megakadályozza a hello forrás kell toobe alkalmazott toohello céltól.
3. Az újraszinkronizálás befejezését követően folytatódik a normál változásreplikálás. Alapértelmezés szerint az újraszinkronizálás automatikusan munkaidőn kívüli ütemezett toorun, de egy virtuális gépet manuálisan szinkronizálja újra. Például manuálisan folytathatja az újraszinkronizálást, ha hálózatkimaradás vagy egyéb kimaradás következik be. toodo e, jelölje be hello VM hello portálon > **újraszinkronizálása**.

    ![Manuális újraszinkronizálás](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Újrapróbálkozások

Ha hiba lép fel a replikáció során, a rendszer automatikusan újrapróbálkozik. A vonatkozó logika két kategóriába sorolható:

**Kategória** | **Részletek**
--- | ---
**Helyreállíthatatlan hibák** | A rendszer nem kísérli meg a helyreállításukat. A virtuális gép állapota **Kritikusra** vált, és rendszergazdai beavatkozás szükséges. Ezek a hibák például:; VHD-lánc megszakadt Érvénytelen állapot hello replika virtuális gép; Hálózati hitelesítési hibák: hitelesítési hibák; Virtuális gép nem található a hibák (az önálló Hyper-V kiszolgálók)
**Helyreállítható hibák** | A próbálkozások minden replikációs időköztől, használja az exponenciális vissza-ki, amely növeli a hello újrapróbálkozási időköz hello indítás hello első kísérlet 1, 2, 4, 8, és 10 perc. Ha a hiba nem szűnik meg, a rendszer 30 percenként újrapróbálkozik. Példák: hálózati hibák; nem elegendő lemezterületből fakadó hibák; alacsony memóriaállapot. |

## <a name="protection-and-recovery-lifecycle"></a>A védelem és helyreállítás életciklusa

![munkafolyamat](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Következő lépések

- [Üzembe helyezési előfeltételek ellenőrzése](site-recovery-prereq.md)
- Hibaelhárítás a következőkkel:
    - [Védelem figyelése és hibaelhárítása](site-recovery-monitoring-and-troubleshooting.md)
    - [A Microsoft ügyfélszolgálatának segítsége](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Gyakori hibák és megoldások](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)
