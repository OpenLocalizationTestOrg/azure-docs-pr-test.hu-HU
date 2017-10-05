---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: Get tools (Windows) |} Microsoft Docs"
description: "Az eszközök és a szoftvert telepíteni a a Windows rendszert futtató számítógépen, létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT-központ az."
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
ms.openlocfilehash: 0d8ba03df63d0b8657a9e275fc636e806c66b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a>Szerezze be az eszközöket (Windows 7 és újabb verziók)
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
- A Windows rendszerű számítógépeken.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

Kattintson az alábbi hivatkozásokra kattintva töltse le és telepítse a Git szoftver, a Windows Node.js LTS.

- [Git letöltése a Windows rendszerhez](https://git-scm.com/download/win/)
- [Node.js-es lts verzió letöltése a Windows rendszerhez](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a>Node.js fejlesztői eszközök telepítése

Használhat [gulp.js](http://gulpjs.com/) automatikus központi telepítési és a parancsprogramok végrehajtását.

Nyomja le az ENTER `Windows + R`, típus `cmd` nyomja le az ENTER `Enter` nyisson meg egy parancssori ablakot, és futtassa a következő parancsot:

```cmd
npm install -g gulp
```

Ha a telepítési problémákat tapasztal, tekintse meg a [hibaelhárítási útmutatója](iot-hub-gateway-kit-c-troubleshooting.md) gyakori problémák megoldásainak.

> [!Note]
> Csomópont, NPM és Gulp Node.js létre automatizálási parancsfájlokat futtatásához szükséges.

## <a name="install-python"></a>Python telepítése

Python 2.7, 3.4-es vagy 3.5-ös közül választhat. Ebben az oktatóanyagban a Python 2.7 használjuk. Ha már telepítette a python, folytassa a következő szakasszal.

[Python letöltése a Windows rendszerhez](https://www.python.org/downloads/)

Ahol Python.exe és pip.exe vannak telepítve a rendszer a mappák elérési útját adja hozzá kell `PATH` környezeti változó. Alapértelmezés szerint a python.exe telepítve van a `C:\Python27` és pip.exe telepítve van-e `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Telepítse az Azure CLI-t

Az Azure parancssori felület telepítéséhez kövesse az alábbi lépéseket:

1. Nyisson meg egy parancssori ablakot rendszergazdaként.

2. Az Azure parancssori felület telepítése a következő parancsok futtatásával:

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   A telepítés 5 percig is eltarthat.

3. Ellenőrizze a telepítést a következő parancs futtatásával:

   ```cmd
   az iot -h
   ```

   Ha a telepítés sikerült a következő kimenetet kell megjelennie.

   ![Az Azure parancssori felület telepítésének ellenőrzése](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

Visual Studio Code az oktatóanyag későbbi részében, konfigurációs fájlok szerkesztésére használhatja.

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése.

## <a name="summary"></a>Összefoglalás

Telepítette a szükséges eszközök és a szoftver a számítógépen. A következő feladathoz az Azure parancssori felület használatával létrehoz egy IoT-központot, és regisztrálja az eszközt az IoT hub.

## <a name="next-steps"></a>Következő lépések
[IoT Hub létrehozása és az eszköz regisztrálása](iot-hub-gateway-kit-c-lesson2-register-device.md)
