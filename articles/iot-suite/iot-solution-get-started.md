---
title: "MyDriving Azure IoT-példa: első lépések |} Microsoft Docs"
description: "Ismerkedés az alkalmazást, hogyan átfogó bemutatója tooarchitect az IoT-rendszer Microsoft Azure Stream Analytics, a Machine Learning és az Event Hubs használatával."
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
ms.openlocfilehash: 411b9a992deb22b915f8291d8559e2917d976b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT system: első lépések
MyDriving egy rendszer, amely bemutatja a hello tervezési és megvalósítási egy tipikus [az eszközök internetes hálózatát](iot-suite-overview.md) (IoT) megoldás, amely telemetriai adatokat gyűjt a eszközöket, feldolgozza ezeket az adatokat hello felhőben, és alkalmazza a gépi tanulás tooprovide egy adaptív választ. hello bemutató a car utazgatással kapcsolatos adatokat naplózza a mobiltelefonjára, mind a kiszolgáló, amely adatokat gyűjt a car ellenőrző rendszerből származó adatokkal. Az adatok tooprovide visszajelzés a vezetői stílus összehasonlító tooother felhasználók használ.

hello valós MyDriving célja tooget létrehozni a saját IoT-megoldás-t elindította. De előtt, hogy jelentkezzen be a hello MyDriving alkalmazás – a vizsgálat felhasználói csoport tagja lesz. Ez lehetővé teszi egy élmény hello alkalmazás és hello rendszer mögötte vásárlói, mielőtt jobban elmélyedne hello architektúra. Azt is bemutatja a tooHockeyApp, a ritkán használt adatok módja hello alpha és a béta disztribúciók a alkalmazások tootest felhasználók kezelése.

## <a name="use-hello-mobile-experience"></a>Mobil élmény hello használata
Hello MyDriving alkalmazást is használhatja, ha az Android, iOS vagy Windows 10-es eszköz.

### <a name="android-and-windows-10-mobile-installation"></a>Android és Windows 10 Mobile-telepítés
Az eszközön:

1. Fejlesztői alkalmazások engedélyezése:
   
   * Android: Az **beállítások** > **biztonsági**, engedélyezi, hogy az alkalmazások **ismeretlen források**.
   * Windows 10: Az **beállítások** > **frissítések** > **a fejlesztők**, beállíthatja **fejlesztői mód**.
