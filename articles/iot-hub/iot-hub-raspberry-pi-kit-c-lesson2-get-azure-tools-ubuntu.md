---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 2: Azure-eszközök (Ubuntu) |} Microsoft Docs"
description: "Ubuntu Python és az Azure parancssori felület (CLI) telepíthető."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, az azure parancssori felület"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a03512f2-fabe-40c5-8505-b93eef8e5bec
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1af4848f9fe0508e362c15b36eec8a35aea9ac5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a>Azure-eszközök (Ubuntu 16.04) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 és újabb verziók](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Hello Azure parancssori felület (CLI) telepítése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan tooinstall hello Azure parancssori felület.
* Hogyan tooadd hello Azure parancssori felület egy IoT ablaktábla.

## <a name="what-you-need"></a>Mi szükséges
* Egy Ubuntu számítógép az internethez.
* Aktív Azure-előfizetés. Ha nincs fiókja, létrehozhat egy [ingyenes próbafiók](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése
hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure, amely lehetővé teszi, a parancssor tooprovision közvetlenül a toowork, és kezelheti az erőforrásokat.

tooinstall hello Azure CLI legújabb, kövesse az alábbi lépéseket:

1. Futtassa a következő parancsokat egy terminálablakot hello. Öt perc tooinstall hello Azure CLI vehet igénybe.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:

   ```bash
   az iot -h
   ```

Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.

![Kimeneti sikerességét jelző](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Összefoglalás
Hello Azure parancssori felület telepítése. A következő feladathoz toocreate a Azure IoT-központ és az eszköz identitása használt hello Azure parancssori felület.

## <a name="next-steps"></a>Következő lépések
[Az IoT hub létrehozni és regisztrálni az málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

