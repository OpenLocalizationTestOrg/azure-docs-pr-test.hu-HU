---
title: "aaaDevice-kezelés az Azure IoT Hub |} Microsoft Docs"
description: "Az Azure IoT Hub eszközfelügyeleti lehetőségeinek áttekintése: vállalati eszközök életciklusa és eszközfelügyeleti minták, mint például újraindítás, gyári beállítások visszaállítása, belső vezérlőprogram frissítése, konfigurálás, ikereszközök, lekérdezések, feladatok."
services: iot-hub
documentationcenter: 
author: bzurcher
manager: timlt
editor: 
ms.assetid: a367e715-55f6-4593-bd68-7863cbf0eb81
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: briz
ms.openlocfilehash: 7e22fb6eb3c541a513b16a047c7c3ef557255532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-device-management-with-iot-hub"></a>Az IoT Hub-eszközfelügyelet áttekintése
## <a name="introduction"></a>Bevezetés
Az Azure IoT Hub hello szolgáltatásokat és a olyan bővíthetőségi modell, amely engedélyezi az eszköz és a háttér-fejlesztők toobuild robusztus Eszközkezelési megoldásokkal nyújt. Korlátozott érzékelők és egycélú mikrovezérlők, toopowerful átjárók, hogy az útvonal-kommunikációt eszközökből álló csoportokat az eszközök közé.  Ezenkívül hello használati esetek és az IoT-operátorok követelményei eltérőek jelentősen iparágakban.  Annak ellenére, hogy ezt a különbséget Eszközkezelés IoT-központ a hello képességeit, és biztosít a minták kód szalagtárak toocater tooa érdekében számos eszközök és a végfelhasználók számára.

A sikeres vállalati IoT-megoldás létrehozása a kritikus fontosságú része tooprovide stratégia hogyan kezelik az operátorok a hello folyamatos kezelését az eszközök gyűjteménye. Az IoT-operátorok egyszerű és megbízható eszközök és alkalmazások, amelyek lehetővé teszik a hello toofocus azokat a feladatokat a további stratégiai aspektusainak van szükség. Ez a cikk a következő információkat tartalmazza:

* Azure IoT Hub megközelítés toodevice felügyeleti rövid áttekintést.
* Az általános eszközfelügyeleti alapelvek leírása.
* Eszközök életciklusa hello leírását.
* Az általános eszközfelügyeleti minták áttekintése.

## <a name="device-management-principles"></a>Az eszközfelügyelet alapelvei
IoT számos lehetőséget kínál vele egyedi készletének eszközt felügyeleti kihívást, és minden vállalati szintű megoldást a következő alapelveket hello kell venni:

![Az eszközfelügyelet alapelveinek ábrája][img-dm_principles]

* **Méretezés és automatizálás**: az IoT-megoldások igényel, amely rendszeres feladatok automatizálásához és viszonylag kis műveleteket egyszerű eszközök személyzeti toomanage több millió eszközök. Napi, operátorok várt toohandle eszköz műveletek távolról, egyszerre több, és tooonly riasztást a közvetlen figyelmet igénylő problémák merülhetnek fel.
* **Nyitottságának és kompatibilitási**: hello eszköz ökoszisztéma számította különböző. Felügyeleti eszközök a következőkhöz ideális tooaccommodate eszközosztályok, platformokról és protokollok számos kell lennie. Operátorok képesnek kell lennie a korlátozott számos különböző típusú eszközök, hello toosupport beágyazott single-folyamat processzorok, a toopowerful és a számítógépek teljesen működőképes.
* **A környezet figyelése**: az IoT-környezetek dinamikusak és állandóan változnak. A szolgáltatás megbízhatósága létfontosságú. Eszköz felügyeleti műveletek során tényezők tooensure a következő fiók hello kell venni, hogy a karbantartási állásidő nem befolyásolják a kritikus fontosságú üzleti működését és veszélyes feltételek létrehozása:
    * SLA karbantartási időszakok
    * A hálózat és a tápellátás állapota
    * Használatban lévő feltételek
    * Az eszköz földrajzi helyei
