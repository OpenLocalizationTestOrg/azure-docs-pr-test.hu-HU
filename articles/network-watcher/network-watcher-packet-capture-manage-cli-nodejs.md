---
title: "Csomag rögzíti az Azure hálózati figyelőt - Azure CLI 1.0 kezelése |} Microsoft Docs"
description: "Ezen a lapon ismerteti, hogyan kezelheti a csomag rögzítése funkció az Azure CLI 1.0 használatával hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 91588910334859c1ea77186674d5bfb31b311b36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="95da1-103">Csomag rögzíti kezelése az Azure CLI 1.0 használata Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="95da1-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="95da1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="95da1-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="95da1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="95da1-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="95da1-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="95da1-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="95da1-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="95da1-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="95da1-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="95da1-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="95da1-109">Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek nyomon követéséhez forgalma és a virtuális gép létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="95da1-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="95da1-110">Annak érdekében, hogy csak a kívánt forgalom rögzíti a rögzítési munkamenet szűrők célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="95da1-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="95da1-111">Csomagrögzítéssel segít diagnosztizálni hálózati rendellenességeket proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="95da1-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="95da1-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, hálózati behatolások, ügyfél-kiszolgáló közötti kommunikációt, és még sok más hibakeresési információkat való összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="95da1-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="95da1-113">Őket távolról elindítása csomag rögzíti, ez a funkció megkönnyíti a csomagrögzítéssel fut, manuálisan, a kívánt számítógépet, amely értékes időt takaríthat meg okozta terheket.</span><span class="sxs-lookup"><span data-stu-id="95da1-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="95da1-114">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="95da1-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="95da1-115">Ez a cikk végigvezeti Önt a különböző felügyeleti feladatok, amelyek jelenleg a csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="95da1-115">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="95da1-116">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="95da1-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="95da1-117">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="95da1-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="95da1-118">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="95da1-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="95da1-119">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="95da1-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="95da1-120">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="95da1-120">Before you begin</span></span>

