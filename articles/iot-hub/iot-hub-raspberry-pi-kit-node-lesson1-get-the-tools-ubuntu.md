---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: eszközök (Ubuntu) beszerzése |} Microsoft Docs"
description: "Töltse le és Ubuntu hello szükséges eszközök és a szoftverek hello első mintaalkalmazás Pi telepítéséhez."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-fejlesztési, iot szoftverek, internet dolgot szoftver, telepítse a git az ubuntu, Futtatás gulp, csomópont js ubuntu telepítése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f566fa0d1faf8b2321707145f675e3d87f0bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a>Hello eszközök (Ubuntu 16.04) beolvasása

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a>Mit fog
Hello Fejlesztőeszközök és hello első mintaalkalmazás málna Pi 3 hello szoftver letöltése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooinstall a Git szoftver, Node.js.
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk hello-mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.
  * hello minimálisan szükséges verziója Node.js 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.

## <a name="what-do-you-need"></a>Mire van szüksége
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
Használhat [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését. Hello is a [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.

Telepítés `gulp` és `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:

```bash
sudo npm install -g device-discovery-cli gulp
```

Ha problémák Ubuntu Node.js és a további fejlesztői eszközök telepítése, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése
[Töltse le](https://code.visualstudio.com/docs/setup/linux) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás
Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.

## <a name="next-steps"></a>Következő lépések
[Hello villogási minta alkalmazás létrehozását és telepítését](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

