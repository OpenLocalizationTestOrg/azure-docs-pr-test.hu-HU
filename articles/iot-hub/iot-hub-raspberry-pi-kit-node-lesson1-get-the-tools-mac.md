---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftver, internet dolgot szoftver, python mac telepítése, telepítse a git a mac, Futtatás gulp, csomópont js mac telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 382b066cb7ece7ffdeb22b162b725727b22e5fac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a>Hello eszközök (macOS 10.10) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Hello Fejlesztőeszközök és hello első mintaalkalmazást a málna Pi 3 hello szoftver letöltése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooinstall a Git szoftver, Node.js.
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk hello-mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.
  * hello minimálisan szükséges verziója Node.js 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.

## <a name="what-you-need"></a>Mi szükséges
toocomplete ennél a műveletnél, szüksége lesz:

* Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.
* MacOS Yosemite (10.10) futtató Mac vagy újabb.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js
tooinstall Git és a Node.js, használja a hello [Homebrew](http://brew.sh) segédprogramja csomag a következő lépések végrehajtásával:

1. Telepítse a Homebrew. Ha már telepített Homebrew, nyissa meg toostep 2.
   
   1. Nyomja le az `Cmd + Space` , és írja be `Terminal` tooopen egy terminált.
   2. Futtassa a következő parancs hello:
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. Telepítse a Git és Node.js hello a következő parancs futtatásával:
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése
Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését. Használjon hello [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.

Telepítés `gulp` és `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:

```bash
npm install -g device-discovery-cli gulp
```

Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás
Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.

## <a name="next-steps"></a>Következő lépések
[Hello villogási minta alkalmazás létrehozását és telepítését](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

