---
title: "Ellenőrizze a kapcsolatot az Azure hálózati figyelőt - Azure-portál |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan ellenőrizze a kapcsolatot a hálózati figyelőt az Azure-portálon"
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
ms.openlocfilehash: ca62bea581acb59d3c3c0b8a204cc9d42de2b27f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-the-azure-portal"></a><span data-ttu-id="10bec-103">Ellenőrizze a kapcsolatot az Azure hálózati figyelőt az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="10bec-103">Check connectivity with Azure Network Watcher using the Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="10bec-104">Portal</span><span class="sxs-lookup"><span data-stu-id="10bec-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="10bec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="10bec-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="10bec-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="10bec-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="10bec-107">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="10bec-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="10bec-108">Megtudhatja, hogyan ellenőrizheti, ha egy közvetlen TCP-kapcsolatot a virtuális gép egy adott végpont is hozható létre kapcsolat használatára.</span><span class="sxs-lookup"><span data-stu-id="10bec-108">Learn how to use connectivity to verify if a direct TCP connection from a virtual machine to a given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="10bec-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="10bec-109">Before you begin</span></span>

<span data-ttu-id="10bec-110">Ez a cikk feltételezi, hogy rendelkezik-e a következőket:</span><span class="sxs-lookup"><span data-stu-id="10bec-110">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="10bec-111">Ellenőrizze a kapcsolatot szeretne hálózati figyelőt régióban példánya.</span><span class="sxs-lookup"><span data-stu-id="10bec-111">An instance of Network Watcher in the region you want to check connectivity.</span></span>

* <span data-ttu-id="10bec-112">Ellenőrizze a kapcsolatot a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="10bec-112">Virtual machines to check connectivity with.</span></span>

<span data-ttu-id="10bec-113">A PowerShell használatával REST API hívása ARMclient szolgál.</span><span class="sxs-lookup"><span data-stu-id="10bec-113">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="10bec-114">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="10bec-114">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient).</span></span>

<span data-ttu-id="10bec-115">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="10bec-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="10bec-116">Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="10bec-116">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="10bec-117">A bővítmény telepítése a Windows virtuális gép a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="10bec-117">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-the-preview-capability"></a><span data-ttu-id="10bec-118">Regisztrálja a előzetes funkció</span><span class="sxs-lookup"><span data-stu-id="10bec-118">Register the preview capability</span></span>

<span data-ttu-id="10bec-119">Kapcsolat ellenőrzése jelenleg nyilvános előzetes verziójában, regisztrálni kell a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="10bec-119">Connectivity check is currently in public preview, to use this feature it needs to be registered.</span></span> <span data-ttu-id="10bec-120">Ehhez futtassa a következő PowerShell-példa:</span><span class="sxs-lookup"><span data-stu-id="10bec-120">To do this, run the following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="10bec-121">Ellenőrizze a regisztráció sikeres volt-e, futtassa a következő Powershell-példa:</span><span class="sxs-lookup"><span data-stu-id="10bec-121">To verify the registration was successful, run the following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="10bec-122">Ha a szolgáltatás megfelelően lett-e regisztrálva, a kimeneti meg kell felelnie a következő:</span><span class="sxs-lookup"><span data-stu-id="10bec-122">If the feature was properly registered, the output should match the following:</span></span>

```
FeatureName                             ProviderName      RegistrationState
-----------                             ------------      -----------------
AllowNetworkWatcherConnectivityCheck    Microsoft.Network Registered
```

## <a name="log-in-with-armclient"></a><span data-ttu-id="10bec-123">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="10bec-123">Log in with ARMClient</span></span>

<span data-ttu-id="10bec-124">Jelentkezzen be a Azure hitelesítő adataival armclient.</span><span class="sxs-lookup"><span data-stu-id="10bec-124">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="10bec-125">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="10bec-125">Retrieve a virtual machine</span></span>

<span data-ttu-id="10bec-126">Futtassa a következő parancsfájl egy virtuális gép visszaállítására.</span><span class="sxs-lookup"><span data-stu-id="10bec-126">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="10bec-127">Ezek az információk szükségesek a kapcsolat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="10bec-127">This information is needed for running connectivity.</span></span> 

