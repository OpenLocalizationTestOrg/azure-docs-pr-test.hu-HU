---
title: "M0 toocloud: csatlakozás lágyított M0 Wi-Fi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és csatlakozzon a Adafruit lágyított M0 Wi-Fi tooAzure IoT-központ toosend toohello Azure felhőalapú adatplatform ebben az oktatóanyagban."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a>Csatlakozás Adafruit lágyított M0 Wi-Fi tooAzure hello felhőben az IoT-központ
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Egy BME280 lágyított M0 Wi-Fi és az IoT-központ közötti kapcsolat](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

Ebben az oktatóanyagban meg először a Arduino board használatifeltétel hello alapjait tanulási. Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Mit

Csatlakozás Adafruit lágyított M0 Wi-Fi tooan IoT-központ az Ön által létrehozott. Akkor futtassa a mintaalkalmazást M0 Wi-Fi toocollect hello hőmérséklet és a páratartalom adatok egy BME280. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.


## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az IoT-központ és az eszköz regisztrálása az lágyított M0 WiFi
* Hogyan tooconnect lágyított M0 Wi-Fi hello érzékelő és a számítógép
* Hogyan toocollect érzékelőadatait lágyított M0 Wi-Fi mintaalkalmazás futtatásával
* Hogyan toosend hello érzékelő adatokat tooyour IoT-központ

## <a name="what-you-need"></a>Mi szükséges

![Hello oktatóanyaghoz szükség részei](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete Ez a művelet következő részek a lágyított M0 Wi-Fi Starter Kit hello szüksége:

* hello lágyított M0 Wi-Fi tábla
* Egy Micro USB tooType egy USB-kábellel

A fejlesztési környezet dolgok következő hello is szüksége lesz:

* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* A Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.
* A Wi-Fi M0 lágyított tooconnect vezeték nélküli hálózat.
* Az internetes kapcsolat toodownload hello konfigurációs eszközt.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb. A korábbi hello Azure IoT Hub szalagtár nem működik.

Ha nem rendelkezik érzékelő, a következő elemek hello opcionálisak. Akkor is hello beállítással, szimulált érzékelő adatokat:

* BME280 hőmérséklet és a páratartalom érzékelő
* Egy breadboard
* M/M átkötés fenyegetéseknek

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a>Csatlakozás lágyított M0 Wi-Fi hello érzékelő és a számítógép
Ebben a szakaszban hello érzékelők tooyour board csatlakozzon. Majd csatlakoztassa a eszköz tooyour számítógép további használatra.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a>Csatlakozás egy DHT22 hőmérséklet és a páratartalom érzékelő tooFeather M0 WiFi

Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat használata. Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.

![Kapcsolatok referencia](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Érzékelő PIN-kód használja a következő vezetékezést hello:


| Kezdő (érzékelő)           | Záró (tábla)            | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 27A)            | 3V (PIN-kód 3A)            | Piros kábel     |
| GND (PIN-kód 29A)            | GND [PIN-kód 6]           | Fekete kábel   |
| SCK (PIN-kód 30A)            | SCK (PIN-kód 12A)          | Sárga kábel  |
| SDO (PIN-kód 31A)            | MI (PIN-kód 14A)           | A fehér kábel   |
| SDI (PIN-kód 32A)            | M0 (PIN-kód 13A)           | Kék kábel    |
| CS (PIN-kód 33A)             | GPIO 5 (PIN-kód 15J)       | Narancssárga kábel  |

További információkért lásd: [Adafruit BME280 páratartalom + légnyomás + hőmérséklet-érzékelő kitörése](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) és [Adafruit lágyított M0 Wi-Fi érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Most már a lágyított M0 Wi-Fi kell csatlakoztatni a működő érzékelő.

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a>Wi-Fi M0 lágyított tooyour számítógép

Használjon hello Micro USB tooType A USB kábel tooconnect lágyított M0 Wi-Fi tooyour számítógépet, látható módon:

![Lágyított Huzzah tooyour számítógép](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Adja hozzá a soros port engedélyek (csak Ubuntu)

Ubuntu használatakor ellenőrizze, rendelkezik hello engedélyek toooperate a hello USB port a lágyított M0 Wi-Fi-e. tooadd soros port engedélyek, kövesse az alábbi lépéseket:


1. A terminálon futtassa a következő parancsok hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   A következő kimenetek hello egyik beolvasása:

   * crw-rw---1 legfelső szintű uucp xxxxxxxx
   * crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx

   Figyelje meg, hogy hello kimenet `uucp` vagy `dialout` hello csoport tulajdonos neve hello USB-porttal.

2. tooadd hello toohello felhasználócsoportra, és futtassa a következő parancs hello:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   Hello előző lépésben beszerzett hello csoport tulajdonos neve `<group-owner-name>`. Ubuntu felhasználónév `<username>`.

3. Hello módosítás tooappear, az Ubuntu kijelentkezni, és jelentkezzen be újra.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ

Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást a lágyított M0 Wi-Fi. hello mintaalkalmazás teszi hello LED pislogás lágyított M0 Wi-Fi be. Majd küldi hello hőmérséklet és a páratartalom adatgyűjtés hello BME280 érzékelő tooyour az IoT-központ.

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a>Hello mintaalkalmazás beszerzése a Githubról és hello Arduino IDE előkészítése

hello mintaalkalmazást a Githubon található. Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást. tooclone hello minta tárház, kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssort vagy terminálablakot.

2. Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.
3. Futtassa a következő parancs hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a>Hello csomag telepítése a lágyított M0 Wi-Fi hello Arduino IDE

1. Nyissa meg a mintaalkalmazás hello tároló hello mappát.

2. Nyissa meg a hello app.ino fájlt hello Arduino IDE hello alkalmazás mappájában.

   ![Nyissa meg a mintaalkalmazás hello Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Kattintson a **fájl** > **beállítások** (Windows/Linux) vagy **Arduino** > **beállítások** (Mac), másolja és hello csatolás alatt a hello **további modulok Manager URL-címet** hello Arduino IDE beállítások lehetőséget.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Kattintson a **eszközök** > **Board** > **modulok Manager**, és telepítse a hello `Arduino SAMD Boards` verzió `1.6.2` vagy újabb. 

1. Ezt a hello ugyanazon ablakra, telepítse `Adafruit SAMD Boards` csomag tooadd hello board fájl definíciókat.

   ![hello esp8266 csomag telepítve van](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Kattintson a **eszközök** > **Board** > **Adafruit M0 Wi-Fi**.

5. Illesztőprogramok telepítéséhez (csak Windows). Csatlakoztatásakor lágyított M0 Wi-Fi, szükség lehet a tooinstall illesztőprogramot. Kattintson a [hello letöltési hivatkozás hello weblapon](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello illesztőprogram telepítő. Hajtsa végre a hello lépéseket tooinstall hello illesztőprogramokat.

### <a name="install-necessary-libraries"></a>Szükséges kódtárak telepítése

1. Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.

2. Keresse meg a következő könyvtár nevek egyenként hello. Az egyes tárak talált, kattintson a **telepítése**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Telepítse manuálisan `Adafruit_WINC1500`. Nyissa meg túl[ezen a webhelyen](https://github.com/adafruit/Adafruit_WINC1500) kattintson **Klónozás vagy letöltési** > **töltse le a ZIP-**. A Arduino ide folytassa túl**vázlat** > **közé tartozik könyvtár** > **.zip könyvtár hozzáadása** , és adja hozzá a hello zip-fájl.

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Hello mintaalkalmazás használja, ha nincs valós BME280 érzékelő

Ha nincs valós BME280 érzékelő, hello mintaalkalmazás szimulálhatja hőmérséklet és a páratartalom adatokat. tooset hello minta alkalmazás szimulált toouse adatokat, kövesse az alábbi lépéseket:

1. Nyissa meg hello `config.h` hello fájlban `app` mappát.

2. Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Mentse a fájlt hello `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a>Hello minta alkalmazás tooFeather M0 Wi-Fi telepítése

1. Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a lágyított M0 Wi-Fi hello soros port.

2. Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooFeather M0 Wi-Fi telepítése.

### <a name="enter-your-credentials"></a>Adja meg hitelesítő adatait

Ha sikeresen befejeződött a feltöltés hello, kövesse ezeket a lépéseket tooenter a hitelesítő adatait:

1. Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.

2. Hello soros figyelő ablak hello jobb alsó sarkában, válassza ki a **nincs sor befejezési** hello bal oldali hello legördülő listában.
3. Válassza ki **115200 átviteli** hello legördülő listában a megfelelő hello.
4. Hello hello felső beviteli mezőbe, írja be a következő adatokat, ha Ön hello tooprovide kéri, majd kattintson **küldése**:

   * Wi-Fi SSID
   * Wi-Fi jelszó
   * Eszköz kapcsolati karakterlánc

> [!Note]
> hello hitelesítő adatok hello EEPROM a lágyított M0 Wi-Fi tárolódik. Ha hello lágyított M0 Wi-Fi board hello Visszaállítás gombra kattint, hello mintaalkalmazás megkérdezi tooerase hello információkat. Adja meg `Y` tooerase hello információkat. Megkérdezi, hogy tooprovide hello információ még egyszer.

### <a name="verify-that-hello-sample-application-is-running-successfully"></a>Győződjön meg arról, hogy a hello mintaalkalmazás sikeresen fut.

Ha a hello soros figyelő ablakból parancskimenet hello és hello villogó LED a lágyított M0 Wi-Fi, hello mintaalkalmazás sikeresen fut:

![Végső kimenetet a Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Következő lépések

Sikeresen csatlakoztatva lágyított M0 Wi-Fi tooyour IoT hub és rögzített hello érzékelő adatokat tooyour IoT-központ küldött. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

