---
title: "Virtuális hálózati átjáró és az Azure hálózati figyelőt - REST használó kapcsolatok hibáinak elhárítása |} Microsoft Docs"
description: "Ezen a lapon ismerteti a virtuális hálózati átjárók és az Azure hálózati figyelőt használatával REST-kapcsolatok hibáinak elhárítása"
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
ms.openlocfilehash: bc61be74d85a309c158716460b918baaf4fa94dc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a><span data-ttu-id="88fee-103">Virtuális hálózati átjáró és az Azure hálózati figyelőt alkalmazó kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-103">Troubleshoot Virtual Network gateway and Connections using Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="88fee-104">Portal</span><span class="sxs-lookup"><span data-stu-id="88fee-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="88fee-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="88fee-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="88fee-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="88fee-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="88fee-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="88fee-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="88fee-108">REST API</span><span class="sxs-lookup"><span data-stu-id="88fee-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="88fee-109">Hálózati figyelőt sok képességeket biztosít, a hálózati erőforrások az Azure-ban ismertetése vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="88fee-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="88fee-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="88fee-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="88fee-111">Erőforrások hibaelhárítása hívható a portálon, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="88fee-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="88fee-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat állapotát, és visszaadja az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="88fee-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="88fee-113">Ez a cikk végigvezeti Önt a különböző felügyeleti feladatok, amelyek erőforrás hibaelhárítási aktuálisan elérhető.</span><span class="sxs-lookup"><span data-stu-id="88fee-113">This article takes you through the different management tasks that are currently available for resource troubleshooting.</span></span>

