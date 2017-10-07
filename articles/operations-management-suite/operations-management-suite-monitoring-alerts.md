---
title: "a Microsoft-termékek figyelési aaaAlert felügyeleti |} Microsoft Docs"
description: "Riasztás néhány problémát jelöl, amely a rendszergazda figyelmet igényel.  Ez a cikk ismerteti, hogyan riasztások létrehozása és kezelése a System Center Operations Manager (SCOM) és a Naplóelemzési hello különbségeit és hello két termékek egy hibrid riasztáskezelési stratégia kihasználva a bevált gyakorlat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 526a3d92f3b6a5d6dec2f3979a934cc94c13f2e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-alerts-with-microsoft-monitoring"></a>A Microsoft figyelési riasztások kezelése
Riasztás néhány problémát jelöl, amely a rendszergazda figyelmet igényel.  Nincsenek a köztük lévő különbségeket System Center Operations Manager (SCOM) és a Naplóelemzési Operations Management Suite (OMS) riasztások létrehozását, hogyan kezeli, és elemezni, és hogyan értesítés jelenik meg, hogy egy kritikus problémát észlelt.

## <a name="alerts-in-operations-manager"></a>A riasztásokat az Operations Manager programban
Scom riasztások akkor jönnek létre egyedi szabályok vagy figyelők tooindicate egy adott probléma.  A figyelő riasztást generálhat amikor hibaállapota közben olyan szabályt hozhat létre egy riasztási tooindicate néhány kritikus problémát, amely nem közvetlenül kapcsolódó toohello állapotát egy felügyelt objektum.  Felügyeleti csomagok a munkafolyamatok által létrehozott riasztások hello alkalmazás vagy szolgáltatás, amely az általuk kezelt számos tartalmaznak.  Hello folyamatán új felügyeleti csomag része, hogy túl sok riasztás a problémákra, amelyek nem a kritikus fontolja meg nem érkezik tooensure hangolása azt.

