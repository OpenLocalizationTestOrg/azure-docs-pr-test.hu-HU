---
title: "aaaRaspberry Pi toocloud (C) - csatlakozás málna Pi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban málna Pi tooAzure IoT-központ málna Pi toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi raspberry pi iot hub, raspberry pi küldési adatok toocloud raspberry pi toocloud"
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/12/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05086890458e196d7fdc87a53fcabb9386245d6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-c"></a>Csatlakozás málna Pi tooAzure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Ebben az oktatóanyagban akkor először tanulási hello használatának alapjait málna Pi Raspbian futtató. Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Windows 10 IoT minta, nyissa meg a toohello [Windows fejlesztői központ](http://www.windowsondevices.com/).

Még nem rendelkezik egy csomagot? Próbálja [málna Pi online szimulátor](iot-hub-raspberry-pi-web-simulator-get-started.md). Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Mit

* Létrehoz egy IoT-központot.
* Eszköz regisztrálása az IoT hub a pi.
* A telepítő Raspberry Pi.
* Futtassa a mintaalkalmazást Pi toosend érzékelő adatokat tooyour IoT-központ.

Csatlakozás málna Pi tooan IoT-központ az Ön által létrehozott. Akkor futtassa a mintaalkalmazást Pi toocollect hőmérséklet és a páratartalom adatok BME280 érzékelő. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.
* Hogyan tooconnect BME280 érzékelő a pi tartományban.
* Hogyan toocollect érzékelőadatait Pi mintaalkalmazás futtatásával.
* Hogyan toosend érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

