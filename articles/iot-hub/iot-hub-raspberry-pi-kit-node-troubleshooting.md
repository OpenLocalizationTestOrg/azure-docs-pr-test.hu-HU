---
title: "Raspberry Pi (C) csatlakozzon az Azure IoT - hárítsa el a |} Microsoft Docs"
description: "A Pi Node.js málna élmény hibaelhárítási lap"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Az alkalmazás futtatása is, de a LED nem villogó.
A probléma mindig a hardver áramkör kapcsolatra vonatkozó. Az alábbi lépések segítségével azonosíthatja a problémákat:

1. Ellenőrizze, hogy úgy döntött, hogy a helyes **GPIO** a táblán. A két portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.
2. Ellenőrizze, hogy helyesen-e a LED polaritásának. A hosszabb engedélyezi kell jeleznie a **pozitív**, anód PIN-kódot.
3. Használja a **3.3V PIN-kód** és **GND PIN-kód** málna a Pi a 3. A tartományvezérlő power Pi kezelje. Ellenőrizze, hogy jól működik a LED-jét.

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Más hardverekkel kapcsolatos problémák szerepelnek
Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: a [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá a `--verbose` hibakeresési lehetőséget. Próbálja meg a Ctrl + C aktuális gulp feladatok befejezéséhez, és futtassa a következő parancsot a konzolablakban, hogy hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz-felderítési problémák
A gyakori problémák elhárításában súgó a `devdisco` parancs, ellenőrizze a [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm problémák
Próbálja meg frissíteni az npm-csomagot a következő paranccsal:

```bash
npm install -g npm
```

Ha a probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Távoli hibakeresés
### <a name="run-the-sample-application-in-debug-mode"></a>Futtassa a mintaalkalmazást hibakeresési módban
```bash
gulp run --debug
```

Amikor készen áll a hibakeresési motor, megjelennie ```Debugger listening on port 5858``` a konzol kimeneti.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Visual Studio Code kapcsolódni a távoli eszköz konfigurálása
1. Nyissa meg a **Debug** a bal oldali panelen.
2. Kattintson a zöld **Start Debugging** (F5) gombra. A Visual Studio Code launch.json fájlt nyit meg.
3. Frissítse a launch.json fájlt az alábbi tartalommal. Cserélje le `[device hostname or IP address]` , a tényleges eszköz IP-címét vagy állomásnevét nevét.

> [!NOTE]
> További információt a Visual Studio hibakeresési módját, tekintse meg [hibakeresést a Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


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

![Távoli hibakeresési konfigurálása](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>A távoli alkalmazás csatlakoztatása
Kattintson a zöld **Start Debugging** a hibakeresés (F5) gombra.

Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) további információt a hibakereső.

![Távoli, interaktív hibakeresés](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Az Azure CLI-problémák
Az Azure parancssori felület (CLI) preview build. Megoldásokat keres, használhatja a [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Ha az eszköz hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a a **problémák** a GitHub-tárház szakasza.

A Súgó gyakori problémák elhárításához, tekintse meg a [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python telepítési problémák
### <a name="legacy-installation-issues-macos"></a>A hagyományos telepítési problémák (macOS)
A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek. Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva. A korábbi telepítés egyes pip csomagok létrehozásakor a gyökérszintű, amely azt eredményezi, az engedély hiba. A megoldás, hogy távolítsa el azokat a legfelső szintű telepített csomagokat. Ez a feladat befejezéséhez tegye a következőket:

1. Ugrás: /usr/local/lib/python2.7/site-packages
2. Legfelső szintű által létrehozott csomagok:`ls -l | grep root`
3. A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`
4. Telepítse újra a Python.

## <a name="azure-iot-hub-issues"></a>Az Azure IoT Hub-problémák
Ha sikeresen korábban létesített az Azure IoT hub Azure parancssori felület segítségével, és kezelheti az eszközöket, amelyek csatlakozik, az IoT hub eszköz van szüksége, próbálkozzon a következő eszközök.

### <a name="device-explorer"></a>Eszköz explorer
A [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eszköz a Windows helyi számítógépen fut, és az Azure IoT hub csatlakozik. A következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):


* *Eszköz Identitáskezelés* szeretnék telepíteni és kezelni az IoT hub regisztrált eszközökre.
* *Eszköz-felhő kap* , figyelheti az IoT hub az eszközről küldött üzeneteket.
* *Felhő eszközre küldése* úgy küldhet üzeneteket az eszközök az IoT hub a.

Az IoT hub kapcsolati karakterlánc ebből az eszközből készülék képességeinek használatához konfigurálása.

### <a name="iothub-explorer"></a>IOT hubbal-explorer
[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy minta többplatformos parancssori felület eszköz eszközök kezelésére. Az eszköz segítségével az identitásjegyzékhez lévő eszközök kezeléséhez, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő-eszközre küldött üzenetek küldése.

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

## <a name="azure-storage-issues"></a>Az Azure tárolási problémák
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft, amelyek segítségével a Windows, a macOS és a Linux Azure Storage-adatokkal dolgozni. Ez az eszköz segítségével, a tábla csatlakozhat, és azt az adatok megtekintéséhez. Az eszköz segítségével az Azure Storage problémák elhárításához.

