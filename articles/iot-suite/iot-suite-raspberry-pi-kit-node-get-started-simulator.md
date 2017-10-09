---
title: "egy málna Pi tooAzure IoT Suite aaaConnect szimulált telemetriai Node.js használatával |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. Node.js tooconnect használata a távoli felügyeleti megoldás málna Pi toohello szimulált telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: f65eeaa6e83fd89cdedae8fa8386a3e9ef8417d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és a Node.js segítségével szimulált telemetriai adatokat küldhet

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse hello málna Pi 3 toosimulate hőmérséklet és a páratartalom adatok toosend toohello felhő. hello oktatóprogram:

- Raspbian operációs rendszer, hello Node.js programozási nyelv, és a Microsoft Azure IoT SDK hello Node.js tooimplement egy minta eszköz.
- mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a>Töltse le és konfigurálja a hello minta

Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.

### <a name="install-nodejs"></a>A Node.js telepítése

Ha még nem tette meg, telepítse a málna Pi Node.js. hello IoT SDK for Node.js 0.11.5 Node.js vagy újabb verzió szükséges. hello következő lépések bemutatják, hogyan tooinstall Node.js v6.10.2 a málna Pi meg:

1. A következő parancs tooupdate hello a málna telepítővel:

    ```sh
    sudo apt-get update
    ```

1. A következő parancs toodownload hello Node.js bináris tooyour málna Pi hello használata:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. A következő parancs tooinstall hello bináris hello használata:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. A következő parancs tooverify Node.js v6.10.2 sikeresen telepített hello használata:

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a>Klónozás hello adattárak

Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello követően egy terminált a parancsok a Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Hello eszköz kapcsolati karakterlánc frissítése

Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Hello sor keresése:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején. Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).

## <a name="run-hello-sample"></a>Hello minta futtatásához

Futtatási hello következő parancsok hello minta tooinstall hello csomagokat:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

Hello minta program hello málna Pi most futtathatja. Adja meg a hello parancsot:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Következő lépések

A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
