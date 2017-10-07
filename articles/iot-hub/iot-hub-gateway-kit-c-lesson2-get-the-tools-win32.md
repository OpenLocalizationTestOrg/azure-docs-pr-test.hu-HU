---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: Get tools (Windows) |} Microsoft Docs"
description: "Hello eszközeinek és hello szoftver telepítése a Windows rendszert futtató állomáson, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, dolgot szoftvert, az azure CLI-t, az internet telepítse a git szoftvert a windows, Futtatás gulp, csomópont js windows telepítése, telepítse a windows npm, python Windows telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 18ae6ee4-574a-4d5f-9838-ca2a78165628
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3b30b60a0115413394992061a88dde4cd442ac19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a>Első hello eszközök (Windows 7 és újabb verziók)
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

- Hogyan tooinstall [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).
  - Git egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a lecke hello-mintaalkalmazás Git tárolja.
  - NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
- Hogyan toouse [NPM](https://www.npmjs.com/) tooinstall Node.js fejlesztői eszközök.
  - hello minimálisan szükséges verziója Node.js 4.5-ös LTS.
  - NPM hello csomag kezelők a Node.js egyike.
- Hogyan tooinstall Visual Studio Code.
  - A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében. Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.
- Hogyan tooinstall Python.
  - Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.
- Hogyan tooinstall hello Azure parancssori felület.
  - hello Azure CLI olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül egy parancssor tooprovision dolgozhassanak és kezelheti az erőforrásokat.
- Hogyan toouse hello Azure CLI toocreate egy IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

- Az Internet kapcsolat toodownload hello eszközök és szoftverek.
- A Windows rendszerű számítógépeken.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

A következő hivatkozások toodownload hello kattintson, és telepítse a Git és a Node.js-es lts verzió Windows.

- [Git letöltése a Windows rendszerhez](https://git-scm.com/download/win/)
- [Node.js-es lts verzió letöltése a Windows rendszerhez](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.

Nyomja le az ENTER `Windows + R`, típus `cmd` nyomja le az ENTER `Enter` tooopen egy parancssori ablakot, és majd a futtatási hello a következő parancsot:

```cmd
npm install -g gulp
```

Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) a megoldások toocommon problémákat.

> [!Note]
> Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.

## <a name="install-python"></a>Python telepítése

Python 2.7, 3.4-es vagy 3.5-ös közül választhat. Ebben az oktatóanyagban a Python 2.7 használjuk. Ha már telepítette a python, nyissa meg a következő szakasz toohello.

[Python letöltése a Windows rendszerhez](https://www.python.org/downloads/)

Szükség tooadd hello elérési útját, amelyben Python.exe és pip.exe is telepített toohello rendszer hello mappák `PATH` környezeti változó. Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése

tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssori ablakot rendszergazdaként.

2. Hello Azure parancssori felület telepítése hello a következő parancsok futtatásával:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   hello telepítési 5 percig is eltarthat.

3. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:

   ```cmd
   az iot -h
   ```

   Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

A gazdaszámítógépen telepített összes hello szükséges eszközök és szoftverek. A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-lesson2-register-device.md)
