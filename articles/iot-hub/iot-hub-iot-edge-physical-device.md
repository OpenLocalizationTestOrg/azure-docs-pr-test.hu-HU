---
title: "Azure IoT peremhálózati rendelkező fizikai eszköz aaaUse |} Microsoft Docs"
description: "Hogyan toouse egy Texas eszközök SensorTag eszköz toosend adatok tooan IoT-központot egy málna Pi 3 eszközön futó IoT peremhálózati átjárón keresztül. hello átjáró Azure IoT peremhálózati épül."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a>Azure IoT peremhálózati használatát egy málna Pi tooforward eszköz a felhőbe küldött üzeneteket tooIoT Hub

Ez a forgatókönyv a hello [Bluetooth alacsony energia minta] [ lnk-ble-samplecode] bemutatja, hogyan toouse [Azure IoT peremhálózati] [ lnk-sdk] számára:

* Továbbítsa az eszközről a felhőbe telemetriai tooIoT Hub fizikai eszközről.
* Útvonal-parancsok az IoT-központ tooa fizikai eszközről.

A bemutató tartalma:

* **Architektúra**: hello Bluetooth alacsony energia minta fontos architekturális információkat.
* **Létrehozása és futtatása**: hello lépéseket szükséges toobuild és futtatási hello minta.

## <a name="architecture"></a>Architektúra

hello a bemutató ismerteti, hogyan toobuild, és futtassa az IoT-átjárónak egy málna Pi 3 futtató Raspbian Linux. hello átjáró IoT peremhálózati épül. hello minta egy Texas eszközök SensorTag Bluetooth alacsony energia (int) eszköz toocollect hőmérséklet-adatokat használja.

Az IoT-átjárónak hello futtatásakor azt:

* Kapcsolatot hoz létre tooa SensorTag eszköz hello Bluetooth alacsony energia (int) protokoll használatával.
* TooIoT Hub csatlakozik hello HTTP protokoll használatával.
* Hello SensorTag eszköz tooIoT Hub telemetriai továbbítja.
* Az IoT-központ toohello SensorTag eszközről parancsok irányítja.

hello IoT peremhálózati modulok a következő hello tartalmaz:

* A *BLA modul* , amely egy táblázat eszköz tooreceive hőmérséklet származó adatokkal hello eszköz és küldési parancsok toohello eszköz felületek.
* A *BLA felhő toodevice modul* , amely az eszköz küldi hello BLA utasításokat az IoT-központ JSON-üzenetek *BLA modul*.
* A *naplózó modul* , amely az összes átjáró üzenetek tooa helyi fájl naplózza.
* Egy *identitás hozzárendelési modul* , amely átalakítja az int eszköz MAC-címek és Azure IoT Hub eszköz identitásokat.
* Egy *IoT-központ modul* , amely feltölt telemetriai adatok tooan IoT-központot, és eszközparancsok fogad egy IoT-központot.
* A *BLA nyomtató modul* , telemetriai hello BLA eszközről értelmezi, és kiírja a formázott adatok toohello konzol tooenable hibaelhárítási és hibakeresési.

### <a name="how-data-flows-through-hello-gateway"></a>Hogyan adatáramlás hello átjárón keresztül

a következő blokk diagram hello hello telemetriai adatainak feltöltése adatok folyamata csővezeték mutatja be:

