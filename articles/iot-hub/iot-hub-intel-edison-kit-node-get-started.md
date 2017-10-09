---
title: "aaaIntel Edison toocloud (Node.js) - csatlakozás Intel Edison tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és ebben az oktatóanyagban Intel Edison tooAzure IoT-központ Intel Edison toosend adatok toohello Azure cloud platform csatlakoznak."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot intel edison, intel edison iot-központot, intel edison küldése adatok toocloud, intel edison toocloud"
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfc3387efc532b4b83f0626a9cf61d12c2952af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-nodejs"></a>Csatlakozás Intel Edison tooAzure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Ebben az oktatóanyagban akkor először hello használatának alapjait Intel Edison tanulási. Majd megismerheti, hogyan tooseamlessly összekapcsolni használatával eszközök toohello felhőalapú [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

Még nem rendelkezik egy csomagot? Start [Itt](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Mit

* A telepítő Intel Edison és és Groove-modulok.
* Létrehoz egy IoT-központot.
* Eszköz regisztrálása az Edison az IoT hub a.
* Futtassa a mintaalkalmazást Edison toosend érzékelő adatokat tooyour IoT-központ.

Csatlakoztassa az Intel Edison tooan IoT-központ az Ön által létrehozott. Majd, futtassa a mintaalkalmazást Edison toocollect hőmérséklet és a páratartalom adatok Groove hőmérséklet-érzékelő. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot.
* Hogyan tooconnect Edison rendelkező Groove hőmérséklet-érzékelő.
* Hogyan toocollect érzékelőadatait Edison mintaalkalmazás futtatásával.
* Hogyan toosend érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

![Mi szükséges](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* hello Intel Edison board
* Arduino bővítése tábla
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* A Mac vagy a Windows vagy Linux rendszerű számítógép.
* Az internethez.
* Egy Micro B tooType egy USB-kábellel
* A közvetlen aktuális (DC) tápegység. A tápegység kell tekinthető meg az alábbiak szerint:
  - 7-15V TARTOMÁNYVEZÉRLŐ
  - Legalább 1500mA
  - hello center/belső PIN-kód hello pozitív sarkpontot hello energiaellátás kell lennie.

a következő elemek hello nem kötelező:

* Groove alap pajzs V2
* Groove - hőmérséklet-érzékelő
* Groove kábel
* Bármely Oszlopelválasztó sávok vagy csavart hello csomagolása, beleértve a két csavart toofasten hello modul toohello bővítése board és négy csavart és műanyag térköztartók szerepel.

> [!NOTE] 
Ezek az elemek nem kötelező, mert hello kód a minta támogatási szimulált érzékelőadatait.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>A telepítő Intel Edison

### <a name="assemble-your-board"></a>A tábla összeállítása

Jelen szakaszban található lépéseket tooattach az Intel® Edison modul tooyour bővítése tábla.

1. A bővítés üzenőfalon, hello lyuk hello modul hello csavart hello bővítése táblán a sorba állítása hello Intel® Edison modul belül fehér hello vázlat helyezze el.

2. Nyomja le a hello modul hello szavak alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. A két hello (hello csomagban található) hexadecimális nuts toosecure hello modul toohello bővítése tábla használatával.

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. Helyezze be a elment hello négy sarok lyukat hello bővítése táblán egyikében. Csavarás és szigoríthatja fehér hello műanyag térköztartók alakzatot hello csavart egyikét.

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. Ismételje meg a másik három sarok térköztartók hello.

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

Most már a üzenőfalon kész.

   ![üzenőfalon kész](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a>Hello Groove talál pajzs és hello hőmérséklet-érzékelő

1. Hely hello Groove talál pajzs tooyour táblán. Győződjön meg arról, hogy a tábla szorosan csatlakoztatott összes PIN-kód.
   
   ![Groove alap pajzs ikon](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. Használjon Groove kábel tooconnect Groove hőmérséklet-érzékelő alakzatot hello Groove talál pajzs **A0** port.

   ![Csatlakozás tootemperature érzékelő](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison és érzékelő kapcsolat](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

Most már készen áll az érzékelő.

### <a name="power-up-edison"></a>Energiagazdálkodási Edison mentése

1. Beépülő modul hello tápegység.

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. A zöld LED-jét (DS1 feliratú a hello Arduino * bővítése board) kell bonyolít le, és megvilágítottnak maradnak.

3. Várjon egy percet a hello board toofinish másolatából végrehajtott indítása.

   > [!NOTE]
   > Ha egy tartományvezérlő tápegység nem rendelkezik, akkor is power hello board USB-porton keresztül. Lásd: `Connect Edison tooyour computer` című szakaszban talál információt. A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.

### <a name="connect-edison-tooyour-computer"></a>Edison tooyour számítógép

1. Váltás le hello mikrokapcsoló felé hello két micro USB-porttal, így Edison eszköz módban van. Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Váltás a hello mikrokapcsoló](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.

   ![Felső micro USB-port](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. Plug hello USB-kábel másik végén a számítógépbe.

   ![Számítógép USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).

## <a name="download-and-run-hello-configuration-tool"></a>Hello konfigurációs eszköz letöltése és futtatása
Hello legújabb konfiguráló eszköz az beszerzése [Ez a hivatkozás](https://software.intel.com/en-us/iot/hardware/edison/downloads) alatt hello feltüntetve `Installers` fejléc. Hello eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint

### <a name="flash-firmware"></a>Belső vezérlőprogram Flash
1. A hello `Set up options` kattintson `Flash Firmware`.
2. Válassza ki a hello kép tooflash alakzatot a tábla hello következő tevékenységek végrehajtásával:
   - toodownload és a flash hello legújabb belső vezérlőprogram lemezképpel érhetők el az Intel, a tábla válasszon `Download hello latest image version xxxx`.
   - tooflash a tábla olyan képpel már mentette a számítógépen, válassza ki `Select hello local image`. Keresse meg a tooand válassza hello kép tooflash tooyour board szeretné.
3. hello telepítő eszköz tooflash kísérli meg a tábla. hello teljes villogó folyamat too10 percig is tarthat.

### <a name="set-password"></a>Jelszó beállítása
1. A hello `Set up options` kattintson `Enable Security`.
2. Az Intel® Edison board egyéni nevet állíthatja be. Ez nem kötelező.
3. Adja meg a tábla jelszavát, majd kattintson az `Set password`.
4. Üzemszünetének hello jelszót, amelyre később szolgál.

### <a name="connect-wi-fi"></a>Wi-Fi csatlakozás
1. A hello `Set up options` kattintson `Connect Wi-Fi`. Várjon, amíg fel tooone perc, a számítógép vizsgálatok elérhető Wi-Fi-hálózatok.
2. A hello `Detected Networks` legördülő listára, válassza ki a hálózaton.
3. A hello `Security` legördülő listában, jelölje be hello hálózati biztonság típusa.
4. Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.
5. Üzemszünetének hello IP-címet, amely később szolgál.

> [!NOTE]
> Győződjön meg arról, hogy Edison ugyanaz, mint a számítógép hálózati csatlakoztatott toohello. A számítógép csatlakozik a tooyour Edison hello IP-cím használatával.

   ![Csatlakozás tootemperature érzékelő](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

Gratulálunk! Sikeresen konfigurálta az Edison.

## <a name="run-a-sample-application-on-intel-edison"></a>Futtassa a mintaalkalmazást az Intel Edison

### <a name="prepare-hello-azure-iot-device-sdk"></a>Hello Azure IoT eszköz SDK előkészítése

1. A fogadó számítógép tooconnect tooyour Intel Edison az SSH-ügyfél következő hello egyikét használhatja. hello IP-cím hello konfigurációs eszköze, hello jelszó pedig egy adott eszköz a beállított hello.
    - [A puTTY](http://www.putty.org/) Windows.
    - hello beépített SSH-ügyfél Ubuntu vagy macOS.

2. Klónozás hello sample app tooyour ügyféleszközön. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. Az összes csomag toohello tárház mappa toorun hello a következő parancs tooinstall navigáljon, közegészségre veszélyesnek perc toocomplete is eltarthat.
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-hello-sample-application"></a>Konfigurálja és hello mintaalkalmazás futtatása

1. Nyissa meg a konfigurációs fájl hello hello a következő parancsok futtatásával:

   ```bash
   nano config.json
   ```

   ![A konfigurációs fájl](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   Két makrók van ebben a fájlban configurate is. hello először egy van `INTERVAL`, amely megadja, hogy két toocloud küldött üzenetek hello időközét. második hello `simulatedData`, vagyis az, hogy toouse szimulált-e érzékelőadatait vagy nem logikai értéket.

   Ha Ön **nincs hello érzékelő**, beállíthatja hello `simulatedData` érték túl`true` toomake hello mintaalkalmazás létrehozása, és használjon szimulált érzékelőadatait.

1. Mentse és zárja be a vezérlő-O billentyűkombináció lenyomásával > Enter > CTRL-X.


1. Futtassa a mintaalkalmazást hello hello a következő parancs futtatásával:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Győződjön meg arról, hogy Ön-e beillesztési hello eszköz kapcsolati karakterláncot a hello szimpla idézőjelben.

Meg kell jelennie a hello parancskimenet, hogy látható hello érzékelő adatokat és hello küldött üzenetek tooyour IoT-központot.

![Kimeneti - érzékelő adatokat küldött az Intel Edison tooyour IoT-központ](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a>Következő lépések

Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
