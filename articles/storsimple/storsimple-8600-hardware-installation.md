---
title: "a Microsoft Azure StorSimple 8600 aaaInstall eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toounpack, állványra szerelése és bekábelezése a StorSimple 8600 hello szoftver konfigurálása és központi telepítése előtt."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/24/2016
ms.author: alkohli
ms.openlocfilehash: 0fc0ddf076725fededdde33a260b950b72edc8db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Kicsomagolása, állványra-csatlakoztatási, és a StorSimple 8600 bekábelezése
## <a name="overview"></a>Áttekintés
A Microsoft Azure StorSimple 8600 kettős ház eszköz, és egy elsődleges és egy EBOD ház áll. Ez az oktatóanyag azt ismerteti, hogyan toounpack, állványra-szerelheti és bekábelezheti hello StorSimple 8600 eszközhardver hello StorSimple szoftver konfigurálása előtt.

## <a name="unpack-your-storsimple-8600-device"></a>Bontsa ki a StorSimple 8600 eszköz
hello alábbi lépések nyújtanak törölje, részletes utasításokat toounpack a StorSimple 8600 tárolóeszköz. Ez az eszköz a két mezőbe, hello elsődleges ház egyet, majd egy másikat a hello EBOD ház nyújtják. Ezek a mezők két majd kerülnek mezőt.

### <a name="prepare-toounpack-your-device"></a>Az eszköz toounpack előkészítése
Előtt kicsomagolja az eszközt, tekintse át a következő információ hello.

