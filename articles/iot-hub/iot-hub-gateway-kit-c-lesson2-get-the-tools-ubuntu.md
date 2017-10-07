---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Hello eszközeinek és hello szoftver telepítése a gazdagép Ubuntu rendszert futtató számítógépen, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
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
ms.openlocfilehash: c9edca91e791ef914b1920178b66eadd12ae0281
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Hello eszközök (Ubuntu 16.04) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog

- Telepítse a Git, Node.js, Gulp, Python.
- Hello Azure parancssori felület (CLI) telepítése. 

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).
## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan tooinstall a Git szoftver, Node.js.
  - Git egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a lecke hello-mintaalkalmazás Git tárolja.
  - NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
- Hogyan toouse NPM tooinstall Node.js fejlesztői eszközök.
  - hello minimálisan szükséges verziója Node.js 4.5-ös LTS.
  - NPM hello csomag kezelők a Node.js egyike.
- Hogyan tooinstall Visual Studio Code.
  - A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében. Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.
- Hogyan tooinstall hello Azure parancssori felület
  - hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül egy parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.
- Hogyan toouse hello Azure CLI toocreate egy IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

- Az Internet kapcsolat toodownload hello eszközök és szoftverek.
- Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

tooinstall Git és a Node.js, kövesse az alábbi lépéseket:

1. Nyomja le az `Ctrl + Alt + T` tooopen egy terminált.
2. Futtassa a következő parancsok hello:

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.

tooinstall gulp, futtassa a következő parancs hello terminálban hello:

```bash
sudo npm install -g gulp
```

Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) a megoldások toocommon problémákat.

> [!Note]
> Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése

tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:

1. Futtassa a következő parancsok hello terminálban hello:

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   hello telepítési 5 percig is eltarthat.

2. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:

   ```bash
   az iot -h
   ```
Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.
![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)

### <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.

[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

A gazdaszámítógépen telepített összes hello szükséges eszközök és szoftverek. A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-lesson2-register-device.md)
