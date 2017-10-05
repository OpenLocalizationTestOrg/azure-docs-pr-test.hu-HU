---
title: "Raspberry Pi (C) csatlakozzon az Azure IoT - hárítsa el a |} Microsoft Docs"
description: "Hibaelhárítás lap málna Pi Node.js élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 3653c79b-8a0c-41d4-b0bf-f6b4edb4d233
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 828669db23fa8d608029134fbe364033456d935a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Az alkalmazás futtatása is, de a LED nem villogó.
A probléma mindig kapcsolódik, a hardver expressroute kapcsolattal. Az alábbi lépések segítségével azonosíthatja a problémákat.

1. Ellenőrizze, hogy úgy döntött, hogy a helyes **GPIO** a táblán. A két portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.
2. Ellenőrizze, hogy helyesen-e a LED polaritásának. A hosszabb engedélyezi kell jeleznie a **pozitív**, anód PIN-kódot.
3. Használja a **3.3V PIN-kód** és **GND PIN-kód** málna a Pi a 3. A tartományvezérlő power Pi kezelje. Ellenőrizze, hogy jól működik a LED-jét.

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Más hardverekkel kapcsolatos problémák szerepelnek
Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget. Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz-felderítési problémák
A gyakori problémák elhárításában súgó a `devdisco` parancs, ellenőrizze a [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>NPM problémák
Próbálja meg frissíteni az NPM-csomagot a következő parancsot:

```bash
npm install -g npm
```

Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Távoli hibakeresés

Távoli hibaelhárítási támogatás a Visual Studio Code C/C++ bővítmény hamarosan elérhető lesz.

Egy meanwhile GDB használhat a kedvenc SSH terminál keresztül:

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a>Azure-CLI problémák
Az Azure parancssori felület (CLI) preview build. Keresse meg a megoldást a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) megoldások keresése. Próbálja meg az Azure-cli parancsok nem a várt módon működik legújabb verziójára történő frissítés.

Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.

A gyakori problémák elhárításához tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).

Ha megfelel a "Egy olyan verzióra, amely eleget tesz a követelmény nem található", futtassa a következő parancsot a pip frissítése a legújabb verzióra.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Python telepítési problémák
### <a name="legacy-installation-issues-macos"></a>A hagyományos telepítési problémák (macOS)
Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek. Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva. Néhány **pip** a korábbi telepítés csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba. A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat. Ez a feladat befejezéséhez tegye a következőket:

1. Ugrás: /usr/local/lib/python2.7/site-packages
2. Csomagok létrehozása a legfelső szintű:`ls -l | grep root`
3. A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`
4. Telepítse újra a Python.

## <a name="azure-iot-hub-issues"></a>Az Azure IoT Hub-problémák
Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és meg kell-e olyan eszköz, amely az IoT hub csatlakozó eszközök kezelésére, és próbálkozzon az alábbi eszközöket:

### <a name="device-explorer"></a>Eszköz Explorer
[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik. A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):

* *Eszköz Identitáskezelés* szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.
* *Eszköz-felhő kap* , figyelheti az IoT hub az eszközről küldött üzeneteket.
* *Felhő eszközre küldése* úgy küldhet üzeneteket az eszközök az IoT hub a.

Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.

### <a name="iot-hub-explorer"></a>Az IoT-központ Explorer
[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére. Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.

Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:

```
npm install -g iothub-explorer@latest
```

A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure Portal
Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez. Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.

## <a name="azure-storage-issues"></a>Az Azure storage-problémák
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni. Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez. Az eszköz segítségével az Azure Storage problémák elhárításához.