- [<span data-ttu-id="88fee-114">**A virtuális hálózati átjáró hibaelhárítása**</span><span class="sxs-lookup"><span data-stu-id="88fee-114">**Troubleshoot a Virtual Network gateway**</span></span>](#troubleshoot-a-virtual-network-gateway)
- [<span data-ttu-id="88fee-115">**Végezzen hibaelhárítást a kapcsolaton**</span><span class="sxs-lookup"><span data-stu-id="88fee-115">**Troubleshoot a Connection**</span></span>](#troubleshoot-connections)

## <a name="before-you-begin"></a><span data-ttu-id="88fee-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="88fee-116">Before you begin</span></span>

<span data-ttu-id="88fee-117">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="88fee-117">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="88fee-118">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="88fee-118">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="88fee-119">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="88fee-119">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="88fee-120">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="88fee-120">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="88fee-121">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="88fee-121">Overview</span></span>

<span data-ttu-id="88fee-122">Hálózati figyelő hibaelhárítási lehetővé teszi a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="88fee-122">Network Watcher troubleshooting provides the ability troubleshoot issues that arise with Virtual Network gateways and Connections.</span></span> <span data-ttu-id="88fee-123">Ha a kérelem a hibakeresési erőforrás, a naplók kérdez le, és megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="88fee-123">When a request is made to the resource troubleshooting, logs are querying and inspected.</span></span> <span data-ttu-id="88fee-124">Ha vizsgálat befejeződött, a rendszer visszairányítja az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="88fee-124">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="88fee-125">A hibaelhárítás API-kérések hosszúak futó kérések, amelyek vissza eredményt több percbe is beletelhet.</span><span class="sxs-lookup"><span data-stu-id="88fee-125">The troubleshoot API requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="88fee-126">Naplók egy tárolót, a storage-fiókok vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="88fee-126">Logs are stored in a container on a storage account.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="88fee-127">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="88fee-127">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a><span data-ttu-id="88fee-128">A virtuális hálózati átjáró hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-128">Troubleshoot a Virtual Network gateway</span></span>


### <a name="post-the-troubleshoot-request"></a><span data-ttu-id="88fee-129">POST kérelem kapcsolatos problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-129">POST the troubleshoot request</span></span>

<span data-ttu-id="88fee-130">Az alábbi példában egy virtuális hálózati átjáró állapotának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="88fee-130">The following example queries the status of a Virtual Network gateway.</span></span>

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

<span data-ttu-id="88fee-131">Mivel ez a művelet hosszú fut, a művelet és az URI lekérdezése, az eredmény eredmény abban az esetben a válaszfejlécet látható módon a következő választ az URI:</span><span class="sxs-lookup"><span data-stu-id="88fee-131">Since this operation is long running, the URI for querying the operation and the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="88fee-132">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="88fee-132">**Important Values**</span></span>

* <span data-ttu-id="88fee-133">**Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza az URI-lekérdezés az aszinkron művelet hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-133">**Azure-AsyncOperation** - This property contains the URI to query the Async troubleshoot operation</span></span>
* <span data-ttu-id="88fee-134">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="88fee-134">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="query-the-async-operation-for-completion"></a><span data-ttu-id="88fee-135">A lekérdezés az aszinkron művelet befejezésére</span><span class="sxs-lookup"><span data-stu-id="88fee-135">Query the async operation for completion</span></span>

<span data-ttu-id="88fee-136">A művelet URI lekérdezési használhatja a műveletnek az előrehaladását, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="88fee-136">Use the operations URI to query for the progress of the operation as seen in the following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="88fee-137">Bemutatja, amíg a művelet van folyamatban, a válasz **esetbejegyzések** az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="88fee-137">While the operation is in progress, the response shows **InProgress** as seen in the following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="88fee-138">A művelet befejezésekor állapota **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="88fee-138">When the operation is complete the status changes to **Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-the-results"></a><span data-ttu-id="88fee-139">Az eredmények visszakeresésére</span><span class="sxs-lookup"><span data-stu-id="88fee-139">Retrieve the results</span></span>

<span data-ttu-id="88fee-140">Miután visszaadott állapotát **sikeres**, a GET metódust hívni az operationresult adatokat a URI-t az eredmények visszakeresésére.</span><span class="sxs-lookup"><span data-stu-id="88fee-140">Once the status returned is **Succeeded**, call a GET Method on the operationResult URI to retrieve the results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="88fee-141">A következő válaszok tipikus csökkent választ adott vissza, amikor egy átjáró hibaelhárítási eredmények lekérdezéséről példák.</span><span class="sxs-lookup"><span data-stu-id="88fee-141">The following responses are examples of a typical degraded response returned when querying the results of troubleshooting a gateway.</span></span> <span data-ttu-id="88fee-142">Lásd: [az eredmények ismertetése](#understanding-the-results) pontosítása a válaszban szereplő tulajdonságok jelenti az eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="88fee-142">See [Understanding the results](#understanding-the-results) to get clarification on what the properties in the response mean.</span></span>

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by the expected resolution time, contact support",
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
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
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


## <a name="troubleshoot-connections"></a><span data-ttu-id="88fee-143">Kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-143">Troubleshoot Connections</span></span>

<span data-ttu-id="88fee-144">A következő példa egy kapcsolat állapotának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="88fee-144">The following example queries the status of a Connection.</span></span>

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
> <span data-ttu-id="88fee-145">A hibaelhárítás művelet nem futtatható párhuzamosan kapcsolatot és a megfelelő átjárók.</span><span class="sxs-lookup"><span data-stu-id="88fee-145">The troubleshoot operation cannot be run in parallel on a Connection and its corresponding gateways.</span></span> <span data-ttu-id="88fee-146">A művelet elvégzése után az előző erőforrás futtatására.</span><span class="sxs-lookup"><span data-stu-id="88fee-146">The operation must complete prior to running it on the previous resource.</span></span>

<span data-ttu-id="88fee-147">Mivel ez egy hosszú ideig futó tranzakció a válaszfejlécet látható módon a következő válasz a lekérdezésre, a művelet és az eredmény URI-JÁNAK URI adott vissza:</span><span class="sxs-lookup"><span data-stu-id="88fee-147">Since this is a long running transaction, in the response header, the URI for querying the operation and the URI for the result is returned as shown in the following response:</span></span>

<span data-ttu-id="88fee-148">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="88fee-148">**Important Values**</span></span>

* <span data-ttu-id="88fee-149">**Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza az URI-lekérdezés az aszinkron művelet hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="88fee-149">**Azure-AsyncOperation** - This property contains the URI to query the Async troubleshoot operation</span></span>
* <span data-ttu-id="88fee-150">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="88fee-150">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="query-the-async-operation-for-completion"></a><span data-ttu-id="88fee-151">A lekérdezés az aszinkron művelet befejezésére</span><span class="sxs-lookup"><span data-stu-id="88fee-151">Query the async operation for completion</span></span>

<span data-ttu-id="88fee-152">A művelet URI lekérdezési használhatja a műveletnek az előrehaladását, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="88fee-152">Use the operations URI to query for the progress of the operation as seen in the following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="88fee-153">Bemutatja, amíg a művelet van folyamatban, a válasz **esetbejegyzések** az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="88fee-153">While the operation is in progress, the response shows **InProgress** as seen in the following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="88fee-154">A művelet végeztével állapota **sikeres**.</span><span class="sxs-lookup"><span data-stu-id="88fee-154">When the operation is complete, the status changes to **Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

<span data-ttu-id="88fee-155">A következő válaszok példák vissza, ha a kapcsolat hibaelhárítási eredmények lekérdezéséről tipikus választ.</span><span class="sxs-lookup"><span data-stu-id="88fee-155">The following responses are examples of a typical response returned when querying the results of troubleshooting a Connection.</span></span>

### <a name="retrieve-the-results"></a><span data-ttu-id="88fee-156">Az eredmények visszakeresésére</span><span class="sxs-lookup"><span data-stu-id="88fee-156">Retrieve the results</span></span>

<span data-ttu-id="88fee-157">Miután visszaadott állapotát **sikeres**, a GET metódust hívni az operationresult adatokat a URI-t az eredmények visszakeresésére.</span><span class="sxs-lookup"><span data-stu-id="88fee-157">Once the status returned is **Succeeded**, call a GET Method on the operationResult URI to retrieve the results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="88fee-158">A következő válaszok példák vissza, ha a kapcsolat hibaelhárítási eredmények lekérdezéséről tipikus választ.</span><span class="sxs-lookup"><span data-stu-id="88fee-158">The following responses are examples of a typical response returned when querying the results of troubleshooting a Connection.</span></span>

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time the gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while the Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If the condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting the VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by the expected resolution time, contact support",
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
          "actionText": "If you are still experience problems with the VPN gateway, please try resetting the VPN gateway.",
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

## <a name="understanding-the-results"></a><span data-ttu-id="88fee-159">Az eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="88fee-159">Understanding the results</span></span>

<span data-ttu-id="88fee-160">A művelet szöveg általános útmutatást biztosít a probléma megoldására.</span><span class="sxs-lookup"><span data-stu-id="88fee-160">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="88fee-161">Is művelet az a probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="88fee-161">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="88fee-162">Abban az esetben nincs további útmutatás, ha a válasz biztosít nyissa meg a támogatási esetet URL-címét.</span><span class="sxs-lookup"><span data-stu-id="88fee-162">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="88fee-163">A válasz és tartalmát képező tulajdonságaival kapcsolatos további információkért látogasson el a [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="88fee-163">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="88fee-164">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="88fee-164">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="88fee-165">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="88fee-165">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="88fee-166">Tártallózó további információt itt található: a következő hivatkozásra: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="88fee-166">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="88fee-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88fee-167">Next steps</span></span>

<span data-ttu-id="88fee-168">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági szabályok, amelyek lehet, hogy a szóban forgó.</span><span class="sxs-lookup"><span data-stu-id="88fee-168">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
