---
title: "Az Azure IoT - lecke 1 Connect Arduino (C): eszközök konfigurálása |} Microsoft Docs"
description: "Adafruit lágyított M0 Wi-Fi konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Állítsa be, arduino csatlakozni arduino pc, a telepítő arduino, arduino board"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9e319292e5d30dea7e45857e435825861aad1c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a>Az eszköz konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Konfigurálja a Adafruit lágyított M0 Wi-Fi Arduino üzenőfalon, az első használatnál össze a üzenőfalon, működtetéséhez azt be. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Mi szükséges
A művelet végrehajtásához a következő részek a Adafruit lágyított M0 Wi-Fi Starter Kit szüksége:

* A Wi-Fi M0 Adafruit lágyított tábla
* A típus egy USB-kábel Micro B

![csomag][kit]

Emellett a következőkre van szükség:

* Windows, Mac vagy Linux rendszerű számítógép.
* Vezeték nélküli kapcsolat a Arduino kártya való csatlakozáshoz.
* Az internethez konfigurációs eszköz letöltése.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Megtudhatja, hogyan állíthatja össze a Arduino üzenőfalon, és kapcsolja be az szolgáltatáshoz a következő rendelkezésre.
* Hogyan Ubuntu soros port engedélyek hozzáadásához.

## <a name="connect-your-arduino-board-to-your-computer"></a>A Arduino board kapcsolódni a számítógéphez

1. A micro USB-kábellel csatlakoztassa a felső micro USB-porttal.

   ![Felső micro USB-port][top-micro-usb-port]

2. Másik végén USB-kábellel csatlakoztassa a számítógépet.

   ![Számítógép USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Ubuntu a soros port engedélyek hozzáadása

Ez a szakasz kihagyhatja, ha a Windows vagy macOS. Ubuntu van szüksége az alábbi lépések végrehajtásával győződjön meg arról a normál linux felhasználó jogosult az USB-port a Arduino kártya működéséhez.

1. Most már a normál felhasználói terminálról:

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   Hasonlót fog kapni:

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   A "0" lehet, hogy egy másik számot, vagy esetleg visszaadott több bejegyzés. Az első esetben az adatokat, igazolnia kell van `uucp`, a második van `dialout`, vagyis a fájl a csoport tulajdonosa.

2. Adja hozzá felhasználót a csoporthoz:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Ha `group-name` az első lépésben található adatok és `username` a linux-felhasználónév.

3. Szüksége lesz, és a módosítás érvénybe lépéséhez, majd fejezze be a telepítő újbóli bejelentkezésre.

## <a name="summary"></a>Összefoglalás
Ebben a cikkben a Arduino board konfigurálása hogy megismerte. A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek előkészítésekor a Arduino indítópanel mintaalkalmazás futtatása.

![Készen áll a hardver][hardware-is-ready]

## <a name="next-steps"></a>Következő lépések
[Eszközök][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md