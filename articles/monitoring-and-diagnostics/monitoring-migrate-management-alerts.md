---
title: "Telepítse át az Azure-riasztások felügyeleti események napló Tevékenységriasztásokat |} Microsoft Docs"
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
ms.openlocfilehash: 08a457029d3721f5c38dbcd2d2aab7d09a241d8f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a><span data-ttu-id="5ce3e-104">Telepítse át a felügyeleti események Azure riasztások tevékenységnapló riasztások</span><span class="sxs-lookup"><span data-stu-id="5ce3e-104">Migrate Azure alerts on management events to Activity Log alerts</span></span>


> [!WARNING]
> <span data-ttu-id="5ce3e-105">A felügyeleti események riasztások ki lesz kapcsolva a következőnél október 1.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-105">Alerts on management events will be turned off on or after October 1.</span></span> <span data-ttu-id="5ce3e-106">Az alábbi utasításokat követve segítségével azonosíthatja, ha ezek a riasztások, és át őket, ha így van.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-106">Use the directions below to understand if you have these alerts and migrate them if so.</span></span>
>
> 

## <a name="what-is-changing"></a><span data-ttu-id="5ce3e-107">Mi van módosítása</span><span class="sxs-lookup"><span data-stu-id="5ce3e-107">What is changing</span></span>

<span data-ttu-id="5ce3e-108">Az Azure Monitor (korábban Azure Insights) érhető el egy olyan képességet, amely felügyeleti események ki indított és értesítések egy webhook URL-címe vagy e-mail címekre létrehozott riasztás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-108">Azure Monitor (formerly Azure Insights) offered a capability to create an alert that triggered off of management events and generated notifications to a webhook URL or email addresses.</span></span> <span data-ttu-id="5ce3e-109">Előfordulhat, hogy létrehozta az egyik riasztást küld, az alábbi módszerek bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="5ce3e-109">You may have created one of these alerts any of these ways:</span></span>
* <span data-ttu-id="5ce3e-110">Az Azure-portálon az egyes erőforrástípusok figyelés -> riasztások-hozzáadása riasztás, ha "Riasztás" értéke "Események" ></span><span class="sxs-lookup"><span data-stu-id="5ce3e-110">In the Azure portal for certain resource types, under Monitoring -> Alerts -> Add Alert, where “Alert on” is set to “Events”</span></span>
* <span data-ttu-id="5ce3e-111">Az Add-AzureRmLogAlertRule PowerShell-parancsmag futtatásával</span><span class="sxs-lookup"><span data-stu-id="5ce3e-111">By running the Add-AzureRmLogAlertRule PowerShell cmdlet</span></span>
* <span data-ttu-id="5ce3e-112">Közvetlenül a [riasztási REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) rendelkező odata.type = "ManagementEventRuleCondition" és a dataSource.odata.type = "RuleManagementEventDataSource"</span><span class="sxs-lookup"><span data-stu-id="5ce3e-112">By directly using [the alert REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) with odata.type = “ManagementEventRuleCondition” and dataSource.odata.type = “RuleManagementEventDataSource”</span></span>
 
<span data-ttu-id="5ce3e-113">A következő PowerShell-parancsfájl az összes riasztás listáját, amelyeknek az előfizetés, valamint az egyes riasztások beállítása feltételek felügyeleti események adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-113">The following PowerShell script returns a list of all alerts on management events that you have in your subscription, as well as the conditions set on each alert.</span></span>

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

<span data-ttu-id="5ce3e-114">Felügyeleti események rendelkezik nincs riasztás, ha a fenti PowerShell-parancsmag kimeneteként az alábbihoz hasonló figyelmeztetés üzenetből:</span><span class="sxs-lookup"><span data-stu-id="5ce3e-114">If you have no alerts on management events, the PowerShell cmdlet above will output a series of warning messages like this one:</span></span>

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

<span data-ttu-id="5ce3e-115">A figyelmeztető üzenetek figyelmen kívül hagyható.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-115">These warning messages can be ignored.</span></span> <span data-ttu-id="5ce3e-116">Ha a felügyeleti események riasztások rendelkezik, a PowerShell parancsmag kimenete a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="5ce3e-116">If you do have alerts on management events, the output of this PowerShell cmdlet will look like this:</span></span>

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

<span data-ttu-id="5ce3e-117">Minden riasztás szaggatott vonal választja el egymástól, és a részletek közé tartoznak az erőforrás-azonosítója a riasztás és figyeli az adott szabályra.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-117">Each alert is separated by a dashed line and details include the resource ID of the alert and the specific rule being monitored.</span></span>