<span data-ttu-id="10bec-128">A következő kódot a következő változó igényeihez értékek:</span><span class="sxs-lookup"><span data-stu-id="10bec-128">The following code needs values for the following variables:</span></span>

- <span data-ttu-id="10bec-129">**a subscriptionId** -az előfizetés-azonosító használata.</span><span class="sxs-lookup"><span data-stu-id="10bec-129">**subscriptionId** - The subscription ID to use.</span></span>
- <span data-ttu-id="10bec-130">**resourceGroupName** -virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="10bec-130">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="10bec-131">A virtuális gép Azonosítóját a következő kimeneti használják a következő példa:</span><span class="sxs-lookup"><span data-stu-id="10bec-131">From the following output, the ID of the virtual machine is used in the following example:</span></span>

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

## <a name="check-connectivity-to-a-virtual-machine"></a><span data-ttu-id="10bec-132">Ellenőrizze a kapcsolatot a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="10bec-132">Check connectivity to a virtual machine</span></span>

<span data-ttu-id="10bec-133">Ebben a példában a cél virtuális gép kapcsolatát ellenőrzi a 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="10bec-133">This example checks connectivity to a destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="10bec-134">Példa</span><span class="sxs-lookup"><span data-stu-id="10bec-134">Example</span></span>

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

<span data-ttu-id="10bec-135">Mivel ez a művelet hosszú fut, az URI, az eredmény a válasz fejlécében vissza, ahogy az a következő választ:</span><span class="sxs-lookup"><span data-stu-id="10bec-135">Since this operation is long running, the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="10bec-136">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="10bec-136">**Important Values**</span></span>

* <span data-ttu-id="10bec-137">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="10bec-137">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="response"></a><span data-ttu-id="10bec-138">Válasz</span><span class="sxs-lookup"><span data-stu-id="10bec-138">Response</span></span>

<span data-ttu-id="10bec-139">Az előző példában a rendszer a következő választ.</span><span class="sxs-lookup"><span data-stu-id="10bec-139">The following response is from the previous example.</span></span>  <span data-ttu-id="10bec-140">A válaszban a `ConnectionStatus` van **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="10bec-140">In this response, the `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="10bec-141">Láthatja, hogy a mintavételt küldése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="10bec-141">You can see that all the probes sent failed.</span></span> <span data-ttu-id="10bec-142">A kapcsolat a felhasználó által konfigurált miatt a virtuális készülék meghiúsult `NetworkSecurityRule` nevű **UserRule_Port80**, beállítva, hogy a bejövő forgalom blokkolása a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="10bec-142">The connectivity failed at the virtual appliance due to a user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured to block incoming traffic on port 80.</span></span> <span data-ttu-id="10bec-143">Ezek az információk segítségével kutatás kapcsolódási problémák.</span><span class="sxs-lookup"><span data-stu-id="10bec-143">This information can be used to research connection issues.</span></span>

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

## <a name="validate-routing-issues"></a><span data-ttu-id="10bec-144">Útválasztási problémák ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="10bec-144">Validate routing issues</span></span>

<span data-ttu-id="10bec-145">A példában egy virtuális gép és a távoli végpont közötti kapcsolatot ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="10bec-145">The example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="10bec-146">Példa</span><span class="sxs-lookup"><span data-stu-id="10bec-146">Example</span></span>

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

<span data-ttu-id="10bec-147">Mivel ez a művelet hosszú fut, az URI, az eredmény a válasz fejlécében vissza, ahogy az a következő választ:</span><span class="sxs-lookup"><span data-stu-id="10bec-147">Since this operation is long running, the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="10bec-148">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="10bec-148">**Important Values**</span></span>

* <span data-ttu-id="10bec-149">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="10bec-149">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="response"></a><span data-ttu-id="10bec-150">Válasz</span><span class="sxs-lookup"><span data-stu-id="10bec-150">Response</span></span>

<span data-ttu-id="10bec-151">A következő példában a `connectionStatus` jelenik meg, mint **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="10bec-151">In the following example, the `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="10bec-152">Az a `hops` részleteket megtekintheti a `issues` , amely a forgalom miatt blokkolta a `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="10bec-152">In the `hops` details, you can see under `issues` that the traffic was blocked due to a `UserDefinedRoute`.</span></span>

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

## <a name="check-website-latency"></a><span data-ttu-id="10bec-153">Ellenőrizze a webhely késés</span><span class="sxs-lookup"><span data-stu-id="10bec-153">Check website latency</span></span>

<span data-ttu-id="10bec-154">A következő példa a webhely csatlakozási ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="10bec-154">The following example checks the connectivity to a website.</span></span>

### <a name="example"></a><span data-ttu-id="10bec-155">Példa</span><span class="sxs-lookup"><span data-stu-id="10bec-155">Example</span></span>

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

<span data-ttu-id="10bec-156">Mivel ez a művelet hosszú fut, az URI, az eredmény a válasz fejlécében vissza, ahogy az a következő választ:</span><span class="sxs-lookup"><span data-stu-id="10bec-156">Since this operation is long running, the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="10bec-157">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="10bec-157">**Important Values**</span></span>

* <span data-ttu-id="10bec-158">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="10bec-158">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="response"></a><span data-ttu-id="10bec-159">Válasz</span><span class="sxs-lookup"><span data-stu-id="10bec-159">Response</span></span>

<span data-ttu-id="10bec-160">A következő válasz láthatja a `connectionStatus` jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="10bec-160">In the following response, you can see the `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="10bec-161">Sikeres kapcsolat esetén a késési értékek találhatók.</span><span class="sxs-lookup"><span data-stu-id="10bec-161">When a connection is successful, latency values are provided.</span></span>

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

