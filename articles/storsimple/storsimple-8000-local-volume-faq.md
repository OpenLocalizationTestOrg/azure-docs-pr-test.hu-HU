---
title: "aaaStorSimple helyileg rögzített kötetek – gyakori kérdések |} Microsoft Docs"
description: "Itt választ toofrequently ismételt StorSimple kapcsolatos kérdéseket helyileg rögzített kötetek."
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2017
ms.author: manuaery
ms.openlocfilehash: a3a6557ca15e7e1947b45dcfd005640103c09591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple helyileg rögzített kötetek: gyakori kérdések (GYIK)
## <a name="overview"></a>Áttekintés
Az alábbiakban hello kérdések és válaszok, lehetséges, hogy a StorSimple helyileg rögzített kötet létrehozásakor rétegzett kötet tooa helyileg rögzített kötet konvertálása (és fordítva), vagy biztonsági mentése és visszaállítása egy helyileg rögzített kötet.

Kérdések és válaszok vannak elrendezve a következő kategóriák hello

* Egy helyileg rögzített kötet létrehozása
* Biztonsági mentése egy helyileg rögzített
* A rétegzett kötet tooa helyileg rögzített kötet konvertálása
* Egy helyileg rögzített kötet visszaállítása
* Egy helyileg rögzített kötet feladatátvételét

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Egy helyileg rögzített kötet létrehozásával kapcsolatos kérdések
**K.** Mi az, hogy egy helyileg rögzített kötet, amely képes vagyok létrehozni a hello 8000 sorozat eszközeire hello maximális mérete?

**A** helyileg rögzített kötetekhez mentése too8.5 TB, rétegzett kötetekhez too200 TB hello 8100-as eszközön be megadhat-t a StorSimple 8000 Series Update 3.0 futtató eszközökön. Hello nagyobb 8600 eszköz a helyileg rögzített kötetekhez too22.5 TB mentése vagy mentése too500 TB rétegzett kötetek oszthat.

**K.** A 8100 eszköz tooUpdate 3.0 nemrég frissítette, és, ha egy helyileg rögzített kötet toocreate hello maximális méret pedig csak 6 TB nem 8.5 TB. Miért nem hozható létre egy 8.5 TB-os köteten?

**A** az eszköz fut-e frissítés 3.0, oszthat too8.5 TB mentése helyileg rögzített kötetekhez, vagy fel a too200 TB rétegzett kötetek hello 8100-as eszközön. Ha az eszköz már rendelkezik rétegzett kötet, hello szabad lemezterület egy helyileg rögzített kötet létrehozásához arányosan alacsonyabb, mint a maximális korlát lesz. Például, ha a 8100-as eszközhöz már megadva körülbelül 106 TB, rétegzett kötetekhez (vagyis hello felének rétegzett kapacitás), akkor hello maximális méretét, amelyet létrehozhat hello 8100 eszközön helyi kötet lesz szűkített too4 TB () nagyjából fele hello maximális helyileg rögzített kötet kapacitása).

Mivel helyi helyet a hello eszköz használt toohost hello használata a rétegzett kötetek készlete, ha hello eszköz rendelkezik rétegzett kötet hello szabad terület a helyileg rögzített kötet létrehozásához csökken. Ezzel szemben helyileg rögzített kötet arányosan létrehozása csökkenti a rétegzett kötetek hello rendelkezésre álló terület. a következő táblák hello hello rétegzett elérhető kapacitás a 8100 és 8600 hello eszközökön foglalja össze, helyileg rögzített kötetek létrehozásakor.

#### <a name="update-30"></a>Frissítés 3.0 
| Helyileg rögzített kötetekhez kiosztott kapacitást | Rendelkezésre álló kapacitásból toobe 8100 rétegzett kötetek - kiépítve | Rendelkezésre álló kapacitásból toobe 8600 rétegzett kötetek - kiépítve |
| --- | --- | --- |
| 0 |200 TB |500 TB |
| 1 TB |176.5 TB |477.8 TB |
| 4 TB |105.9 TB |411.1 TB |
| 8.5 TB |0 TB |311.1 TB |
| 10 TB |NA |277.8 TB |
| 15 TB |NA |166.7 TB |
| 22.5 TB |NA |0 TB |

