---
title: "aaaStorSimple 8000 Series Update 4 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 4."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 4bca8ca222e6706fd6eaf56b702b0d34b6ffd1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>A StorSimple 8000 Series Update 4 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés

hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 4 hello kritikus megnyitott problémák azonosításához. Ebben a kiadásban szereplő hello StorSimple szoftverfrissítések listáját is tartalmaznak. 

Update 4 alkalmazott tooany StorSimple eszköz kiadásban (GA) vagy a frissítés 0,1 futó frissítés 3.1 keresztül lehet. hello eszköz Update 4 társított verziója 6.3.9600.17820.

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.

> [!IMPORTANT]
> * Update 4 eszköz szoftver, a legpontosabb Beállításhoz belső vezérlőprogram, LSI illesztőprogram és belső vezérlőprogramjának, lemez belső vezérlőprogram, Storport és Spaceport, biztonsági és más operációs rendszer frissítése érdekében rendelkezik. Megközelítőleg 4 óra tooinstall veszi ezt a frissítést. Vezérlőprogram-frissítés zavaró frissítésről, és az eszköz egy leállásának eredményez. Azt javasoljuk, hogy telepítenie Update 4 tookeep az eszköz naprakész. 
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. Néhány napot várni, és a frissítések újra ezeket a majd vizsgálatát hamarosan elérhető lesz.

## <a name="whats-new-in-update-4"></a>What's new in Update 4

hello következő fontos fejlesztést tartalmaz, és hibajavítások végzett frissítés 4.

* **Azure ad automatikus terület-visszanyerést algoritmusok** – 4. frissítés, hello automatizált terület-visszanyerést algoritmusok a következők továbbfejlesztett tooadjust hello terület-visszanyerést alapján várt hello ciklusok visszaigényelt hello felhőben rendelkezésre álló terület. 

* **A helyileg rögzített kötetekhez a teljesítményt javító** – Update 4 forgatókönyvekben, amelyek magas adatfeldolgozást (adatok összehasonlítható toovolume méret) helyileg rögzített kötetekhez hello teljesítménye javult.

