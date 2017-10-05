---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Az eszközök és a szoftver telepítése a gazdagép Ubuntu rendszert futtató számítógépen, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, dolgot szoftvert, az azure CLI-t, az eszközök internetes git telepíthető ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0bac1412-385b-4255-a33f-9d44c35feb3e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 234b60e1f8eaff52ce07f54d4d12de2421cc1a52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Eszközök beszerzése (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog

- Telepítse a Git, Node.js, Gulp, Python.
- Telepítse az Azure parancssori felület (CLI). 

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).
## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan kell telepíteni a Git és Node.js.
  - Git egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a lecke a mintaalkalmazás Git tárolja.
  - NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
- Hogyan Node.js fejlesztői eszközök telepítése az NPM segítségével.
  - A Node.js minimálisan szükséges verziója a 4.5-ös LTS.
  - NPM egyike a Node.js csomag feletteseit.
- Hogyan kell telepíteni a Visual Studio Code.
  - A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében. Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.
- Az Azure parancssori felület telepítése
  - Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.
- Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

- Az eszközök és szoftverek letöltése az internethez.
- Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

Telepítse a Git szoftver, Node.js, kövesse az alábbi lépéseket:

1. Nyomja le az `Ctrl + Alt + T` egy terminált megnyitásához.
2. Futtassa az alábbi parancsot:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.

Gulp telepítéséhez futtassa a terminálban a következő parancsot:

```bash
sudo npm install -g gulp
```

Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) gyakori problémák megoldásainak.

> [!Note]
> Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t

Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:

1. A következő parancsokat a terminálban:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   A telepítés 5 percig is eltarthat.

2. Ellenőrizze a telepítést a következő parancs futtatásával:

   ```bash
   az iot -h
   ```
Ha a telepítés sikerült a következő kimenetet kell megjelennie.
![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.

[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

Telepítette a szükséges eszközök és a szoftver a számítógépen. A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-lesson2-register-device.md)
