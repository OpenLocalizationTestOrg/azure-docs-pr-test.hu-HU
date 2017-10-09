---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: Get tools (Windows) |} Microsoft Docs"
description: "Telepítse a Python és hello Azure parancssori felület (CLI) Windows 7 és újabb verziók."
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
ms.openlocfilehash: 25b50214322137ea32861fd1131c474e2fc7ebb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Első Azure-eszközök (Windows 7 és újabb verziók)
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Telepítse a Python és hello Azure parancssori felület (CLI). Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan tooinstall Python.
* Hogyan tooinstall hello Azure parancssori felület.

## <a name="what-you-need"></a>Mi szükséges
* Egy internetkapcsolattal rendelkező Windows-számítógép.
* Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-python"></a>Python telepítése
[Telepítse a Python](https://www.python.org/downloads/) a Windows-számítógépen. Python 2.7, 3.4-es vagy 3.5-ös verzióját is telepítheti. Ez az oktatóanyag a Python 2.7 alapul. Ha már telepítette a Python, nyissa meg a következő szakaszban toohello és hello Azure parancssori felület telepítése.

Szükség tooadd hello elérési útját, amelyben python.exe és pip.exe is telepített toohello rendszer hello mappák `PATH` környezeti változó. Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése
hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül a parancssori tooprovision dolgozhassanak és kezelheti az erőforrásokat.

tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssori ablakot rendszergazdaként.
2. Futtassa a következő parancsok hello:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Megjelenik a hello parancskimenet sikeres hello telepítés esetén.

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Összefoglalás
Hello Azure parancssori felület telepítése. A Tovább gombra a feladatütemezés toocreate az Azure IoT hub- és eszközidentitások hello Azure parancssori felület használatával.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