**K.** Miért van helyileg rögzített kötet létrehozása hosszú ideig futó művelet?

**V.** Helyileg rögzített kötetek vannak kiosztása. toocreate lemezterületének hello helyi rétegeken hello eszköz, adatai a meglévő rétegzett kötetek előfordulhat, hogy leküldött toohello felhő hello kiépítési folyamat során. És mivel ez attól függ, hogy hello kötet kiépítése folyamatban, a eszköz- és hello rendelkezésre álló sávszélesség toohello felhőalapú, meglévő adatok hello hello mérete hello idő toocreate egy kötet lehet, hogy több óra is lehet.

**K.** Mennyi ideig tart toocreate egy helyileg rögzített kötet?

**V.** Helyileg rögzített kötet kiosztása van, mert néhány meglévő adatok a rétegzett kötetek előfordulhat, hogy leküldött toohello felhő hello kiépítési folyamat során. Ezért hello igénybe vett idő toocreate egy helyileg rögzített kötet több tényezőtől hello kötet, hello méretétől függ hello adatok az eszközön, és rendelkezésre álló sávszélesség hello. Egy frissen telepített eszköz, amely rendelkezik a kötet hello idő toocreate egy helyileg rögzített kötet az adatok terabájt / körülbelül 10 perc. A helyi kötetek létrehozása azonban olyan eszköz, amely használatban van a fenti hello tényezők alapján több óráig is eltarthat.

**K.** Szeretném toocreate egy helyileg rögzített kötet. Vannak-e be bevált gyakorlatokat, tisztában kell toobe?

**V.** Helyileg rögzített kötetek alkalmasak, amely mindig adatok helyi garanciákat igényelnek és bizalmas toocloud késések fordulnak elő. Figyelembe véve az összes, a munkaterhelés helyi kötetek használatát, vegye figyelembe a következő hello:

* Helyileg rögzített kötet kiosztása, és helyi kötetek létrehozása hatással van a rétegzett kötetek hello rendelkezésre álló terület. Ezért javasoljuk, hogy kisebb méretű kötetek kezdődnie, és a a tárolási követelmény növekedése növelheti.
* A helyi kötetek egy hosszú ideig futó művelet, például előfordulhat, hogy a rétegzett kötetek toohello felhőből kérdez le a meglévő adatokat. Ennek eredményeképpen ezeken a köteteken a csökkentett teljesítményt tapasztalhat.
* A helyi kötetek egy időigényes művelet. érintett hello tényleges idő több tényezőtől függ: hello hello kötet kiépítése folyamatban, az eszközön, és a rendelkezésre álló sávszélesség adatok méretét. Ha nem támogatja a meglévő kötetek toohello felhőt, a kötet létrehozása lassabb. Javasoljuk, hogy a meglévő kötetek felhőalapú pillanatfelvételek igénybe vehet egy kötet kiépítése előtt.
* Átalakíthatja a meglévő rétegzett kötetek toolocally rögzített kötetet, és az átalakításhoz magában foglalja a kiépítés hello az eszközön helyileg rögzített kötet (a hozzáadása toobringing rétegzett-adatok, ha vannak ilyenek, hello felhőből) származó hello terület. Ebben az esetben ez az egy hosszú ideig futó művelet, amely azt korábban már korábban említettük tényezőktől függ. Javasoljuk, hogy biztonsági másolatot a meglévő kötetek előzetes tooconversion, hello folyamat lassabb még akkor is, ha meglévő kötetek nem készül biztonsági mentés lesz. Az eszköz is előfordulhatnak teljesítménycsökkenés a folyamat során.

