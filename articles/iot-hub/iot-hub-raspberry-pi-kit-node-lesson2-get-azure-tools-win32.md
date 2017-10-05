---
title: "Csatlakozás Azure IoT - lecke 2 málna Pi (csomópont): a Get tools (Windows) |} Microsoft Docs"
description: "Telepítse a Python és az Azure parancssori felület (CLI) Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: acfa13e3-6d2c-4e68-9a77-1cbc2cf3f9c1
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7b927997b738f7a80b71c06d4dff2278dc7c64e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Első Azure-eszközök (Windows 7 és újabb verziók)
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Telepítse a Python és az Azure parancssori felület (CLI). Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan kell telepíteni a Python.
* Tudnivalók az Azure parancssori felület telepítése.

## <a name="what-you-need"></a>Mi szükséges
* Egy internetkapcsolattal rendelkező Windows-számítógép.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-python"></a>Python telepítése
[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen. Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti. Ez az oktatóanyag a Python 2.7 alapul. Ha már telepítette a Python, folytassa a következő szakasszal, és az Azure parancssori felület telepítése.

Ahol python.exe és pip.exe vannak telepítve a rendszer a mappák elérési útját adja hozzá kell `PATH` környezeti változó. Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t
Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure. Dolgozunk közvetlenül parancsot a parancssorból kiépíthetőségének és kezelhetőségének erőforrásokat.

Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssori ablakot rendszergazdaként.
2. Futtassa az alábbi parancsot:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Ellenőrizze a telepítést a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Ha a telepítés sikeres megjelenik a következő kimenetet.

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Összefoglalás
Az Azure parancssori felület telepítése. A következő feladathoz az Azure IoT hub- és eszközidentitások létrehozása az Azure parancssori felület használatával.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

