---
title: "MyDriving Azure IoT-példa: első lépések |} Microsoft Docs"
description: "Ismerkedés az alkalmazást, a Microsoft Azure Stream Analytics, a Machine Learning és az Event Hubs használatával az IoT-rendszer tervezővel hogyan átfogó bemutatója."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT system: első lépések
MyDriving egy rendszer, amely bemutatja a megtervezését és megvalósítását egy tipikus [az eszközök internetes hálózatát](iot-suite-overview.md) (IoT) megoldás, amely telemetriai adatokat gyűjt a eszközöket, feldolgozza ezeket az adatokat a felhőben, és gépi tanulással adjon meg érvényes egy adaptív választ. A bemutató a car utazgatással kapcsolatos adatokat naplózza a mobiltelefonjára, mind a kiszolgáló, amely adatokat gyűjt a car ellenőrző rendszerből származó adatokkal. A más felhasználókkal szemben a vezetői style visszajelzést használja ezeket az adatokat.

A valós MyDriving célja a saját IoT-megoldás létrehozása a kezdéshez. De előtt, hogy jelentkezzen be a MyDriving maga az alkalmazás – a vizsgálat felhasználói csoport tagja lesz. Ez lehetővé teszi a számára az alkalmazás és a rendszer mögötte vásárlói, mielőtt jobban elmélyedne architektúrájának. Azt is bemutatja a Hockeyappra, egy ritkán használt adatok módszer az alkalmazások alpha és a béta disztribúciók kezelése felhasználók tesztelése.

## <a name="use-the-mobile-experience"></a>A mobil élmény használata
A MyDriving alkalmazást is használhatja, ha az Android, iOS vagy Windows 10-es eszköz.

### <a name="android-and-windows-10-mobile-installation"></a>Android és Windows 10 Mobile-telepítés
Az eszközön:

1. Fejlesztői alkalmazások engedélyezése:
   
   * Android: Az **beállítások** > **biztonsági**, engedélyezi, hogy az alkalmazások **ismeretlen források**.
   * Windows 10: Az **beállítások** > **frissítések** > **a fejlesztők**, beállíthatja **fejlesztői mód**.