2. A béta teszt csapat csatlakoznak, és regisztrál, vagy a bejelentkezés, [Hockeyappra](https://rink.hockeyapp.net). Hockeyappra lehetővé teszi a tootest felhasználók könnyen toodistribute korai kiadásaiban.
   
   Ha Windows 10 használata esetén használja a hello Edge böngésző.
   
   Ha egy Build 2016 résztvevő, jelentkezzen be hello azonos hello Microsoft gombok segítségével regisztrált hello konferencia, a Microsoft-fiókja e-mail. Már regisztrálva van a Hockeyappra.
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
3. Alkalmazás letöltéséhez és telepítéséhez hello innen:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Két elemek vannak. Telepítse a hello **megbízható személyek**. Majd telepítse hello alkalmazást.

*Probléma merül fel hello alkalmazás indítása a Windows 10 Mobile?* A telefon lehet egy frissítést, vagy két mögött. Győződjön meg arról, hello legújabb frissítéseinek van, vagy telepítse:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>iOS-telepítés
Ha a felügyelt Build 2016, töltse le a hello alkalmazást a Hockeyappra a teszt csapat tagjaként:

1. IOS-eszközön, jelentkezzen be túl[Hockeyappra](https://rink.hockeyapp.net).
   Ugyanazon Microsoft-fiókja e-mail hello konferencia regisztrált hello hello Microsoft bejelentkezési gombok, és jelentkezzen be az egyik használható. (A hello e-mailek és a jelszó mező nem használható.)
   
   ![Hockeyappra bejelentkezési képernyő](./media/iot-solution-get-started/image1.png)
2. Hello Hockeyappra irányítópulton válassza ki a MyDriving, és le is.
3. Engedélyezze a hello bétaverziót a Hockeyappra:
   
   a. Nyissa meg túl**beállítások** > **általános** > **profilok és kezelése.**
   
   b. Megbízható hello **Bit Stadium GmbH** tanúsítványt.

Ha a Build 2016 nem vették fel, létrehozhatja és saját kezűleg hello alkalmazás telepítése:

1. Töltse le a hello kód [a Githubról].
2. Hozza létre és telepíthet [Xamarin segítségével].

További részletekért található hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Első OBD adapter (nem kötelező)
Ez az hello része, amelyek miatt ez egy valódi az eszközök internetes hálózatát rendszer! Hello alkalmazással nélkül, de Szórakozás hello valós dolog, és kevésbé költséges.

A helyi diagnosztika (OBD) a hello garázs használ tootune fel a car és páratlan zajnak vagy figyelmeztetés lámpa diagnosztizálja car hello funkciója. Csak a car a kiváló régiségek, ha található valahol szoftvercsatorna hello kézi, általában egy flap alatt hello irányítópult mögött. Hello jobb összekötővel hello motor teljesítmény metrikákat kaphat, és egyes módosításokat. Egy OBD összekötő olcsón megvásárolható hello szokásos helyről. A telefonján Bluetooth vagy Wi-Fi tooan alkalmazás használatával tud csatlakozni.

Ebben az esetben az oktatóanyagban módosítjuk tooconnect a car toohello felhő. közvetlen kapcsolatot hello hello OBD tooyour telefon, de az alkalmazás működik, mint egy továbbítót. A car telemetriai küldött egyenes toohello MyDriving IoT-központot, ahol feldolgozásra toolog a közúti való adatváltások számát, és a vezetői stílus értékeléséhez.

egy OBD eszköz tooconnect:

1. Ellenőrizze, hogy rendelkezik-e a car egy OBD szoftvercsatorna.
2. Szerezzen be egy OBD adapter:
   
   * Ha egy Android- vagy Windows phone használata esetén szükséges egy Bluetooth-kompatibilis OBD II adapter. Használtuk [BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz].
   * Ha egy iOS-telefon használata esetén szükséges egy Wi-Fi-kompatibilis OBD adapter. Használtuk [ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó].
3. Hajtsa végre a hello vonatkozó utasításokat a OBD adapter tooconnect azt tooyour phone. Vegye figyelembe a következőket hello:
   
   * A Bluetooth-adapter típusnak kell megfeleltetni hello telefon, a hello **beállítások** lap.
   * A Wi-Fi adapter a hello tartomány 192.168.xxx.xxx címmel kell rendelkeznie.
4. Ha több autók, külön adapter kaphat mindegyik (legfeljebb három).

Ha egy OBD adapter nem rendelkezik, hello alkalmazás továbbra is küldi a hely és a hello phone GPS fogadó toohello vissza sebességű adatok befejezését, majd fogja kérni, ha azt szeretné, egy OBD toosimulate.

Az hello található további hogyan hello alkalmazása használja-e a hello OBD adapter származó adatok és beállítások létrehozásához a saját OBD eszközök szakaszban 2.1-es, "Az IoT-eszközök," [MyDriving használati útmutató](http://aka.ms/mydrivingdocs).

## <a name="use-hello-app"></a>Hello alkalmazás használata
Hello alkalmazás indításához. Egy kezdeti gyors üzembe helyezés toowalk van annak működéséről nyújt.

### <a name="track-your-trips"></a>Nyomon követheti a való adatváltások számát
Koppintson a hello rekord gombra (nagy piros kör üdvözlő képernyőt hello alján) toostart egy út, és koppintson újra tooend.

![Hello rekord gombra nyomon követését utazás ábrája](./media/iot-solution-get-started/image2.png)

Egy út minden egyes indításakor nincs OBD eszköz esetén kéri toouse hello szimulátor gombra.

Egy út hello végén koppintson a hello Leállítás gombra, és összefoglaló információk.

![Egy összegző út – példa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Tekintse át a való adatváltások számát
![Egy korábbi út – példa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Tekintse át a profil
![A vezetés stílusú profil – példa](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Teszt visszajelzését
A saját IoT rendszerek létrehozott MyDriving toohelp ismertető, mert biztosan szeretnénk toohear az Ön kapcsolatos mennyire működik. Értesítsen minket, ha:

* Nehézségek vagy kihívásokkal futtatja.
* Van így könnyebben megfelelőbbek tooyour forgatókönyv bővítmény pont.
* Egy sokkal hatékonyabb módja tooaccomplish található egyes igényeinek.
* Javaslatai bármely más MyDriving vagy ebben a dokumentációban javítására.

Hello MyDriving App magát, használhatja a hello beépített Hockeyappra visszajelzés mechanizmus: iOS és Android rendszeren csak biztosítják a telefon egy rázó, vagy használjon hello **visszajelzés** menüparancshoz. Ez automatikusan elvégzi a képernyőfelvételen látható, hogy tudjuk lesz, mi, hogy van szó. És bármely kellemetlen összeomlás esetén Hockeyappra gyűjt hello összeomlási naplókat tootell velünk róluk. Is adhat a visszajelzést keresztül hello [Hockeyappra portal].

Is fájl egy [problémát a Githubon], vagy hagyja meg az alábbi megjegyzést (en-us edition).

Számítunk toohearing az Ön!

## <a name="next-steps"></a>Következő lépések
* Fedezze fel hello [MyDriving a referencia-útmutató](http://aka.ms/mydrivingdocs) toounderstand hogyan előre tervezett és a beépített hello teljes MyDriving rendszer.
* [Létrehozásakor és központi telepítésekor a rendszer a saját](iot-solution-build-system.md) az Azure Resource Manager-parancsfájlok használatával. Hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs) is végigvezeti azokon a területeken, ahol lesz testreszabásokat hello legtöbb.

[a Githubról]: https://github.com/Azure-Samples/MyDriving
[Xamarin segítségével]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX termékek 34t5 Bluetooth OBDII vizsgálati eszköz]: http://www.amazon.com/gp/product/B005NLQAHS
[ScanTool OBDLink MX Wi-Fi: OBD Adapter/diagnosztikai képolvasó]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[Hockeyappra portal]: https://rink.hockeyapp.org
[problémát a Githubon]: https://github.com/Azure-Samples/MyDriving/issues