* **Sok szerepkörök szolgáltatás**: hello egyedi munkafolyamatok és a folyamatok IoT műveleti szerepkörök támogatás elengedhetetlen. hello műveleti személyzet harmonikus együttműködve megkötések miatt a belső informatikai részlegek hello.  Akkor is meg kell keresnie, fenntartható módon toosurface valós idejű eszköz műveletek információk toosupervisors és más üzleti vezetői szerepköröket.

## <a name="device-lifecycle"></a>Az eszközök életciklusa
Nincs olyan általános eszköz felügyeleti fázisokra, amelyek közös tooall vállalati IoT-projektek. Az Azure IoT öt szakaszból áll hello eszköz életciklusa belül:

![hello öt Azure IoT eszköz életciklusa fázisa: megtervezése, konfigurálta, konfigurálása, figyeléséhez, kivonása][img-device_lifecycle]

A öt szakaszok belül van több operátor követelmények, amelyeket teljesített tooprovide teljes körű megoldást:

* **Tervezze meg**: engedélyezze a operátorok toocreate egy eszköz metaadatok séma, amely lehetővé teszi őket tooeasily, és pontosan a lekérdezést, és a csoportot az eszközök tömeges felügyeleti műveleteihez. Hello eszköz iker toostore használhatja az eszköz metaadatait címkék és a Tulajdonságok hello formájában.
  
    *A további olvasási*: [Ismerkedés az eszköz twins][lnk-twins-getstarted], [eszköz twins ismertetése][lnk-twins-devguide], [hogyan a két eszköztulajdonságok toouse][lnk-twin-properties].
* **Kiépítés**: biztonságosan új eszközök tooIoT Hub és engedélyezése operátorok tooimmediately felderítése eszközképességek kiépítéséhez.  Hello IoT-központ identitás beállításjegyzék toocreate rugalmas eszköz identitások és a hitelesítő adatokat használja, és ehhez a művelethez egyszerre egy feladat használatával. Build eszközök tooreport a képességek és a feltételek a hello eszköz iker eszköztulajdonságok keresztül.
  
    *A további olvasási*: [eszköz Identitáskezelést][lnk-identity-registry], [felügyeleti eszköz identitások tömeges][lnk-bulk-identity], [Hogyan toouse eszköz iker tulajdonságok][lnk-twin-properties].
* **Konfigurálása**: tömeges megkönnyítése konfigurációs módosítások és a belső vezérlőprogram frissítése toodevices rendszerállapot és a biztonság megőrzésével. A kívánt tulajdonságok, illetve közvetlen módszerek és szórásos feladatok használatával ezek az eszközfelügyeleti műveletek tömegesen is végrehajthatók.
  
    *A további olvasási*: [közvetlen módszerekkel][lnk-c2d-methods], [meghívni egy közvetlen eszköz metódust][lnk-methods-devguide], [hogyan a két eszköztulajdonságok toouse][lnk-twin-properties], [ütemezés és a szórásos feladatok][lnk-jobs], [több eszközönfeladatokütemezése] [lnk-jobs-devguide].
* **A figyelő**: eszközök összesített állapota, folyamatban lévő műveletek, és a figyelmet igénylő riasztási operátorok tooissues hello állapotának figyelése.  Hello eszköz iker tooallow eszközök tooreport valós idejű működési feltételek és a frissítési műveletek állapotának vonatkoznak. Build hatékony irányítópult legtöbb azonnali probléma jelentések adott felület hello eszköz iker lekérdezések használatával.
  
    *A további olvasási*: [hogyan toouse eszköz iker tulajdonságok][lnk-twin-properties], [IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás] [ lnk-query-language].