További információt túl[helyileg rögzített kötet létrehozása](storsimple-8000-manage-volumes-u2.md#add-a-volume)

**K.** Hozható létre több helyileg rögzített kötetekhez: hello ugyanannyi időt vesz igénybe?

**V.** Igen, de a helyileg rögzített kötet létrehozása és bővítése feladat feldolgozása sorrendben történik.

Helyileg rögzített kötet kiosztása, és ez megköveteli, hogy helyi terület hello eszközön (ami a rétegzett kötetek toobe toohello felhő leküldött hello kiépítési folyamat során a meglévő adatok) létrehozását. Ezért ha egy üzembe helyezési feladat van folyamatban, más helyi kötet-létrehozási feladatok, várósorba kerülnek, amíg a feladat nem fejeződött.

Hasonlóképpen ha egy meglévő helyi kötet alatt ki van bontva, vagy a rétegzett kötetek alakít tooa helyileg rögzített kötet, majd hello létrehozása egy új helyileg rögzített kötet van várólistába hello előző feladat befejeződött. Egy helyileg rögzített kötet méretének hello bővülő magában foglalja a hello hello meglévő helyi terület, hogy a kötet kiterjesztését. A rétegzett toolocally rögzített kötet konverzió is magában foglalja a helyileg rögzített kötet eredő hello helyi terület hello létrehozását. Mind a műveletek, a létrehozása vagy a helyi terület bővítése long fut. feladat.

Ezek a feladatok megtekintheti a hello **feladatok** panelen hello StorSimple Device Manager szolgáltatás található. hello aktívan által feldolgozott folyamat folyamatosan frissített tooreflect hello előrehaladását terület kiépítés. helyileg rögzített kötet feladatok fennmaradó hello fel van tüntetve fut, de a folyamat leállt, és azok hello ahhoz, azok sorba állított leltárhoz.

**K.** Törölt egy helyileg rögzített kötet. Miért nem látom visszaigényelt hello terület tükröződnek hello rendelkezésre álló terület toocreate meg egy új kötetet?

**V.** Ha töröl egy helyileg rögzített kötetet, hello szabad lemezterület az új kötetekre nem frissülhet azonnal. hello StorSimple Device Manager szolgáltatás körülbelül óránként frissíti a hello helyi terület érhető el. Javasoljuk, hogy egy órával, mielőtt újból toocreate hello új kötet várja.

**K.** Helyileg rögzített kötetekhez hello felhő készüléken támogatottak?

**V.** Helyileg rögzített kötetekhez hello felhő készülék (8010 és 8020 eszközök korábban hivatkozott tooas hello StorSimple virtuális eszköz) nem támogatottak.

**K.** Használja az Azure PowerShell-parancsmagok toocreate hello és helyileg rögzített kötetek kezelése?

**V.** Nem, nem hozható létre (Azure PowerShell használatával hoz létre kötet Szintezett) Azure PowerShell-parancsmagok segítségével helyileg rögzített kötetekhez. Is javasoljuk, hogy nem használható hello Azure PowerShell-parancsmagok toomodify egy helyileg rögzített kötet bármely tulajdonságait, a rendszer hello kötet típus tootiered módosítása nemkívánatos hatásának hello azt.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Egy helyileg rögzített kötet biztonsági mentésével kapcsolatos kérdések
**K.** Azok a támogatott helyileg rögzített kötetek helyi pillanatképeket?

**V.** Igen, a helyileg rögzített kötetek helyi pillanatképeket is igénybe vehet. Azonban Határozottan javasoljuk, hogy Ön rendszeresen, készítsen biztonsági másolatot a helyileg rögzített kötetek az adatokat a védett felhő pillanatképek tooensure hello egy olyan vészhelyzet esetén lehetőségéről.

Vegye figyelembe, hogy a helyileg rögzített kötetek helyi pillanatképeket is képes réteg kimenő toohello felhő hello helyi réteg hello eszköz toostay nem garantált.

**K.** Vannak-e bármiféle irányelv, amely a helyileg rögzített kötetekhez helyi pillanatképek kezeléséhez?

**V.** Gyakran használják a hello helyileg rögzített kötet lehet, hogy helyi terület az hello eszköz toobe gyorsan felhasznált okozhat, és a az adatokat a rétegzett kötetek alatt leküldött toohello felhő adatforgalommal nagy mértékű mellett helyi pillanatképeket. Ezért javasoljuk, hogy helyi pillanatfelvételek hello számának minimálisra csökkenthető.

**K.** Kaptam arról, hogy a helyi pillanatképeket helyileg rögzített kötetek érvényteleníti előfordulhat, hogy riasztást. Ha ez fordulhat?

**V.** Gyakori mellett a hello helyileg rögzített kötet helyi terület okozhat a hello eszköz toobe gyorsan felhasznált adatforgalommal nagy mértékű helyi pillanatképeket. Hello helyi rétegeken hello eszköz fokozottan használata esetén kiterjesztett felhő kimaradás teljenek hello eszköz eredményezhet, és a bejövő írások toohello kötet előfordulhat hello pillanatképek érvénytelenítési (mert nem lehet szóköz létezik tooupdate hello pillanatképek toorefer toohello régebbi adatblokkok felülírt). Egy ilyen helyzetben hello írások toohello kötet továbbra is toobe kiszolgált, de lehet, hogy a helyi pillanatképeket hello érvénytelen. Nincs nem gyakorolt hatás tooyour meglévő felhőalapú pillanatfelvételek.

hello figyelmeztetést, hogy ez a helyzet akkor merülnek fel, és meg cím hello azonos időben megtekintésével vagy a helyi pillanatképeket ütemezi tootake kevésbé gyakoriak helyi pillanatképekkel vagy törlése régebbi helyi pillanatképeket, amelyeket már nem biztosítására toonotify szükséges.

Ha hello helyi pillanatképeket érvénytelenné válnak, egy értesítés, hogy helyi pillanatképeinek hello hello adott biztonsági mentési házirend érvénytelenítve lett hello helyi pillanatképek volt érvénytelenítve időbélyegek hello listája mellett információ riasztást fog kapni. Ezeket a pillanatképeket a automatikusan törölve lesz, és képes tooview már nem fogja őket a hello **biztonsági mentés katalógusok** panel az Azure-portálon hello.

## <a name="questions-about-converting-a-tiered-volume-tooa-locally-pinned-volume"></a>A rétegzett kötet tooa helyileg rögzített kötet konvertálása kapcsolatos kérdések
**K.** Vagyok, ha az egyes lassúsága hello eszközön egy rétegzett kötet tooa helyileg rögzített kötet konvertálása során tartják be. Miért van ennek az oka?

**V.** hello átalakítási folyamat két lépésből áll:

1. Kiépítés terület hello hello az eszközön helyileg rögzített hamarosan-az--alakítható kötet.
2. Rétegzett adatok letöltése a helyi hello felhő tooensure biztosítja.

Ezek hosszúak konvertált hello eszközön, és a rendelkezésre álló sávszélesség adatok hello kötet méretének hello függő műveletek futtatása. A meglévő rétegzett kötetek adatai előfordulhat, hogy kerülnek toohello felhő hello kiépítési folyamat részeként, mivel az eszköz találkozhatnak teljesítménycsökkenés ebben az időszakban. Ezenkívül hello átalakítási folyamat lassabb lehet, ha:

* Meglévő kötetek nem mentett toohello felhő; ezért javasoljuk, hogy a kötetek előzetes tooinitiating konverzió biztonsági mentését.
* Sávszélesség-szabályozási házirendeket alkalmaztak, ami korlátozhatja hello rendelkezésre álló sávszélesség toohello felhő; ezért javasoljuk, hogy egy dedikált 40 MB/s vagy több kapcsolat toohello felhő.
* hello átalakítási folyamat a fenti; toohello több tényezők miatt több órát is igénybe vehet ezért javasoljuk, hogy a művelet esetén nem csúcsait vagy a hétvégi tooavoid végrehajtása hello end fogyasztók hatással.

További információt túl[rétegzett kötet tooa helyileg rögzített kötet konvertálása](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)

**K.** Visszavonhatja a hello kötet átalakítási műveletet?

**V.** Nem, nem hello Mégse hello átalakítási műveletet egyszer kezdeményezett. A hello előző kérdéssel bemutatott, vegye figyelembe a hello lehetséges teljesítményproblémák, hogy előfordulhat, hogy hello folyamat során előforduló, és kövesse a bevált gyakorlatokat hello fent felsorolt, ha azt tervezi, hogy az átalakításhoz.

**K.** Toomy kötet mi történik, ha sikertelen hello átalakítási művelet?

**V.** Kötet átalakítás toocloud csatlakozási problémák miatt sikertelen lehet. hello eszköz végül leállíthatja hello átalakítási folyamat után a sikertelen kísérletek toobring rétegzett hello felhő-adatok sorozata. Ilyen esetben hello kötettípus továbbra is toobe hello forrás kötet típus előzetes tooconversion, és:

* Kritikus riasztás emelt toonotify lesz a hello kötet típuskonverziós hiba. További információ a [kapcsolódó toolocally rögzített kötetek riasztások](storsimple-8000-manage-alerts.md#locally-pinned-volume-alerts)
* Ha egy rétegzett tooa helyileg rögzített kötetet, hello kötet mindaddig rétegzett kötet tooexhibit tulajdonságainak adatok továbbra is előfordulhatnak hello felhő. Javasoljuk, hogy hello kapcsolódási problémák megoldásához, és ismételje meg hello átalakítási műveletet.
* Ehhez hasonlóan egy helyileg rögzített tooa konverzió rétegzett kötet meghiúsul, bár egy helyileg rögzített kötet hello kötet lesz megjelölve, ha működik a rétegzett kötet (mivel az adatok toohello felhő kiömlött sikerült rendelkezik). Azonban továbbra is toooccupy terület hello helyi rétegen hello eszköz. Ezen a helyen nem lesz elérhető az egyéb helyileg rögzített kötetek esetében. Javasoljuk, hogy ez a művelet tooensure hello kötet konvertálása befejeződött, és hello eszközön hello a helyi terület szabadítható fel újra.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Egy helyileg rögzített kötet visszaállításával kapcsolatos gyakori kérdésekre
**K.** Azonnal vissza helyileg rögzített kötetek?

**V.** Igen, azonnal visszaállnak helyileg rögzített kötetekhez. Amint hello metaadat-információi hello kötet lekért hello felhő hello visszaállítási művelet részeként, hello kötet online állapotba kerül, és hello gazdagép által elérhető. Helyi garanciákat hello kötet adatok nem lesznek megtalálhatók, amíg minden hello adat hello felhőből le van töltve, és problémákat tapasztalhat azonban hello ideje alatt hello visszaállítási ezeken a köteteken teljesítmény csökkenteni.

**K.** Mennyi ideig tart toorestore egy helyileg rögzített kötet?

**V.** Helyileg rögzített kötetek azonnal vissza és a hálózatra, amint hello kötet metaadatait lekért hello felhőbe, miközben hello kötetadatokról tovább toobe a hello háttérben tölti. Ez utóbbi részét hello visszaállítási művelet--beolvasása vissza hello helyi garanciákat hello kötet--hosszú ideig futó művelet, és az összes hello adatok toobe végzett helyi újra több órát is igénybe vehet. hello igénybe vett idő toocomplete hello azonos több tényezőtől függ, többek között visszaállítandó hello kötet méretének hello és hello rendelkezésre álló sávszélességet. Törölve lett a hello eredeti kötet visszaállítása folyamatban van, további időt toocreate hello helyi terület hello eszközön hello visszaállítási művelet részeként kell venni.

**K.** A meglévő helyileg rögzített tooan régebbi pillanatkép a kötetről (hajt végre, amikor hello kötet volt Szintezett) toorestore kell. Hello kötet visszaállnak, ebben az esetben rétegzett?

**V.** Nem, a hello kötet visszaáll egy helyileg rögzített kötet. Bár hello pillanatkép-dátumok toohello idő, amikor hello kötet volt rétegzett, meglévő kötetek visszaállítása közben StorSimple mindig kötet hello típusú hello lemezen használja, mert jelenleg létezik.

**K.** Nemrég bővítve I a helyileg rögzített kötetet, de most kell toorestore hello adatok tooa időpontja hello kötet mérete kisebb. Átméretezi visszaállítási hello aktuális kötet és fog van szükség tooextend hello hello kötet méretének hello visszaállítás befejezése után?

**V.** Igen, hello visszaállítási átméretezési hello kötet, és szüksége lesz tooextend hello hello kötet méretének hello visszaállítás befejezése után.

**K.** Módosítható visszaállítás során egy kötet hello típusú?

**A.**nem, nem módosíthatja hello kötettípus visszaállítás során.

* Törölt kötetek visszaállnak hello pillanatfelvétel tárolt hello típusként.
* Meglévő kötetek visszaállítása a jelenlegi típus függetlenül hello pillanatfelvétel tárolt hello típusa alapján (lásd az előző két toohello kérdések).

**K.** A helyileg rögzített kötetet kell toorestore, de egy helytelen pont I mennyi idő pillanatfelvétel. Hello visszaállításhoz is megszakítja?

**V.** Igen, megszakíthatja a folyamatban lévő helyreállítási művelet. hello kötet hello állapotának vissza lesz állítva toohello állapot hello visszaállítási hello elején. Azonban bármely toohello kötet hello visszaállítása közben végzett írási műveletek elvesznek.

**K.** A visszaállítási művelet a helyileg rögzített kötetekhez egyik kezdődik, és most a várakozó-katalógus létrehozása recollect I nem látom pillanatképet. Ez alkalmazott?

**V.** Ez a hello ideiglenes pillanatképet, amely előzetes toohello visszaállítási művelet jön létre, és a visszaállításhoz használt abban az esetben hello visszaállítási megszakadt, vagy nem sikerül. Ne törölje a pillanatkép; akkor lesz automatikusan törlődik hello visszaállítás befejeződött. Ez akkor fordulhat elő, ha a visszaállítási feladat csak a helyi számítógépre van rögzítve, kötetek vagy a helyileg rögzített és a rétegzett kötetek kombinációját. Ha hello visszaállítási feladat csak a rétegzett kötetek tartalmazza, majd ezt a viselkedést nem történik.

**K.** Egy helyileg rögzített kötetet tud klónozni?

**V.** Igen, akkor is. Azonban hello helyileg rögzített kötet lesz klónozható egy rétegzett kötet alapértelmezés szerint. További információt túl[egy helyileg rögzített kötet klónozása](storsimple-8000-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Egy helyileg rögzített kötet feladatátvételét kapcsolatos kérdések
**K.** Az eszköz tooanother fizikai eszköz keresztül kell toofail. A helyileg rögzített kötetekhez sikertelen lesz, a helyileg rögzített vagy rétegzett több mint?

**V.** hello helyileg rögzített kötetek feladatátvétel történt a helyileg rögzített Ha hello céleszköz fut a StorSimple 8000 series update 3-as vagy magasabb.

További információ a [feladatátvétel és a vész-Helyreállítási helyileg rögzített kötetek verziója között](storsimple-8000-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**K.** Helyileg rögzített kötetekhez azonnal visszaállnak vészhelyreállítás (DR) során?

**V.** Igen, a helyileg rögzített kötetek visszaállítása azonnal a feladatátvétel során. Amint hello metaadat-információi hello kötet lekért hello felhő hello feladatátvételi művelet részeként, hello kötet hello céleszközön online állapotba, és hello gazdagép által elérhető. Eközben hello kötet adatok továbbra is toodownload hello háttérben, és ezeken a köteteken csökkentett teljesítmény hello feladatátvételi hello időtartamára.

**K.** Hello feladatátvételi feladata befejezve látható, hogyan követhető nyomon a helyileg rögzített kötetet, amelyek helyreállítása hello előrehaladását hello céleszközön?

**V.** Feladatátvételi művelet során hello feladatátvételi feladatban befejezettként, miután hello feladatátvételi készlet összes hello kötetek rendelkezik azonnal visszaállítva és hello céleszközön online állapotba. Ez magában foglalja a helyileg rögzített kötetekhez keresztül; sikertelen lehet, hogy azonban helyi garanciákat hello adatok csak akkor érhető el hello kötet összes adatát hello letöltésekor. A nyomon minden helyileg rögzített kötet, amely át hello megfelelő visszaállítási feladatok hello feladatátvételi részeként létrehozott figyelése nem volt. Ezek a feladatok egyes visszaállítási csak a helyileg rögzített kötetekhez hozhatók létre.

**K.** Módosítható a feladatátvétel során egy kötet hello típusú?

**V.** Nem, a feladatátvétel során hello kötet típusa nem módosítható. Ha Ön nem működnek keresztül tooanother fizikai eszköz, amely a StorSimple 8000 series fut. 3, hello kötetek feladatátvétel történt a tárolt hello pillanatfelvétel hello kötettípus alapján.

**K.** I átveheti a kötettároló a helyileg rögzített kötetekhez toohello felhő készülék?

**V.** Igen, akkor is. hello helyileg rögzített kötetek a rétegzett kötetek átvétele fog megtörténni. További információ a [feladatátvétel és a vész-Helyreállítási helyileg rögzített kötetek verziója között](storsimple-8000-device-failover-disaster-recovery.md#common-considerations-for-device-failover)