<span data-ttu-id="95da1-121">Ez a cikk feltételezi, hogy rendelkezik-e a következőket:</span><span class="sxs-lookup"><span data-stu-id="95da1-121">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="95da1-122">A csomagrögzítéssel létrehozni kívánt hálózati figyelőt régióban példánya</span><span class="sxs-lookup"><span data-stu-id="95da1-122">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="95da1-123">A csomag rögzítési engedélyezett bővítményekhez virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="95da1-123">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="95da1-124">Csomagrögzítéssel megköveteli az ügynök a virtuális gépen kell futnia.</span><span class="sxs-lookup"><span data-stu-id="95da1-124">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="95da1-125">Az ügynök telepítve van a kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="95da1-125">The Agent is installed as an extension.</span></span> <span data-ttu-id="95da1-126">Virtuálisgép-bővítmények útmutatásra van szüksége, látogasson el [virtuálisgép-bővítmények és a szolgáltatások](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="95da1-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="95da1-127">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="95da1-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="95da1-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="95da1-128">Step 1</span></span>

<span data-ttu-id="95da1-129">Futtassa a `azure vm extension set` parancsmag a csomag rögzítési ügynök telepítéséhez a Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="95da1-129">Run the `azure vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="95da1-130">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="95da1-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="95da1-131">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="95da1-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="95da1-132">2. lépés</span><span class="sxs-lookup"><span data-stu-id="95da1-132">Step 2</span></span>

<span data-ttu-id="95da1-133">Győződjön meg arról, hogy az ügynök telepítve van-e, futtassa a `vm extension get` parancsmagot, és adja át az erőforrás-csoport és a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="95da1-133">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="95da1-134">Ellenőrizze a megjelenő listában annak érdekében, hogy az ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="95da1-134">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="95da1-135">Az alábbi minta futtatását a válasz példája`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="95da1-135">The following sample is an example of the response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up the VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="95da1-136">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="95da1-136">Start a packet capture</span></span>

<span data-ttu-id="95da1-137">Ha az előző lépések befejeződött, a csomag rögzítési ügynök telepítve van a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="95da1-137">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="95da1-138">1. lépés</span><span class="sxs-lookup"><span data-stu-id="95da1-138">Step 1</span></span>

<span data-ttu-id="95da1-139">A következő lépésre hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="95da1-139">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="95da1-140">Ez a változó átadott a `network watcher show` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="95da1-140">This variable is passed to the `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="95da1-141">2. lépés</span><span class="sxs-lookup"><span data-stu-id="95da1-141">Step 2</span></span>

<span data-ttu-id="95da1-142">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="95da1-142">Retrieve a storage account.</span></span> <span data-ttu-id="95da1-143">Ez a tárfiók a csomag rögzítési fájl tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="95da1-143">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="95da1-144">3. lépés</span><span class="sxs-lookup"><span data-stu-id="95da1-144">Step 3</span></span>

<span data-ttu-id="95da1-145">A csomagrögzítéssel által tárolt adatok szűrők használható.</span><span class="sxs-lookup"><span data-stu-id="95da1-145">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="95da1-146">Az alábbi példa állít be egy csomagrögzítéssel több szűrők.</span><span class="sxs-lookup"><span data-stu-id="95da1-146">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="95da1-147">Az első három szűrők összegyűjtéséhez kimenő TCP-forgalom csak a helyi IP 10.0.0.3 célport 20, a 80-as és a 443-as.</span><span class="sxs-lookup"><span data-stu-id="95da1-147">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="95da1-148">Az utolsó szűrő gyűjt csak az UDP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="95da1-148">The last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="95da1-149">A csomagrögzítéssel több szűrő adható meg.</span><span class="sxs-lookup"><span data-stu-id="95da1-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="95da1-150">Ha egy összetett struktúra használ, célszerű szűrők használata a json-fájl szintaktikai hibák elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="95da1-150">If you are using a complex filter structure, it is better to use filters as a json file to avoid syntax errors.</span></span> <span data-ttu-id="95da1-151">Például használja a jelzőt "-r" (ahelyett, hogy "-f"), és adja át a következő szűrőket tartalmazó json-fájl helyét:</span><span class="sxs-lookup"><span data-stu-id="95da1-151">For example, use the flag "-r" (instead of "-f") and pass the location of a json file containing the following filters:</span></span>

```json
[
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"20"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"80"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"443"
    },
    {
        "protocol":"UDP"
    }
]
```


<span data-ttu-id="95da1-152">A következő példa egy futtatásának várható kimenete a `network watcher packet-capture create` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="95da1-152">The following example is the expected output from running the `network watcher packet-capture create` cmdlet.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="95da1-153">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="95da1-153">Get a packet capture</span></span>

<span data-ttu-id="95da1-154">Fut a `network watcher packet-capture show` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel állapotát.</span><span class="sxs-lookup"><span data-stu-id="95da1-154">Running the `network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="95da1-155">A következő példa egy kimenete a `network watcher packet-capture show` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="95da1-155">The following example is the output from the `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="95da1-156">A következő példa a rögzítés befejezése után van.</span><span class="sxs-lookup"><span data-stu-id="95da1-156">The following example is after the capture is complete.</span></span> <span data-ttu-id="95da1-157">A PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="95da1-157">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="95da1-158">Ezt az értéket jeleníti meg, hogy a csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="95da1-158">This value shows that the packet capture was successful and ran its time.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="95da1-159">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="95da1-159">Stop a packet capture</span></span>

<span data-ttu-id="95da1-160">Futtassa a `network watcher packet-capture stop` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="95da1-160">By running the `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="95da1-161">A parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="95da1-161">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="95da1-162">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="95da1-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="95da1-163">A csomagrögzítéssel törlése nem érinti a tárfiókban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="95da1-163">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="95da1-164">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="95da1-164">Download a packet capture</span></span>

<span data-ttu-id="95da1-165">A csomag rögzítési munkamenet befejezése után a rögzítési fájl is fel kell tölteni a blob storage vagy a virtuális gép helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="95da1-165">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="95da1-166">A tárolási helye a csomagrögzítéssel definiálása a munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="95da1-166">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="95da1-167">Eszköz eléréséhez rögzítési-fájlokat egy tárfiókkal a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="95da1-167">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="95da1-168">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="95da1-168">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="95da1-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95da1-169">Next steps</span></span>

<span data-ttu-id="95da1-170">Csomag rögzíti a virtuális gép a riasztások megtekintésével automatizálása [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="95da1-170">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="95da1-171">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="95da1-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
