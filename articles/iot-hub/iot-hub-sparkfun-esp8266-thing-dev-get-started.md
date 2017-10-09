---
title: "aaaESP8266 toocloud - csatlakozás Sparkfun ESP8266 dolog fejlesztői tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban toosend adatok toohello Azure cloud platform Sparkfun ESP8266 dolog fejlesztői tooAzure IoT-központ az csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a>Csatlakozás Sparkfun ESP8266 dolog fejlesztői tooAzure hello felhőben az IoT-központ

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 dolog fejlesztői és az IoT-központ közötti kapcsolat](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Mit fog

Csatlakoztassa a Sparkfun ESP8266 dolog fejlesztői tooan IoT hubot hoz létre. Futtassa a mintaalkalmazást ESP8266 toocollect hőmérséklet és a páratartalom adatok egy DHT22 érzékelő. Végezetül küldése hello érzékelő adatokat tooyour IoT-központot.

> [!NOTE]
> Ha más ESP8266 modulok használ, továbbra is kövesse ezeket a lépéseket tooconnect azt tooyour IoT-központot. Attól függően, hogy hello ESP8266 üzenőfalon használ, szükség lehet a tooreconfigure hello `LED_PIN`. Például ha az Eszközintelligencia-Thinker ESP8266 használ, módosíthatja az `0` túl`2`. Még nem rendelkezik a csomag?: kattintson a [Itt](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Amiről tanulni fog

* Hogyan toocreate az IoT-központ és az eszköz regisztrálása dolog helyőrzőkivétel fejlesztői
* Hogyan tooconnect dolog fejlesztői hello érzékelő és a számítógép.
* Hogyan toocollect érzékelőadatait dolog helyőrzőkivétel fejlesztői mintaalkalmazás futtatásával
* Hogyan toosend hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-will-need"></a>Mit kell

![Hello oktatóanyaghoz szükség részei](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete Ez a művelet következő részek a dolog fejlesztői Starter Kit hello szüksége:

* hello Sparkfun ESP8266 dolog fejlesztői tábla.
* Micro USB tooType egy USB-kábellel.

A fejlesztési környezetet is kell hello következő:

* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.
* Sparkfun ESP8266 dolog fejlesztői tooconnect a vezeték nélküli hálózatot.
* Internet kapcsolat toodownload hello konfigurációs eszközt.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verziót (vagy újabb), korábbi verziói hello AzureIoT szalagtár nem működik.

hello következő elemeket nem kötelező abban az esetben, ha nincs érzékelő. Akkor is hello szimulált érzékelőadatait használatának lehetőségét.

* Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő.
* Egy breadboard.
* M/M átkötés fenyegetéseknek.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a>Csatlakozás ESP8266 dolog fejlesztői hello érzékelő és a számítógép

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a>Csatlakozás DHT22 hőmérséklet és a páratartalom érzékelő tooESP8266 dolog fejlesztői

Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat a következőképpen használhatja. Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.

![Kapcsolatok referencia](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Az érzékelő PIN-kód a következő vezetékezést hello használjuk:

| Indítsa el a (érzékelő)           | Záró (tábla)           | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 27F)            | 3V (PIN-kód 8A)           | Piros kábel     |
| ADATOK (PIN-kód 28F)           | GPIO 2 (PIN-kód 9A)       | A fehér kábel    |
| GND (PIN-kód 30F)            | GND (PIN-kód 7J)          | Fekete kábel   |


- További információkért lásd: [DHT22 érzékelő telepítő](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) és [Sparkfun ESP8266 dolog fejlesztői specifikációja](https://www.sparkfun.com/products/13711)

Most már a Sparkfun ESP8266 dolog fejlesztői kell csatlakoztatni a működő érzékelő.

![Csatlakozás dht22 ESP8266 dolog fejlesztői](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a>Sparkfun ESP8266 dolog fejlesztői tooyour számítógép

Hello Micro USB tooType A USB kábel tooconnect Sparkfun ESP8266 dolog fejlesztői tooyour számítógép a következőképpen használhatja.

![lágyított huzzah tooyour számítógép](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Soros port engedélyek – csak Ubuntu hozzáadása

Ubuntu használatakor ellenőrizze, hogy a normál felhasználói rendelkezik hello engedélyek toooperate a hello USB port a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői a szokványos felhasználói engedélyeinek tooadd soros port kövesse az alábbi lépéseket:

1. Futtassa a következő parancsokat a terminálon hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   A következő kimenetek hello egyik beolvasása:

   * crw-rw---1 legfelső szintű uucp xxxxxxxx
   * crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx

   Hello kimenet, figyelje meg `uucp` vagy `dialout` , amely hello tulajdonos neve hello USB-porttal.

1. Hello felhasználói toohello csoport hozzáadása hello a következő parancs futtatásával:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`hello kapott csoport tulajdonos neve van hello előző lépésben. `<username>`Ubuntu felhasználónevének van.

1. Jelentkezzen ki Ubuntu, és jelentkezzen be azt újra az tootake hatás hello módosítása.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ

Ebben a részben telepíti, és futtassa a mintaalkalmazást a Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői hello mintaalkalmazás villogjon-hello LED Sparkfun ESP8266 dolog fejlesztői a, és küldi hello hőmérséklet és a páratartalom adatgyűjtés hello DHT22 érzékelő tooyour az IoT-központ.

### <a name="get-hello-sample-application-from-github"></a>Hello mintaalkalmazás beszerzése a Githubról

hello mintaalkalmazást a Githubon található. Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást. tooclone hello minta tárház, kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssort vagy terminálablakot.
1. Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.
1. Futtassa a következő parancs hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Sparkfun ESP8266 dolog fejlesztői Arduino ide hello telepítéséhez:

1. Nyissa meg a mintaalkalmazás hello tároló hello mappát.
1. Nyissa meg a hello app.ino fájlt a Arduino IDE hello app mappában.

   ![Nyissa meg a mintaalkalmazás hello arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Kattintson Arduino IDE hello, **fájl** > **beállítások**.
1. A hello **beállítások** párbeszédpanel hello ikon következő toohello kattintson **további modulok Manager URL-címet** szövegmezőben.
1. Hello előugró ablak, írja be a következő URL-cím hello, és kattintson **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![a arduino ide pont tooa alkalmazáscsomag URL-címe](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. A hello **preferencia** párbeszédpanel, kattintson a **OK**.
1. Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.
   ESP8266 2.2.0 vagy újabb verziójával kell telepíteni.

   ![hello esp8266 csomag telepítve van](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Kattintson a **eszközök** > **Board** > **Sparkfun ESP8266 dolog fejlesztői**.

### <a name="install-necessary-libraries"></a>Szükséges kódtárak telepítése

1. Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.
1. Keresse meg a következő könyvtár nevek egyenként hello. Az egyes hello könyvtárban talál, kattintson a **telepítése**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Nincs valós DHT22 érzékelő?

hello mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő. tooenable hello alkalmazás szimulált toouse mintaadatok, kövesse az alábbi lépéseket:

1. Nyissa meg hello `config.h` hello fájlban `app` mappát.
1. Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Mentse `Control-s`.

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a>Hello minta alkalmazás tooSparkfun ESP8266 dolog fejlesztői telepítése

1. Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a soros port hello Sparkfun ESP8266 dolog helyőrzőkivétel fejlesztői
1. Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooSparkfun ESP8266 dolog helyőrzőkivétel fejlesztői telepítése

> [!Note]
> MacOS használata üzenetek feltöltése során a következő hello valószínűleg látni sikerült. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Nyissa meg a ternimal ablakot, és a probléma alatt műveletek toosolve Befejezés.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Adja meg hitelesítő adatait

Ha sikeresen befejeződött a feltöltés hello, kövesse a hitelesítő adatok hello lépéseket tooenter:

1. Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.
1. Hello soros figyelő ablakban figyelje meg hello két legördülő listák hello jobb alsó sarkában.
1. Válassza ki **nincs sor befejezési** hello bal oldali legördülő listát.
1. Válassza ki **115200 átviteli** hello jobb legördülő listát.
1. Hello beviteli mezőbe hello soros figyelő ablak hello tetején található, írja be a következő információ, ha a rendszer felkéri tooprovide hello őket, és kattintson a **küldése**.
   * Wi-Fi SSID
   * Wi-Fi jelszó
   * Eszköz kapcsolati karakterlánc

> [!Note]
> hello hitelesítő adatokat a Sparkfun ESP8266 dolog fejlesztési EEPROM hello van tárolva. Ha hello Sparkfun ESP8266 dolog fejlesztői board hello Visszaállítás gombra kattint, hello mintaalkalmazás kéri, ha azt szeretné, tooerase hello információkat. Adja meg `Y` toohave hello információk törlése és a rendszer kéri-e újra tooprovide hello információt.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Győződjön meg arról hello mintaalkalmazás sikeresen fut.

Ha a hello hello soros figyelő ablakból parancskimenet, és hello villogó LED a Sparkfun ESP8266 dolog fejlesztői, hello mintaalkalmazás sikeresen fut-e.

![végső kimenetet a arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Következő lépések

Sikeresen csatlakozott egy Sparkfun ESP8266 dolog fejlesztői tooyour IoT-központ és rögzített hello érzékelő adatokat tooyour IoT-központ küldött. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
