---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 2: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "A Mac számítógépen telepíti, létrehoz egy IoT-központot, és az eszközt regisztrálni kell az IoT-központ hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, internet dolgot szoftvert, az azure cli, install python mac, telepítse a git mac, gulp futtatja, a telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 391d60f3cbb209698cae53098efed360ac0f5fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos"></a>Hello eszközök (macOS) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog

- Telepítse a Git, Node.js, Gulp, Python.
- Hello Azure parancssori felület (CLI) telepítése. 

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

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
- Az operációs rendszer X Yosemite (10.10) vagy újabb rendszert futtató Mac számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

tooinstall Git és a Node.js, segédprogrammal hello Homebrew csomag felügyeleti ezeket a lépéseket követve:

1. [Töltse le](http://brew.sh/) és Homebrew telepítse. Ha már telepített Homebrew, nyissa meg toostep 2.
   1. Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.
   2. Futtassa a következő parancs hello:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Telepítse a Git és Node.js hello a következő parancs futtatásával:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) tooautomate üzembe helyezési és parancsprogramok végrehajtását.

tooinstall gulp, futtassa a következő parancs hello terminálban hello:

```bash
npm install -g gulp
```

Hello telepítési problémákat tapasztal, tekintse meg a hello [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-sim-troubleshooting.md) a megoldások toocommon problémákat.

> [!Note]
> Csomópont, NPM és Gulp a node.js fejlesztett szükséges toorun automatizálási parancsfájlokat.

## <a name="install-python"></a>Python telepítése

Bár Mac OS X Python 2.7, azt javasoljuk, hogy telepítse a Python Homebrew keresztül. Lásd: [Python telepítése Mac OS x](http://docs.python-guide.org/en/latest/starting/install/osx/).

Telepítse a Python és a pip hello a következő parancs futtatásával:

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a>Hello Azure parancssori felület telepítése

tooinstall hello Azure CLI használata esetén kövesse az alábbi lépéseket:

1. Futtassa a következő parancsok hello terminálban hello:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   hello telepítési 5 percig is eltarthat.

2. Ellenőrizze a hello telepítést hello a következő parancs futtatásával:
   ```bash
   az iot -h
   ```
   Meg kell jelennie a hello parancskimenet sikeres hello telepítés esetén.

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code később használható hello oktatóanyag tooedit konfigurációs fájlok.

[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

A Mac számítógépen telepített összes hello szükséges eszközök és szoftverek. A következő feladathoz toouse hello Azure CLI toocreate egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[Az IoT-központ létrehozni és regisztrálni az eszközt](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
