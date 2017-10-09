---
title: "Azure hálózati figyelőt - Azure-portálon aaaCheck kapcsolatot |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toocheck kapcsolatot a hálózati figyelőt hello Azure-portálon"
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
ms.date: 08/02/2017
ms.author: gwallace
ms.openlocfilehash: 8560011906fcce46d31556fc52cbfa671e8e653a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a><span data-ttu-id="29e25-103">Ellenőrizze a kapcsolatot az Azure hálózati figyelőt hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="29e25-103">Check connectivity with Azure Network Watcher using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="29e25-104">Portál</span><span class="sxs-lookup"><span data-stu-id="29e25-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="29e25-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29e25-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="29e25-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="29e25-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="29e25-107">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="29e25-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="29e25-108">Ismerje meg, hogyan hozhatók létre a toouse kapcsolat tooverify, ha egy virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="29e25-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="29e25-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="29e25-109">Before you begin</span></span>

<span data-ttu-id="29e25-110">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="29e25-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="29e25-111">Egy példány toocheck kapcsolat kívánt hálózati figyelőt hello régióban.</span><span class="sxs-lookup"><span data-stu-id="29e25-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="29e25-112">Virtuális gépek toocheck kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="29e25-112">Virtual machines toocheck connectivity with.</span></span>

<span data-ttu-id="29e25-113">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="29e25-113">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="29e25-114">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="29e25-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient).</span></span>

<span data-ttu-id="29e25-115">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="29e25-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="29e25-116">Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="29e25-116">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="29e25-117">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="29e25-117">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="29e25-118">Hello előzetes funkció regisztrálása</span><span class="sxs-lookup"><span data-stu-id="29e25-118">Register hello preview capability</span></span>

<span data-ttu-id="29e25-119">Kapcsolat ellenőrzése jelenleg toouse nyilvános előzetes verziójában ez a szolgáltatás toobe regisztrálni kell.</span><span class="sxs-lookup"><span data-stu-id="29e25-119">Connectivity check is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="29e25-120">toodo e, futtassa a következő PowerShell-példa hello:</span><span class="sxs-lookup"><span data-stu-id="29e25-120">toodo this, run hello following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="29e25-121">tooverify hello a regisztráció sikeres volt, futtassa a következő Powershell-példa hello:</span><span class="sxs-lookup"><span data-stu-id="29e25-121">tooverify hello registration was successful, run hello following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="29e25-122">Hello szolgáltatás megfelelően lett-e regisztrálva, ha hello kimeneti meg kell felelnie a következő hello:</span><span class="sxs-lookup"><span data-stu-id="29e25-122">If hello feature was properly registered, hello output should match hello following:</span></span>

```
FeatureName                             ProviderName      RegistrationState
-----------                             ------------      -----------------
AllowNetworkWatcherConnectivityCheck    Microsoft.Network Registered
```

## <a name="log-in-with-armclient"></a><span data-ttu-id="29e25-123">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="29e25-123">Log in with ARMClient</span></span>

<span data-ttu-id="29e25-124">Jelentkezzen be a Azure hitelesítő adataival tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="29e25-124">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="29e25-125">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="29e25-125">Retrieve a virtual machine</span></span>

<span data-ttu-id="29e25-126">A következő parancsfájl tooreturn hello egy virtuális gép futtatásához.</span><span class="sxs-lookup"><span data-stu-id="29e25-126">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="29e25-127">Ezek az információk szükségesek a kapcsolat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="29e25-127">This information is needed for running connectivity.</span></span> 

<span data-ttu-id="29e25-128">a következő kód hello értékeket a következő változók hello szüksége van:</span><span class="sxs-lookup"><span data-stu-id="29e25-128">hello following code needs values for hello following variables:</span></span>

