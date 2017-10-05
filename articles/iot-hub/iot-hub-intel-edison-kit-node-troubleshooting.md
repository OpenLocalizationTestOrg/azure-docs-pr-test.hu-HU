---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - 4. lecke: hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap Intel Edison Node.js-élmény"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino hibaelhárítása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: f11c5521-8a36-44c0-baad-f5ec26ab4618
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d54dd4a13ed87152fb6c039f38a5ffe8b4470df9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá a `--verbose` hibakeresési lehetőséget. Állítsa le a jelenlegi gulp feladatok használatával próbál `Ctrl + C`, és futtassa a következő parancsot a konzolablakban tekintse meg a hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg. 

```bash
gulp --verbose
```

### <a name="npm-issues"></a>NPM problémák
Próbálja meg frissíteni az NPM-csomagot a következő parancsot:

```bash
npm install -g npm
```

Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].

## <a name="remote-debugging"></a>Távoli hibakeresés

### <a name="run-the-sample-application-in-debug-mode"></a>Futtassa a mintaalkalmazást hibakeresési módban

```bash
gulp run --debug
```

Ha készen áll a hibakeresési motor, kell tudni ```Debugger listening on port 5858``` a konzol kimeneti a.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Visual STUDIO Code kapcsolódni a távoli eszköz konfigurálása

Nyissa meg a **Debug** bal oldali panelen.

Kattintson a zöld **Start Debugging** (F5) gombra. Visual STUDIO Code nyitna a **launch.json** fájl, amelyet frissíteni kell.

Frissítés a **launch.json** fájlt a következő tartalmat, hogy lecseréli `[device hostname or IP address]` a tényleges eszköz IP-címet vagy állomásnevet.  

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Távoli hibakeresési konfigurálása](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>A távoli alkalmazás csatlakoztatása

Kattintson a zöld **Start Debugging** (F5) gombra, és kihasználhatják a hibakeresést.

Elolvashatja [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) további információt a hibakereső.

![Távoli, interaktív hibakeresés](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

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
[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT hub csatlakozik. A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):

- _Eszköz Identitáskezelés_ szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.
- _Eszköz-felhő kap_ , figyelheti az IoT hub az eszközről küldött üzeneteket.
- _Felhő eszközre küldése_ úgy küldhet üzeneteket az eszközök az IoT hub a.

Konfigurálja a `IoT hub connection string` ebből az eszközből készülék képességeinek használatához.

### <a name="iot-hub-explorer"></a>Az IoT-központ Explorer
[Az IoT-központ Explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközügyfeleken kezelésére. Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő eszközparancsok küldése.

Telepítse a legújabb (előzetes) verzióját az IOT hubbal-explorer eszköz, a következő parancsot a parancssori környezetben:

```bash
npm install -g iothub-explorer@latest
```

A következő paranccsal minden IOT hubbal-explorer parancsot és paramétereket kapcsolatos további segítség:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure Portal
Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez. Érdemes lehet használni a [Azure-portálon](../azure-portal-overview.md) kiépítéséhez, kezeléséhez, és hibakeresése az Azure-erőforrások.

## <a name="azure-storage-issues"></a>Az Azure storage-problémák
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, amelyekkel dolgozni Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok. Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez. Az eszköz segítségével az Azure Storage problémák elhárításához.

## <a name="next-steps"></a>Következő lépések
Ezen a lapon csak Intel Edison Kit leggyakoribb problémákat foglalja magában. Alsó megjegyzések további hibaelhárítási jelentés problémákat is hagyhatja.

Lépjen vissza a [Intel Edison (Node.js) az első lépései](iot-hub-intel-edison-kit-node-get-started.md)

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started