![SCOM riasztás nézet](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM teljes riasztáskezelési biztosít, amelyek módosíthatók a rendszergazdái tooresolve hello probléma munkánk állapotú riasztásokat.  Ha hello problémát sikerült megoldani, hello rendszergazda hello ekkor már megjelenik riasztási tooclosed nézetek aktív riasztások megjelenítése értékre állítja.  A figyelők által generált riasztások hello monitor visszatér megfelelő állapotba tooa automatikusan megoldhatók.

![Riasztási állapot](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>A Naplóelemzési riasztások
A Naplóelemzési riasztást a napló lekérdezésből, amely rendszeres időközönként automatikusan fut jön létre.  A napló lekérdezésből riasztási szabályt hozhat létre.  Hello lekérdezés, melyet hello feltételeknek megfelelő eredményeket ad vissza, ha riasztás jön létre.  Ennek oka lehet egy adott lekérdezés, amely riasztást hoz létre, ha egy adott esemény észlelhető, vagy használhat egy általános lekérdezést, hogy bármely hibaesemény megkeresi kapcsolódó tooa adott alkalmazásra.

Napló Analytics riasztások toohello OMS tárház eseményként vannak megírva, és napló lekérdezéssel lehet beolvasni.  Nem rendelkeznek egy állapot SCOM események például, hogy adhatja meg, amikor hello problémát sikerült megoldani.

![OMS riasztás](media/operations-management-suite-monitoring-alerts/oms-alert.png)

SCOM Naplóelemzési használja forrásként, ha SCOM-riasztások íródtak toohello OMS-tárház módon hozhatók létre és módosíthatók.  

![SCOM-riasztás](media/operations-management-suite-monitoring-alerts/scom-alert.png)

Hello [Riasztáskezelési megoldás](http://technet.microsoft.com/library/mt484092.aspx) tooretrieve más-más részhalmazához riasztások aktív riasztások, és számos gyakori lekérdezések összegzését tartalmazza.  Ez lehetővé teszi az scom jelentés mint a riasztások hatékonyabb elemzése.  Részletekbe menően tárhatják a hello összesítések toodetailed adatokból, és más-más részhalmazához tooretrieve riasztások alkalmi lekérdezések létrehozása.

![Riasztási felügyeleti megoldás](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Értesítések
Scom értesítéseket küldeni levelezőkiszolgáló vagy szöveget az adott feltételeknek megfelelő válasz tooalerts.  Létrehozhat, amelyek értesítést kap, attól függően, hogy a figyelt hello objektumként ilyen feltételek különböző személyek rendelkeznek, hello riasztást, hello típusú észlelt probléma vagy hello súlyossága hello másik értesítés-előfizetések nap időt.

Néhány előfizetések lehet használt tooimplement számos felügyeleti csomagok teljes értesítési stratégiáját.

![SCOM-riasztások](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Naplóelemzési értesítheti, levelezési keresztül, hogy létrejött-e riasztást úgy, hogy az e-mail értesítési műveletet minden egyes [riasztási szabály](http://technet.microsoft.com/library/mt614775.aspx).  Nincs hello képességét SCOM hello előfizetés toomultiple értesítések a kezelhető egyetlen szabállyal.  Szükség toocreate saját riasztási szabályok mivel OMS nem ad semmilyen előre konfigurált.

![Log Analytics-riasztások](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Ön nem teljesen SCOM riasztások kezelése a Naplóelemzési azonban csak akkor módosíthatja azokat az operatív konzol hello óta.  A Naplóelemzési akkor hasznos, egy riasztás kezelés feldolgozni, ha a tartozó elemzésére szolgáló eszközöket biztosít, hogy SCOM önmagában nem rendelkezik.

## <a name="alert-remediation"></a>Riasztási szervizelés
[Szervizelés](http://technet.microsoft.com/library/mt614775.aspx) tooan kísérlet tooautomatically megfelelő hello probléma riasztást által azonosított hivatkozik.

SCOM lehetővé teszi toorun diagnosztikai és helyreállítási műveletek válasz tooa figyelő állapota nem kifogástalan megadása.  Ez akkor fordul elő, egyidejű toohello figyelő hello riasztás létrehozása.  Diagnosztikai és helyreállítási műveletek általában valósul meg hello ügynökön futtatott parancsfájlból.  A diagnosztikai kísérletek toogather további információ a hello észlelt probléma a helyreállítási kísérletek toocorrect hello probléma.

A Naplóelemzési toostart lehetővé teszi egy [Azure Automation-runbook](https://azure.microsoft.com/documentation/services/automation/) , vagy hívja a webhook válasz tooa Naplóelemzési riasztásban.  Runbookok PowerShell megvalósított összetett logikát is tartalmazhat.  hello parancsfájl az Azure-ban fut, és elérhető bármely Azure-erőforrások vagy a külső erőforrás hello felhő.  Azure Automation szolgáltatásbeli hello képességét tooexecute runbookok rendelkezik egy kiszolgálót a helyi adatközpontban, de ez a funkció már nem érhető el, a válasz tooLog Analytics riasztások hello runbook indításakor.

Scom helyreállítások mind OMS runbookjai tartalmazhatnak PowerShell-parancsfájlok, de helyreállítások nehezebb toocreate és felügyelhetők, mert azok tartalmaznia kell egy felügyeleti csomagot.  A Runbookok szerzői, tesztelése és a runbookok kezelésére funkciókat biztosító Azure Automation vannak tárolva.

SCOM adatforrásként Naplóelemzési használja, ha létrehozhat egy SCOM riasztások tárolt hello OMS-tárház egy napló lekérdezés tooretrieve használatával Naplóelemzési riasztás.  Ez lehetővé teszi egy Azure Automation-runbook toorun válasz tooa SCOM riasztásban.  Természetesen hello runbook fog futni az Azure-ban, mert nem lenne a helyszíni problémák helyreállítások életképes stratégiáját.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hello részleteit [riasztások a System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).

