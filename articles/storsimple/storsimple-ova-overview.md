---
title: "aaaMicrosoft Azure StorSimple virtuális tömb áttekintése |} Microsoft Docs"
description: "A StorSimple virtuális tömb, egy integrált tárolási megoldás, amely kezeli a Microsoft Azure felhőalapú tárolást és a helyszíni virtuális tömb közötti tárolási feladatok elvégzéséről hello ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/09/2016
ms.author: alkohli
ms.openlocfilehash: 8978e074142940748857150cc93b37272349d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-storsimple-virtual-array"></a>Bevezetés toohello StorSimple virtuális tömb
## <a name="overview"></a>Áttekintés
a Microsoft Azure StorSimple virtuális tömb hello egy integrált tárolási megoldás, amely kezeli a tárolási feladatok elvégzéséről a hipervizor és a Microsoft Azure felhőalapú tárolást futtató helyszíni virtuális tömb között. hello virtuális egy hatékony, költséghatékony és egyszerűen kezelhető fájl- vagy iSCSI-kiszolgálói megoldás, amely kiküszöböli számos hello problémák és a vállalati tárolás és az adatvédelem társított is. hello virtuális tömbjének értéke különösen jól alkalmazható, távoli office/fiókirodák számára.

Ez a témakör áttekintést hello virtuális tömb – az alábbiakban néhány egyéb erőforrások:

