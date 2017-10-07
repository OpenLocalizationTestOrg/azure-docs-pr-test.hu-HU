---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 2: Azure-eszközök (macOS) |} Microsoft Docs"
description: "MacOS Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f2d7d584-7734-401c-976c-81788a7282a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a079250fd94fa9bc1c11b6c21de02a8d46f6f3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a>Azure-eszközök (macOS 10.10) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Hello Azure parancssori felület (CLI) telepítése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

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

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Összefoglalás
Hello Azure parancssori felület telepítése. A következő feladathoz toocreate az Azure IoT hub- és eszközidentitások használatával hello Azure parancssori felület.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

