---
title: "Az Azure IoT - lecke 1 Connect Intel Edison (C): eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Edison első minta alkalmazásához macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git mac, telepítés csomópont js mac gépen"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 3f764f2e-25fa-4dde-9e8d-d278453fccfd
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 27939f731121522f688e606052492bda8ae045fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a>Eszközök beszerzése (macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog
Töltse le a fejlesztői eszközök és a szoftver az Intel Edison első minta alkalmazásához. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

> [!NOTE]
> Bár a programozási nyelv, a fő logikájának C, Node.js eszközök szerepelnek a megszerzett létrehozásához és központi telepítéséhez alkalmazásokat.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan kell telepíteni a Git és Node.js.
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk a mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan további Node.js fejlesztői eszközök telepítése az NPM segítségével.
  * A Node.js minimálisan szükséges verziója a 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) a csomag kezelők, a Node.js egyike.

## <a name="what-you-need"></a>Mi szükséges
A művelet elvégzéséhez szüksége lesz:
* A fejlesztői eszközök és a szoftverfrissítések letöltése az internethez.
* MacOS Yosemite (10.10) futtató Mac vagy újabb.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js
A Git szoftver, Node.js telepítéséhez használja a [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:

1. Telepítse a Homebrew. Ha már telepített Homebrew, folytassa a 2.

   1. Nyomja le az `Cmd + Space` , és írja be `Terminal` egy terminált megnyitásához.
   2. Futtassa az alábbi parancsot:

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Telepítse a Git és a Node.js, a következő parancs futtatásával:

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése
Használjon [gulp.js](http://gulpjs.com) a Edison a minta-alkalmazás központi telepítésének automatizálásához.

Telepítse `gulp` a terminálban a következő parancs futtatásával:

```bash
sudo npm install -g gulp
```

Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.

## <a name="summary"></a>Összefoglalás
A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése. A következő feladata a létrehozásához, telepítéséhez és a Edison futtassa a mintaalkalmazást.

## <a name="next-steps"></a>Következő lépések
[A villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
