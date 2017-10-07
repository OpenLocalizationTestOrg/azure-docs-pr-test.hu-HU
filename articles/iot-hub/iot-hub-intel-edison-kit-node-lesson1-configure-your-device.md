---
title: "Intel Edison (csomópont) tooAzure IoT - lecke 1 Kapcsolódás: eszközök konfigurálása |} Microsoft Docs"
description: "Intel Edison konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "beállítása, arduino csatlakozás arduino toopc, a telepítő arduino, arduino board"
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
ms.openlocfilehash: c0609542a49924c979f71297d808c09acefca0bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a>Az Intel Edison konfigurálása
## <a name="what-you-will-do"></a>Mit fog
Első alkalommal hello üzenőfalon, működtetéséhez azt fel összegyűjtésével Intel Edison konfigurálása és telepítése konfigurációs eszköz tooyour asztali operációs rendszer tooflash Edison a belső vezérlőprogram, állítsa be a jelszavát, és csatlakoztassa tooWi-Fi. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan tooassemble Edison board, és kapcsolja be a mentést.
* Hogyan tooflash Edison belső vezérlőprogram, állítsa be a jelszót, és csatlakozzon a Wi-Fi.

## <a name="what-you-need"></a>Mi szükséges
toocomplete Ez a művelet következő részek a Intel Edison Starter Kit hello szüksége:

