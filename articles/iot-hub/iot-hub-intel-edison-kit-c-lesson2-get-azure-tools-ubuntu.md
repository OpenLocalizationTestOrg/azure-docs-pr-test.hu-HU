---
title: "Az Azure IoT - lecke 2 Connect Intel Edison (C): Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure cli, iot felhőalapú szolgáltatás, arduino felhő"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 2463cb8e-5758-4d72-af98-62520d41f2f7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 897ab63af85a1f830ed49084ce7c3e74c84a4cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a>Azure-eszközök (Ubuntu 16.04) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog
Telepítse az Azure parancssori felület (CLI). Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Tudnivalók az Azure parancssori felület telepítése.
* Hogyan lehet az Azure parancssori felület egy IoT részcsoport hozzáadni.

## <a name="what-you-need"></a>Mi szükséges
* Egy Ubuntu számítógép az internethez.
* Aktív Azure-előfizetés. Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t
Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure-ra, így közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.

Az Azure CLI legújabb telepítéséhez kövesse az alábbi lépéseket:

1. Futtassa a következő parancsokat egy terminálablakot. Az Azure parancssori felület telepítése öt percig is eltarthat.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Ellenőrizze a telepítést a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Ha a telepítés sikerült a következő kimenetet kell megjelennie.

![Kimeneti sikerességét jelző](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Összefoglalás
Az Azure parancssori felület telepítése. A következő feladathoz, ha az Azure IoT hub- és eszközidentitások az Azure parancssori felület használatával.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az Intel Edison][create-your-iot-hub-and-register-intel-edison]


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
