---
title: "Connect Intel Edison (C) tooAzure IoT - lecke 1: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és Ubuntu hello szükséges eszközök és a szoftverek hello első mintaalkalmazás Edison telepítéséhez."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztői eszközök, az iot-fejlesztési, iot szoftver, internet dolgot szoftver, az ubuntu, telepítés csomópont js ubuntu telepítés git"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 4c7b8e04-b892-459b-8b03-85bcaff2465c
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1ebd599def4e8bb33d891517cc76bc2fcdc3c35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Hello eszközök (Ubuntu 16.04) beolvasása

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

* Hogyan tooinstall a Git szoftver, Node.js
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk hello-mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.
  * hello minimálisan szükséges verziója Node.js 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.

## <a name="what-you-need"></a>Mi szükséges
toocomplete ennél a műveletnél, szüksége lesz:
* Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.
* Ubuntu 16.04 vagy újabb rendszerrel működő számítógép.

## <a name="install-git-nodejs-and-npm"></a>Telepítse a Git, Node.js és NPM
Használjon hello billentyűparancsot `Ctrl + Alt + T` tooopen egy terminál és futtatási hello a következő parancsokat:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése
Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooEdison tooautomate hello központi telepítését.

Telepítés `gulp` hello hello terminálban parancs a következő futtatásával:

```bash
sudo npm install -g gulp
```

Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás
Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítése, és a Edison hello mintaalkalmazás futtatása.

## <a name="next-steps"></a>Következő lépések
[Hello villogási minta alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-application]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
