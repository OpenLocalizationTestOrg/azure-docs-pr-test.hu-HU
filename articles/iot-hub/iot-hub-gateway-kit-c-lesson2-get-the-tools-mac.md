---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "A Mac számítógépen telepíti, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, iot felhőalapú szolgáltatás, internet dolgot szoftvert, az azure cli, install python mac, telepítse a git mac, gulp futtatja, a telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 94e538ef-9857-4023-9c3c-e92a0be7c395
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 07bc5f2cf6542273c334371b77520c674c5d2f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a>Szerezze be az eszközöket (MacOS)
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

- Telepítése [Git](https://git-scm.com/) és [Node.js](https://nodejs.org/en/).
  - Git egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a lecke a mintaalkalmazás Git tárolja.
  - NODE.js a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
- Használata [NPM](https://www.npmjs.com/) Node.js fejlesztői eszközök telepítése.
  - A Node.js minimálisan szükséges verziója a 4.5-ös LTS.
  - NPM egyike a Node.js csomag feletteseit.
- Hogyan kell telepíteni a Visual Studio Code.
  - A Visual Studio Code egy többplatformos, a Windows, Linux és macOS egyszerűsített, de hatékony forráskód szerkesztőjében. Nagyszerű hibakeresés, beágyazott Git-vezérlő, szintaxis kiemelését, intelligens kód befejezési, kódtöredékek, és támogatása kód újrabontása is rendelkezik.
- Hogyan kell telepíteni a Python.
  - Python széles körben használt magas szintű, általános célú, értelmezett és dinamikus programozási nyelv.
- Tudnivalók az Azure parancssori felület telepítése.
  - Az Azure parancssori felület olyan többplatformos parancssori környezetet biztosít az Azure. Közvetlenül a parancssorból kiépítését működik, és kezelheti az erőforrásokat.
- Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.

## <a name="what-you-need"></a>Mi szükséges

- Az eszközök és szoftverek letöltése az internethez.
- Az operációs rendszer X Yosemite (10.10) vagy újabb rendszert futtató Mac számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

Telepítse a Git szoftver, Node.js, a segédprogrammal Homebrew csomag felügyeleti ezeket a lépéseket követve:

1. [Töltse le](http://brew.sh/) és Homebrew telepítse. Ha már telepített Homebrew, folytassa a 2.
   1. Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.
   2. Futtassa az alábbi parancsot:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. Telepítse a Git és a Node.js, a következő parancs futtatásával:

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.

Gulp telepítéséhez futtassa a terminálban a következő parancsot:

```bash
npm install -g gulp
```

Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) gyakori problémák megoldásainak.

> [!Note]
> Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.

## <a name="install-python"></a>Python telepítése

Bár Mac OS X Python 2.7, azt javasoljuk, hogy telepítse a Python Homebrew keresztül. Lásd: [Python telepítése Mac OS x](http://docs.python-guide.org/en/latest/starting/install/osx/).

Telepítse a Python és a pip a következő parancs futtatásával:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t

Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:

1. A következő parancsokat a terminálban:
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   A telepítés 5 percig is eltarthat.

2. Ellenőrizze a telepítést a következő parancs futtatásával:
   ```bash
   az iot -h
   ```
   Ha a telepítés sikerült a következő kimenetet kell megjelennie.

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.

[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

Telepítette a szükséges eszközök és a szoftver a Mac számítógépen. A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[Az IoT-központ létrehozni és regisztrálni az eszközt](iot-hub-gateway-kit-c-lesson2-register-device.md)
