---
title: "az Azure Automation-forgatókönyv grafikus kezelése aaaError |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooimplement hibakezelési grafikus Azure Automation-runbook logikája."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
ms.openlocfilehash: b9ff01361d2ebd9c0174b074a7a290b1cc2fd1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Hibakezelés az Azure Automation grafikus runbookokban

A kulcs runbook tervezési egyszerű tooconsider azonosítja, hogy a runbook előfordulhat, hogy különböző problémákat. Az ilyen helyzetek közé tartoznak a sikeres műveletek, a várt hibaállapotok és a váratlan hibafeltételek.

A runbookoknak hibakezelési képességekkel is kell rendelkezniük. toovalidate tevékenység kimeneti hello, vagy hiba lép fel grafikus forgatókönyvek kezeléséhez, használhatja a Windows PowerShell tevékenységeket, feltételes logikához hello kimeneti hivatkozásra hello tevékenység megadása, vagy egy másik módszer alkalmazása.          

Gyakran Ha egy nem okozó hibát, amely runbook-tevékenység fellép, a következő tevékenység feldolgozása hello hiba függetlenül. hello hiba valószínűleg toogenerate kivételt, de a következő tevékenység hello továbbra is engedélyezett toorun. Ez az hello módon, hogy PowerShell tervezett toohandle hibák.    

PowerShell végrehajtása során előforduló hibák hello típusú leáll, vagy nem okozó. hello különbségei lezáró és nem okozó hibák a következők:

* **Leállítja a hiba**: súlyos hiba, amely teljesen leállítja hello parancsot (vagy parancsfájl végrehajtása) végrehajtása közben. Ilyenek például a nem létező parancsmagok, a parancsmag futását megakadályozó szintaktikai hibák vagy az egyéb végzetes hibák.

* **A nem okozó hibát**: nem súlyos hiba, amely lehetővé teszi a végrehajtását toocontinue annak ellenére, hogy hello hiba. Ilyenek például a műveleti hibák, például a „fájl nem található” vagy az engedélyekkel kapcsolatos problémák.

Azure Automation grafikus forgatókönyvek továbbfejlesztettük a hello funkció tooinclude hibakezelés. A kivételeket mostantól nem megszakító hibákká változtathatja, valamint hibahivatkozásokat hozhat létre a tevékenységek között. Ez a folyamat lehetővé teszi egy runbook Szerző toocatch hibákat, és felismert vagy váratlan feltételek kezelése.  

## <a name="when-toouse-error-handling"></a>Amikor toouse hibakezelés

Ha egy kritikus tevékenységet, amely egy hiba vagy kivételt jelez, akkor fontos tooprevent hello következő tevékenység a runbook a feldolgozási és toohandle hello hiba megfelelően. Ez kritikus fontosságú abban az esetben, ha a runbookok valamilyen üzleti vagy szolgáltatási műveleti folyamatot támogatnak.

Minden egyes tevékenységhez, amely hibát eredményezhet hello runbook Szerző egy mutató tooany más tevékenység hiba hivatkozást adhat hozzá.  hello céltevékenységre is lehet bármely típusú kód tevékenységek, például egy másik runbook hívja, a parancsmag meghívása, és így tovább.

Ezenkívül hello céltevékenységre is kimenő hivatkozásokat. Ezek lehetnek szokványos hivatkozások vagy hibahivatkozások. Ez azt jelenti, hogy hello runbook Szerző komplex hibakezelő logikai keresésére átrendezésével tooa nélkül is létrehozható tevékenység kód. hello ajánlott gyakorlat az toocreate közös funkcióinak dedikált hibakezelő runbookokhoz, de nincs kötelező. Egy PowerShell-kód nem tevékenység a hibakezelő logikai hello csak lehetőséget.  

Például fontolja meg a runbook toostart megpróbál egy virtuális Gépet, és telepíteni az alkalmazást. Hello virtuális gép nem indul el megfelelően, ha két műveleteket hajtja végre:

1. Értesítést küld a problémáról.
2. Elindít egy másik runbookot, amely automatikusan új virtuális gépet helyez üzembe.

Egy megoldás toohave tooan tevékenység leírók lépés egy mutató HIV hiba van. Például kapcsolódhat hello **Write-Warning** parancsmag tooan tevékenység lépés két, például a hello **Start-AzureRmAutomationRunbook** parancsmag.

Ez a viselkedés sok runbookok használatban volt is generalize, két tevékenységet tegyen egy külön hiba kezelési runbook és a korábbi javasolt következő hello útmutatást. Előtt hívja meg a hibakezelő runbookot, akkor képes hello adatokból hello eredeti runbook egyéni üzenetet létrehozni, és akkor továbbítja, hibakezelés runbook paraméter toohello.

## <a name="how-toouse-error-handling"></a>Hogyan toouse hibakezelés

Minden tevékenység rendelkezik a kivételeket nem megszakító hibákká módosító konfigurációval. Alapértelmezés szerint ez a beállítás le van tiltva. Azt javasoljuk, hogy engedélyezi-e ezt a beállítást, olyan tevékenységeket, ha azt szeretné, hogy toohandle hibák.  

Ha engedélyezi ezt a konfigurációt, lezáró mind a nem okozó hibák hello tevékenység nem okozó hibák kezeli, és hiba kapcsolattal rendelkező kezelhető vannak biztosítva.  

Miután ezt a beállítást, a kezelő hello hiba tevékenység létrehozása. Ha a tevékenység esetleges hibákat, majd hello hivatkozások követi kimenő hiba, és hello rendszeres hivatkozások nem, akkor is, ha hello tevékenység, valamint a rendszeres kimenetet hoz létre.<br><br> ![Példa egy Automation runbook hibahivatkozásra](media/automation-runbook-graphical-error-handling/error-link-example.png)

A következő példa hello, a runbook egy virtuális gép hello számítógép nevét tartalmazó változó kéri le. A következő tevékenység hello toostart hello virtuális gép majd újrapróbálkozik.<br><br> ![Példa egy Automation runbook hibakezelésére](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Hello **Get-AutomationVariable** tevékenység és **Start-AzureRmVm** konfigurált tooconvert kivételek tooerrors vannak.  Ha nincsenek hello kapják meg a változó és kezdő problémák hello virtuális gép, majd a hibák akkor jönnek létre.<br><br> ![Egy Automation runbook hibakezelési tevékenységének beállításai](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

A tevékenységek tooa egyetlen haladjanak hiba hivatkozások **hiba felügyeleti** tevékenység (kód tevékenység). Ez a tevékenység része hello használó egyszerű PowerShell-kifejezést *Throw* feldolgozás, jelszavat kulcsszó toostop *$Error.Exception.Message* tooget hello hello leíró üzenet aktuális kivétel.<br><br> ![Példa egy Automation runbook hibakezelési kódra](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Következő lépések

* További információ a hivatkozások és a grafikus forgatókönyvek típusú hivatkozás toolearn lásd [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md#links-and-workflow).

* További információk a runbook végrehajtása toomonitor runbook feladatokat, valamint egyéb technikai részleteket lásd: hogyan toolearn [nyomon követheti a runbook-feladatok](automation-runbook-execution.md).
