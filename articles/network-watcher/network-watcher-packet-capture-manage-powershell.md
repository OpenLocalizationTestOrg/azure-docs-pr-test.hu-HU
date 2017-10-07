---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - PowerShell |} Microsoft Docs"
description: "Ezen a lapon azt ismerteti, hogyan toomanage hello csomagok rögzítése a PowerShell használatával hálózati figyelőt szolgáltatása"
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
ms.openlocfilehash: 77a522a1b05e020a73ba7140c1410615eb8761da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="34b5c-103">Csomag rögzítésekre kezelése a PowerShell használata Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="34b5c-103">Manage packet captures with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="34b5c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="34b5c-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="34b5c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="34b5c-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="34b5c-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="34b5c-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="34b5c-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="34b5c-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="34b5c-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="34b5c-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="34b5c-109">Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="34b5c-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="34b5c-110">Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti.</span><span class="sxs-lookup"><span data-stu-id="34b5c-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="34b5c-111">Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="34b5c-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="34b5c-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="34b5c-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="34b5c-113">Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="34b5c-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="34b5c-114">Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="34b5c-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="34b5c-115">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="34b5c-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="34b5c-116">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="34b5c-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="34b5c-117">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="34b5c-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="34b5c-118">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="34b5c-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="34b5c-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="34b5c-119">Before you begin</span></span>

<span data-ttu-id="34b5c-120">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="34b5c-120">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="34b5c-121">Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban</span><span class="sxs-lookup"><span data-stu-id="34b5c-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>

* <span data-ttu-id="34b5c-122">Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.</span><span class="sxs-lookup"><span data-stu-id="34b5c-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="34b5c-123">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="34b5c-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="34b5c-124">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="34b5c-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="34b5c-125">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="34b5c-125">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="34b5c-126">1. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-126">Step 1</span></span>

```powershell
$VM = Get-AzureRmVM -ResourceGroupName testrg -Name VM1
```

### <a name="step-2"></a><span data-ttu-id="34b5c-127">2. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-127">Step 2</span></span>

<span data-ttu-id="34b5c-128">hello lekéri hello sémakiterjesztési adatok, a következő példa szükséges toorun hello `Set-AzureRmVMExtension` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="34b5c-128">hello following example retrieves hello extension information needed toorun hello `Set-AzureRmVMExtension` cmdlet.</span></span> <span data-ttu-id="34b5c-129">Ez a parancsmag hello csomagok rögzítési ügynököt telepít hello Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="34b5c-129">This cmdlet installs hello packet capture agent on hello guest virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="34b5c-130">Hello `Set-AzureRmVMExtension` parancsmag eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="34b5c-130">hello `Set-AzureRmVMExtension` cmdlet may take several minutes toocomplete.</span></span>

<span data-ttu-id="34b5c-131">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="34b5c-131">For Windows virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentWindows -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
```

<span data-ttu-id="34b5c-132">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="34b5c-132">For Linux virtual machines:</span></span>

```powershell
$AzureNetworkWatcherExtension = Get-AzureRmVMExtensionImage -Location WestCentralUS -PublisherName Microsoft.Azure.NetworkWatcher -Type NetworkWatcherAgentLinux -Version 1.4.13.0
$ExtensionName = "AzureNetworkWatcherExtension"
Set-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -Location $VM.Location -VMName $VM.Name -Name $ExtensionName -Publisher $AzureNetworkWatcherExtension.PublisherName -ExtensionType $AzureNetworkWatcherExtension.Type -TypeHandlerVersion $AzureNetworkWatcherExtension.Version.Substring(0,3)
````

<span data-ttu-id="34b5c-133">hello következő példája a sikeres válasz hello futtatása után `Set-AzureRmVMExtension` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="34b5c-133">hello following example is a successful response after running hello `Set-AzureRmVMExtension` cmdlet.</span></span>

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
```

### <a name="step-3"></a><span data-ttu-id="34b5c-134">3. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-134">Step 3</span></span>

<span data-ttu-id="34b5c-135">tooensure, amely hello ügynök van telepítve, futtassa hello `Get-AzureRmVMExtension` parancsmagot, és adja át hello virtuális gép nevét és hello bővítmény neve.</span><span class="sxs-lookup"><span data-stu-id="34b5c-135">tooensure that hello agent is installed, run hello `Get-AzureRmVMExtension` cmdlet and pass it hello virtual machine name and hello extension name.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName $VM.ResourceGroupName  -VMName $VM.Name -Name $ExtensionName
```

<span data-ttu-id="34b5c-136">a következő minta hello hello válasz futtatását példája`Get-AzureRmVMExtension`</span><span class="sxs-lookup"><span data-stu-id="34b5c-136">hello following sample is an example of hello response from running `Get-AzureRmVMExtension`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="34b5c-137">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="34b5c-137">Start a packet capture</span></span>

<span data-ttu-id="34b5c-138">Ha hello előző lépések befejeződött, hello csomagok rögzítési ügynök hello virtuális gépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="34b5c-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="34b5c-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-139">Step 1</span></span>

