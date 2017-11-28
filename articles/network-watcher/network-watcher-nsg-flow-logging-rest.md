---
title: "Hálózati biztonsági csoport kezelése folyamata naplók Azure hálózati figyelőt - REST API-t és |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan Azure hálózati figyelőt REST API-t a hálózati biztonsági csoport folyamata kezelése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: c89a2ab4c39978771c940a819493b4e2283d5f9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="d2a65-103">Hálózati biztonsági csoport konfigurálásával folyamata naplók REST API használatával</span><span class="sxs-lookup"><span data-stu-id="d2a65-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d2a65-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2a65-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="d2a65-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2a65-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="d2a65-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d2a65-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="d2a65-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d2a65-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="d2a65-108">REST API</span><span class="sxs-lookup"><span data-stu-id="d2a65-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="d2a65-109">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi, hogy tekintse meg a hálózati biztonsági csoporton keresztül bemenő és kimenő IP-forgalom hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="d2a65-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="d2a65-110">A folyamat naplók json formátumban vannak megírva, és a kimenő és bejövő forgalom alapon egy szabályt, a hálózati adapter a folyamat vonatkozik, a folyamat (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), 5 rekordos információk megjelenítése, és ha a forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="d2a65-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d2a65-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d2a65-111">Before you begin</span></span>

<span data-ttu-id="d2a65-112">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="d2a65-112">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="d2a65-113">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="d2a65-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="d2a65-114">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="d2a65-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="d2a65-115">A hálózati figyelő REST API-hívásokat az erőforráscsoport neve a kérés URI a hálózati figyelőt tartalmazó erőforráscsoportot nem az erőforrások hajt a diagnosztikai műveletek a.</span><span class="sxs-lookup"><span data-stu-id="d2a65-115">For Network Watcher REST API calls the resource group name in the request URI is the resource group that contains the Network Watcher, not the resources you are performing the diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="d2a65-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d2a65-116">Scenario</span></span>

<span data-ttu-id="d2a65-117">A cikkben szereplő forgatókönyv bemutatja, hogyan engedélyezheti, letilthatja és lekérdezési folyamat naplók a REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="d2a65-117">The scenario covered in this article shows you how to enable, disable, and query flow logs using the REST API.</span></span> <span data-ttu-id="d2a65-118">Hálózati biztonsági csoport folyamat loggings kapcsolatos további információkért látogasson el a [hálózati biztonsági csoport adatfolyam-naplózás – áttekintés](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2a65-118">To learn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="d2a65-119">Ebben a forgatókönyvben a tartalma:</span><span class="sxs-lookup"><span data-stu-id="d2a65-119">In this scenario, you will:</span></span>

* <span data-ttu-id="d2a65-120">Attribútumfolyam naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d2a65-120">Enable flow logs</span></span>
* <span data-ttu-id="d2a65-121">Tiltsa le a folyamat Naplók</span><span class="sxs-lookup"><span data-stu-id="d2a65-121">Disable flow logs</span></span>
* <span data-ttu-id="d2a65-122">Lekérdezés folyamata naplók állapota</span><span class="sxs-lookup"><span data-stu-id="d2a65-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="d2a65-123">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="d2a65-123">Log in with ARMClient</span></span>

<span data-ttu-id="d2a65-124">Jelentkezzen be a Azure hitelesítő adataival armclient.</span><span class="sxs-lookup"><span data-stu-id="d2a65-124">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="d2a65-125">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d2a65-125">Register Insights provider</span></span>

<span data-ttu-id="d2a65-126">Ahhoz, hogy sikeresen, működjön naplózás folyamata a **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d2a65-126">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="d2a65-127">Ha nem tudja, ha a **Microsoft.Insights** szolgáltató regisztrálva van, futtassa a következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d2a65-127">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="d2a65-128">Engedélyezze a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="d2a65-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="d2a65-129">A parancsot a folyamat naplók engedélyezéséhez az alábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="d2a65-129">The command to enable flow logs is shown in the following example:</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="d2a65-130">Az előző példában válasza a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d2a65-130">The response returned from the preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="d2a65-131">Tiltsa le a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="d2a65-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="d2a65-132">Az alábbi példa használatával tiltsa le a folyamat naplókat.</span><span class="sxs-lookup"><span data-stu-id="d2a65-132">Use the following example to disable flow logs.</span></span> <span data-ttu-id="d2a65-133">A hívás megegyezik a folyamat naplók engedélyezése kivéve **hamis** az engedélyezett tulajdonság be van állítva.</span><span class="sxs-lookup"><span data-stu-id="d2a65-133">The call is the same as enabling flow logs, except **false** is set for the enabled property.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="d2a65-134">Az előző példában válasza a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d2a65-134">The response returned from the preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="query-flow-logs"></a><span data-ttu-id="d2a65-135">Lekérdezési folyamat naplókat</span><span class="sxs-lookup"><span data-stu-id="d2a65-135">Query flow logs</span></span>

<span data-ttu-id="d2a65-136">A következő REST-hívást lekérdezések a folyamat állapotának naplózza a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="d2a65-136">The following REST call queries the status of flow logs on a Network Security Group.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="d2a65-137">A következő egy példa válasza:</span><span class="sxs-lookup"><span data-stu-id="d2a65-137">The following is an example of the response returned:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="download-a-flow-log"></a><span data-ttu-id="d2a65-138">A folyamat napló letöltése</span><span class="sxs-lookup"><span data-stu-id="d2a65-138">Download a flow log</span></span>

<span data-ttu-id="d2a65-139">A folyamat napló tárolási helye a létrehozásakor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="d2a65-139">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="d2a65-140">A folyamat naplók tárfiókba menti eléréséhez eszköz a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="d2a65-140">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="d2a65-141">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="d2a65-141">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="d2a65-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2a65-142">Next steps</span></span>

<span data-ttu-id="d2a65-143">Megtudhatja, hogyan [a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="d2a65-143">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="d2a65-144">Megtudhatja, hogyan [jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="d2a65-144">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
