---
title: "aaaStorSimple 8000 sorozat hardverösszetevők és állapot |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor hello hardverösszetevők a StorSimple eszköz hello StorSimple Device Manager szolgáltatáson keresztül."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 85b398e4b1a6b8921792b8945331325940082eb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomonitor-hardware-components-and-status"></a>Hello StorSimple Device Manager szolgáltatás toomonitor hardverösszetevők és állapota
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a hello a helyszíni StorSimple 8000 series eszköz különböző fizikai és logikai összetevőnél. Azt is bemutatja, hogyan toomonitor hello hello segítségével eszköz összetevő-állapot **állapotát és a hardver állapotának** hello StorSimple Device Manager szolgáltatás paneljén.

Hello **állapotát és a hardver állapotának** panel hello StorSimple eszköz összetevők hello hardver állapotát jeleníti meg.

A hello összetevőlistát a 8100 nincsenek három szakaszait:

* **A megosztott összetevőit** – ezek részei hello vezérlők, például a lemezmeghajtók, ház, PCM összetevők PCM hőmérséklet feszültségérzékelő. sor, és aktuális érzékelők sor.
* **A vezérlő 0 összetevők** – hello összetevők vezérlő 0, például a tartományvezérlő, SAS bővítő és összekötőt, a tartományvezérlő hőmérséklet-érzékelő és hello különböző hálózati illesztőt.
* **1. vezérlő összetevők** – hello, tartományvezérlői 1, a vezérlő 0 részletes hasonló toothose alkotó összetevők.

Egy 8600 eszköznek további összetevők, amelyek megfelelnek a toohello kiterjesztett álló, lemezcsoport a lemezek (EBOD) szolgáltatással. Hello összetevők listáját, a szakaszok is vannak öt. Ezek a szakaszok is vannak három elsődleges szolgáltatással hello hello összetevőket tartalmaznak, amelyek azonos toohello ismertetettekhez a 8100. Nincsenek két további szakaszaiban a hello EBOD ház, amelyek ismertetik:

* **EBOD vezérlő 0 összetevők** – hello összetevők, amelyek megtalálhatók a EBOD ház 0, például a hello EBOD vezérlő, SAS bővítő összekötő és a tartományvezérlő hőmérséklet-érzékelő.
* **EBOD vezérlő 1 összetevők** – hello EBOD ház 1, alkotó összetevők hasonló toothose részletes a EBOD ház 0.
* **EBOD ház megosztott összetevők** – hello összetevők hello EBOD ház és hello EBOD vezérlő részét nem képező PCM szerepelnek.

> [!NOTE]
> **hello hardverállapot a StorSimple felhő alapplatformjaként (8010-es/8020-as modell) nem érhető el.**


## <a name="monitor-hello-hardware-status"></a>Hello hardver állapotának figyelése
Hajtsa végre a következő lépéseket tooview hello hardver állapota az eszköz összetevő hello:

1. Keresse meg a túl**eszközök**, jelöljön ki egy adott StorSimple eszközt. Nyissa meg túl**figyelő > hardver állapotának**.

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health1.png)

2. Keresse meg a hello **hardverösszetevők** szakaszt, és hello elérhető összetevői közül választhat. Egyszerűen kattintson hello összetevő címke tooexpand hello listára, és hello hello állapotának megjelenítése összetevők eszköz. Lásd: hello [hello elsődleges ház részletes összetevő listája](#component-list-for-primary-enclosure-of-storsimple-device) és hello [hello EBOD ház részletes összetevő listája](#component-list-for-ebod-enclosure-of-storsimple-device).

    ![](./media/storsimple-8000-monitor-hardware-status/hw-health2.png)

3. A következő szín kódolási sémát toointerpret hello összetevő-állapot hello használata:
   
   * **Jelölőnégyzet zöld** – jelöli a megfelelő összetevő **OK** állapotát.
   * **Sárga** – azt jelzi, egy csökkent összetevő **figyelmeztetés** állapotát.
   * **Piros felkiáltójel** – Denotes egy sikertelen összetevő, amely rendelkezik egy **hiba** állapotát.
   * **A fekete szöveg fehér** – azt jelzi, amely nem található.
   
   hello következő képernyőfelvétel egy-az összetevők eszköz **OK**, **figyelmeztetés**, és **hiba** állapotát.
       
   ![](./media/storsimple-8000-monitor-hardware-status/hw-health3.png)

   Növekvő hello **megosztott összetevők listájában**, láthatja, hogy hello NVRAM és hello fürt állapotromlást.

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health5.png)

   Növekvő hello **vezérlő 1 összetevők** lista, láthatja, hogy a fürtcsomópont hello sikertelen volt.  

   ![](./media/storsimple-8000-monitor-hardware-status/hw-health4.png)  

