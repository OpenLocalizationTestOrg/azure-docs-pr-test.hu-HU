---
title: "Az IoT - Connect Arduino (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap Adafruit lágyított M0 Wi-Fi Arduino élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino hibaelhárítása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ed793041ffa1887dbe73067f7c48d2417e982866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
A Adafruit lágyított M0 Wi-Fi Arduino táblán gyakori problémák megoldásával kapcsolatos további információkért lásd: hello [hivatalos hibaelhárítási lap](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget. Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg.

```bash
gulp --verbose
```

Vagy felveheti `--listen` tooopen soros port toooutput eszköz szereplő információkat.

```bash
gulp --listen
``` 

### <a name="npm-issues"></a>NPM problémák
Próbálja meg tooupdate az NPM csomag hello a következő parancsot:

```bash
npm install -g npm
```

Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].

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
[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik. Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):

* *Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.
* *Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.
* *Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.

Konfigurálja a `IoT hub connection string` belül az eszköz toouse annak képességeit.

### <a name="iot-hub-explorer"></a>Az IoT-központ Explorer
[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől. Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.


tooinstall hello hello IOT hubbal-explorer eszköz, futtassa a következő parancsot a parancssori környezetben hello legújabb (előzetes) verzióját:

```bash
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

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md