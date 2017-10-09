---
title: "Csatlakozás Arduino tooAzure IoT - lecke 2: Azure-eszközök (macOS) |} Microsoft Docs"
description: "MacOS Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9b719293-01d2-4a2d-9c49-476e67f4816d
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0ec4131e54af5475cd0b4240480c3fda497e14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a>Azure-eszközök (macOS 10.10) beolvasása

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog

Hello Azure parancssori felület (CLI) telepítése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) a Adafruit lágyított M0 Wi-Fi Arduino kártya.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan tooinstall Azure parancssori felület.
* Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.

## <a name="what-you-need"></a>Mi szükséges
* A Mac, az internetet.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-python"></a>Python telepítése
Bár macOS Python 2.7 hello kezdő verzióról, azt javasoljuk, hogy telepítse a Python Homebrew keresztül. Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Telepítse a Python és a pip hello a következő parancs futtatásával:

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése
hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül a parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.

tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:

1. Futtassa a következő parancsokat egy terminálablakot hello. Öt perc tooinstall hello Azure CLI vehet igénybe.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.

![Kimeneti sikerességét jelző][output]

## <a name="summary"></a>Összefoglalás
Hello Azure parancssori felület telepítése. A következő feladathoz toocreate az Azure IoT hub- és eszközidentitások használatával hello Azure parancssori felület.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozása és regisztrálása a Arduino tábla][create-your-iot-hub-and-register-your-arduino-board]


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_osx.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md