<span data-ttu-id="34b5c-140">hello tovább tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="34b5c-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="34b5c-141">Ez a változó átadása toohello `New-AzureRmNetworkWatcherPacketCapture` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="34b5c-141">This variable is passed toohello `New-AzureRmNetworkWatcherPacketCapture` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName  
```

### <a name="step-2"></a><span data-ttu-id="34b5c-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-142">Step 2</span></span>

<span data-ttu-id="34b5c-143">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="34b5c-143">Retrieve a storage account.</span></span> <span data-ttu-id="34b5c-144">Ez a tárfiók használt toostore hello csomagok rögzítési fájl.</span><span class="sxs-lookup"><span data-stu-id="34b5c-144">This storage account is used toostore hello packet capture file.</span></span>

```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName testrg -Name testrgsa123
```

### <a name="step-3"></a><span data-ttu-id="34b5c-145">3. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-145">Step 3</span></span>

<span data-ttu-id="34b5c-146">Szűrők használt toolimit hello hello csomagrögzítéssel által tárolt adatokat is lehet.</span><span class="sxs-lookup"><span data-stu-id="34b5c-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="34b5c-147">hello alábbi példa hoz létre két szűrőt.</span><span class="sxs-lookup"><span data-stu-id="34b5c-147">hello following example sets up two filters.</span></span>  <span data-ttu-id="34b5c-148">Egy szűrő gyűjti a TCP kimenő forgalom csak a helyi IP 10.0.0.3 toodestination 20, a 80-as és a 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="34b5c-148">One filter collects outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="34b5c-149">hello második szűrő csak a UDP-forgalom gyűjti.</span><span class="sxs-lookup"><span data-stu-id="34b5c-149">hello second filter collects only UDP traffic.</span></span>

```powershell
$filter1 = New-AzureRmPacketCaptureFilterConfig -Protocol TCP -RemoteIPAddress "1.1.1.1-255.255.255" -LocalIPAddress "10.0.0.3" -LocalPort "1-65535" -RemotePort "20;80;443"
$filter2 = New-AzureRmPacketCaptureFilterConfig -Protocol UDP
```

> [!NOTE]
> <span data-ttu-id="34b5c-150">A csomagrögzítéssel több szűrő adható meg.</span><span class="sxs-lookup"><span data-stu-id="34b5c-150">Multiple filters can be defined for a packet capture.</span></span>

### <a name="step-4"></a><span data-ttu-id="34b5c-151">4. lépés</span><span class="sxs-lookup"><span data-stu-id="34b5c-151">Step 4</span></span>

<span data-ttu-id="34b5c-152">Futtassa a hello `New-AzureRmNetworkWatcherPacketCapture` parancsmag toostart hello csomagok rögzítési folyamat, hello szükséges értékeket az előző lépésekben hello beolvasott átadásakor.</span><span class="sxs-lookup"><span data-stu-id="34b5c-152">Run hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet toostart hello packet capture process, passing hello required values retrieved in hello preceding steps.</span></span>
```powershell

New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $vm.Id -PacketCaptureName "PacketCaptureTest" -StorageAccountId $storageAccount.id -TimeLimitInSeconds 60 -Filters $filter1, $filter2
```

<span data-ttu-id="34b5c-153">hello alábbi példa egy hello várható kimenet hello futtatását `New-AzureRmNetworkWatcherPacketCapture` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="34b5c-153">hello following example is hello expected output from running hello `New-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="34b5c-154">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="34b5c-154">Get a packet capture</span></span>

<span data-ttu-id="34b5c-155">Hello futtató `Get-AzureRmNetworkWatcherPacketCapture` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="34b5c-155">Running hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```powershell
Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

<span data-ttu-id="34b5c-156">hello alábbi példa: hello hello kimenete `Get-AzureRmNetworkWatcherPacketCapture` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="34b5c-156">hello following example is hello output from hello `Get-AzureRmNetworkWatcherPacketCapture` cmdlet.</span></span> <span data-ttu-id="34b5c-157">hello következő példa egy hello rögzítési befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="34b5c-157">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="34b5c-158">hello PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="34b5c-158">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="34b5c-159">Ezt az értéket jeleníti meg, hogy hello csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="34b5c-159">This value shows that hello packet capture was successful and ran its time.</span></span>
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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="34b5c-160">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="34b5c-160">Stop a packet capture</span></span>

<span data-ttu-id="34b5c-161">Hello futtatásával `Stop-AzureRmNetworkWatcherPacketCapture` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="34b5c-161">By running hello `Stop-AzureRmNetworkWatcherPacketCapture` cmdlet, if a capture session is in progress it is stopped.</span></span>

```powershell
Stop-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="34b5c-162">hello parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="34b5c-162">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="34b5c-163">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="34b5c-163">Delete a packet capture</span></span>

```powershell
Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName "PacketCaptureTest"
```

> [!NOTE]
> <span data-ttu-id="34b5c-164">A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="34b5c-164">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="34b5c-165">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="34b5c-165">Download a packet capture</span></span>

<span data-ttu-id="34b5c-166">A csomag rögzítési munkamenet befejezése után hello rögzítési fájl lehet feltöltött tooblob tooa vagy a helyi fájl a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="34b5c-166">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="34b5c-167">hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="34b5c-167">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="34b5c-168">Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="34b5c-168">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="34b5c-169">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="34b5c-169">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="34b5c-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34b5c-170">Next steps</span></span>

<span data-ttu-id="34b5c-171">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="34b5c-171">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="34b5c-172">Keresése, ha bizonyos forgalom engedélyezett kívül a virtuális gép orr ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="34b5c-172">Find if certain traffic is allowed in orr out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->














