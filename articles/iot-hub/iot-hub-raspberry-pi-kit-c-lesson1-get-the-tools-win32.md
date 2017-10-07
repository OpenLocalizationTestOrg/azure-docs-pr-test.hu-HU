---
title: 'Connect Raspberry pi (C) tooAzure IoT - lecke 1: Get tools (Windows) |} Microsoft Docs'
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Pi telepítéséhez Windows 7 és újabb verziók."
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
ms.openlocfilehash: 70ae6d15f9d6af116ff065a79a30d99afc67bffd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a>Első hello eszközök (Windows 7 vagy újabb)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a>Mit fog
Hello Fejlesztőeszközök és hello első mintaalkalmazás málna Pi 3 hello szoftver letöltése. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

> [!NOTE]
> Bár programozási nyelv hello fő logikájának hello C, Node.js eszközök szerepelnek hello során tapasztalatokat toodiscover eszközök, és hozza létre, és minta alkalmazások központi telepítése.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooinstall a Git szoftver, Node.js.
  * [Git](https://git-scm.com) van egy nyílt forráskódú elosztott verziókezelő rendszer. Ez a cikk hello-mintaalkalmazás Git tárolja.
  * [NODE.js](https://nodejs.org/en/) van a JavaScript futásidejű és gazdag csomag-ökoszisztéma.
* Hogyan toouse NPM tooinstall további Node.js fejlesztői eszközök.
  * Node.js hello minimális verziójára vonatkozó követelményt a 4.5-ös LTS.
  * [NPM](https://www.npmjs.com) egyike hello Node.js csomag feletteseit.

## <a name="what-you-need"></a>Mi szükséges

toocomplete ennél a műveletnél, szüksége lesz:

* Az Internet kapcsolat toodownload hello Fejlesztőeszközök és hello szoftver.
* Windows rendszerű számítógép.

## <a name="install-git-and-nodejs"></a>Telepítse a Git szoftver, Node.js

Alább toodownload hello hivatkozásaira kattint, és telepítse a Git és a Node.js-es lts verzió a Windows.

* [Git letöltése a Windows rendszerhez](https://git-scm.com/download/win/)
* [Node.js-es lts verzió letöltése a Windows rendszerhez](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a>További Node.js fejlesztői eszközök telepítése

Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooPi tooautomate hello központi telepítését. Használjon hello [cli-eszköz-felderítési](https://github.com/Azure/device-discovery-cli) tooretrieve hálózati adatokat az IoT-eszközökről.

Nyisson meg egy parancssort rendszergazdaként. Telepítés `gulp` és `device-discovery-cli` hello a következő parancs futtatásával:

```bash
npm install -g device-discovery-cli gulp
```

Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója](iot-hub-raspberry-pi-kit-c-troubleshooting.md) a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás

Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítése, és futtassa a hello mintaalkalmazást a Pi.

## <a name="next-steps"></a>Következő lépések

[Hello villogási alkalmazás létrehozását és telepítését](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
