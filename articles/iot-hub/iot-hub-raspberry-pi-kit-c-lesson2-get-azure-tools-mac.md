---
title: "Az Azure IoT - lecke 2 Connect Raspberry pi (C): Azure-eszközök (macOS) |} Microsoft Docs"
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
ms.openlocfilehash: 3810990f4a27270fa45709f4d9dbb36a8f4369a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a>Azure-eszközök (macOS 10.10) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Telepítse az Azure parancssori felület (CLI). Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Tudnivalók az Azure parancssori felület telepítése.
* Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.

## <a name="what-you-need"></a>Mi szükséges
* A Mac, az internetet.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-python"></a>Python telepítése
Bár macOS Python 2.7 kívül a mezőbe, azt javasoljuk, hogy telepítse a Python Homebrew keresztül. Lásd: [Python telepítése a macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Telepítse a Python és a pip a következő parancs futtatásával:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t
Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat. 

Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:

1. Futtassa a következő parancsokat egy terminálablakot. Az Azure parancssori felület telepítése öt percig is eltarthat.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Ellenőrizze a telepítést a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Ha a telepítés sikerült a következő kimenetet kell megjelennie.

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Összefoglalás
Az Azure parancssori felület telepítése. A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

