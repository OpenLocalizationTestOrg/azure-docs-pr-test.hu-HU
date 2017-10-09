---
title: "Azure Automation forgatókönyv aaaStarting |} Microsoft Docs"
description: "Hello többféle módszerrel is használt toostart egy runbookot, az Azure Automationben, valamint az információk is hello Azure-portál és a Windows PowerShell biztosít foglalja össze."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e44bce5b56b8e803f9247fbb4f3d4db7ab35c913
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a>Runbook elindítása az Azure Automationben
hello a következő táblázat segítségével meghatározhatja, hogy hello metódus toostart az Azure Automationben legmegfelelőbb tooyour adott forgatókönyv, amely runbook. Ez a cikk tartalmazza a runbook indítása a hello Azure-portálon, és a Windows PowerShell használatával. Részletek a hello más módszerekkel igénybe más dokumentáció, amely hello a lenti hivatkozásokra kattintva érheti el.

| **MÓDSZER** | **JELLEMZŐI** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>Az interaktív felhasználói kezelőfelület legegyszerűbb módja.<br> <li>Űrlap tooprovide egyszerű paraméterértékeket.<br> <li>Feladat állapotát nyomon.<br> <li>Az Azure bejelentkezési hitelesített hozzáférést. |
| [A Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>A Windows PowerShell-parancsmagokkal a parancssorból hívható.<br> <li>Több lépést automatizált megoldást szerepeljenek.<br> <li>A tanúsítvány vagy egyszerű / szolgáltatás OAuth felhasználói kérelem hitelesítésekor egyszerű.<br> <li>Adja meg egyszerű és összetett paraméter értékét.<br> <li>Feladat-állapotok nyomon követésére.<br> <li>Ügyfél toosupport PowerShell-parancsmagok szükséges. |
| [Azure Automation szolgáltatásbeli API](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>Legrugalmasabb módszer, de a legtöbb komplex is.<br> <li>Felelnek meg, és bármilyen egyéni kód használhat, amelyek HTTP-kérelmekre.<br> <li>A kérelem hitelesítése a tanúsítványt, vagy Oauth felhasználó egyszerű / szolgáltatás egyszerű.<br> <li>Adja meg egyszerű és összetett paraméter értékét.<br> <li>Feladat-állapotok nyomon követésére. |
| [Webhook](automation-webhooks.md) |<li>Indítsa el a runbook egyetlen HTTP-kérelemből.<br> <li>Felhasználók hitelesítése a biztonsági jogkivonat URL-címben.<br> <li>Ügyfél nem bírálhatja felül a webhook létrehozásakor megadott paraméterértékek. Runbook megadhat egyetlen paramétert, amely hello HTTP kérelem részletes adatait a telepítéskor.<br> <li>Nincs lehetősége tootrack feladat állapota a webhook URL-CÍMÉT. |
| [Válaszoljon tooAzure riasztás](../log-analytics/log-analytics-alerts.md) |<li>Elindít egy forgatókönyvet az adott válasz tooAzure riasztás.<br> <li>Konfigurálja webhook runbook és tooalert hivatkozásra.<br> <li>Felhasználók hitelesítése a biztonsági jogkivonat URL-címben. |
| [Ütemezés](automation-schedules.md) |<li>Óránkénti, napi, heti vagy havi ütemezés szerint automatikusan elindítja a runbookot.<br> <li>Azure-portálon, a PowerShell-parancsmagok vagy az Azure API ütemezés módosítására.<br> <li>Adja meg a paramétert értékek toobe használt ütemezéssel. |
| [Egy másik runbookból](automation-child-runbooks.md) |<li>Egy runbook használata egy másik runbook egy tevékenységére.<br> <li>Akkor hasznos, ha több runbookok által használt funkciók.<br> <li>Adja meg a paramétert értékek toochild runbookot, és kimeneti a szülőrunbook. |

hello alábbi kép ábrázolja részletes, lépésenkénti folyamat hello életciklusának egy runbook. Ez magában foglalja a különböző módokon egy runbook indítását az Azure Automationben hibrid forgatókönyv-feldolgozó tooexecute Azure Automation-forgatókönyv és a különböző összetevők közötti interakció szükséges összetevőket. Automation-forgatókönyveket az adatközpontban található végrehajtás alatt álló toolearn tekintse meg a túl[hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md)

![Runbook-architektúra](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a>Runbook indítása a hello Azure-portálon
1. Hello Azure-portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.
2. Hello központ menüben válassza ki a **Runbookok**.
3. A hello **Runbookok** panelen válassza ki a runbookot, és kattintson a **Start**.
4. Ha hello runbook paraméterekkel rendelkezik, mindegyik paraméterhez szövegmezőben felszólító tooprovide értékeket fogja. Lásd: [Runbook-paraméterek](#Runbook-parameters) alatt kapcsolatos tovább információkért paraméterek.
5. A hello **feladat** panelen megtekintheti hello hello runbook-feladat állapotának.

## <a name="starting-a-runbook-with-windows-powershell"></a>Runbook indítása a Windows PowerShell használatával
Használhatja a hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart egy runbookot, a Windows PowerShell használatával. hello alábbi példakód elindít egy Test-Runbook nevű runbookot.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Az objektum kezdő-AzureRmAutomationRunbook értéket ad vissza, a feladat használható tootrack állapotának hello runbook indítását követően. Ezután használhatja a feladatobjektum rendelkező [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) hello feladat állapotának toodetermine hello és [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget annak kimenetét. a következő példakód hello elindít egy Test-Runbook nevű, megvárja, amíg befejeződött, majd megjeleníti annak kimenetét.

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

Ha hello runbookhoz paraméterek szükségesek, akkor meg kell adnia azokat egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) Ha hello hello kivonattábla kulcsa megegyezik-e a hello nevét és értékét hello hello paraméter értéke. hello következő példa bemutatja, hogyan toostart egy karakterlánc-paraméterrel rendelkező runbook nevű Utónév és Vezetéknév, egy RepeatCount nevű egész számot és egy Show nevű logikai paraméterrel. A paraméterek további információkért lásd: [Runbook-paraméterek](#Runbook-parameters) alatt.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook-paraméterek
Amikor elindít egy runbookot hello Azure portál vagy a Windows PowerShell, hello utasítás hello Azure Automation webszolgáltatás keresztül küld el. Ez a szolgáltatás nem támogatja az összetett adattípusú paramétereket. Ha egy összetett paraméterhez tooprovide érték van szüksége, akkor meg kell hívnia az beágyazott egy másik runbookból a [az Azure Automation runbookjai gyermek](automation-child-runbooks.md).

Azure Automation webszolgáltatás hello paramétereket a bizonyos adattípusokkal hello a következő részekben leírtak szerint a speciális funkciókat biztosít.

### <a name="named-values"></a>Névvel ellátott értékek
Ha hello paraméter adattípusa [objektum], akkor is használhatja a következő JSON formátumban toosend azt listája nevű értékek hello: *{Név1: 'Érték1', Name2: 'Érték2', név3: "Érték3"}*. Ezek az értékek csak egyszerű típusok lehetnek. hello runbook kap hello paraméterként egy [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) érték nevű tooeach megfelelő tulajdonságokkal.

Fontolja meg a teszt runbook, amely fogad egy felhasználó paraméter a következő hello.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

hello következő szöveg használható hello felhasználói paraméter.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Az eredmény kimenete a következő hello.

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>Tömbök
Ha hello paraméter egy tömb, például [array] vagy [string []], akkor hello JSON formátumban toosend követően az értékek listájának: *[érték1, érték2, érték3]*. Ezek az értékek csak egyszerű típusok lehetnek.

Fontolja meg a teszt runbook, amely fogad egy paraméter, a következő hello *felhasználói*.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

hello következő szöveg használható hello felhasználói paraméter.

```
["Joe","Smith",2,true]
```

Az eredmény kimenete a következő hello.

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Hitelesítő adatok
Ha hello paraméter adattípusa **PSCredential**, akkor megadhatja egy Azure Automation hello neve [hitelesítőadat-eszköz](automation-credentials.md). hello runbook lekéri hello hitelesítő adat hello névvel.

Fontolja meg a teszt runbook, amely fogad egy paraméter, hitelesítő adat a következő hello.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

hello következő szöveg használható hello felhasználói paraméter, feltéve, hogy létezik egy nevű hitelesítőeszköz *a hitelesítő adatok*.

```
My Credential
```

Feltéve, hello felhasználónév hello hitelesítő adatok a *jsmith*, az eredmény kimenete a következő hello.

```
jsmith
```

## <a name="next-steps"></a>Következő lépések
* hello runbook architektúra aktuális cikkben runbookok kezelése Azure és a helyszíni az hello hibrid forgatókönyv-feldolgozó erőforrások magas szintű áttekintést nyújt.  Automation-forgatókönyveket az adatközpontban található végrehajtás alatt álló toolearn tekintse meg a túl[hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md).
* toolearn hello más runbookok által használt egyedi vagy közös funkciók, moduláris runbookok toobe létrehozásával kapcsolatos további információkért tekintse meg a túl[Gyermekforgatókönyvek](automation-child-runbooks.md).

