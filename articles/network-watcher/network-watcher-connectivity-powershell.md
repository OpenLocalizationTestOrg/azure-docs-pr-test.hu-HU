---
title: "Azure hálózati figyelőt - PowerShell aaaCheck kapcsolatot |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan tootest csatlakozni a hálózati figyelőt PowerShell használatával"
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
ms.openlocfilehash: 4bcb90a72f178445c38b7bd7fc5054c5d0c200bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="ffa64-103">Ellenőrizze a kapcsolatot az Azure hálózati figyelőt PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ffa64-103">Check connectivity with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="ffa64-104">Portál</span><span class="sxs-lookup"><span data-stu-id="ffa64-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="ffa64-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ffa64-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="ffa64-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ffa64-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="ffa64-107">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="ffa64-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="ffa64-108">Ismerje meg, hogyan hozhatók létre a toouse kapcsolat tooverify, ha egy virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ffa64-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ffa64-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ffa64-109">Before you begin</span></span>

<span data-ttu-id="ffa64-110">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="ffa64-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="ffa64-111">Egy példány toocheck kapcsolat kívánt hálózati figyelőt hello régióban.</span><span class="sxs-lookup"><span data-stu-id="ffa64-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="ffa64-112">Virtuális gépek toocheck kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="ffa64-112">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="ffa64-113">Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="ffa64-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="ffa64-114">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="ffa64-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="ffa64-115">Hello előzetes funkció regisztrálása</span><span class="sxs-lookup"><span data-stu-id="ffa64-115">Register hello preview capability</span></span>

<span data-ttu-id="ffa64-116">Kapcsolat jelenleg toouse nyilvános előzetes verziójában ez a szolgáltatás toobe regisztrálni kell.</span><span class="sxs-lookup"><span data-stu-id="ffa64-116">Connectivity is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="ffa64-117">toodo e, futtassa a következő PowerShell-példa hello:</span><span class="sxs-lookup"><span data-stu-id="ffa64-117">toodo this, run hello following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="ffa64-118">tooverify hello a regisztráció sikeres volt, futtassa a következő Powershell-példa hello:</span><span class="sxs-lookup"><span data-stu-id="ffa64-118">tooverify hello registration was successful, run hello following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="ffa64-119">Hello szolgáltatás megfelelően lett-e regisztrálva, ha hello kimeneti meg kell felelnie a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ffa64-119">If hello feature was properly registered, hello output should match hello following:</span></span>

```
FeatureName         ProviderName      RegistrationState
-----------         ------------      -----------------
AllowNetworkWatcherConnectivityCheck  Microsoft.Network Registered
```

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="ffa64-120">Ellenőrizze a kapcsolat tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="ffa64-120">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="ffa64-121">Ebben a példában kapcsolat tooa cél virtuális gép ellenőrzi a 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="ffa64-121">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="ffa64-122">Példa</span><span class="sxs-lookup"><span data-stu-id="ffa64-122">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"
$destVMName = "Database0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName
$VM2 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $destVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationId $VM2.Id -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="ffa64-123">Válasz</span><span class="sxs-lookup"><span data-stu-id="ffa64-123">Response</span></span>

