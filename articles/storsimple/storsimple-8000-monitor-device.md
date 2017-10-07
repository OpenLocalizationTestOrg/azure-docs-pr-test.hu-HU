---
title: "aaaMonitor a StorSimple 8000 series eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toomonitor használati, i/o-teljesítmény és kapacitás kihasználtságát."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 092dab8dd301c50fc12316b4031a8d1b34fab876
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomonitor-your-storsimple-device"></a>Hello StorSimple Device Manager szolgáltatás toomonitor a StorSimple eszköz használata
## <a name="overview"></a>Áttekintés
A StorSimple megoldásban belül hello StorSimple Device Manager szolgáltatás toomonitor konkrét eszközökhöz használható. Hozzon létre egyéni diagramok i/o teljesítmény, tárolókapacitás használatát, hálózati teljesítményt és eszköz teljesítménymutatók alapján, és ezek toohello irányítópulton rögzítheti. További információ: túl[testre szabhatja a portál irányítópultján](/articles/azure-portal/azure-portal-dashboards.md).

tooview hello megfigyelési információk egy adott eszközre, a hello Azure-portálon, válassza ki a hello StorSimple Device Manager szolgáltatást. Az eszközök hello listából válassza ki azt az eszközt, és folytassa a túl**figyelő**. Láthatja majd hello **kapacitás**, **használati**, és **teljesítmény** diagramok hello kijelölt eszközre vonatkozóan.

## <a name="capacity"></a>Kapacitás
**Kapacitás** számok hello kiépített és hello hello eszközön rendelkezésre. fennmaradó kapacitás hello majd jelenik meg helyileg rögzített vagy rétegzett.

kiépített hello és a szabad kapacitás további bontják rétegzett és helyileg rögzített kötetek. Az egyes kötetek hello kapacitás és kapacitás hello eszközön fennmaradó hello ez megjelenik.