![Figyelmeztetés ikon](./media/storsimple-safety/IC740879.png)![nagy súlyozást ikon](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **figyelmeztetés!**

1. Győződjön meg arról, hogy rendelkezik két személyek elérhető toomanage hello súly hello eszköz, ha manuálisan is kezel. A teljesen konfigurált ház mérjük is meg fel too32 kg (70 lbs.).
2. Hello jelölőnégyzetet elhelyez egy egyszerű, szintű felületen.

Ezután hajtsa végre a következő lépéseket toounpack hello az eszközt.

#### <a name="toounpack-your-device"></a>toounpack az eszköz
1. Vizsgálja meg a hello mezőbe, majd hello csomagolás használatos crushes, darabok, vízjel sérülés vagy más egyértelmű károkért. Ha hello mező vagy a csomag súlyosan sérült, nem nyitható meg a hello mezőbe. Adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) toohelp meg ellenőrzéséhez, hogy hello eszköz működőképes állapotban van.
2. Hello külső megnyitásához, és ezután vegye ki a megfelelő tooprimary és EBOD csatolmányt hello két mezőbe. Elsődleges hello és EBOD csatolmányt most csomagolja ki. a következő ábra hello kicsomagolása hello nézet egy hello házakat látható.
   
    ![Bontsa ki a tárolóeszköz](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **A tárolóeszköz kibontott nézete**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Csomagolási mezőbe |
   |   2 |SAS-kábel (a Kellékek és kábelek tálca) |
   |   3 |Alsó hab |
   |   4 |Eszköz |
   |   5 |Felső hab |
   |   6 |Kiegészítő mezőben |
3. Után kicsomagolása hello két jelölőnégyzetéből, győződjön meg arról, hogy rendelkezik-e:
   
   * 1 elsődleges ház (hello elsődleges ház és EBOD ház van két külön mező)
   * 1 EBOD ház
   * 4 tápkábelek, minden mezőbe 2
   * 2 SAS-kábel (tooconnect hello elsődleges ház tooEBOD ház)
   * 1 fordított kábel
   * 2 soros konzol kábelek
   * a soros hozzáféréshez 1 soros-USB-konverter
   * 4 QSFP-az-adapterek SFP + 10 GbE hálózati adapterrel való használatra
   * 2 állványra csatlakoztatási kits (4 ügyféloldali sínek hardver, 2 minden hello elsődleges ház és EBOD ház csatlakoztatni), minden mezőbe 1
   * Első lépések dokumentáció
     
     Ha Ön nem kapta meg a fent felsorolt hello elemek bármelyikét [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).  

következő lépés hello toorack-csatlakoztatási van az eszköz.

## <a name="rack-mount-your-storsimple-8600-device"></a>Állvány csatlakoztatást a StorSimple 8600 eszköz
Hajtsa végre a hello következő lépések tooinstall a StorSimple 8600 tárolóeszköz szabványos 19 hüvelykes szekrényben az első és hátsó bejegyzéseket. Az eszköz két ház rendelkezik: egy elsődleges ház és egy EBOD ház. Mindkét esetben kell toobe állványra szerelt.

a következő eljárások hello ismertet, amelyek több lépést hello telepítési áll.

> [!IMPORTANT]
> StorSimple eszközökhöz kell lennie az állványra szerelt megfelelő működéséhez.
> 
> 

### <a name="site-preparation"></a>Hely előkészítése
hello ház szabványos 19 hüvelykes szekrényben, amely rendelkezik az első és hátsó bejegyzéseket egyaránt telepíteni kell. A következő eljárás tooprepare állvány telepítéshez hello használata.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>állvány telepítéshez tooprepare hello hely
1. Győződjön meg arról, hogy hello elsődleges és EBOD ház nyugalmi biztonságosan egy egyszerű, stabil és szintű munkahelyi felületén (vagy hasonló).
2. Ellenőrizze célhelyeként tooset fel van szabványos AC hello hely energiagazdálkodási egy független forrás vagy a szünetmentes tápegység (UPS) állvány Power terjesztési egységek (PDU).
3. Győződjön meg arról, hogy egy 4U (2 X 2U) bővítőhely toomount hello ház profilokhoz hello állvány érhető el.

![Figyelmeztetés ikon](./media/storsimple-safety/IC740879.png)![nagy súlyozást ikon](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **figyelmeztetés!**

 Győződjön meg arról, hogy rendelkezik két személyek elérhető toomanage hello súly, ha hello Eszközbeállítás manuálisan is kezel. A teljesen konfigurált ház mérjük is meg fel too32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Állvány Előfeltételek
hello ház készültek a kabinetfájl szabványos 19 hüvelykes szekrényben telepítése:

* Az állvány 27.84 hüvelyk minimális mélysége utáni toopost
* Legfeljebb 32 kg hello eszköz súlyú
* Maximális hátsó nyomás 5 Pascal (0,5 mm vízjel mérőműszer)

### <a name="rack-mounting-rail-kit"></a>Állvány csatlakoztatása oldalon csomag
Sínek csatlakoztatása készlete nyújtanak hello 19 hüvelykes állvány kabinet való használatra. hello sínek lettek tesztelve toohandle hello maximális ház súly. Ezek sínek lehetővé teszi több házakat hello állvány lemezterülete adatvesztés nélkül telepítési. Telepítse a hello EBOD ház.

#### <a name="tooinstall-hello-ebod-enclosure-on-hello-rails"></a>tooinstall hello EBOD ház hello sínen
1. Hajtsa végre ezt a lépést, csak akkor, ha a belső sínek nincsenek telepítve az eszközön. Általában hello belső sínek hello gyári telepített. Ha sínek nincsenek telepítve, akkor telepítse hello balra-oldalon, és jobb-oldalon diák toohello hello ház váz oldalán. Hat metrika csavart segítségével mindkét oldalon csatolja azokat. a tájolás diák megjelölve hello oldalon toohelp **LH – első** és **RH – első**, és hello célból, hogy a hello ház hello hátrafelé dobozának kúpos.
   
    ![Csatlakoztatását oldalon diák tooenclosure készülékház](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Csatlakoztatását oldalon diák hello ház toohello oldala**
   
   | Címke | Leírás |
   | --- | --- |
   |  1 |M 3 x 4 gomb-head csavart |
   |  2 |Váz diák |
2. Hello bal oldalon és a jobb oldalon szerelvények toohello állvány kabinet függőleges tagok csatolni. hello zárójeleket megjelölve **LH**, **RH**, és **Ez az oldal fel** tooguide le a megfelelő tájolását.
3. Keresse meg a hello oldalon PIN-kódok hello első és hátsó hello oldalon szerelvény. Kiterjesztése hello oldalon toofit hello állvány bejegyzések között, és hello PIN-kód beszúrása hello első és hátsó-állvány post függőleges tag lyuk. Győződjön meg arról, hogy hello oldalon szerelvény be-e szint.
4. Biztonságos hello oldalon szerelvény toohello állvány használatával két hello metrika csavart függőleges tagok megadott. Használjon egy csavart hello első és hátsó hello a.
5. Ismételje meg ezeket a lépéseket hello más oldalon szerelvény.
   
     ![Csatlakoztatását oldalon diák toorack kabinet](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Oldalon szerelvények toohello állvány csatolása**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Rögzítéssel csavart |
   |   2 |Négyzetes-lyuk első állvány post csavart |
   |   3 |PIN-kód helye első oldalon balra |
   |   4 |Rögzítéssel csavart |
   |   5 |Bal oldali hátsó oldalon hely PIN-kód |

### <a name="mounting-hello-ebod-enclosure-in-hello-rack"></a>Hello EBOD ház hello szekrényben csatlakoztatása
Csak telepített hello állvány sínek végezze el a következő lépéseket toomount hello EBOD ház hello szekrényben hello.

#### <a name="toomount-hello-ebod-enclosure"></a>toomount hello EBOD ház
1. A segítségnyújtó növekedési hello ház és hello állvány sínek igazíthatja.
2. Gondosan hello ház beszúrása hello sínek, és majd leküldeni teljesen hello állvány a kabinetfájl.
   
    ![Hello szekrényben eszköz beszúrása](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Hello ház hello szekrényben csatlakoztatása**
3. Távolítsa el a hello bal és jobb első nyomkarima caps modulba húzza hello caps szabad. hello nyomkarima caps egyszerűen illesztés hello karimával helyezik.
4. Biztonságos hello ház hello állvány be egy megadott Phillips-head csavart keresztül minden nyomkarima, bal és jobb telepítésével.
5. Nyomja le az azokat egy helyen, és azokat a hely illesztésének hello nyomkarima caps telepítéséhez.
   
     ![Nyomkarima caps telepítése](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Hello nyomkarima caps telepítése**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Ház rögzítési csavart |

### <a name="mounting-hello-primary-enclosure-in-hello-rack"></a>Hello elsődleges ház hello szekrényben csatlakoztatása
Miután befejezte a hello EBOD ház csatlakoztatni, szüksége lesz a toomount hello elsődleges ház következő hello ugyanazokat a lépéseket.

> [!NOTE]
> * Néhány üres bővítőhely a hello állványra hello elsődleges ház és hello EBOD ház között lehetséges toohave.
> * A megadott hello 2m SAS kábel tooconnect hello elsődleges ház toohello EBOD ház használja.
> * Nincsenek nem korlátozza a hello hello központi egység toohello EBOD egység relatív elhelyezését. Ezért hello elsődleges ház helyezhető hello felső tárhely és az alábbi hello EBOD ház – vagy fordítva.
> 
> 

következő lépés hello toocable van az eszköz a tápellátáshoz, a hálózati és a soros hozzáféréshez.

## <a name="cable-your-storsimple-8600-device"></a>A StorSimple 8600 bekábelezése
hello alábbi eljárások azt ismertetik, hogyan toocable a StorSimple 8600 eszköz a tápellátáshoz, a hálózati és a soros kapcsolatok.

### <a name="prerequisites"></a>Előfeltételek
Előkészületek toocable az eszközt, szüksége lesz:

* Az elsődleges ház és hello EBOD ház, teljesen kicsomagolása
* 4 tápkábelek (2 minden egyes elsődleges hello és hello EBOD ház) az eszköz használati utasításának megfelelően
* 2 SAS-kábel hello eszköz tooconnect hello EBOD ház toohello elsődleges ház található meg
* Hozzáférés too2 Áramelosztó egységekből (PDU-k) (ajánlott)
* Hálózati kábel
* A megadott soros kábelek
* Soros-USB-konverter hello megfelelő illesztőprogram telepítve a számítógépen (ha szükséges)
* A megadott 4 QSFP-a-adapterek SFP + 10 GbE hálózati adapterrel való használatra
* [Támogatott hardverek hello 10 GbE hálózati adapterek a StorSimple eszköz](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS- és Power kábelek
Az eszköz elsődleges ház és egy EBOD ház is rendelkezik. Ehhez a hello egységek toobe kábel együtt soros csatlakozású SCSI (SAS) kapcsolódási és a teljesítmény.

Beállítása hello ezen az eszközön első alkalommal, amikor lépésekkel hello SAS először kábelek és majd a teljes hello lépései power kábelek.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Hálózati kábel
Az eszköz aktív-készenléti állapotban lévő konfigurációban van: az adott időpontban egy tartományvezérlő modul aktív és feldolgozási összes lemez és hálózat műveletek során hello más vezérlő modul készenléti állapotba. Tartományvezérlő hiba lép fel, ha a hello készenléti vezérlő azonnal aktiválja, és továbbra is az összes hello lemez- és hálózati műveletek.

toosupport a redundáns vezérlő feladatátvételi van szüksége az eszköz hálózati, ahogy az alábbi lépésekkel hello toocable.

#### <a name="toocable-for-network-connection"></a>a hálózati kapcsolat toocable
1. Az eszköz minden tartományvezérlőn hat hálózati adapterrel rendelkezik: négy 1 GB/s és a két 10 GB/s Ethernet portokat. Tekintse meg a ábra tooidentify hello adatok portok követően az eszköz hello csatlakozópanel toohello.
   
     ![Csatlakozópanel 8600 eszköz](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Az eszköz visszaállítása a megjelenítő hello adatok portok**
   
   | Címke | Leírás |
   | --- | --- |
   |   0,1,4,5 |1 GbE hálózati adapterek |
   |   2,3 |10 GbE hálózati adapterek |
   |   6 |Soros portokhoz |
2. Tekintse meg a következő ábra a hálózati kábel hello. (kék folytonos vonal által hello minimális hálózati konfiguráció látható. A magas rendelkezésre állás és teljesítmény szükséges további konfigurációs látható szaggatott vonal.)

![A hálózati 4U bekábelezésére](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Az eszköz kábelek hálózati**

| Címke | Leírás |
| --- | --- |
| A |Internet-hozzáféréssel rendelkező LAN |
| B |A vezérlő 0 |
| C# |PCM 0 |
| D |1. vezérlő |
| E |PCM 1 |
| F |EBOD vezérlő 0 |
| G |1. EBOD vezérlő |
| H, I |Gazdagépek (például a fájlkiszolgálók) |
| 0-5 |Hálózati illesztők |
| 6 |Elsődleges ház |
| 7 |EBOD ház |

Hello eszköz kábelek, hello minimális konfiguráció kell rendelkeznie:

* Legalább két hálózati adapterrel minden tartományvezérlőn, a felhő hozzáféréshez és az egyikben iSCSI csatlakoztatva. hello DATA 0 port automatikusan engedélyezve és konfigurálva hello a hello eszköz soros konzolon keresztül. DATA 0, leszámítva adatokat egy másik portra kell toobe hello klasszikus Azure portál segítségével konfigurálható. Ebben az esetben csatlakozni a DATA 0 port toohello elsődleges LAN (Internet-hozzáféréssel rendelkező hálózati). hello más adatok portok lehet kapcsolatban tooSAN/iSCSI LAN (VLAN) szegmens hello hálózat, attól függően, hogy szánt hello szerepkör.
* Minden egyes tartományvezérlő csatlakoztatva toohello azonos csatolóinak azonos hálózati tooensure rendelkezésre állási vezérlő feladatátvétel esetén. Például ha úgy dönt, hogy a DATA 0 tooconnect és a DATA 3 egy hello, tartományvezérlői, kell a DATA 0 és a DATA 3 megfelelő tooconnect hello hello más vezérlő.

Vegye figyelembe a magas rendelkezésre állás és teljesítmény:

* Ha lehetséges, állítsa be két hálózati csatoló a felhőelérést (1 gbe-s) és az iSCSI (ajánlott 10 GbE) egy másik pár minden tartományvezérlőn.
* Ha lehetséges, kapcsolódó hálózati adapterek minden tartományvezérlő tootwo különböző kapcsolókhoz tooensure rendelkezésre állás kapcsoló hibát. hello ábrázolható hello két 10 GbE hálózati illesztőket, DATA 2 és a DATA 3, minden egyes tartományvezérlő csatlakoztatva tootwo különböző kapcsolókhoz. További információkért tekintse meg a toohello **hálózati illesztőt** alatt hello [a StorSimple eszköz követelményei magas rendelkezésre állású](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Ha SFP + adó az 10 GbE hálózati adaptereket használ, használjon hello megadott QSFP-SFP + adapterek. További információ: túl[támogatott hardveres hello 10 GbE hálózati adapterek a StorSimple eszköz](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Soros port kábelek
Hajtsa végre a következő lépéseket toocable hello a soros port.

#### <a name="toocable-for-serial-connection"></a>soros kapcsolat toocable
1. Az eszköz soros minden vezérlő egy csavarkulcsot ábrázoló ikon által azonosított-porttal rendelkezik. toolocate hello portok, tekintse meg a toohello bemutató ábrát, hello adatok portok a hello hátsó az eszközt.
2. Az eszköz csatlakozópanel hello aktív vezérlő azonosításához. Egy villogó kék LED azt jelzi, hogy hello vezérlő aktív.
3. Használjon a megadott hello soros kábelt (ha szükséges, a laptop átalakítója hello USB – soros), és a konzol vagy a számítógép (eszközzel terminálemuláció toohello) toohello soros port hello aktív vezérlő.
4. Hello soros-USB-illesztőprogramot (hello eszközzel) telepítése a számítógépre.
5. Hello soros-kapcsolat állítható be az alábbiak szerint:
   
   * 115 200 átviteli
   * 8 adatbitek
   * 1 stop bit
   * Nincs paritás
   * Folyamatvezérlés beállítása túl**nincs**
6. Ellenőrizze, hogy hello kapcsolat hello konzolon Enter billentyű megnyomásával működik. A soros konzol menü megjelenjen-e.

> [!NOTE]
> **Lights-Out felügyeleti:** telepítésekor hello eszköz van távoli adatközpontban vagy korlátozott hozzáférésű számítógép helyiségben, gondoskodjon arról, hogy hello soros kapcsolatok tooboth tartományvezérlők mindig csatlakoztatott tooa soros konzol kapcsoló vagy hasonló berendezések. Ez lehetővé teszi a sávon kívüli a távvezérlés és a támogatási műveletek esetén a hálózati megszakítása vagy váratlan meghibásodások esetén.
> 
> 

A teljesítmény, a hálózati hozzáférést, az eszköz kábelek befejeződött, és soros connection.hello következő lépés egy tooconfigure hello szoftver az eszközön.

## <a name="next-steps"></a>Következő lépések
Most már készen áll túl[központi telepítése és konfigurálása a helyszíni StorSimple eszköz](storsimple-deployment-walkthrough-u2.md).

