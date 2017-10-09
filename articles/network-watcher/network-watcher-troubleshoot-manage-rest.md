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
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a>Virtuális hálózati átjáró és az Azure hálózati figyelőt alkalmazó kapcsolatok hibáinak elhárítása

> [!div class="op_single_selector"]
> - [Portál](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban. Ezek a képességek egyik erőforrás hibaelhárítás. Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül. Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.

Ez a cikk végigvezeti a erőforrás hibaelhárítási aktuálisan elérhető különböző felügyeleti feladatok hello.

- [**A virtuális hálózati átjáró hibaelhárítása**](#troubleshoot-a-virtual-network-gateway)
- [**Végezzen hibaelhárítást a kapcsolaton**](#troubleshoot-connections)

## <a name="before-you-begin"></a>Előkészületek

ARMclient használt toocall hello REST API használatával PowerShell. ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)

Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.

Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Áttekintés

Hálózati figyelő hibaelhárítási hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához. Ha a kérelem toohello erőforrás hibakeresési, naplók kérdez le, és megvizsgálja. Ha vizsgálat befejeződött, hello eredmény akkor minősül. hello hibaelhárítása API kérelmek hosszúak futó kérelmek, amihez több több percig tooreturn eredményt. Naplók egy tárolót, a storage-fiókok vannak tárolva.

## <a name="log-in-with-armclient"></a>Jelentkezzen be ARMClient

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a>A virtuális hálózati átjáró hibaelhárítása


### <a name="post-hello-troubleshoot-request"></a>POST hello kérelem hibáinak elhárítása

a következő példa lekérdezések hello állapotát a virtuális hálózati átjáró hello.

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

Mivel ez a művelet hosszú fut, URI hello hello művelet lekérdezi és hello URI hello eredménye az eredmény abban az esetben hello válaszfejléc hello válasz a következő ábrán:

**Fontos értékek**

* **Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza hello URI tooquery hello aszinkron művelet hibáinak elhárítása
* **Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.

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

### <a name="query-hello-async-operation-for-completion"></a>Lekérdezési hello aszinkron művelet befejezésére

Hello műveletek URI tooquery használhatja hello művelet előrehaladását hello hello a következő példában látható módon:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

Amíg hello művelet van folyamatban, hello válasz látható **esetbejegyzések** hello a következő példában látható módon:

```json
{
  "status": "InProgress"
}
```

Hello művelet esetén teljes hello állapotmódosítások túl**sikeres**.

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-hello-results"></a>Hello eredmények beolvasásához

Hello állapot visszaadása van **sikeres**, a GET metódust hívni hello operationresult adatokat a URI tooretrieve hello eredmények.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

hello következő válaszok példák egy tipikus csökkent válasza hello eredménye egy átjáró hibaelhárítási lekérdezésekor. Lásd: [hello eredmények ismertetése](#understanding-the-results) tooget tisztázása milyen hello tulajdonságai a hello válasz közepét.

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


## <a name="troubleshoot-connections"></a>Kapcsolatok hibáinak elhárítása

a következő példa lekérdezések hello kapcsolat állapotának hello.

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
> hello hibaelhárítása művelet nem futtatható egyidejűleg kapcsolatot és a megfelelő átjárók. hello műveletet kell végeznie az előzetes toorunning azt hello előző erőforráson.

Mivel ez egy hosszú ideig futó tranzakció hello válaszfejléc, hello válasz a következő ábrán hello URI hello művelet és hello URI hello eredmény lekérdezése adott vissza:

**Fontos értékek**

* **Azure-aszinkron műveletek** – Ez a tulajdonság tartalmazza hello URI tooquery hello aszinkron művelet hibáinak elhárítása
* **Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.

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

### <a name="query-hello-async-operation-for-completion"></a>Lekérdezési hello aszinkron művelet befejezésére

Hello műveletek URI tooquery használhatja hello művelet előrehaladását hello hello a következő példában látható módon:

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

Amíg hello művelet van folyamatban, hello válasz látható **esetbejegyzések** hello a következő példában látható módon:

```json
{
  "status": "InProgress"
}
```

Hello művelet befejeződése után hello állapota túl**sikeres**.

```json
{
  "status": "Succeeded"
}
```

hello következő válaszok példák vissza, ha a kapcsolat hibaelhárítási hello eredmények lekérdezéséről tipikus választ.

### <a name="retrieve-hello-results"></a>Hello eredmények beolvasásához

Hello állapot visszaadása van **sikeres**, a GET metódust hívni hello operationresult adatokat a URI tooretrieve hello eredmények.

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

hello következő válaszok példák vissza, ha a kapcsolat hibaelhárítási hello eredmények lekérdezéséről tipikus választ.

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

## <a name="understanding-hello-results"></a>Hello eredmények ismertetése

hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást. Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást. Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.  Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)

A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz, amely használható a Tártallózó. Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)

## <a name="next-steps"></a>Következő lépések

Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.
