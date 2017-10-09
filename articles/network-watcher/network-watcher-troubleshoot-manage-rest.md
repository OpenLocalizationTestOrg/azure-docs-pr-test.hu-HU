---
title: "Virtuális hálózati átjáró aaaTroubleshoot és kapcsolatok használata Azure hálózati figyelőt - REST |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan REST-tootroubleshoot virtuális hálózati átjárók és az Azure hálózati figyelőt használatával kapcsolatokhoz"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: cc89b46643fdbfefe53727b45d6b7d06914b58a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a><span data-ttu-id="08e6d-103">Virtuális hálózati átjáró és az Azure hálózati figyelőt alkalmazó kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-103">Troubleshoot Virtual Network gateway and Connections using Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="08e6d-104">Portál</span><span class="sxs-lookup"><span data-stu-id="08e6d-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="08e6d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08e6d-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="08e6d-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08e6d-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="08e6d-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="08e6d-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="08e6d-108">REST API</span><span class="sxs-lookup"><span data-stu-id="08e6d-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="08e6d-109">Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="08e6d-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="08e6d-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="08e6d-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="08e6d-111">Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="08e6d-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="08e6d-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="08e6d-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="08e6d-113">Ez a cikk végigvezeti a erőforrás hibaelhárítási aktuálisan elérhető különböző felügyeleti feladatok hello.</span><span class="sxs-lookup"><span data-stu-id="08e6d-113">This article takes you through hello different management tasks that are currently available for resource troubleshooting.</span></span>

