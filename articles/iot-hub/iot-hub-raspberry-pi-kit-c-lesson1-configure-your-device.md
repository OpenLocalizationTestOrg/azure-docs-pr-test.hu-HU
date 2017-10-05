---
title: "Az Azure IoT - lecke 1 Connect Raspberry pi (C): eszközök konfigurálása |} Microsoft Docs"
description: "Málna Pi 3 konfigurálása az első használatra, és telepítse a Raspbian az operációs rendszer, a szabad operációs rendszer, amely a málna Pi hardver van optimalizálva."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "telepítés raspbian, raspbian letöltése, telepítése raspbian, raspbian beállítás raspberry pi telepítési raspbian, raspberry pi telepítése operációs rendszer, raspberry pi sd-kártya telepítése, málna pi csatlakozásának, csatlakozni raspberry pi raspberry pi kapcsolat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 8ee9b23c-93f7-43ff-8ea1-e7761eb87a6f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2a380f78d67db47a0dcab5b90843404921510528
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a>Az eszköz konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Az első használatnál Pi konfigurálja, és a Raspbian operációs rendszer telepítése. Raspbian egy ingyenes operációs rendszer, amely a málna Pi hardver van optimalizálva. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan Raspbian Pi telepítése.
* Hogyan lehet a Pi bekapcsolására, USB-kábelen keresztül.
* Hogyan Pi csatlakozhat a hálózati kábel vagy vezeték nélküli hálózat használatával.
* Hogyan LED hozzáadása a breadboard Pi csatlakoztassa.

## <a name="what-you-need"></a>Mi szükséges
A művelet végrehajtásához a következő részek a málna Pi 3 Starter Kit kell:

* A Pi 3 málna tábla
* A 16 GB-os microSD-kártyán
* Az 5-volt 2-amp tápegység a 6-mértékű kiszolgálóhasználat micro USB-kábellel
* A breadboard
* Összekötő fenyegetéseknek
* Egy 560 ohmos ellenállás
* Egy szórt 10 mm LED
* Az Ethernet-kábel

![Az Starter Kit dolgot](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Emellett a következőkre van szükség:

* Vezetékes vagy vezeték nélküli kapcsolat pi való csatlakozáshoz.
* Egy USB-SD adapter vagy mini-SD-kártyán írása az operációs rendszer képet a microSD-kártyán.
* Windows, Mac vagy Linux rendszerű számítógép. A számítógép a microSD-kártyán Raspbian telepítéséhez használt.
* Az internethez, a szükséges eszközöket és a szoftverek letöltéséhez.

## <a name="install-raspbian-on-the-microsd-card"></a>Telepítse a MicroSD-kártyán Raspbian
Készítse elő a microSD-kártyán Raspbian kép telepítéséhez.

1. Töltse le a Raspbian.
   1. [Töltse le](https://www.raspberrypi.org/downloads/raspbian/) Raspbian Jessie a Pixel a .zip fájlt.
   2. Bontsa ki a Raspbian lemezképet a számítógép egyik mappájába.
2. A microSD-kártyán Raspbian telepítése.
   1. [Töltse le](https://www.etcher.io) és telepítse a Etcher SD-kártya író segédprogramot.
   2. Futtassa a Etcher, és az 1. lépésben válassza ki a kibontott Raspbian kép.
   3. Jelölje ki a microSD-kártyát meghajtót.
      Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott ki a megfelelő meghajtó.
   4. Kattintson a **Flash** a microSD-kártyán Raspbian telepítése.
   5. Eltávolítja a számítógépről a microSD-kártyán, ha a telepítés befejeződött.
      Biztonságos a microSD-kártyán közvetlenül eltávolítani, mert Etcher automatikusan kiadása vagy leválasztja a microSD-kártyán befejezése után is.
   6. A Pi a microSD-kártyán beilleszteni.

![Az SD-kártya beszúrása](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>A Pi bekapcsolása
Kapcsolja be a Pi a micro USB-kábelen és a tápegység.

![Bekapcsolás](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Fontos, hogy a tápegység használja a csomag, amely nem alacsonyabb 2A. Győződjön meg arról, hogy a málna rendelkezik-e elegendő power megfelelő működéséhez.

## <a name="enable-ssh"></a>SSH engedélyezése
A November 2016 kiadás Raspbian rendelkezik az SSH-kiszolgálót, alapértelmezés szerint le van tiltva. Engedélyezze manuálisan kell. Olvassa el a [hivatalos utasításokat](https://www.raspberrypi.org/documentation/remote-access/ssh/) vagy csatlakoztassa egy figyelő, és navigáljon **beállítások -> málna Pi konfigurációs** SSH engedélyezéséhez.

## <a name="connect-raspberry-pi-3-to-the-network"></a>Málna Pi 3 csatlakoznak a hálózathoz
A Pi kapcsolódhatnak egy vezetékes hálózathoz vagy vezeték nélküli hálózathoz. Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik. Például Pi is csatlakozik ugyanahhoz a kapcsolóhoz, amely a számítógép csatlakozik.

### <a name="connect-to-a-wired-network"></a>Kapcsolódjon vezetékes hálózathoz
Az Ethernet kábellel Pi a vezetékes hálózathoz való kapcsolódáshoz. A Pi két LED bekapcsolása, ha létrejött a kapcsolat.

![Csatlakozás az Ethernet-kábel segítségével](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a>A vezeték nélküli hálózat
Kövesse a [utasításokat](https://www.raspberrypi.org/learning/software-guide/wifi/) a Pi a vezeték nélküli hálózathoz való kapcsolódáshoz málna Pi alapját. Ezek az utasítások igényli, hogy először csatlakoztassa a figyelő és a billentyűzet Pi.

## <a name="connect-the-led-to-pi"></a>Csatlakozás a LED Pi
A feladat végrehajtásához használja a [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), az összekötő fenyegetéseknek, a LED-jét, és a ellenállás. Csatlakoztassa őket, hogy a [általános célú bemeneti/kimeneti](https://www.raspberrypi.org/documentation/usage/gpio/) pi (GPIO) portok.

![Breadboard LED és ellenállás](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Csatlakozás a LED rövidebb szakasza **GPIO GND (PIN-kód 6)**.
2. Csatlakoztassa a LED hosszabb szakasza a ellenállás egy szakasza.
3. Csatlakoztassa a többi szakasza a ellenállás **GPIO 4 (PIN-kód 7)**.

Ne feledje, hogy a LED polaritás fontos. Ez a beállítás polaritás aktív alacsony gyakran nevezik.

![Átalakítókábelre](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Gratulálunk! Sikeresen konfigurálta az Pi.

## <a name="summary"></a>Összefoglalás
Ebben a cikkben megtanulta már Raspbian telepítésével, Pi csatlakoznak a hálózathoz, és LED kapcsolódás Pi Pi konfigurálása. Vegye figyelembe, hogy a LED nem még bonyolít le. A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek mintaalkalmazás futó Pi előkészítésekor.

![Készen áll a hardver](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Következő lépések
[Eszközök](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

