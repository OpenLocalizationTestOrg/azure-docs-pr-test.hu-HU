---
title: "aaaTurn a StorSimple 8000 series eszköz be- és kikapcsolása |} Microsoft Docs"
description: "Hogyan tooturn egy új StorSimple eszközön leállt vagy megszakadt power eszköz bekapcsolása, és tiltsa le a futó eszközt mutatja be."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 8e9c6e6c-965c-4a81-81bd-e1c523a14c82
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85434bde9377e330cd6ba4797fd5fd68bcee944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="turn-on-or-turn-off-your-storsimple-8000-series-device"></a>Kapcsolja be, vagy kapcsolja ki a StorSimple 8000 series eszköz
## <a name="overview"></a>Áttekintés
A Microsoft Azure StorSimple eszköz leállítása nincs szükség a szokásos működésének részeként. Egy új vagy egy eszköz, amelynek toobe állítsa le a tooturn azonban szükség lehet. Általában a leállás szükséges azokban az esetekben, ahol le kell cserélnie sikertelen hardver, fizikailag helyezze át egy egység, vagy igénybe vehet egy eszköz nem működik. Ez az oktatóanyag leírja a szükséges hello eljárás bekapcsolása és a Leállítás fázisában a StorSimple eszköz különböző helyzetekben.

## <a name="turn-on-a-new-device"></a>Új eszköz bekapcsolása
hello először hello StorSimple eszköz bekapcsolása lépései attól függenek, hello eszköz-e egy 8100 vagy egy 8600 modell. hello 8100 rendelkezik egy önálló elsődleges ház, mivel hello 8600 rendelkező elsődleges ház és egy EBOD ház kettős-ház eszköz. hello mind a két modellben részletes lépéseket lásd: a következő szakaszok hello.

