---
title: "a felügyeleti események tooActivity napló riasztások Azure riasztások aaaMigrate |} Microsoft Docs"
description: "A felügyeleti események riasztások eltávolítja a október 1. Készíti áttelepítése meglévő riasztásokat."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: johnkem
ms.openlocfilehash: e00bc4f0bad4e8f97443310770c333d250e343ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a>Telepítse át az Azure felügyeleti események tooActivity napló riasztások-riasztások


> [!WARNING]
> A felügyeleti események riasztások ki lesz kapcsolva a következőnél október 1. Ha ezek a riasztások, és át őket, ha igen, használja a hello irányban toounderstand alatt.
>
> 

## <a name="what-is-changing"></a>Mi van módosítása

Azure Monitor (korábban Azure Insights) kínált egy funkció toocreate kijelentkezés felügyeleti események elindul, és értesítések tooa webhook URL-cím vagy az e-mail címek generált riasztást. Előfordulhat, hogy létrehozta az egyik riasztást küld, az alábbi módszerek bármelyikét:
* Hello Azure-portál a bizonyos típusú erőforrások, a figyelés -> riasztások hozzáadása riasztás, ha "Riasztás" értéke túl -> "Események"
* Futó hello Add-AzureRmLogAlertRule PowerShell-parancsmaggal
* Közvetlenül a [riasztási REST API hello](http://docs.microsoft.com/rest/api/monitor/alertrules) rendelkező odata.type = "ManagementEventRuleCondition" és a dataSource.odata.type = "RuleManagementEventDataSource"
 
hello következő PowerShell-parancsfájl listáját adja vissza az összes riasztás a felügyeleti események, amelyek az előfizetést, valamint a minden riasztás beállítása hello feltételek vannak.

```powershell
Login-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Felügyeleti események rendelkezik nincs riasztás, ha a fenti hello PowerShell-parancsmag kimeneteként az alábbihoz hasonló figyelmeztetés üzenetből:

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

A figyelmeztető üzenetek figyelmen kívül hagyható. Ha a felügyeleti események rendelkezik riasztások, hello PowerShell parancsmag kimenete a következőképpen jelenik meg:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Minden riasztás szaggatott vonal választja el egymástól, és részleteket tartalmaznak hello riasztás és hello adott szabály figyelt hello erőforrás-azonosító.

Ez a funkció túl lett állítva[Azure tevékenység napló riasztások](monitoring-activity-log-alerts.md). Az új értesítések lehetővé teszik a tooset tevékenységnapló események vonatkozó feltétellel, és értesítést kaphat, ha egy új esemény megfelel hello feltétel. Számos fejlesztést tartalmaz a riasztásokat a management események is biztosítanak:
* A csoport értesítési címzettek ("műveletek") között számos riasztásokról felhasználhatja [művelet csoportok](monitoring-action-groups.md), módosítása, akik egy riasztást kell fogadnia hello összetettsége csökkentése.
* Közvetlenül a telefonján a SMS művelet csoportokkal is kap értesítést.
* Is [Tevékenységriasztásokat napló létrehozása a Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* Létrehozhat feltételek nagyobb rugalmasságot és összetettsége toomeet igény szerinti.
* Értesítések a gyorsabban érkeznek.
 
## <a name="how-toomigrate"></a>Hogyan toomigrate
 
Új tevékenység napló értesítés toocreate lehetőségek közül választhat:
* Hajtsa végre a [az útmutató, hogyan toocreate a riasztás hello Azure-portálon](monitoring-activity-log-alerts.md)
* Ismerje meg, hogyan túl[Resource Manager-sablon használatával riasztás létrehozása](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Riasztások a korábban létrehozott felügyeleti események nem lesz automatikusan áttelepített tooActivity napló riasztásokat. A jelenleg konfigurált, és manuálisan hozza létre őket tevékenység napló riasztásként felügyeleti események PowerShell parancsfájl toolist hello riasztások megelőző toouse hello van szüksége. A október 1, amely után riasztások felügyeleti események a már nem lesznek láthatók az Azure-előfizetése előtt kell végezni. Más típusú Azure riasztások, például Azure metrika riasztások, az Application Insights-riasztások és Naplóelemzési riasztások nem érinti ez a változás. Ha kérdése van, az alábbi hello megjegyzések fel.


## <a name="next-steps"></a>Következő lépések

* További információ [műveletnapló](monitoring-overview-activity-logs.md)
* Konfigurálása [napló Tevékenységriasztásokat Azure-portálon](monitoring-activity-log-alerts.md)
* Konfigurálása [Tevékenységriasztásokat napló erőforrás-kezelő használatával](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md)
* További információ [szolgáltatáshoz értesítést](monitoring-service-notifications.md)
* További információ [művelet csoportok](monitoring-action-groups.md)
