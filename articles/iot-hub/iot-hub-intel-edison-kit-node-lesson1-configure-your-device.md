---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Intel Edison konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Állítsa be, arduino csatlakozni arduino pc, a telepítő arduino, arduino board"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 372c9b6d-e701-4ff6-8151-d262aa76aa55
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 87bf3a917af096e43a43a2143afa4bf43a72d7fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a>Az Intel Edison konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Intel Edison konfigurálása az első alkalommal a üzenőfalon, működtetéséhez azt fel összegyűjtésével és konfigurációs eszköz telepítése az asztali operációs Flash Edison a belső vezérlőprogram, hogy beállította a jelszót, és csatlakoztassa Wi-Fi. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan Edison board össze, és kapcsolja be a mentést.
* Hogyan Edison a belső vezérlőprogram flash, jelszó beállítását, és csatlakozzon a Wi-Fi.

## <a name="what-you-need"></a>Mi szükséges
A művelet végrehajtásához a következő részek a Intel Edison Starter Kit kell:

* Intel® Edison modul
* Arduino bővítése tábla
* Bármely Oszlopelválasztó sávok vagy csavart a csomagban, beleértve a két csavart vizsgálókocsihoz a bővítés board és négy csavart és műanyag térköztartók modul a része.
* A típus egy USB-kábel Micro B
* A közvetlen aktuális (DC) tápegység. A tápegység kell tekinthető meg az alábbiak szerint:
  - 7-15V TARTOMÁNYVEZÉRLŐ
  - Legalább 1500mA
  - A Központ/belső PIN-kódot kell lennie a pozitív sarkpontot az energiaellátás

  ![Az Starter Kit dolgot](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Emellett a következőkre van szükség:

* Windows, Mac vagy Linux rendszerű számítógép.
* Vezeték nélküli kapcsolat Edison való csatlakozáshoz.
* Az internethez konfigurációs eszköz letöltése.

## <a name="assemble-your-board"></a>A tábla összeállítása

Ez a szakasz az Intel® Edison modul csatlakoztatni a bővítés board lépéseket tartalmazza.

1. A bővítés üzenőfalon, a bővítés táblán csavart, a modul a lyuk sorba állítása az Intel® Edison modul, a fehér vázlatban helyezze el.

2. Nyomja le a modul a szavakat alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. A bővítés board modul védelmére használja a két hexadecimális nuts (a csomagban található).

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Helyezze be elment egyet a négy sarok lyuk a bővítés táblán. Csavarás és szigoríthatja a csavart alakzatot fehér műanyag térköztartók egyikét.

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Ismételje meg a másik három sarok térköztartók.

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Most már a üzenőfalon kész.

   ![üzenőfalon kész](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Energiagazdálkodási Edison mentése

1. Csatlakoztassa a tápegység.

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. A zöld LED-jét (DS1 feliratú a táblán Arduino * bővítése) kell bonyolít le, és megvilágítottnak maradnak.

3. Várjon egy percet a kártya a rendszerindítás befejezéséhez.

   > [!NOTE]
   > Ha nem rendelkezik a tartományvezérlő tápegység, továbbra is a USB-porton keresztül board power. Lásd: `Connect Edison to your computer` című szakaszban talál információt. A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.

## <a name="connect-edison-to-your-computer"></a>Edison kapcsolódni a számítógéphez

1. Váltás a két micro USB-porttal felé mikrokapcsoló le, hogy Edison eszköz módban van. Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Váltás a mikrokapcsoló le](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. A micro USB-kábellel csatlakoztassa a felső micro USB-porttal.

   ![Felső micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Másik végén USB-kábellel csatlakoztassa a számítógépet.

   ![Számítógép USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).

## <a name="download-and-run-the-configuration-tool"></a>A konfigurációs eszköz letöltése és futtatása
A legfrissebb konfigurációs eszköz az beszerzése [erre a hivatkozásra](https://software.intel.com/en-us/iot/hardware/edison/downloads) tartozó a `Installers` fejléc. Az eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint

### <a name="flash-firmware"></a>Belső vezérlőprogram Flash
1. Az a `Set up options` kattintson `Flash Firmware`.
2. Válassza ki a lemezképet flash alakzatot a tábla a következő módszerek valamelyikével:
   - Töltse le, és a tábla a legújabb belső vezérlőprogram lemezképpel érhetők el az Intel flash, jelölje be `Download the latest image version xxxx`.
   - A tábla már mentette a számítógépen lemezképhez flash, jelölje be `Select the local image`. Keresse meg és jelölje ki a flash a kártyához kívánt lemezképet.
3. A telepítő eszköz megkísérli a tábla flash. A teljes villogó folyamat akár 10 percet is igénybe vehet.

### <a name="set-password"></a>Jelszó beállítása
1. Az a `Set up options` kattintson `Enable Security`.
2. Az Intel® Edison board egyéni nevet állíthatja be. Ez nem kötelező.
3. Adja meg a tábla jelszavát, majd kattintson az `Set password`.
4. A jelszót, amely később üzemszünetének.

### <a name="connect-wi-fi"></a>Wi-Fi csatlakozás
1. Az a `Set up options` kattintson `Connect Wi-Fi`. Várjon, amíg legfeljebb egy percig a számítógép elérhető Wi-Fi hálózatok keres.
2. Az a `Detected Networks` legördülő listára, válassza ki a hálózaton.
3. Az a `Security` legördülő listára, válassza ki a hálózati biztonság típusa.
4. Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.
5. Az IP-cím, amely később üzemszünetének.

> [!NOTE]
> Győződjön meg arról, hogy Edison és a számítógép ugyanahhoz a hálózathoz csatlakozik. A számítógép csatlakozik a Edison az IP-cím használatával.

Gratulálunk! Sikeresen konfigurálta az Edison.

## <a name="summary"></a>Összefoglalás
Ebben a cikkben korábban megtudta, hogyan állítsa össze a Edison üzenőfalon, a belső vezérlőprogram, a telepítő jelszó flash és konfigurációs eszköz használatával csatlakoztassa Wi-Fi. Vegye figyelembe, hogy a LED nem még bonyolít le. A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek mintaalkalmazás futó Edison előkészítésekor.

## <a name="next-steps"></a>Következő lépések
[Eszközök][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md