![IO-kapacitás](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Használat
**Használati** számok metrikák kapcsolódó toohello hello kötetek, a kötettárolók vagy az eszköz által használt adatok tárhely mennyiségét. Hello tárolókapacitás kihasználtságát elsődleges tárhelyét, a felhőalapú tárolást és az eszköz tárolóját alapján jelentéseket is létrehozhat. Kapacitáskihasználás egy adott köteten, egy adott kötettároló vagy az összes kötet tároló mérhető.
Alapértelmezés szerint az elmúlt 24 órára hello használati igen. Hello diagram toochange hello időtartama keresztül mely hello használati való kiválasztással jelentett módosíthatja:
* Elmúlt 24 órában
* Elmúlt 7 nap
* Elmúlt 30 nap
* Elmúlt 90 nap
* Elmúlt év


hello elsődleges, a felhő és a helyi tároló használt jelentheti az alábbiak szerint:

### <a name="primary-storage-usage"></a>Elsődleges tárhely kihasználtsága
A diagramokat hello adatmennyiség tooStorSimple kötetek írása előtt hello adatok deduplikációja és tömörített megjelenítése. Hello kötettároló, vagy csak egy kötet összes kötet által használt elsődleges tárhely tekintheti meg. hello elsődleges tárolási használt elsődleges rétegzett tárolás felhasznált és elsődleges helyileg rögzített tárolót használja a további oszlanak meg.

hello következő diagramok megjelenítése hello elsődleges tárolási a StorSimple eszköz előtt és után egy felhő-pillanatfelvételt készült használnak. Ez ugyanis csak a kötet adatait, egy felhő-pillanatfelvételt nem kell módosítani a hello elsődleges tárolási. Ahogy látja, hello diagram ábrázolja hello elsődleges rétegzett vagy helyileg rögzített használt tároló felhő pillanatképének miatt nincs különbség. hello felhőalapú pillanatfelvétel lépések du. körülbelül 11:50 azokon az eszközökön.

![Felhő pillanatkép utáni elsődleges kapacitáskihasználás](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Ha most futtatja IO hello csatlakozó állomás tooyour StorSimple eszközön, látni fogja az elsődleges rétegzett tárolás növekedése vagy elsődleges helyileg rögzített használt tároló függően (Szintezett vagy helyileg rögzített) kötetek esetén írhat hello adatokat. Az alábbiakban hello elsődleges tárolási használati diagramok a StorSimple eszköz. Ezen az eszközön hello StorSimple gazdagépen elindította a szolgáltató írása, körülbelül 2:30 pm a rétegzett kötetek hello eszközön. Hello csúcs hello írási bájt/s hello állomáson futó toohello IO megfelelő tekintheti meg.

![Ha a futó IO rétegzett kötet teljesítmény](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Ha megnézzük hello elsődleges rétegelt tárolás használt, amely másolatot állapotba került, mivel az elsődleges hello helyileg rögzített használati változatlan marad, mert nincsenek nem írt kiszolgált toohello helyileg rögzített kötetek hello eszközön.

![Elsődleges tárolókapacitás kihasználtságát IO a rétegzett kötetek futtatásakor](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Ha futtatja 3. frissítéssel vagy újabb rendszerre, akkor is hello elsődleges tárolókapacitás kihasználtságát szerint lebontva egy egyéni köteten, az összes kötegről, minden rétegzett kötetek és minden helyileg rögzített kötetekhez alább látható módon. Az összes helyileg rögzített kötetekhez lehetővé teszi a bontásához tooquickly megállapítása, hány hello helyi rétegen használ.

![Az összes rétegzett kötet elsődleges kapacitáskihasználás](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Az összes helyileg rögzített kötet elsődleges kapacitáskihasználás](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Egyes hello kötetek hello listában kattintson a Tovább, és hello megfelelő használati.

![Az összes helyileg rögzített kötet elsődleges kapacitáskihasználás](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>A felhőalapú tároló használata
A diagramokat megjelenítése hello használt felhőbeli tárhely mennyiségét. Ezek az adatok deduplikációja és tömöríti. Ez a mennyiség magában foglalja a felhőalapú pillanatfelvételek, amelyek esetleg nem tükröződnek elsődleges kötet és az örökölt vagy szükséges megőrzési célra maradnak adatokat tartalmaz. Összehasonlíthatja hello elsődleges és a felhő tárolási felhasználási adatok tooget hello adatok csökkentési képet értékelje, annak ellenére hello száma nem pontos.

hello következő diagramok megjelenítése hello felhőbeli tárhely kihasználtságát a StorSimple eszköz egy felhő-pillanatfelvételt időpontjában volt.

* időpontban elindított hello felhőalapú pillanatfelvétel körülbelül 11:50-kor az eszközön, és láthatja, hogy a felhő-pillanatfelvételt hello előtt merült fel nem használt felhőbeli tárhelyhez. 
* Egyszer hello felhőalapú pillanatfelvétel befejeződött, hello felhőbeli tárhely kihasználtságát változásszinkronizálás 0,89 GB fel. 
* Miközben folyamatban volt hello felhőalapú pillanatfelvétel, nincs is megfelelő maximális hello IO eszköz toocloud a.

    ![Felhőbeli tárhely kihasználtságát a felhő-pillanatfelvételt előtt](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Felhőbeli tárhely kihasználtságát felhő pillanatkép utáni](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![Eszköz toocloud során egy felhő-pillanatfelvételt tevékenységének](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Helyi tárterület-használat
Diagramokat jelenít meg hello teljes kihasználtság hello eszköz, amely lesz több mint elsődleges tárhelyhasználatot mert hello SSD-lineáris réteget tartalmaz. A réteg tartalmaz egy is megtalálható hello eszköz más szintekre adatok mennyiségét. hello hello SSD-lineáris réteg kapacitása nem szakad meg, úgy, hogy ha új adatok, hello régi adatok áthelyezett toohello HDD-réteg (amikor deduplikált és tömörített) és ezt követően toohello felhő.

Az idő múlásával felhasznált és helyi tárhelyet valószínűleg megnöveli a együtt amíg hello adatok kezdete toobe elsődleges tárolási rétegzett toohello felhő. Ezen a ponton hello használt helyi tárhely valószínűleg akkor kezdődik, tooplateau, de a használt elsődleges tároló hello növeli, több adatot ír.

hello következő diagramok megjelenítése hello elsődleges tárolási a StorSimple eszköz használhatók, ha egy felhő-pillanatfelvételt kerül. hello felhőalapú pillanatfelvétel: 00:50 óra és az hello helyi tároló ekkor csökken. a 1.445 GB too1.09 GB csökkent hello helyi tárhelyet. Ez azt jelzi, hogy nagy valószínűséggel hello hello lineáris SSD-réteget a tömörítetlen adatokhoz lett deduplikált, tömörített, hello HDD-réteget áthelyezése. Vegye figyelembe, hogy hello eszköz már egy nagy mennyiségű adatot hello SSD és HDD-réteg is, nem jelenhet meg ezen csökken. Ebben a példában hello eszközhöz tartozik egy kisebb mennyiségű adatot.

![Felhő pillanatképet készít a helyi tárhely kihasználtságát](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Teljesítmény
**Teljesítmény** számok metrikák olvasási toohello száma és a kapcsolódó írási műveletek között vagy hello iSCSI kezdeményező felületek hello gazdagép-kiszolgálón és a hello eszközön vagy a hello eszköz és a hello felhő. A teljesítmény mérhető egy adott köteten, egy adott kötettároló vagy az összes kötet tároló. Teljesítmény is CPU-használat és a hálózat adatátviteli sebességét hello különböző hálózati adapterek az eszközön.

### <a name="io-performance-for-initiator-toodevice"></a>A kezdeményező toodevice i/o-teljesítmény
az alábbi hello diagram hello i/o-hello kezdeményező tooyour eszköz az összes hello kötet egy éles eszköz jeleníti meg. hello metrikák ábrázolt vannak írási és olvasási bájt / mp. Is diagram olvasási, írási és függőben lévő I/O vagy olvasási és írási késések fordulnak elő.

![A kezdeményező toodevice I/O teljesítménye](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-toocloud"></a>Az eszköz toocloud i/o-teljesítmény
A hello ugyanarra az eszközre hello i/o-műveletek ábrázolási hello eszköz toohello felhőből hello adatokhoz tartozó összes hello kötettárolók. Ezen az eszközön hello adatok csak hello lineáris szinten lévő és nem rendelkezik kiömlött toohello felhő. Nincsenek eszköz toohello felhőből előforduló olvasási és írási műveletek. Emiatt hello csúcsait hello diagramon egy 5 perces időközönként megfelelő toohello gyakoriság, amellyel a hello szívverés be van jelölve hello eszköz és hello szolgáltatás között lehet.

A hello ugyanarra az eszközre egy felhő-pillanatfelvételt kerül a kötet adatait: 00:50 óra indítása. Ennek következtében a hello eszköz toohello felhőből áramló adatokat. Írás ennek az időtartamnak a volt kiszolgált toohello felhő. hello IO diagram csúcsot hello írási bájt/s megfelelő toohello időben amikor hello pillanatkép készült jeleníti meg.

![Eszköz toocloud során egy felhő-pillanatfelvételt tevékenységének](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Az eszköz hálózati illesztők tartozó hálózati teljesítmény
**Hálózati teljesítmény** számok metrikák kapcsolódó toohello továbbított adatmennyiséget hello iSCSI kezdeményező hálózati interfész hello gazdagép-kiszolgálón és hello eszköz és hello eszköz és hello felhő között. Ez a mérőszám az egyes hello iSCSI hálózati adapterek figyelheti az eszközön.

hello következő diagramok megjelenítése hello hálózati teljesítményt hello Data 0, 1 1 GbE hálózati az eszközön, amely mindkét felhő engedélyezve (alapértelmezett) és az iSCSI-kompatibilis. Ezen az eszközön. június 14-körülbelül 9 du. az adatok hello cloud (pillanatképeket létező felhőalapú, ebben az időszakban, mely pontok tootiering alatt hello toomove hello adatokat hello felhőbe) kiszolgált toohello felhő IO eredményező be lett rétegzett. Nincs megfelelő csúcs hello hálózati átviteli kijelző hello ugyanannyi időt vesz igénybe, és a legtöbb hello hálózati forgalom érték kimenő toohello felhő.

![A Data 0 hálózati teljesítményt](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Ha úgy tekintünk hello Data 1 felület átviteli diagram, egy másik 1 GbE hálózati adapter, amely volt az iSCSI-kompatibilis csak, majd gyakorlatilag hálózati forgalom történt ennek az időtartamnak a.

![Az adatok 1 tartozó hálózati teljesítmény](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>Az eszköz CPU-felhasználás
**CPU-felhasználás** számok metrikák kapcsolódó toohello CPU-használata az eszközön. hello következő diagram ábrázolja hello Processzor kihasználtsága statisztikák eszköz éles környezetben.

![Az eszköz CPU-felhasználás](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hello StorSimple Device Manager szolgáltatás eszközök irányítópulttal](storsimple-device-dashboard.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

