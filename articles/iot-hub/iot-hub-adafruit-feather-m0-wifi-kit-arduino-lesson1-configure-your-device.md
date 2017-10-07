---
title: "Connect Arduino (C) tooAzure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Adafruit lágyított M0 Wi-Fi konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "beállítása, arduino csatlakozás arduino toopc, a telepítő arduino, arduino board"
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
ms.openlocfilehash: 30b764e8ff6221995456283a226e79f064b2d74e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a>Az eszköz konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Konfigurálja a Adafruit lágyított M0 Wi-Fi Arduino üzenőfalon, az első használatnál összeállításakor hello üzenőfalon, működtetéséhez azt be. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Mi szükséges
toocomplete Ez a művelet következő részek a Adafruit lágyított M0 Wi-Fi Starter Kit hello szüksége:

* hello Adafruit lágyított M0 Wi-Fi tábla
* Egy Micro B tooType egy USB-kábellel

![csomag][kit]

Emellett a következőkre van szükség:

* Windows, Mac vagy Linux rendszerű számítógép.
* A Arduino Bizottság tooconnect a vezeték nélküli kapcsolat.
* Az internetes kapcsolat toodownload konfigurációs eszközt.

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooassemble a Arduino tábla és a tápkábelek azt az alábbi hello szolgáltatáshoz tanítás.
* Hogyan Ubuntu tooadd soros port engedélyeit.

## <a name="connect-your-arduino-board-tooyour-computer"></a>A Arduino board tooyour számítógép

1. Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.

   ![Felső micro USB-port][top-micro-usb-port]

2. Plug hello USB-kábel másik végén a számítógépbe.

   ![Számítógép USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Ubuntu a soros port engedélyek hozzáadása

Ez a szakasz kihagyhatja, ha a Windows vagy macOS. Ubuntu, a következő lépések toomake meg arról, hogy hello normál linux felhasználó a Arduino kártya hello USB-porttal rendelkezik a hello engedélyek toooperate hello kell.

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

   hello "0" lehet egy másik számot, vagy esetleg visszaadott több bejegyzést. Hello azt az első eset hello adatok, igazolnia kell a `uucp`, hello második van `dialout`, amely hello fájl hello csoport tulajdonosa.

2. Felhasználói toohello toohello csoport hozzáadása:

   ```bash
   sudo usermod -a -G group-name username
   ```

   Ha `group-name` hello első lépésben található hello adat és `username` a linux-felhasználónév.

3. Toolog és újra kell használni a tootake hatás módosítása és a teljes hello beállítása.

## <a name="summary"></a>Összefoglalás
A cikkben, hogy megismerte hogyan tooconfigure a Arduino tábla. hello következő feladata tooinstall hello szükséges eszközök és szoftverek előkészítésekor a Arduino indítópanel mintaalkalmazás futtatása.

![Készen áll a hardver][hardware-is-ready]

## <a name="next-steps"></a>Következő lépések
[Hello eszközök beszerzése][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md