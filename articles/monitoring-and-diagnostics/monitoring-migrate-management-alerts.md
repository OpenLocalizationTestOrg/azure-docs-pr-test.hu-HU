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
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a><span data-ttu-id="10f44-104">Telepítse át az Azure felügyeleti események tooActivity napló riasztások-riasztások</span><span class="sxs-lookup"><span data-stu-id="10f44-104">Migrate Azure alerts on management events tooActivity Log alerts</span></span>


> [!WARNING]
> <span data-ttu-id="10f44-105">A felügyeleti események riasztások ki lesz kapcsolva a következőnél október 1.</span><span class="sxs-lookup"><span data-stu-id="10f44-105">Alerts on management events will be turned off on or after October 1.</span></span> <span data-ttu-id="10f44-106">Ha ezek a riasztások, és át őket, ha igen, használja a hello irányban toounderstand alatt.</span><span class="sxs-lookup"><span data-stu-id="10f44-106">Use hello directions below toounderstand if you have these alerts and migrate them if so.</span></span>
>
> 

## <a name="what-is-changing"></a><span data-ttu-id="10f44-107">Mi van módosítása</span><span class="sxs-lookup"><span data-stu-id="10f44-107">What is changing</span></span>

<span data-ttu-id="10f44-108">Azure Monitor (korábban Azure Insights) kínált egy funkció toocreate kijelentkezés felügyeleti események elindul, és értesítések tooa webhook URL-cím vagy az e-mail címek generált riasztást.</span><span class="sxs-lookup"><span data-stu-id="10f44-108">Azure Monitor (formerly Azure Insights) offered a capability toocreate an alert that triggered off of management events and generated notifications tooa webhook URL or email addresses.</span></span> <span data-ttu-id="10f44-109">Előfordulhat, hogy létrehozta az egyik riasztást küld, az alábbi módszerek bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="10f44-109">You may have created one of these alerts any of these ways:</span></span>
* <span data-ttu-id="10f44-110">Hello Azure-portál a bizonyos típusú erőforrások, a figyelés -> riasztások hozzáadása riasztás, ha "Riasztás" értéke túl -> "Események"</span><span class="sxs-lookup"><span data-stu-id="10f44-110">In hello Azure portal for certain resource types, under Monitoring -> Alerts -> Add Alert, where “Alert on” is set too“Events”</span></span>
* <span data-ttu-id="10f44-111">Futó hello Add-AzureRmLogAlertRule PowerShell-parancsmaggal</span><span class="sxs-lookup"><span data-stu-id="10f44-111">By running hello Add-AzureRmLogAlertRule PowerShell cmdlet</span></span>
* <span data-ttu-id="10f44-112">Közvetlenül a [riasztási REST API hello](http://docs.microsoft.com/rest/api/monitor/alertrules) rendelkező odata.type = "ManagementEventRuleCondition" és a dataSource.odata.type = "RuleManagementEventDataSource"</span><span class="sxs-lookup"><span data-stu-id="10f44-112">By directly using [hello alert REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) with odata.type = “ManagementEventRuleCondition” and dataSource.odata.type = “RuleManagementEventDataSource”</span></span>
 
<span data-ttu-id="10f44-113">hello következő PowerShell-parancsfájl listáját adja vissza az összes riasztás a felügyeleti események, amelyek az előfizetést, valamint a minden riasztás beállítása hello feltételek vannak.</span><span class="sxs-lookup"><span data-stu-id="10f44-113">hello following PowerShell script returns a list of all alerts on management events that you have in your subscription, as well as hello conditions set on each alert.</span></span>

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

<span data-ttu-id="10f44-114">Felügyeleti események rendelkezik nincs riasztás, ha a fenti hello PowerShell-parancsmag kimeneteként az alábbihoz hasonló figyelmeztetés üzenetből:</span><span class="sxs-lookup"><span data-stu-id="10f44-114">If you have no alerts on management events, hello PowerShell cmdlet above will output a series of warning messages like this one:</span></span>

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

<span data-ttu-id="10f44-115">A figyelmeztető üzenetek figyelmen kívül hagyható.</span><span class="sxs-lookup"><span data-stu-id="10f44-115">These warning messages can be ignored.</span></span> <span data-ttu-id="10f44-116">Ha a felügyeleti események rendelkezik riasztások, hello PowerShell parancsmag kimenete a következőképpen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="10f44-116">If you do have alerts on management events, hello output of this PowerShell cmdlet will look like this:</span></span>

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

<span data-ttu-id="10f44-117">Minden riasztás szaggatott vonal választja el egymástól, és részleteket tartalmaznak hello riasztás és hello adott szabály figyelt hello erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="10f44-117">Each alert is separated by a dashed line and details include hello resource ID of hello alert and hello specific rule being monitored.</span></span>

<span data-ttu-id="10f44-118">Ez a funkció túl lett állítva[Azure tevékenység napló riasztások](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="10f44-118">This functionality has been transitioned too[Azure Monitor Activity Log Alerts](monitoring-activity-log-alerts.md).</span></span> <span data-ttu-id="10f44-119">Az új értesítések lehetővé teszik a tooset tevékenységnapló események vonatkozó feltétellel, és értesítést kaphat, ha egy új esemény megfelel hello feltétel.</span><span class="sxs-lookup"><span data-stu-id="10f44-119">These new alerts enable you tooset a condition on Activity Log events and receive a notification when a new event matches hello condition.</span></span> <span data-ttu-id="10f44-120">Számos fejlesztést tartalmaz a riasztásokat a management események is biztosítanak:</span><span class="sxs-lookup"><span data-stu-id="10f44-120">They also offer several improvements from alerts on management events:</span></span>
* <span data-ttu-id="10f44-121">A csoport értesítési címzettek ("műveletek") között számos riasztásokról felhasználhatja [művelet csoportok](monitoring-action-groups.md), módosítása, akik egy riasztást kell fogadnia hello összetettsége csökkentése.</span><span class="sxs-lookup"><span data-stu-id="10f44-121">You can reuse your group of notification recipients (“actions”) across many alerts using [Action Groups](monitoring-action-groups.md), reducing hello complexity of changing who should receive an alert.</span></span>
* <span data-ttu-id="10f44-122">Közvetlenül a telefonján a SMS művelet csoportokkal is kap értesítést.</span><span class="sxs-lookup"><span data-stu-id="10f44-122">You can receive a notification directly on your phone using SMS with Action Groups.</span></span>
* <span data-ttu-id="10f44-123">Is [Tevékenységriasztásokat napló létrehozása a Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="10f44-123">You can [create Activity Log Alerts with Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
* <span data-ttu-id="10f44-124">Létrehozhat feltételek nagyobb rugalmasságot és összetettsége toomeet igény szerinti.</span><span class="sxs-lookup"><span data-stu-id="10f44-124">You can create conditions with greater flexibility and complexity toomeet your specific needs.</span></span>
* <span data-ttu-id="10f44-125">Értesítések a gyorsabban érkeznek.</span><span class="sxs-lookup"><span data-stu-id="10f44-125">Notifications are delivered more quickly.</span></span>
 
## <a name="how-toomigrate"></a><span data-ttu-id="10f44-126">Hogyan toomigrate</span><span class="sxs-lookup"><span data-stu-id="10f44-126">How toomigrate</span></span>
 
<span data-ttu-id="10f44-127">Új tevékenység napló értesítés toocreate lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="10f44-127">toocreate a new Activity Log Alert, you can either:</span></span>
* <span data-ttu-id="10f44-128">Hajtsa végre a [az útmutató, hogyan toocreate a riasztás hello Azure-portálon](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-128">Follow [our guide on how toocreate an alert in hello Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="10f44-129">Ismerje meg, hogyan túl[Resource Manager-sablon használatával riasztás létrehozása](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-129">Learn how too[create an alert using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
 
<span data-ttu-id="10f44-130">Riasztások a korábban létrehozott felügyeleti események nem lesz automatikusan áttelepített tooActivity napló riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="10f44-130">Alerts on management events that you have previously created will not be automatically migrated tooActivity Log Alerts.</span></span> <span data-ttu-id="10f44-131">A jelenleg konfigurált, és manuálisan hozza létre őket tevékenység napló riasztásként felügyeleti események PowerShell parancsfájl toolist hello riasztások megelőző toouse hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="10f44-131">You need toouse hello preceding PowerShell script toolist hello alerts on management events that you currently have configured and manually recreate them as Activity Log Alerts.</span></span> <span data-ttu-id="10f44-132">A október 1, amely után riasztások felügyeleti események a már nem lesznek láthatók az Azure-előfizetése előtt kell végezni.</span><span class="sxs-lookup"><span data-stu-id="10f44-132">This must be done before October 1, after which alerts on management events will no longer be visible in your Azure subscription.</span></span> <span data-ttu-id="10f44-133">Más típusú Azure riasztások, például Azure metrika riasztások, az Application Insights-riasztások és Naplóelemzési riasztások nem érinti ez a változás.</span><span class="sxs-lookup"><span data-stu-id="10f44-133">Other types of Azure alerts, including Azure Monitor metric alerts, Application Insights alerts, and Log Analytics alerts are unaffected by this change.</span></span> <span data-ttu-id="10f44-134">Ha kérdése van, az alábbi hello megjegyzések fel.</span><span class="sxs-lookup"><span data-stu-id="10f44-134">If you have any questions, post in hello comments below.</span></span>


## <a name="next-steps"></a><span data-ttu-id="10f44-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10f44-135">Next steps</span></span>

* <span data-ttu-id="10f44-136">További információ [műveletnapló](monitoring-overview-activity-logs.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-136">Learn more about [Activity Log](monitoring-overview-activity-logs.md)</span></span>
* <span data-ttu-id="10f44-137">Konfigurálása [napló Tevékenységriasztásokat Azure-portálon](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-137">Configure [Activity Log Alerts via Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="10f44-138">Konfigurálása [Tevékenységriasztásokat napló erőforrás-kezelő használatával](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-138">Configure [Activity Log Alerts via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
* <span data-ttu-id="10f44-139">Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-139">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md)</span></span>
* <span data-ttu-id="10f44-140">További információ [szolgáltatáshoz értesítést](monitoring-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-140">Learn more about [Service Notifications](monitoring-service-notifications.md)</span></span>
* <span data-ttu-id="10f44-141">További információ [művelet csoportok](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="10f44-141">Learn more about [Action Groups](monitoring-action-groups.md)</span></span>
