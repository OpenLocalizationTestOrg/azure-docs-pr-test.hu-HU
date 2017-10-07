---
title: "a feladatátvétel aaaStorSimple 8000 sorozat eszközeire vész-helyreállítási |} Microsoft Docs"
description: "Ismerje meg, hogy a StorSimple eszköz tooitself, egy másik fizikai eszköz vagy egy felhőalapú készülék toofail."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 9d01ee30b15b77072b1d4dfe9a215abc83ffba28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>A StorSimple 8000 series eszköz a feladatátvételi és katasztrófa-helyreállítás

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti a hello eszköz feladatátvételi szolgáltatás hello StorSimple 8000 sorozat eszközeire, és hogyan Ez a funkció használt toorecover StorSimple eszközökhöz Ha katasztrófa történik. A StorSimple eszköz feladatátvételi toomigrate hello adatait használja a forráseszközt hello datacenter tooanother céleszközön. Ez a cikk útmutatást hello tooStorSimple 8000 sorozat fizikai eszközöket és a felhő készülékek 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.

StorSimple használ hello **eszközök** panel toostart hello eszköz feladatátvételi szolgáltatást hello egy olyan vészhelyzet esetén. A panel összes hello StorSimple eszközök csatlakoztatott tooyour StorSimple Device Manager szolgáltatás sorolja fel.

