---
title: Azure Automation runbookjai aaaChild |} Microsoft Docs
description: "Az Azure Automationben runbook indítása másik runbookból és a közöttük információk megosztása hello különböző módszerét ismerteti."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 919887b9-43e2-4c16-883c-f81807fe37db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: magoedte;bwren
ms.openlocfilehash: d3d06818d344b565d53cc4f4705b41dcfcf9a376
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="child-runbooks-in-azure-automation"></a>Azure Automation runbookjai gyermek
Akkor célszerű az Azure Automation toowrite újrafelhasználható, moduláris runbookok egy más runbookok által használható önálló funkcióval. A szülő runbook gyakran telefonhívásokhoz egy vagy több gyermekrunbookot szükséges tooperform funkciót. Két módon toocall gyermekrunbook, és mindegyik rendelkezik-e különböző módszer közötti különbségeket tisztában kell lennie, hogy megállapíthassa, amely a különböző forgatókönyvek esetén ajánlott lesz.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Beágyazott Futtatás használatával gyermek runbook meghívása
tooinvoke beágyazottan indított runbook másik runbookból hello runbook hello nevét használja, és adjon meg értékeket a paraméterek ugyanúgy egy tevékenységet vagy egy parancsmagot használhatja.  Az összes runbook hello azonos Automation-fiók vannak elérhető tooall más toobe az ilyen módon használt. hello szülőrunbook megvárja, hogy hello gyermek runbook toocomplete toohello következő sorra helyezése előtt, és minden kimenetet közvetlenül toohello szülő visszaadja.

A beágyazottan indított runbook indításakor, ugyanaz, mint a szülő runbook hello feladat hello fusson. Nincs hello gyermekrunbook annak feladatelőzményeiben hello feladatelőzmények feltüntetése lesz. Kivételeket és adatfolyam-hello gyermekrunbook kimenetét hello szülő társítva lesz. Ez kevesebb feladatot eredményez, és megkönnyíti a könnyebb tootrack és tootroubleshoot óta hello gyermekrunbook összes kivételét és adatfolyam-kimenetét bármelyike hello szülő feladatukhoz társított.

Egy runbook közzétételekor runbookok gyermekrunbookoknak már közzé kell tenni. Ennek oka az Azure Automation runbookok társítást hoz létre, amikor gyermekrunbookkal. Ellenkező esetben a szülőrunbook hello megfelelően jelenik-e toopublish, de kivételt hoz létre, amikor elindul. Ez akkor fordul elő, ha újbóli hello szülőrunbook rendelés tooproperly hivatkozás hello gyermek runbookok. Ha bármelyik alárendelt runbookokat hello megváltoznak, mert hello társítás fog létre lett hozva, nem kell toorepublish hello szülőrunbook.

