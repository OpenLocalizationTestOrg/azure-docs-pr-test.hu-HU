---
title: "az IoT-átjáró tooconnect egy eszköz tooAzure IoT-központ aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Intel NUC egy IoT-átjáró tooconnect egy TI SensorTag, és a küldési érzékelő adatokat tooAzure IoT-központ hello felhő."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-átjáró eszköz toocloud kapcsolódni"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a>Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata

> [!NOTE]
> Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy befejezte [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). A [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), úgy konfigurálja az IoT átjáróként hello Intel NUC eszközt.

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Megismerheti, hogyan toouse egy IoT-átjáró tooconnect egy Texas eszközök SensorTag (CC2650STK) tooAzure IoT-központot. hello IoT átjáró elküldi a hőmérséklet és a páratartalom során összegyűjtött adatokat hello SensorTag tooAzure IoT-központ.

## <a name="what-you-will-do"></a>Mit fog

- Létrehoz egy IoT-központot.
- Eszköz regisztrálása a hello IoT-központ hello SensorTag.
- Hello kapcsolatot az IoT-átjáró hello és hello SensorTag engedélyezése.
- Egy táblázat minta alkalmazás toosend SensorTag adatok tooyour IoT-központ futtatása.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) befejeződött a amely Intel NUC IoT átjáróként beállítása.
- * Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
- Egy SSH-ügyfél rendszert futtató számítógépen. A puTTY Windows ajánlott. Linux- és macOS egy SSH-ügyfél már rendelkeznek.
- hello IP-cím és hello felhasználónév és jelszó tooaccess hello átjáró hello SSH-ügyfél.
- Az internethez.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Itt az új eszköz regisztrálásához a SensorTag

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a>Hello kapcsolatot az IoT-átjáró hello és hello SensorTag engedélyezése

Ebben a szakaszban hajtsa végre a következő feladatok hello:

- Bluetooth-kapcsolat beolvasása hello SensorTag hello MAC-címét.
- A hello IoT átjáró toohello SensorTag Bluetooth kapcsolat létrehozására.

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a>Bluetooth-csatlakozási hello MAC-címét hello SensorTag lekérése

1. Hello állomáson hello SSH-ügyfél, és csatlakozzon a toohello IoT átjáró.
1. Bluetooth feloldása hello a következő parancs futtatásával:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Az IoT-átjárón hello hello Bluetooth szolgáltatás elindítása, és írja be a Bluetooth rendszerhéj tooconfigure futtatásával Bluetooth hello a következő parancsokat:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Hello Bluetooth-vezérlő futtatásával bekapcsolás hello a következő parancsot a következő hello Bluetooth rendszerhéj:

   ```bash
   power on
   ```

   ![az IoT-átjárót hello bluetoothctl hello Bluetooth-vezérlő bekapcsolása](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Indítsa el a Bluetooth-eszközök közelben hello a következő parancs futtatásával keresése:

   ```bash
   scan on
   ```

   ![Bluetooth-eszközök a bluetoothctl közelben vizsgálata](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Nyomja le az hello párosítás hello SensorTag gombjára. zöld hello a hello SensorTag villanás vezetett.
1. A Bluetooth rendszerhéj hello meg kell jelennie hello SensorTag megtalálható-e. Jegyezze fel a hello hello SensorTag MAC-címét. Ebben a példában a hello hello SensorTag MAC-címét a `24:71:89:C0:7F:82`.
1. Kapcsolja ki a hello vizsgálat hello a következő parancs futtatásával:

   ```bash
   scan off
   ```

   ![Állítsa le a közeli bluetoothctl a Bluetooth-eszközök keresése](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a>Hello IoT átjáró toohello SensorTag a Bluetooth-kapcsolat kezdeményezéséhez

1. Csatlakozás toohello SensorTag hello a következő parancs futtatásával:

   ```bash
   connect <MAC address>
   ```

   ![Csatlakozás toohello SensorTag bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Hello SensorTag megszakítja a kapcsolatot, és zárja be a hello Bluetooth rendszerhéj hello a következő parancsok futtatásával:

   ```bash
   disconnect
   exit
   ```

   ![Válassza le a bluetoothctl SensorTag hello](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Sikeresen engedélyezve van a hello kapcsolat hello SensorTag és hello IoT-átjáró között.

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a>Egy táblázat minta alkalmazás toosend SensorTag adatok tooyour IoT-központ futtatása

Bluetooth alacsony energia (int) mintaalkalmazás hello Azure IoT peremhálózati biztosítja. hello mintaalkalmazás BLA kapcsolat gyűjti az adatokat, és küldje hello adatok tooyou IoT-központot. toorun hello mintaalkalmazás kell:

1. Hello mintaalkalmazás konfigurálása.
1. Hello mintaalkalmazás futtatása hello IoT-átjárón.

### <a name="configure-hello-sample-application"></a>Hello mintaalkalmazás konfigurálása

1. Nyissa meg a toohello mappa hello mintaalkalmazásnak hello a következő parancs futtatásával:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Nyissa meg a konfigurációs fájl hello hello a következő parancs futtatásával:

   ```bash
   vi ble_gateway.json
   ```

1. Hello konfigurációs fájlt töltse ki a következő értékek hello:

   **IoTHubName**: hello az IoT hub nevét.

   **IoTHubSuffix**: hello elsődleges kulcs hello eszköz kapcsolati karakterlánc le feljegyzett az beszerzése IoTHubSuffix. Győződjön meg arról, hogy hello elsődleges kulcs hello eszköz kapcsolati karakterlánc beolvasása, az IoT hub kapcsolati karakterlánc az elsődleges kulcsa nem hello. hello formátumban van elsődleges kulcs hello hello eszköz kapcsolati karakterlánc `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Átviteli**: hello alapértelmezett értéke `amqp`. Ez az érték hello protokoll transpotation során jeleníti meg. Annak oka az lehet `http`, `amqp`, vagy `mqtt`.

   **macAddress**: hello hello le feljegyzett SensorTag MAC-címét.

   **deviceID**: hello eszközt az IoT hub létrehozott azonosítója.

   **deviceKey**: hello hello eszköz kapcsolati karakterlánc az elsődleges kulcsa.

   ![Teljes hello konfigurációs fájljának hello BLA mintaalkalmazás](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Nyomja le az `ESC` és típus `:wq` toosave hello fájlt.

### <a name="run-hello-sample-application"></a>Hello mintaalkalmazás futtatása

1. Győződjön meg arról, hogy hello SensorTag be van kapcsolva.
1. Futtassa a következő parancs hello:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Következő lépések

[Az IoT-átjáró használata az érzékelő adatokat átalakításhoz Azure IoT oldala](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