## <a name="check-connectivity-to-a-storage-endpoint"></a><span data-ttu-id="10bec-162">Ellenőrizze a kapcsolatot a storage-végponthoz</span><span class="sxs-lookup"><span data-stu-id="10bec-162">Check connectivity to a storage endpoint</span></span>

<span data-ttu-id="10bec-163">A következő példa a kapcsolat a virtuális gépen blog tárfiókba ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="10bec-163">The following example checks the connectivity from a virtual machine to a blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="10bec-164">Példa</span><span class="sxs-lookup"><span data-stu-id="10bec-164">Example</span></span>

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

<span data-ttu-id="10bec-165">Mivel ez a művelet hosszú fut, az URI, az eredmény a válasz fejlécében vissza, ahogy az a következő választ:</span><span class="sxs-lookup"><span data-stu-id="10bec-165">Since this operation is long running, the URI for the result is returned in the response header as shown in the following response:</span></span>

<span data-ttu-id="10bec-166">**Fontos értékek**</span><span class="sxs-lookup"><span data-stu-id="10bec-166">**Important Values**</span></span>

* <span data-ttu-id="10bec-167">**Hely** – Ez a tulajdonság tartalmazza az adott az eredmény nem a művelet befejezésekor URI</span><span class="sxs-lookup"><span data-stu-id="10bec-167">**Location** - This property contains the URI where the results are when the operation is complete</span></span>

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

### <a name="response"></a><span data-ttu-id="10bec-168">Válasz</span><span class="sxs-lookup"><span data-stu-id="10bec-168">Response</span></span>

<span data-ttu-id="10bec-169">A következő példa az előző API-hívás futtató válaszát.</span><span class="sxs-lookup"><span data-stu-id="10bec-169">The following example is the response from running the previous API call.</span></span> <span data-ttu-id="10bec-170">Az ellenőrzés sikeres, mert a `connectionStatus` tulajdonság jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="10bec-170">As the check is successful, the `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="10bec-171">A tárolási blob és a késleltetés eléréséhez szükséges ugrások száma kapcsolatos részleteket rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="10bec-171">You are provided the details regarding the number of hops required to reach the storage blob and latency.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="10bec-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10bec-172">Next steps</span></span>

<span data-ttu-id="10bec-173">Csomag rögzíti a virtuális gép a riasztások megtekintésével automatizálása [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="10bec-173">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="10bec-174">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="10bec-174">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














