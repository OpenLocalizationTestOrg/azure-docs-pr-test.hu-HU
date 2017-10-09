---
title: "Hálózati biztonsági csoport folyamat aaaManage naplózza az Azure hálózati figyelőt - REST API |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan naplózza az toomanage hálózati biztonsági csoport folyamata az Azure hálózati figyelőt REST API-hoz"
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
ms.openlocfilehash: be81e35f4d01c67efef99773e9b4e2ae4b8e209e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="aaf0a-103">Hálózati biztonsági csoport konfigurálásával folyamata naplók REST API használatával</span><span class="sxs-lookup"><span data-stu-id="aaf0a-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="aaf0a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="aaf0a-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="aaf0a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaf0a-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="aaf0a-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="aaf0a-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="aaf0a-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="aaf0a-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="aaf0a-108">REST API</span><span class="sxs-lookup"><span data-stu-id="aaf0a-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="aaf0a-109">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="aaf0a-110">A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="aaf0a-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="aaf0a-111">Before you begin</span></span>

<span data-ttu-id="aaf0a-112">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="aaf0a-113">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="aaf0a-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="aaf0a-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="aaf0a-115">Hálózati figyelő REST API-hívások hello hello kérelem URI hello hálózati figyelőt nem hello olyan erőforrásokat tartalmaz, a hello diagnosztikai műveleteket hajt végre hello erőforráscsoport erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-115">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="aaf0a-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="aaf0a-116">Scenario</span></span>

<span data-ttu-id="aaf0a-117">a cikkben szereplő hello forgatókönyv bemutatja, hogyan tooenable, tiltsa le és lekérdezés flow hello REST API használatával a naplókat.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-117">hello scenario covered in this article shows you how tooenable, disable, and query flow logs using hello REST API.</span></span> <span data-ttu-id="aaf0a-118">toolearn hálózati biztonsági csoport folyamat loggings, kapcsolatos további információkért látogasson el [hálózati biztonsági csoport adatfolyam-naplózás – áttekintés](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aaf0a-118">toolearn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="aaf0a-119">Ebben a forgatókönyvben a tartalma:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-119">In this scenario, you will:</span></span>

* <span data-ttu-id="aaf0a-120">Attribútumfolyam naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aaf0a-120">Enable flow logs</span></span>
* <span data-ttu-id="aaf0a-121">Tiltsa le a folyamat Naplók</span><span class="sxs-lookup"><span data-stu-id="aaf0a-121">Disable flow logs</span></span>
* <span data-ttu-id="aaf0a-122">Lekérdezés folyamata naplók állapota</span><span class="sxs-lookup"><span data-stu-id="aaf0a-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="aaf0a-123">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="aaf0a-123">Log in with ARMClient</span></span>

<span data-ttu-id="aaf0a-124">Jelentkezzen be a Azure hitelesítő adataival tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-124">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="aaf0a-125">Elemzések szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="aaf0a-125">Register Insights provider</span></span>

<span data-ttu-id="aaf0a-126">Ahhoz, hogy a folyamat sikeres legyen, hello az naplózási toowork **Microsoft.Insights** szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-126">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="aaf0a-127">Ha nem biztos benne, hogy hello **Microsoft.Insights** szolgáltató regisztrált, futtatási hello következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-127">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="aaf0a-128">Engedélyezze a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="aaf0a-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="aaf0a-129">hello parancs tooenable folyamata naplók hello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-129">hello command tooenable flow logs is shown in hello following example:</span></span>

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

<span data-ttu-id="aaf0a-130">hello választ adott vissza az előző hello például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-130">hello response returned from hello preceding example is as follows:</span></span>

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

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="aaf0a-131">Tiltsa le a hálózati biztonsági csoport folyamata naplók</span><span class="sxs-lookup"><span data-stu-id="aaf0a-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="aaf0a-132">A következő példa toodisable folyamat használata hello naplózza.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-132">Use hello following example toodisable flow logs.</span></span> <span data-ttu-id="aaf0a-133">hello tekintendő, amely hello ugyanaz, mint engedélyezése folyamata naplókat, kivéve **hamis** hello engedélyezve tulajdonság be van állítva.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-133">hello call is hello same as enabling flow logs, except **false** is set for hello enabled property.</span></span>

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

<span data-ttu-id="aaf0a-134">hello választ adott vissza az előző hello például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-134">hello response returned from hello preceding example is as follows:</span></span>

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

## <a name="query-flow-logs"></a><span data-ttu-id="aaf0a-135">Lekérdezési folyamat naplókat</span><span class="sxs-lookup"><span data-stu-id="aaf0a-135">Query flow logs</span></span>

<span data-ttu-id="aaf0a-136">a következő lekérdezések hello folyamat állapotának REST-hívást hello naplózza a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-136">hello following REST call queries hello status of flow logs on a Network Security Group.</span></span>

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

<span data-ttu-id="aaf0a-137">hello az alábbiakban látható egy példa hello válasza:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-137">hello following is an example of hello response returned:</span></span>

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

## <a name="download-a-flow-log"></a><span data-ttu-id="aaf0a-138">A folyamat napló letöltése</span><span class="sxs-lookup"><span data-stu-id="aaf0a-138">Download a flow log</span></span>

<span data-ttu-id="aaf0a-139">a folyamat napló hello tárolási helye a létrehozásakor van definiálva.</span><span class="sxs-lookup"><span data-stu-id="aaf0a-139">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="aaf0a-140">Egy eszköz tooaccess ezen folyamat mentett naplók tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="aaf0a-140">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="aaf0a-141">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="aaf0a-141">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="aaf0a-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aaf0a-142">Next steps</span></span>

<span data-ttu-id="aaf0a-143">Ismerje meg, hogyan túl[a powerbi-jal az NSG folyamata naplók megjelenítése](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="aaf0a-143">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="aaf0a-144">Ismerje meg, hogyan túl[jelenítheti meg az NSG folyamata naplók nyílt forráskódú eszközökkel](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="aaf0a-144">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
