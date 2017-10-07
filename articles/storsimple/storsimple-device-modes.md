---
title: "aaaChange hello StorSimple eszköz mód |} Microsoft Docs"
description: "Hello StorSimple eszköz módokat ismerteti és bemutatja hogyan toouse Windows PowerShell a StorSimple toochange hello eszköz mód."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 299fd380a83bcd06780c97937f4064f0791b440d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a>A StorSimple eszköz hello eszköz üzemmód módosítása
Ez a cikk ismerteti, rövid hello különböző módokat, amelyben a StorSimple eszköz működhet. A StorSimple eszköz működhet három üzemmódban: normál, karbantartási és helyreállítás. 

A cikk elolvasása után tudni fogja:

* Milyen hello StorSimple eszköz módok a következők:
* Hogyan toofigure kimenő üzemmódjának hello StorSimple-eszköz van
* Hogyan normál toomaintenance üzemmódról toochange és *ez fordítva is igaz*

felügyeleti feladatok felett hello csak a StorSimple eszköz hello Windows PowerShell felületén keresztül végezhető el.

## <a name="about-storsimple-device-modes"></a>A StorSimple eszköz módokkal kapcsolatos
A StorSimple eszköz normál, karbantartási vagy helyreállítási módban is működik. Módokban az alábbiakban röviden bemutatjuk.

### <a name="normal-mode"></a>Normál mód
Ez normál működési mód hello teljesen konfigurált a StorSimple eszköz típusúként van definiálva. Alapértelmezés szerint az eszköz normál üzemmódban kell lennie.

### <a name="maintenance-mode"></a>A karbantartási mód
Néha hello StorSimple-eszköz szükség toobe karbantartási módba helyezve. Ez a mód lehetővé teszi hello eszközön tooperform karbantartási és zavaró frissítések telepítése, például azok kapcsolatos toodisk belső vezérlőprogram.

A StorSimple helyezhetik hello rendszer csak a Windows PowerShell hello keresztül karbantartási módba. Az összes i/o-kérelmek fel van függesztve, ebben a módban. Például nem felejtő közvetlen elérésű memória (NVRAM) vagy a fürtszolgáltatás hello szolgáltatás is le van állítva. Mindkét hello tartományvezérlők adja meg, vagy lépjen ki az ebben a módban újraindul. Hello karbantartási módból való kilépéskor minden hello szolgáltatás folytatódik, és megfelelő kell lennie. Ez eltarthat néhány percig.

> [!NOTE]
> **A karbantartási mód csak egy megfelelően működő eszközön támogatott. Nem támogatott egy eszközön, amely az egyik vagy mindkét hello, tartományvezérlői nem megfelelően működnek.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Helyreállítási módban
Helyreállítási módban, "A Windows csökkentett módban hálózati támogatás" jelentheti. Helyreállítási módban hello Microsoft Support csoport végez, és lehetővé teszi, hogy azok tooperform diagnosztika hello rendszeren. hello elsődleges helyreállítási módban célja tooretrieve hello rendszer naplóit.

Ha a rendszer helyreállítási módba kerül, a következő lépéshez lépjen kapcsolatba a Microsoft Support. További információ: túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Hello eszköz helyreállítási módban nem helyezhető el. Hello eszköz rossz állapotban van, ha a helyreállítási módban tooget hello eszköz megpróbál olyan állapotba, amelyben Microsoft Support szakembereinek ellenőrizheti azt.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Határozza meg a StorSimple eszköz mód
#### <a name="toodetermine-hello-current-device-mode"></a>toodetermine hello aktuális eszköz mód
1. Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Nézze meg a szalagcím üdvözlőüzenetére hello hello eszköz soros konzoljához menüjében. Ez az üzenet explicit módon azt jelzi, hogy hello eszköz karbantartás vagy helyreállítási módban. Hello üzenet nem tartalmaz semmilyen konkrét információkat toohello rendszer mód, ha hello eszköz normál módban van.

## <a name="change-hello-storsimple-device-mode"></a>Hello StorSimple eszköz módjának módosítása
Hello StorSimple eszköz helyezzen karbantartási üzemmódról (normál) tooperform karbantartási, vagy telepítse a karbantartási mód frissítéseket. Hajtsa végre a következő eljárások tooenter vagy kilépés a karbantartási mód hello.

> [!IMPORTANT]
> Mielőtt karbantartási módba, ellenőrizze, hogy mindkét eszközvezérlők kifogástalan hello elérésével **hardverállapot** a hello **karbantartási** hello klasszikus Azure portálra a lap. Ha az egyik vagy mindkét hello tartományvezérlők nem működik megfelelően, forduljon a Microsoft Support hello további lépéseket. További információ: túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="tooenter-maintenance-mode"></a>tooenter a karbantartási mód
1. Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Amikor a rendszer kéri, adja meg a hello **eszköz rendszergazdai jelszava**. hello alapértelmezett jelszava: `Password1`.
3. Írja be a parancssorba hello 
   
    `Enter-HcsMaintenanceMode`
4. Látni fogja a tájékoztatással, hogy a karbantartási mód figyelmeztető üzenetet fog megzavarhatják összes i/o-kérelmek és sever hello kapcsolat toohello a klasszikus Azure portálon, és megerősítés kéri. Típus **Y** tooenter karbantartási módban.
5. Mindkét tartományvezérlők újraindul. Ha hello újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy hello eszköz karbantartási módban.

#### <a name="tooexit-maintenance-mode"></a>tooexit a karbantartási mód
1. Jelentkezzen be toohello eszköz soros konzoljához. Ellenőrizze az üzenetből hello szalagcím, amely az eszköz karbantartási módban van.
2. Hello parancsot, írja be a parancssorba:
   
    `Exit-HcsMaintenanceMode`
3. Ekkor megjelenik egy figyelmeztető üzenet, és egy megerősítő üzenetet. Típus **Y** tooexit karbantartási módban.
4. Mindkét tartományvezérlők újraindul. Ha hello újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy hello eszköz normál üzemmódban.

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan túl[normál és a karbantartási mód frissítések alkalmazása](storsimple-update-device.md) a StorSimple eszköz.

