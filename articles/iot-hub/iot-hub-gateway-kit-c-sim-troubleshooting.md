---
title: "A szimulált eszköz & Azure IoT átjáró - hibaelhárítása |} Microsoft Docs"
description: "Intel NUC átjáró hibaelhárítási lap"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT problémák, internet dolgot problémák"
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: cc7add6273e66aadeca9a4915a44f5edf61a0a59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a>Hibaelhárítás

## <a name="hardware-issues"></a>Hardverproblémák

### <a name="ti-sensortag-cannot-be-connected"></a>Nem lehet csatlakozni a TI SensorTag

tootroubleshoot SensorTag kapcsolódási problémák, használja a hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Probléma lehet az Intel NUC

tootroubleshoot rendszerindítási problémák, tekintse meg a túl[Intel NUC nem rendszerindító hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).

tootroubleshoot operációs rendszerrel kapcsolatos problémák, tekintse meg a túl[Intel NUC operációs rendszer hibaelhárítási](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).

tootroubleshoot más olyan problémák, tekintse meg a túl[villogni és az Intel NUC-e sípoló kódokat](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).

## <a name="nodejs-package-issues"></a>NODE.js csomag problémák

### <a name="no-response-during-gulp-tasks"></a>Gulp műveletek során nincs válasz.

Ha a futó feladatok gulp problémákat tapasztal, adhat hozzá hello `--verbose` hibakeresési lehetőséget. Próbálja tooterminate aktuális gulp feladatok használatával `Ctrl + C`, majd a futtatási hello végrehajtja a parancsot a konzol ablakot toosee hibakeresési üzeneteket. A részletes hibaüzenetek a konzol kimeneti jelenhet meg.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz-felderítési problémák

A Súgó hello gyakori problémák elhárításában `discover-sensortag` paranccsal, ellenőrizze a hello [wiki lapján](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>npm problémák

Próbálja meg tooupdate az npm csomag hello a következő parancs futtatásával:

```bash
npm install -g npm
```

Ha hello probléma továbbra is fennáll, hagyja meg a megjegyzéseit, ez a cikk végén hello, vagy hozzon létre egy GitHub probléma a [minta tárház](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).

## <a name="remote-debugging"></a>Távoli hibakeresés
> Alább utasításokat úgy van kialakítva, ebben az oktatóanyagban használt node.js parancsfájlokat hibakereséshez.
### <a name="run-hello-sample-application-in-debug-mode"></a>Hibakeresési módban hello mintaalkalmazás futtatása

Futtassa a mintaalkalmazást hello hibakeresési módban hello a következő parancs futtatásával:

```bash
gulp run --debug
```

Amikor készen áll a hello hibakeresési motor, láthatja `Debugger listening on port 5858` a konzol kimeneti hello.

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a>Visual Studio Code tooconnect toohello távoli eszközök konfigurálása

1. Nyissa meg hello **Debug** a hello bal oldali panelen.
2. Kattintson a zöld hello **Start Debugging** (F5) gombra. A Visual Studio Code megnyílik egy `launch.json` fájlt.
3. Frissítés hello `launch.json` hello tartalom a következő fájl. Cserélje le `[device hostname or IP address]` hello tényleges IP cím vagy állomásnév eszköznévvel.

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Távoli hibakeresési konfigurálása](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a>Toohello távoli alkalmazás csatlakoztatása

Kattintson a zöld hello **Start Debugging** (F5) gomb toostart hibakeresést.

Olvasási [Visual STUDIO Code JavaScript](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn hello hibakereső többet.

![Hibakeresési BLA-minta](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Az Azure CLI-problémák

az Azure parancssori felület (CLI) hello preview build. tooseek megoldások hello használható [Preview telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).

Ha hello eszközzel hibáit, a fájl egy [probléma](https://github.com/Azure/azure-cli/issues) a hello **problémák** hello GitHub-tárház szakasza.

Segítséget a gyakori problémák elhárításához, ellenőrizze a hello [információs](https://github.com/Azure/azure-cli/blob/master/README.rst).

Ha megfelel a "Nem található olyan verzióra, amely eleget tesz a hello követelmény", adjon futtatási hello következő parancsot a tooupgrade pip toolastest verziója.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Python telepítési problémák

### <a name="legacy-installation-issues-macos"></a>A hagyományos telepítési problémák (macOS)

A pip telepítése, amikor egy engedély hibát vált ki, ha régebbi csomagok vannak telepítve a **su** engedélyek. Ebben az esetben az okozza, hogy a Python brew (macOS) használatával egy korábban elindított telepítése nincs teljesen eltávolítva. A korábbi telepítés egyes pip csomagok hello engedély hibát okoz a legfelső szintű lettek létrehozva. megoldás hello tooremove azokat a csomagokat, legfelső szintű telepítve van. Ez a feladat használható a következő lépéseket toocomplete hello:

1. Nyissa meg túl`/usr/local/lib/python2.7/site-packages`
2. Legfelső szintű által létrehozott csomagok:`ls -l | grep root`
3. A 2. lépésben csomagok eltávolításához:`sudo rm -rf {package name}`
4. Telepítse újra a Python.

## <a name="azure-iot-hub-issues"></a>Az Azure IoT Hub-problémák

Ha sikeresen korábban létesített a hello Azure CLI Azure IoT hubot, és egy eszköz toomanage hello eszközök tooyour IoT-központ csatlakozó van szüksége, próbálkozzon a következő eszközök hello.

### <a name="device-explorer"></a>Eszköz Explorer

[Eszköz Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) Windows helyi gépen fut, és az Azure IoT-központ tooyour csatlakozik. Hello következő kommunikál [IoT-központok végpontjai](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Eszköz identity management tooprovision és az IoT hub regisztrált eszközök kezeléséhez.
- Eszköz-felhő, megfigyelheti, hogy az eszköz tooyour IoT hub küldött üzenetek fogadása
- Úgy is küldhet üzenetet tooyour eszközök az IoT hub a felhőből eszközre küldése

Konfigurálja az IoT hub kapcsolati karakterlánc az eszköz toouse belül minden képességet.

### <a name="iothub-explorer"></a>IOT hubbal-explorer

[IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) egy olyan minta többplatformos parancssori felület eszköz toomanage eszközügyfeleitől. Hello eszköz toomanage hello eszközeiket használják a hello identitásjegyzékhez, figyelheti az eszköz a felhőbe küldött üzeneteket, és felhő eszközparancsok küldése.

tooinstall hello legújabb (előzetes) verzióját hello IOT hubbal-explorer eszköz, futtassa a következő parancs hello:

```bash
npm install -g iothub-explorer@latest
```

További segítséget tooget összes hello az IOT hubbal-explorer parancsot és paramétereket, futtassa a következő parancs hello:

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a>hello Azure-portálon

Egy teljes parancssori felület segítségével létrehozhatja és az Azure erőforrások kezeléséhez. Érdemes lehet toouse hello [Azure-portálon](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp kiépítését, kezelése és hibakeresése az Azure-erőforrások.

## <a name="azure-storage-issues"></a>Az Azure tárolási problémák

[A Microsoft Azure Tártallózó (előzetes verzió)](http://storageexplorer.com/) egy különálló alkalmazás, a Microsoft használható toowork macOS, Linux és a Windows Azure Storage-adatokkal. Az eszköz használatával tooyour tábla csatlakozhat, és azt hello adatok megtekintéséhez. Az eszköz tootroubleshoot használhatja az Azure tárolási problémák.
