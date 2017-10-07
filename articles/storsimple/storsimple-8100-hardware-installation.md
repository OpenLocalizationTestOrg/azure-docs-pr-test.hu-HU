---
title: "a Microsoft Azure StorSimple 8100 aaaInstall eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toounpack, állványra szerelése és bekábelezése a StorSimple 8100 hello szoftver konfigurálása és központi telepítése előtt."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 76b94e57318b6c25dc3333ae73edc9e4752b7d1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Kicsomagolása, állványra-csatlakoztatási, és a StorSimple 8100 bekábelezése
## <a name="overview"></a>Áttekintés
A Microsoft Azure StorSimple 8100-e egy egyetlen ház, állványra szerelt eszköz. Ez az oktatóanyag azt ismerteti, hogyan toounpack, állványra-szerelheti és bekábelezheti hello StorSimple 8100 hardver beállítása és hello StorSimple eszköz üzembe helyezése előtt.

## <a name="unpack-your-storsimple-8100-device"></a>Bontsa ki a StorSimple 8100-as eszközhöz
hello alábbi lépések nyújtanak törölje, részletes leírja, hogyan toounpack a StorSimple 8100 tárolóeszköz. Ez az eszköz egyetlen mezőben van kiadva.

### <a name="prepare-toounpack-your-device"></a>Az eszköz toounpack előkészítése
Előtt kicsomagolja az eszközt, tekintse át a következő információ hello.

