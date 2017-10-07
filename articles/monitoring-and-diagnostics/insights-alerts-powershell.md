---
title: "az Azure-szolgáltatások - PowerShell aaaCreate riasztások |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a>Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - PowerShell
> [!div class="op_single_selector"]
> * [Portál](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Parancssori felület](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan riasztások tooset be a metrika az Azure PowerShell használatával.  

A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.

* **Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ. Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.    
* **Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be. További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)

A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:

* e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése
* e-mail küldése a megadott tooadditional e-maileket.
* A webhook hívása
* egy Azure-runbook (csak az Azure-portálon hello) végrehajtásának elindítása

Konfigurálhatja, és a riasztási szabályok használatával adatainak beolvasása

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [parancssori felület (CLI)](insights-alerts-command-line-interface.md)
* [Az Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931945.aspx)

További információkért mindig beírhatja ```Get-Help``` és majd hello használatához segítséget keres a PowerShell-parancsot.

## <a name="create-alert-rules-in-powershell"></a>A riasztási szabályok létrehozása a PowerShell
1. Jelentkezzen be tooAzure.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Listájának hello előfizetések rendelkezésére. Győződjön meg arról, hogy megfelelő előfizetés hello dolgozik. Ha nem, állítsa be úgy egy toohello jobb hello kimenete használatával `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. toolist meglévő szabályokat egy erőforráscsoport, a következő parancs hello használata:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. a szabály toocreate kell toohave több fontos adatra először.

  * Hello **erőforrás-azonosító** hello erőforrás keresi tooset riasztást
  * Hello **metrikai meghatározásainak** az adott erőforrás érhető el

     Egyirányú tooget hello erőforrás-azonosító toouse hello Azure-portálon. Ha hello erőforrás létrehozása már be van állítva, válassza ki azt a hello portálon. Hello következő panelen válassza ki *tulajdonságok* alatt hello *beállítások* szakasz. **ERŐFORRÁS-azonosító** mező kitöltése hello következő panelen. Egy másik módja toouse hello [Azure erőforrás-kezelő](https://resources.azure.com/).

     A webes alkalmazás például az erőforrás-azonosító

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Használhat `Get-AzureRmMetricDefinition` tooview hello listája, minden metrikadefiníciót egy adott erőforráshoz.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     hello alábbi példa hoz létre adott mérőszám egység hello és táblázat hello metrikájú nevét.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     A Get-AzureRmMetricDefinition elérhető lehetőségek teljes listáját a Get-MetricDefinitions futtatásával áll rendelkezésre.
5. Példa állítja be a riasztást követő webhely erőforráson hello. hello riasztási eseményindítók Ha 5 percig, majd újra amikor megkapja sincs forgalom 5 percig következetesen kap minden forgalom.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. toocreate webhook vagy küldési e-mail elindítja a riasztást, amikor először létre kell hoznia hello e-mailek és/vagy webhook. Majd ezt követően a hello szabály azonnal létrehozása hello - műveletek címke és a hello a következő példában látható módon. Nem társítható webhook vagy e-mailek már hozott létre a szabályokat PowerShell segítségével.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. tooverify, hogy a riasztások elkészültek megfelelően hello egyes szabályok alapján.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. A riasztások törlése. Ezek a parancsok a cikkben korábban létrehozott hello szabályok törlése.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Következő lépések
* [Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.
* További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).
* További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).
* További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).
* Első egy [diagnosztikai naplók gyűjtésére áttekintése](monitoring-overview-of-diagnostic-logs.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.
* Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
