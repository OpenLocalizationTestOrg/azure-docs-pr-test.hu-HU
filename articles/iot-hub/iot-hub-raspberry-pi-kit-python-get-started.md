---
title: "aaaRaspberry Pi toocloud (Python) - csatlakozás málna Pi tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban málna Pi tooAzure IoT-központ málna Pi toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot raspberry pi raspberry pi iot hub, raspberry pi küldési adatok toocloud raspberry pi toocloud"
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 86f5c91ab9dd4e23c563437827fb7d2d06916d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-python"></a>Csatlakozás málna Pi tooAzure IoT Hub (Python)

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

## <a name="set-up-raspberry-pi"></a>Málna Pi beállítása

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

### <a name="enable-ssh-and-i2c"></a>SSH- és I2C engedélyezése

1. Csatlakozás Pi toohello monitor, billentyűzet és egér. Indítsa el a Pi, majd jelentkezzen be Raspbian `pi` a hello felhasználónévvel és `raspberry` hello jelszóként.
1. Kattintson a hello Raspberry ikon > **beállítások** > **málna Pi konfigurációs**.

   ![hello Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. A hello **felületek** lapon **I2C** és **SSH** túl**engedélyezése**, és kattintson a **OK**. Ha nem rendelkezik a fizikai érzékelők, és szeretné, hogy szimulált toouse érzékelőadatait, ez a lépés nem kötelező megadni.

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH és I2C található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-hello-sensor-toopi"></a>Csatlakozás hello érzékelő tooPi

Hello breadboard és átkötés fenyegetéseknek tooconnect LED és egy BME280 tooPi a következőképpen használhatja. Ha még nem rendelkezik hello érzékelő [hagyja ki ezt a szakaszt](#connect-pi-to-the-network).

![hello málna Pi és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

hello BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat. És hello LED fog villogni, ha egy eszköz és a hello közötti kommunikációt. 

Érzékelő PIN-kód használja a következő vezetékezést hello:

| Indítsa el a (érzékelő & LED)     | Záró (tábla)            | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 5 g.)             | 3.3V PWR (1. Pin)       | A fehér kábel   |
| GND (7G PIN-kód)             | GND (PIN-kód 6)            | Barna kábel   |
| SDI (PIN-kód 10G)            | I2C1 SDA (3 PIN-kód)       | Piros kábel     |
| SCK (8G PIN-kód)             | I2C1 SCL (PIN-kód 5)       | Narancssárga kábel  |
| LED VDD (PIN-kód 18F)        | GPIO 24 (PIN-kód 18)       | A fehér kábel   |
| LED GND (PIN-kód 17F)        | GND (PIN-kód 20)           | Fekete kábel   |

Kattintson a tooview [málna Pi 2. és 3 PIN-kód hozzárendelések](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) hivatkozhat.

Miután sikeresen csatlakozott BME280 tooyour málna Pi, meg kell például a kép alatt.

![Csatlakoztatott Pi és BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a>A Pi toohello hálózat

Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység. Használjon hello Ethernet kábel tooconnect Pi tooyour vezetékes hálózati, vagy hajtsa végre a hello [hello málna Pi Foundation utasításainak](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour vezeték nélküli hálózathoz. A Pi sikeresen csatlakoztatott toohello hálózati után tootake hello jegyezze fel kell [IP-címet a pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Csatlakoztatott toowired hálózati](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello. Például, ha a számítógép vezeték nélküli hálózathoz csatlakoztatott tooa Pi pedig vezetékes hálózathoz csatlakoztatott tooa, akkor előfordulhat, hogy nem lásd: hello IP-cím hello devdisco kimenet.

## <a name="run-a-sample-application-on-pi"></a>A Pi mintaalkalmazás futtatása

### <a name="install-hello-prerequisite-packages"></a>Hello előfeltételként szükséges csomagok telepítése

SSH-ügyfél követően a gazdagép számítógép tooconnect tooyour málna Pi hello egyikét használhatja.
   
   **Windows-felhasználók**
   1. Töltse le és telepítse [PuTTY](http://www.putty.org/) Windows. 
   1. Másolja a Pi hello gazdagép nevét (vagy IP-cím) szakasz hello IP-címét, és válassza ki az SSH hello kapcsolattípus.
   
   
   **Mac és Ubuntu felhasználók**
   
   Ubuntu vagy macOS hello beépített SSH-ügyfél használja. Előfordulhat, hogy toorun `ssh pi@<ip address of pi>` tooconnect Pi SSH-kapcsolaton keresztül.
   > [!NOTE] 
   hello alapértelmezett felhasználónév az `pi` , és hello jelszó `raspberry`.


### <a name="configure-hello-sample-application"></a>Hello mintaalkalmazás konfigurálása

1. Klónozza a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   5 makrók van ebben a fájlban configurate is. hello először egy van `MESSAGE_TIMESPAN`, amely megadja, hogy hello időtartam (ezredmásodpercben) két toocloud küldött üzenetek között. második hello `SIMULATED_DATA`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket. `I2C_ADDRESS`hello I2C cím a BME280 érzékelő csatlakoztatva van. `GPIO_PIN_ADDRESS`hello GPIO cím szolgál a LED-jét. hello utolsó még `BLINK_TIMESPAN`, amely hello timespan megadva, amikor a LED ezredmásodpercben be van kapcsolva.

   Ha Ön **nincs hello érzékelő**, beállíthatja hello `SIMULATED_DATA` érték túl`True` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.

1. Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.

### <a name="build-and-run-hello-sample-application"></a>Hozza létre és hello mintaalkalmazás futtatása

1. Build hello mintaalkalmazás hello a következő parancs futtatásával. Mivel hello Azure IoT készült SDK-k Python burkolók hello Azure IoT eszköz C SDK fölött, ha azt szeretné, vagy toogenerate hello Python-könyvtárakat a forráskód kell kell toocompile hello C-függvénytárak.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   Azt is megadhatja, futtassa a kívánt hello verzió `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Ha paraméter nélkül parancsfájlt futtatja, hello parancsfájl automatikusan észleli, python telepített verziójának hello (keresési feladatütemezési 2.7 -> 3.4 -> 3.5). Győződjön meg arról, hogy a Python verzió tartja konzisztens során kialakításához és futtatásához. 
   
   > [!NOTE] 
   Épület hello Python ügyféloldali kódtár (iothub_client.so) kisebb, mint 1GB RAM-MAL rendelkező Linux rendszerű eszközökön, láthatja a build a lent látható módon iothub_client_python.cpp felépítésekor első Beragadt 98 % `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Ha ezt a problémát tapasztal, ellenőrizze a hello memóriahasználatának hello eszköz használatával `free -m command` egy másik terminál ablakban ebben az időszakban. Ha kevés a memória iothub_client_python.cpp fájl fordítása közben fut, előfordulhat, hogy növelje a lapozófájl-terület tooget hello tootemporarily több elérhető memória toosuccessfully hello Python ügyféloldali eszköz SDK-könyvtár létrehozása.
   
1. Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben. Ha hello python 3, akkor is használhatja és hello parancs `python3 app.py '<your Azure IoT hub device connection string>'`.


   Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.

   ![Kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Következő lépések

Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot. a málna Pi egy parancssori felületen, a elküldte tooyour IoT hub vagy küldési üzenetek tooyour málna Pi toosee köszönőüzenetei lásd: hello [kezelése felhő eszközt az IOT hubbal-explorer oktatóanyag üzenetküldési](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
