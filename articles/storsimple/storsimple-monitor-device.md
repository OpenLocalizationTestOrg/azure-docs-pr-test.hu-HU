---
title: "aaaMonitor a StorSimple eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás toomonitor i/o-teljesítményét, tárolókapacitás használatát, hálózati teljesítményt és eszköz teljesítményét."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: c1f614a7f52728650bfadb3335435b8b5a17e6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomonitor-your-storsimple-device"></a>Hello StorSimple Manager szolgáltatás toomonitor a StorSimple eszköz használata
## <a name="overview"></a>Áttekintés
A StorSimple megoldásban belül hello StorSimple Manager szolgáltatás toomonitor konkrét eszközökhöz használható. I/o teljesítmény, tárolókapacitás használatát, hálózati teljesítményt és eszköz teljesítménymutatók alapján egyéni diagramokat hozhat létre. 

tooview hello monitoring információk egy adott eszközhöz hello klasszikus Azure portálon válassza hello StorSimple Manager szolgáltatás. Kattintson a hello **figyelő** lapot, és majd válassza ki az eszközök hello listája. Hello **figyelő** lap tartalmaz a következő információ hello.

## <a name="io-performance"></a>I/o-teljesítmény
**I/o-teljesítmény** számok metrikák olvasási toohello száma és a kapcsolódó írási műveletek között vagy hello iSCSI kezdeményező felületek hello gazdagép-kiszolgálón és a hello eszközön vagy a hello eszköz és a hello felhő. A teljesítmény mérhető egy adott köteten, egy adott kötettároló vagy az összes kötet tároló.

az alábbi hello diagram hello i/o-hello kezdeményező tooyour eszköz az összes hello kötet egy éles eszköz jeleníti meg. hello metrikák ábrázolt olvassa el és írási bájt / mp, olvasási és írási I/O műveletek másodpercenkénti számát, és olvasási és írási késések fordulnak elő.

