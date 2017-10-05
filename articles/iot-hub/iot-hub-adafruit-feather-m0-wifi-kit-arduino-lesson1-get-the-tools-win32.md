---
title: "Csatlakozás Azure IoT - lecke 1 Arduino: Get tools (Windows) |} Microsoft Docs"
description: "Töltse le és telepítse a szükséges eszközök és szoftverek Adafruit lágyított M0 Wi-Fi első minta alkalmazásához Windows 7 és újabb verziók."
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
ms.openlocfilehash: 5d27c016c4a74e31455e676b3c3070a8e262b21f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a>Szerezze be az eszközöket (Windows 7 vagy újabb)

> [!div class="op_single_selector"]
> * [Windows 7 vagy újabb][windows]
> * [Ubuntu 16.04][ubuntu]
> * [macOS 10.10][macos]

## <a name="what-you-will-do"></a>Mit fog

A fejlesztői eszközök és a szoftver a Adafruit lágyított M0 Wi-Fi Arduino kártya első mintaalkalmazás letöltése.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

> [!NOTE]
> Bár a programozási nyelv, a fő logikájának Arduino, Node.js eszközök szerepelnek a megszerzett létrehozásához és központi telepítéséhez alkalmazásokat.

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

Használjon [gulp.js](http://gulpjs.com) a Arduino táblán a minta-alkalmazás központi telepítésének automatizálásához.

Nyisson meg egy parancssort rendszergazdaként. Telepítés `gulp`, `device-discovery-cli` a terminálban a következő parancs futtatásával:

```bash
npm install -g gulp device-discovery-cli
```

Ha problémák, Node.js és a további Node.js fejlesztői eszközök telepítése a számítógépre, tekintse meg a [hibaelhárítási útmutatója] [ troubleshooting] gyakori problémák megoldásainak.

## <a name="install-visual-studio-code"></a>Visual Studio Code telepítése

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és a Visual Studio Code telepítése. A Visual Studio Code a Windows, Linux és macOS egy egyszerűsített, de hatékony forráskód szerkesztőjében. A mintakód szerkesztése a szerkesztő használata az oktatóanyag későbbi részében.

## <a name="summary"></a>Összefoglalás

A szükséges fejlesztői eszközök és az első mintaalkalmazás szoftver telepítése. A következő feladata a létrehozásához, telepítéséhez és futtassa a mintaalkalmazást a Arduino táblán.

## <a name="next-steps"></a>Következő lépések

[Létrehozhat és telepíthet a villogási mintaalkalmazás][create-and-deploy-the-blink-sample-application]
<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md