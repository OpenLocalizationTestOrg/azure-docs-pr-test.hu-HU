---
title: "Azure Automation-feladat adatok tooOMS Naplóelemzési aaaForward |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toosend feladat állapotát és a runbook-feladat az adatfolyamokat tooMicrosoft Operations Management Suite Naplóelemzési toodeliver további betekintést és kezelése."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/2017
ms.author: magoedte
ms.openlocfilehash: e78b6c6677d6502711ce828e2d32b7a91922ae26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-toolog-analytics-oms"></a>Feladat állapotát és a feladat adatfolyam továbbítása Automation tooLog szolgáltatás (OMS)
Automatizálási runbook feladat állapotát és a feladat adatfolyamok tooyour a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület küldhet.  Feladat naplózza, és a feladat adatfolyamok láthatók a hello Azure-portálon, vagy a PowerShell használatával, az egyes feladatok, és ez lehetővé teszi tooperform egyszerű vizsgálatokat. Most Log Analytics segítségével:

* Az automatizálási feladatok insight letöltése
* A runbook-feladat állapota (például felfüggesztett vagy sikertelen) alapuló riasztás vagy e-mail eseményindító
* Speciális lekérdezéseket írhat a feladat adatfolyamok között
* Feladatok összefüggéseket Automation-fiókok között
* A feladatelőzmények megjelenítheti az adott idő alatt     

## <a name="prerequisites-and-deployment-considerations"></a>Előfeltételek és központi telepítésével kapcsolatos megfontolások
az automatizálási küldése toostart tooLog Analytics naplózza, akkor kell:

