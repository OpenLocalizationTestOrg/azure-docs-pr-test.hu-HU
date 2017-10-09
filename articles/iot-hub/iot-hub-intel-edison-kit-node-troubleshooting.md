---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 4: hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 9502300f7f372255572b49bea45dd3e1fdaeeb37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás
## <a name="hardware-issues"></a>Hardverproblémák
Intel Edison gyakori problémáinak megoldásával kapcsolatban további információkért lásd: hello [hivatalos hibaelhárítási lap](https://software.intel.com/en-us/node/637974).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák
### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.
Ha problémába ütközik az éppen futó feladatok gulp, adhat hozzá hello `--verbose` hibakeresési lehetőséget. Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg. 

```bash
gulp --verbose
```

### <a name="npm-issues"></a>NPM problémák
Próbálja meg tooupdate az NPM csomag hello a következő parancsot:

```bash
npm install -g npm
```

Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház][sample-repository].

## <a name="remote-debugging"></a>Távoli hibakeresés

### <a name="run-hello-sample-application-in-debug-mode"></a>Hibakeresési módban hello mintaalkalmazás futtatása

```bash
gulp run --debug
```

Ha hello hibakeresési motor készen áll, meg kell tudni toosee ```Debugger listening on port 5858``` hello konzol kimenetből.

### <a name="configure-vs-code-tooconnect-toohello-remote-device"></a>Visual STUDIO Code tooconnect toohello távoli eszközök konfigurálása

Nyissa meg hello **Debug** a hello bal oldali panelen.

Kattintson a zöld hello **Start Debugging** (F5) gombra. Visual STUDIO Code nyitna a **launch.json** fájlt, amely tooupdate van szüksége.

Frissítés hello **launch.json** tartalom a következő hello fájlt, hogy lecseréli `[device hostname or IP address]` hello tényleges eszköz IP-címet vagy állomásnevet.  

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

### <a name="attach-toohello-remote-application"></a>Toohello távoli alkalmazás csatlakoztatása

Kattintson a zöld hello **Start Debugging** (F5) gombra, és kihasználhatják a hibakeresést.

Elolvashatja [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.

![Távoli, interaktív hibakeresés](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

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

- _Eszköz Identitáskezelés_ tooprovision és az IoT hub regisztrált eszközök kezeléséhez.
- _Eszköz-felhő kap_ , megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek.
- _Felhő eszközre küldése_ úgy küldhet üzeneteket tooyour eszközök az IoT hub a.

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
[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, melyekkel a toowork Microsoft [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) Windows, a macOS és a Linux adatok. Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez. Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.

## <a name="next-steps"></a>Következő lépések
Ezen a lapon csak a leggyakoribb problémák hello Intel Edison Kit tartalmazza. Alsó megjegyzések tooreport problémákkal kapcsolatos további információkat is hagyhatja.

Lépjen vissza túl[Intel Edison (Node.js) az első lépései](iot-hub-intel-edison-kit-node-get-started.md)

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started