![A kezdeményező toodevice I/O teljesítménye](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

A hello ugyanarra az eszközre hello i/o-műveletek ábrázolási hello eszköz toohello felhőből hello adatokhoz tartozó összes hello kötettárolók. Ezen az eszközön hello adatok csak hello lineáris szinten lévő és nem rendelkezik kiömlött toohello felhő. Nincsenek eszköz toohello felhőből előforduló olvasási és írási műveletek. Emiatt hello csúcsait hello diagramon egy 5 perces időközönként megfelelő toohello gyakoriság, amellyel a hello szívverés be van jelölve hello eszköz és hello szolgáltatás között lehet. 

![Az eszköz toocloud I/O teljesítménye](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

A hello ugyanarra az eszközre egy felhő-pillanatfelvételt készült 2:00 pm kezdődő kötet adatok. Ennek következtében a hello eszköz toohello felhőből áramló adatokat. Olvasási / írási műveletek ennek az időtartamnak a volt kiszolgált toohello felhő. hello IO diagram megjeleníti a legmagasabb hello megfelelő különböző metrikák toohello idő, amikor hello pillanatkép készült. 

![Felhő pillanatképet készít a eszköz toocloud I/O teljesítménye](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Kapacitáskihasználás
**Kapacitáskihasználás** számok metrikák kapcsolódó toohello hello kötetek, a kötettárolók vagy az eszköz által használt adatok tárhely mennyiségét. Hello tárolókapacitás kihasználtságát elsődleges tárhelyét, a felhőalapú tárolást és az eszköz tárolóját alapján jelentéseket is létrehozhat. Kapacitáskihasználás egy adott köteten, egy adott kötettároló vagy az összes kötet tároló mérhető.

hello elsődleges, a felhő és a tárolási kapacitás eszköz jelentheti az alábbiak szerint:

### <a name="primary-storage-capacity-utilization"></a>Elsődleges tárolókapacitás kihasználtságát
A diagramokat hello adatmennyiség tooStorSimple kötetek írása előtt hello adatok deduplikációja és tömörített megjelenítése. Minden kötetet vagy csak egy kötet hello elsődleges tárhely kihasználtságát tekintheti meg.

Ilyen esetekben hello elsődleges tárolási kötet kapacitás kihasználtságát diagramokat az összes kötet, és minden egyes hello egyes köteteket és sum hello elsődleges adatok megtekintésekor lehet eltérő hello két szám közé. összes kötet elsődleges adatok teljes hello nem vehet fel toohello összegeként hello elsődleges adatok hello egyéni kötetek. Ennek oka lehet hello következő tooone:

* **Pillanatkép-adatok tartalmazza az összes kötet**: Ez a viselkedés látható csak akkor, ha a frissítés 3-nál korábbi verzióját futtatja. az összes hello kötet látható hello elsődleges adatok értéke hello elsődleges minden olyan kötetre és hello pillanatkép adatainak hello összege. egy adott kötet látható hello elsődleges adatok tooonly hello adatmennyiség hello köteten lefoglalt megfelel-e (és nem tartalmaz megfelelő mennyiségi pillanatképadatok hello).
  
    Ez is viszonylag által a következő egyenlet hello:
  
    *Elsődleges data (az összes kötegről) = összege (elsődleges adatok (kötet i) + (kötet i) pillanatkép-adatok mérete)*
  
    *Ha az elsődleges adatok (kötet i) lefoglalt elsődleges adatok toovolume mérete = i*
  
    Hello pillanatképek hello szolgáltatáson keresztül törlődnek, ha hello törlés hello háttérben aszinkron módon végezhető el. Hello kötet adatok mérete toobe hello pillanatkép törlését követően frissítése némi időbe telhet. 
  
    Ha fut a frissítés, 3-as vagy újabb verzióját, majd hello pillanatkép-adatok nem szerepel hello kötet adatait. És hello elsődleges kihasználtsági kiszámítása a következőképpen történik:
  
    * Elsődleges data (az összes kötegről) = (elsődleges adatok (kötet i) összege
  
    *Ha az elsődleges adatok (kötet i) lefoglalt elsődleges adatok toovolume mérete = i*
* **Kötetek figyelési le van tiltva az összes kötet**: Ha kötetek az eszközön, amelynek figyelési ki van kapcsolva, figyelési adatok ezek egyéni kötetek hello nem lesz elérhető a hello diagramon. Azonban az összes kötet hello diagram hello adatok tartalmaz hello kötetek, amelynek figyelési ki van kapcsolva. 
* **Tartalmazza az összes kötet élő társított biztonsági másolatok törlődik a kötetek**: Ha pillanatkép-adatokat tartalmazó kötetek törlődnek, de kapcsolódó hello pillanatképek továbbra is, az eltérés jelenhetnek meg.
* **Törli az összes kötet kötetek**: bizonyos esetekben régebbi kötet előfordulhat, hogy létezik, annak ellenére, hogy ezek törölték. törlés hello hatásának nem látható, és hello eszköz előfordulhat, hogy megjelenítése alacsonyabb rendelkezésre álló kapacitásból. Toocontact Microsoft Support tooremove kell ezeket a köteteket.

hello következő diagramok megjelenítése hello elsődleges tárolókapacitás kihasználtságát a StorSimple eszköz előtt és után egy felhő-pillanatfelvételt kerül. Ez ugyanis csak a kötet adatait, egy felhő-pillanatfelvételt nem kell módosítani a hello elsődleges tárolási. Ahogy látja, hello diagram ábrázolja hello elsődleges tárolókapacitás kihasználtságát felhő pillanatképének miatt nincs különbség. hello felhőalapú pillanatfelvétel kezdete körülbelül 2:00 pm azokon az eszközökön.

![Elsődleges tárolókapacitás kihasználtságát felhőalapú pillanatfelvétel előtt](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Felhő pillanatkép utáni elsődleges kapacitáskihasználás](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Ha futtatja Update 2 vagy újabb rendszerre, akkor is hello elsődleges tárolókapacitás kihasználtságát szerint lebontva egy egyéni köteten, az összes kötegről, minden rétegzett kötetek és minden helyileg rögzített kötetekhez alább látható módon. Az összes helyileg rögzített kötetekhez lehetővé teszi a bontásához tooquickly megállapítása, hány hello helyi rétegen használ.

![Az összes helyileg rögzített kötet elsődleges kapacitáskihasználás](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Felhő tárolókapacitás kihasználtságát
A diagramokat megjelenítése hello használt felhőbeli tárhely mennyiségét. Ezek az adatok deduplikációja és tömöríti. Ez a mennyiség magában foglalja a felhőalapú pillanatfelvételek, amelyek esetleg nem tükröződnek elsődleges kötet és az örökölt vagy szükséges megőrzési célra maradnak adatokat tartalmaz. Összehasonlíthatja hello elsődleges és a felhő tárolási felhasználási adatok tooget hello adatok csökkentési képet értékelje, annak ellenére hello száma nem pontos. hello következő diagramok megjelenítése hello felhő tárolókapacitás kihasználtságát a StorSimple eszköz előtt és után egy felhő-pillanatfelvételt kerül. hello felhőalapú pillanatfelvétel időpontban elindított körülbelül 2:00 pm azokon az eszközökön, és láthatja a hello felhő tárolókapacitás kihasználtságát változásszinkronizálás: hello azonos idő, a 5.73 MB too4.04 GB-os növelését.

![Felhő tárolókapacitás kihasználtságát felhőalapú pillanatfelvétel előtt](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![A felhő felhő pillanatképet készít a tárolókapacitás kihasználtságát](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Eszköz tárolókapacitás kihasználtságát
Diagramokat jelenít meg hello teljes kihasználtság hello eszköz, amely lesz több mint elsődleges tárhely kihasználtságát mert hello SSD-lineáris réteget tartalmaz. A réteg tartalmaz egy is megtalálható hello eszköz más szintekre adatok mennyiségét. hello hello SSD-lineáris réteg kapacitása nem szakad meg, úgy, hogy ha új adatok, hello régi adatok áthelyezett toohello HDD-réteg (amikor deduplikált és tömörített) és ezt követően toohello felhő.

Idővel elsődleges tárolókapacitás kihasználtságát és a tárolókapacitás kihasználtságát eszköz valószínűleg megnöveli a együtt amíg hello adatok kezdete toobe rétegzett toohello felhő. Ezen a ponton hello eszköz tárolókapacitás kihasználtságát valószínűleg akkor kezdődik, tooplateau, de hello elsődleges kapacitás nagyobb lesz, több adatot ír.

hello következő diagramok megjelenítése hello elsődleges tárolókapacitás kihasználtságát a StorSimple eszköz előtt és után egy felhő-pillanatfelvételt kerül. hello felhőalapú pillanatfelvétel, 2:00 pm és az hello eszköz tárolókapacitás kihasználtságát ekkor csökken. a 11.58 GB too7.48 GB csökkent hello eszköz tárolókapacitás kihasználtságát. Ez azt jelzi, hogy nagy valószínűséggel hello hello lineáris SSD-réteget a tömörítetlen adatokhoz lett deduplikált, tömörített, hello HDD-réteget áthelyezése. Vegye figyelembe, hogy hello eszköz már egy nagy mennyiségű adatot hello SSD és HDD-réteg is, nem jelenhet meg ezen csökken. Ebben a példában hello eszközhöz tartozik egy kisebb mennyiségű adatot.

![Eszköz tárolókapacitás kihasználtságát felhőalapú pillanatfelvétel előtt](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Eszköz tárolókapacitás kihasználtságát felhő pillanatkép utáni](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Hálózati teljesítmény
**Hálózati teljesítmény** számok metrikák kapcsolódó toohello továbbított adatmennyiséget hello iSCSI kezdeményező hálózati interfész hello gazdagép-kiszolgálón és hello eszköz és hello eszköz és hello felhő között. Ez a mérőszám az egyes hello iSCSI hálózati adapterek figyelheti az eszközön.

a következő diagram megjelenítése hello hálózati teljesítményt hello Data 0 és 4 adatok, az eszköz két 1 GbE hálózati adapterek hello. Ebben az esetben Data 0 volt felhő-engedélyezve van, mivel az adatok 4 volt az iSCSI-kompatibilis. Mindkét hello bejövő és kimenő forgalmát a StorSimple eszköz hello tekintheti meg. hello strukturálatlan sor du. 3:24-től kezdődő hello diagram van miatt toohello tényt, hogy azt csak 5 percenként adatokat gyűjt, és figyelmen kívül lesz hagyva. 

![Az Data4 tartozó hálózati teljesítmény](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Az Data4 tartozó hálózati teljesítmény](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Eszköz teljesítmény
**Eszköz teljesítményének** számok metrikák kapcsolódó az eszköz toohello teljesítményét. hello következő diagram ábrázolja hello Processzor kihasználtsága statisztikák eszköz éles környezetben.

![Az eszköz CPU-felhasználás](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hello StorSimple Manager szolgáltatás eszközök irányítópulttal](storsimple-device-dashboard.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