- [<span data-ttu-id="08e6d-114">**A virtuális hálózati átjáró hibaelhárítása**</span><span class="sxs-lookup"><span data-stu-id="08e6d-114">**Troubleshoot a Virtual Network gateway**</span></span>](#troubleshoot-a-virtual-network-gateway)
- [<span data-ttu-id="08e6d-115">**Végezzen hibaelhárítást a kapcsolaton**</span><span class="sxs-lookup"><span data-stu-id="08e6d-115">**Troubleshoot a Connection**</span></span>](#troubleshoot-connections)

## <a name="before-you-begin"></a><span data-ttu-id="08e6d-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="08e6d-116">Before you begin</span></span>

<span data-ttu-id="08e6d-117">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="08e6d-117">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="08e6d-118">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="08e6d-118">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="08e6d-119">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="08e6d-119">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="08e6d-120">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="08e6d-120">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="08e6d-121">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="08e6d-121">Overview</span></span>

<span data-ttu-id="08e6d-122">Hálózati figyelő hibaelhárítási hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="08e6d-122">Network Watcher troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network gateways and Connections.</span></span> <span data-ttu-id="08e6d-123">Ha a kérelem toohello erőforrás hibakeresési, naplók kérdez le, és megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="08e6d-123">When a request is made toohello resource troubleshooting, logs are querying and inspected.</span></span> <span data-ttu-id="08e6d-124">Ha vizsgálat befejeződött, hello eredmény akkor minősül.</span><span class="sxs-lookup"><span data-stu-id="08e6d-124">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="08e6d-125">hello hibaelhárítása API kérelmek hosszúak futó kérelmek, amihez több több percig tooreturn eredményt.</span><span class="sxs-lookup"><span data-stu-id="08e6d-125">hello troubleshoot API requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="08e6d-126">Naplók egy tárolót, a storage-fiókok vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="08e6d-126">Logs are stored in a container on a storage account.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="08e6d-127">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="08e6d-127">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a><span data-ttu-id="08e6d-128">A virtuális hálózati átjáró hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-128">Troubleshoot a Virtual Network gateway</span></span>


### <a name="post-hello-troubleshoot-request"></a><span data-ttu-id="08e6d-129">POST hello kérelem hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-129">POST hello troubleshoot request</span></span>

<span data-ttu-id="08e6d-130">a következő példa lekérdezések hello állapotát a virtuális hálózati átjáró hello.</span><span class="sxs-lookup"><span data-stu-id="08e6d-130">hello following example queries hello status of a Virtual Network gateway.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

<span data-ttu-id="08e6d-131">Mivel ez a művelet hosszú fut, URI hello hello művelet lekérdezi és hello URI hello eredménye az eredmény abban az esetben hello válaszfejléc hello válasz a következő ábrán:</span><span class="sxs-lookup"><span data-stu-id="08e6d-131">Since this operation is long running, hello URI for querying hello operation and hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="08e6d-132">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="08e6d-132">**Important Values**</span></span>

* <span data-ttu-id="08e6d-133">**Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza hello URI tooquery hello aszinkron művelet hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-133">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="08e6d-134">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="08e6d-134">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="08e6d-135">Lekérdezési hello aszinkron művelet befejezésére</span><span class="sxs-lookup"><span data-stu-id="08e6d-135">Query hello async operation for completion</span></span>

<span data-ttu-id="08e6d-136">Hello műveletek URI tooquery használhatja hello művelet előrehaladását hello hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="08e6d-136">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="08e6d-137">Amíg hello művelet van folyamatban, hello válasz látható **esetbejegyzések** hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="08e6d-137">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="08e6d-138">Hello művelet esetén teljes hello állapotmódosítások túl**sikeres**.</span><span class="sxs-lookup"><span data-stu-id="08e6d-138">When hello operation is complete hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-hello-results"></a><span data-ttu-id="08e6d-139">Hello eredmények beolvasásához</span><span class="sxs-lookup"><span data-stu-id="08e6d-139">Retrieve hello results</span></span>

<span data-ttu-id="08e6d-140">Hello állapot visszaadása van **sikeres**, a GET metódust hívni hello operationresult adatokat a URI tooretrieve hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="08e6d-140">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="08e6d-141">hello következő válaszok példák egy tipikus csökkent válasza hello eredménye egy átjáró hibaelhárítási lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="08e6d-141">hello following responses are examples of a typical degraded response returned when querying hello results of troubleshooting a gateway.</span></span> <span data-ttu-id="08e6d-142">Lásd: [hello eredmények ismertetése](#understanding-the-results) tooget tisztázása milyen hello tulajdonságai a hello válasz közepét.</span><span class="sxs-lookup"><span data-stu-id="08e6d-142">See [Understanding hello results](#understanding-the-results) tooget clarification on what hello properties in hello response mean.</span></span>

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by hello expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a><span data-ttu-id="08e6d-143">Kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-143">Troubleshoot Connections</span></span>

<span data-ttu-id="08e6d-144">a következő példa lekérdezések hello kapcsolat állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="08e6d-144">hello following example queries hello status of a Connection.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

> [!NOTE]
> <span data-ttu-id="08e6d-145">hello hibaelhárítása művelet nem futtatható egyidejűleg kapcsolatot és a megfelelő átjárók.</span><span class="sxs-lookup"><span data-stu-id="08e6d-145">hello troubleshoot operation cannot be run in parallel on a Connection and its corresponding gateways.</span></span> <span data-ttu-id="08e6d-146">hello műveletet kell végeznie az előzetes toorunning azt hello előző erőforráson.</span><span class="sxs-lookup"><span data-stu-id="08e6d-146">hello operation must complete prior toorunning it on hello previous resource.</span></span>

<span data-ttu-id="08e6d-147">Mivel ez egy hosszú ideig futó tranzakció hello válaszfejléc, hello válasz a következő ábrán hello URI hello művelet és hello URI hello eredmény lekérdezése adott vissza:</span><span class="sxs-lookup"><span data-stu-id="08e6d-147">Since this is a long running transaction, in hello response header, hello URI for querying hello operation and hello URI for hello result is returned as shown in hello following response:</span></span>

<span data-ttu-id="08e6d-148">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="08e6d-148">**Important Values**</span></span>

* <span data-ttu-id="08e6d-149">**Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza hello URI tooquery hello aszinkron művelet hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="08e6d-149">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="08e6d-150">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="08e6d-150">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="08e6d-151">Lekérdezési hello aszinkron művelet befejezésére</span><span class="sxs-lookup"><span data-stu-id="08e6d-151">Query hello async operation for completion</span></span>

<span data-ttu-id="08e6d-152">Hello műveletek URI tooquery használhatja hello művelet előrehaladását hello hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="08e6d-152">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="08e6d-153">Amíg hello művelet van folyamatban, hello válasz látható **esetbejegyzések** hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="08e6d-153">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="08e6d-154">Hello művelet befejeződése után hello állapota túl**sikeres**.</span><span class="sxs-lookup"><span data-stu-id="08e6d-154">When hello operation is complete, hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

<span data-ttu-id="08e6d-155">hello következő válaszok példák vissza, ha a kapcsolat hibaelhárítási hello eredmények lekérdezéséről tipikus választ.</span><span class="sxs-lookup"><span data-stu-id="08e6d-155">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

### <a name="retrieve-hello-results"></a><span data-ttu-id="08e6d-156">Hello eredmények beolvasásához</span><span class="sxs-lookup"><span data-stu-id="08e6d-156">Retrieve hello results</span></span>

<span data-ttu-id="08e6d-157">Hello állapot visszaadása van **sikeres**, a GET metódust hívni hello operationresult adatokat a URI tooretrieve hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="08e6d-157">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="08e6d-158">hello következő válaszok példák vissza, ha a kapcsolat hibaelhárítási hello eredmények lekérdezéséről tipikus választ.</span><span class="sxs-lookup"><span data-stu-id="08e6d-158">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by hello expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-hello-results"></a><span data-ttu-id="08e6d-159">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="08e6d-159">Understanding hello results</span></span>

<span data-ttu-id="08e6d-160">hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást.</span><span class="sxs-lookup"><span data-stu-id="08e6d-160">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="08e6d-161">Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="08e6d-161">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="08e6d-162">Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="08e6d-162">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="08e6d-163">Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="08e6d-163">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="08e6d-164">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="08e6d-164">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="08e6d-165">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="08e6d-165">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="08e6d-166">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="08e6d-166">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="08e6d-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08e6d-167">Next steps</span></span>

<span data-ttu-id="08e6d-168">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="08e6d-168">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
