---
title: "Az Azure IoT - lecke 1 Connect Raspberry pi (C): eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközöket és a szoftver az első mintaalkalmazás pi Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git az ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 28ebba82e90d91470518cd830c96e6da39d8b9b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Eszközök beszerzése (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Töltse le a fejlesztői eszközök és a szoftver a málna Pi 3 első minta alkalmazásához. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> Bár a programozási nyelv, a fő logikájának C, Node.js eszközök használata a megszerzett az eszközök, és felépítéséhez és minta alkalmazások központi telepítése.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* A Git szoftver, Node.js telepítése
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk a mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.
  * A Node.js minimálisan szükséges verziója a 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.

## <a name="what-you-need"></a>Mi szükséges
A művelet elvégzéséhez szüksége lesz:

* A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.
* Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.

## <a name="install-git-nodejs-and-npm"></a>Telepítse a Git, Node.js és NPM
Használja a billentyűparancsot `Ctrl + Alt + T` nyisson meg egy terminált, és futtassa a következő parancsokat:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése
Használjon [gulp.js](http://gulpjs.com) Pi a minta-alkalmazás központi telepítésének automatizálásához. Használja a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) az IoT-eszközök hálózati adatainak lekérésére.

Telepítés `gulp` és `device-discovery-cli` a terminálban a következő parancs futtatásával:

```bash
sudo npm install -g device-discovery-cli gulp
```

Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) gyakori problémák megoldásainak.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.

## <a name="summary"></a>Összefoglalás
A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése. A következő feladat létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Pi-hoz.

## <a name="next-steps"></a>Következő lépések
[A villogóalkalmazás elkészítése és üzembe helyezése](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

