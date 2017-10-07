---
title: "Az IoT - Connect Raspberry pi (C) tooAzure hibaelhárítása |} Microsoft Docs"
description: "Hibaelhárítás lap hello málna Pi Node.js élmény"
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
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a>hello alkalmazás futtatása is jól, de hello LED nem villogó.
A probléma mindig kapcsolódó toohardware expressroute kapcsolat. A következő lépéseket tooidentify problémák hello használata:

1. Ellenőrizze, hogy helyes-e hello választott **GPIO** a táblán. a két hello portokat kell **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**.
2. Ellenőrizze, hogy helyes-e a LED hello polaritásának. hello hosszabb engedélyezi kell jeleznie hello **pozitív**, anód PIN-kódot.
3. Használjon hello **3.3V PIN-kód** és **GND PIN-kód** málna Pi 3. A Pi hello DC power kezelje. Ellenőrizze, hogy hello LED remekül működik.

![LED meghatározása](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Más hardverekkel kapcsolatos problémák szerepelnek
Málna Pi 3 gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá hello `--verbose` hibakeresési lehetőséget. Próbálja tooterminate aktuális gulp feladatokat a Ctrl + C használatával, és futtassa a következő parancsot a konzol ablakot toosee hibakeresési üzeneteket hello. A részletes hibaüzenetek a konzol kimeneti jelenhet meg.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz-felderítési problémák
A Súgó hello gyakori problémák elhárításában `devdisco` paranccsal, ellenőrizze a hello [információs](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>npm problémák
Próbálja meg tooupdate az npm csomag hello a következő parancs használatával:

```bash
npm install -g npm
```

Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).

## <a name="remote-debugging"></a>Távoli hibakeresés
### <a name="run-hello-sample-application-in-debug-mode"></a>Hibakeresési módban hello mintaalkalmazás futtatása
```bash
gulp run --debug
```

Amikor készen áll a hello hibakeresési motor, láthatja ```Debugger listening on port 5858``` a konzol kimeneti hello.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Visual Studio Code tooconnect toohello távoli eszközök konfigurálása
1. Nyissa meg hello **Debug** a hello bal oldali panelen.
2. Kattintson a zöld hello **Start Debugging** (F5) gombra. A Visual Studio Code launch.json fájlt nyit meg.
3. A következő tartalmat hello hello launch.json fájl frissítése. Cserélje le `[device hostname or IP address]` hello tényleges IP cím vagy állomásnév eszköznévvel.

> [!NOTE]
> toolearn Visual Studio hibakeresési hello kapcsolatos további információkért tekintse meg a túl[hibakeresést a Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


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

### <a name="attach-toohello-remote-application"></a>Toohello távoli alkalmazás csatlakoztatása
Kattintson a zöld hello **Start Debugging** (F5) gomb toostart hibakeresést.

Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.

![Távoli, interaktív hibakeresés](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Az Azure CLI-problémák
az Azure parancssori felület (CLI) hello preview build. tooseek megoldások hello használható [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.

Segítséget a gyakori problémák elhárításához, ellenőrizze a hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python telepítési problémák
### <a name="legacy-installation-issues-macos"></a>A hagyományos telepítési problémák (macOS)
A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek. Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva. A korábbi telepítés egyes pip csomagok hello engedély hibát okoz a legfelső szintű lettek létrehozva. megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van. Ez a feladat használható a következő lépéseket toocomplete hello:

1. Ugrás: /usr/local/lib/python2.7/site-packages
2. Legfelső szintű által létrehozott csomagok:`ls -l | grep root`
3. A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`
4. Telepítse újra a Python.

## <a name="azure-iot-hub-issues"></a>Az Azure IoT Hub-problémák
Ha sikeresen korábban létesített az Azure IoT hub Azure parancssori felület segítségével, és egy eszköz toomanage hello eszközök tooyour IoT-központ csatlakozó van szüksége, próbálkozzon a következő eszközök hello.

### <a name="device-explorer"></a>Eszköz explorer
Hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eszköz a Windows helyi számítógépen fut, és az Azure IoT-központ tooyour csatlakozik. Hello következő kommunikál [IoT-központok végpontjai](iot-hub-devguide.md):


* *Eszköz Identitáskezelés* tooprovision és az IoT hub regisztrált eszközök kezeléséhez.
* *Eszköz-felhő kap* , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.
* *Felhő eszközre küldése* úgy küldhet üzeneteket tooyour eszközök az IoT hub a.

Konfigurálja az IoT hub kapcsolati karakterlánc az eszköz toouse belül minden képességet.

### <a name="iothub-explorer"></a>IOT hubbal-explorer
[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközök. A hello identitásjegyzékhez hello eszköz toomanage hello eszközöket használnak, eszköz-a-felhőbe küldött üzeneteket figyelése és felhő-eszközre küldött üzenetek küldése.

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

## <a name="azure-storage-issues"></a>Az Azure tárolási problémák
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal. Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez. Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.

