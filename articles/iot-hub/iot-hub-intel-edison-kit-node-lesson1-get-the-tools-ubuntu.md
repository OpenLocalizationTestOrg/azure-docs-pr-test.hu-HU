---
title: "Csatlakozás Azure IoT - lecke 1 Intel Edison (csomópont): (Ubuntu) eszközök beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Edison első minta alkalmazásához Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztői eszközök, az iot-fejlesztési, iot szoftver, internet dolgot szoftver, az ubuntu, telepítés csomópont js ubuntu telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 9ab5b161-7ec5-41a6-9c5f-4456e4882752
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 74c5f06c2b12d140814bfb75125d60b83addf70c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a>Eszközök beszerzése (Ubuntu 16.04)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog
Töltse le a fejlesztői eszközök és a szoftver az Intel Edison első minta alkalmazásához. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

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
Használjon [gulp.js](http://gulpjs.com) Edison a minta-alkalmazás központi telepítésének automatizálásához.

Telepítse `gulp` a terminálban a következő parancs futtatásával:

```bash
sudo npm install -g gulp
```

Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.

## <a name="summary"></a>Összefoglalás
A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése. A következő feladata a létrehozásához, telepítéséhez és a Edison futtassa a mintaalkalmazást.

## <a name="next-steps"></a>Következő lépések
[Létrehozhat és telepíthet a villogási mintaalkalmazás][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