1. hello November 2016 vagy újabb kiadása [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. A Naplóelemzési munkaterület. További információkért lásd: [Ismerkedés a Naplóelemzési](../log-analytics/log-analytics-get-started.md). 
3. hello ResourceId az Azure Automation-fiókhoz

az Azure Automation-fiók és a Naplóelemzési munkaterület, futtassa a következő PowerShell hello toofind hello ResourceId:

```powershell
# Find hello ResourceId for hello Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find hello ResourceId for hello Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Ha több Automation-fiókot, vagy a munkaterületek, parancsok, megelőző hello hello kimenete található hello *neve* tooconfigure kell, és másolja a hello értéke *ResourceId*.

Ha toofind hello kell *neve* az Automation-fiók hello Azure-portálon jelölje ki az Automation-fiók hello **Automation-fiók** panelhez, és válassza **mindenbeállítás**.  A hello **összes beállítás** panelen, a **Fiókbeállítások** kiválasztása **tulajdonságok**.  A hello **tulajdonságok** panelen, jegyezze fel ezeket az értékeket.<br> ![Automation-fiók tulajdonságai](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Log Analytics-integráció beállítása
1. A számítógépen indítása **Windows PowerShell** a hello **Start** képernyő.  
2. Másolja és illessze be a következő PowerShell hello és hello hello érték szerkesztése `$workspaceId` és `$automationAccountId`.  A hello `-Environment` paraméter érvényes értékei a *AzureCloud* vagy *AzureUSGovernment* attól függően, hogy hello felhőalapú környezet használata.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

A parancsfájl futtatása után látni fogja az Log Analytics-rekordok új JobLogs vagy írása JobStreams 10 percen belül.

toosee hello naplókat, futtassa a következő lekérdezés a Naplóelemzési naplófájl-keresési hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Konfiguráció ellenőrzése
az Automation-fiók küldő tooconfirm tooyour Naplóelemzési munkaterület naplózza, ellenőrizze, hogy diagnosztika helyesen van-e beállítva hello Automation-fiókot a következő PowerShell hello használata:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Hello kimenet győződjön meg arról, hogy:
+ A *naplók*, a következő hello *engedélyezve* van *igaz*
+ hello értékének *WorkspaceId* toohello ResourceId a Naplóelemzési munkaterület beállítása


## <a name="log-analytics-records"></a>Log Analytics-rekordok
Azure Automation diagnosztika kétféle típusú rekordok Naplóelemzési hoz létre, és címkével rendelkeznek, **típus = AzureDiagnostics**.

### <a name="job-logs"></a>Feladatnaplóit
| Tulajdonság | Leírás |
| --- | --- |
| TimeGenerated |Dátum és idő, amikor hello runbook-feladat végrehajtása. |
| RunbookName_s |hello runbook hello neve. |
| Caller_s |Kik kezdeményeztek hello műveletet.  Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer. |
| Tenant_g | A hívó hello hello bérlői azonosító GUID. |
| JobId_g |GUID azonosítója, amely hello hello runbook feladatának azonosítója. |
| ResultType |runbook-feladat hello hello állapota.  Lehetséges értékek:<br>- Elindítva<br>- Leállítva<br>- Felfüggesztve<br>- Sikertelen<br>-Befejeződött |
| Kategória | Hello típusú adatok besorolása.  Az automatizáláshoz a hello értéke JobLogs. |
| OperationName | Megadja az Azure-ban végrehajtott művelet hello típusát.  Az automatizáláshoz a hello értéke feladat. |
| Erőforrás | Hello Automation-fiók neve |
| SourceSystem | A Naplóelemzési hogyan hello adatokat gyűjteni. Mindig *Azure* az Azure diagnostics. |
| ResultDescription |Hello runbook feladat eredménye állapotokat ismerteti.  Lehetséges értékek:<br>- A feladat elindult<br>- A feladat nem sikerült<br>- A feladat befejeződött |
| CorrelationId |GUID azonosítója, amely hello hello runbook-feladat korrelációs azonosítója. |
| ResourceId |Hello Azure Automation szolgáltatásbeli fiók erőforrás azonosítóját hello runbook adja meg. |
| SubscriptionId | hello Azure-előfizetéssel azonosítót (GUID) hello Automation-fiók. |
| ResourceGroup | Az Automation-fiók hello hello erőforráscsoport nevét. |
| ResourceProvider | MICROSOFT. AUTOMATIZÁLÁS |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Feladat adatfolyamok
| Tulajdonság | Leírás |
| --- | --- |
| TimeGenerated |Dátum és idő, amikor hello runbook-feladat végrehajtása. |
| RunbookName_s |hello runbook hello neve. |
| Caller_s |Kik kezdeményeztek hello műveletet.  Lehetséges értékek: egy e-mail-cím vagy egy ütemezett feladatokat tartalmazó rendszer. |
| StreamType_s |feladatfolyam hello típusa. Lehetséges értékek:<br>- Folyamatban<br>- Kimenet<br>- Figyelmeztetés<br>- Hiba<br>- Hibakeresés<br>- Részletes |
| Tenant_g | A hívó hello hello bérlői azonosító GUID. |
| JobId_g |GUID azonosítója, amely hello hello runbook feladatának azonosítója. |
| ResultType |runbook-feladat hello hello állapota.  Lehetséges értékek:<br>-Folyamatban |
| Kategória | Hello típusú adatok besorolása.  Az automatizáláshoz a hello értéke JobStreams. |
| OperationName | Megadja az Azure-ban végrehajtott művelet hello típusát.  Az automatizáláshoz a hello értéke feladat. |
| Erőforrás | Hello Automation-fiók neve |
| SourceSystem | A Naplóelemzési hogyan hello adatokat gyűjteni. Mindig *Azure* az Azure diagnostics. |
| ResultDescription |Hello kimeneti adatfolyamba hello runbookból tartalmazza. |
| CorrelationId |GUID azonosítója, amely hello hello runbook-feladat korrelációs azonosítója. |
| ResourceId |Hello Azure Automation szolgáltatásbeli fiók erőforrás azonosítóját hello runbook adja meg. |
| SubscriptionId | hello Azure-előfizetéssel azonosítót (GUID) hello Automation-fiók. |
| ResourceGroup | Az Automation-fiók hello hello erőforráscsoport nevét. |
| ResourceProvider | MICROSOFT. AUTOMATIZÁLÁS |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>A Naplóelemzési bejelentkezik Automation megtekintése
Most, hogy használatba vette az Automation feladat naplók tooLog Analytics küldésekor, nézzük meg, mi mindent, ezek a naplók Naplóelemzési belül.

toosee hello naplókat, futtassa a következő lekérdezés hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>E-mailt küld, ha egy runbook-feladat sikertelen lesz, vagy felfüggesztése
Egy felső ügyfelünk kéri esetén a hello képességét toosend szöveg vagy egy e-mailt probléma merül fel a runbook-feladatok.   

toocreate riasztást szabály, akkor először hozzon létre a hello runbook napló keresése feladat azt jelzi, hogy hello riasztást kell meghívnia.  Kattintson a hello **riasztás** toocreate gombra, és a riasztási szabály hello konfigurálása.

1. Hello napló elemzés áttekintése lapon kattintson **naplófájl-keresési**.
2. A riasztás napló keresési lekérdezés létrehozásához írja be a következő keresési lekérdezés mezőbe hello hello: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` szerint is csoportosíthatók által hello RunbookName használatával:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Ha több mint egy automatizálási fiók vagy előfizetés tooyour munkaterületről naplók beállítását követően csoportosíthatók a a riasztások előfizetés és az Automation-fiók.  Automation-fiók neve JobLogs hello-keresés mezője hello erőforrás származtatható.  
3. tooopen hello **riasztási szabály hozzáadása** kattintson **riasztási** hello oldal hello tetején. Hello beállítások tooconfigure hello figyelmeztetéssel kapcsolatos további részletekért lásd: [Naplóelemzési riasztások](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Található összes feladatot, amely hibákkal fejeződött be
Ezenkívül tooalerting hibáiról, megtalálhatja, ha a runbook-feladatok nem okozó hibát tartalmaz. Ezekben az esetekben PowerShell egy hibafolyam eredményez, de nem okozó hibák hello nem a feladat toosuspend okozhatják, vagy sikertelen.    

1. A Naplóelemzési munkaterületet, kattintson **naplófájl-keresési**.
2. Hello lekérdezés mezőbe, írja be a `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` majd **keresési**.

### <a name="view-job-streams-for-a-job"></a>Nézet feladat adatfolyamok feladat
Ha egy feladat hibakeresés is érdemes lehet toolook hello feladat adatfolyamok be.  hello következő lekérdezés jeleníti meg a GUID 2ebd22ea-e05e-4eb9 – 9d 76-d73cbd4356e0 egyetlen feladat összes hello adatfolyamot:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Korábbi feladat állapotának megtekintése
Mindemellett érdemes lehet toovisualize a feladatelőzményekben adott idő alatt.  A lekérdezés toosearch időbeli használhat hello a feladatok állapotát.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS korábbi feladat állapota diagram](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Összefoglalás
Az Automation feladat állapotát és az adatfolyam adatok tooLog Analytics elküldésével az automatizálási feladatok által hello állapotának jobb betekintést kaphat:
+ Beállítása riasztások toonotify, ha probléma van a
+ Egyéni nézetek és a keresési lekérdezések toovisualize a eredmények, runbook-feladat állapotát, és egyéb kapcsolódó fő mutatók vagy metrikákat.  

A Naplóelemzési működési áttekinthetősége tooyour automatizálási feladatok biztosít, és segít a cím incidensek gyorsabb.  

## <a name="next-steps"></a>Következő lépések
* toolearn hogyan tooconstruct különböző keresési lekérdezések és felülvizsgálati hello Automation feladat naplók és a Log Analyticshez kapcsolatos további információkért lásd: [Log Analytics-e jelentkezni a keresések](../log-analytics/log-analytics-log-searches.md)
* Hogyan toocreate és lekérése kimenet és a hiba a runbookok, kapcsolatban lásd: toounderstand [Runbook kimenet és üzenetek](automation-runbook-output-and-messages.md)
* További információk a runbook végrehajtása toomonitor runbook feladatokat, valamint egyéb technikai részleteket lásd: hogyan toolearn [nyomon követheti a runbook-feladatok](automation-runbook-execution.md)
* További információ az OMS szolgáltatáshoz és a gyűjtemény adatforrások toolearn lásd [gyűjtése Azure storage adatok a Naplóelemzési – áttekintés](../log-analytics/log-analytics-azure-storage.md)