4. Ha egy összetevő, amely nem része egy **kifogástalan** állapot, forduljon a Microsoft Support. Ha a riasztás az eszköz engedélyezve van, az e-mail riasztások fog kapni. Ha egy meghibásodott hardverelem tooreplace van szüksége, tekintse meg [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).

## <a name="component-list-for-primary-enclosure-of-storsimple-device"></a>A StorSimple eszköz elsődleges ház összetevő listája
hello következő tábla vázol fel hello hello elsődleges ház (jelenlegi, mind a 8100 és 8600) a helyszíni StorSimple eszköz található fizikai és logikai összetevőket.

| Összetevő | Modul | Típus | Hely | A mező cserélhető Cisco egységet (FRU)? | Leírás |
| --- | --- | --- | --- | --- | --- |
| [0 – 11] tárolóhelyen lévő meghajtó |Lemezmeghajtó |Fizikai |Közös |Igen |Egy sor számára jelenik meg az egyes hello SSD, vagy hello HDD meghajtókat hello elsődleges szolgáltatással. |
| Környezeti hőmérséklet-érzékelő |Ház |Fizikai |Közös |Nem |Intézkedések hello hello váz hőmérsékletet. |
| Közepes vezérlősík hőmérséklet-érzékelő |Ház |Fizikai |Közös |Nem |Intézkedések hello hello közepes vezérlősík hőmérséklete. |
| Hallható |Ház |Fizikai |Közös |Nem |Azt jelzi, hogy hello hallható alrendszer belül hello váz működési. |
| Ház |Ház |Fizikai |Közös |Igen |Eszközök befogadására szolgáló vázat hello jelenlétét jelzi. |
| Ház beállításai |Ház |Fizikai |Közös |Nem |Hello váz toohello előlapja hivatkozik. |
| Sor feszültség-érzékelő |PCM |Fizikai |Közös |Nem |Számos sor feszültség-érzékelő állapotukra jelenik meg, amely azt jelzi, hogy hello mért feszültség tűréshatáron belül van. |
| Sor aktuális érzékelők |PCM |Fizikai |Közös |Nem |Számos sor aktuális érzékelők állapotukra jelenik meg, amely azt jelzi, hogy hello mért aktuális tűréshatáron belül van. |
| A PCM hőmérséklet-érzékelő |PCM |Fizikai |Közös |Nem |Tűréshatáron belül például a bemeneti és interaktív terület érzékelők állapotukra jelenik meg, amely azt jelzi, hogy hello mért hőmérséklet van számos hőmérséklet-érzékelő. |
| [0-1]. tápegység |PCM |Fizikai |Közös |Igen |Egy sor két PCMs hello hello eszköz oldalán található bemutatják az egyes hello áramforrással rendelkezik a hello. |
| Hűtési [0-1] |PCM |Fizikai |Közös |Igen |Egy sor élő hello két PCMs négy hűtőventilátorok bemutatják az egyes hello. |
| Akkumulátor [0-1] |PCM |Fizikai |Közös |Igen |Egy sor minden rendszer illeszkedik az hello PCM hello biztonsági mentési akkumulátor modulok áll rendelkezésre. |
| Metis |N/A |Logikai |Közös |N/A |Hello akkumulátorok hello állapotát jeleníti meg: e azok díjszabási kell, és vannak eléri az élettartam végi. |
| Fürt |N/A |Logikai |Közös |N/A |Megjeleníti a hello fürtökön létrehozott hello két integrált vezérlő modulok között állapotának hello. |
| Fürtcsomópont |N/A |Logikai |Közös |N/A |Hello vezérlő hello fürt részeként hello állapotát jelzi. |
| A fürtkvórum |N/A |Logikai | |N/A |Hello többsége lemez tagság hello HDD tárolókészlet hello jelenlétét jelzi. |
| HDD adatterülethez |N/A |Logikai |Közös |N/A |a hello merevlemez-meghajtó (HDD) tárolókészletben használt hello tárolóhely. |
| HDD felügyeleti hely |N/A |Logikai |Közös |N/A |hello felügyeleti feladatok HDD a tárolókészletben lefoglalt hello terület. |
| HDD kvórum terület |N/A |Logikai |Közös |N/A |hello fürtkvórum HDD a tárolókészletben lefoglalt hello terület. |
| HDD helyettesítő terület |N/A |Logikai |Közös |N/A |hello vezérlő helyettesítő HDD a tárolókészletben lefoglalt hello terület. |
| SSD-adatterülethez |N/A |Logikai |Közös |N/A |a hello tartós állapotú meghajtót (SSD) tárolókészletben használt hello tárolóhely. |
| SSD NVRAM terület |N/A |Logikai |Közös |N/A |hello hello az NVRAM logika külön SSD a tárolókészletben található tárolóhely. |
| HDD-tárolókészletben |N/A |Logikai |Közös |N/A |Megjeleníti az eszközről HDD létrehozott hello logikai tárolókészlet állapotának hello. |
| A tárolókészlet SSD |N/A |Logikai |Közös |N/A |Megjeleníti az SSD-k eszközről létrehozott hello logikai tárolókészlet állapotának hello. |
| A vezérlő [0-1] [-state] |I/O |Fizikai |Tartományvezérlő |Igen |Hello vezérlő hello állapotát jeleníti meg, és hogy belül hello váz aktív vagy készenléti üzemmódban van. |
| A vezérlő hőmérséklet-érzékelő |I/O |Fizikai |Tartományvezérlő |Nem |I/o-modul, CPU hőmérséklet, DIMM és PCIe érzékelők például számos hőmérséklet-érzékelő állapotukra jelenik meg, amely azt jelzi, hogy van-e észlelt hello hőmérséklet tűréshatáron belül van. |
| SAS bővítő |I/O |Fizikai |Tartományvezérlő |Nem |Hello soros csatlakoztatott SCSI (SAS) bővítő, amely használt tooconnect hello integrált toohello tárolóvezérlő hello állapotát jelzi. |
| SAS-összekötő [0-1] |I/O |Fizikai |Tartományvezérlő |Nem |Minden egyes SAS-összekötő, amely integrált használt tooconnect tárolási toohello SAS bővítő hello állapotát jelzi. |
| SBB közepes vezérlősík memóriamodulok közötti |I/O |Fizikai |Tartományvezérlő |Nem |Hello közepes vezérlősík összekötő használt tooconnect hello állapotát minden egyes tartományvezérlő toohello közepes vezérlősík jelzi. |
| Processzormag |I/O |Fizikai |Tartományvezérlő |Nem |Hello Processzormagok belül minden vezérlő hello állapotát jelzi. |
| Ház electronics power |I/O |Fizikai |Tartományvezérlő |Nem |Hello ház használt hello power rendszert hello állapotát jelzi. |
| Ház electronics diagnosztika |I/O |Fizikai |Tartományvezérlő |Nem |Hello diagnosztika alrendszereket hello vezérlő által biztosított hello állapotát jelzi. |
| Alaplapi felügyeleti vezérlővel (BMC) |I/O |Fizikai |Tartományvezérlő |Nem |Hello alaplapi felügyeleti vezérlővel (BMC), amely egy speciális szolgáltatás processzor hello hardvereszköz figyeli az érzékelők keresztül, és hello rendszergazda független kapcsolaton keresztül kommunikál hello állapotát jelzi. |
| Ethernet |I/O |Fizikai |Tartományvezérlő |Nem |Az egyes hello hálózati felületek, ez azt jelenti, hogy hello felügyeleti és hello vezérlő megadott adatok portok hello állapotát jelzi. |
| NVRAM |I/O |Fizikai |Tartományvezérlő |Nem |NVRAM, a biztonsági mentést készített hello akkumulátor hello esemény áramszünet esetén az alkalmazáshoz kritikus információk tooretain ellátó nem felejtő közvetlen elérésű memória hello állapotát jelzi. |