- <span data-ttu-id="29e25-129">**a subscriptionId** -előfizetési azonosító toouse hello.</span><span class="sxs-lookup"><span data-stu-id="29e25-129">**subscriptionId** - hello subscription ID toouse.</span></span>
- <span data-ttu-id="29e25-130">**resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="29e25-130">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="29e25-131">Hello következő kimeneti, hello virtuális gép hello azonosítója szerepel a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="29e25-131">From hello following output, hello ID of hello virtual machine is used in hello following example:</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="29e25-132">Ellenőrizze a kapcsolat tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="29e25-132">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="29e25-133">Ebben a példában kapcsolat tooa cél virtuális gép ellenőrzi a 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="29e25-133">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="29e25-134">Példa</span><span class="sxs-lookup"><span data-stu-id="29e25-134">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/Database0"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'resourceId': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="29e25-135">Mivel ez a művelet hosszú fut, hello hello eredmény URI-JÁNAK eredmény abban az esetben hello válaszfejléc látható hello válasz a következő módon:</span><span class="sxs-lookup"><span data-stu-id="29e25-135">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="29e25-136">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="29e25-136">**Important Values**</span></span>

* <span data-ttu-id="29e25-137">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="29e25-137">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: f09b55fe-1d3a-4df7-817f-bceb8d2a94c8
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/f09b55fe-1d3a-4df7-817f-bceb8d2a94c8?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 367a91aa-7142-436a-867d-d3a36f80bc54
x-ms-routing-request-id: WESTUS2:20170602T202117Z:367a91aa-7142-436a-867d-d3a36f80bc54
Date: Fri, 02 Jun 2017 20:21:16 GMT

null
```

### <a name="response"></a><span data-ttu-id="29e25-138">Válasz</span><span class="sxs-lookup"><span data-stu-id="29e25-138">Response</span></span>

<span data-ttu-id="29e25-139">válasz a következő hello hello előző példa származik.</span><span class="sxs-lookup"><span data-stu-id="29e25-139">hello following response is from hello previous example.</span></span>  <span data-ttu-id="29e25-140">A válaszban hello `ConnectionStatus` van **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="29e25-140">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="29e25-141">Láthatja, hogy az összes hello mintavételt küldése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="29e25-141">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="29e25-142">hello kapcsolat meghiúsult hello virtuális készülék felhasználó által konfigurált esedékes tooa `NetworkSecurityRule` nevű **UserRule_Port80**, konfigurált tooblock bejövő forgalmat a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="29e25-142">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="29e25-143">Ezek az információk használt tooresearch kapcsolódási problémák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="29e25-143">This information can be used tooresearch connection issues.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "0cb75c91-7ebf-4df8-8424-15594d6fb51c",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "06dee00a-9c4a-4fb1-b2ea-fa0a539ca684"
      ],
      "issues": []
    },
    {
      "type": "VirtualAppliance",
      "id": "06dee00a-9c4a-4fb1-b2ea-fa0a539ca684",
      "address": "10.1.2.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "75e0cfa5-f9d2-48d8-b705-2c7016f81570"
      ],
      "issues": []
    },
    {
      "type": "VirtualAppliance",
      "id": "75e0cfa5-f9d2-48d8-b705-2c7016f81570",
      "address": "10.1.3.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "86caf6aa-33b0-48a1-b4da-f3c9ce785072"
      ],
      "issues": [
        {
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule",
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ]
        }
      ]
    },
    {
      "type": "VnetLocal",
      "id": "86caf6aa-33b0-48a1-b4da-f3c9ce785072",
      "address": "10.1.4.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Unreachable",
  "probesSent": 100,
  "probesFailed": 100
}
```

## <a name="validate-routing-issues"></a><span data-ttu-id="29e25-144">Útválasztási problémák ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="29e25-144">Validate routing issues</span></span>

<span data-ttu-id="29e25-145">hello például a virtuális gépek és a távoli végpont közötti kapcsolatot ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="29e25-145">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="29e25-146">Példa</span><span class="sxs-lookup"><span data-stu-id="29e25-146">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "13.107.21.200"
$destinationPort = "80"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="29e25-147">Mivel ez a művelet hosszú fut, hello hello eredmény URI-JÁNAK eredmény abban az esetben hello válaszfejléc látható hello válasz a következő módon:</span><span class="sxs-lookup"><span data-stu-id="29e25-147">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="29e25-148">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="29e25-148">**Important Values**</span></span>

