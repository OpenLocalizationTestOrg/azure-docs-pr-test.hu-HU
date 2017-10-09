---
title: "Csatlakozás Arduino tooAzure IoT - lecke 1: Get tools (Windows) |} Microsoft Docs"
description: "Töltse le és hello szükséges eszközök és szoftverek hello első mintaalkalmazás Adafruit lágyított M0 Wi-Fi telepítéséhez Windows 7 és újabb verziók."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino fejlesztőeszközök, iot fejlesztési, iot szoftver, internet dolgot szoftver, telepítse git csomópont js windows telepítése a windows rendszeren"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9cfb8cd2-eafb-4ba2-b23e-d94e114ff3a6
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4dd946da6c84293987e166fd1d17fac117e94e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a>Első hello eszközök (Windows 7 vagy újabb)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog

Hello Fejlesztőeszközök és hello szoftver hello a Adafruit lágyított M0 Wi-Fi Arduino kártya első mintaalkalmazás letöltése.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

> [!NOTE]
> Bár programozási nyelv hello fő logikájának hello Arduino, Node.js eszközök hello során tapasztalatokat toobuild használt alkalmazások és központi telepítésekor minta.

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

Használjon [gulp.js](http://gulpjs.com) hello minta alkalmazás tooyour Arduino board tooautomate hello központi telepítését.

Nyisson meg egy parancssort rendszergazdaként. Telepítés `gulp`, `device-discovery-cli` hello hello terminálban parancs a következő futtatásával:

```bash
npm install -g gulp device-discovery-cli
```

Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, lásd: hello [hibaelhárítási útmutatója] [ troubleshooting] a megoldások toocommon problémákat.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A szerkesztő később hello oktatóanyag tooedit hello mintakód használható.

## <a name="summary"></a>Összefoglalás

Szükséges hello fejlesztői eszközök és szoftverek hello első mintaalkalmazás telepítése. hello tovább feladat toocreate, telepítheti, és futtassa a hello mintaalkalmazást a Arduino táblán.

## <a name="next-steps"></a>Következő lépések

[Hello villogási minta alkalmazás létrehozását és telepítését][create-and-deploy-the-blink-sample-application]
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md