---
title: "aaaStorSimple feladatátvétel és vész-helyreállítási |} Microsoft Docs"
description: "Megtudhatja, hogyan toofail a StorSimple eszköz tooitself, egy másik fizikai eszköz vagy egy virtuális eszközt."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: 00ce365f8a9095d1f0292e665d7f9eaa844b44ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>A StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag leírja hello lépéseket szükséges toofail hello esemény egy olyan vészhelyzet esetén a StorSimple eszköz keresztül. A feladatátvétel lehetővé teszi a toomigrate az adatok egy forrás eszközt hello datacenter tooanother fizikai vagy virtuális eszköz található hello azonos vagy különböző földrajzi elhelyezkedését. 

Vészhelyreállítás (DR) keresztül hello eszköz feladatátvételi szolgáltatás vezénylését van, és hello kezdeményezi **eszközök** lap. Ezen a lapon minden hello StorSimple eszközök csatlakoztatott tooyour StorSimple Manager szolgáltatás vannak. Minden eszköz hello rövid nevét, állapotát, kiépített és a maximális kapacitás típus és modell jelenik meg.

![Eszközök oldal](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Ebben az oktatóanyagban hello útmutatást tooStorSimple fizikai és virtuális eszközökkel érvényes összes szoftver verziója között.

## <a name="disaster-recovery-dr-and-device-failover"></a>Vészhelyreállítás (DR) és az eszköz feladatátvételi
Egy vész-helyreállítási forgatókönyv szerint hello elsődleges eszköz nem működik. Ebben a helyzetben hello felhőbeli adatát hello sikertelen eszköz tooanother eszközzel társított elsődleges eszköz hello hello segítségével áthelyezheti *forrás* és egy másik eszköz megadásával, hello *cél*. Egy vagy több kötet tárolók toomigrate toohello céleszközön választhatja ki. A folyamat be nem hivatkozott tooas hello *feladatátvételi*. 

Hello a feladatátvételi hello forrás eszközről hello kötettárolók tulajdonjogai módosulnak és átvitt toohello céleszközön. Amikor hello kötettárolók módosítja tulajdonjogot, ezeket a rendszer törli hello forrás eszközről. Hello törlés befejezése után hello céleszköz tudja majd sikerült vissza.

A vész-Helyreállítási, hello legutóbbi biztonsági mentés általában követő használt toorestore hello adatok toohello céleszközön. Azonban ha több biztonsági mentési házirendjétől hello ugyanazon a köteten, majd a biztonsági mentési házirend hello kötetek legnagyobb számú hello kivételezett lekérdezi és hello házirend a legfrissebb biztonsági másolat használt toorestore hello adatok hello céleszközön.

Tegyük fel, ha két biztonsági mentési házirendek (egy alapértelmezett és egy egyéni) *defaultPol*, *customPol* a következő adatok hello:

* *defaultPol* : egy kötet *vol1*, napi kezdő pozíció: 10:30 PM futtatja.
* *customPol* : négy kötetek *vol1*, *vol2*, *vol3*, *vol4*, napi kezdő pozíció: 10:00 PM futtatja.

Ebben az esetben *customPol* több köteten van, és összeomlásbiztos rangsoroljuk használható. hello legfrissebb biztonsági másolat a szabályzat az használt toorestore adatai.

## <a name="considerations-for-device-failover"></a>Eszköz feladatátvételi szempontjai
Egy olyan vészhelyzet esetén hello esetben toofail kiválaszthatja a StorSimple eszköz keresztül:

* tooa fizikai eszköz 
* tooitself
* virtuális eszköz tooa

Minden eszköz feladatátvételi tartsa szem előtt tartva hello következő:

* hello Dr-előfeltételeket, hogy hello kötettárolók belül minden hello kötetek offline módban, és hello kötettárolók rendelkeznek a hozzárendelt felhő-pillanatfelvételt. 
* rendelkezésre álló Céleszközök hello vész-Helyreállítási eszközökre, amelyeken elegendő lemezterület tooaccommodate kijelölt kötettárolók hello. 
* hello csatlakoztatott tooyour eszközök szolgáltatással, de nem felelnek meg a megfelelő hello feltételeinek lemezterület nem lesz elérhető cél eszközként.
* Egy korlátozott időtartamra, a vész-Helyreállítási következő hello adatelérési teljesítményét jelentősen érintheti, hello eszközként fog kell tooaccess hello hello felhő adatait és helyileg tárolja.

#### <a name="device-failover-across-software-versions"></a>Eszköz feladatátvételi szoftver verziója között
Előfordulhat, hogy a StorSimple Manager szolgáltatás a központi telepítésben lévő több eszközön, mind a fizikai, mind a virtuális, az összes futó különböző szoftver verziója. Attól függően, hello szoftverének verziójával, hello kötet típusok hello eszközök is lehetnek. Például egy alkalmazást futtató eszköznek az Update 2-es vagy magasabb volna helyileg rögzített és rétegzett kötet (az archiválás alatt álló részhalmazának Szintezett). A frissítés előtti 2 eszköz a hello ugyanakkor előfordulhat, hogy rendelkezik rétegzett és archiválási kötetek. 

Ha egy másik szoftver verziója és hello probléma kötet típusú futtató során vész-Helyreállítási tooanother eszköz átveheti a következő tábla toodetermine hello használata.

| A feladatátvételt | Fizikai eszköz számára engedélyezett | Virtuális eszköz számára engedélyezett |
| --- | --- | --- |
| 2. frissítés toopre-1 (kiadás, 0,1, 0,2, 0,3) frissítése |Nem |Nem |
| 2 tooUpdate 1 (1, 1.1-es, 1.2-es) frissítése |Igen <br></br>Ha helyileg rögzített vagy rétegzett kötet vagy kötetek mindig feladatátvételt, két, hello vegyesen rétegzett. |Igen<br></br>Ha használatával helyileg rögzített kötetek, ezek rétegzett feletti sikertelen. |
| 2. frissítés tooUpdate 2 (újabb verzió) |Igen<br></br>A helyileg rögzített vagy rétegzett kötetek vagy a két kombinációját, ha hello kötetek mindig feladatátvétel történt, hello kötettípus; indítása rétegzett rétegzett és helyileg rögzített, a helyileg rögzített. |Igen<br></br>Ha használatával helyileg rögzített kötetek, ezek rétegzett feletti sikertelen. |

#### <a name="partial-failover-across-software-versions"></a>Részleges feladatátvételi szoftver verziója között
Ha azt tervezi, tooperform egy részleges frissítés előtti 1 tooa cél-frissítés 1-es vagy újabb rendszerű eszközt a StorSimple forrás feladatátvevő, hajtsa végre ezt az útmutatást. 

| A részleges feladatátvétel | Fizikai eszköz számára engedélyezett | Virtuális eszköz számára engedélyezett |
| --- | --- | --- |
| Frissítés előtti 1 (kiadás, 0,1, 0,2, 0,3) tooUpdate 1 vagy újabb verzió |Igen, tekintse meg alább a hello bevált gyakorlat tipp. |Igen, tekintse meg alább a hello bevált gyakorlat tipp. |

> [!TIP]
> Megváltozott egy felhőalapú metaadatokat és az adatokat formátum az 1. frissítés vagy újabb verzió használható. Ezért nem javasoljuk a részleges feladatátvétel, a frissítés előtti 1 tooUpdate 1 vagy újabb verzió. Ha tooperform részleges feladatátvétel van szüksége, javasoljuk, Update 1 vagy újabb mindkét hello eszközök (a forrás és a cél), és ezután folytathatja a hello feladatátvételi. 
> 
> 

## <a name="fail-over-tooanother-physical-device"></a>Feladatok átadása tooanother fizikai eszköz
Hajtsa végre a következő lépéseket toorestore hello az eszköz tooa cél fizikai eszköz.

1. Győződjön meg arról, hogy hello kötettároló toofail kívánt felhőalapú pillanatfelvételek vannak társítva.
2. A hello **eszközök** hello kattintson **Kötettárolók** fülre.
3. Válassza ki, hogy milyen toofail tooanother eszközön keresztül kötettároló. Kattintson a hello kötet tároló toodisplay hello kötetek listáját a tárolóban. Jelöljön ki egy kötetet, majd **Offline állapotba** tootake hello kötet offline állapotba. Ismételje meg ezt a folyamatot a hello kötettároló összes hello kötetet.
4. Ismétlődő hello előző lépést az összes hello kötettárolók milyen toofail tooanother eszközön keresztül.
5. A hello **eszközök** kattintson **feladatátvételi**.
6. A megnyitott, a varázsló hello **válassza ki a kötet tároló toofail keresztül**:
   
   1. Hello kötettárolók, jelölje ki hello kötettárolók milyen toofail keresztül.
      **Csak a hozzárendelt felhő pillanatképekkel kötettárolók hello és a kötetek offline állapotban jelennek meg.**
   2. A **válassza ki a céleszközön** hello kötetek kijelölt hello tárolókban, válassza ki a céleszközön az elérhető eszközök hello legördülő listából. Csak az olyan hello eszközökre, amelyeken a rendelkezésre álló kapacitásból hello hello legördülő listában jelennek meg.
   3. Végül ellenőrizze az összes hello feladatátvételi beállításokat alatt **megerősítéséhez feladatátvétel**. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. A feladatátvételi feladatban hoz létre, amely keresztül hello figyelhető **feladatok** lap. Hello keresztül megbukott kötettároló helyi köteteken, majd látják egyes visszaállítási feladat minden helyi kötet (nem a rétegzett kötetek) hello tárolóban. Ezek a feladatok visszaállítási egy idő toocomplete is igénybe vehet. Valószínű, előfordulhat, hogy korábban végezze el a hello feladatátvételi feladatot. Ne feledje, hogy ezek a kötetek fog helyi garanciákat csak hello visszaállítási feladatok végrehajtását követően. Hello feladatátvétel elvégzése után nyissa meg toohello **eszközök** lap.                                            
   
   1. Válassza ki a feladatátvételi folyamat hello hello cél eszközként használt hello eszközt.
   2. Nyissa meg toohello **Kötettárolók** lap. Minden hello kötettárolók, hello kötetek hello régi eszközről, és szerepelnie kell.

## <a name="failover-using-a-single-device"></a>Egyetlen eszköz használatával a feladatátvételi
Hajtsa végre a következő lépéseket, ha csak egyetlen eszköz és a szükséges tooperform feladatátvevő hello.

1. Az összes hello kötet felhőalapú pillanatfelvételek az eszközt a hálózatról.
2. Az eszköz toofactory Alapértelmezések visszaállítása. Hajtsa végre a hello részletes utasításait [hogyan tooreset a StorSimple eszköz toofactory alapértelmezett beállítások](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Állítsa be az eszközt, és regisztrálja újra a StorSimple Manager szolgáltatásban.
4. A hello **eszközök** lapon jelennek meg hello régi eszköz **Offline**. hello újonnan regisztrált eszközre kell megjeleníteni **Online**.
5. Hello új eszközhöz, hello minimális konfigurálásának befejezése hello eszköz először. 
   
   > [!IMPORTANT]
   > **Ha hello minimális konfiguráció nem fejeződött be először, a vész-Helyreállítási hello aktuális végrehajtása hiba miatt sikertelen lesz. Ez a viselkedés egy későbbi kiadásban lesz kijavítva.**
   > 
   > 
6. Jelöljön ki hello régi eszköz (kapcsolat nélküli állapota), majd **feladatátvételi**. Hello varázslóban megjelenő az eszköz feladatátvétele, és adja meg a hello céleszköz hello újonnan regisztrált eszközre. Részletes útmutatásért lásd túl[tooanother fizikai eszköz feladatátvételt](#fail-over-to-another-physical-device).
7. Egy eszköz-visszaállítási feladat jön létre, hogy hello a figyelheti **feladatok** lap.
8. Miután hello feladat sikeresen befejeződött, hello új eszközhöz való hozzáféréshez, és keresse meg a toohello **Kötettárolók** lap. Minden hello kötettárolók hello régi eszközről kell áttelepített toohello új eszköz.

## <a name="fail-over-tooa-storsimple-virtual-device"></a>Feladatok átadása tooa StorSimple virtuális eszköz
Rendelkeznie kell egy StorSimple virtuális eszköz létrehozása, és a korábbi toorunning konfigurálása ezt az eljárást. Ha 2. frissítés fut, érdemes egy 8020-as modell virtuális eszköz az hello vész-Helyreállítási 64 TB-os és prémium szintű tárolást használ. 

Hajtsa végre a következő lépéseket toorestore hello eszköz tooa cél StorSimple virtuális eszköz hello.

1. Győződjön meg arról, hogy hello kötettároló toofail kívánt felhőalapú pillanatfelvételek vannak társítva.
2. A hello **eszközök** hello kattintson **Kötettárolók** fülre.
3. Válassza ki, hogy milyen toofail tooanother eszközön keresztül kötettároló. Kattintson a hello kötet tároló toodisplay hello kötetek listáját a tárolóban. Jelöljön ki egy kötetet, majd **Offline állapotba** tootake hello kötet offline állapotba. Ismételje meg ezt a folyamatot a hello kötettároló összes hello kötetet.
4. Ismétlődő hello előző lépést az összes hello kötettárolók milyen toofail tooanother eszközön keresztül.
5. A hello **eszközök** kattintson **feladatátvételi**.
6. A megnyitott, a varázsló hello **válassza ki a kötet tároló toofailover**, végezze el a következő hello:
   
    a. Hello kötettárolók, jelölje ki hello kötettárolók milyen toofail keresztül.
   
    **Csak a hozzárendelt felhő pillanatképekkel kötettárolók hello és a kötetek offline állapotban jelennek meg.**
   
    b. A **válassza ki a céleszközön hello kötetek kijelölt hello tárolókban lévő**, válassza ki az elérhető eszközök hello legördülő listából a StorSimple virtuális eszköz hello. **Csak az olyan hello eszközökre, amelyeken elegendő kapacitással hello legördülő listában jelennek meg.**  
7. Végül ellenőrizze az összes hello feladatátvételi beállításokat alatt **megerősítéséhez feladatátvétel**. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Hello feladatátvétel elvégzése után nyissa meg toohello **eszközök** lap.
   
    a. Válassza ki a hello StorSimple virtuális eszköz hello feladatátvételi folyamat hello cél eszközként használttal.
   
    b. Nyissa meg toohello **Kötettárolók** lap. Most már minden hello kötettárolók, valamint hello kötetek hello régi eszközről szerepelnie kell.

![Videó elérhető](./media/storsimple-device-failover-disaster-recovery/Video_icon.png)**Videó elérhető**

Hogyan visszaállíthatja egy sikertelen keresztül fizikai eszköz tooa virtuális eszköz hello felhőben videó toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Feladat-visszavétel
Az Update 3 és újabb verziókban a StorSimple feladat-visszavétel is támogatja. Hello feladatátvételi befejeződése után hello következő műveletek történnek meg:

* feladatátvétel történt hello kötettárolók megtisztítja hello forrás eszközről.
* A háttérben futó feladat (feladatátvételt) kötettároló / hello forráseszközt indítható. Ha toofailback kísérletet, amíg hello feladat van folyamatban, egy értesítés toothat hatást fog kapni. Toowait szüksége lesz, amíg hello feladat teljes toostart hello feladat-visszavétel nem. 
  
    hello idő toocomplete hello törlésének kötettárolók függő számos tényező befolyásolja, például az adatok mennyiségét, hello adatok, a biztonsági mentések és a rendelkezésre álló hálózati sávszélesség hello száma korát hello a művelethez. Ha a teszt feladatátvétel/visszavételek azt tervezi, azt javasoljuk, hogy tesztelje kötettárolók a kevesebb adat (GB). A legtöbb esetben elindíthatja hello feladat-visszavétel 24 óra hello feladatátvételi befejeződése után. 

## <a name="frequently-asked-questions"></a>Gyakori kérdések
Q. **Mi történik, ha a vész-Helyreállítási hello meghibásodik, vagy részleges sikeres rendelkezik?**

A. Hello DR sikertelen lesz, azt javasoljuk, hogy újból megpróbálná. hello körül, még egyszer vész-Helyreállítási tudja, hogy mit összes végezhető el, és ha hello folyamat leállását hello első alkalommal. hello vész-Helyreállítási folyamat elindítja az ettől a ponttól kezdve. 

Q. **Törölhetek egy eszközt, amíg hello eszköz feladatátvétel van folyamatban?**

A. Egy eszköz nem törölhető, amíg folyamatban van egy vész-Helyreállítási. Az eszköz csak a vész-Helyreállítási hello befejezése után törölhetők.

Q.    **Ha nem hello szemétgyűjtés start hello forrás eszközön, hogy a helyi adatok hello forrás eszköz törlése?**

A. A szemétgyűjtés elérhetővé válik hello forráseszközt csak azt követően hello eszköz teljesen karbantartása. hello tisztítás tartalmaz objektumokat, amelyek nem tudták keresztül a hello forrás eszköz, például kötetek, a biztonsági másolat objektumok (adatokat nem), a kötettárolók és a házirendek törléséről.

Q. **Mi történik, ha hello hello kötettárolók hello forráseszközt a társított feladat törlése sikertelen?**

A.  Ha hello törlése feladat sikertelen, akkor szüksége lesz hello kötettárolók toomanually eseményindító hello törlését. A hello **eszközök** lapon válassza ki a forrás eszközt, majd kattintson **kötettárolók**. Jelölje be hello kötettárolók keresztül, és a hello lap aljára hello sikertelen kattintson **törlése**. Ha törölt minden hello kötettárolók hello forrás eszközön a feladatátvételt, megkezdheti hello feladat-visszavételre.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Üzleti folytonosság vészhelyreállítási (BCDR)
Üzleti folytonosság (BCDR) vészhelyreállítás hello teljes Azure-adatközpontban leáll a működése következik be. Ez befolyásolhatja a StorSimple Manager szolgáltatás és hello társított StorSimple eszközökhöz.

Ha StorSimple eszközök regisztrálása előtt történt egy olyan vészhelyzet esetén volt, a StorSimple eszköz egy gyári tooundergo esetleg. Hello katasztrófa utáni hello StorSimple eszköz jelenik meg offline állapotúként. hello StorSimple eszköz törlését hello portálról, és a gyári beállítások visszaállítása el kell végezni, egy friss regisztrációs követ.

## <a name="next-steps"></a>Következő lépések
* Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-deactivate-and-delete-device.md).
* Információ a hogyan toouse hello StorSimple Manager szolgáltatás, nyissa meg túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

