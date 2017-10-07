---
title: "Connect Raspberry pi (C) tooAzure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Málna Pi 3 konfigurálása az első használatra, és telepítse a hello Raspbian OS, egy ingyenes operációs rendszer, amely hello málna Pi hardver van optimalizálva."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "telepítés raspbian, raspbian letöltési tooinstall raspbian raspbian raspberry a telepítés pi telepítése raspbian raspberry pi telepítése operációs rendszer, raspberry pi sd kártya telepítés raspberry pi csatlakozásának, csatlakozás tooraspberry pi raspberry pi kapcsolat"
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
ms.openlocfilehash: ba3466f6d5d46352326a2a63eb011e117da5aca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a>Az eszköz konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Az első használatnál Pi konfigurálja, és hello Raspbian operációs rendszer telepítéséhez. Raspbian egy ingyenes operációs rendszer, amely hello málna Pi hardver van optimalizálva. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooinstall a Pi Raspbian.
* Hogyan toopower Pi mentése USB-kábelen keresztül.
* Hogyan tooconnect Pi toohello hálózati kábel vagy vezeték nélküli hálózat használatával.
* Hogyan tooadd egy LED toohello breadboard és tooPi csatlakoztassa.

## <a name="what-you-need"></a>Mi szükséges
toocomplete Ez a művelet következő részek a málna Pi 3 Starter Kit hello szüksége:

* hello málna Pi 3 tábla
* hello 16 GB-os microSD-kártyán
* hello 5-volt 2-amp tápegység hello 6-mértékű kiszolgálóhasználat micro USB-kábellel
* hello breadboard
* Összekötő fenyegetéseknek
* Egy 560 ohmos ellenállás
* Egy szórt 10 mm LED
* hello kábel

![Az Starter Kit dolgot](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Emellett a következőkre van szükség:

* A Pi tooconnect a vezetékes vagy vezeték nélküli kapcsolat.
* Egy USB-SD adapter vagy mini-SD-kártya tooburn hello OS képet hello microSD-kártyán.
* Windows, Mac vagy Linux rendszerű számítógép. hello számítógép használt tooinstall Raspbian hello microSD-kártyán.
* Az Internet kapcsolat toodownload hello szükséges eszközök és szoftverek.

## <a name="install-raspbian-on-hello-microsd-card"></a>Hello MicroSD-kártyán Raspbian telepítése
Készítse elő a hello microSD-kártyán hello Raspbian lemezkép telepítéséhez.

1. Töltse le a Raspbian.
   1. [Töltse le](https://www.raspberrypi.org/downloads/raspbian/) hello .zip-fájlt a Pixel Raspbian Jessie.
   2. Bontsa ki a hello Raspbian kép tooa mappát a számítógépén.
2. Telepítse a Raspbian toohello microSD-kártyán.
   1. [Töltse le](https://www.etcher.io) és hello Etcher SD-kártya író segédprogram telepítéséhez.
   2. Futtassa a Etcher és hello Raspbian kép kibontott válassza az 1. lépésben.
   3. Válassza ki a hello microSD-kártyát meghajtó.
      Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott hello megfelelő meghajtót.
   4. Kattintson a **Flash** tooinstall Raspbian toohello microSD-kártyán.
   5. Ha a telepítés hello microSD-kártyán eltávolítása a számítógépről.
      Ennek az oka biztonságos tooremove hello microSD-kártyán közvetlenül Etcher automatikusan kiadása vagy leválasztja hello microSD-kártyán befejezését követően.
   6. A Pi hello microSD-kártyán beilleszteni.

![Hello SD-kártya behelyezése](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>A Pi bekapcsolása
Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység.

![Bekapcsolás](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Fontos fontos toouse hello tápegység hello Kit legalább 2/a. toomake, hogy a málna megfelelően van-e elegendő power toowork.

## <a name="enable-ssh"></a>SSH engedélyezése
Frissítésétől hello 2016. novemberi kiadásban Raspbian hello SSH-kiszolgálót alapértelmezés szerint engedélyezve van. Tooenable kell azt manuálisan. Olvassa el a toohello [hivatalos utasításokat](https://www.raspberrypi.org/documentation/remote-access/ssh/) vagy csatlakoztassa egy figyelő, és lépjen túl**beállítások -> málna Pi konfigurációs** tooenable SSH.

## <a name="connect-raspberry-pi-3-toohello-network"></a>Málna Pi 3 toohello hálózat
Vezetékes vagy vezeték nélküli hálózaton tooa Pi tooa is elérheti. Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello. Például csatlakoztathatja a Pi toohello azonos váltani, hogy a számítógép csatlakoztatva van.

### <a name="connect-tooa-wired-network"></a>Csatlakozás tooa vezetékes hálózathoz
Hello Ethernet kábel tooconnect vezetékes hálózathoz Pi tooyour használja. hello a Pi két LED bekapcsolása, ha hello kapcsolatot.

![Csatlakozás az Ethernet-kábel segítségével](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a>Tooa vezeték nélküli hálózat
Hajtsa végre a hello [utasításokat](https://www.raspberrypi.org/learning/software-guide/wifi/) hello málna Pi Foundation tooconnect Pi tooyour vezeték nélküli hálózathoz. Ezek az utasítások használatba toofirst csatlakozni a figyelő és a billentyűzet tooPi.

## <a name="connect-hello-led-toopi"></a>Csatlakozás hello LED tooPi
toocomplete ezt a feladatot, használjon hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello összekötő fenyegetéseknek, hello LED és ellenállás hello. Csatlakoztassa őket toohello [általános célú bemeneti/kimeneti](https://www.raspberrypi.org/documentation/usage/gpio/) pi (GPIO) portok.

![Breadboard LED és ellenállás](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Hello LED rövidebb szakasza hello túl csatlakozás**GPIO GND (PIN-kód 6)**.
2. Csatlakozás hello hello LED tooone szakasza hello ellenállás hosszabb szakasza.
3. Csatlakozás más hello ellenállás szakasza túl hello**GPIO 4 (PIN-kód 7)**.

Vegye figyelembe, hogy hello LED polaritás fontos. Ez a beállítás polaritás aktív alacsony gyakran nevezik.

![Átalakítókábelre](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Gratulálunk! Sikeresen konfigurálta az Pi.

## <a name="summary"></a>Összefoglalás
A cikkben, hogy megismerte hogyan tooconfigure helyszerepköreinek Raspbian, kapcsolódó Pi tooa hálózati kapcsolódás egy LED tooPi Pi tartományban. Vegye figyelembe, hogy hello LED nem még bonyolít le. hello következő feladata tooinstall hello szükséges eszközök és szoftverek mintaalkalmazás futó Pi előkészítésekor.

![Készen áll a hardver](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Következő lépések
[Hello eszközök beszerzése](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

