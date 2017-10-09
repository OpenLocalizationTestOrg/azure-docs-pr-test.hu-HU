---
title: "aaaIoT DevKit toocloud - csatlakozás IoT DevKit AZ3166 tooAzure IoT-központ |} Microsoft Docs"
description: "Megtudhatja, hogyan toosetup, és csatlakozzon az IoT DevKit AZ3166 tooAzure IoT-központ az Azure-felhőbe toohello toosend adatplatform ebben az oktatóanyagban."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: fd36abaed1fd9f73028b84312bca9e900fb9c004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a>Csatlakozás IoT DevKit AZ3166 tooAzure hello felhőben az IoT-központ

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lehet használt toodevelop és prototípus kihasználva a Microsoft Azure-szolgáltatások az eszközök internetes hálózatát (IoT) megoldásokat. Ez magában foglalja egy Arduino kompatibilis board gazdag perifériák és érzékelők, egy nyílt forráskódú board csomagot, és egy növekvő [projektek katalógus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Mit
Csatlakozás [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub létrehozása, az érzékelők hőmérséklet és a páratartalom adatok gyűjtése hello, és hello adatok tooIoT hub küldése.

Még nem rendelkezik egy DevKit? Egy új beolvasása [Itt](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan tooconnect IoT DevKit tooWireless hozzáférési pont, és a fejlesztőkörnyezet előkészítése.
* Hogyan toocreate az IoT-központ és az eszköz regisztrálása az MXChip IoT DevKit.
* Hogyan toocollect érzékelőadatait MXChip IoT DevKit mintaalkalmazás futtatásával.
* Hogyan toosend hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

* Egy MXChip IoT DevKit kártya micro USB-kábellel. [Most töltse le innen](https://aka.ms/iot-devkit-purchase)
* A Windows 10 vagy macOS 10.10 + rendszert futtató számítógépen
* Aktív Azure-előfizetés
  * Aktiválja a [ingyenes 30 napos próbafiókot Microsoft Azure](https://azureinfo.microsoft.com/us-freetrial.html)

## <a name="prepare-your-hardware"></a>Készítse elő a hardvert

Hello hardver tooyour számítógép bekötése.

### <a name="hardware-you-need"></a>Szükséges hardver

* DevKit tábla
* Micro USB-kábellel

![első-lépések – hardver](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a>DevKit tooyour számítógép

1. Csatlakoztassa az USB end tooyour PC
2. Csatlakozás Micro USB end toohello DevKit
3. zöld hello LED következő toopower megerősíti, hogy kapcsolat

![első lépések-csatlakozású](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Wi-Fi konfigurálása

IoT-projektek internetkapcsolat támaszkodnak. A következő utasításokat tooconfigure hello DevKit tooconnect tooWiFi hello használata.

### <a name="enter-ap-mode"></a>Ázsia és a Csendes üzemmód megadása

Tartsa lenyomva a B gomb, majd állítsa vissza a leküldéses és kiadás hello a gombra, majd a kiadás gomb a b kiszolgálóra. A DevKit konfigurálásához a Wi-Fi hozzáférési mód adja meg. üdvözlő képernyőt hello szolgáltatás, állítsa be a hello DevKit Identifier(SSID), valamint a hello konfigurációs portál IP-cím jelenik meg:

![első-elindult-wifi-hozzáférési pont](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a>Csatlakozás tooDevKit hozzáférési pont

Most már használja egy másik Wi-Fi engedélyezett eszköz (PC vagy telefonon keresztül) tooconnect toohello DevKit SSID (hello fenti képernyőfelvételen kiemelt), hello jelszót hagyja üresen.

![első-elindult-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Wi-Fi DevKit konfigurálása

Megnyitás hello IP-címet a hello DevKit képernyőn a számítógépen vagy a mobiltelefon böngésző, válassza ki azt szeretné, hogy hello DevKit tooconnect hello Wi-Fi hálózatot, majd hello jelszót írja be. Kattintson a **Connect** toocomplete:

![első-elindult-Wi-Fi-portál](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Miután hello csatlakozás sikeres, hello DevKit néhány másodperc múlva újraindul. Ha sikeres volt, az üdvözlő képernyőt hello Wi-Fi nevét és IP-címet fog látni:

![első lépések – Wi-Fi-ip-](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
hello IP-cím hello fénykép megjelenő előfordulhat, hogy nem egyezik a tényleges IP hello rendelve, és a hello DevKit képernyőn jelenik meg. Ez nem rendellenes, DHCP toodynamically IP-címek hozzárendelése Wi-Fi használ.

Wi-Fi beállítások konfigurálása után a hitelesítő adatok maradnak az adott kapcsolathoz hello eszköz akkor is, ha a választható le. Például, ha az otthoni Wi-Fi hello DevKit konfigurálva, és ezután tartott hello DevKit toohello office, akkor tooreconfigure Ázsia és a Csendes módban (lépéstől kezdve **Ázsia és a Csendes módot adja meg**) tooconnect hello DevKit tooyour office Wi-Fi. 

## <a name="start-using-devkit"></a>Ismerkedés a DevKit

hello alapértelmezett app DevKit futó ellenőrizze hello hello belső vezérlőprogramjának legújabb verzióját, és a meg néhány érzékelő diagnosztikai adatok megjelenítéséhez.

### <a name="upgrade-toohello-latest-firmware"></a>Toohello legújabb belső vezérlőprogram frissítése

Kérni fogja az üdvözlő képernyőt mindkét hello aktuális és a legújabb belső vezérlőprogram-verziója, ha egy frissítés szükséges. Hajtsa végre a [belső vezérlőprogram frissítése](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) tooupgrade útmutató azt.

![első-elindult-belső vezérlőprogram](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Ez egy egyszeri annak érdekében, a hello DevKit fejlesztése elindítását követően, és töltse fel az alkalmazás, hogy hello legújabb belső vezérlőprogrammal rendelkeznek az alkalmazás.

### <a name="test-various-sensors"></a>Különböző érzékelők tesztelése

Nyomja le az ENTER gombot B tootest érzékelők, akkor folytassa a lenyomásával, és tehessen közzé hello B gomb toocycle keresztül minden érzékelő.

![első-elindult-érzékelő](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Fejlesztői környezet előkészítése

Most azt is, hogy idő tooset hello fejlesztési környezet létrehozása: eszközök és toobuild, IoT-alkalmazásokhoz kábítás csomagjai. Lehetősége van a Windows vagy macOS verzió tooyour operációs rendszer szerint.


### <a name="windows"></a>Windows

Javasoljuk, hogy toouse hello telepítési csomag tooprepare hello fejlesztési környezet. Ha problémába ütközik, kövesse hello [manuális](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget azt meg.

#### <a name="download-latest-package"></a>Töltse le a legfrissebb csomagot

Hello `.zip` letöltött fájl tartalmazza az összes szükséges eszközök és DevKit fejlesztéshez szükséges csomagok.

> [!div class="button"]
[Letöltés](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> Hello `.zip` fájl tartalmazza-e hello alábbi eszközök és a csomagok. Ha már rendelkezik néhány összetevőt telepíteni, hello parancsfájl azonosítja, és hagyja ki őket.
> * NODE.js és a Yarn: futásidejű hello telepítési parancsfájlt és automatizált feladatok
> * [Az Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -platformfüggetlen parancssori élmény kezeléséhez az Azure-erőforrások, hello MSI függő Python és a pip tartalmazza.
> * [A Visual Studio Code](https://code.visualstudio.com/): DevKit fejlesztési egyszerűsített kód szerkesztése
> * [A Visual Studio Code kiterjesztése Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): lehetővé teszi, hogy Arduino fejlesztési Visual STUDIO Code
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): hello kiterjesztése Arduino támaszkodik ezt az eszközt
> * DevKit Bizottság csomag: Eszköz láncok, könyvtárak és hello DevKit projektek
> * St. csatolású segédprogram: Alapvető segédprogramok és illesztőprogramok

#### <a name="run-installation-script"></a>Telepítési parancsfájl futtatása

A Windows Fájlkezelőben keresse meg a hello `.zip` és kiszolgálóprojektjét, keresse meg `install.cmd`, kattintson a jobb gombbal, és válassza ki **"Futtatás rendszergazdaként"** toostart.

![első lépések-Futtatás-rendszergazda](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

A telepítés során jelenik meg minden egyes eszköz vagy a csomag hello előrehaladását.

![első lépések – telepítés](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a>Erősítse meg a tooinstall illesztőprogramok

Visual STUDIO Code hello Arduino bővítmény hello Arduino IDE támaszkodik. Ha hello első alkalommal hello Arduino IDE telepíti, rákérdezéses tooinstall megfelelő illesztőprogramok fogja:

![első lépések-illesztőprogram](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Attól függően, hogy az internetkapcsolat sebessége körülbelül 10 percig toofinish telepítési kell vennie. Hello telepítés befejeződése után megtekintheti a Visual Studio Code és Arduino IDE parancsikonok az asztalon.

> [!NOTE] 
Alkalmanként Visual STUDIO Code elindításakor kérni fogja, amely nem található Arduino IDE vagy a kapcsolódó tábla csomag hiba történt. azt, zárja be és a kód Arduino IDE indítsa el egyszer toosolve és a Visual STUDIO Code kell megkeresheti Arduino IDE elérési utat helyesen.


### <a name="macos-preview"></a>macOS (előzetes verzió)

Kövesse ezeket a lépéseket tooprepare fejlesztőkörnyezet macOS.

#### <a name="install-azure-cli-20"></a>Az Azure CLI 2.0 telepítése

Hajtsa végre a hello [hivatalos útmutató](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:

Azure CLI 2.0 telepítése egy `curl` parancs:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

Majd indítsa újra a parancs-rendszerhéj módosítások tootake hatás:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Arduino IDE telepítése

a Visual Studio Code Arduino bővítmény hello hello Arduino IDE támaszkodik. Töltse le és telepítse a hello [Arduino IDE tartozó macOS](https://www.arduino.cc/en/Main/Software).

#### <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Töltse le és telepítse [macOS a Visual Studio Code](https://code.visualstudio.com/). Ez lesz hello elsődleges fejlesztőeszköz DevKit IoT-alkalmazások készítéséhez.

####  <a name="download-latest-package"></a>Töltse le a legfrissebb csomagot

1. Telepítse a Node.js. Használhatja a legnépszerűbb macOS Csomagkezelő [Homebrew](https://brew.sh/) vagy [előzetesen elkészített telepítő](https://nodejs.org/en/download/) tooinstall azt.

2. Töltse le `.zip` DevKit fejlesztési Visual STUDIO Code szükséges feladat parancsfájlokat tartalmazó fájl.

   > [!div class="button"]
   [Letöltés](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Keresse meg a hello `.zip` és bontsa ki. Majd indítsa el **Terminálszolgáltatások** alkalmazást, és futtassa a következő parancsok tooconfigure hello:

   Kibontott mappát tooyour macOS felhasználói mappa áthelyezése:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Telepítés `npm` csomagok:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Arduino a Visual STUDIO Code-kiterjesztés telepítése

A Visual Studio Code lehetővé teszi tooinstall piactér bővítmények közvetlenül a hello eszköz, hello bővítmények ikon hello menü bal oldali ablaktáblán kattintson és kereshet `Arduino` tooinstall:

![telepítés-bővítmények](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>DevKit Bizottság csomag telepítése

Szüksége lesz tooadd hello DevKit board a Visual Studio Code hello Board Manager használatával.

1. Használja `Cmd+Shift+P` tooinvoke parancs paletta és típus **Arduino** majd keresse meg és jelölje **Arduino: Board Manager**.

2. Kattintson a **"További URL-címet"** jobb hello alján.
   ![telepítés-további-URL-címek](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. A hello `settings.json` fájlt, adja hozzá a sort hello aljához `USER SETTINGS` ablaktáblán, és mentse.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![telepítés-beállítások-json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Most a hello Board Manager keresse meg a "az3166", és telepítse hello legújabb verzióját.
   ![telepítés-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Most már rendelkezik az összes hello szükséges eszközöket és a macOS telepített csomagok.


## <a name="open-project-folder"></a>Projekt megnyitása mappa

### <a name="launch-vs-code"></a>Indítsa el a VS Code szerkesztőt.

Győződjön meg arról, hogy a DevKit nincs csatlakoztatva. Indítsa el a Visual STUDIO Code először, és csatlakoztassa hello DevKit tooyour számítógépet. Visual STUDIO Code automatikusan megtalálja, és egy bemutató lapja felugró:

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
Alkalmanként Visual STUDIO Code elindításakor kérni fogja hibával, ami Arduino IDE vagy a kapcsolódó tábla csomag nem található. azt, zárja be és a kód Arduino IDE indítsa el ismét toosolve és a Visual STUDIO Code kell megkeresheti Arduino IDE elérési utat helyesen.

### <a name="open-arduino-examples-folder"></a>Arduino példák mappa megnyitása

Váltás túl**"Arduino példák"** lapján keresse meg a túl`Examples for MXCHIP AZ3166 > AzureIoT` , majd kattintson a `GetStarted`.

![Mini solution példák](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Ha véletlenül tooclose hello ablaktáblában tooreload használja azt `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke parancs paletta és típus **Arduino** toofind válassza ki **Arduino: Példák**.

## <a name="provision-azure-services"></a>Azure-szolgáltatások kiépítése

Hello megoldás ablakban, a feladat futtatása `Ctrl+P` (macOS: `Cmd+P`) írja be a "felhő-provision feladat":

Visual STUDIO Code terminál, egy interaktív parancssori végigvezeti Önt a kiépítési hello hello igényelt az Azure-szolgáltatások:

![Mini-solution-felhő-kiépítés](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Build és Arduino vázlat feltöltése

### <a name="install-required-library"></a>Telepítse a szükséges kódtári

1. Nyomja le az `F1` vagy `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke parancs paletta és típus **Arduino** majd keresse meg és jelölje **Arduino: könyvtárkezelő**.

2. Keresse meg `ArduinoJson` könyvtárban, és kattintson **telepítése**

### <a name="build-and-upload-hello-device-code"></a>Build és hello kód feltöltése

Használjon `Ctrl+P` (macOS: `Cmd+P`) toorun "feladat eszköz – feltöltés". a Terminálszolgáltatások hello arra fogja kérni, tooenter konfigurációs módja. toodo Igen, tartsa lenyomva a gombot A, majd leküldéses és kiadás hello alaphelyzetbe állítása gomb. "Konfiguráció" üdvözlő képernyőt jeleníti meg. Ez a tooset hello kapcsolati karakterláncot, amely lekéri a "feladat felhő-provision" lépéssel.

Majd ellenőrzése, majd ismét feltölteni a hello Arduino vázlat fog elindulni:

![Mini-solution-eszköz-feltöltése](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

hello DevKit újraindul, és a elinduló hello kódot.

## <a name="test-hello-project"></a>Teszt hello projekt

VS-kódban kattintson a hello power plug ikonjára hello állapotsorban tooopen hello soros figyelő.

Amikor megjelenik a következő eredmények hello hello mintaalkalmazás sikeresen fut:

* hello soros képernyők hello alatt hello képernyőfelvétel a hello tartalomként ugyanazokat az információkat.
* a MXChip IoT DevKit LED hello villogó van.

![Végső kimenetet a Visual STUDIO Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problémák és visszajelzés

Található [– gyakori kérdések](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Ha problémákat tapasztal, vagy érheti el az alábbi hello csatornák toous.

## <a name="next-steps"></a>Következő lépések

Sikeresen csatlakozott egy MXChip IoT DevKit tooyour IoT-központot, és elküldött hello rögzített érzékelő adatokat tooyour IoT-központot.

első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:

- [Eszközök felhőalapú üzenetkezelése az iothub-explorerrel](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Az IoT-központ üzenetek tooAzure adattárolás mentése](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Használja a Power BI toovisualize valós idejű érzékelőadatok Azure IoT hubról](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Használja az Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Időjárás: hello érzékelő adatokat az IoT hub használata az Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Eszközkezelés az iothub-explorerrel](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Távoli figyelés és értesítések a Logic Apps használatával](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)