## <a name="component-list-for-ebod-enclosure-of-storsimple-device"></a>A StorSimple eszköz EBOD ház összetevő listája
hello következő tábla vázol fel hello hello a helyszíni StorSimple eszköz (csak szerepel 8600 modellben) EBOD ház található fizikai és logikai összetevőket.

| Összetevő | Modul | Típus | Hely | FRU? | Leírás |
| --- | --- | --- | --- | --- | --- |
| [0 – 11] tárolóhelyen lévő meghajtó |Lemezmeghajtó |Fizikai |Közös |Igen |Egy sor minden HDD meghajtókat a hello EBOD ház hello eleje hello áll rendelkezésre. |
| Környezeti hőmérséklet-érzékelő |Ház |Fizikai |Közös |Nem |Intézkedések hello hello váz hőmérsékletet. |
| Közepes vezérlősík hőmérséklet-érzékelő |Ház |Fizikai |Közös |Nem |Intézkedések hello hello közepes vezérlősík hőmérséklete. |
| Hallható |Ház |Fizikai |Közös |Nem |Azt jelzi, hogy hello hallható alrendszer belül hello váz működési. |
| Ház |Ház |Fizikai |Közös |Igen |Eszközök befogadására szolgáló vázat hello jelenlétét jelzi. |
| Ház beállításai |Ház |Fizikai |Közös |Nem |Toohello OPS vagy hello előlapja hello váz hivatkozik. |
| Sor feszültség-érzékelő |PCM |Fizikai |Közös |Nem |Számos sor feszültség-érzékelő állapotukra jelenik meg, amely azt jelzi, hogy hello mért feszültség tűréshatáron belül van. |
| Sor aktuális érzékelők |PCM |Fizikai |Közös |Nem |Számos sor aktuális érzékelők állapotukra jelenik meg, amely azt jelzi, hogy hello mért aktuális tűréshatáron belül van. |
| A PCM hőmérséklet-érzékelő |PCM |Fizikai |Közös |Nem |Tűréshatáron belül például a bemeneti és interaktív terület érzékelők állapotukra jelenik meg, amely azt jelzi, hogy hello mért hőmérséklet van számos hőmérséklet-érzékelő. |
| [0-1]. tápegység |PCM |Fizikai |Közös |Igen |Egy sor két PCMs hello hello eszköz oldalán található bemutatják az egyes hello áramforrással rendelkezik a hello. |
| Hűtési [0-1] |PCM |Fizikai |Közös |Igen |Egy sor élő hello két PCMs négy hűtőventilátorok bemutatják az egyes hello. |
| Helyi tároló [HDD] |N/A |Logikai |Közös |N/A |Megjeleníti az eszközről HDD létrehozott hello logikai tárolókészlet állapotának hello. |
| A vezérlő [0-1] [-state] |I/O |Fizikai |Tartományvezérlő |Igen |Megjeleníti a hello EBOD modul hello tartományvezérlőinek állapotának hello. |
| A EBOD hőmérséklet-érzékelő |I/O |Fizikai |Tartományvezérlő |Nem |Minden tartományvezérlőről számos hőmérséklet-érzékelő állapotukra jelenik meg, amely azt jelzi, hogy észlelt hello hőmérséklet tűréshatáron belül van. |
| SAS bővítő |I/O |Fizikai |Tartományvezérlő |Nem |Hello SAS bővítő használt tooconnect hello integrált toohello tárolóvezérlő hello állapotát jelzi. |
| SAS-összekötő [0-2] |I/O |Fizikai |Tartományvezérlő |Nem |Minden egyes SAS-összekötő, amely integrált használt tooconnect tárolási toohello SAS bővítő hello állapotát jelzi. |
| SBB közepes vezérlősík memóriamodulok közötti |I/O |Fizikai |Tartományvezérlő |Nem |Hello közepes vezérlősík összekötő használt tooconnect hello állapotát minden egyes tartományvezérlő toohello közepes vezérlősík jelzi. |
| Ház electronics power |I/O |Fizikai |Tartományvezérlő |Nem |Hello ház használt hello power rendszert hello állapotát jelzi. |
| Ház electronics diagnosztika |I/O |Fizikai |Tartományvezérlő |Nem |Hello diagnosztika alrendszereket hello vezérlő által biztosított hello állapotát jelzi. |
| Kapcsolat toodevice vezérlő |I/O |Fizikai |Tartományvezérlő |Nem |Hello kapcsolatot hello EBOD i/o-modul és a hello eszköz vezérlő hello állapotát jelzi. |

## <a name="next-steps"></a>Következő lépések
* toouse hello StorSimple Device Manager szolgáltatás tooadminister az eszközt, lépjen túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).
* Ha egy eszköz összetevő, amely a csökkentett teljesítményű vagy sikertelen állapotához tootroubleshoot van szüksége, tekintse meg a túl[ellenőrzési mutatók StorSimple](storsimple-monitoring-indicators.md).
* tooreplace sikertelen hardverösszetevő, lásd: [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).
* Ha Ön most folytatja, tooexperience eszközökkel kapcsolatos problémákat, [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md).

