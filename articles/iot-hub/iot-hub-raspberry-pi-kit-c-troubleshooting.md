---
title: "Az IoT - Connect Raspberry pi (C) tooAzure hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 4f1ea81dd25d10a39c2939f5ee5f19f6d2ba2b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a>hello alkalmazás futtatása is jól, de hello LED nem villogó.
A probléma mindig kapcsolódó toohello hardver expressroute kapcsolat. A következő lépéseket tooidentify problémák hello használata.

1. Ellenőrizze, hogy helyes-e hello választott **GPIO** a táblán. a két hello portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.
2. Ellenőrizze, hogy helyes-e a LED hello polaritásának. hello hosszabb engedélyezi kell jeleznie hello **pozitív**, anód PIN-kódot.
3. Használjon hello **3.3V PIN-kód** és **GND PIN-kód** málna Pi 3. A Pi hello DC power kezelje. Ellenőrizze, hogy hello LED remekül működik.

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Más hardverekkel kapcsolatos problémák szerepelnek
Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget. Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz-felderítési problémák
A Súgó hello gyakori problémák elhárításában `devdisco` paranccsal, ellenőrizze a hello [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>NPM problémák
Próbálja meg tooupdate az NPM csomag hello a következő parancsot:

```bash
npm install -g npm
```

Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Távoli hibakeresés

Távoli hibaelhárítási támogatás a Visual Studio Code C/C++ bővítmény hamarosan elérhető lesz.

Egy meanwhile GDB használhat a kedvenc SSH terminál keresztül:

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a>Azure-CLI problémák
az Azure parancssori felület (CLI) hello preview build. Keressen megoldást a hello [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek megoldásokat. Amikor parancsok nem a várt módon működik, próbálja meg tooupgrade Azure-cli toolatest verziója.

Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.

A gyakori problémák elhárításához tekintse meg hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).

Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Python telepítési problémák
### <a name="legacy-installation-issues-macos"></a>A hagyományos telepítési problémák (macOS)
Amikor telepít **pip**, egy engedély hibát vált ki, ha a régebbi csomagok, amelyeket együtt telepített **su** engedélyek. Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva. Néhány **pip** a korábbi telepítés csomagok hello engedély hibát okoz a legfelső szintű hozott létre. megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van. Ez a feladat használható a következő lépéseket toocomplete hello:

1. Ugrás: /usr/local/lib/python2.7/site-packages
2. Csomagok létrehozása a legfelső szintű:`ls -l | grep root`
3. A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`
4. Telepítse újra a Python.

## <a name="azure-iot-hub-issues"></a>Az Azure IoT Hub-problémák
Ha az Azure IoT hubot sikeresen kiépítette már `azure-cli`, és egy eszköz toomanage hello eszközök tooyour IoT-központ, a következő eszközök próbálja hello csatlakozó van szüksége:

### <a name="device-explorer"></a>Eszköz Explorer
[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik. Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):

* *Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.
* *Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.
* *Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.

Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.

### <a name="iot-hub-explorer"></a>Az IoT-központ Explorer
[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől. Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.

tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:

```
npm install -g iothub-explorer@latest
```

A következő parancs tooget további súgó összes hello IOT hubbal-explorer parancsot és paramétereket hello használhatja:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure Portal
Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez. Érdemes lehet toouse hello [Azure-portálon](../azure-portal-overview.md) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.

## <a name="azure-storage-issues"></a>Az Azure storage-problémák
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal. Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez. Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.
