---
title: "Csomag rögzíti az Azure hálózati figyelőt - PowerShell kezelése |} Microsoft Docs"
description: "Ezen a lapon ismerteti, hogyan kezelheti a csomag rögzítési szolgáltatása hálózati figyelőt PowerShell használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 04d82085-c9ea-4ea1-b050-a3dd4960f3aa
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abd3b3641da80ee835fac85b4bde68594449e451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="150ca-103">Csomag rögzítésekre kezelése a PowerShell használata Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="150ca-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="150ca-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="150ca-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="150ca-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="150ca-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="150ca-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="150ca-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="150ca-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="150ca-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="150ca-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="150ca-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="150ca-109">Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek nyomon követéséhez forgalma és a virtuális gép létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="150ca-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="150ca-110">Annak érdekében, hogy csak a kívánt forgalom rögzíti a rögzítési munkamenet szűrők célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="150ca-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="150ca-111">Csomagrögzítéssel segít diagnosztizálni hálózati rendellenességeket proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="150ca-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="150ca-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, hálózati behatolások, ügyfél-kiszolgáló közötti kommunikációt, és még sok más hibakeresési információkat való összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="150ca-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="150ca-113">Őket távolról elindítása csomag rögzíti, ez a funkció megkönnyíti a csomagrögzítéssel fut, manuálisan, a kívánt számítógépet, amely értékes időt takaríthat meg okozta terheket.</span><span class="sxs-lookup"><span data-stu-id="150ca-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="150ca-114">Ez a cikk végigvezeti Önt a különböző felügyeleti feladatok, amelyek jelenleg a csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="150ca-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="150ca-115">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="150ca-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="150ca-116">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="150ca-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="150ca-117">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="150ca-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="150ca-118">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="150ca-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="150ca-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="150ca-119">Before you begin</span></span>

<span data-ttu-id="150ca-120">Ez a cikk feltételezi, hogy rendelkezik-e a következőket:</span><span class="sxs-lookup"><span data-stu-id="150ca-120">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="150ca-121">A csomagrögzítéssel létrehozni kívánt hálózati figyelőt régióban példánya</span><span class="sxs-lookup"><span data-stu-id="150ca-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>

* <span data-ttu-id="150ca-122">A csomag rögzítési engedélyezett bővítményekhez virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="150ca-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="150ca-123">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="150ca-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="150ca-124">A bővítmény telepítése a Windows virtuális gép a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="150ca-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="150ca-125">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="150ca-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="150ca-126">1. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="150ca-127">2. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-127">Step 2</span></span>

<span data-ttu-id="150ca-128">Az alábbi példa futtatásához szükséges bővítmény adatainak beolvasása a `Set-AzureRmVMExtension` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="150ca-128">The following example retrieves the extension information needed to run the `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="150ca-129">Ez a parancsmag a csomag rögzítési ügynököt telepíti a Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="150ca-129">This cmdlet installs the packet capture agent on the guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="150ca-130">A `Set-AzureRmVMExtension` parancsmag több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="150ca-130">The `Set-AzureRmVMExtension` cmdlet may take several minutes to complete.</span></span>

<span data-ttu-id="150ca-131">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="150ca-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="150ca-132">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="150ca-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="150ca-133">A következő példa a sikeres válasz egy futtatása után a `Set-AzureRmVMExtension` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="150ca-133">The following example is a successful response after running the `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="150ca-134">3. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-134">Step 3</span></span>

<span data-ttu-id="150ca-135">Győződjön meg arról, hogy az ügynök telepítve van-e, futtassa a `Get-AzureRmVMExtension` parancsmagot, és adja át a virtuális gép nevét és a bővítmény neve.</span><span class="sxs-lookup"><span data-stu-id="150ca-135">To ensure that the agent is installed, run the `Get-AzureRmVMExtension` cmdlet and pass it the virtual machine name and the extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="150ca-136">Az alábbi minta futtatását a válasz példája`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="150ca-136">The following sample is an example of the response from running `Get-AzureRmVMExtension`</span></span>

```
ResourceGroupName       : testrg
VMName                  : testvm1
Name                    : AzureNetworkWatcherExtension
Location                : westcentralus
Etag                    : null
Publisher               : Microsoft.Azure.NetworkWatcher
ExtensionType           : NetworkWatcherAgentWindows
TypeHandlerVersion      : 1.4
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1/
                          extensions/AzureNetworkWatcherExtension
PublicSettings          : 
ProtectedSettings       : 
ProvisioningState       : Succeeded
Statuses                : 
SubStatuses             : 
AutoUpgradeMinorVersion : True
ForceUpdateTag          : 
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="150ca-137">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="150ca-137">Start a packet capture</span></span>

<span data-ttu-id="150ca-138">Ha az előző lépések befejeződött, a csomag rögzítési ügynök telepítve van a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="150ca-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="150ca-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-139">Step 1</span></span>

<span data-ttu-id="150ca-140">A következő lépésre hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="150ca-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="150ca-141">Ez a változó átadott a `New-AzureRmNetworkWatcherPacketCapture` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="150ca-141">This variable is passed to the `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="150ca-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-142">Step 2</span></span>

<span data-ttu-id="150ca-143">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="150ca-143">Retrieve a storage account.</span></span> <span data-ttu-id="150ca-144">Ez a tárfiók a csomag rögzítési fájl tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="150ca-144">This storage account is used to store the packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="150ca-145">3. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-145">Step 3</span></span>

