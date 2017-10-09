---
title: "egy málna Pi tooAzure IoT Suite aaaConnect C használatával szimulált telemetriai |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. C tooconnect használata a távoli felügyeleti megoldás málna Pi toohello szimulált telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 3c4fa43b381385d1a7896cada34aa3aa0b8e5fb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a>Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és a C használatával szimulált telemetriai adatokat küldhet

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse hello málna Pi 3 toosimulate hőmérséklet és a páratartalom adatok toosend toohello felhő. hello oktatóprogram:

- Az operációs rendszer Raspbian, hello C programozási nyelv, és C tooimplement egy minta eszközt a Microsoft Azure IoT SDK hello.
- mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a>Töltse le és konfigurálja a hello minta

Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.

### <a name="clone-hello-repositories"></a>Klónozás hello adattárak

Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello követően egy terminált a parancsok a Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Hello eszköz kapcsolati karakterlánc frissítése

Nyissa meg hello minta forrásfájl hello **nano** szerkesztő hello a következő parancs használatával:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

Keresse meg az alábbi hello:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Cserélje le hello helyőrző értékeket hello eszköz- és IoT-központ adatokat létrehozott és mentett ebben az oktatóanyagban hello elején. Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).

## <a name="build-hello-sample"></a>Hello minta

A Microsoft Azure IoT eszköz SDK hello hello csomagokat telepítése c hello hello málna Pi követően egy terminál-parancsok futtatásával:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Most már lefordíthatja frissítése hello megoldást a hello málna Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

Hello minta program hello málna Pi most futtathatja. Adja meg a hello parancsot:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Következő lépések

A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