* <span data-ttu-id="29e25-149">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="29e25-149">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 15eeeb69-fcef-41db-bc4a-e2adcf2658e0
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/15eeeb69-fcef-41db-bc4a-e2adcf2658e0?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4370b798-cd8b-4d3e-ba28-22232bc81dc5
x-ms-routing-request-id: WESTUS:20170602T202606Z:4370b798-cd8b-4d3e-ba28-22232bc81dc5
Date: Fri, 02 Jun 2017 20:26:05 GMT

null
```

### <a name="response"></a><span data-ttu-id="29e25-150">Válasz</span><span class="sxs-lookup"><span data-stu-id="29e25-150">Response</span></span>

<span data-ttu-id="29e25-151">A következő példa hello, hello `connectionStatus` jelenik meg, mint **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="29e25-151">In hello following example, hello `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="29e25-152">A hello `hops` részleteket megtekintheti a `issues` hello forgalom miatt blokkolta tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="29e25-152">In hello `hops` details, you can see under `issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "5528055a-b393-4751-97bc-353d8c0aaeff",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "66eefa79-5bfe-48b2-b6ca-eec8247457a3"
      ],
      "issues": [
        {
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute",
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ]
        }
      ]
    },
    {
      "type": "Destination",
      "id": "66eefa79-5bfe-48b2-b6ca-eec8247457a3",
      "address": "13.107.21.200",
      "resourceId": "Unknown",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Unreachable",
  "probesSent": 100,
  "probesFailed": 100
}
```

## <a name="check-website-latency"></a><span data-ttu-id="29e25-153">Ellenőrizze a webhely késés</span><span class="sxs-lookup"><span data-stu-id="29e25-153">Check website latency</span></span>

<span data-ttu-id="29e25-154">a következő példa ellenőrzések hello kapcsolat tooa webhely hello.</span><span class="sxs-lookup"><span data-stu-id="29e25-154">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="29e25-155">Példa</span><span class="sxs-lookup"><span data-stu-id="29e25-155">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "http://bing.com"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="29e25-156">Mivel ez a művelet hosszú fut, hello hello eredmény URI-JÁNAK eredmény abban az esetben hello válaszfejléc látható hello válasz a következő módon:</span><span class="sxs-lookup"><span data-stu-id="29e25-156">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="29e25-157">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="29e25-157">**Important Values**</span></span>

* <span data-ttu-id="29e25-158">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="29e25-158">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: e49b12c7-c232-472c-b6d2-6c257ce80fa5
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/e49b12c7-c232-472c-b6d2-6c257ce80fa5?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: c3d9744f-5683-427d-bdd1-636b68ab01b6
x-ms-routing-request-id: WESTUS:20170602T203101Z:c3d9744f-5683-427d-bdd1-636b68ab01b6
Date: Fri, 02 Jun 2017 20:31:00 GMT