* [Új eszköz csak az elsődleges ház](#new-device-with-primary-enclosure-only)
* [Új eszköz EBOD ház](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Új eszköz csak az elsődleges ház
a StorSimple 8100 hello modell a ház egyetlen eszközről szó. Az eszköz redundáns energia- és hűtési modulok (PCMs) tartalmazza. Mindkét PCMs toodifferent power források tooensure magas rendelkezésre állású csatlakoztatva, és telepítve kell lennie.

Végezze el a következő lépéseket toocable hello az eszköz power.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

> [!NOTE]
> Az eszköz telepítésének befejezése és kábelek mennyiségét utasításokat, nyissa meg túl[a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md). Ellenőrizze, hogy hello utasításokat pontosan kövesse.
> 
> 

### <a name="new-device-with-ebod-enclosure"></a>Új eszköz EBOD ház
egy elsődleges ház- és egy EBOD ház hello StorSimple 8600 modellnek. Ehhez a hello egységek toobe kábel együtt soros csatlakozású SCSI (SAS) kapcsolódási és a teljesítmény.

Beállítása hello ezen az eszközön első alkalommal, amikor lépésekkel hello SAS először kábelek és majd a teljes hello lépései power kábelek.

[!INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

> [!NOTE]
> Az eszköz telepítésének befejezése és kábelek mennyiségét utasításokat, nyissa meg túl[a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md). Ellenőrizze, hogy hello utasításokat pontosan kövesse.

## <a name="turn-on-a-device-after-shutdown"></a>Egy eszköz bekapcsolása leállást követően
után le lett állítva a StorSimple eszköz bekapcsolása hello lépései eltérőek attól függően, hogy hello eszköz-e egy 8100 vagy egy 8600 modellt. hello 8100 rendelkezik egy önálló elsődleges ház, mivel hello 8600 rendelkező elsődleges ház és egy EBOD ház kettős-ház eszköz.

* [Csak az elsődleges ház eszköz](#device-with-primary-enclosure-only)
* [EBOD ház eszköz](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Csak az elsődleges ház eszköz
A leállás után eljárás tooturn elsődleges ház és nem EBOD ház egy StorSimple eszközön a következő hello használata.

#### <a name="tooturn-on-a-device-with-a-primary-enclosure-only"></a>egy eszközön csak egy elsődleges ház tooturn
1. Ellenőrizze, hogy hello power vált is működik, és hűtési modulok (PCMs) hello OFF állásban van. Ha hello kapcsolók nincsenek hello OFF állásban, majd azokat toohello OFF pozíciója és tükrözése hello fény toogo Várakozás ki.
2. Kapcsolja be a hello eszköz által hello power kapcsolók mindkét PCMs toohello ON pozíciója a tükrözés. hello eszköz kapcsolja be.
3. Teljes mértékben be kapcsolva, amely az eszköz hello tooverify a következő ellenőrzés hello:
   
   1. hello OK LED mindkét PCM modulokat a rendszer zöld.
   2. hello állapot LED mindkét tartományvezérlőn teli zöld.
   3. hello, tartományvezérlői egyik kék LED van villogó, hello amely azt jelzi, hogy hello vezérlő aktív.
      
      Ha ezek a feltételek nem teljesülnek, majd az eszköz nincs kifogástalan állapotban. Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>EBOD ház eszköz
A leállás után eljárás tooturn elsődleges ház és egy EBOD ház egy StorSimple eszközön a következő hello használata. Pontosan leírt sorrendben végrehajtani a műveletet. Hiba toodo vezetne adatvesztés.

#### <a name="tooturn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>egy elsődleges és egy EBOD ház eszközön tooturn
1. Győződjön meg arról, hogy hello EBOD ház csatlakoztatott toohello elsődleges ház. További információkért lásd: [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).
2. Győződjön meg arról, hogy hello Power és hűtési modulok (PCMs) mindkét hello EBOD és elsődleges ház hello OFF állásban van. Ha hello kapcsolók nincsenek hello OFF állásban, majd azokat toohello OFF pozíciója és tükrözése hello fény toogo Várakozás ki.
3. Kapcsolja be a hello EBOD ház első mindkét PCMs toohello ON pozíciója a hello power kapcsolók átállításával. hello PCM LED zöld kell lennie. Ez egységen egy zöld EBOD vezérlő LED azt jelzi, hogy hello EBOD ház meg.
4. Kapcsolja be a hello elsődleges ház mindkét PCMs toohello ON pozíciója a hello power kapcsolók átállításával. hello teljes rendszer most kell lennie.
5. Győződjön meg arról, hogy hello SAS LED zöld, amely biztosítja, hogy hello kapcsolat hello EBOD ház között, és hello elsődleges ház helyes.

## <a name="turn-on-a-device-after-a-power-loss"></a>Egy eszköz bekapcsolása után egy áramellátás megszakadása miatt
A StorSimple eszköz egy áramkimaradás vagy megszakítás leállíthat. hello áramforrások egyik vagy mindkét áramforrások hello áramkimaradás esetén fordulhat elő. hello helyreállítási lépések eltérőek attól függően, hogy hello eszköz-e egy 8100 vagy egy 8600 modellt. hello 8100 rendelkezik egy önálló elsődleges ház, mivel hello 8600 rendelkező elsődleges ház és egy EBOD ház kettős-ház eszköz. Ez a szakasz ismerteti az egyes forgatókönyvek esetében hello helyreállítási folyamatot.

* [Csak az elsődleges ház eszköz](#8100)
* [EBOD ház eszköz](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Csak az elsődleges ház eszköz<a name="8100">
hello rendszer továbbra is a normál működés, ha nincs energiagazdálkodási adatvesztés tooone a áramforrással rendelkezik. Azonban tooensure magas rendelkezésre állású hello eszköz, visszaállítási power toohello tápegység lehető legrövidebb időn belül.

Ha egy áramkimaradás vagy mindkét áramforrással rendelkezik power szolgáltatásainak megszakítása nélkül, hello rendszer le fog állni egy szabályos és szabályozott módon. Hello power helyreáll, amikor a rendszer hello automatikusan bekapcsolása.

### <a name="device-with-ebod-enclosure-a-name8600"></a>EBOD ház eszköz<a name="8600">
#### <a name="power-loss-on-one-power-supply"></a>Adjon egy működik az áramellátás megszakadása miatt
hello rendszer továbbra is a normál működés, ha nincs energiagazdálkodási adatvesztés tooone hello elsődleges ház vagy hello EBOD ház a áramforrással rendelkezik. Azonban tooensure magas rendelkezésre állású hello eszköz, állítsa vissza a power toohello tápegység lehető legrövidebb időn belül.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Az elsődleges és EBOD ház mindkét áramforrással rendelkezik az áramellátás megszakadása miatt
Ha egy energiagazdálkodási kimaradás vagy power szolgáltatásainak megszakítása nélkül mindkét áramforrással rendelkezik, hello EBOD ház azonnal leáll, és hello elsődleges ház rendezett és szabályozott módon le fog állni. Energiagazdálkodási visszaállításakor hello készülék automatikusan elindul.

Ha hello power manuálisan kapcsolható ki, majd hajtsa végre a következő lépéseket toorestore power toohello rendszer hello.

1. Kapcsolja be a hello EBOD ház.
2. Miután hello EBOD ház a, kapcsolja be a hello elsődleges ház.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>A mindkét áramforrással rendelkezik, a EBOD ház az áramellátás megszakadása miatt
A kábelek beállításakor győződjön meg arról, hogy hello EBOD van soha nem csatlakozott önmagában tooa PDU külön. Ha hello EBOD és elsődleges ház sikertelen volt hello hello rendszer egy időben állítja helyre.

Ha csak hello EBOD ház mindkét áramforrások nem sikerül, a rendszer hello nem automatikusan helyre fog. Követő lépéseket tooturn hello rendszeren hello igénybe vehet, majd állítsa vissza azt tooa megfelelő állapothoz:

1. Ha hello elsődleges ház be van kapcsolva, kapcsolja ki a teljesítmény és a hűtési modulok (PCMs).
2. Várjon, amíg hello rendszer tooshut le néhány percig.
3. Kapcsolja be a hello EBOD ház.
4. Miután hello EBOD ház a, kapcsolja be a hello elsődleges ház.

## <a name="turn-on-a-device-after-hello-primary-and-ebod-enclosure-connection-is-lost"></a>Eszköz engedélyezése után hello elsődleges és EBOD ház kapcsolat elvész.
E hello hello készenléti vezérlő és hello megfelelő EBOD vezérlő között, a hello eszköz toowork továbbra is. Hello kapcsolatot az aktív vezérlő hello és hello megfelelő EBOD vezérlő nem vesztek el, ha feladatátvételi jöjjön létre, és hello eszköz továbbra is toowork szokásos módon.

Ha mindkét soros csatlakozású SCSI (SAS) kábelek törlődnek, vagy hello kapcsolat hello EBOD ház és elsődleges hello ház között van daraboltak, hello eszköz nem fog működni. Ezen a ponton hajtsa végre a lépéseket követve hello.

### <a name="tooturn-on-hello-device-after-connection-is-lost"></a>a kapcsolat megszakadása után hello eszköz tooturn
1. Hozzáférés hello hello eszköz oldalán.
2. Hello SAS kábelkapcsolat hello EBOD ház és elsődleges hello ház között megszakad, ha minden SAS lane LED hello EBOD ház lesz.
3. Állítsa le a teljesítmény és a hűtési modulok (PCMs) hello EBOD ház és elsődleges ház hello.
4. Várjon, amíg az összes hello LED-hátsó mindkét hello ház hello kikapcsolása.
5. Hello SAS-kábel be újra, és győződjön meg arról, hogy van-e a jó kapcsolat hello EBOD ház és elsődleges hello ház között.
6. Kapcsolja be a hello EBOD ház első által tükrözés mindkét PCM kapcsolók toohello ON pozícióját.
7. Gondoskodjon arról, hogy hello EBOD ház úgy, hogy zöld hello LED ON értékre állítva.
8. Kapcsolja be a hello elsődleges ház.
9. Gondoskodjon arról, hogy hello elsődleges ház által ellenőrzi, hogy hello vezérlő zöld LED-jét.
10. Győződjön meg arról, hogy hello EBOD ház kapcsolatot hello elsődleges ház jó úgy, hogy hello SAS lane (négy EBOD vezérlőnként) LED-e minden a.

> [!IMPORTANT]
> Ha hello SAS-kábel hibás vagy hello kapcsolat hello EBOD ház és elsődleges hello ház között nem helyes, a rendszer hello bekapcsolásakor, helyreállítási módba kerül. Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) Ha ez történik.


## <a name="turn-off-a-running-device"></a>Egy futó eszközök kikapcsolása
Előfordulhat, hogy fut a StorSimple eszköz toobe áll le, ha áthelyezik, nem működik, venni, vagy egy hibás összetevő, amelyet a cseréje toobe rendelkezik. hello lépései eltérőek attól függően, hogy hello StorSimple eszköz-e egy 8100 vagy egy 8600 modellt. hello 8100 rendelkezik egy önálló elsődleges ház, mivel hello 8600 rendelkező elsődleges ház és egy EBOD ház kettős-ház eszköz. Ez a szakasz részletesen hello lépéseket tooshut le a futó eszközt.

* [Elsődleges ház eszköz](#8100a)
* [EBOD ház eszköz](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Elsődleges ház eszköz<a name="8100a">
tooshut le szabályosan és szabályozott módon hello eszközt, ezt megteheti hello a klasszikus Azure portálon keresztül hello Windows PowerShell vagy a StorSimple. 

> [!IMPORTANT]
> Nem állnak le a futó eszköz hello főkapcsoló hello hátsó hello eszköz használatával.
> 
> Hello eszköz leállítása, előtt ellenőrizze, hogy a hello eszköz összetevők kifogástalan. A klasszikus Azure portálon hello, keresse meg a túl**eszközök** > **karbantartási** > **hardverállapot**, és ellenőrizze, hogy minden hello állapotát összetevők zöld. Ez igaz csak egy kifogástalan rendszer. Ha hello rendszer tooreplace hibás összetevő leállítása lesznek, megjelenik egy sikertelen (piros), vagy a csökkentett teljesítményű hello megfelelő összetevő hello (sárga) állapotát **hardverállapot**.
> 
> 

Ha hozzáfér a Windows PowerShell hello StorSimple vagy a klasszikus Azure portálon hello, kövesse hello [állítsa le a StorSimple eszköz](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>EBOD ház eszköz<a name="8600a">
> [!IMPORTANT]
> Hello elsődleges ház és hello EBOD ház leállítása előtt győződjön meg arról, hogy hello eszköz összetevők kifogástalan. Az Azure-portálon hello, keresse meg a túl**eszközök** > **figyelő** > **hardver állapotának**, és ellenőrizze, hogy az összes hello összetevő kifogástalan.


#### <a name="tooshut-down-a-running-device-with-ebod-enclosure"></a>tooshut le EBOD ház futó eszközök
1. Kövesse a felsorolt lépéseket hello [állítsa le a StorSimple eszköz](storsimple-8000-manage-device-controller.md#shut-down-a-storsimple-device) a hello elsődleges ház.
2. Hello után elsődleges ház leáll, az állítja le hello EBOD tükrözés, energia- és hűtési modul (PCM) kapcsolók ki.
3. amely hello EBOD tooverify le lett állítva, ellenőrizze, hogy az összes fények a hello hello EBOD ház oldalán ki van kapcsolva.

> [!NOTE]
> hello SAS-kábel, amelyek használt tooconnect hello EBOD ház toohello elsődleges ház tilos megvonni amíg hello rendszerének leállása után.

## <a name="next-steps"></a>Következő lépések
[Forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) Ha problémába ütközik a bekapcsolását, vagy a StorSimple eszköz leállítása.

