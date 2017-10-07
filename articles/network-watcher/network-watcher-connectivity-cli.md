---
title: "Azure hálózati figyelőt - Azure CLI 2.0 aaaCheck kapcsolatot |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse kapcsolatot ellenőrzi az Azure CLI 2.0 használatával hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: e94e0fad03fd36ebf4e1fdf9e3cfee934b289deb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="edbb2-103">Ellenőrizze a kapcsolatot az Azure hálózati figyelőt Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="edbb2-103">Check connectivity with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="edbb2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="edbb2-104">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="edbb2-105">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="edbb2-105">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="edbb2-106">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="edbb2-106">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="edbb2-107">Ismerje meg, hogyan hozhatók létre a toouse kapcsolat tooverify, ha egy virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="edbb2-107">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="edbb2-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="edbb2-108">Before you begin</span></span>

<span data-ttu-id="edbb2-109">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="edbb2-109">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="edbb2-110">Egy példány toocheck kapcsolat kívánt hálózati figyelőt hello régióban.</span><span class="sxs-lookup"><span data-stu-id="edbb2-110">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="edbb2-111">Virtuális gépek toocheck kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="edbb2-111">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="edbb2-112">Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="edbb2-112">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="edbb2-113">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="edbb2-113">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="edbb2-114">Hello előzetes funkció regisztrálása</span><span class="sxs-lookup"><span data-stu-id="edbb2-114">Register hello preview capability</span></span> 

<span data-ttu-id="edbb2-115">Kapcsolat ellenőrzése jelenleg toouse nyilvános előzetes verziójában ez a szolgáltatás toobe regisztrálni kell.</span><span class="sxs-lookup"><span data-stu-id="edbb2-115">Connectivity check is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="edbb2-116">toodo a, CLI minta a következő futtatási hello</span><span class="sxs-lookup"><span data-stu-id="edbb2-116">toodo this, run hello following CLI sample</span></span>

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

<span data-ttu-id="edbb2-117">tooverify hello a regisztráció sikeres volt, futtassa a következő parancsot a CLI hello:</span><span class="sxs-lookup"><span data-stu-id="edbb2-117">tooverify hello registration was successful, run hello following CLI command:</span></span>

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

<span data-ttu-id="edbb2-118">Hello szolgáltatás megfelelően lett-e regisztrálva, ha hello kimeneti meg kell felelnie a következő hello:</span><span class="sxs-lookup"><span data-stu-id="edbb2-118">If hello feature was properly registered, hello output should match hello following:</span></span> 

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowNetworkWatcherConnectivityCheck",
  "name": "Microsoft.Network/AllowNetworkWatcherConnectivityCheck",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
``` 

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="edbb2-119">Ellenőrizze a kapcsolat tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="edbb2-119">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="edbb2-120">Ebben a példában kapcsolat tooa cél virtuális gép ellenőrzi a 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="edbb2-120">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="edbb2-121">Példa</span><span class="sxs-lookup"><span data-stu-id="edbb2-121">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="edbb2-122">Válasz</span><span class="sxs-lookup"><span data-stu-id="edbb2-122">Response</span></span>

<span data-ttu-id="edbb2-123">válasz a következő hello hello előző példa származik.</span><span class="sxs-lookup"><span data-stu-id="edbb2-123">hello following response is from hello previous example.</span></span>  <span data-ttu-id="edbb2-124">A válaszban hello `ConnectionStatus` van **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="edbb2-124">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="edbb2-125">Láthatja, hogy az összes hello mintavételt küldése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="edbb2-125">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="edbb2-126">hello kapcsolat meghiúsult hello virtuális készülék felhasználó által konfigurált esedékes tooa `NetworkSecurityRule` nevű **UserRule_Port80**, konfigurált tooblock bejövő forgalmat a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="edbb2-126">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="edbb2-127">Ezek az információk használt tooresearch kapcsolódási problémák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="edbb2-127">This information can be used tooresearch connection issues.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a><span data-ttu-id="edbb2-128">Útválasztási problémák ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="edbb2-128">Validate routing issues</span></span>

<span data-ttu-id="edbb2-129">hello például a virtuális gépek és a távoli végpont közötti kapcsolatot ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="edbb2-129">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="edbb2-130">Példa</span><span class="sxs-lookup"><span data-stu-id="edbb2-130">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="edbb2-131">Válasz</span><span class="sxs-lookup"><span data-stu-id="edbb2-131">Response</span></span>

<span data-ttu-id="edbb2-132">A következő példa hello, hello `connectionStatus` jelenik meg, mint **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="edbb2-132">In hello following example, hello `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="edbb2-133">A hello `hops` részleteket megtekintheti a `issues` hello forgalom miatt blokkolta tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="edbb2-133">In hello `hops` details, you can see under `issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a><span data-ttu-id="edbb2-134">Ellenőrizze a webhely késés</span><span class="sxs-lookup"><span data-stu-id="edbb2-134">Check website latency</span></span>

<span data-ttu-id="edbb2-135">a következő példa ellenőrzések hello kapcsolat tooa webhely hello.</span><span class="sxs-lookup"><span data-stu-id="edbb2-135">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="edbb2-136">Példa</span><span class="sxs-lookup"><span data-stu-id="edbb2-136">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a><span data-ttu-id="edbb2-137">Válasz</span><span class="sxs-lookup"><span data-stu-id="edbb2-137">Response</span></span>

<span data-ttu-id="edbb2-138">A válasz a következő hello, hogy hello `connectionStatus` jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="edbb2-138">In hello following response, you can see hello `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="edbb2-139">Sikeres kapcsolat esetén a késési értékek találhatók.</span><span class="sxs-lookup"><span data-stu-id="edbb2-139">When a connection is successful, latency values are provided.</span></span>

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="edbb2-140">Ellenőrizze a kapcsolat tooa tárolási végpont</span><span class="sxs-lookup"><span data-stu-id="edbb2-140">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="edbb2-141">hello alábbi példa hello kapcsolatát ellenőrzi a virtuális gép tooa blog tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="edbb2-141">hello following example checks hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="edbb2-142">Példa</span><span class="sxs-lookup"><span data-stu-id="edbb2-142">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a><span data-ttu-id="edbb2-143">Válasz</span><span class="sxs-lookup"><span data-stu-id="edbb2-143">Response</span></span>

<span data-ttu-id="edbb2-144">hello következő json-ja hello példa egy válasz hello előző parancsmag futtatását.</span><span class="sxs-lookup"><span data-stu-id="edbb2-144">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="edbb2-145">Mivel hello ellenőrzés sikeres, hello `connectionStatus` tulajdonság jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="edbb2-145">As hello check is successful, hello `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="edbb2-146">Útválasztók ugrásainak szükséges tooreach hello storage-blob és a késleltetés hello száma hello részleteket rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="edbb2-146">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a><span data-ttu-id="edbb2-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edbb2-147">Next steps</span></span>

<span data-ttu-id="edbb2-148">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="edbb2-148">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="edbb2-149">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="edbb2-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>