* **Kivonás**: cseréje vagy eszközök leszerelése meghibásodás után, frissítse a ciklus, vagy hello szolgáltatás élettartama hello végén.  Hello eszköz iker toomaintain eszközök adatait használja, ha folyamatban van a hello fizikai eszköz helyett, vagy ha hatókörről archivált. Használja az IoT-központ identitásjegyzékhez hello biztonságosan visszavonásával eszköz identitások és a hitelesítő adatokat.
  
    *A további olvasási*: [hogyan toouse eszköz iker tulajdonságok][lnk-twin-properties], [eszköz Identitáskezelést][lnk-identity-registry].

## <a name="device-management-patterns"></a>Eszközfelügyeleti minták
Az IoT-központ lehetővé teszi, hogy a következő eszköz felügyeleti minták készlete hello.  Hello [eszköz felügyeleti oktatóanyagok] [ lnk-get-started] mutatja be részletesen tooextend ezek kombinációját toofit a pontos forgatókönyv, és hogyan toodesign új minták alapján ezek alapvető sablonok.

* **Újraindítás** -hello háttér-alkalmazás tájékoztatja hello eszköz közvetlen módszerrel, hogy újraindítást kezdeményezett.  hello eszköz által használt hello jelentett tulajdonságok tooupdate hello újraindítás hello eszköz állapotát.
  
    ![Az eszközfelügyelet újraindítási mintájának ábrája][img-reboot_pattern]
* **Gyári beállítások visszaállítása** -hello háttér-alkalmazás tájékoztatja hello eszköz közvetlen módszerrel, hogy már kezdeményezte a gyári beállításainak visszaállítását.  hello eszköz által használt hello jelentett tulajdonságok tooupdate hello gyári hello eszköz állapotát.
  
    ![Az eszközfelügyelet eszköz gyári alaphelyzetbe állítási mintájának ábrája][img-facreset_pattern]
* **Konfigurációs** -hello háttér-alkalmazás szükséges hello tulajdonságok tooconfigure szoftver hello eszközön futó használ.  hello eszköz által használt hello jelentett hello eszköz tulajdonságok tooupdate konfigurációs állapotát.
  
    ![Az eszközfelügyelet konfigurációs mintájának ábrája][img-config_pattern]
* **Belső vezérlőprogram frissítési** -hello háttér-alkalmazás tájékoztatja hello eszköz közvetlen módszerrel, hogy már kezdeményezte a belső vezérlőprogram-frissítés.  hello eszköz kezdeményezi a többlépéses folyamatot toodownload hello belső vezérlőprogram-lemezkép, hello belső vezérlőprogram lemezképének alkalmazása, és végül újracsatlakozás toohello IoT-központ szolgáltatás.  Teljes hello többlépéses folyamatot a hello eszköz által használt hello jelentett tulajdonságok tooupdate hello folyamatát és hello eszköz állapotát.
  
    ![Az eszközfelügyelet belsővezérlőprogram-frissítési mintájának ábrája][img-fwupdate_pattern]
* **Jelentéskészítési folyamat- és állapot** -hello megoldás háttérrendszerének futtató eszköz iker lekérdezések, azokat az eszközöket, tooreport hello állapotát és előrehaladását hello eszközökön futó műveletek között.
  
    ![Az eszközfelügyelet előrehaladási és állapotmeghatározási jelentéskészítési mintájának ábrája][img-report_progress_pattern]

## <a name="next-steps"></a>Következő lépések
hello képességeit, a mintákat és a kód tárak eszközkezelés, biztosít az IoT-központ lehetővé teszik, amelyek megfelelnek a vállalati IoT operátor követelményeinek belül minden eszköz életciklusfázis toocreate IoT-alkalmazásokhoz.

IoT Hub-hello eszköz felügyeleti funkciókat megtanulni toocontinue lásd: hello [Ismerkedés az eszközkezelés] [ lnk-get-started] oktatóanyag.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-node-node-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md