a gyermekrunbook beágyazottan meghívott hello paraméterek értéke lehet bármely adattípushoz, többek között összetett objektumok, és nincs nincs [JSON-szerializálás](automation-starting-a-runbook.md#runbook-parameters) , amikor elkezdi hello runbook hello Azure felügyeleti portál használatával, vagy a hello Start-AzureRmAutomationRunbook parancsmag.

### <a name="runbook-types"></a>Runbook-típusok
Milyen típusú hívhatják meg egymással:

* A [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) és [grafikus forgatókönyvek](automation-runbook-types.md#graphical-runbooks) hívhatják meg minden egyéb beágyazott (mindkettő PowerShell-alapú).
* A [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks) és grafikus PowerShell-munkafolyamati forgatókönyvek hívhatják meg minden egyéb beágyazott (mindkettő PowerShell munkafolyamat alapú)
* PowerShell típusú hello és hello PowerShell munkafolyamat-típusok nem hívható meg egymással beágyazott, és meg kell felelnie a Start-AzureRmAutomationRunbook.

Ha sorrendben függetlenül attól, hogy közzététele:

* hello PowerShell munkafolyamatok és a grafikus PowerShell-munkafolyamati forgatókönyvek runbookok csak kérdések sorrendjének közzététele.

Meghívja a beágyazott Futtatás használatával grafikus vagy PowerShell-munkafolyamati gyermekrunbook, csak hello runbook hello nevét használja.  A PowerShell gyermek runbook hívásakor a neve előtt kell *.\\*  , hogy a parancsfájl hello toospecify hello helyi könyvtárban található. 

### <a name="example"></a>Példa
a következő példa hello hív meg, egy teszt gyermekrunbook, amely három paramétert, egy összetett objektum, egy egész számot és egy logikai érték fogadja. hello gyermekrunbook kimenetét hello tooa változó van hozzárendelve.  Ebben az esetben hello gyermekrunbook egy PowerShell-munkafolyamati forgatókönyv

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Az alábbiakban látható hello ugyanebben a példában egy PowerShell-forgatókönyv használatával hello gyermekhelyként.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true


## <a name="starting-a-child-runbook-using-cmdlet"></a>Parancsmag használatával gyermekrunbook indítása
Használhatja a hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) parancsmag toostart a runbook [toostart runbookkal a Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Ez a parancsmag használati két módja van.  Az egyik mód hello parancsmag adja vissza hello feladatazonosító, amint a gyermekrunbook hello hello gyermek feladat jön létre.  A más módját, amely engedélyezi a hello megadásával hello **-Várjon, amíg** paraméter, hello parancsmag megvárja hello gyermek feladat befejeződik, és visszatér a hello kimeneti hello gyermek runbook.

a parancsmaggal indított gyermekrunbook hello feladatot a szülőrunbook hello külön feladatot fog futni. Ez több feladatot eredményez, mint hello beágyazottan indított runbook meghívása és nehezebb tootrack válnak. hello szülő több gyermekrunbookot aszinkron módon nélkül indíthatják minden toocomplete vár. A párhuzamos végrehajtás hello gyermekrunbookokat hívja, hogy ilyen típusú hello szülőrunbook kellene toouse hello [parallel kulcsszót](automation-powershell-workflow.md#parallel-processing).

A parancsmaggal indított gyermekrunbook paramétereinek megadott egy kivonattáblát a [Runbook-paraméterek](automation-starting-a-runbook.md#runbook-parameters). Csak egyszerű adattípusok használhatók. Ha hello runbook rendelkezik összetett adattípusú paraméterrel, majd, beágyazottan kell meghívni.

### <a name="example"></a>Példa
hello alábbi példa egy gyermek runbook paraméterek és kezdődik majd vár hello Start-AzureRmAutomationRunbook használatával toocomplete-várja meg a paramétert. Ezt követően a hello gyermekrunbook begyűjti a kimenetet.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>A gyermekrunbookok meghívására szolgáló módszerek összehasonlítása
hello következő táblázat összefoglalja hello különbségei hello két módszert egy runbook másik runbookból hívja.

|  | Beágyazott | Parancsmag |
|:--- |:--- |:--- |
| Feladat |Gyermek runbookokat hello azonos hello szülőként feladat futtatását. |Egy külön feladat hello gyermek runbook jön létre. |
| Végrehajtás |A folytatás előtt a szülőrunbook megvárja a hello gyermek runbook toocomplete. |Szülő runbook továbbra is fennáll, a gyermekrunbook indítását követően azonnal *vagy* szülőrunbook megvárja-e a hello gyermek feladat toofinish. |
| Kimenet |Szülőrunbook közvetlenül lekérheti kimeneti gyermekrunbook. |Szülő runbook kell lekérnie a kimenetet gyermek runbook-feladat *vagy* szülőrunbook közvetlenül lekérheti kimeneti gyermekrunbook. |
| Paraméterek |Hello gyermekrunbook paramétereinek értékeit külön kell meghatározni, és bármilyen adattípus használható. |Értékek hello gyermekrunbook paraméterei egyesíthetők kell egyetlen kivonattáblába, és csak egyszerű, a tömb, és objektum-adattípust adott használja ki a JSON-szerializálást. |
| Automation-fiók |Szülő runbook csak használhatja gyermekrunbook hello azonos automation-fiók. |Szülő runbook használhatja a gyermek runbook bármely hello az automation-fiók azonos Azure-előfizetés és még egy másik előfizetést, ha a kapcsolat tooit rendelkezik. |
| Közzététel |Gyermekrunbook közzé kell tenni, mielőtt a szülőrunbook közzététele. |Gyermekrunbook közzé kell tenni a szülőrunbook indítása előtt. |

## <a name="next-steps"></a>Következő lépések
* [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)
* [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)

