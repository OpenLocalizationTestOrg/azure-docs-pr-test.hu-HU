---
title: "az Azure Automationben aaaRunbook végrehajtási |} Microsoft Docs"
description: "Azure Automation forgatókönyv feldolgozásának módja hello részleteit ismerteti."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d10c8ce2-2c0b-4ea7-ba3c-d20e09b2c9ca
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: bwren
ms.openlocfilehash: bdb535675443353d44640bc7773de3f9dac5e42c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-execution-in-azure-automation"></a>A Runbook végrehajtása az Azure Automationben
Az Azure Automationben elindít egy runbookot, ha egy feladat jön létre. Egy feladat a runbook egyszeri futtatási példánya. Egy Azure Automation dolgozó van hozzárendelve toorun minden feladatot. Munkavállalók több Azure-fiókra által megosztott, amíg feladatokat azok másik Automation-fiók el különítve egymástól. Melyik worker szolgáltatások hello kérelem szabályozhatják a feladat nem rendelkeznek.  Egyetlen runbook fut egyszerre több feladattal rendelkezhet. Amikor runbookokat hello listája hello Azure-portálon, hello állapotának indított minden runbook feladatokat sorolja fel. Minden runbook feladatok hello listája rendelés tootrack hello állapota az egyes tekintheti meg. Egy másik feladatállapotok hello ismertetését lásd: [feladatállapotok](#job-statuses).

hello következő diagramon láthatók a runbook-feladatok hello életciklusát [grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) és [PowerShell munkafolyamat-forgatókönyvekről](automation-runbook-types.md#powershell-workflow-runbooks).

![Feladatállapotok - PowerShell munkafolyamat](./media/automation-runbook-execution/job-statuses.png)

hello következő diagramon láthatók a runbook-feladatok hello életciklusát [PowerShell-forgatókönyvek](automation-runbook-types.md#powershell-runbooks).

![Feladatállapotok - PowerShell-parancsfájl](./media/automation-runbook-execution/job-statuses-script.png)

A feladatok rendelkezik hozzáférési tooyour Azure erőforrások azáltal, hogy a kapcsolat tooyour Azure-előfizetés. Csak rendelkeznek hozzáféréssel tooresources az Adatközpont hello nyilvános felhőből elérhető erőforrások esetén.

## <a name="job-statuses"></a>Feladatállapotok
hello következő táblázatban hello különböző állapotok lehetségesek feladat.

| status | Leírás |
|:--- |:--- |
| Befejeződött |hello feladat sikeresen befejeződött. |
| Nem sikerült |A [grafikus és a PowerShell-munkafolyamati forgatókönyvek](automation-runbook-types.md), hello runbook toocompile nem sikerült.  A [PowerShell-parancsfájl runbookok](automation-runbook-types.md), hello runbookja sikertelen toostart vagy hello feladat kivételbe ütközött. |
| Nem sikerült, erőforrás Várakozás |hello feladat végrehajtása nem sikerült, mert elérte hello [igazságos elosztása révén](#fairshare) háromszor korlátjának növelését, és indította el ugyanazon ellenőrzőpont hello, vagy a hello start hello runbook minden alkalommal, amikor. |
| A várólistára |hello feladat vár erőforrások egy automatizálási feldolgozó toocome érhető el, hogy indíthatók el. |
| Indulás alatt |hello feladat tooa munkavégző lett rendelve, és hello rendszer elindításával hello folyamatán. |
| Folytatása |hello rendszer hello folyamatának hello feladat folytatása után fel lett függesztve. |
| Fut |hello feladat fut. |
| Rendszert futtató erőforrások vár |hello feladat lett távolítva a memóriából, mert elérte a hello [igazságos elosztása révén](#fairshare) korlátot. Röviddel a legutóbbi ellenőrzőponttól folytatja. |
| Leállítva |hello feladatot leállította hello felhasználó, mielőtt befejeződhetett volna. |
| Leállítás |hello rendszer hello folyamatának hello feladat leállítása. |
| Felfüggesztve |hello feladat fel lett függesztve, hello felhasználó, hello rendszer vagy hello runbook egy parancsa. A felfüggesztett feladatokat újra lehet indítani, és indítsa újra az utolsó ellenőrzőpontot, illetve hello hello runbook kezdve, ha nem rendelkezik az ellenőrzőpontokat. hello runbook csak felfüggesztik hello rendszer, ha kivétel lép. Alapértelmezés szerint ErrorActionPreference túl van beállítva**Folytatás**, ami azt jelenti, hello feladatot a hiba folyamatosan működik. Ha ezt a preferenciaváltozót átállítja túl**leállítása**, majd hello feladat felfüggeszti az hiba.  Érvényes túl[grafikus és a PowerShell-munkafolyamati forgatókönyvek](automation-runbook-types.md) csak. |
| Felfüggesztése |hello rendszer megpróbál toosuspend hello feladat hello felhasználó hello kérésére. hello runbooknak el kell érnie a következő ellenőrzőpontot felfüggesztés előtt. Ha már elhagyta az utolsó ellenőrzőpontot, majd befejezné a felfüggesztés előtt.  Érvényes túl[grafikus és a PowerShell-munkafolyamati forgatókönyvek](automation-runbook-types.md) csak. |

## <a name="viewing-job-status-from-hello-azure-portal"></a>Hello Azure-portál a feladat állapotának megtekintése
Megtekintheti az összes runbook-feladatok összesített állapota vagy hello Azure-portálon vagy az integráció konfigurálása a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület tooforward runbook feladat állapotát az adott runbook-feladatok részleteit elemezze és feladat adatfolyamokat.  OMS Naplóelemzési integrálása kapcsolatos további információkért lásd: [feladat állapotát és a feladat adatfolyam továbbítása Automation tooLog szolgáltatás (OMS)](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Automatizálási runbook-feladatok összegzése
A kijelölt Automation-fiók sarkában hello, megtekintheti az összes runbook-feladatok hello kijelölt Automation-fiók alatt **Projekt statisztika** csempére.<br><br> ![Projekt statisztika csempe](./media/automation-runbook-execution/automation-account-job-status-summary.png).<br> Ez a csempe egy száma és a grafikus ábrázolása hello feladat állapota az összes feladat végrehajtása jeleníti meg.  

Megadja a hello csempére kattintva a hello **feladatok** panel, amelyen az összes feladat végrehajtása, állapot, a feladat végrehajtása és a kezdési és befejezési időpontjai összesített listáját tartalmazza.<br><br> ![Automation-fiók feladatok panelen](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  Szűrheti a feladatok listájának hello kiválasztásával **szűrve** és szűrést végezni egy adott runbook, a feladat állapotát, vagy hello hello legördülő listáról, a dátum/idő tartomány toosearch belül.<br><br> ![Szűrő feladat állapota](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Azt is megteheti, megtekintheti a feladat összefoglaló információk egy adott runbook runbook kijelölésével hello **Runbookok** panel az Automation-fiók, és jelölje ki hello **feladatok** csempére.  Ez megadja hello **feladatok** panelt, és ott rákattinthat a hello feladat rekord tooview azok részleteit és kimenetét.<br><br> ![Automation-fiók feladatok panelen](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>Feladat összegzése
Megtekintheti az összes, hogy egy adott runbookhoz és azok állapotával kapcsolatos hello feladatok listáját. Szűrheti a listát feladatok állapota, és a hello hello dátumtartomány utolsó módosítás toohello feladat. tooview a részletes információkat és a kimeneti, kattintson az egyes feladatok hello nevére. hello hello feladat részletes nézetében hello értékeket tartalmaz, amelyeket toothat feladat hello runbook-paraméterek.

A következő lépéseket tooview hello feladatok egy runbook hello is használhatja.

1. Hello Azure-portálon, válassza ki **Automation** majd válassza ki az Automation-fiók hello nevét.
2. Hello hub, válassza ki **Runbookok** majd a hello **Runbookok** panelen hello listából válasszon ki egy runbookot.
3. A kiválasztott hello runbook hello paneljén kattintson a hello **feladatok** csempére.
4. Kattintson a hello listában hello feladatok egyikét, és a hello runbook feladat részletei panelen megtekintheti azok részleteit és kimenetét.

## <a name="retrieving-job-status-using-windows-powershell"></a>A Windows PowerShell feladatállapot lekérése
Használhatja a hello [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) egy adott feladat egy runbook és hello adatai létrehozott tooretrieve hello feladatokat. Ha egy runbook indítása a Windows PowerShell használatával [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), akkor adja vissza, hello eredményezett feladatot. Használjon [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)egy feladat kimenetének kimeneti tooget.

hello alábbi Példaparancsok beolvasása hello a minta-runbookhoz tartozó utolsó feladatot és annak állapotát, a hello runbook paramétereinek megadott hello értékek és a hello hello feladat eredményét jeleníti meg.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>Igazságos elosztása révén
Rendelés tooshare erőforrások közötti összes runbookokat hello felhőben, az Azure Automation ideiglenesen el lesz bármely követően három óráig futott.  Ebben az időszakban, a feladatok [PowerShell-alapú runbookok](automation-runbook-types.md#powershell-runbooks) le van állítva, és nem fog újraindulni.  feladat állapotának megjelenítése hello **leállítva**.  Az ilyen típusú runbook mindig újraindítják hello elejétől, mivel nem támogatják az ellenőrzőpontokat.  

[PowerShell Munkafolyamatain alapuló runbookok](automation-runbook-types.md#powershell-workflow-runbooks) folytatódik a legutóbbi [ellenőrzőpont](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints).  Miután három óra, hello runbook-feladat fel lesz függesztve hello szolgáltatást és annak állapotát jeleníti meg **fut, az erőforrások Várakozás**.  Amikor elérhetővé válik a védőfalat, hello runbook automatikusan újraindul hello Automation szolgáltatás és hello legutóbbi ellenőrzőponttól folytatása.  Ez történik akkor normál PowerShell-munkafolyamat felfüggesztése/újraindításhoz.  Ha hello runbook újra meghaladja a futtatókörnyezet, három óra hello folyamat ismétlődik, toothree alkalommal be.  Hello harmadik újraindítás után, ha hello runbook még nem fejeződött be három óra, majd hello runbook-feladat sikertelen volt, hello feladat állapotánál **sikertelen Várakozás erőforrásokra**.  Ebben az esetben a következő kivétel hello hibával hello kap.

*hello feladat nem tovább fut, mert azt a ismételten fürtből hello azonos ellenőrzőpont. Győződjön meg arról, hogy a Runbook nem hajt végre műveletek végzésekor állapotában megőrzése nélkül.*

Ez az tooprotect hello szolgáltatás runbookok futásának befejezése nélkül határozatlan ideig, mivel ezek nem képesek toomake azt újra learningmodule nélkül toohello a következő ellenőrzőpontot.

Ha hello runbook nem ellenőrzőpontokkal rendelkezik, vagy hello feladat nem érte el a hello első ellenőrzőpont learningmodule előtt, majd újraindul hello elejétől.  

Ha létrehozta a forgatókönyvet, biztosítania kell, hogy hello idő toorun között két ellenőrzőpontokat tevékenységeket nem haladja meg a három óra. Szükség lehet, hogy nem érte el a három óra határértékét vagy hosszú feloszthatja tooadd ellenőrzőpontokat tooyour runbook tooensure műveletek futtatása. Például a runbook egy ismételt indexelése előfordulhat, hogy végre nagy SQL-adatbázis. Ha egy művelet nem fejeződik be valós hello belül megosztás korlátot, majd hello feladat a memóriából és újraindításra kerülnek hello elejétől. Ebben az esetben kell hello ismételt indexelése művelet több lépést, például egy olyan táblát újraindexelés, egyszerre történő feloszthatja, és helyezze egy ellenőrzőpontot egyes műveletek után, így hello feladat sikerült folytatása után utolsó toocomplete hello a művelethez.

## <a name="next-steps"></a>Következő lépések
* toolearn hello többféle módszerrel is lehet használt toostart egy runbookot, az Azure Automationben bővebben lásd: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)

