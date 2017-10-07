---
title: "egy átjáró tooAzure használatával az Intel NUC IoT Suite aaaConnect |} Microsoft Docs"
description: "Hello Microsoft IoT kereskedelmi átjáró Kit és hello távoli figyelési előkonfigurált megoldást használni. Használjon hello Azure IoT peremhálózati átjáró tooenable egy SensorTag eszköz tooconnect toohello távoli felügyeleti megoldás, telemetriai toohello felhő küldése és meghívni a hello megoldás irányítópultja toomethods válaszol."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Csatlakozzon a távoli felügyeleti előkonfigurált megoldás Azure IoT peremhálózati átjáró toohello és telemetriai adatokat küldhet egy SensorTag a

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse Azure IoT peremhálózati toosend hőmérséklet és a páratartalom adatait SensorTag toohello távoli eszközfigyelésre előre konfigurált megoldás. hello SensorTag toohello Intel NUC átjáró Bluetooth használatával csatlakozik. hello oktatóprogram:

- Az Azure IoT peremhálózati tooimplement minta átjáró.
- mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

## <a name="overview"></a>Áttekintés

Az oktatóanyag befejezése hello a következő lépéseket:

- Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése. Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.
- Az Intel NUC átjáró eszköz toocommunicate a számítógép és a távoli felügyeleti megoldás hello beállítása.
- Az Intel NUC átjáró tooreceive telemetriai beállítása az SensorTag eszközökről, és elküldi a toohello távoli figyelési irányítópulton.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas eszközök BLA SensorTag][lnk-sensortag]. Ez az oktatóanyag telemetriai adatokat lekérdezi hello SensorTag eszköz Bluetooth-on keresztül.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Bluetooth-kapcsolat konfigurálása

Bluetooth szolgáltatást hello Intel NUC tooenable hello SensorTag eszköz tooconnect, telemetriai adatokat küldhet.

### <a name="find-hello-mac-address-of-hello-sensortag"></a>A hello SensorTag hello MAC-cím keresése

1. Hello Intel NUC hello rendszerhéjat futtassa a következő parancs toounblock hello Bluetooth szolgáltatás hello:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Futtatási hello következő parancsok toostart hello Bluetooth Intel NUC hello szolgáltatást, és írja be a hello Bluetooth rendszerhéj:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Futtassa a következő parancs toopower hello Bluetooth tartományvezérlőn hello:

    ```bash
    power on
    ```

    Ha hello, tartományvezérlői, megjelenik egy üzenet **power módosítása a sikeres**.

1. Futtassa a következő parancs tooscan a Bluetooth-eszközök közelben hello:

    ```bash
    scan on
    ```

1. Nyomja le az hello power gombra a hello SensorTag toomake felderíthető azt. zöld hello LED tokenkódot.

1. Amikor megjelenik egy üzenet hello rendszerhéj adott hello vezérlő talált hello SensorTag, jegyezze fel a hello hello eszköz MAC-címét. hello MAC-cím a következőképpen néz **A0:E6:F8:B5:F6:00**. Hello MAC-cím hello oktatóanyag későbbi részében szüksége, hello átjáró konfigurálásakor.

1. Futtassa a következő parancs tooturn ki Bluetooth ellenőrzését hello:

    ```bash
    scan off
    ```

1. Futtassa a következő parancs tooverify, hogy csatlakozhasson toohello SensorTag eszköz hello:

    ```bash
    connect <SensorTag MAC address>
    ```

    Ha a csatlakozás sikeres, a hello rendszerhéj hello üzenetet jelenít meg **sikeres kapcsolat** majd kinyomtatja hello SensorTag eszközére vonatkozó információt. Ha nem sikerül, ellenőrizze a hello SensorTag továbbra is be van kapcsolva.

1. Most hello SensorTag válassza le, és lépjen ki a hello Bluetooth rendszerhéj hello a következő parancsok futtatásával:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a>Hello egyéni IoT peremhálózati modul létrehozása

Most már lefordíthatja hello egyéni IoT peremhálózati modul, amely lehetővé teszi a hello átjáró toosend üzenetek toohello távoli figyelési megoldást igényelnek. Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].

Hello egyéni IoT peremhálózati modulok hello forráskódja letölthető GitHub hello a következő parancsok használatával:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Build hello hello a következő parancsok használatával egyéni IoT peremhálózati modult:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

hello build script hello libsensor2remotemonitoring.so egyéni IoT peremhálózati modul hello build mappába helyezi.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Konfigurálására és futtatására hello IoT peremhálózati átjáró

Most már a SensorTag eszköz tooyour távoli figyelési irányítópult konfigurálhatja hello IoT peremhálózati átjáró toosend telemetriai adatokat. Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].

> [!TIP]
> Ebben az oktatóanyagban hello szabvány használata `vi` hello Intel NUC a szövegszerkesztőben. Ha még nem használta `vi` , el kell végeznie egy bevezető oktatóanyag, például a [Unix - hello vi szerkesztő oktatóanyag] [ lnk-vi-tutorial] toofamiliarize saját kezűleg a szerkesztő. Másik lehetőségként telepítése könnyebben hello [nano](https://www.nano-editor.org/) hello paranccsal szerkesztő `smart install nano -y`.

Nyissa meg hello minta konfigurációs fájlt a hello **vi** szerkesztő hello a következő parancs használatával:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

Keresse meg az alábbi hello konfigurációban hello IOT hubbal modul hello:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Cserélje le a hello helyőrző értékeket az IoT-központ adatokat létrehozott és mentett hello hello elejére ebben az oktatóanyagban. hello érték IoTHubName a következőhöz hasonló **yourrmsolution37e08**, és hello IoTSuffix értéke általában **azure-devices.net**.

Keresse meg az alábbi hello konfigurációban a hello hozzárendelési module hello:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Cserélje le a hello **macAddress** helyőrzőt hello a korábban feljegyzett SensorTag MAC-címét. Cserélje le a hello **deviceID** és **deviceKey** helyőrzőt hello azonosítók és kulcsok hello távoli figyelési megoldást korábban létrehozott hello két eszközökhöz.

Keresse meg az alábbi hello konfigurációban hello SensorTag modul hello:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Cserélje le a hello **eszköz\_mac\_cím** helyőrzőt hello a korábban feljegyzett SensorTag MAC-címét.

Mentse a módosításokat.

Most futtathatja hello átjáró hello a következő parancsok használatával:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

hello IoT peremhálózati átjáró hello Intel NUC kezdődik, és telemetriai küldi hello SensorTag toohello távoli figyelési megoldást:

![Az IoT-átjárónak telemetriai küldi hello SensorTag][img-telemetry]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

## <a name="view-hello-telemetry"></a>Nézet hello telemetriai adat

hello átjáró most hello SensorTag toohello távoli felügyeleti megoldás a telemetriai adatokat küld. Hello telemetriai hello megoldás irányítópultján tekintheti meg. Parancsok tooyour SensorTag eszköz hello átjárón keresztül hello megoldás irányítópultról is küldhet.

- Keresse meg a toohello megoldás irányítópultja.
- Jelölje be hello eszköz hello SensorTag hello a jelölő hello átjárót konfigurált **eszköz tooView** legördülő menüből.
- hello telemetriai hello SensorTag eszközről hello irányítópulton jeleníti meg.

![Hello SensorTag eszközökről telemetriai adatainak megjelenítése][img-telemetry-display]

> [!WARNING]
> Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config]. Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.


## <a name="next-steps"></a>Következő lépések

A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started