null
```

### <a name="response"></a><span data-ttu-id="29e25-159">Válasz</span><span class="sxs-lookup"><span data-stu-id="29e25-159">Response</span></span>

<span data-ttu-id="29e25-160">A válasz a következő hello, hogy hello `connectionStatus` jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="29e25-160">In hello following response, you can see hello `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="29e25-161">Sikeres kapcsolat esetén a késési értékek találhatók.</span><span class="sxs-lookup"><span data-stu-id="29e25-161">When a connection is successful, latency values are provided.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "6adc0fe1-e384-4220-b1b1-f0d181220072",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "b50b7076-9ff2-4782-b40e-0b89cf758f74"
      ],
      "issues": []
    },
    {
      "type": "Internet",
      "id": "b50b7076-9ff2-4782-b40e-0b89cf758f74",
      "address": "204.79.197.200",
      "resourceId": "Internet",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Reachable",
  "avgLatencyInMs": 1,
  "minLatencyInMs": 0,
  "maxLatencyInMs": 7,
  "probesSent": 100,
  "probesFailed": 0
}
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="29e25-162">Ellenőrizze a kapcsolat tooa tárolási végpont</span><span class="sxs-lookup"><span data-stu-id="29e25-162">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="29e25-163">hello alábbi példa hello kapcsolatát ellenőrzi a virtuális gép tooa blog tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="29e25-163">hello following example checks hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="29e25-164">Példa</span><span class="sxs-lookup"><span data-stu-id="29e25-164">Example</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$sourceResourceId = "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Compute/virtualMachines/MultiTierApp0"
$destinationAddress = "https://build2017nwdiag360.blob.core.windows.net/"
$destinationPort = "0"
$requestBody = @"
{
  'source': {
    'resourceId': '${sourceResourceId}',
    'port': 0
  },
  'destination': {
    'address': '${destinationAddress}',
    'port': ${destinationPort}
  }
}
"@

$response = armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/connectivityCheck?api-version=2017-03-01" $requestBody
```

<span data-ttu-id="29e25-165">Mivel ez a művelet hosszú fut, hello hello eredmény URI-JÁNAK eredmény abban az esetben hello válaszfejléc látható hello válasz a következő módon:</span><span class="sxs-lookup"><span data-stu-id="29e25-165">Since this operation is long running, hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="29e25-166">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="29e25-166">**Important Values**</span></span>

* <span data-ttu-id="29e25-167">**Hely** -ezt a tulajdonságot tartalmaz hello URI, ahol hello eredmények esetén hello művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="29e25-167">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: c4ed3806-61ea-4a6b-abc1-9d6f2afc79c2
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/c4ed3806-61ea-4a6b-abc1-9d6f2afc79c2?api-version=2017-03-01
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 93bf5af0-fef5-4b7a-bb9e-9976ba5cdb95
x-ms-routing-request-id: WESTUS2:20170602T200504Z:93bf5af0-fef5-4b7a-bb9e-9976ba5cdb95
Date: Fri, 02 Jun 2017 20:05:03 GMT

null
```

### <a name="response"></a><span data-ttu-id="29e25-168">Válasz</span><span class="sxs-lookup"><span data-stu-id="29e25-168">Response</span></span>

<span data-ttu-id="29e25-169">hello következő példa egy hello válasz hello előző API-hívás futtatását.</span><span class="sxs-lookup"><span data-stu-id="29e25-169">hello following example is hello response from running hello previous API call.</span></span> <span data-ttu-id="29e25-170">Mivel hello ellenőrzés sikeres, hello `connectionStatus` tulajdonság jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="29e25-170">As hello check is successful, hello `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="29e25-171">Útválasztók ugrásainak szükséges tooreach hello storage-blob és a késleltetés hello száma hello részleteket rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="29e25-171">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```json
{
  "hops": [
    {
      "type": "Source",
      "id": "6adc0fe1-e384-4220-b1b1-f0d181220072",
      "address": "10.1.1.4",
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "nextHopIds": [
        "b50b7076-9ff2-4782-b40e-0b89cf758f74"
      ],
      "issues": []
    },
    {
      "type": "Internet",
      "id": "b50b7076-9ff2-4782-b40e-0b89cf758f74",
      "address": "13.71.200.248",
      "resourceId": "Internet",
      "nextHopIds": [],
      "issues": []
    }
  ],
  "connectionStatus": "Reachable",
  "avgLatencyInMs": 1,
  "minLatencyInMs": 0,
  "maxLatencyInMs": 7,
  "probesSent": 100,
  "probesFailed": 0
}
```

## <a name="next-steps"></a><span data-ttu-id="29e25-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29e25-172">Next steps</span></span>

<span data-ttu-id="29e25-173">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="29e25-173">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="29e25-174">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="29e25-174">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














