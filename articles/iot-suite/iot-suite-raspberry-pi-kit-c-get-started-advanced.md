---
title: "egy málna Pi tooAzure aaaConnect IoT Suite segítségével C toosupport belső vezérlőprogram frissítése |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. C tooconnect használata a távoli felügyeleti megoldás málna Pi toohello, telemetriai adatokat küldhet az érzékelők toohello felhő, és hajtsa végre a távoli vezérlőprogram-frissítés."
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
ms.openlocfilehash: 36d39c6d754ddb025fd3f6b74d7795ed907b754c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és engedélyezze a távoli belső vezérlőprogram-frissítésekre C használatával

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse hello Microsoft Azure IoT Starter Kit málna Pi 3:

* A hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a hello felhő fejlesztéséhez.
* Engedélyezi, és végezze el a távoli belső vezérlőprogram frissítési tooupdate hello ügyfélalkalmazás hello málna Pi.

hello oktatóprogram:

* Az operációs rendszer Raspbian, hello C programozási nyelv, és C tooimplement egy minta eszközt a Microsoft Azure IoT SDK hello.
* mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

## <a name="overview"></a>Áttekintés

Az oktatóanyag befejezése hello a következő lépéseket:

* Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése. Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.
* Állítsa be a számítógép és a hello az eszköz és az érzékelők toocommunicate távoli felügyeleti megoldás.
* Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és megtekintheti a hello megoldás irányítópultja telemetriát.
* Hello minta eszköz kód tooupdate hello-ügyfélalkalmazás használata.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a>Töltse le és konfigurálja a hello minta

Most töltse le, és a málna Pi hello távoli figyelési ügyfélalkalmazás konfigurálása.

### <a name="clone-hello-repositories"></a>Klónozás hello adattárak

Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello parancsok követően a Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Hello eszköz kapcsolati karakterlánc frissítése

Nyissa meg hello minta konfigurációs fájlt a hello **nano** szerkesztő hello a következő parancs használatával:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Cserélje le hello helyőrző értékeket hello azonosító és az IoT-központ eszközadatokat létrehozott és mentett ebben az oktatóanyagban hello elején.

Amikor elkészült, hello hello deviceinfo információja fájl tartalmát a következő példa hello hasonlóan kell kinéznie:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).

## <a name="build-hello-sample"></a>Hello minta

Ha még nem tette meg, telepítse hello csomagokat a Microsoft Azure IoT eszköz SDK hello C hello hello málna Pi követően egy terminál-parancsok futtatásával:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Most már lefordíthatja hello megoldást a hello málna Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

Hello minta program hello málna Pi most futtathatja. Adja meg a hello parancsot:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Hello megoldás irányítópulton kattintson **eszközök** toovisit hello **eszközök** lap. Válassza ki a málna Pi hello **eszközlista**. Válassza a **módszerek**:

    ![Az irányítópult eszközök][img-list-devices]

1. A hello **metódus meghívása** lapon, válassza ki **InitiateFirmwareUpdate** a hello **metódus** legördülő menüből.

1. A hello **FWPackageURI** mezőbe írja be **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Az archívumfájl hello végrehajtásának hello belső vezérlőprogram 2.0-s verzióját tartalmazza.

1. Válasszon **InvokeMethod**. hello málna Pi hello alkalmazást egy visszaigazoló hátsó toohello megoldás irányítópultja küld. Ezután elindítja a letöltés hello új hello belső vezérlőprogram verziójának hello belső vezérlőprogram frissítési folyamat:

    ![Módszer előzmények megjelenítése][img-method-history]

## <a name="observe-hello-firmware-update-process"></a>Tekintse át az hello belső vezérlőprogram frissítési folyamat

Figyelheti a hello belső vezérlőprogram frissítési folyamat hello eszközön futó, és hello megtekintésével jelentett hello megoldás irányítópultjának tulajdonságok:

1. Hello előrehaladását a frissítési folyamat hello hello málna Pi tekintheti meg:

    ![Megjeleníti a frissítési folyamatot][img-update-progress]

    > [!NOTE]
    > hello távoli figyelési alkalmazás újraindul csendes hello frissítése befejeződött. Hello paranccsal `ps -ef` tooverify fut-e. Ha azt szeretné, hogy tooterminate hello folyamat, használja a hello `kill` hello folyamatazonosító parancsot.

1. Hello hello belső vezérlőprogram-frissítés állapota a, megtekintheti a hello eszközt, hello megoldás portál által jelentett módon. hello alábbi képernyőfelvételen látható hello állapotát és minden szakaszhoz hello frissítési folyamat, és új belsővezérlőprogram-verziónként hello időtartama:

    ![Feladat állapotának megjelenítése][img-job-status]

    Lépjen vissza toohello irányítópult, ellenőrizheti a hello eszköz továbbra is küldi hello belső vezérlőprogram frissítése a következő telemetriai adatokat.

> [!WARNING]
> Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config]. Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.

## <a name="next-steps"></a>Következő lépések

A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md