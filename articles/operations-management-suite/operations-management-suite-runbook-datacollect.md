---
title: Log Analytics-adatok egy runbookhoz, az Azure Automationben aaaCollecting |} Microsoft Docs
description: "Lépésenkénti útmutató, amely végigvezeti egy runbook létrehozása az Azure Automation toocollect adatok történő hello OMS tárházba Naplóelemzési analízis."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Egy Azure Automation-runbook Naplóelemzési az adatok gyűjtése
Jelentős mennyiségű Naplóelemzési adatokat gyűjteni különböző forrásokból, beleértve a [adatforrások](../log-analytics/log-analytics-data-sources.md) az ügynökökre és is [adatgyűjtés az Azure-ból](../log-analytics/log-analytics-azure-storage.md).  Vannak olyan helyzetek toocollect adatokat kell, amely nem érhető el a szabványos források keresztül, ha.  Ebben az esetben használhatja hello [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md) toowrite adatok tooLog Analytics bármely REST API-ügyfél.  Egy általános metódus tooperform ezen adatok gyűjtése az Azure Automationben használ egy runbookot.   

Ez az oktatóanyag bemutatja, hogyan létrehozásához és az Azure Automation toowrite adatok tooLog Analytics runbook ütemezése hello folyamaton keresztül.


## <a name="prerequisites"></a>Előfeltételek
Ebben a forgatókönyvben a következő erőforrások az Azure-előfizetéshez konfigurált hello igényel.  Egy ingyenes fiókot is lehet.

- [A Naplóelemzési munkaterület jelentkezzen](../log-analytics/log-analytics-get-started.md).
- [Azure automation-fiók](../automation/automation-offering-get-started.md).

## <a name="overview-of-scenario"></a>Forgatókönyv áttekintése
Ebben az oktatóanyagban automatizálási feladatok adatokat gyűjt a runbook fog írni.  Az Azure Automation Runbookjai PowerShell használatával valósíthatók meg, írása, és egy parancsfájl tesztelése hello Azure Automation-szerkesztőben kezdi.  Miután ellenőrizte, hogy hello szükséges információkat gyűjt, fogja, hogy adatokat tooLog Analytics írása, és ellenőrizze, hello egyéni adattípus.  Végezetül a rendszeres időközönként létre fog hozni egy ütemezés toostart hello runbookot.

> [!NOTE]
> Azure Automation toosend feladat információk tooLog Analytics ezt a runbookot nélkül is konfigurálhat.  Ebben a forgatókönyvben elsősorban a használt toosupport hello oktatóanyag, és javasolt, hogy küldjön hello adatok tooa teszt munkaterületen.  


