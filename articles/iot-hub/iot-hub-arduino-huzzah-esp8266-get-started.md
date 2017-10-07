---
title: "aaaESP8266 toocloud - csatlakozás lágyított HUZZAH ESP8266 tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban toosend adatok toohello Azure cloud platform Adafruit lágyított HUZZAH ESP8266 tooAzure IoT-központ az csatlakoznak."
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
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a>Csatlakozás Adafruit lágyított HUZZAH ESP8266 tooAzure hello felhőben az IoT-központ

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![DHT22 lágyított HUZZAH ESP8266 és az IoT-központ közötti kapcsolat](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Mit


Csatlakozás Adafruit lágyított HUZZAH ESP8266 tooan IoT-központ az Ön által létrehozott. Ezután egy DHT22 érzékelő adatokon ESP8266 toocollect hello hőmérséklet és a páratartalom futtassa a mintaalkalmazást. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.

> [!NOTE]
> Ha más ESP8266 modulok használata, továbbra is kövesse ezeket a lépéseket tooconnect azt tooyour IoT-központot. Attól függően, hogy hello ESP8266 üzenőfalon használ, szükség lehet a tooreconfigure hello `LED_PIN`. Például az Eszközintelligencia-Thinker ESP8266 használata, érdemes lehet módosítani az `0` túl`2`. Még nem rendelkezik egy csomagot? Az beszerzése hello [Azure-webhelyen](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az IoT-központ és az eszköz regisztrálása az lágyított HUZZAH ESP8266
* Hogyan tooconnect lágyított HUZZAH ESP8266 hello érzékelő és a számítógép
* Hogyan toocollect érzékelőadatait lágyított HUZZAH ESP8266 mintaalkalmazás futtatásával
* Hogyan toosend hello érzékelő adatokat tooyour IoT-központ

## <a name="what-you-need"></a>Mi szükséges

![Hello oktatóanyaghoz szükség részei](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete Ez a művelet következő részek a lágyított HUZZAH ESP8266 Starter Kit hello szüksége:

* hello lágyított HUZZAH ESP8266 board
* Egy Micro USB tooType egy USB-kábellel

A fejlesztési környezet dolgok következő hello is szüksége lesz:

* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* Mac vagy Windows vagy az Ubuntu rendszert futtató számítógép.
* Lágyított HUZZAH ESP8266 tooconnect a vezeték nélküli hálózatot.
* Internet kapcsolat toodownload hello konfigurációs eszközt.
* [Arduino IDE](https://www.arduino.cc/en/main/software) 1.6.8 verzió vagy újabb. A korábbi hello AzureIoT szalagtár nem működik.

hello következő elemeket nem kötelező abban az esetben, ha nincs érzékelő. Akkor is hello szimulált érzékelőadatait használatának lehetőségét.

* Egy Adafruit DHT22 hőmérséklet és a páratartalom érzékelő
* Egy breadboard
* M/M átkötés fenyegetéseknek


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a>Csatlakozás lágyított HUZZAH ESP8266 hello érzékelő és a számítógép
Ebben a szakaszban hello érzékelők tooyour board csatlakozzon. Majd csatlakoztassa a eszköz tooyour számítógép további használatra.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a>Csatlakozás egy DHT22 hőmérséklet és a páratartalom érzékelő tooFeather HUZZAH ESP8266

Hello breadboard és átkötés fenyegetéseknek toomake hello kapcsolat a következőképpen használhatja. Ha még nem rendelkezik érzékelő, ez a szakasz kihagyása, mert a szimulált érzékelő adatokat helyette használhatja.

![Kapcsolatok referencia](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


Érzékelő PIN-kód használja a következő vezetékezést hello:


| Indítsa el a (érzékelő)           | Záró (tábla)           | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 31F)            | 3V (PIN-kód 58H)           | Piros kábel     |
| ADATOK (PIN-kód 32F)           | GPIO 2 (PIN-kód 46A)       | Kék kábel    |
| GND (PIN-kód 34F)            | GND (PIN-kód 56I)          | Fekete kábel   |

További információkért lásd: [Adafruit DHT22 érzékelő telepítő](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) és [Adafruit lágyított HUZZAH Esp8266 érintkezőkiosztása szerepel](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Most már a lágyított Huzzah ESP8266 kell csatlakoztatni a működő érzékelő.

![Csatlakozás DHT22 lágyított Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a>Lágyított HUZZAH ESP8266 tooyour számítógép

Amint tovább, hello Micro USB tooType A USB kábel tooconnect lágyított HUZZAH ESP8266 tooyour számítógépet használni.

![Lágyított Huzzah tooyour számítógép](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Adja hozzá a soros port engedélyek (csak Ubuntu)


Ha Ubuntu használ, győződjön meg arról a hello USB port a lágyított HUZZAH ESP8266 rendelkezik hello engedélyek toooperate. tooadd soros port engedélyek, kövesse az alábbi lépéseket:


1. Futtassa a következő parancsokat a terminálon hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   A következő kimenetek hello egyik beolvasása:

   * crw-rw---1 legfelső szintű uucp xxxxxxxx
   * crw-rw---1 legfelső szintű kitárcsázáshoz xxxxxxxx

   Figyelje meg, hogy hello kimenet `uucp` vagy `dialout` hello csoport tulajdonos neve hello USB-porttal.

1. Hello felhasználói toohello csoport hozzáadása hello a következő parancs futtatásával:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`hello kapott csoport tulajdonos neve van hello előző lépésben. `<username>`Ubuntu felhasználónevének van.

1. Ubuntu kijelentkezni, és jelentkezzen be újból a hello módosítása tooappear.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Érzékelő adatokat gyűjteni, és elküldi a tooyour IoT-központ

Ebben a szakaszban telepítése, és futtassa a mintaalkalmazást lágyított HUZZAH ESP8266. hello mintaalkalmazás villogjon-hello LED lágyított HUZZAH ESP8266 a és küldi hello hőmérséklet és a páratartalom adatgyűjtés hello DHT22 érzékelő tooyour az IoT-központ.

### <a name="get-hello-sample-application-from-github"></a>Hello mintaalkalmazás beszerzése a Githubról

hello mintaalkalmazást a Githubon található. Klónozás hello minta tárház, amely tartalmazza a Githubról hello mintaalkalmazást. tooclone hello minta tárház, kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssort vagy terminálablakot.
1. Nyissa meg a tooa mappára, ahol hello minta alkalmazás toobe tárolja.
1. Futtassa a következő parancs hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Hello csomag telepítése a lágyított HUZZAH ESP8266 hello Arduino IDE:

1. Nyissa meg a mintaalkalmazás hello tároló hello mappát.
1. Nyissa meg a hello app.ino fájlt hello Arduino IDE hello alkalmazás mappájában.

   ![Nyissa meg a mintaalkalmazás hello Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Kattintson Arduino IDE hello, **fájl** > **beállítások**.
1. Hello a **beállítások** párbeszédpanelen kattintson az ikonra következő toohello hello **további modulok Manager URL-címet** mezőbe.
1. Hello előugró ablak, írja be a következő URL-cím hello, és kattintson **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![A Arduino IDE pont tooa alkalmazáscsomag URL-címe](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. A hello **preferencia** párbeszédpanel, kattintson a **OK**.
1. Kattintson a **eszközök** > **Board** > **modulok Manager**, majd keresse meg a esp8266.

   Modulok Manager, az azt jelenti, hogy telepítve van-e a ESP8266 2.2.0 vagy újabb verziójával.

   ![hello esp8266 csomag telepítve van](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Kattintson a **eszközök** > **Board** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Szükséges kódtárak telepítése

1. Hello Arduino IDE, kattintson **vázlat** > **közé tartozik könyvtár** > **szalagtárak kezelése**.
1. Keresse meg a következő könyvtár nevek egyenként hello. Az egyes tárak talált, kattintson a **telepítése**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Nincs valós DHT22 érzékelő?

hello mintaalkalmazás szimulálhatja a hőmérséklet és a páratartalom adatok, abban az esetben, ha nincs valós DHT22 érzékelő. tooset hello minta alkalmazás szimulált toouse adatokat, kövesse az alábbi lépéseket:

1. Nyissa meg hello `config.h` hello fájlban `app` mappát.
1. Keresse meg a következő kódsort hello, és módosítsa hello értéket `false` túl`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Hello mintaadatok alkalmazás szimulált toouse konfigurálása](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Mentse a fájlt hello `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a>Hello minta alkalmazás tooFeather HUZZAH ESP8266 telepítése

1. Kattintson Arduino IDE hello, **eszköz** > **Port**, majd kattintson a lágyított HUZZAH ESP8266 hello soros port.
1. Kattintson a **vázlat** > **feltöltése** toobuild és hello minta alkalmazás tooFeather HUZZAH ESP8266 telepítése.

### <a name="enter-your-credentials"></a>Adja meg hitelesítő adatait

Ha sikeresen befejeződött a feltöltés hello, kövesse ezeket a lépéseket tooenter a hitelesítő adatait:

1. Kattintson Arduino IDE hello, **eszközök** > **soros figyelő**.
1. Hello soros figyelő ablakban figyelje meg, a két hello legördülő listákból hello jobb alsó sarkában található.
1. Válassza ki **nincs sor befejezési** hello bal oldali legördülő listát.
1. Válassza ki **115200 átviteli** hello jobb legördülő listát.
1. Hello beviteli mezőbe hello soros figyelő ablak hello tetején található, írja be a következő információ, ha a rendszer felkéri tooprovide hello őket, és kattintson a **küldése**.
   * Wi-Fi SSID
   * Wi-Fi jelszó
   * Eszköz kapcsolati karakterlánc

> [!Note]
> hello hitelesítő adatai hello EEPROM a lágyított HUZZAH ESP8266. Ha hello lágyított HUZZAH ESP8266 board hello Visszaállítás gombra kattint, hello mintaalkalmazás megkérdezi tooerase hello információkat. Adja meg `Y` toohave hello adatokat törölni. Tooprovide hello információ még egyszer kell adnia.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Győződjön meg arról hello mintaalkalmazás sikeresen fut.

Ha a hello hello soros figyelő ablakból parancskimenet, és hello villogó LED a lágyított HUZZAH ESP8266 hello mintaalkalmazás sikeresen fut-e.

![Végső kimenetet a Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Következő lépések

Sikeresen csatlakozott egy lágyított HUZZAH ESP8266 tooyour IoT-központ, és rögzített hello érzékelő adatokat tooyour IoT-központ küldött. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

