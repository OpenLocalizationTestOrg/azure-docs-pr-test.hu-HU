---
title: "aaaRaspberry Pi toocloud (Node.js) - csatlakozás málna Pi tooAzure IoT-központ |} Microsoft Docs"
description: "Csatlakozás málna Pi toosend adatok toohello Azure felhőben az IoT-központ málna Pi tooAzure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot raspberry pi raspberry pi iot hub, raspberry pi küldési adatok toocloud raspberry pi toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.openlocfilehash: 07bc66983c427eab8118be18d91abb25deb03ad3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a>Csatlakozás málna Pi tooAzure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Ebben az oktatóanyagban akkor először tanulási hello használatának alapjait málna Pi Raspbian futtató. Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Windows 10 IoT minta, nyissa meg a toohello [Windows fejlesztői központ](http://www.windowsondevices.com/).

Még nem rendelkezik egy csomagot? Próbálja meg hello [málna Pi 3 emulátor](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/). Vagy egy új csomag vásárlása [Itt](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Mit

* A telepítő Raspberry Pi.
* Létrehoz egy IoT-központot.
* Eszköz regisztrálása az IoT hub a pi.
* Futtassa a mintaalkalmazást Pi toosend érzékelő adatokat tooyour IoT-központ.

Csatlakozás málna Pi tooan IoT-központ az Ön által létrehozott. Akkor futtassa a mintaalkalmazást Pi toocollect hőmérséklet és a páratartalom adatok BME280 érzékelő. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.
* Hogyan tooconnect BME280 érzékelő a pi tartományban.
* Hogyan toocollect érzékelőadatait Pi mintaalkalmazás futtatásával.
* Hogyan toosend érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

![Mi szükséges](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

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
   1. [Töltse le a Pixel Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/) (hello .zip fájl).
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

   ![hello Raspbian beállítások menü](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. A hello **felületek** lapon **I2C** és **SSH** túl**engedélyezése**, és kattintson a **OK**. Ha nem rendelkezik a fizikai érzékelők, és szeretné, hogy szimulált toouse érzékelőadatait, ez a lépés nem kötelező megadni.

   ![I2C és a Raspberry Pi SSH engedélyezése](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH és I2C található további referencia dokumentumok [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) és [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-hello-sensor-toopi"></a>Csatlakozás hello érzékelő tooPi

Hello breadboard és átkötés fenyegetéseknek tooconnect LED és egy BME280 tooPi a következőképpen használhatja. Ha nincs hello érzékelő, hagyja ki az ebben a szakaszban.

![hello málna Pi és érzékelő kapcsolat](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

hello BME280 érzékelő gyűjthet a hőmérséklet és a páratartalom adatokat. És hello LED fog villogni, ha egy eszköz és a hello közötti kommunikációt. 

Érzékelő PIN-kód használja a következő vezetékezést hello:

| Indítsa el a (érzékelő & LED)     | Záró (tábla)            | Kábel szín   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kód 5 g.)             | 3.3V PWR (1. Pin)       | A fehér kábel   |
| GND (7G PIN-kód)             | GND (PIN-kód 6)            | Barna kábel   |
| SCK (8G PIN-kód)             | I2C1 SDA (3 PIN-kód)       | Narancssárga kábel  |
| SDI (PIN-kód 10G)            | I2C1 SCL (PIN-kód 5)       | Piros kábel     |
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

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a>Klónozza a mintaalkalmazást és hello előfeltételként szükséges csomagok telepítése

1. SSH-ügyfél követően a gazdagép számítógép tooconnect tooyour málna Pi hello egyikét használhatja.
    - [A puTTY](http://www.putty.org/) Windows. A Pi tooconnect hello IP-címe van szüksége az SSH-kapcsolaton keresztül.
    - hello beépített SSH-ügyfél Ubuntu vagy macOS. Előfordulhat, hogy kell futtatnia `ssh pi@<ip address of pi>` tooconnect Pi SSH-kapcsolaton keresztül.

   > [!NOTE] 
   hello alapértelmezett felhasználónév az `pi` , és hello jelszó `raspberry`.

1. Telepítse a Node.js és NPM tooyour Pi.
   
   Először a Node.js-verzió érdemes egyeztetni a következő parancs hello. 
   
   ```bash
   node -v
   ```

   Ha hello verziószáma kisebb, mint a 4.x-es, vagy nincs nem Node.js a Pi, majd futtassa a következő parancs tooinstall hello, vagy frissítse a Node.js.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Klónozza a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. A következő parancs hello összes csomag telepítéséhez. Ez magában foglalja az Azure IoT-eszközök SDK, BME280 érzékelő és huzalozási Pi függvénytárak.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Eltarthat néhány percig toofinish a telepítési folyamat denpening a hálózati kapcsolatban.

### <a name="configure-hello-sample-application"></a>Hello mintaalkalmazás konfigurálása

1. Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Két elem ebben a fájlban configurate is. hello először egy van `interval`, amely megadja, hogy két toocloud küldött üzenetek hello időközét. második hello `simulatedData`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.

   Ha Ön **nincs hello érzékelő**, beállíthatja hello `simulatedData` érték túl`true` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.

1. Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.

### <a name="run-hello-sample-application"></a>Hello mintaalkalmazás futtatása

1. Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.


Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.

![Kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Következő lépések

Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]