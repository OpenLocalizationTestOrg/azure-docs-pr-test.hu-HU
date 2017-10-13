---
title: "Felhőbe - ESP8266 lágyított HUZZAH ESP8266 csatlakozni az Azure IoT Hub |} Microsoft Docs"
description: "Megtudhatja, hogyan kell beállítania, és Azure IoT-központ az adatokat küldeni az Azure felhőalapú platform ebben az oktatóanyagban Adafruit lágyított HUZZAH ESP8266 csatlakozni."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 6a450579c848fe6030a328ddf410f139baae2324
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Csatlakozás Adafruit lágyított HUZZAH ESP8266 Azure IoT Hub a felhőben

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 lágyított HUZZAH ESP8266 és az IoT-központ közötti kapcsolat](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Mit


Az IoT-központ az Ön által létrehozott Adafruit lágyított HUZZAH ESP8266 csatlakozni. Majd futtassa a mintaalkalmazást a ESP8266 DHT22 érzékelő a hőmérséklet és a páratartalom adatokat gyűjteni. Végezetül az érzékelő adatokat küldött az IoT hub.

> [!NOTE]
> Más ESP8266 modulok használata, akkor is továbbra is kövesse az alábbi lépéseket az IoT hub csatlakozni. Attól függően, hogy a ESP8266 üzenőfalon használ, akkor módosítania kell, hogy engedélyezzék a `LED_PIN`. Például az Eszközintelligencia-Thinker ESP8266 használata, érdemes lehet módosítani az `0` való `2`. Még nem rendelkezik egy csomagot? Az beszerzése a [Azure-webhelyen](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Ismertetett témák

* Létrehoz egy IoT-központot, és lágyított HUZZAH ESP8266 az eszköz regisztrálása
* Lágyított HUZZAH ESP8266 az érzékelő és a számítógép összekapcsolása
* Futtatja a mintaalkalmazás lágyított HUZZAH ESP8266 érzékelő adatainak gyűjtéséről
* Útmutató az érzékelő adatokat küldeni az IoT hub

## <a name="what-you-need"></a>Mi szükséges

![Az oktatóanyaghoz szükség részei](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

A művelet végrehajtásához a következő részek a lágyított HUZZAH ESP8266 Starter Kit kell:

* A lágyított HUZZAH ESP8266 board
* A típus egy USB-kábel Micro USB

A fejlesztési környezetet is kell a következőket:

* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.
* Vezeték nélküli hálózat lágyított HUZZAH ESP8266 való csatlakozáshoz.
* A kiszolgálókonfigurációs eszköz letöltéséhez internetkapcsolat.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb. Korábbi verzióiban a AzureIoT szalagtár nem működik.

A következő elemek nem kötelező, abban az esetben, ha nincs érzékelő. Akkor is a szimulált érzékelőadatait használatának lehetőségét.

* Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő
* Egy breadboard
* M/M átkötés fenyegetéseknek


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Csatlakozás lágyított HUZZAH ESP8266 az érzékelő és a számítógép
Ebben a szakaszban csatlakozhat az érzékelők a tábla. Majd a eszközt csatlakoztat a számítógéphez további használatra.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Csatlakozás lágyított HUZZAH ESP8266 DHT22 hőmérséklet és a páratartalom érzékelő

Használják a breadboard és átkötés az alábbiak szerint a kapcsolathoz. Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.

![Kapcsolatok referencia](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


PIN-kód érzékelő használja a következő vezetékezést:


| Indítsa el a (érzékelő)           | Záró (tábla)           | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 31F)            | 3V (PIN-kód 58H)           | Piros kábel     |
| ADATOK (PIN-kód 32F)           | GPIO 2 (PIN-kód 46A)       | Kék kábel    |
| GND (PIN-kód 34F)            | GND (PIN-kód 56I)          | Fekete kábel   |

További információkért lásd: [Adafruit DHT22 érzékelő telepítő](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) és [Adafruit lágyított HUZZAH Esp8266 érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Most már a lágyított Huzzah ESP8266 kell csatlakoztatni a működő érzékelő.

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Lágyított HUZZAH ESP8266 kapcsolódni a számítógéphez

Látható tovább, a Micro USB,-típus egy USB-kábel segítségével lágyított HUZZAH ESP8266 kapcsolódni a számítógéphez.

![Lágyított Huzzah kapcsolódni a számítógéphez](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Adja hozzá a soros port engedélyek (csak Ubuntu)


Ha Ubuntu használ, győződjön meg arról a engedélye ahhoz, hogy a lágyított HUZZAH ESP8266 USB port működik. Soros port engedélyek hozzáadásához kövesse az alábbi lépéseket:


1. A következő parancsokat a terminálon:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   A következő kimenetek egyik beolvasása:

   * crw-rw---1 legfelső szintű uucp xxxxxxxx
   * crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx

   Figyelje meg, hogy a kimenet `uucp` vagy `dialout` a csoport tulajdonosának neve az USB-porttal.

1. A felhasználó felvétele a csoportba a következő parancs futtatásával:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`az az előző lépésben beolvasott tulajdonos neve van. `<username>`Ubuntu felhasználónevének van.

1. Ubuntu kijelentkezni, és jelentkezzen be újból a módosítás jelennek meg.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Érzékelő adatokat gyűjteni, és küldje el az IoT hub

Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást lágyított HUZZAH ESP8266. A mintaalkalmazás az lágyított HUZZAH ESP8266 LED villogjon, és elküldi a hőmérséklet és a páratartalom gyűjtött adatokat a DHT22 érzékelő az IoT hubhoz.

### <a name="get-the-sample-application-from-github"></a>A mintaalkalmazás beszerzése a Githubról

A mintaalkalmazás tárolja a Githubon. Klónozza a minta-tárház, amely tartalmazza a mintaalkalmazást a Githubról. A minta-tárház klónozása, kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssort vagy terminálablakot.
1. Nyissa meg a mappát, ahol a mintaalkalmazáshoz történő tárolását.
1. Futtassa az alábbi parancsot:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

A csomag telepítése lágyított HUZZAH ESP8266 a Arduino ide:

1. Nyissa meg a mappát, ahol a mintaalkalmazás tárolja.
1. Nyissa meg a app.ino fájlt a Arduino ide app mappában.

   ![Nyissa meg a mintaalkalmazás Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Kattintson a Arduino ide **fájl** > **beállítások**.
1. Az a **beállítások** párbeszédpanel mellett kattintson a ikonra a **további modulok Manager URL-címet** mezőbe.
1. Az előugró ablakban írja be a következő URL-címet, és kattintson **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Mutasson a Arduino IDE egy alkalmazáscsomag URL-címe](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. Az a **preferencia** párbeszédpanel, kattintson a **OK**.
1. Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.

   Modulok Manager, az azt jelenti, hogy telepítve van-e a ESP8266 2.2.0 vagy újabb verziójával.

   ![A esp8266 csomag telepítve van](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Kattintson a **eszközök** > **Board** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Szükséges kódtárak telepítése

1. Kattintson a Arduino ide **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.
1. Keresse meg a következő könyvtár nevek egyenként. Az egyes tárak talált, kattintson a **telepítése**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Nincs valós DHT22 érzékelő?

A mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő. Állítsa be a mintaalkalmazást szimulált adatokat, kövesse az alábbi lépéseket:

1. Nyissa meg a `config.h` fájlt a `app` mappát.
1. Keresse meg a következő kódsort, és módosítsa az értéket `false` való `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurálja a mintaalkalmazás szimulált adatok használata](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Mentse a fájlt a `Control-s`.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>A mintaalkalmazás lágyított HUZZAH ESP8266 történő telepítése

1. Kattintson a Arduino ide **eszköz** > **Port**, majd kattintson a soros port a lágyított HUZZAH ESP8266.
1. Kattintson a **vázlat** > **feltöltése** felépítéséhez és az lágyított HUZZAH ESP8266 minta alkalmazást telepíti.

### <a name="enter-your-credentials"></a>Adja meg hitelesítő adatait

Ha sikeresen befejeződött a feltöltés, kövesse az alábbi lépéseket a hitelesítő adatok megadását:

1. Kattintson a Arduino ide **eszközök** > **soros figyelő**.
1. A soros figyelő ablakban figyelje meg, a két legördülő lista jobb alsó sarkában.
1. Válassza ki **nincs sor befejezési** a bal oldali legördülő listát.
1. Válassza ki **115200 átviteli** jobb legördülő listája.
1. A beviteli mezőbe, a soros figyelő ablak tetején található, adja meg az az alábbi adatokat, ha a rendszer kéri azokat, és kattintson a **küldése**.
   * Wi-Fi SSID
   * Wi-Fi jelszó
   * Eszköz kapcsolati karakterlánc

> [!Note]
> A hitelesítő adatokat a lágyított EEPROM HUZZAH ESP8266 tárolódik. Ha a lágyított HUZZAH ESP8266 táblán a Visszaállítás gombra kattint, a mintaalkalmazást megkérdezi, hogy szeretné-e használni a. Adja meg `Y` az adatok törlése. Adja meg az adatokat a második alkalommal kéri.

### <a name="verify-the-sample-application-is-running-successfully"></a>Ellenőrizze a mintaalkalmazás sikeresen fut-e

A következő kimeneti a soros figyelő ablakból és a villogó LED lágyított HUZZAH ESP8266 látható, ha sikeresen fut a mintaalkalmazáshoz.

![Végső kimenetet a Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Következő lépések

Sikeresen egy lágyított HUZZAH ESP8266 csatlakozik az IoT hub, és a rögzített érzékelő adatokat küldött az IoT hub. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

