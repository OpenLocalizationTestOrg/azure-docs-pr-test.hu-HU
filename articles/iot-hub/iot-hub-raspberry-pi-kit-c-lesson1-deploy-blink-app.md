---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 1: alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazás hello a Githubból, és toodeploy gulp az alkalmazás tooyour málna Pi 3 tábla. A mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi vezetett villogási, villogási vezetett a raspberry pi"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f601d1e1-2bc3-4cc5-a6b1-0467e5304dcf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: e90c3360c4de1873313db19561c781eb21dbf1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Hello villogási alkalmazás létrehozását és telepítését
## <a name="what-you-will-do"></a>Mit fog
Klónozza a C mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooRaspberry Pi 3 használja. hello mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan toouse hello `device-discover-cli` Pi információ a hálózati eszköz tooretrieve.
* Hogyan toodeploy és futtatási hello mintaalkalmazást a Pi.
* Hogyan távolról futó Pi toodeploy és hibakeresési alkalmazásokhoz.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveletek hello:

* [Az eszköz konfigurálása](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Hello eszközök beszerzése](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-hello-ip-address-and-host-name-of-pi"></a>Hello IP-cím és a Pi név beszerzése
Nyisson meg egy parancssort a Windows vagy a Terminálszolgáltatások macOS vagy Ubuntu, és futtassa a parancsot a következő hello:

```bash
devdisco list --eth
```

Amely hasonló toohello következő kimenetnek kell megjelennie:

![Eszköz felderítése](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Jegyezze fel a hello `IP address` és `hostname` pi. Ez a cikk későbbi részében tájékoztatásra van szüksége.

> [!NOTE]
> Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello. Például, ha a számítógép vezeték nélküli hálózathoz csatlakoztatott tooa Pi pedig vezetékes hálózathoz csatlakoztatott tooa, akkor előfordulhat, hogy nem lásd: hello IP-cím hello devdisco kimenet.

## <a name="open-hello-sample-application"></a>Nyissa meg hello mintaalkalmazás
tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:

1. A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

Hello `main.c` hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:

1. Hello eszköz konfigurációs fájl létrehozása hello a következő parancs futtatásával:
   
   ```bash
   gulp init
   ```
   
   hello konfigurációs fájl `config-raspberrypi.json` hello segítségével toolog tooPi felhasználói hitelesítő adatokat tartalmaz. tooavoid hello szivárgásával járnak a felhasználói hitelesítő adatok, hello konfigurációs fájl jön létre hello almappájában `.iot-hub-getting-started` hello otthoni mappa a számítógépen.

2. Nyissa meg a Visual Studio Code hello eszköz konfigurációs fájl hello a következő parancs futtatásával:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Cserélje le a hello helyőrző `[device hostname or IP address]` hello IP-címmel vagy hello állomásnév korábban portáltól "Szerezze be a hello IP cím és a host name pi."
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Használhat SSH-kulcs felhasználónév és jelszó helyett, tooRaspberry Pi kapcsolódáskor. A rendezés toodo ez toogenerate hello kulcs használatával kell **ssh-keygen** és **ssh--azonosítót pi @\<eszköz címe\>**.
>
> A Windows ezen parancsok is elérhetők, a **Git bash eszközt**.
>
> A MacOS toorun kell **brew telepítése ssh--azonosítót**.
>
> Sikeresen feltöltése hello kulcs toohello málna Pi, után cserélje le a **device_password** rendelkező **device_key_path** tulajdonság **config-raspberrypi.json**.
>
> Frissített sorok az alábbiak szerint kell kinéznie:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Gratulálunk! Sikeresen létrehozta a hello pi első mintaalkalmazást.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
### <a name="install-hello-azure-iot-hub-sdk-on-pi"></a>A Pi hello Azure IoT Hub SDK telepítése
Hello Azure IoT Hub SDK telepítése Pi hello a következő parancs futtatásával:

```bash
gulp install-tools
```

Ez a feladat eltarthat néhány percig toocomplete hello első futtatásakor.

### <a name="deploy-and-run-hello-sample-app"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Ellenőrizze a hello az alkalmazás akkor működik
hello mintaalkalmazás automatikusan leáll, miután hello LED villogjon-20 alkalommal a. Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) a megoldások toocommon problémákat.
![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Összefoglalás
A Pi hello szükséges eszközök toowork telepítése és telepített egy minta alkalmazás tooPi tooblink hello LED-jét. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely a Pi tooAzure IoT-központ toosend és üzeneteket fogadni.

## <a name="next-steps"></a>Következő lépések
[Azure-eszközök beszerzése](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