![Figyelmeztetés ikon](./media/storsimple-safety/IC740879.png)![nagy súlyozást ikon](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **figyelmeztetés!**

1. Győződjön meg arról, hogy rendelkezik hello ház két személyek elérhető toomanage hello súlya, ha manuálisan is kezel. A teljesen konfigurált ház mérjük is meg fel too32 kg (70 lbs.).
2. Hello jelölőnégyzetet elhelyez egy egyszerű, szintű felületen.

Ezután hajtsa végre a következő lépéseket toounpack hello az eszközt.

#### <a name="toounpack-your-device"></a>toounpack az eszköz
1. Vizsgálja meg a hello mezőbe, majd hello csomagolás használatos crushes, darabok, vízjel sérülés vagy más egyértelmű károkért. Ha hello mező vagy a csomag súlyosan sérült, nem nyitható meg a hello mezőbe. Adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) toohelp meg ellenőrzéséhez, hogy hello eszköz működőképes állapotban van.
2. Csomagolja ki hello mezőbe. a következő kép hello a StorSimple eszköz hello csomagolva nézetét jeleníti meg.
   
     ![Bontsa ki a tárolóeszköz](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **A tárolóeszköz kibontott nézete**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Csomagolási mezőbe |
   |   2 |Alsó hab |
   |   3 |Eszköz |
   |   4 |Felső hab |
   |   5 |Kiegészítő mezőben |
3. Után hello mezőben kicsomagolása, győződjön meg arról, hogy rendelkezik-e:
   
   * 1 egyetlen ház eszköz
   * 2 tápkábelek
   * 1 fordított kábel
   * 2 soros konzol kábelek
   * a soros hozzáféréshez 1 soros-USB-konverter
   * 1 hamisíthatatlan T10 csavarhúzóra
   * 4 QSFP-az-adapterek SFP + 10 GbE hálózati adapterrel való használatra
   * 1. állvány-csatlakoztatási kit (2 ügyféloldali sínek csatlakoztatni hardverkonfiguráción)
   * Getting Started dokumentáció
     
     Ha Ön nem kapta meg a fent felsorolt hello elemek bármelyikét [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md).

következő lépés hello toorack-csatlakoztatási van az eszköz.

## <a name="rack-mount-your-storsimple-8100-device"></a>Állvány csatlakoztatást a StorSimple 8100-as eszközhöz
Hajtsa végre a hello következő lépések tooinstall a StorSimple 8100 tárolóeszköz szabványos 19 hüvelykes szekrényben az első és hátsó bejegyzéseket. a StorSimple 8100 hello eszközhöz tartozik egy önálló elsődleges ház.

a következő eljárások hello ismertet, amelyek több lépést hello telepítési áll.

> [!IMPORTANT]
> StorSimple eszközökhöz kell lennie az állványra szerelt megfelelő működéséhez.
> 
> 

### <a name="prepare-hello-site"></a>Hello hely előkészítése
hello eszköz, amely az első és hátsó bejegyzéseket szabványos 19 hüvelykes állvány kell telepíteni. A következő eljárás tooprepare állvány telepítéshez hello használata.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>állvány telepítéshez tooprepare hello hely
1. Ellenőrizze, hogy hello eszközön biztonságosan alapja a strukturálatlan stabil és szintű munkahelyi felület (vagy hasonlót).
2. Győződjön meg arról, hogy ha fel szeretné tooset hello helyhez tartozik szabványos AC power független forrásból származó, vagy egy állvány terjesztési teljesítménye (PDU) amely egy szünetmentes rendelkezik tápegység (UPS).
3. Győződjön meg arról, hogy egy 2U tárolóhely toomount hello eszköz profilokhoz hello állvány érhető el.

![Figyelmeztetés ikon](./media/storsimple-safety/IC740879.png)![nagy súlyozást ikon](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **figyelmeztetés!**

Győződjön meg arról, hogy rendelkezik két személyek elérhető toomanage hello súly, ha hello Eszközbeállítás manuálisan is kezel. A teljesen konfigurált ház mérjük is meg fel too32 kg (70 lbs.).

### <a name="rack-prerequisites"></a>Állvány Előfeltételek
hello 8100-as ház kabinetfájl a szabványos 19 hüvelykes szekrényben telepítési készült:

* Az állvány 27.84 hüvelyk minimális mélysége toopost utáni.
* Legfeljebb 32 kg hello eszköz súlyú
* 5 Pascal (0,5 mm vízjel mérőműszer) maximális hátsó terhelés.

### <a name="rack-mounting-rail-kit"></a>Állvány csatlakoztatása oldalon csomag
Sínek csatlakoztatása készlete hello 19 hüvelykes állvány kabinet való használatra biztosítja. hello sínek lettek tesztelve toohandle hello maximális ház súly. Ezek sínek lehetővé teszi több borító hello állvány lemezterülete adatvesztés nélkül telepítését.

#### <a name="tooinstall-hello-device-on-hello-rails"></a>hello sínen tooinstall hello eszköz
1. Hajtsa végre ezt a lépést, csak akkor, ha a belső sínek nincsenek telepítve az eszközön. Általában hello belső sínek hello gyári telepített. Ha sínek nincsenek telepítve, akkor telepítse hello balra-oldalon, és jobb-oldalon diák toohello hello ház váz oldalán. Hat metrika csavart segítségével mindkét oldalon csatolja azokat. a tájolás diák megjelölve hello oldalon toohelp **LH – első** és **RH – első**, és hello célból, hogy a hello ház hello hátrafelé dobozának kúpos.<br/>
   
    ![Csatlakoztatását oldalon diák tooenclosure készülékház](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Csatlakoztatását belső oldalon diák hello ház toohello oldala**
   
    Címke | Leírás
    ----- | -----------
    1     | M 3 x 4 gomb-head csavart
    2     | Váz diák

2. Hello külső bal oldalon és a jobb oldalon külső szerelvények toohello állvány kabinet függőleges tagok csatolni. hello zárójeleket megjelölve **LH**, **RH**, és **Ez az oldal fel** tooguide le a hello javítsa ki a tájolás.
3. Keresse meg a hello oldalon PIN-kódok hello első és hátsó hello oldalon szerelvény. Kiterjesztése hello oldalon toofit hello állvány bejegyzések között, és hello PIN-kód beszúrása hello első és hátsó állvány post függőleges tag lyuk. Győződjön meg arról, hogy hello oldalon szerelvény be-e szint.
4. Használjon a megadott hello metrika csavart toosecure hello oldalon szerelvény toohello állvány függőleges tagokat. Használjon egy csavart hello első és hátsó hello a.
5. Ismételje meg ezeket a lépéseket hello más oldalon szerelvény.<br/>
   
     ![Csatlakoztatását oldalon diák toorack kabinet](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Külső oldalon szerelvények toohello állvány csatolása**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Rögzítéssel csavart |
   |   2 |Négyzetes-lyuk első állvány post csavart |
   |   3 |Első hely PIN-kód bal oldalon |
   |   4 |Rögzítéssel csavart |
   |   5 |Hátsó hely PIN-kód bal oldalon |

### <a name="mounting-hello-device-in-hello-rack"></a>Hello szekrényben hello eszköz csatlakoztatása
Csak telepített hello állvány sínek végezze el a következő lépéseket toomount hello eszköz hello szekrényben hello.

#### <a name="toomount-hello-device"></a>toomount hello eszköz
1. A segítségnyújtó növekedési hello ház és hello állvány sínek igazíthatja.
2. Gondosan hello eszköz beszúrása hello sínek, és majd leküldéses hello eszköz teljes mértékben be hello állvány kabinetfájl.<br/>
   
    ![Hello szekrényben eszköz beszúrása](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Hello szekrényben hello eszköz csatlakoztatása**
3. Távolítsa el a hello bal és jobb első nyomkarima caps modulba húzza hello caps szabad. hello nyomkarima caps egyszerűen illesztés hello karimával helyezik.
4. Biztonságos hello ház hello szekrényben egy megadott Phillips-head csavart keresztül minden nyomkarima, bal és jobb telepítésével.
5. Nyomja le az azokat egy helyen, és azokat a hely illesztésének hello nyomkarima caps telepítéséhez.<br/>
   
     ![Nyomkarima caps telepítése](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Hello nyomkarima caps telepítése**
   
   | Címke | Leírás |
   | --- | --- |
   |   1 |Ház rögzítési csavart |

következő lépés hello toocable van az eszköz a tápellátáshoz, a hálózati és a soros hozzáféréshez.

## <a name="cable-your-storsimple-8100-device"></a>A StorSimple 8100 bekábelezése
hello alábbi eljárások azt ismertetik, hogyan toocable a StorSimple 8100 eszköz a tápellátáshoz, a hálózati és a soros kapcsolatok.

### <a name="prerequisites"></a>Előfeltételek
Mielőtt elkezdené, az eszköz kábelek hello, szüksége lesz:

* A tárolóeszköz teljesen kicsomagolása és az állványra szerelt.
* az eszköz használati utasításának megfelelően 2 kábelek
* Hozzáférés too2 Áramelosztó egységekből (ajánlott).
* Hálózati kábel
* A megadott soros kábelek
* Soros USB konverter hello megfelelő illesztőprogram telepítve a számítógépen (ha szükséges)
* A megadott 4 QSFP-a-adapterek SFP + 10 GbE hálózati adapterrel való használatra
* [Támogatott hardverek hello 10 GbE hálózati adapterek a StorSimple eszköz](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Energiagazdálkodási kábelek
Az eszköz redundáns energia- és hűtési modulok (PCMs) tartalmazza. Mindkét PCMs toodifferent power források tooensure magas rendelkezésre állású csatlakoztatva, és telepítve kell lennie.

Végezze el a következő lépéseket toocable hello az eszköz power.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Hálózati kábel
Az eszköz egy aktív-készenléti állapotban lévő konfigurációs: egy adott időpontban egy tartományvezérlő modul aktív és feldolgozási összes lemez és hálózat műveletek során hello más vezérlő modul készenléti állapotba. Ha egy tartományvezérlő sikertelen, a hello készenléti vezérlő azonnal aktiválni, és továbbra is minden hello lemez- és hálózati műveletet.

toosupport a redundáns vezérlő feladatátvételi van szüksége az eszköz a következő lépéseket hello hálózati toocable.

#### <a name="toocable-for-network-connection"></a>a hálózati kapcsolat toocable
1. Az eszköz minden tartományvezérlőn hat hálózati adapterrel rendelkezik: négy 1 GB/s, és két 10 gbps-os Ethernet-portokat. Hello hello csatlakozópanel az eszköz a portok különböző adatok azonosítására.
   
    ![A 8100-as eszközön Csatlakozópanel](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Vissza az hello eszköz megjelenítő adatok portok**
   
   | Címke | Leírás |
   | --- | --- |
   |   0,1,4,5 |1 GbE hálózati adapterek |
   |   2,3 |10 GbE hálózati adapterek |
   |   6 |Soros portokhoz |
2. Tekintse meg a következő ábra a hálózati kábel hello. (kék folytonos vonal által hello minimális hálózati konfiguráció látható. A magas rendelkezésre állás és teljesítmény szükséges további konfigurációs látható szaggatott vonal.)

    ![A hálózati 2U bekábelezésére](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Az eszköz kábelek hálózati**

   |Címke | Leírás |
   |----- | ----------- |
   | A    | Internet-hozzáféréssel rendelkező LAN |
   | B    | A vezérlő 0 |
   | C#    | PCM 0 |
   | D    | 1. vezérlő |
   | E    | PCM 1 |
   | F, G | Hosts |
   | 0-5  | Hálózati illesztők |



Hello eszköz kábelek, hello minimális konfiguráció kell rendelkeznie:

* Legalább két hálózati adapterrel minden tartományvezérlőn, a felhő hozzáféréshez és az egyikben iSCSI csatlakoztatva. hello DATA 0 port automatikusan engedélyezve és konfigurálva hello a hello eszköz soros konzolon keresztül. DATA 0, leszámítva adatokat egy másik portra kell toobe hello klasszikus Azure portál segítségével konfigurálható. Ebben az esetben csatlakozni a DATA 0 port toohello elsődleges LAN (Internet-hozzáféréssel rendelkező hálózati). hello más adatok portok lehet kapcsolatban tooSAN/iSCSI LAN (VLAN) szegmens hello hálózat, attól függően, hogy szánt hello szerepkör.
* Minden egyes tartományvezérlő csatlakoztatva toohello azonos csatolóinak azonos hálózati tooensure rendelkezésre állási vezérlő feladatátvétel esetén. Például ha úgy dönt, hogy a DATA 0 tooconnect és a DATA 3 egy hello, tartományvezérlői, kell a DATA 0 és a DATA 3 megfelelő tooconnect hello hello más vezérlő.

Vegye figyelembe a magas rendelkezésre állás és teljesítmény:

* Ha lehetséges, állítsa be két hálózati csatoló a felhőelérést (1 gbe-s) és az iSCSI (ajánlott 10 GbE) egy másik pár minden tartományvezérlőn.
* Ha lehetséges, kapcsolódó hálózati adapterek minden tartományvezérlő tootwo különböző kapcsolókhoz tooensure rendelkezésre állás kapcsoló hibát. hello ábrázolható hello két 10 GbE hálózati illesztőket, DATA 2 és a DATA 3, minden egyes tartományvezérlő csatlakoztatva tootwo különböző kapcsolókhoz.

További információkért tekintse meg a toohello **hálózati illesztőt** alatt hello [a StorSimple eszköz követelményei magas rendelkezésre állású](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Ha SFP + adó az 10 GbE hálózati adaptereket használ, használjon hello megadott QSFP-SFP + adapterek. További információ: túl[támogatott hardveres hello 10 GbE hálózati adapterek a StorSimple eszköz](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Soros port kábelek
Hajtsa végre a következő lépéseket toocable hello a soros port.

#### <a name="toocable-for-serial-connection"></a>soros kapcsolat toocable
1. Az eszköz soros minden vezérlő egy csavarkulcsot ábrázoló ikon által azonosított-porttal rendelkezik. Tekintse meg a hello toohello ábra [hálózati kábeleket](#network-cabling) toolocate hello soros port az eszköz hello csatlakozópanel szakasz.
2. Az eszköz csatlakozópanel hello aktív vezérlő azonosításához. Egy villogó kék LED azt jelzi, hogy hello vezérlő aktív.
3. Használjon a megadott hello soros kábelek (ha szükséges, a laptop átalakítója hello USB – soros), és a konzol vagy a számítógép (eszközzel terminálemuláció toohello) toohello soros port hello aktív vezérlő.
4. Hello soros-USB-illesztőprogramot (hello eszközzel) telepítése a számítógépre.
5. Hello soros kapcsolat beállítása az alábbiak szerint: 115 200 átviteli, 8 adatbitek, 1 stop bit, nem a paritásos és adatfolyam vezérlés tooNone beállítása.
6. Ellenőrizze, hogy hello kapcsolat hello konzolon Enter billentyű megnyomásával működik. A soros konzol menü megjelenjen-e.

> [!NOTE]
> **Lights-Out felügyeleti**: Ha hello eszköz telepítve van távoli adatközpontban vagy korlátozott hozzáférésű számítógép helyiségben, gondoskodjon arról, hogy hello soros kapcsolatok tooboth tartományvezérlők mindig csatlakoztatott tooa soros konzol kapcsoló vagy hasonló berendezések. Ez lehetővé teszi sávon kívüli a távvezérlés és a támogatási műveleteket, ha hálózati hibákat vagy váratlan meghibásodások esetén.
> 
> 

Az eszköz most már bekábelezte a tápellátáshoz, a hálózati hozzáférés és a soros kapcsolat. következő lépés hello tooconfigure hello szoftver, és az eszköz üzembe helyezése.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[központi telepítése és konfigurálása a helyszíni StorSimple eszköz](storsimple-deployment-walkthrough-u2.md).