## <a name="1-install-data-collector-api-module"></a>1. Data Collector API-modul telepítése
Minden [hello HTTP adatait gyűjtője API-kéréseket](../log-analytics/log-analytics-data-collector-api.md#create-a-request) megfelelően formázva, és egy authorization fejlécet tartalmaz.  Ehhez a runbookban, de a kódot, amely leegyszerűsíti ezt a folyamatot modul használatával hello mennyisége csökkenthető.  Egy modul, melyekkel [OMSIngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI) a PowerShell-galériában hello.

toouse egy [modul](../automation/automation-integration-modules.md) egy runbookban, telepítenie kell az Automation-fiókban.  Minden olyan forgatókönyvben hello ugyanazt a fiókot használhatja a hello funkciók hello modulban.  Új modul választásával telepítheti **eszközök** > **modulok** > **a modul hozzá lesz adva** az Automation-fiókban.  

PowerShell-galériában hello azonban lehetővé teszi egy gyors beállítás toodeploy modul közvetlen tooyour automatizálási fiókot, hogy használhassa ezt a beállítást a jelen oktatóanyag.  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. Nyissa meg túl[PowerShell-galériában](https://www.powershellgallery.com/).
2. Keresse meg **OMSIngestionAPI**.
3. Kattintson a hello **tooAzure automatizálás telepítése** gombra.
4. Válassza ki az automation-fiók, és kattintson a **OK** tooinstall hello modul.


## <a name="2-create-automation-variables"></a>2. Automatizálási változók létrehozása
[Automatizálási változók](..\automation\automation-variables.md) az Automation-fiók minden forgatókönyve használt értékek tárolásához.  Akkor runbookok több rugalmas lehetővé tételével toochange ezeket az értékeket szerkesztése nélkül hello tényleges runbook. Hello HTTP adatait gyűjtője API származó kérelmek hello azonosítója és kulcsa hello OMS-munkaterület szükséges, és változó eszközök ideális toostore ezt az információt.  

![Változók](media/operations-management-suite-runbook-datacollect/variables.png)

1. Hello Azure-portálon lépjen a tooyour Automation-fiók.
2. Válassza ki **változók** alatt **megosztott erőforrások**.
2. Kattintson a **változó hozzáadása** és hello két változó létrehozása a következő táblázat hello.

| Tulajdonság | Munkaterület-azonosító értéke | Munkaterület-kulcs értéke |
|:--|:--|:--|
| Név | WorkspaceId | WorkspaceKey |
| Típus | Karakterlánc | Karakterlánc |
| Érték | Illessze be a munkaterület azonosítója a Naplóelemzési munkaterület hello. | Illessze be kell jelentkezniük az hello elsődleges vagy másodlagos kulcsot a Naplóelemzési munkaterületet. |
| Titkosított | Nem | Igen |



## <a name="3-create-runbook"></a>3. Runbook létrehozása

Azure Automation szolgáltatásbeli szerkesztővé rendelkezik hello portálon, ahol szerkesztheti, és tesztelje a forgatókönyvet.  A hello beállítás toouse hello parancsfájl szerkesztő toowork van [PowerShell közvetlenül](../automation/automation-edit-textual-runbook.md) vagy [létrehozhat egy grafikus](../automation/automation-graphical-authoring-intro.md).  Ebben az oktatóanyagban a PowerShell-parancsfájllal fog működni. 

![Forgatókönyv szerkesztése](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. Keresse meg a tooyour Automation-fiók.  
2. Kattintson a **Runbookok** > **hozzáadása egy runbook** > **hozzon létre egy új runbookot**.
3. Hello runbook neve, írja be a következőt **gyűjtése-Automation-feladat**.  Hello runbooktípusba, válassza a **PowerShell**.
4. Kattintson a **létrehozása** toocreate hello runbook és kezdő hello szerkesztő.
5. Másolja és illessze be a következő kódot a hello runbook hello.  Tekintse meg a toohello megjegyzések hello parancsfájlban hello kód ismertetése.
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a>4. Runbook tesztelése
Azure Automation szolgáltatásbeli tartalmaz egy olyan környezetben túl[tesztelje a forgatókönyvet](../automation/automation-testing-runbook.md) közzététel előtt.  Vizsgálja meg a hello runbook által gyűjtött hello adatokat, és ellenőrizze a közzététel előtt elvárt Analytics tooLog tooproduction azt írja. 
 
![Runbook tesztelése](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. Kattintson a **mentése** toosave hello runbook.
1. Kattintson a **teszt ablaktábla** tooopen hello runbook hello tesztkörnyezetben.
3. A runbook paraméterekkel rendelkezik, mivel Ön felszólító tooenter értékeinek őket.  Adja meg a hello hello erőforráscsoport nevét, és hello automatizálási fiókot, amellyel a folyamatos toocollect feladat adatait.
4. Kattintson a **Start** toohello hello runbook indítása.
3. hello runbook elindul állapotú **várakozik** előtt túl kerül**futtató**.  
3. részletes kimenet hello runbook megjelenjen-e a json formátumban gyűjtött hello feladatok.  Ha nincsenek feladatok jelenik meg, majd lépett nincsenek feladatok létrehozott hello automation-fiók hello az elmúlt egy óra.  Indítsa el minden olyan forgatókönyvben hello automation-fiókban, és utána végezze el újra hello tesztelése.
4. Győződjön meg arról, hello kimeneti hello szereplő hibák utáni elemzés parancs tooLog nem mutatja.  Egy üzenet hasonló toohello következő kell rendelkeznie.

    ![POST kimeneti](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. A Naplóelemzési rekordjainak ellenőrzése
Miután hello runbook vizsgálat befejeződött, és ellenőrizte, hogy hello kimeneti sikeresen megérkezett, hello rekordok használatával lettek létrehozva ellenőrizheti egy [napló keresés a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).

![Kimenet](media/operations-management-suite-runbook-datacollect/log-output.png)

1. Hello Azure-portálon válassza ki a Naplóelemzési munkaterület.
2. Kattintson a **keresési jelentkezzen**.
3. A következő parancs típusa hello `Type=AutomationJob_CL` hello Keresés gombra. Vegye figyelembe, hogy hello rekordtípus tartalmaz, amely nincs meghatározva hello parancsfájl _CL.  Névutótagot automatikusan hozzáfűzött toohello napló típusa tooindicate, hogy a rendszer egy egyéni napló típusa.
4. Meg kell jelennie egy vagy több rekordot adott vissza, jelezve, hogy hello runbook a várt módon működik.


## <a name="6-publish-hello-runbook"></a>6. Hello runbook közzététele
Miután ellenőrizte, hogy adott hello runbook megfelelően működik, toopublish kell, így éles környezetben is futtatható.  Tooedit folytatja, és hello runbook tesztelése hello közzétett verzió módosítása nélkül.  

![Runbook közzététele](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. Térjen vissza a tooyour automation-fiók.
2. Kattintson a **Runbookok** válassza **gyűjtése-Automation-feladat**.
3. Kattintson a **szerkesztése** , majd **közzététele**.
4. Kattintson a **Igen** verziója, amelyet toooverwrite hello korábban ismételt tooverify közzétételekor.

## <a name="7-set-logging-options"></a>7. Naplózási beállítások megadása 
A teszteket, képes tooview volt [részletes kimenet](../automation/automation-runbook-output-and-messages.md#message-streams) mert hello parancsfájl hello $VerbosePreference változó beállítása.  A végleges kell tooset hello naplózási tulajdonságait: hello runbook Ha azt szeretné, hogy tooview részletes kimenet.  Ebben az oktatóanyagban használt hello runbook hello json küldött adatok mennyisége tooLog Analytics ekkor jelennek meg.

![Naplózás és nyomkövetés](media/operations-management-suite-runbook-datacollect/logging.png)

1. Válassza ki a runbook tulajdonságainak hello **naplózás és nyomkövetés** alatt **Runbookbeállítások**.
2. Hello beállításának módosítása **részletes rekordok naplózására** túl**a**.
3. Kattintson a **Save** (Mentés) gombra.

## <a name="8-schedule-runbook"></a>8. Runbook ütemezése
hello leggyakoribb módja toostart figyelési adatokat összegyűjtő runbook tooschedule azt toorun automatikusan.  Hozzon létre ehhez a [Azure Automation ütemezési](../automation/automation-schedules.md) és tooyour runbook csatlakoztatás.

![Runbook ütemezése](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. Válassza ki a runbook tulajdonságainak hello, **ütemezések** alatt **erőforrások**.
2. Kattintson a **ütemezés hozzáadása** > **ütemezés tooyour runbook hivatkozás** > **hozzon létre egy új ütemezést**.
5. A hello értékek hello ütemezést, majd kattintson a következő típus **létrehozása**.

| Tulajdonság | Érték |
|:--|:--|
| Név | AutomationJobs-óránként |
| Indítása | Válassza ki a bármikor legalább 5 perccel korábbi hello aktuális idő. |
| Ismétlődés | Ismétlődő |
| Ismétlődik minden | 1 óra |
| Készlet lejárati | Nem |

Hello ütemezés létrehozása után kell tooset hello paraméter értékeket, amelyeket a rendszer minden alkalommal, amikor az ütemezés hello runbook elindul.

6. Kattintson a **konfigurálása paraméterek és futtatási beállítások**.
7. Adja meg az értékeket a **ResourceGroupName** és **AutomationAccountName**.
8. Kattintson az **OK** gombra. 

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Ellenőrizze a runbook elindul az ütemezés szerint
Egy runbook indítását, Everytime [feladat jön létre](../automation/automation-runbook-execution.md) és a kimenetet a naplóba.  Valójában ezek a hello ugyanazt a feladatot, amely hello runbook gyűjt.  Ellenőrizheti, hogy hello runbook hello feladatok hello runbook ellenőrzésével hello ütemezések kezdő időpontja hello letelte után megfelelően elindul.

![Feladatok](media/operations-management-suite-runbook-datacollect/jobs.png)

1. Válassza ki a runbook tulajdonságainak hello, **feladatok** alatt **erőforrások**.
2. Meg kell jelennie, minden alkalommal hello runbook feladatok listáját lett elindítva.
3. Kattintson a hello feladatok tooview egyik hozzá tartozó részletek.
4. Kattintson a **összes napló** tooview hello naplókat, valamint a kimeneti hello runbookból.
5. Görgessen toohello alsó toofind egy bejegyzés hasonló toohello az alábbi képen.<br>![Részletes](media/operations-management-suite-runbook-datacollect/verbose.png)
6. Kattintson arra a bejegyzésre tooview hello részletes tooLog Analytics elküldött json-adatokat.



## <a name="next-steps"></a>Következő lépések
- Használjon [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) egy nézet megjelenítése toocreate hello, hogy toohello Naplóelemzési tárház korábban összegyűjtött adatokat.
- A runbookot a csomag egy [felügyeleti megoldás](operations-management-suite-solutions-creating.md) toodistribute toocustomers.
- További információ [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/).
- További információ [Azure Automation](https://docs.microsoft.com/azure/automation/).
- További tudnivalók hello [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md).