* Intel® Edison modul
* Arduino bővítése tábla
* Bármely Oszlopelválasztó sávok vagy csavart hello csomagolása, beleértve a két csavart toofasten hello modul toohello bővítése board és négy csavart és műanyag térköztartók szerepel.
* Egy Micro B tooType egy USB-kábellel
* A közvetlen aktuális (DC) tápegység. A tápegység kell tekinthető meg az alábbiak szerint:
  - 7-15V TARTOMÁNYVEZÉRLŐ
  - Legalább 1500mA
  - hello center/belső PIN-kód hello pozitív sarkpontot hello energiaellátás kell lennie.

  ![Az Starter Kit dolgot](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Emellett a következőkre van szükség:

* Windows, Mac vagy Linux rendszerű számítógép.
* Egy Edison tooconnect a vezeték nélküli kapcsolat.
* Az internetes kapcsolat toodownload konfigurációs eszközt.

## <a name="assemble-your-board"></a>A tábla összeállítása

Jelen szakaszban található lépéseket tooattach az Intel® Edison modul tooyour bővítése tábla.

1. A bővítés üzenőfalon, hello lyuk hello modul hello csavart hello bővítése táblán a sorba állítása hello Intel® Edison modul belül fehér hello vázlat helyezze el.

2. Nyomja le a hello modul hello szavak alatt `What will you make?` csak úgy érzi, hogy a beépülő modult.

   ![indítópanel 2 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. A két hello (hello csomagban található) hexadecimális nuts toosecure hello modul toohello bővítése tábla használatával.

   ![indítópanel 3 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Helyezze be a elment hello négy sarok lyukat hello bővítése táblán egyikében. Csavarás és szigoríthatja fehér hello műanyag térköztartók alakzatot hello csavart egyikét.

   ![indítópanel 4 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Ismételje meg a másik három sarok térköztartók hello.

   ![indítópanel 5 összeállítása](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Most már a üzenőfalon kész.

   ![üzenőfalon kész](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Energiagazdálkodási Edison mentése

1. Beépülő modul hello tápegység.

   ![Beépülő modul tápegység](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. A zöld LED-jét (DS1 feliratú a hello Arduino * bővítése board) kell bonyolít le, és megvilágítottnak maradnak.

3. Várjon egy percet a hello board toofinish másolatából végrehajtott indítása.

   > [!NOTE]
   > Ha egy tartományvezérlő tápegység nem rendelkezik, akkor is power hello board USB-porton keresztül. Lásd: `Connect Edison tooyour computer` című szakaszban talál információt. A tábla, ilyen módon működtetéséhez azt eredményezheti, hogy a tábláról előre nem látható viselkedéshez különösen akkor, ha a Wi-Fi használatával, vagy befolyásoló tényezők motorok.

## <a name="connect-edison-tooyour-computer"></a>Edison tooyour számítógép

1. Váltás le hello mikrokapcsoló felé hello két micro USB-porttal, így Edison eszköz módban van. Az eszköz és a gazdagép üzemmód közötti különbségeket, tekintse át [Itt](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Váltás a hello mikrokapcsoló](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.

   ![Felső micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Plug hello USB-kábel másik végén a számítógépbe.

   ![Számítógép USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Tudni fogják, hogy a tábla teljes inicializálását, amikor a számítógép csatlakoztat egy új meghajtót (hasonlóan egy SD-kártya beszúrása a számítógép).

## <a name="download-and-run-hello-configuration-tool"></a>Hello konfigurációs eszköz letöltése és futtatása
Hello legújabb konfiguráló eszköz az beszerzése [Ez a hivatkozás](https://software.intel.com/en-us/iot/hardware/edison/downloads) alatt hello feltüntetve `Installers` fejléc. Hello eszköz hajtható végre, és kövesse a képernyőn megjelenő utasításokat, amennyiben szükséges Tovább gombra kattint

### <a name="flash-firmware"></a>Belső vezérlőprogram Flash
1. A hello `Set up options` kattintson `Flash Firmware`.
2. Válassza ki a hello kép tooflash alakzatot a tábla hello következő tevékenységek végrehajtásával:
   - toodownload és a flash hello legújabb belső vezérlőprogram lemezképpel érhetők el az Intel, a tábla válasszon `Download hello latest image version xxxx`.
   - tooflash a tábla olyan képpel már mentette a számítógépen, válassza ki `Select hello local image`. Keresse meg a tooand válassza hello kép tooflash tooyour board szeretné.
3. hello telepítő eszköz tooflash kísérli meg a tábla. hello teljes villogó folyamat too10 percig is tarthat.

### <a name="set-password"></a>Jelszó beállítása
1. A hello `Set up options` kattintson `Enable Security`.
2. Az Intel® Edison board egyéni nevet állíthatja be. Ez nem kötelező.
3. Adja meg a tábla jelszavát, majd kattintson az `Set password`.
4. Üzemszünetének hello jelszót, amelyre később szolgál.

### <a name="connect-wi-fi"></a>Wi-Fi csatlakozás
1. A hello `Set up options` kattintson `Connect Wi-Fi`. Várjon, amíg fel tooone perc, a számítógép vizsgálatok elérhető Wi-Fi-hálózatok.
2. A hello `Detected Networks` legördülő listára, válassza ki a hálózaton.
3. A hello `Security` legördülő listában, jelölje be hello hálózati biztonság típusa.
4. Adja meg a felhasználónevet és jelszót információkat, majd kattintson az `Configure Wi-Fi`.
5. Üzemszünetének hello IP-címet, amely később szolgál.

> [!NOTE]
> Győződjön meg arról, hogy Edison ugyanaz, mint a számítógép hálózati csatlakoztatott toohello. A számítógép csatlakozik a tooyour Edison hello IP-cím használatával.

Gratulálunk! Sikeresen konfigurálta az Edison.

## <a name="summary"></a>Összefoglalás
A cikkben hogy megismerte hogyan tooassemble hello Edison üzenőfalon, a belső vezérlőprogram, a telepítő jelszó flash, és csatlakoztassa tooWi-Fi configuration tool használatával. Vegye figyelembe, hogy hello LED nem még bonyolít le. hello következő feladata tooinstall hello szükséges eszközök és szoftverek mintaalkalmazás futó Edison előkészítésekor.

## <a name="next-steps"></a>Következő lépések
[Hello eszközök beszerzése][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md