<span data-ttu-id="150ca-146">A csomagrögzítéssel által tárolt adatok szűrők használható.</span><span class="sxs-lookup"><span data-stu-id="150ca-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="150ca-147">A következő példában két szűrő beállítása.</span><span class="sxs-lookup"><span data-stu-id="150ca-147">The following example sets up two filters.</span></span>  <span data-ttu-id="150ca-148">Egy szűrő csak a helyi IP 10.0.0.3 kimenő TCP-forgalom a célport 20, a 80-as és a 443-as gyűjti.</span><span class="sxs-lookup"><span data-stu-id="150ca-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="150ca-149">A második szűrő csak a UDP-forgalom gyűjti.</span><span class="sxs-lookup"><span data-stu-id="150ca-149">The second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="150ca-150">A csomagrögzítéssel több szűrő adható meg.</span><span class="sxs-lookup"><span data-stu-id="150ca-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="150ca-151">4. lépés</span><span class="sxs-lookup"><span data-stu-id="150ca-151">Step 4</span></span>

<span data-ttu-id="150ca-152">Futtassa a `New-AzureRmNetworkWatcherPacketCapture` az előző lépésben beolvasott parancsmagot, hogy a csomag rögzítési folyamat elindításához szükséges értékeket átadásakor.</span><span class="sxs-lookup"><span data-stu-id="150ca-152">Run the `New-AzureRmNetworkWatcherPacketCapture` cmdlet to start the packet capture process, passing the required values retrieved in the preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="150ca-153">A következő példa egy futtatásának várható kimenete a `New-AzureRmNetworkWatcherPacketCapture` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="150ca-153">The following example is the expected output from running the `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"3bf27278-8251-4651-9546-c7f369855e4e"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]


```

## <a name="get-a-packet-capture"></a><span data-ttu-id="150ca-154">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="150ca-154">Get a packet capture</span></span>

<span data-ttu-id="150ca-155">Fut a `Get-AzureRmNetworkWatcherPacketCapture` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel állapotát.</span><span class="sxs-lookup"><span data-stu-id="150ca-155">Running the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="150ca-156">A következő példa egy kimenete a `Get-AzureRmNetworkWatcherPacketCapture` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="150ca-156">The following example is the output from the `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="150ca-157">A következő példa a rögzítés befejezése után van.</span><span class="sxs-lookup"><span data-stu-id="150ca-157">The following example is after the capture is complete.</span></span> <span data-ttu-id="150ca-158">A PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="150ca-158">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="150ca-159">Ezt az értéket jeleníti meg, hogy a csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="150ca-159">This value shows that the packet capture was successful and ran its time.</span></span>
```
Name                    : PacketCaptureTest
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatcher
                          s/NetworkWatcher_westcentralus/packetCaptures/PacketCaptureTest
Etag                    : W/"4b9a81ed-dc63-472e-869e-96d7166ccb9b"
ProvisioningState       : Succeeded
Target                  : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testvm1
BytesToCapturePerPacket : 0
TotalBytesPerSession    : 1073741824
TimeLimitInSeconds      : 60
StorageLocation         : {
                            "StorageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Storage/storageA
                          ccounts/examplestorage",
                            "StoragePath": "https://examplestorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-00000
                          0000000/resourcegroups/testrg/providers/microsoft.compute/virtualmachines/testvm1/2017/02/01/packetcapture_22_42_48_238.cap"
                          }
Filters                 : [
                            {
                              "Protocol": "TCP",
                              "RemoteIPAddress": "1.1.1.1-255.255.255",
                              "LocalIPAddress": "10.0.0.3",
                              "LocalPort": "1-65535",
                              "RemotePort": "20;80;443"
                            },
                            {
                              "Protocol": "UDP",
                              "RemoteIPAddress": "",
                              "LocalIPAddress": "",
                              "LocalPort": "",
                              "RemotePort": ""
                            }
                          ]
CaptureStartTime        : 2/1/2017 10:43:01 PM
PacketCaptureStatus     : Stopped
StopReason              : TimeExceeded
PacketCaptureError      : []
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="150ca-160">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="150ca-160">Stop a packet capture</span></span>

<span data-ttu-id="150ca-161">Futtassa a `Stop-AzureRmNetworkWatcherPacketCapture` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="150ca-161">By running the `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="150ca-162">A parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="150ca-162">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="150ca-163">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="150ca-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="150ca-164">A csomagrögzítéssel törlése nem érinti a tárfiókban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="150ca-164">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="150ca-165">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="150ca-165">Download a packet capture</span></span>

<span data-ttu-id="150ca-166">A csomag rögzítési munkamenet befejezése után a rögzítési fájl is fel kell tölteni a blob storage vagy a virtuális gép helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="150ca-166">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="150ca-167">A tárolási helye a csomagrögzítéssel definiálása a munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="150ca-167">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="150ca-168">Eszköz eléréséhez rögzítési-fájlokat egy tárfiókkal a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="150ca-168">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="150ca-169">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="150ca-169">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="150ca-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="150ca-170">Next steps</span></span>

<span data-ttu-id="150ca-171">Csomag rögzíti a virtuális gép a riasztások megtekintésével automatizálása [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="150ca-171">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="150ca-172">Keresése, ha bizonyos forgalom engedélyezett kívül a virtuális gép orr ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="150ca-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