<span data-ttu-id="ffa64-124">válasz a következő hello hello előző példa származik.</span><span class="sxs-lookup"><span data-stu-id="ffa64-124">hello following response is from hello previous example.</span></span>  <span data-ttu-id="ffa64-125">A válaszban hello `ConnectionStatus` van **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="ffa64-125">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="ffa64-126">Láthatja, hogy az összes hello mintavételt küldése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="ffa64-126">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="ffa64-127">hello kapcsolat meghiúsult hello virtuális készülék felhasználó által konfigurált esedékes tooa `NetworkSecurityRule` nevű **UserRule_Port80**, konfigurált tooblock bejövő forgalmat a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="ffa64-127">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="ffa64-128">Ezek az információk használt tooresearch kapcsolódási problémák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="ffa64-128">This information can be used tooresearch connection issues.</span></span>

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "c5222ea0-3213-4f85-a642-cee63217c2f3",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurat
                   ions/ipconfig1",
                       "NextHopIds": [
                         "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa",
                       "Address": "10.1.2.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "0f1500cd-c512-4d43-b431-7267e4e67017"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "0f1500cd-c512-4d43-b431-7267e4e67017",
                       "Address": "10.1.3.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "a88940f8-5fbe-40da-8d99-1dee89240f64"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "NetworkSecurityRule",
                           "Context": [
                             {
                               "key": "RuleName",
                               "value": "UserRule_Port80"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "VnetLocal",
                       "Id": "a88940f8-5fbe-40da-8d99-1dee89240f64",
                       "Address": "10.1.4.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurati
                   ons/ipconfig1",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="validate-routing-issues"></a><span data-ttu-id="ffa64-129">Útválasztási problémák ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ffa64-129">Validate routing issues</span></span>

<span data-ttu-id="ffa64-130">hello például a virtuális gépek és a távoli végpont közötti kapcsolatot ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="ffa64-130">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="ffa64-131">Példa</span><span class="sxs-lookup"><span data-stu-id="ffa64-131">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress 13.107.21.200 -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="ffa64-132">Válasz</span><span class="sxs-lookup"><span data-stu-id="ffa64-132">Response</span></span>

<span data-ttu-id="ffa64-133">A következő példa hello, hello `ConnectionStatus` jelenik meg, mint **Unreachable**.</span><span class="sxs-lookup"><span data-stu-id="ffa64-133">In hello following example, hello `ConnectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="ffa64-134">A hello `Hops` részleteket megtekintheti a `Issues` hello forgalom miatt blokkolta tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="ffa64-134">In hello `Hops` details, you can see under `Issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span> 

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "b4f7bceb-07a3-44ca-8bae-adec6628225f",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "3fee8adf-692f-4523-b742-f6fdf6da6584"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "UserDefinedRoute",
                           "Context": [
                             {
                               "key": "RouteType",
                               "value": "User"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "Destination",
                       "Id": "3fee8adf-692f-4523-b742-f6fdf6da6584",
                       "Address": "13.107.21.200",
                       "ResourceId": "Unknown",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-website-latency"></a><span data-ttu-id="ffa64-135">Ellenőrizze a webhely késés</span><span class="sxs-lookup"><span data-stu-id="ffa64-135">Check website latency</span></span>

<span data-ttu-id="ffa64-136">a következő példa ellenőrzések hello kapcsolat tooa webhely hello.</span><span class="sxs-lookup"><span data-stu-id="ffa64-136">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="ffa64-137">Példa</span><span class="sxs-lookup"><span data-stu-id="ffa64-137">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress http://bing.com/
```

### <a name="response"></a><span data-ttu-id="ffa64-138">Válasz</span><span class="sxs-lookup"><span data-stu-id="ffa64-138">Response</span></span>

<span data-ttu-id="ffa64-139">A válasz a következő hello, hogy hello `ConnectionStatus` jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="ffa64-139">In hello following response, you can see hello `ConnectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="ffa64-140">Sikeres kapcsolat esetén a késési értékek találhatók.</span><span class="sxs-lookup"><span data-stu-id="ffa64-140">When a connection is successful, latency values are provided.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 7
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "1f0e3415-27b0-4bf7-a59d-3e19fb854e3e",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0",
                       "Address": "204.79.197.200",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="ffa64-141">Ellenőrizze a kapcsolat tooa tárolási végpont</span><span class="sxs-lookup"><span data-stu-id="ffa64-141">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="ffa64-142">a következő példa hello ellenőrzi a hello csatlakozást a virtuális gép tooa blog tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="ffa64-142">hello following example tests hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="ffa64-143">Példa</span><span class="sxs-lookup"><span data-stu-id="ffa64-143">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://contosostorageexample.blob.core.windows.net/ 
```

### <a name="response"></a><span data-ttu-id="ffa64-144">Válasz</span><span class="sxs-lookup"><span data-stu-id="ffa64-144">Response</span></span>

<span data-ttu-id="ffa64-145">hello következő json-ja hello példa egy válasz hello előző parancsmag futtatását.</span><span class="sxs-lookup"><span data-stu-id="ffa64-145">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="ffa64-146">Hello cél érhető el, mert hello `ConnectionStatus` tulajdonság jeleníti meg, mint a **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="ffa64-146">As hello destination is reachable, hello `ConnectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="ffa64-147">Útválasztók ugrásainak szükséges tooreach hello storage-blob és a késleltetés hello száma hello részleteket rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="ffa64-147">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 8
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "9e7f61d9-fb45-41db-83e2-c815a919b8ed",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "1e6d4b3c-7964-4afd-b959-aaa746ee0f15"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "1e6d4b3c-7964-4afd-b959-aaa746ee0f15",
                       "Address": "13.71.200.248",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="next-steps"></a><span data-ttu-id="ffa64-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffa64-148">Next steps</span></span>

<span data-ttu-id="ffa64-149">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ffa64-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<span data-ttu-id="ffa64-150">Ha a forgalmat blokkol, és nem kell, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello hálózati biztonsági csoport és a biztonsági szabályok definiált.</span><span class="sxs-lookup"><span data-stu-id="ffa64-150">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<!-- Image references -->














