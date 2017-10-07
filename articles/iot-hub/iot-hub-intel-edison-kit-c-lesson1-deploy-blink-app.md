---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 1: alkalmazás központi telepítése |} Microsoft Docs"
description: "Klónozza a C mintaalkalmazás hello a Githubból, és futtassa gulp toodeploy az alkalmazás tooyour Intel Edison board. A mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino vezetett projektek, arduino vezetett villogási, vezetett arduino villogási arduino villogási program, a arduino villogási – példa"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: b02dfd3f-28fd-4b52-8775-eb0eaf74d707
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: fa84fae812dd742a2ad4997a5e213c8e40e6fcf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-hello-blink-application"></a>Hello villogási alkalmazás létrehozását és telepítését
## <a name="what-you-will-do"></a>Mit fog
Klónozza a C mintaalkalmazás hello a Githubból, és hello gulp eszköz toodeploy hello minta alkalmazás tooIntel Edison használja. hello mintaalkalmazás villogjon hello csatlakoztatott LED toohello board két másodpercenként. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
* Hogyan toodeploy és futtatási hello mintaalkalmazást a Edison.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta a következő műveletek hello:

* [Állítsa be az eszközt][configure-your-device]
* [Hello eszközök beszerzése][get-the-tools]

## <a name="open-hello-sample-application"></a>Nyissa meg hello mintaalkalmazás
tooopen hello mintaalkalmazást, kövesse az alábbi lépéseket:

1. A Githubból hello minta tárház klónozása hello a következő parancs futtatásával:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-edison-getting-started.git
   ```
2. Nyissa meg a Visual Studio Code hello mintaalkalmazás hello a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-edison-getting-started
   cd Lesson1
   code .
   ```

   ![Tárház szerkezete][repo-structure]

hello hello fájlban `app` almappa is hello kód toocontrol hello LED tartalmazó hello kulcs forrásfájl.

### <a name="install-application-dependencies"></a>Telepítse az alkalmazásfüggőségek
Hello szalagtárak és más modulok hello a következő parancs futtatásával kell hello mintaalkalmazás telepítése:

```bash
npm install
```

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:

1. Hello eszköz konfigurációs fájl létrehozása hello a következő parancs futtatásával:

   ```bash
   gulp init
   ```

   hello konfigurációs fájl `config-edison.json` hello segítségével toolog tooEdison felhasználói hitelesítő adatokat tartalmaz. tooavoid hello szivárgásával járnak a felhasználói hitelesítő adatok, hello konfigurációs fájl jön létre hello almappájában `.iot-hub-getting-started` hello otthoni mappa a számítógépen.

2. Nyissa meg a Visual Studio Code hello eszköz konfigurációs fájl hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

3. Cserélje le a hello helyőrző `[device hostname or IP address]` és `[device password]` hello IP-címét és jelszavát, amely korábbi lecke jelölésű.

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson1/vscode-config-mac.png)

Gratulálunk! Sikeresen létrehozta a hello első mintaalkalmazás Edison.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
### <a name="install-hello-azure-iot-hub-sdk-on-edison"></a>Edison hello Azure IoT Hub SDK telepítése
Hello Azure IoT Hub SDK telepítése Edison hello a következő parancs futtatásával:

```bash
gulp install-tools
```

Ez a feladat egy hosszú ideig toocomplete, attól függően, hogy a hálózati kapcsolatot is igénybe vehet. Az igények toobe csak egyszeri futtatás egy Edison.

### <a name="deploy-and-run-hello-sample-app"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Telepíthet, és futtassa a hello mintaalkalmazást hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

### <a name="verify-hello-app-works"></a>Ellenőrizze a hello az alkalmazás akkor működik
hello mintaalkalmazás automatikusan leáll, miután hello LED villogjon-20 alkalommal a. Ha nem lát hello LED villogó, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.

![LED villogó][led-blinking]

## <a name="summary"></a>Összefoglalás
A Edison hello szükséges eszközök toowork telepítése és telepített egy minta alkalmazás tooEdison tooblink hello LED-jét. Most hozzon létre, telepítheti, és futtassa egy másik olyan mintaalkalmazást, amely összeköti az IoT-központ toosend Edison tooAzure és üzeneteket fogadni.

## <a name="next-steps"></a>Következő lépések
[Első hello Azure-eszközök][get-the-azure-tools]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[Configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson1/repo_structure_c.png
[led-blinking]: media/iot-hub-intel-edison-lessons/lesson1/led_blinking_c.jpg
[get-the-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
