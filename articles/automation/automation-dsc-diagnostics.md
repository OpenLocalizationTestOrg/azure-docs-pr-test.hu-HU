---
title: "Azure Automation DSC jelentési adatok tooOMS Naplóelemzési aaaForward |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toosend kívánt állapot konfigurációs szolgáltatása (DSC) jelentési adatok tooMicrosoft Operations Management Suite Naplóelemzési toodeliver további betekintést és felügyeleti."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.openlocfilehash: 21f78d5549d53ba3d7e237f55d9086f380cf3351
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="forward-azure-automation-dsc-reporting-data-toooms-log-analytics"></a>Azure Automation DSC jelentési adatok tooOMS Naplóelemzési továbbítsa

Automation DSC csomópont állapota adatok tooyour a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület küldhet.  
Megfelelőségi állapota látható hello Azure-portálon, vagy a PowerShell használatával, a csomópontok számára, és az egyedi DSC erőforrások a csomópont-konfigurációt. Log Analytics segítségével:

* A megfelelőségi adatok lekérése a felügyelt csomópontok és az egyes erőforrások
* Indítás, egy e-mailek vagy a riasztás a megfelelőségi állapot alapján
* Speciális lekérdezéseket írhat a felügyelt csomópontokon
* Megfelelőségi állapot összefüggéseket Automation-fiókok között
* A csomópont megfelelőségi előzményei megjelenítheti az adott idő alatt

## <a name="prerequisites"></a>Előfeltételek

az Automation DSC küldése toostart jelentések tooLog elemzés, szükséges:

* hello November 2016 vagy újabb kiadása [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
* Egy Azure Automation-fiókra. További információkért lásd: [Ismerkedés az Azure Automation szolgáltatással](automation-offering-get-started.md)
* A Naplóelemzési munkaterület egy **Automation & vezérlő** szolgáltatásajánlat. További információkért lásd: [Ismerkedés a Naplóelemzési](../log-analytics/log-analytics-get-started.md).
* Legalább egy Azure Automation DSC-csomópont. További információkért lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md) 

## <a name="set-up-integration-with-log-analytics"></a>Log Analytics-integráció beállítása

adatok importálása az Azure Automation DSC azokat a lépéseket követve teljes hello a Naplóelemzési toobegin:

1. Jelentkezzen be tooyour Azure PowerShell-fiókot. Lásd: [jelentkezzen be az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Hello beolvasása _ResourceId_ az automatizálási fiók hello a következő PowerShell-parancs futtatásával: (Ha egynél több automation-fiók, válassza a hello _ResourceID_ hello fiók tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Első hello _ResourceId_ a Naplóelemzési munkaterület hello a következő PowerShell-parancs futtatásával: (Ha egynél több munkaterületen, válassza a hello _ResourceID_ kívánt hello munkaterület tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. A következő PowerShell-parancsot, hogy futási hello `<AutomationResourceId>` és `<WorkspaceResourceId>` a hello _ResourceId_ értékek az egyes hello előző lépéseket:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Ha azt szeretné, hogy Azure Automation DSC-ből származó adatok importálása a Naplóelemzési toostop, futtassa a következő PowerShell-paranccsal hello.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-hello-dsc-logs"></a>Hello DSC-naplók megtekintése

Az Automation DSC adatok Log Analyticshez való integráció beállítása után a **naplófájl-keresési** gomb megjelenik hello **DSC-csomópontok** panelen található az automation-fiók. Kattintson a hello **naplófájl-keresési** tooview hello naplókat a DSC-csomópont adatait gombra.

![Napló Keresés gomb](media/automation-dsc-diagnostics/log-search-button.png)

Hello **naplófájl-keresési** panel nyílik meg, és megjelenik egy **DscNodeStatusData** művelete minden DSC-csomópont, és egy **DscResourceStatusData** minden művelet [DSC erőforrás](https://msdn.microsoft.com/powershell/dsc/resources) hello csomópontok konfigurációs alkalmazott toothat hívása.

Hello **DscResourceStatusData** művelet nem sikerült DSC erőforrásokat hiba adatait tartalmazza.

Kattintson az adott műveletre vonatkozó hello toosee hello listaadatai lévő egyes műveletek.

Megtekintheti a hello naplók [Log Analyticshez a rákeresve. Lásd: [található adatokat, és napló keresések](../log-analytics/log-analytics-log-searches.md).
Típus hello következő lekérdezés toofind a DSC naplók:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

Hello lekérdezés hello műveletnév is szűkítheti. Például: ' típus = AzureDiagnostics ResourceProvider = "MICROSOFT. AUTOMATIZÁLÁSI"kategória ="DscNodeStatus"OperationName ="DscNodeStatusData"

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>E-mailt küld, ha a DSC-megfelelőségi ellenőrzés sikertelen lesz.

A felső ügyfelek kéréseire egyike hello képességét toosend szöveg vagy egy e-mailt, amikor probléma merül fel a DSC-konfiguráció.   

szabály toocreate riasztást, akkor először hozzon létre egy napló keresése hello DSC jelentés azt jelzi, hogy hello riasztást kell meghívnia.  Kattintson a hello **riasztás** toocreate gombra, és a riasztási szabály hello konfigurálása.

1. Hello napló elemzés áttekintése lapon kattintson **naplófájl-keresési**.
1. A riasztás napló keresési lekérdezés létrehozásához írja be a következő keresési lekérdezés mezőbe hello hello:`Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`

  Ha több mint egy automatizálási fiók vagy előfizetés tooyour munkaterületről naplók beállítását követően csoportosíthatók a a riasztások előfizetés és az Automation-fiók.  
  Automation-fiók neve DscNodeStatusData hello-keresés mezője hello erőforrás származtatható.  
1. tooopen hello **riasztási szabály hozzáadása** kattintson **riasztási** hello oldal hello tetején. Hello beállítások tooconfigure hello figyelmeztetéssel kapcsolatos további információkért lásd: [Naplóelemzési riasztások](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Sikertelen a DSC-erőforrások található összes csomópont

Egy Naplóelemzési előnye, hogy a csomópontok közötti sikertelen ellenőrzést kereshet.
toofind DSC-erőforrások, melyeknél nem sikerült az összes példányát.

1. Hello napló elemzés áttekintése lapon kattintson **naplófájl-keresési**.
1. A riasztás napló keresési lekérdezés létrehozásához írja be a következő keresési lekérdezés mezőbe hello hello:`Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`

### <a name="view-historical-dsc-node-status"></a>Korábbi DSC csomópont állapotának megtekintése

Mindemellett érdemes lehet toovisualize a DSC-csomópont állapotát előzmények adott idő alatt.  
A lekérdezés toosearch időbeli használhat hello állapotának a DSC-csomópont állapotát.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Ez megjeleníti hello csomópont állapot diagram adott idő alatt.

## <a name="log-analytics-records"></a>Log Analytics-rekordok

Azure Automation diagnosztika rekordok két kategóriába Naplóelemzési hoz létre.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Tulajdonság | Leírás |
| --- | --- |
| TimeGenerated |Dátum és idő, amikor hello megfelelőségi ellenőrzés futtatta. |
| OperationName |DscNodeStatusData |
| ResultType |E hello csomópont megfelel. |
| NodeName_s |hello hello felügyelt csomópont neve. |
| NodeComplianceStatus_s |E hello csomópont megfelel. |
| DscReportStatus |E hello megfelelőségi ellenőrzés sikeresen lefutott. |
| ConfigurationMode | Hello konfigurációs Mitől alkalmazott toohello csomópont. A lehetséges értékek: __"ApplyOnly"__,__"ApplyandMonitior"__, és __"ApplyandAutoCorrect"__. <ul><li>__ApplyOnly__: DSC hello konfigurációjának alkalmazására szolgál, és nincs semmi hatása további, kivéve, ha az új konfiguráció fejlesztőre toohello célcsomóponttal, vagy ha az új konfiguráció lekért egy kiszolgáló. Az új konfiguráció első alkalmazása után DSC nem ellenőrzi a korábban konfigurált állapotból eltéréseket. A DSC kísérletek tooapply hello konfigurációs, amíg az sikeres előtt nem __ApplyOnly__ lép érvénybe. </li><li> __ApplyAndMonitor__: hello alapértelmezett érték. hello LCM alkalmazza minden új konfigurációt. Az új konfiguráció első alkalmazása után a hello célcsomóponttal drifts szükséges hello állapotból, ha DSC jelentések hello ellentmondás az naplókat. A DSC kísérletek tooapply hello konfigurációs, amíg az sikeres előtt nem __ApplyAndMonitor__ lép érvénybe.</li><li>__ApplyAndAutoCorrect__: DSC alkalmazza minden új konfigurációt. Az új konfiguráció első alkalmazása után hello célcsomóponttal drifts szükséges hello állapotból, ha DSC hello ellentmondás naplók az jelentéseket, és majd újra alkalmazza hello aktuális konfigurációja.</li></ul> |
| HostName_s | hello hello felügyelt csomópont neve. |
| IP-cím | a felügyelt csomópont hello hello IPv4-címét. |
| Kategória | DscNodeStatus |
| Erőforrás | hello hello Azure Automation-fiók neve. |
| Tenant_g | A hívó hello hello bérlői azonosító GUID. |
| NodeId_g |Hello felügyelt csomópont azonosító GUID. |
| DscReportId_g |Hello jelentés azonosító GUID. |
| LastSeenTime_t |Dátum és idő, amikor hello jelentés legutóbbi megtekintése. |
| ReportStartTime_t |Dátum és idő, amikor hello jelentés elindítása. |
| ReportEndTime_t |Dátum és idő, amikor hello jelentés befejeződött. |
| NumberOfResources_d |hello alkalmazott konfiguráció toohello csomópont nevű DSC erőforrások hello száma. |
| SourceSystem | A Naplóelemzési hogyan hello adatokat gyűjteni. Mindig *Azure* az Azure diagnostics. |
| ResourceId |Megadja a hello Azure Automation-fiók. |
| ResultDescription | hello leírást ehhez a művelethez. |
| SubscriptionId | hello Azure-előfizetéssel azonosítót (GUID) hello Automation-fiók. |
| ResourceGroup | Az Automation-fiók hello hello erőforráscsoport nevét. |
| ResourceProvider | MICROSOFT. AUTOMATIZÁLÁS |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Hello hello megfelelőségi jelentés korrelációs azonosító, GUID. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Tulajdonság | Leírás |
| --- | --- |
| TimeGenerated |Dátum és idő, amikor hello megfelelőségi ellenőrzés futtatta. |
| OperationName |DscResourceStatusData|
| ResultType |Hello erőforrás-e megfelelő. |
| NodeName_s |hello hello felügyelt csomópont neve. |
| Kategória | DscNodeStatus |
| Erőforrás | hello hello Azure Automation-fiók neve. |
| Tenant_g | A hívó hello hello bérlői azonosító GUID. |
| NodeId_g |Hello felügyelt csomópont azonosító GUID. |
| DscReportId_g |Hello jelentés azonosító GUID. |
| DscResourceId_s |hello hello DSC erőforráspéldány neve. |
| DscResourceName_s |hello hello DSC-erőforrás neve. |
| DscResourceStatus_s |E DSC erőforrás hello megfelel a szabályzatnak. |
| DscModuleName_s |hello hello PowerShell-modult, amely tartalmazza a hello DSC erőforrás neve. |
| DscModuleVersion_s |hello PowerShell modul, amely tartalmazza a hello DSC erőforrás hello verziója. |
| DscConfigurationName_s |hello konfigurációs hello neve toohello csomópont alkalmazza. |
| ErrorCode_s | hello hibakód, ha hello erőforrás sikertelen volt. |
| ErrorMessage_s |hello hibaüzenet, ha hello erőforrás nem sikerült. |
| DscResourceDuration_d |hello időtartamot (másodpercekben), hello DSC erőforrás futott. |
| SourceSystem | A Naplóelemzési hogyan hello adatokat gyűjteni. Mindig *Azure* az Azure diagnostics. |
| ResourceId |Megadja a hello Azure Automation-fiók. |
| ResultDescription | hello leírást ehhez a művelethez. |
| SubscriptionId | hello Azure-előfizetéssel azonosítót (GUID) hello Automation-fiók. |
| ResourceGroup | Az Automation-fiók hello hello erőforráscsoport nevét. |
| ResourceProvider | MICROSOFT. AUTOMATIZÁLÁS |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Hello hello megfelelőségi jelentés korrelációs azonosító, GUID. |

## <a name="summary"></a>Összefoglalás

Az Automation DSC adatok tooLog Analytics elküldésével kaphat az Automation DSC-csomópontok által hello állapotának jobb betekintést:

* Beállítása riasztások toonotify, ha probléma van a
* Egyéni nézetek és a keresési lekérdezések toovisualize a eredmények, runbook-feladat állapotát, és egyéb kapcsolódó fő mutatók vagy metrikákat.  

A Naplóelemzési nagyobb működési látható tooyour Automation DSC-adatokat biztosít, és gyorsabban segít a cím incidensek.  

## <a name="next-steps"></a>Következő lépések

* bővebben a hogyan tooconstruct különböző keresési lekérdezések, és nézze meg hello Automation DSC naplózza a Log Analyticshez toolearn lásd: [Log Analytics-e jelentkezni a keresések](../log-analytics/log-analytics-log-searches.md)
* További információ az Azure Automation DSC használata toolearn lásd [Ismerkedés az Azure Automation DSC](automation-dsc-getting-started.md)
* További információ az OMS szolgáltatáshoz és a gyűjtemény adatforrások toolearn lásd [gyűjtése Azure storage adatok a Naplóelemzési – áttekintés](../log-analytics/log-analytics-azure-storage.md)

