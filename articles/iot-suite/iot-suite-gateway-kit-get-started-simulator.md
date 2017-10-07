---
title: "egy átjáró tooAzure használatával az Intel NUC IoT Suite aaaConnect |} Microsoft Docs"
description: "Hello Microsoft IoT kereskedelmi átjáró Kit és hello távoli figyelési előkonfigurált megoldást használni. Használjon hello Azure IoT peremhálózati átjáró tooconnect toohello távoli felügyeleti megoldás, szimulált telemetriai toohello felhő küldhet, és válaszolhat a toomethods hello megoldás irányítópultja meghívni."
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
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Csatlakozzon a távoli felügyeleti előkonfigurált megoldás Azure IoT peremhálózati átjáró toohello és szimulált telemetriai adatokat küldhet

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Az oktatóanyag bemutatja, hogyan toouse Azure IoT peremhálózati toosimulate hőmérséklet és a páratartalom adatok toosend toohello távoli felügyelet előre megoldás. hello oktatóprogram:

- Az Azure IoT peremhálózati tooimplement minta átjáró.
- mint hello felhőalapú háttér hello IoT Suite távoli megfigyelési előre konfigurált megoldás.

## <a name="overview"></a>Áttekintés

Az oktatóanyag befejezése hello a következő lépéseket:

- Hello távoli figyelési előkonfigurált megoldás tooyour Azure-előfizetés-példányt telepítése. Ebben a lépésben automatikusan telepíti és konfigurálja a több Azure-szolgáltatásokhoz.
- Az Intel NUC átjáró eszköz toocommunicate a számítógép és a távoli felügyeleti megoldás hello beállítása.
- Konfigurálja a hello IoT peremhálózati átjáró toosend szimulált telemetriai adatokból, hogy a hello megoldás irányítópultja tekintheti meg.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> távoli hello figyelési megoldást kiosztja az Azure-előfizetéshez az Azure szolgáltatások. hello központi telepítés által adott jelentéseket tükrözik a valós vállalati architektúra. tooavoid szükségtelen Azure felhasználási díjak, az előre konfigurált hello megoldást a következő azureiotsuite.com példányának törlése, vele befejezése után. Ha újra kell hello előkonfigurált megoldás, egyszerűen létrehozhatja azt. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Ismételje meg a hello előző lépéseket tooadd egy második eszköz, például egy eszköz azonosítójával **device02**. hello minta adatot küld a két szimulált eszköz hello átjáró toohello távoli figyelési megoldást igényelnek.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

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
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

hello build script hello libsimulator.so egyéni IoT peremhálózati modul hello build mappába helyezi.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Konfigurálására és futtatására hello IoT peremhálózati átjáró

Hello IoT peremhálózati átjáró toosend szimulált telemetriai tooyour távoli figyelési irányítópult mostantól konfigurálható. Átjáró és az IoT-Edge modulok konfigurálásával kapcsolatos további információkért lásd: [Azure IoT peremhálózati fogalmak][lnk-gateway-concepts].

> [!TIP]
> Ebben az oktatóanyagban hello szabvány használata `vi` hello Intel NUC a szövegszerkesztőben. Ha még nem használta `vi` , el kell végeznie egy bevezető oktatóanyag, például a [Unix - hello vi szerkesztő oktatóanyag] [ lnk-vi-tutorial] toofamiliarize saját kezűleg a szerkesztő. Másik lehetőségként telepítése könnyebben hello [nano](https://www.nano-editor.org/) hello paranccsal szerkesztő `smart install nano -y`.

Nyissa meg hello minta konfigurációs fájlt a hello **vi** szerkesztő hello a következő parancs használatával:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Cserélje le a hello **deviceID** és **deviceKey** helyőrzőt hello azonosítók és kulcsok hello távoli figyelési megoldást korábban létrehozott hello két eszközökhöz.

Mentse a módosításokat.

Most futtathatja hello IoT peremhálózati átjáró használatával hello a következő parancsokat:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

hello átjáró hello Intel NUC kezdődik, és elküldi a szimulált telemetriai toohello távoli figyelési megoldást:

![Az IoT-átjárónak állít elő, szimulált telemetriai adat][img-simulated telemetry]

Nyomja le az **Ctrl-C** tooexit hello program tetszőleges időpontban.

## <a name="view-hello-telemetry"></a>Nézet hello telemetriai adat

hello IoT peremhálózati átjáró most küld szimulált telemetriai toohello távoli figyelési megoldást igényelnek. Hello telemetriai hello megoldás irányítópultján tekintheti meg.

- Keresse meg a toohello megoldás irányítópultja.
- Válasszon ki egy konfigurált hello hello átjáró két hello eszközök **eszköz tooView** legördülő menüből.
- hello telemetriai hello átjáró eszközökről hello irányítópulton jeleníti meg.

![A szimulált hello átjáró eszközökről telemetriai megjelenítése][img-telemetry-display]

> [!WARNING]
> Ha nem adja meg hello távoli felügyeleti megoldás az Azure-fiókjával fut, a hello futásakor kell fizetni. Hello távoli figyelési megoldást futtatása közben felhasználás csökkentése kapcsolatos további információkért lásd: [konfigurálása Azure IoT Suite megoldások bemutató céljára előre konfigurált][lnk-demo-config]. Ha befejezte, használja előre konfigurált hello megoldás törlése az Azure-fiókjával.

## <a name="next-steps"></a>Következő lépések

A Microsoft hello [Azure IoT-fejlesztői központhoz](https://azure.microsoft.com/develop/iot/) további mintákat és Azure IoT-dokumentációja.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started