* Gyakorlati tanácsok, lásd: [StorSimple virtuális tömb gyakorlati tanácsok](storsimple-ova-best-practices.md).
* Hello StorSimple 8000 sorozat eszközeire áttekintéséhez lépjen túl[StorSimple 8000 series: hibrid felhő megoldás](storsimple-overview.md). 
* A StorSimple 5000/7000-es hello adatsorozat eszközökkel kapcsolatos információkért lásd az túl[StorSimple Online súgó](http://onlinehelp.storsimple.com/).

hello virtuális tömb hello iSCSI vagy Server Message Block (SMB) protokollt támogatja. A meglévő hipervizor infrastruktúra fut, és rétegezési toohello felhő, felhőbeli biztonsági mentését, gyors visszaállítási, elemszintű helyreállítás és vész-helyreállítási szolgáltatás.

hello következő táblázat összefoglalja hello fontosabb funkciói a StorSimple virtuális tömb hello.

| Szolgáltatás | StorSimple Virtual Array |
| --- | --- |
| Telepítési követelmények |Használja a virtualizálási infrastruktúrában (Hyper-V vagy VMware) |
| Rendelkezésre állás |Egyetlen csomópont |
| Teljes kapacitás (például felhő) |Too64 TB használható kapacitás virtuális tömb mentése |
| Helyi kapacitás |390 GB too6.4 TB használható kapacitás virtuális tömb (kell tooprovision 500 GB too8 TB lemezterület) |
| Natív protokollok |iSCSI- vagy SMB |
| Helyreállítási időre vonatkozó célkitűzés (RTO) |iSCSI: függetlenül mérete kisebb, mint 2 perc |
| Helyreállítási időkorlát (RPO) |Napi és igény szerinti biztonsági mentés |
| Tároló rétegezésével |Felhasználási melegítsük leképezési toodetermine milyen adatokat kell helyezhető el a bejövő vagy kimenő |
| Támogatás |Virtualizálási infrastruktúrájával hello szállító által támogatott |
| Teljesítmény |Függ az alapul szolgáló infrastruktúra |
| Adatok mobilitási |Visszaállíthatja a toohello ugyanarra az eszközre vagy elemszintű helyreállítás (fájlkiszolgáló) |
| A tárolási rétegek |Helyi hipervizor tárolás és a felhő |
| A fájlmegosztás méretének |Rétegzett: mentése too20 TB; helyileg rögzített: akár too2 TB |
| Kötet mérete |Rétegzett: 500 GB too5 TB; helyileg rögzített: 50 GB too500 GB |
| Kötet mérete |Rétegzett: mentése too5 TB; helyileg rögzített: akár too500 GB |
| A pillanatképek |Összeomlás-konzisztens |
| Elemszintű helyreállítás |Igen; felhasználók visszaállíthatják a megosztások |

## <a name="why-use-storsimple"></a>StorSimple miért érdemes használni?
StorSimple-felhasználók és a kiszolgálók tooAzure tárolási csatlakozik (percben), az alkalmazás módosítás nélkül.

hello alábbi táblázat néhány hello nagy előnye, hogy a megoldás a StorSimple virtuális tömb hello.

| Szolgáltatás | Előny |
| --- | --- |
| Transzparens integráció |hello virtuális tömb hello iSCSI vagy hello SMB protokollt támogatja. zökkenőmentes és átlátható toohello felhasználó az hello adatmozgatás hello helyi rétegen és hello felhő szint között. |
| Alacsonyabb tárolási költségek |A StorSimple kiépítése elegendő helyi tárolók toomeet aktuális terén a leggyakrabban használt hello gyakran használt adatokkal. Tárolási igényei nő, mivel a StorSimple rétegek ritkán használt adatok költséghatékony a felhőbeli tárhelyén. hello az adatok deduplikációja és tömörített toohello felhő elküldése előtt toofurther csökken a tárhellyel kapcsolatos követelmények és a költségeket. |
| Egyszerűsített tárolók kezelése |StorSimple több eszközt a StorSimple Device Manager toomanage hello felhőbe központi kezelésére ad lehetőséget. |
| Továbbfejlesztett vész-helyreállítási és megfelelőség |StorSimple elősegíti a gyorsabb vész-helyreállítási hello metaadatok azonnal visszaállítása, és igény szerint hello adat-visszaállítást. Ez azt jelenti, hogy a normál működés minimális megszakítás folytatása. |
| Adatok mobilitási |Az adatok rétegzett toohello felhő elérhetők a helyreállítási és áttelepítési célokból más helyekre. Vegye figyelembe, hogy visszaállíthassa-e az adatok csak toohello eredeti virtuális tömb. Azonban használható vész helyreállítási funkciók toorestore hello teljes virtuális tömb tooanother virtuális tömb. |

## <a name="storsimple-workload-summary"></a>StorSimple a számítási feladatok összefoglalása

Támogatott StorSimple munkaterhelések összefoglalását az alábbi táblázatban.

|Forgatókönyv     |Számítási feladat     |Támogatott      |Korlátozások               |
|-------------|-------------|---------------|---------------------------|
|ROBO együttműködés |Fájlmegosztás     |Igen      |Lásd: [fájlkiszolgáló határok](storsimple-ova-limits.md).<br></br>Lásd: [támogatott SMB verzióját rendszerkövetelményei](storsimple-ova-system-requirements.md).| Az összes verzió     |

## <a name="workflows"></a>Munkafolyamatok
a StorSimple virtuális tömb hello különösen alkalmas munkafolyamatok következő hello:

* [Felhőalapú tárolók kezelése](#cloud-based-storage-management)
* [Független hely biztonsági mentése](#location-independent-backup)
* [Adatok védelme és katasztrófa-helyreállítás](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>felhőalapú tárolók kezelése
Hello StorSimple Device Manager szolgáltatás fut, az Azure portál toomanage adatok több eszközön tárolt hello több helyen is használhatja. Ez különösen fontos elosztott fiókirodai forgatókönyvben. Vegye figyelembe, hogy hello StorSimple Device Manager szolgáltatás toomanage virtuális tömbök és a fizikai StorSimple eszközökhöz külön példányát kell létrehozni. Ne feledje, hogy hello virtuális tömb most már használja a hello új Azure-portálon helyett hello a klasszikus Azure portálon.

![felhőalapú tárolók kezelése](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Független hely biztonsági mentése
Hello virtuális tömb felhőalapú pillanatfelvételek tartalmaz egy kötetet vagy megosztást helyfüggetlen, pont időponthoz kötött másolata. Felhőalapú pillanatfelvételek alapértelmezés szerint engedélyezve vannak, és nem tiltható le. Minden kötetek és megosztások biztonsági mentése hello, azonos időben keresztül napi biztonsági mentési házirend, és választhat, hogy további ad hoc biztonsági mentés minden szükséges.

### <a name="data-protection-and-disaster-recovery"></a>Adatok védelme és katasztrófa-helyreállítás
hello virtuális tömb támogatja az adatok védelme és vészhelyreállítási forgatókönyvek a következő hello:

* **Kötetet vagy megosztást visszaállítási** – hello visszaállítási használják az új munkafolyamat toorecover egy kötet vagy megosztás. Ez a megközelítés toorecover hello teljes kötetet vagy megosztást használja.
* **Elemszintű helyreállítási** – megosztások hozzáférést egyszerűsített toorecent biztonsági mentéseket. Könnyen kártérítés fájlhoz egy speciális *.backup* hello felhőben elérhető mappát. A visszaállítási funkció felhasználói és rendszergazdai beavatkozás nélküli szükség.
* **Vész-helyreállítási** – használható hello feladatátvételi képesség toorecover összes kötetek vagy megosztások tooa új virtuális tömb. Hozzon létre új virtuális tömb hello és hello StorSimple eszköz Manager szolgáltatásban való regisztrálása, majd átadja az eredeti virtuális tömb hello. új virtuális tömb hello azt fogja feltételezni hello kiosztott erőforrásokat. 

## <a name="storsimple-virtual-array-components"></a>A StorSimple virtuális tömb összetevők
hello virtuális tömb hello a következő összetevőket tartalmazza:

* [Virtuális tömb](#virtual-array) – egy hibrid felhő tárolóeszköz létre a virtuális környezet vagy a hipervizor a virtuális gép alapján.  
* [StorSimple Device Manager szolgáltatás](#storsimple-device-manager-service) – az Azure-portál, amely lehetővé teszi egy vagy több StorSimple eszközt, amely földrajzilag különböző helyeken érhető el egyetlen webes felhasználói felületen keresztüli kezelését hello kiterjesztését. Akkor is hello StorSimple Device Manager szolgáltatás toocreate használja és szolgáltatások, megtekintheti és kezelheti az eszközöket és a riasztások, valamint kezelheti és kötetek, megosztások és a meglévő pillanatképeket.
* [Helyi webes felhasználói felület](#local-web-user-interface) – egy webes felhasználói felület, amely használt tooconfigure hello eszközt, hogy azt toohello helyi hálózat, és regisztrálja a hello eszköz hello StorSimple Device Manager szolgáltatással. 
* [Parancssori felület](#command-line-interface) – egy Windows PowerShell-felületet használható egy támogatási munkamenet toostart hello virtuális tömbben.
  hello alábbi szakaszok ismertetik ezeket az összetevőket nagyobb részletességgel és azt ismertetik, hogyan hello megoldás adatok elrendezése, foglal helyet, és elősegíti a tárolók kezelése és az adatok védelmére.

### <a name="virtual-array"></a>Virtuális tömb
hello virtuális tömbjének értéke egy egy csomópontos tárolási megoldás, amely elsődleges tárolására szolgál, a felhőalapú tárolással kommunikációt kezeli, és segít tooensure hello biztonsági és az adatok bizalmas mivoltát összes hello eszközön tárolt.

egy olyan modell, amely letölthető hello virtuális tömb érhető el. hello virtuális tömb maximális kapacitása nem 6.4-es TB hello eszközt (az egy alapul szolgáló tárolási követelmény 8 TB-os) és 64 TB-ot, így a felhőbeli tárhelyén. 

hello virtuális tömb rendelkezik a következő funkciók hello:

* Költséghatékony. Azt elérhetővé válnak a meglévő virtualizációs infrastruktúra használatára és a meglévő Hyper-V vagy VMware hipervizor telepíthetők.
* Hello datacenter fájlcsoportban helyezkedik el, és az iSCSI-kiszolgáló vagy egy fájlkiszolgáló konfigurálhatók. 
* Integrálva van a hello felhőalapú.
* Biztonsági mentések hello felhőben, amely katasztrófa utáni helyreállítás elősegítheti és egyszerűbbé válik a elemszintű helyreállítás (ILR) vannak tárolva. 
* Frissítések toohello virtuális tömb, is alkalmazhat, mint tooa fizikai eszköz akkor vonatkozik rájuk.

> [!NOTE]
> Egy virtuális tömb nem bonthatók ki. Ezért nagyon fontos tooprovision megfelelő tárolási hello virtuális tömb létrehozásakor. 
> 
> 

### <a name="storsimple-device-manager-service"></a>A StorSimple eszköz kezelő szolgáltatás
A Microsoft Azure StorSimple egy webes felhasználói felületet biztosít, amely lehetővé teszi, hogy toocentrally StorSimple Device Manager szolgáltatás hello StorSimple tárterület kezelése. Használhatja a hello StorSimple Device Manager szolgáltatás tooperform hello a következő feladatokat:

* Egyetlen szolgáltatás több StorSimple virtuális tömbök kezelése. 
* Konfigurálása és kezelése a StorSimple virtuális tömbök biztonsági beállításait. (A hello felhő titkosításra a Microsoft Azure API-k függ.)
* Tárfiók hitelesítő adatainak és a tulajdonságok konfigurálása.
* Konfigurálása és kezelése a kötetek vagy megosztások.
* Készítsen biztonsági másolatot, és a kötetek vagy megosztások adatairól.
* Figyelemmel kísérni a teljesítményét.
* Tekintse át a rendszer beállításait, és lehetséges problémák azonosításához.

Hello StorSimple Device Manager szolgáltatás tooperform napi felügyeleti a virtuális tömb használhatja.

További információ: túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Helyi webes felhasználói felülete
hello virtuális tömb magában foglalja a webes felhasználói Felületet, amely egyszeri konfigurálásának és regisztrációjának hello StorSimple Device Manager szolgáltatás hello eszköz szolgál. Akkor le tooshut használni és indítsa újra a hello virtuális tömb, diagnosztikai tesztek futtatása, szoftverek frissítése, hello eszköz rendszergazdai jelszavának módosítása, megtekintheti rendszer naplóit, és forduljon a Microsoft Support toofile szolgáltatási kérelem. 

További információ a webes felhasználói felület hello, nyissa meg túl[használata hello webes felhasználói felület tooadminister a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Parancssori felület
hello szereplő Windows PowerShell-felületet lehetővé teszi egy támogatási munkamenet Microsoft Support tooinitiate így meghatározhatja, hogy és a virtuális tömb előforduló megoldását.

## <a name="storage-management-technologies"></a>Felügyeleti tárolótechnológiák
Ezen felül virtuális tömb toohello és más olyan összetevők, hello StorSimple megoldás által használt hello technológiák tooprovide gyorselérési tooimportant szoftveradatokat a következő, csökkenthető a tároló fogyasztása és a virtuális tömb-on tárolt adatok védelme:

* [Automatikus tárolórétegzés](#automatic-storage-tiering) 
* [Helyileg rögzített megosztásokat és -kötetek](#locally-pinned-shares-and-volumes)
* [A deduplikáció és az adatok tömörítése rétegzett vagy biztonsági másolat toohello felhő](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [Ütemezett és igény szerinti biztonsági mentések tiltása](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatikus tárolórétegzés
hello virtuális tömb hello virtuális tömb és hello felhő egy új rétegezési toomanage tárolt adatokat használ. Nincsenek csak két: hello helyi virtuális tárolótömböt és az Azure felhőbeli tárhelyén. a StorSimple virtuális tömb hello automatikusan hello rétegek hőtérkép, amely nyomon követi az aktuális használati, a korszűrő és a kapcsolatokat tooother adatok alapján történő rendezése a adatok. Adatokat, amelyek a legtöbb aktív (hottest) helyileg van tárolva, miközben kevesebb az aktív és inaktív adatokat a rendszer automatikusan át toohello felhő. (Minden biztonsági mentés hello felhőben vannak tárolva.) StorSimple állítja be, és újrarendezi az adatokat, és a használati minták tárolási hozzárendelését módosítsa. Például bizonyos adatokat válhatnak kevésbé aktív adott idő alatt. Fokozatosan kevésbé aktív válik, mivel azt többszintű toohello felhő ki. Ha ugyanazokat az adatokat ismét aktívvá válik, többszintű toohello tárolótömbhöz.

Egy adott rétegzett fájlmegosztás vagy kötet adatait garantáltan a saját helyi rétegen terület. (körülbelül 10 %-a hello összes kiosztott lemezterület az adott fájlmegosztás vagy kötet). Ez csökkenti a rendelkezésre álló tár hello hello virtuális tömbben az adott fájlmegosztás vagy kötet, miközben biztosítja, hogy egy fájlmegosztás vagy kötet tárolótömbökhöz nem érinti hello rétegezési további kötetek vagy megosztások igényeinek. Így egy fájlmegosztás vagy kötet foglalt terhelése nem kényszerítheti ki más munkaterhelések toohello felhő. 

![Automatikus tárolórétegzés](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> A helyileg rögzített kötet is megadhat, ebben az esetben a hello adatok hello virtuális tömb marad, és soha ne rétegzett toohello felhő. További információ: túl[helyileg rögzített megosztásokat és -kötetek](#locally-pinned-shares-and-volumes).
> 
> 

### <a name="locally-pinned-shares-and-volumes"></a>Helyileg rögzített megosztásokat és -kötetek
Megfelelő megosztások és a helyileg rögzített kötetek is létrehozhat. Ez a funkció biztosítja, hogy a kritikus alkalmazások által megkövetelt adatok hello virtuális tömb marad, és soha ne rétegzett toohello felhő. Helyileg rögzített megosztásokat és -kötetek rendelkezik a következő funkciók hello: 

* Nincsenek tulajdonos toocloud késések vagy kapcsolódási problémák.
* Továbbra is részesülnek StorSimple felhőalapú biztonsági mentés és katasztrófa helyreállítási szolgáltatást.

Visszaállíthatja egy helyileg rögzített vagy, rétegzett kötet vagy egy rétegzett fájlmegosztás vagy kötet, helyileg rögzített. 

A helyileg rögzített kötetekhez kapcsolatos további információkért lásd az túl[hello StorSimple Device Manager szolgáltatás toomanage köteteket](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-toohello-cloud"></a>A deduplikáció és az adatok tömörítése rétegzett vagy biztonsági másolat toohello felhő
StorSimple használja a deduplikáció és az adatok tömörítésének toofurther csökkenti a tárolási követelményeket hello felhőben. A deduplikáció csökkenti hello tárolt hello adatkészlet redundanciájának kiküszöbölése révén tárolt adatok teljes mennyisége. Információk változásával StorSimple figyelmen kívül hagyja változatlanul hello adatok, és csak a hello változtatásokat rögzíti. Emellett a StorSimple csökkenti tárolt adatok mennyisége hello azonosításához, és eltávolítja az ismétlődő adatokat. 

> [!NOTE]
> A virtuális tömb hello tárolt adatokat nem deduplikált vagy tömörített. Minden deduplikációs és tömörítés következik be, mielőtt hello adatküldést toohello felhő.
> 
> 

### <a name="scheduled-and-on-demand-backups"></a>Ütemezett és igény szerinti biztonsági mentések tiltása
StorSimple adatbiztonsági funkciók lehetővé teszik a toocreate igény szerinti biztonsági mentéseket. Emellett egy alapértelmezett biztonsági mentés ütemezése biztosítja, hogy az adatokról napi fel. Biztonsági mentések hello felhőben tárolt növekményes pillanatképek hello formában készül. Csak hello változások rögzítéséhez hello utolsó biztonsági mentés óta, pillanatképeket hozható létre és gyorsan visszaállítva. Ezeket a pillanatképeket különösen fontos a vész-helyreállítási eljárással lehet, mert cserélje le a másodlagos tárterületre rendszerek (például a szalagos biztonsági mentés), és lehetővé teszik adatok tooyour datacenter vagy tooalternate helyek toorestore szükség esetén.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[hello virtuális tömb portal előkészítése](storsimple-virtual-array-deploy1-portal-prep.md).

