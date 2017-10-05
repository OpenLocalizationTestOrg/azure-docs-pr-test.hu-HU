---
title: 'Az Azure IoT - lecke 1 Connect Raspberry pi (C): Get tools (Windows) |} Microsoft Docs'
description: "Töltse le és telepítse a szükséges eszközök és szoftverek pi első mintaalkalmazás Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git szoftvert a windows, csomópont js windows telepítése, npm telepítése windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: bd765ddd-65b7-4241-a391-dc77cb3af1c0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0e58975f4411f97223b2c4374bdd746fe6628c42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a>Szerezze be az eszközöket (Windows 7 vagy újabb)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Töltse le a fejlesztői eszközök és a szoftver málna Pi 3 első minta alkalmazásához. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> Bár a programozási nyelv, a fő logikájának C, Node.js eszközök használata a megszerzett az eszközök, és felépítéséhez és minta alkalmazások központi telepítése.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan kell telepíteni a Git és Node.js.
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk a mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.
  * A Node.js minimális verziójára vonatkozó követelményt a 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.

## <a name="what-you-need"></a>Mi szükséges

A művelet elvégzéséhez szüksége lesz:

* A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.
* Windows rendszerű számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

Töltse le és telepítse a Git szoftver, a Windows Node.js LTS az alábbi hivatkozásokra kattintva.

* [Git letöltése a Windows rendszerhez](https://git-scm.com/download/win/)
* [Node.js-es lts verzió letöltése a Windows rendszerhez](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése

Használjon [gulp.js](http://gulpjs.com) Pi a minta-alkalmazás központi telepítésének automatizálásához. Használja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.

Nyisson meg egy parancssort rendszergazdaként. Telepítés `gulp` és `device-discovery-cli` a következő parancs futtatásával:

```bash
npm install -g device-discovery-cli gulp
```

Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, tekintse meg a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.

## <a name="summary"></a>Összefoglalás

A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése. A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.

## <a name="next-steps"></a>Következő lépések

[A villogóalkalmazás elkészítése és üzembe helyezése](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