* **Heatmap-alapú visszaállítási** – hello korábban a kiadások, a vész-helyreállítási, hello adatokat a következő vissza lett állítva, ami azt eredményezi, a lassú teljesítmény hello hozzáférési minták alapján hello felhőből. 

    Egy új szolgáltatást tartalmazza az Update 4, hogy nyomon követi a gyakran használt adatok toocreate egy heatmap, amikor hello eszköz van használatban előzetes tooDR (az adattömbök leggyakrabban használt rendelkezik nagy kisebb adattömbök szolgálja, míg az alacsony tűz rendelkezik). DR, miután StorSimple használ heatmap tooautomatically visszaállítási hello, és rehydrate hello felhő hello adatait. 

    Minden hello visszaállítások alapú heatmap visszaállítások áll. További információ a hogyan tooquery és Mégse heatmap alapú visszaállítási és rehidratálása feladatok: túl[StorSimple parancsmag-referencia a Windows PowerShell](https://technet.microsoft.com/library/dn688168.aspx).

* **StorSimple diagnosztikai eszköz** – az Update 4, amely könnyen diagnosztizálására tooallow egy StorSimple diagnosztikai eszköz folyamatban van, és a problémák hibaelhárításához kapcsolódó toosystem, a hálózati, a teljesítmény és a hardver összetevő állapotát. Ezt az eszközt a StorSimple fut hello Windows PowerShell segítségével. További információ: túl[StorSimple diagnosztikai eszköz használata – hibaelhárítás](storsimple-8000-diagnostics.md).

* **Felhasználóifelület-alapú StorSimple áttelepítési eszköz** -toothis előzetes kiadás, az áttelepítést 5000-7000-es sorozatból szükséges hello felhasználók tooexecute hello Azure PowerShell felületéről hello áttelepítési munkafolyamat egy részét. Ebben a kiadásban egy könnyen használható Felhasználóifelület-alapú StorSimple áttelepítési eszköz felhasználhatóvá válik támogatási toofacilitate hello áttelepítési ugyanabban a munkafolyamatban. Ez az eszköz a helyreállítási gyűjtők hello összevonása lehetővé. 

* **FIPS-kapcsolatos módosítások** - e és újabb verziók esetében a kiadási, FIPS összes hello StorSimple 8000 sorozat eszközeire alapértelmezés szerint engedélyezve van, az mindkét hello a Microsoft Azure Government és az Azure nyilvános felhőjében fiókokat.

* **Módosításainak frissítésére** – ebben a kiadásban hibák kapcsolódó tooupdate hibák elhárítása.

* **Riasztás a lemezhibák** -egy új riasztás, amelyek figyelmeztetik a közelgő lemezhiba hello felhasználója jelenik meg ebben a kiadásban. Ha ezt a riasztást, lépjen kapcsolatba a Microsoft Support tooship a cserelemezt. További információ: túl[hardver riasztások a StorSimple eszköz](storsimple-manage-alerts.md#hardware-alerts).

* **Tartományvezérlő helyettesítő módosítások** -parancsmag, amely lehetővé teszi a hello felhasználói tooquery hello állapotának hello, tartományvezérlői cseréjét egészül ki ebben a kiadásban. További információért tekintse meg a toohello [parancsmag tooquery tartományvezérlő helyettesítő állapotát](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Az Update 4 megoldott problémák

hello következő táblázat az összefoglalást tartalmazza a problémákat, amelyek a frissítés 4 javítva lett.    

| Nem | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Feladatátvétel |Hello a korábbi kiadás hello a feladatátvételt követően, hogy egy probléma volt kapcsolatos, toocleanup megfigyelt hello ügyfél telephelyén. Ez a probléma fennáll ebben a kiadásban. |Igen |Igen |
| 2 |Helyileg rögzített kötetek |Hello a korábbi verzióban egy probléma toorelated kötet létrehozása a helyileg rögzített kötetekhez, amely kötet létrehozása sikertelen volt. Ez a probléma volt legfelső szintű okozott, és ebben a kiadásban rögzített. |Igen |Nem |
| 3 |Támogatási csomag |Az előző kiadása nem voltak problémák kapcsolódó tooSupport csomag, amely egy System.OutOfMemory kivétel vagy más hibák, és egy támogatási csomag létrehozását hibát okozott. Ezek a hibák ebben a kiadásban rögzítettek. |Igen |Igen |
| 4 |Figyelés |Korábbi változatban nincs probléma kapcsolódó a toomonitoring diagramok a helyileg rögzített kötetekhez ahol fogyasztás látható volt EB. Ez a hiba megoldását az ebben a kiadásban. |Igen |Igen |
| 5 |Migrálás |Az előző kiadása nem voltak 5000-7000-es adatsorozat too8000 sorozat eszközeire való áttelepítést több problémák kapcsolódó toohello megbízhatóságát. Ebben a kiadásban rendelkezik foglalkoztak. ezeket a problémákat. |Igen |Igen |
| 6 |Frissítés |A korábbi kiadásokban, hiba történt a frissítés, hello, tartományvezérlői helyreállítási módba kerülnek, és ezért hello felhasználói hello frissítés nem folytatható toocontact Microsoft Support lenne szükség. <br> Ez a viselkedés ebben a kiadásban megváltozott. Ha hello felhasználó frissítés hibát követően mindkét hello tartományvezérlőkön futó hello azonos 4-es verzióját (Update), hello, tartományvezérlői ne lépje helyreállítási módba. Ha hello felhasználói ezt a hibát észlel, azt javasoljuk, hogy azok egy kis türelmet, és ismételje meg az hello frissítés. hello újrapróbálkozási telepítés sikeres lehet. Ha hello ismét sikertelen, akkor kell forduljon a Microsoft Support. |Igen |Igen |


## <a name="known-issues-in-update-4-from-previous-releases"></a>A korábbi kiadásokból származó Update 4 kapcsolatos ismert problémák

Nincsenek új ismert problémák a frissítési 4. A korábbi kiadásokból származó 4 tooUpdate átvitt problémák listáját, a go túl[Update 3 kibocsátási megjegyzések](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Soros csatlakozású SCSI (SAS) vezérlő és a belső vezérlőprogram frissítések Update 4

Ebben a kiadásban a vezérlő és LSI illesztőprogram és a belső vezérlőprogram frissítések rendelkezik. További információ a hogyan tooinstall ezeket a frissítéseket: [Update 4 telepítése](storsimple-install-update-4.md) a StorSimple eszköz.

## <a name="virtual-device-updates-in-update-4"></a>Az Update 4 virtuális eszköz frissítése

A frissítés alkalmazott toohello StorSimple felhő készülék (más néven hello virtuális eszköz) nem lehet. Új virtuális eszközök létrehozott toobe kell. 

## <a name="next-step"></a>Következő lépés

Ismerje meg, hogyan túl[Update 4 telepítése](storsimple-install-update-4.md) a StorSimple eszköz.