![Mi szükséges](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* hello málna Pi 2 vagy málna Pi 3.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* A figyelő egy USB-billentyűzet és egér tooPi-hez.
* A Mac vagy a Windows vagy Linux rendszerű számítógép.
* Az internethez.
* Egy 16 GB-os vagy újabb microSD-kártyán.
* Egy USB-SD adapter vagy microSD kártya tooburn hello operációsrendszer-képet hello microSD-kártyán.
* Egy 5-volt 2-amp tápegység hello 6-mértékű kiszolgálóhasználat micro USB-kábellel.

a következő elemek hello nem kötelező:

* Az összeállított Adafruit BME280 hőmérséklet, a terhelés, és a páratartalom érzékelő.
* Egy breadboard.
* 6/aljzat átkötés fenyegetéseknek.
* Egy szórt 10 mm LED-jét.


> [!NOTE] 
Ezek az elemek nem kötelező, mert hello kód a minta támogatási szimulált érzékelőadatait.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>A telepítő Raspberry Pi

### <a name="install-hello-raspbian-operating-system-for-pi"></a>A Pi hello Raspbian operációs rendszer telepítése

Készítse elő a hello microSD-kártyán hello Raspbian lemezkép telepítéséhez.

1. Töltse le a Raspbian.
   1. [Töltse le az asztallal Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip fájl).
   1. Bontsa ki a hello Raspbian kép tooa mappát a számítógépén.
1. Telepítse a Raspbian toohello microSD-kártyán.
   1. [Töltse le és telepítse a hello Etcher SD-kártya író segédprogram](https://etcher.io/).
   1. Futtassa a Etcher és hello Raspbian kép kibontott válassza az 1. lépésben.
   1. Válassza ki a hello microSD-kártyát meghajtó. Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott hello megfelelő meghajtót.
   1. Kattintson a Flash tooinstall Raspbian toohello microSD-kártyán.
   1. Ha a telepítés hello microSD-kártyán eltávolítása a számítógépről. Ennek az oka biztonságos tooremove hello microSD-kártyán közvetlenül Etcher automatikusan kiadása vagy leválasztja hello microSD-kártyán befejezését követően.
   1. A Pi hello microSD-kártyán beilleszteni.

### <a name="enable-ssh-and-spi"></a>SSH- és SPI engedélyezése

1. Csatlakozás Pi toohello monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` a hello felhasználónévvel és `raspberry` hello jelszóként.
1. Kattintson a hello Raspberry ikon > **beállítások** > **málna Pi konfigurációs**.

   ![hello Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. A hello **felületek** lapon **SPI** és **SSH** túl**engedélyezése**, és kattintson a **OK**. Ha nem rendelkezik a fizikai érzékelők, és szeretné, hogy szimulált toouse érzékelőadatait, ez a lépés nem kötelező megadni.

   ![SPI és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH és SPI található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-hello-sensor-toopi"></a>Csatlakozás hello érzékelő tooPi

Hello breadboard és átkötés fenyegetéseknek tooconnect LED és egy BME280 tooPi a következőképpen használhatja. Ha még nem rendelkezik hello érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).

![hello málna Pi és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

hello BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat. És hello LED fog villogni, ha egy eszköz és a hello közötti kommunikációt. 

Érzékelő PIN-kód használja a következő vezetékezést hello:

| Indítsa el a (érzékelő & LED)     | Záró (tábla)            | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| LED VDD (PIN-kód 5 g.)         | GPIO 4 (PIN-kód 7)         | A fehér kábel   |
| LED GND (6G PIN-kód)         | GND (PIN-kód 6)            | Fekete kábel   |
| VDD (PIN-kód 18F)            | 3.3V PWR (PIN-kód 17)      | A fehér kábel   |
| GND (PIN-kód 20F)            | GND (PIN-kód 20)           | Fekete kábel   |
| SCK (PIN-kód 21F)            | SPI0 SCLK (PIN-kód 23)     | Narancssárga kábel  |
| SDO (PIN-kód 22F)            | SPI0 MISO (21 PIN-kód)     | Sárga kábel  |
| SDI (PIN-kód 23F)            | SPI0 MOSI (PIN-kód 19)     | Zöld kábel   |
| CS (PIN-kód 24F)             | SPI0 CS (PIN-kód 24)       | Kék kábel    |

Kattintson a tooview [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.

Miután sikeresen csatlakozott BME280 tooyour málna Pi, meg kell például a kép alatt.

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a>A Pi toohello hálózat

Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység. Használjon hello Ethernet kábel tooconnect Pi tooyour vezetékes hálózati, vagy hajtsa végre a hello [hello málna Pi Foundation utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour vezeték nélküli hálózathoz. A Pi sikeresen csatlakoztatott toohello hálózati után tootake hello jegyezze fel kell [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Csatlakoztatott toowired hálózati](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a>A Pi mintaalkalmazás futtatása

### <a name="install-hello-prerequisite-packages"></a>Hello előfeltételként szükséges csomagok telepítése

1. SSH-ügyfél követően a gazdagép számítógép tooconnect tooyour málna Pi hello egyikét használhatja.
   
   **Windows-felhasználók**
   1. Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows. 
   1. Másolja a Pi hello gazdagép nevét (vagy IP-cím) szakasz hello IP-címét, és válassza ki az SSH hello kapcsolattípus.
   
   ![A puTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac és Ubuntu felhasználók**
   
   Ubuntu vagy macOS hello beépített SSH-ügyfél használja. Előfordulhat, hogy toorun `ssh pi@<ip address of pi>` tooconnect Pi SSH-kapcsolaton keresztül.
   > [!NOTE] 
   hello alapértelmezett felhasználónév az `pi` , és hello jelszó `raspberry`.

1. Telepítse a Microsoft Azure IoT eszköz SDK hello hello csomagokat C és Cmake hello a következő parancsok futtatásával:

   ```bash
   grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
   sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
   sudo apt-get update
   sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
   git clone git://git.drogon.net/wiringPi
   cd ./wiringPi
   ./build
   ```


### <a name="configure-hello-sample-application"></a>Hello mintaalkalmazás konfigurálása

1. Klónozza a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app
   ```
1. Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-raspberrypi-client-app
   nano config.h
   ```

   ![A konfigurációs fájl](media/iot-hub-raspberry-pi-kit-c-get-started/6_config-file.png)

   Két makrók van ebben a fájlban configurate is. hello először egy van `INTERVAL`, amely megadja, hogy hello időtartam (ezredmásodpercben) két toocloud küldött üzenetek között. második hello `SIMULATED_DATA`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.

   Ha Ön **nincs hello érzékelő**, beállíthatja hello `SIMULATED_DATA` érték túl`1` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.

1. Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.

### <a name="build-and-run-hello-sample-application"></a>Hozza létre és hello mintaalkalmazás futtatása

1. Build hello mintaalkalmazás hello a következő parancs futtatásával:

   ```bash
   cmake . && make
   ```
   ![Kimeneti összeállítása](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.


Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.

![Kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a>Következő lépések

Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot. a málna Pi egy parancssori felületen, a elküldte tooyour IoT hub vagy küldési üzenetek tooyour málna Pi toosee köszönőüzenetei lásd: hello [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