2. A béta teszt csapat csatlakoznak, és regisztrál, vagy a bejelentkezés, [Hockeyappra](https://rink.hockeyapp.net). Hockeyappra megkönnyíti, hogy az alkalmazás tesztelése a felhasználók korai kiadásaiban terjesztéséhez.
   
   Windows 10 használata, használja az Edge böngészőben.
   
   Ha egy Build 2016 résztvevő, jelentkezzen be a ugyanazon Microsoft-fiókja e-mail regisztrálva a a konferencia használatával a Microsoft gombokra. Már regisztrálva van a Hockeyappra.
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
3. Töltse le és telepítse az alkalmazást innen:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Két elemek vannak. Telepítse a tanúsítványt **megbízható személyek**. Telepítse az alkalmazást.

*Probléma merül fel az alkalmazás indítása a Windows 10 Mobile?* A telefon lehet egy frissítést, vagy két mögött. Győződjön meg arról, hogy telepítve vannak-e a legújabb frissítéseket, vagy telepítse:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>iOS-telepítés
Ha a felügyelt Build 2016, töltse le az alkalmazást a Hockeyappra a teszt csapat tagjaként:

1. Az iOS-eszközön bejelentkezni [Hockeyappra](https://rink.hockeyapp.net).
   A Microsoft bejelentkezési gombok, és jelentkezzen be a ugyanazon Microsoft-fiókja e-mail a konferencia regisztrált egyikét használhatja. (Az e-mailek és a jelszó mező nem használható.)
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
2. A Hockeyappra irányítópulton MyDriving válassza ki, és töltse le azt.
3. A bétaverzió a Hockeyappra engedélyezése:
   
   a. Ugrás a **beállítások** > **általános** > **profilok és kezelése.**
   
   b. Megbízható a **Bit Stadium GmbH** tanúsítványt.

Ha a Build 2016 nem vették fel, létrehozhatja és telepítse az alkalmazást:

1. A kód letöltése [a Githubról].
2. Hozza létre és telepíthet [Xamarin segítségével].

A további részletekért a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Első OBD adapter (nem kötelező)
Ez az a része, amelyek miatt ez egy valódi az eszközök internetes hálózatát rendszer! Használhatja az alkalmazás nélkül, de valódi szórakozás, és kevésbé költséges.

A helyi diagnosztika (OBD) az a, amely a garázsnak használatával a car hangolása és páratlan zajnak vagy figyelmeztetés lámpa diagnosztizálja car szolgáltatása. Csak a car a kiváló régiségek, ha található valahol szoftvercsatorna kézi, általában egy flap alatt az irányítópult mögött. A jobb oldali összekötővel a motor teljesítmény metrikákat kaphat, és egyes módosításokat. Egy OBD összekötő olcsón lehet beszerezni a szokásos helyek. Egy alkalmazást a telefonra Bluetooth vagy Wi-Fi használatával tud csatlakozni.

Ebben az esetben lesz, a car csatlakozni a felhőhöz. A közvetlen kapcsolat az OBD a telefonjára, de az alkalmazás működik, mint egy továbbítót. A car telemetriai adatokat küld rögtön a MyDriving IoT-központ feldolgozza naplózni a közúti való adatváltások számát és a vezetői stílus értékeléséhez.

Csatlakoztassa az OBD eszközt:

1. Ellenőrizze, hogy rendelkezik-e a car egy OBD szoftvercsatorna.
2. Szerezzen be egy OBD adapter:
   
   * Ha egy Android- vagy Windows phone használata esetén szükséges egy Bluetooth-kompatibilis OBD II adapter. Használtuk [BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz].
   * Ha egy iOS-telefon használata esetén szükséges egy Wi-Fi-kompatibilis OBD adapter. Használtuk [ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó].
3. Kövesse az utasításokat, amelyek a OBD adapterhez csatlakozzanak a telefonjára. Vegye figyelembe a következőket:
   
   * A Bluetooth-adapter típusnak kell megfeleltetni a telefont, a a **beállítások** lap.
   * A Wi-Fi adapter az a tartomány 192.168.xxx.xxx címmel kell rendelkeznie.
4. Ha több autók, külön adapter kaphat mindegyik (legfeljebb három).

Ha egy OBD adapter nem rendelkezik, az alkalmazás lesz továbbra is adatokat küldeni a hely és sebességét, a telefon GPS fogadó a háttérben, és ekkor megkérdezi, hogy szeretné-e egy OBD szimulálásához.

Az található további az alkalmazás hogyan használja az adatok OBD adapteréről és beállítások létrehozásához a saját OBD eszközök szakaszban 2.1-es, "Az IoT-eszközök," a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Az alkalmazás használata
Indítsa el az alkalmazást. Egy kezdeti gyors üzembe helyezés hogyan működik lépésre van.

### <a name="track-your-trips"></a>Nyomon követheti a való adatváltások számát
Koppintson a rekord gombra (nagy piros kör a képernyő alján) egy út elindításához, és koppintson újra befejezéséhez.

![A rekord gombra nyomon követését utazás ábrája](./media/iot-solution-get-started/image2.png)

Utazás, minden egyes indításakor nincs OBD eszköz esetén meg kell adnia a szimulátor használni kívánt.

Egy út végén koppintson a Leállítás gombra, és összefoglaló információk.

![Egy összegző út – példa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Tekintse át a való adatváltások számát
![Egy korábbi út – példa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Tekintse át a profil
![A vezetés stílusú profil – példa](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Teszt visszajelzését
MyDriving ismertető segítségével létrehozott saját IoT rendszerek, mert biztosan szeretnénk megosztaná velünk, milyen jól működik kapcsolatban. Értesítsen minket, ha:

* Nehézségek vagy kihívásokkal futtatja.
* Van így könnyebben megfelelőbbek, adott esetben bővítmény pont.
* Egy sokkal hatékonyabb módja bizonyos kell látnia.
* Javaslatai bármely más MyDriving vagy ebben a dokumentációban javítására.

Az alkalmazásban MyDriving magát, használhatja a beépített Hockeyappra visszajelzés mechanizmus: iOS és Android rendszeren csak biztosítják a telefon egy rázó, vagy használja a **visszajelzés** menüparancshoz. Ez automatikusan elvégzi a képernyőfelvételen látható, hogy tudjuk lesz, mi, hogy van szó. És bármely kellemetlen összeomlás esetén Hockeyappra gyűjti a mondja el nekünk azokat-összeomlási naplókat. Visszajelzést keresztül is biztosíthat a [Hockeyappra portal].

Is fájl egy [problémát a Githubon], vagy hagyja meg az alábbi megjegyzést (en-us edition).

Kíváncsian nyújtanak segítséget az Ön!

## <a name="next-steps"></a>Következő lépések
* Megismerkedhet a [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) tudni, hogyan előre tervezett és a teljes MyDriving rendszer beépített.
* [Létrehozásakor és központi telepítésekor a rendszer a saját](iot-solution-build-system.md) az Azure Resource Manager-parancsfájlok használatával. A [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) is végigvezeti azokon a területeken, ahol lesz testreszabásokat a legtöbb.

[a Githubról]: https://github.com/Azure-Samples/MyDriving
[Xamarin segítségével]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz]: http://www.amazon.com/gp/product/B005NLQAHS
[ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[Hockeyappra portal]: https://rink.hockeyapp.org
[problémát a Githubon]: https://github.com/Azure-Samples/MyDriving/issues
