---
title: "Csatlakozás az Azure IoT - 1. lecke Raspberry Pi (C): alkalmazás üzembe helyezése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazást a Githubból, és ezt az alkalmazást a málna Pi 3 board telepítendő gulp. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét."
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
ms.openlocfilehash: 2ae409c6a39521711777ec329d2507a2801cc985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-deploy-the-blink-application"></a>A villogóalkalmazás elkészítése és üzembe helyezése
## <a name="what-you-will-do"></a>Mit fog
Klónozza a C mintaalkalmazást a Githubból, és a gulp eszközzel a mintaalkalmazás málna Pi 3 telepítéséhez. A mintaalkalmazás villogjon a kártyához csatlakoztatott két másodpercenként LED-jét. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan használható a `device-discover-cli` eszköz Pi hálózati adatainak lekérésére.
* Hogyan telepítheti, és futtassa a mintaalkalmazást a Pi.
* Hogyan telepítheti, és távolról futó Pi-alkalmazások hibakeresését.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveleteket:

* [Az eszköz konfigurálása](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md)
* [Eszközök](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

## <a name="obtain-the-ip-address-and-host-name-of-pi"></a>A IP-cím és a név a pi beszerzése
Nyisson meg egy parancssort a Windows vagy a Terminálszolgáltatások macOS vagy Ubuntu, és futtassa a következő parancsot:

```bash
devdisco list --eth
```

A következőhöz hasonló kimenetnek kell megjelennie:

![Eszköz felderítése](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Vegye figyelembe a `IP address` és `hostname` pi. Ez a cikk későbbi részében tájékoztatásra van szüksége.

> [!NOTE]
> Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik. Például ha a számítógép vezeték nélküli hálózathoz Pi egy vezetékes hálózathoz van csatlakoztatva, előfordulhat, hogy nem látja az IP-cím devdisco kimenet.

## <a name="open-the-sample-application"></a>Nyissa meg a mintaalkalmazás
A mintaalkalmazás megnyitásához kövesse az alábbi lépéseket:

1. A Githubból a minta-tárház klónozása a következő parancs futtatásával:
   
    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started.git
    ```
2. Nyissa meg a Visual Studio Code a mintaalkalmazást a következő parancsok futtatásával:
   
    ```bash
    cd iot-hub-c-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-c-mac.png)

A `main.c` fájlt a `app` almappa is szabályozhatja a LED kódot tartalmazó kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
A szalagtárak és egyéb modulok a következő parancs futtatásával kell a mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:

1. Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:
   
   ```bash
   gulp init
   ```
   
   A konfigurációs fájl `config-raspberrypi.json` felhasználói hitelesítő adatokkal kell bejelentkezni Pi tartalmazza. A felhasználói hitelesítő adatok memóriavesztés elkerülése érdekében a konfigurációs fájl jön létre, almappájában `.iot-hub-getting-started` az otthoni mappa a számítógépen.

2. Nyissa meg a Visual Studio Code eszköz konfigurációs fájl a következő parancs futtatásával:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```

3. Cserélje le a helyőrző `[device hostname or IP address]` az IP-cím vagy a korábban kapott állomásnév "szerezze be a IP-cím és a név a pi."
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

> [!NOTE]
> Használhat SSH-kulcs felhasználónév és jelszó helyett, málna Pi történő csatlakozás során. Ehhez meg kell létrehozni, a kulcs használatával **ssh-keygen** és **ssh--azonosítót pi @\<eszköz címe\>**.
>
> A Windows ezen parancsok is elérhetők, a **Git bash eszközt**.
>
> MacOS szüksége futtatásához **brew telepítése ssh--azonosítót**.
>
> Után a kulcs sikeres feltöltését a málna Pi, cserélje le a **device_password** rendelkező **device_key_path** tulajdonság **config-raspberrypi.json**.
>
> Frissített sorok az alábbiak szerint kell kinéznie:
> ```javascript
> "device_user_name": "pi",
> "device_key_path": "id_rsa",
> ```

Gratulálunk! A pi első mintaalkalmazás sikeresen létrehozta.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
### <a name="install-the-azure-iot-hub-sdk-on-pi"></a>Az Azure IoT Hub SDK Pi telepítése
Az Azure IoT Hub SDK Pi telepítése a következő parancs futtatásával:

```bash
gulp install-tools
```

Ez a feladat befejeződik, az első futtatásakor néhány percig is eltarthat.

### <a name="deploy-and-run-the-sample-app"></a>Központi telepítése és a mintaalkalmazás futtatása
Telepíthet, és futtassa a mintaalkalmazást a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

### <a name="verify-the-app-works"></a>Ellenőrizze az alkalmazás akkor működik
A mintaalkalmazás automatikusan leáll, miután a LED villogjon-20 alkalommal a. Ha nem látja a LED villogó, tekintse meg a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.
![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

## <a name="summary"></a>Összefoglalás
Pi használható szükséges eszközök telepítése és telepített egy mintaalkalmazást a LED villogni a-pi tartományban. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti Pi Azure IoT Hub az üzeneteket küldjön és fogadjon.

## <a name="next-steps"></a>Következő lépések
[Azure-eszközök beszerzése](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)

