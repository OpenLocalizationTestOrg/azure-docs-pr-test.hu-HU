---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 1: eszközök (macOS) beszerzése |} Microsoft Docs"
description: "Töltse le és telepítse hello szükséges eszközök és szoftverek hello első mintaalkalmazás Edison macOS."
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
ms.openlocfilehash: 4a53331b0dce73c3dd51de91f07df86e28cbb6b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a>Hello eszközök (macOS 10.10) beolvasása
> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog
Hello Fejlesztőeszközök és hello első mintaalkalmazást az Intel Edison hello szoftver letöltése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

> [!NOTE]
> Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök hello során tapasztalatokat toobuild használt alkalmazások és központi telepítésekor minta.

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
Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooyour Edison tooautomate hello központi telepítését.

Telepítés `gulp` hello hello terminálban parancs a következő futtatásával:

```bash
sudo npm install -g gulp
```

Ha problémák macOS Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/osx) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás
Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.

## <a name="next-steps"></a>Következő lépések
[Hello villogási alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