<span data-ttu-id="5ce3e-118">Ez a funkció átváltott [Azure tevékenység napló riasztások](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="5ce3e-118">This functionality has been transitioned to [Azure Monitor Activity Log Alerts](monitoring-activity-log-alerts.md).</span></span> <span data-ttu-id="5ce3e-119">Az új értesítések lehetővé teszik a tevékenységnapló események állítson be feltételt, és értesítést kaphat, ha egy új esemény megfelel a következő feltételt.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-119">These new alerts enable you to set a condition on Activity Log events and receive a notification when a new event matches the condition.</span></span> <span data-ttu-id="5ce3e-120">Számos fejlesztést tartalmaz a riasztásokat a management események is biztosítanak:</span><span class="sxs-lookup"><span data-stu-id="5ce3e-120">They also offer several improvements from alerts on management events:</span></span>
* <span data-ttu-id="5ce3e-121">A csoport értesítési címzettek ("műveletek") között számos riasztásokról felhasználhatja [művelet csoportok](monitoring-action-groups.md), módosítása, akik egy riasztást kell fogadnia összetettsége csökkentése.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-121">You can reuse your group of notification recipients (“actions”) across many alerts using [Action Groups](monitoring-action-groups.md), reducing the complexity of changing who should receive an alert.</span></span>
* <span data-ttu-id="5ce3e-122">Közvetlenül a telefonján a SMS művelet csoportokkal is kap értesítést.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-122">You can receive a notification directly on your phone using SMS with Action Groups.</span></span>
* <span data-ttu-id="5ce3e-123">Is [Tevékenységriasztásokat napló létrehozása a Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="5ce3e-123">You can [create Activity Log Alerts with Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
* <span data-ttu-id="5ce3e-124">Nagyobb rugalmasságot és igény szerinti erősségét feltételek hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-124">You can create conditions with greater flexibility and complexity to meet your specific needs.</span></span>
* <span data-ttu-id="5ce3e-125">Értesítések a gyorsabban érkeznek.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-125">Notifications are delivered more quickly.</span></span>
 
## <a name="how-to-migrate"></a><span data-ttu-id="5ce3e-126">Hogyan telepítheti át</span><span class="sxs-lookup"><span data-stu-id="5ce3e-126">How to migrate</span></span>
 
<span data-ttu-id="5ce3e-127">Hozzon létre egy új tevékenység napló értesítés, a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="5ce3e-127">To create a new Activity Log Alert, you can either:</span></span>
* <span data-ttu-id="5ce3e-128">Hajtsa végre a [útmutatónkban kapcsolatos riasztás létrehozása az Azure-portálon](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-128">Follow [our guide on how to create an alert in the Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="5ce3e-129">Megtudhatja, hogyan [Resource Manager-sablon használatával riasztás létrehozása](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-129">Learn how to [create an alert using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
 
<span data-ttu-id="5ce3e-130">Riasztások a korábban létrehozott felügyeleti események nem lesz automatikusan áttelepítve a tevékenység napló riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-130">Alerts on management events that you have previously created will not be automatically migrated to Activity Log Alerts.</span></span> <span data-ttu-id="5ce3e-131">A fenti PowerShell-parancsfájl segítségével a riasztásokat a felügyeleti események van beállítva, és manuálisan hozza létre őket tevékenység napló riasztásként kell.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-131">You need to use the preceding PowerShell script to list the alerts on management events that you currently have configured and manually recreate them as Activity Log Alerts.</span></span> <span data-ttu-id="5ce3e-132">A október 1, amely után riasztások felügyeleti események a már nem lesznek láthatók az Azure-előfizetése előtt kell végezni.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-132">This must be done before October 1, after which alerts on management events will no longer be visible in your Azure subscription.</span></span> <span data-ttu-id="5ce3e-133">Más típusú Azure riasztások, például Azure metrika riasztások, az Application Insights-riasztások és Naplóelemzési riasztások nem érinti ez a változás.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-133">Other types of Azure alerts, including Azure Monitor metric alerts, Application Insights alerts, and Log Analytics alerts are unaffected by this change.</span></span> <span data-ttu-id="5ce3e-134">Ha kérdése van, az alábbi megjegyzések fel.</span><span class="sxs-lookup"><span data-stu-id="5ce3e-134">If you have any questions, post in the comments below.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5ce3e-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ce3e-135">Next steps</span></span>

* <span data-ttu-id="5ce3e-136">További információ [műveletnapló](monitoring-overview-activity-logs.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-136">Learn more about [Activity Log](monitoring-overview-activity-logs.md)</span></span>
* <span data-ttu-id="5ce3e-137">Konfigurálása [napló Tevékenységriasztásokat Azure-portálon](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-137">Configure [Activity Log Alerts via Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="5ce3e-138">Konfigurálása [Tevékenységriasztásokat napló erőforrás-kezelő használatával](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-138">Configure [Activity Log Alerts via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
* <span data-ttu-id="5ce3e-139">Tekintse át a [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-139">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md)</span></span>
* <span data-ttu-id="5ce3e-140">További információ [szolgáltatáshoz értesítést](monitoring-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-140">Learn more about [Service Notifications](monitoring-service-notifications.md)</span></span>
* <span data-ttu-id="5ce3e-141">További információ [művelet csoportok](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="5ce3e-141">Learn more about [Action Groups](monitoring-action-groups.md)</span></span>
