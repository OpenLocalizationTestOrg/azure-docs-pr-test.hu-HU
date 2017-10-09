---
title: "egy málna Pi tooAzure aaaConnect IoT Suite használata Node.js toosupport belső vezérlőprogram frissítése |} Microsoft Docs"
description: "A Microsoft Azure IoT Starter Kit hello a hello málna Pi 3 és Azure IoT Suite használja. Node.js tooconnect használata a távoli felügyeleti megoldás málna Pi toohello, telemetriai adatokat küldhet az érzékelők toohello felhő, és hajtsa végre a távoli vezérlőprogram-frissítés."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 43bd3f16ee3d292cd9cffa8bfe7d4ca721e5c39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a>Csatlakozzon a távoli felügyeleti megoldás málna Pi 3 toohello, és engedélyezze a távoli belső vezérlőprogram-frissítésekre Node.js segítségével

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse hello Microsoft Azure IoT Starter Kit málna Pi 3:

* A hőmérséklet és a páratartalom olvasó, amely képes kommunikálni a hello felhő fejlesztéséhez.
* Engedélyezi, és végezze el a távoli belső vezérlőprogram frissítési tooupdate hello ügyfélalkalmazás hello málna Pi.

hello oktatóprogram:

- Raspbian operációs rendszer, hello Node.js programozási nyelv, és a Microsoft Azure IoT SDK hello Node.js tooimplement egy minta eszköz.
- mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

## <a name="overview"></a>Áttekintés

Az oktatóanyag befejezése hello a következő lépéseket:

- Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése. Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.
- Állítsa be a számítógép és a hello az eszköz és az érzékelők toocommunicate távoli felügyeleti megoldás.
- Hello eszköz kód tooconnect toohello távoli figyelési megoldást frissítése, és megtekintheti a hello megoldás irányítópultja telemetriát.
- Hello minta eszköz kód tooupdate hello-ügyfélalkalmazás használata.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

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

Ha még nem tette meg, a Klónozás hello szükséges futtatásával adattárak hello parancsok követően a Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Hello eszköz kapcsolati karakterlánc frissítése

Nyissa meg hello minta konfigurációs fájlt a hello **nano** szerkesztő hello a következő parancs használatával:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Cserélje le hello helyőrző értékeket hello eszközazonosító és az IoT-központ információk létrehozott és mentett ebben az oktatóanyagban hello elején.

Amikor elkészült, hello hello deviceinfo információja fájl tartalmát a következő példa hello hasonlóan kell kinéznie:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).

## <a name="run-hello-sample"></a>Hello minta futtatásához

Futtatási hello következő parancsok hello minta tooinstall hello csomagokat:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

Hello minta program hello málna Pi most futtathatja. Adja meg a hello parancsot:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

hello következő minta kimenet látható egy példa hello kimeneti hello parancssorba a hello málna Pi látja:

![Raspberry Pi-alkalmazás kimenete][img-raspberry-output]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Hello megoldás irányítópulton kattintson **eszközök** toovisit hello **eszközök** lap. Válassza ki a málna Pi hello **eszközlista**. Válassza a **módszerek**:

    ![Az irányítópult eszközök][img-list-devices]

1. A hello **metódus meghívása** lapon, válassza ki **InitiateFirmwareUpdate** a hello **metódus** legördülő menüből.

1. A hello **FWPackageURI** mezőbe írja be **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**. Ez a fájl hello végrehajtásának hello belső vezérlőprogram 2.0-s verzióját tartalmazza.

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


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