![Telemetriai adatainak feltöltése átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

hello telemetriai elem történik, az a táblázat eszköz tooIoT utazás Hub lépésekre:

1. hello BLA eszköz hőmérséklet minta hoz létre, és elküldi azt Bluetooth toohello BLA modul az hello átjáró keresztül.
1. hello BLA modul hello minta kap, és közzéteszi azokat toohello broker együtt hello hello eszköz MAC-címét.
1. hello identitás hozzárendelési modul szerzi be ezt az üzenetet, és az IoT Hub eszköz identitásának egy belső tábla tootranslate hello hello eszköz MAC-címét használja. Az IoT-központ eszközidentitás Eszközazonosító és eszközkulcs áll.
1. hello identitás hozzárendelési modul tesz közzé egy új hello hőmérséklet mintaadatok, hello eszköz hello Eszközazonosító és hello eszközkulcs hello MAC-címét tartalmazó üzenetet.
1. az IoT-központ modul hello (hello identitás hozzárendelési modul által előállított) az új üzenetet kap, és közzéteszi azokat tooIoT Hub.
1. hello naplózó modul hello broker tooa helyi fájl összes üzenetet naplózza.

a következő blokk diagram hello hello eszköz parancs adatok áramlási folyamat mutatja be:

![Eszköz parancs átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. hello modul rendszeres időközönként lekérdezi az IoT-központ hello új parancs üzenetek IoT-központot.
1. Az IoT-központ modul hello új parancs üzenetet kap, ha azt közzéteszi azokat toohello broker.
1. hello identitás hozzárendelési modul szerzi be üdvözlőüzenetére parancsot, és egy belső tábla tootranslate hello IoT Hub eszköz azonosítója tooa eszköz MAC-címet használ. Majd egy új üzenet hello tulajdonságok memóriatérkép üdvözlőüzenetére hello céleszköz hello MAC-címét tartalmazó tesz közzé.
1. hello BLA felhő eszköz modul szerzi be ezt az üzenetet, és fordítja le azt hello megfelelő BLA utasítás hello BLA modul. Majd egy új üzenet tesz közzé.
1. hello BLA modul szerzi be ezt az üzenetet, és végrehajtja a hello i/o-utasítás által hello BLA eszköz kommunikál.
1. hello naplózási modul hello broker tooa fájlhoz üzenetekhez naplózza.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.

> [!NOTE]
> Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].

SSH-ügyfél az Ön tooremotely hozzáférés hello parancs sor a hello málna Pi asztali gépen tooenable kell.

- A Windows tartalmaz egy SSH-ügyfél. Azt javasoljuk, [PuTTY](http://www.putty.org/).
- A legtöbb Linux terjesztéseket, a Mac OS parancssori segédprogram hello a SSH közé További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Készítse elő a hardvert

Ez az oktatóanyag feltételezi, hogy a egy [Texas eszközök SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) eszköz csatlakoztatva tooa málna Pi 3 Raspbian futtatása.

### <a name="install-raspbian"></a>Raspbian telepítése

Beállítások tooinstall Raspbian következő málna Pi 3 eszközén hello bármelyikét használhatja.

* tooinstall hello legújabb verziójára Raspbian, használjon hello [NOOBS] [ lnk-noobs] grafikus felhasználói felületen.
* Manuálisan [letöltése] [ lnk-raspbian] írási és olvasási hello legújabb kép hello Raspbian operációs rendszer tooan SD-kártya.

### <a name="sign-in-and-access-hello-terminal"></a>Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások

Két beállítások tooaccess terminál környezettel rendelkezik a málna Pi meg:

* Ha a billentyűzet és csatlakoztatott tooyour málna Pi figyelése, használhatja a hello Raspbian grafikus felhasználói Felülettel tooaccess egy terminálablakot.

* Hozzáférés hello parancs vonalát. az SSH használata a asztali gépen málna Pi.

#### <a name="use-a-terminal-window-in-hello-gui"></a>A grafikus felhasználói Felülettel hello terminálablakot használni

hello alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**. Hello tálcán hello grafikus felhasználói Felülettel, elindíthatja a hello **Terminálszolgáltatások** segédprogram használatával egy figyelő hello ikon.

#### <a name="sign-in-with-ssh"></a>SSH bejelentkezés

Parancssori hozzáférést tooyour málna Pi SSH használható. hello cikk [SSH (Secure Shell)] [ lnk-pi-ssh] ismerteti, hogyan tooconfigure a málna Pi az SSH és hogyan tooconnect a [Windows] [ lnk-ssh-windows] vagy [Linux és Mac OS][lnk-ssh-linux].

Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.

### <a name="install-bluez-537"></a>BlueZ 5.37 telepítése

hello BLA modulok konzultáljon toohello Bluetooth hardver hello BlueZ verem keresztül. Szükség van BlueZ 5.37 verziója hello modulok toowork megfelelően. Ezek az utasítások ellenőrizze, hogy hello BlueZ megfelelő verziója telepítve van.

1. Állítsa le a hello aktuális bluetooth démon:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Hello BlueZ függőségek telepítése:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Hello BlueZ forráskódja letölthető bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Bontsa ki a forráskód hello:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Könyvtárak toohello az újonnan létrehozott mappa módosítása:

    ```sh
    cd bluez-5.37
    ```

1. Adja meg a beépített hello BlueZ kód toobe:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Build BlueZ:

    ```sh
    make
    ```

1. Telepítse a BlueZ, ha ezzel végzett építési:

    ```sh
    sudo make install
    ```

1. Systemd bluetooth toohello új bluetooth démon hello fájlban mutat, a szolgáltatás konfigurációjának módosítása `/lib/systemd/system/bluetooth.service`. Hello "ExecStart" sorban cserélje le a következő szöveg hello:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a>Kapcsolat toohello SensorTag eszköz a málna Pi 3 eszközről engedélyezése

Futó hello minta előtt meg kell, hogy a málna Pi 3 csatlakozni tud-e toohello SensorTag eszköz tooverify.

1. Győződjön meg arról hello `rfkill` segédprogram:

    ```sh
    sudo apt-get install rfkill
    ```

1. A hello málna Pi 3 bluetooth feloldása, és ellenőrizze, hogy hello verziószáma **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. tooenter hello interaktív bluetooth rendszerhéj hello bluetooth szolgáltatás kiindulva hajtják végre hello **bluetoothctl** parancs:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Adja meg a hello parancs **bekapcsolási** toopower hello Bluetooth-vezérlő be. hello parancs kimenete hasonló toohello következő adja vissza:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. Hello interaktív bluetooth rendszerhéj, adja meg a hello parancsot **vizsgálhat** tooscan bluetooth-eszközök. hello parancs kimenete hasonló toohello következő adja vissza:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Hello SensorTag eszköz felderíthető tétele hello kis gomb (zöld LED kell flash hello). hello málna Pi 3 hello SensorTag eszköz kell észlelése:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    Ebben a példában láthatja, hogy hello hello SensorTag az eszköz MAC-címét **A0:E6:F8:B5:F6:00**.

1. Kapcsolja ki hello megadásával ellenőrzését **ki vizsgálata** parancs:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Csatlakozás tooyour SensorTag-eszközt a MAC-cím megadásával **csatlakozás \<MAC-cím\>**. a következő minta kimenet hello jobb érthetőség kedvéért bizonyos rövidítése:

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Hello GATT jellemzői hello eszközt újra hello listázhatja **lista-attribútumok** parancsot.

1. Most már leválaszthatja hello segítségével hello eszközről **leválasztása** parancsot, és zárja be a hello bluetooth rendszerhéjból hello segítségével **lépjen ki a** parancs:

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Épp most már készen áll a toorun hello BLA IoT peremhálózati minta a málna Pi 3.

## <a name="run-hello-iot-edge-ble-sample"></a>Hello IoT peremhálózati BLA minta futtatásához

toorun hello IoT peremhálózati BLA minta kell toocomplete három feladatok:

* Adja meg az IoT Hub két minta eszköz.
* Build IoT peremhálózati málna Pi 3 eszközén.
* Konfigurálja, és futtasson hello BLA mintát málna Pi 3 eszközén.

Hello írásának időpontjában IoT peremhálózati csak támogatja BLA modulok Linux rendszert futtató átjárókat.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Az IoT Hub két minta eszközök konfigurálása

* [Létrehoz egy IoT-központot] [ lnk-create-hub] az Azure-előfizetéshez kell hello nevét a hub toocomplete Ez a forgatókönyv. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* Hozzáad egy eszközt nevű **SensorTag_01** tooyour IoT-központot, és jegyezze fel az azonosítót és az eszköz kulcsának. Használhatja a hello [eszköz explorer vagy az IOT hubbal-explorer] [ lnk-explorer-tools] eszközök tooadd az eszköz toohello IoT-központ létrehozta az előző lépésben hello és tooretrieve annak kulcsát. Az eszköz toohello SensorTag eszköz hello átjáró konfigurálásakor képeznek le.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Az Azure IoT peremhálózati létrehozása a Raspberry Pi 3-kiszolgálón

Függőségek telepítése Azure IoT szegély:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

Használjon hello következő tooclone IoT peremhálózati és minden submodules tooyour kezdőkönyvtárral parancsokat:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

Ha rendelkezik a málna Pi 3 hello IoT peremhálózati tárház teljes másolata, hozhat létre hello parancs hello SDK tartalmazó hello mappából a következő használatával:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a>Konfigurálja és hello BLA minta futtatásához a málna Pi 3

toobootstrap és futtatási hello minta, konfigurálnia kell minden egyes IoT peremhálózati modul, amely részt vesz az hello átjáró. Ebben a konfigurációban megadott JSON-fájlt, és konfigurálnia kell az öt részt vevő IoT peremhálózati modulok. Nincs JSON mintafájl nevű hello tárházban **átjáró\_sample.json** használható mint hello kiindulópont saját konfigurációs fájl létrehozása. Ez a fájl megtalálható-e hello **minták/ble_gateway/src** hello IoT peremhálózati tárház helyi példányát mappájában.

hello alábbi szakaszok azt ismertetik, hogyan tooedit Ez a konfiguráció hello BLA minta fájlt, és azt feltételezik, hogy hello IoT peremhálózati tárház megtalálható-e hello **/home/pi/iot-edge /** a málna Pi 3 mappájába. Ha hello tárház máshol, módosítsa a hello elérési utak ennek megfelelően.

#### <a name="logger-configuration"></a>Naplózási konfiguráció

Feltéve, hogy hello átjáró tárházban található hello **/home/pi/iot-edge /** mappa, hello naplózó modul konfigurálása az alábbiak szerint:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>BLA modul konfigurációja

hello mintakonfiguráció hello BLA eszköz azt feltételezi, hogy egy Texas eszközök SensorTag eszközt. Bármely szabványos BLA eszköz, amely működhet, a perifériák GATT kell működnie, de szükség lehet a tooupdate hello GATT jellemző azonosítók és adatokat. Adja hozzá az SensorTag eszköz hello MAC-címe:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Ha nem használ egy SensorTag eszközt, tekintse át a Gedélyezése eszköz toodetermine hello dokumentációja tooupdate hello GATT jellemző azonosítókat és az adatértékek szüksége van.

#### <a name="iot-hub-module"></a>Az IoT-központ modulja

Adja hozzá az IoT Hub hello nevét. hello utótag érték általában **azure-devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identitás-hozzárendelési modul konfigurációja

Adja hozzá a MAC-címét hello a SensorTag eszköz és hello eszköz azonosítója és kulcsa hello **SensorTag_01** hozzáadott tooyour IoT Hub eszköz:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>BLA nyomtató modul konfigurációja

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>BLEC2D modul konfigurációja

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Útválasztási konfigurációja

hello következő a konfiguráció biztosítja hello következő IoT peremhálózati modulok között:

* Hello **naplózó** modul kap, és minden üzenetet naplózza.
* Hello **SensorTag** modul küld üzeneteket tooboth hello **leképezési** és **BLA nyomtató** modulok.
* Hello **leképezési** modul küld üzeneteket toohello **IOT hubbal** modul toobe tooyour IoT-központ felküldve.
* Hello **IOT hubbal** modul küld üzenetek biztonsági toohello **leképezési** modul.
* Hello **leképezési** modul küld üzeneteket toohello **BLEC2D** modul.
* Hello **BLEC2D** modul küld üzenetek biztonsági toohello **Sensor Tag** modul.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

toorun hello minta, mint egy paraméterrel toohello fázis hello elérési toohello JSON konfigurációs fájl **gedélyezése\_átjáró** bináris. hello alábbi parancs feltételezi a hello **gateway_sample.json** konfigurációs fájlt. Ez a parancs végrehajtása a hello **iot-edge** hello málna Pi mappájában:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Szükség lehet a toopress kis hello gombra hello SensorTag eszköz toomake azt felderíthető hello minta futtatása előtt.

Hello minta futtatásához használhatja hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz toomonitor hello üzenetek hello IoT peremhálózati átjáró hello SensorTag eszközről továbbítja. Például az IOT hubbal-Explorerben figyelheti eszközről a felhőbe üzenetek hello a következő parancs használatával:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Üzenetküldés a felhőből az eszközökre

hello BLA modul támogatja az IoT-központ toohello eszközről küldő parancsok is. Használhatja a hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz toosend JSON üzenetek hello BLA átjáró modult toohello BLA eszközön továbbítja.

Ha hello Texas eszközök SensorTag eszközt használ, bekapcsolhatja a piros hello LED-jét, zöld LED vagy berregő parancsokat küld az IoT-központot. Az IoT-központ küldeni a parancsok, először küldése a következő két JSON üzenetek sorrendben hello. Ezután bármelyik hello parancsok tooturn hello fény vagy berregő küldhet.

1. Alaphelyzetbe állítja az összes LED és hello berregő (kikapcsolni őket):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. "Távoli" i/o konfigurálni:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Küldheti el a következő parancsok tooturn hello fény vagy hello SensorTag eszközön berregő hello:

* Piros hello LED bekapcsolása:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Zöld hello LED bekapcsolása:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Hello berregő bekapcsolása:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Következő lépések

Ha toogain tájékozottabbak IoT széle és az egyes kódpéldák kísérletezhet, keresse fel hello fejlesztői oktatóanyagok és erőforrások:

* [Az Azure IoT él][lnk-sdk]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