![Eszközök panel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Vészhelyreállítás (DR) és az eszköz feladatátvételi

Egy vész-helyreállítási forgatókönyv szerint hello elsődleges eszköz nem működik. StorSimple használja elsődleges eszköz hello legyen _forrás_ és hello társított felhő adatok tooanother _cél_ eszköz. A folyamat be nem hivatkozott tooas hello *feladatátvételi*. a következő ábra hello a feladatátvételi hello folyamatát mutatja be.

![Mi történik, az eszköz feladatátvételi?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

hello céleszköz feladatátvevő egy fizikai eszköz vagy akár egy felhőalapú készülék lehet. hello céleszköz lehet hello ugyanaz vagy hello forráseszközt földrajzi helyen található.

Hello feladatátvétel során kiválaszthatja áttelepítésre kötettárolók. StorSimple majd módosítja a kötettárolók hello tulajdonjogát hello forrás eszköz toohello céleszközön. Miután hello kötettárolók módosítja tulajdonjogot, StorSimple ezekhez a tárolókhoz hello forráseszközt törli. Hello törlés befejeződése után vissza hello céleszköz sikertelen lehet. _Feladat-visszavétel_ átvitelek hello tulajdonjoga hátsó toohello eredeti forráseszközt.

### <a name="cloud-snapshot-used-during-device-failover"></a>Felhő-pillanatfelvételt eszköz feladatátvétel során

A vész-Helyreállítási következő hello legutóbbi felhőbeli biztonsági mentését használt toorestore hello adatok toohello céleszközön. A felhőalapú pillanatfelvételek további információkért lásd: [hello StorSimple Device Manager szolgáltatás tootake manuális biztonsági mentés használata](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

A StorSimple 8000 Series biztonsági mentési házirendek nincsenek társított biztonsági másolatok. Ha több biztonsági mentési házirendek a hello ugyanazon a köteten, majd a StorSimple választja ki hello hello kötetek legnagyobb száma a biztonsági mentési házirend. StorSimple hello legutóbbi biztonsági mentés hello kiválasztva a biztonsági mentési házirend toorestore hello adatokat, majd használja hello céleszközön.

Tegyük fel, hogy két biztonsági mentési házirend, *defaultPol* és *customPol*:

* *defaultPol*: egy kötet *vol1*, napi kezdő pozíció: 10:30 PM futtatja.
* *customPol*: négy kötetek *vol1*, *vol2*, *vol3*, *vol4*, napi kezdő pozíció: 10:00 PM futtatja.

Ebben az esetben a StorSimple előtérbe helyezi az összeomlásbiztos, és használja *customPol* , mert több köteten. hello legfrissebb biztonsági másolat a szabályzat az használt toorestore adatai. További információt a toocreate és biztonsági mentési házirendek kezelése, nyissa meg túl[hello StorSimple Device Manager szolgáltatás toomanage biztonsági mentési házirendek használata a](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Eszköz feladatátvételi a gyakori szempontok

Eszköz átadása, tekintse át a következő információ hello:

* Egy eszköz feladatátvétel megkezdése előtt hello kötettárolók belül minden hello kötetek offline állapotban kell lennie. Egy nem tervezett feladatátvétel StotSimple kötetek lesz automatikusan kapcsolat nélkül. De egy tervezett feladatátvételt (tootest DR) hajtja végre, ha minden hello kötetek offline állapotba kell végrehajtania.
* Csak hello kötettárolók, amelyek rendelkeznek a hozzárendelt felhő-pillanatfelvételt vész-Helyreállítási fel vannak sorolva. Egy felhőbeli pillanatfelvétel toorecover adatokkal legalább egy kötettárolót kell lennie.
* Ha vannak, amelyek több kötettárolók eloszthatja felhőalapú pillanatfelvételek, StorSimple feladatátadás ezek kötettárolók készletként. A ritka esetben ha, amely több kötettárolók eloszthatja helyi pillanatképek vannak, de a hozzárendelt felhő pillanatképek nem, StorSimple feladatátvételt hello helyi pillanatképeket és hello helyi adatok elvesznek vész-Helyreállítási után.
* rendelkezésre álló Céleszközök hello vész-Helyreállítási eszközökre, amelyeken elegendő lemezterület tooaccommodate kijelölt kötettárolók hello. Azokat az eszközöket, amelyeken nincs elegendő lemezterület nem találhatók cél eszközként.
* (Egy korlátozott ideig) DR, hello adatelérési teljesítményét jelentősen érintheti hello eszközként kell tooaccess hello hello felhő adatait és, helyileg tárolja.

#### <a name="device-failover-across-software-versions"></a>Eszköz feladatátvételi szoftver verziója között

Előfordulhat, hogy egy központi telepítésben lévő StorSimple Device Manager szolgáltatás több eszközön, mindkét fizikai és a felhőben, az összes futó különböző szoftververziók.

Ha a feladatátvétel, vagy egy másik szoftver verziója, hello kötet típusok működése során vész-Helyreállítási futtató hátsó tooanother eszköz sikertelen a következő tábla toodetermine hello használata.

#### <a name="failover-and-failback-across-software-versions"></a>A feladatátvételi és a feladat-visszavétel szoftver verziója között

| Feladatok átadása a / ból | Fizikai eszköz | Felhőalapú készülék |
| --- | --- | --- |
| 4. frissítés 3 tooUpdate |Rétegzett kötetek rétegzett feletti sikertelen. <br></br>Helyileg rögzített kötetekhez átadja a helyileg rögzített. <br></br> Ha pillanatképet készít hello Update 4 eszközön, a következő feladatátvétel [heatmap-alapú nyomkövetési](storsimple-update4-release-notes.md#whats-new-in-update-4) lép működésbe. |Helyileg rögzített, rétegzett kötetek feladatait. |
| 4. frissítés tooUpdate 3 |Rétegzett kötetek rétegzett feletti sikertelen. <br></br>Helyileg rögzített kötetekhez átadja a helyileg rögzített. <br></br> Biztonsági mentések használt toorestore heatmap metaadatainak megőrzése mellett. <br></br>Nyomkövetési Heatmap-alapú frissítés 3. a feladat-visszavétel a következő nem érhető el. |Helyileg rögzített, rétegzett kötetek feladatait. |


## <a name="device-failover-scenarios"></a>Eszköz feladatátvételi forgatókönyvek

Ha egy olyan vészhelyzet esetén, a StorSimple eszköz keresztül úgy is dönthet, toofail:

* [fizikai eszköz tooa](storsimple-8000-device-failover-physical-device.md).
* [tooitself](storsimple-8000-device-failover-same-device.md).
* [tooa felhő készülék](storsimple-8000-device-failover-cloud-appliance.md).

hello előző cikkekben részletes lépéseket az egyes hello feladatátvételi esetekben fent.


## <a name="failback"></a>Feladat-visszavétel

Az Update 3 és újabb verziókban a StorSimple feladat-visszavétel is támogatja. Feladat-visszavétel csak hello megfordítása a feladatátvétel, hello cél lesz hello forrás pedig hello most hello feladatátvételkor eredeti forráseszközt hello céleszköz. 

A feladat-visszavétel során a StorSimple hello hátsó toohello elsődleges hely újra szinkronizálja, hello i/o- és alkalmazás tevékenység leáll, és átmenetek hátsó toohello eredeti helyére.

A feladatátvétel befejeződött, a StorSimple hello a következő műveleteket hajtja végre:

* StorSimple hello forrás eszközről feladatátvétel történt hello kötettárolók törlése.
* StorSimple / kötettároló (keresztül) az eszközön nem sikerült hello forrás egy háttérben futó feladatot indít. Ha toofail vissza, miközben hello feladat van folyamatban, kap egy értesítési toothat hatást. Várja meg, amíg hello feladat teljes toostart hello feladat-visszavételre.
* hello idő kötettárolók toocomplete hello törlését számos tényező befolyásolja, például az adatok mennyiségét, hello adatok, a biztonsági mentések és a rendelkezésre álló hálózati sávszélesség hello száma korát hello a művelethez függ.

Ha tervezi, feladatátvételi teszteket, vagy visszavételek teszteli, azt javasoljuk, hogy tesztelje kötettárolók a kevesebb adat (GB). Általában megkezdheti hello feladat-visszavétel 24 óra hello feladatátvételi befejeződése után.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

Q. **Mi történik, ha a vész-Helyreállítási hello meghibásodik, vagy részleges sikeres rendelkezik?**

A. Hello DR sikertelen lesz, azt javasoljuk, hogy újból megpróbálná. hello második eszköz feladatátvételi feladatban hello első feladat előrehaladásának hello tudomást elindul és ettől a ponttól kezdve.

Q. **Törölhetek egy eszközt, amíg hello eszköz feladatátvétel van folyamatban?**

A. Egy eszköz nem törölhető, amíg folyamatban van egy vész-Helyreállítási. Az eszköz csak a vész-Helyreállítási hello befejezése után törölhetők. Figyelheti hello eszköz feladatátvételi feladat előrehaladását a hello **feladatok** panelen.

Q. **Ha nem hello szemétgyűjtés start hello forrás eszközön, hogy a helyi adatok hello forrás eszköz törlése?**

A. A szemétgyűjtés hello forrás eszközön engedélyezve van, csak azt követően hello eszköz teljesen karbantartása. hello tisztítás tartalmaz objektumokat, amelyek nem tudták keresztül a hello forrás eszköz, például kötetek, a biztonsági másolat objektumok (adatokat nem), a kötettárolók és a házirendek törléséről.

Q. **Mi történik, ha hello hello kötettárolók hello forráseszközt a társított feladat törlése sikertelen?**

A.  Ha hello törlése feladat sikertelen, majd manuálisan törölheti hello kötettárolók. A hello **eszközök** panelen válassza ki a forrás eszközt, és kattintson a **kötettárolók**. Jelölje be hello kötettárolók keresztül, és a hello panel alján hello sikertelen kattintson **törlése**. A törölt minden hello kötettárolók hello forrás eszközön a feladatátvételt, megkezdheti hello feladat-visszavételre. További információ: túl[kötettároló törlése](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>Üzleti folytonosság vészhelyreállítási (BCDR)

Üzleti folytonosság (BCDR) vészhelyreállítás hello teljes Azure-adatközpontban leáll a működése következik be. Ebben a forgatókönyvben hatással lehet a StorSimple Device Manager szolgáltatáshoz, és a hello kapcsolódó StorSimple eszközökhöz.

A StorSimple eszköz regisztrálták előtt történt egy olyan vészhelyzet esetén, ha az eszközt a gyári tooundergo esetleg. Hello katasztrófa utáni hello StorSimple eszköz megjelennek hello Azure portált mint offline állapotú. Ez az eszköz törlését hello portálról. Hello eszköz toofactory visszaállítása, és regisztrálja újra hello szolgáltatásban.

## <a name="next-steps"></a>Következő lépések

Ha készen áll a tooperform eszköz feladatátvétel, a következő forgatókönyvek a részletes utasításokat hello közül választhat:

- [Feladatok átadása tooanother fizikai eszköz](storsimple-8000-device-failover-physical-device.md)
- [Feladatok átadása a toohello azonos eszköz](storsimple-8000-device-failover-same-device.md)
- [Feladatok átadása tooStorSimple felhő készülék](storsimple-8000-device-failover-cloud-appliance.md)

Ha az eszköz rendelkezik feladatátvételt, hello alábbi beállítások közül lehet választani:

* [Inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).
* [Használjon hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

