---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - Azure CLI 1.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomag rögzítése funkció az Azure CLI 1.0 használatával hálózati figyelőt"
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
ms.openlocfilehash: c4b710a8d82ccaaf65876a8c2ef845aa97b5f831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="29b90-103">Csomag rögzíti kezelése az Azure CLI 1.0 használata Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="29b90-103">Manage packet captures with Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="29b90-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="29b90-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="29b90-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="29b90-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="29b90-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="29b90-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="29b90-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="29b90-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="29b90-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="29b90-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="29b90-109">Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="29b90-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="29b90-110">Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti.</span><span class="sxs-lookup"><span data-stu-id="29b90-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="29b90-111">Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="29b90-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="29b90-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="29b90-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="29b90-113">Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="29b90-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="29b90-114">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="29b90-114">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="29b90-115">Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="29b90-115">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="29b90-116">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="29b90-116">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="29b90-117">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="29b90-117">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="29b90-118">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="29b90-118">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="29b90-119">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="29b90-119">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="29b90-120">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="29b90-120">Before you begin</span></span>

<span data-ttu-id="29b90-121">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="29b90-121">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="29b90-122">Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban</span><span class="sxs-lookup"><span data-stu-id="29b90-122">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="29b90-123">Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.</span><span class="sxs-lookup"><span data-stu-id="29b90-123">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29b90-124">Csomagrögzítéssel egy hello virtuális gépen futó ügynök toobe igényel.</span><span class="sxs-lookup"><span data-stu-id="29b90-124">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="29b90-125">egy bővítmény hello ügynök van telepítve.</span><span class="sxs-lookup"><span data-stu-id="29b90-125">hello Agent is installed as an extension.</span></span> <span data-ttu-id="29b90-126">Virtuálisgép-bővítmények útmutatásra van szüksége, látogasson el [virtuálisgép-bővítmények és a szolgáltatások](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="29b90-126">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="29b90-127">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="29b90-127">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="29b90-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29b90-128">Step 1</span></span>

<span data-ttu-id="29b90-129">Futtassa a hello `azure vm extension set` parancsmag tooinstall hello csomagok rögzítési ügynök hello Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="29b90-129">Run hello `azure vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="29b90-130">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="29b90-130">For Windows virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentWindows -o 1.4
```

<span data-ttu-id="29b90-131">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="29b90-131">For Linux virtual machines:</span></span>

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r AzureNetworkWatcherExtension -n NetworkWatcherAgentLinux -o 1.4
````

### <a name="step-2"></a><span data-ttu-id="29b90-132">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29b90-132">Step 2</span></span>

<span data-ttu-id="29b90-133">tooensure, amely hello ügynök van telepítve, futtassa hello `vm extension get` parancsmagot, és adja át hello erőforráscsoport és a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="29b90-133">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="29b90-134">Ellenőrizze, hogy hello eredményül kapott lista tooensure hello ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="29b90-134">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

<span data-ttu-id="29b90-135">a következő minta hello hello válasz futtatását példája`azure vm extension get`</span><span class="sxs-lookup"><span data-stu-id="29b90-135">hello following sample is an example of hello response from running `azure vm extension get`</span></span>

```
info:    Executing command vm extension get
+ Looking up hello VM "virtualMachineName"
data:    Publisher                       Name                        Version  State
data:    ------------------------------  -----------------------     -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentWindows  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="29b90-136">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="29b90-136">Start a packet capture</span></span>

<span data-ttu-id="29b90-137">Ha hello előző lépések befejeződött, hello csomagok rögzítési ügynök hello virtuális gépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="29b90-137">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="29b90-138">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29b90-138">Step 1</span></span>

<span data-ttu-id="29b90-139">hello tovább tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="29b90-139">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="29b90-140">Ez a változó átadása toohello `network watcher show` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="29b90-140">This variable is passed toohello `network watcher show` cmdlet in step 4.</span></span>

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="29b90-141">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29b90-141">Step 2</span></span>

<span data-ttu-id="29b90-142">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="29b90-142">Retrieve a storage account.</span></span> <span data-ttu-id="29b90-143">Ez a tárfiók használt toostore hello csomagok rögzítési fájl.</span><span class="sxs-lookup"><span data-stu-id="29b90-143">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="29b90-144">3. lépés</span><span class="sxs-lookup"><span data-stu-id="29b90-144">Step 3</span></span>

<span data-ttu-id="29b90-145">Szűrők használt toolimit hello hello csomagrögzítéssel által tárolt adatokat is lehet.</span><span class="sxs-lookup"><span data-stu-id="29b90-145">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="29b90-146">hello alábbi példa állít be egy csomagrögzítéssel több szűrők.</span><span class="sxs-lookup"><span data-stu-id="29b90-146">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="29b90-147">hello első három szűrők gyűjteni kimenő TCP-forgalom csak helyi IP 10.0.0.3 toodestination 20, a 80-as és a 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="29b90-147">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="29b90-148">hello utolsó szűrő csak a UDP-forgalom gyűjti.</span><span class="sxs-lookup"><span data-stu-id="29b90-148">hello last filter collects only UDP traffic.</span></span>

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="29b90-149">A csomagrögzítéssel több szűrő adható meg.</span><span class="sxs-lookup"><span data-stu-id="29b90-149">Multiple filters can be defined for a packet capture.</span></span> <span data-ttu-id="29b90-150">Egy összetett struktúra használ, esetén jobb toouse szűrők, egy json-fájl tooavoid szintaktikai hibákat.</span><span class="sxs-lookup"><span data-stu-id="29b90-150">If you are using a complex filter structure, it is better toouse filters as a json file tooavoid syntax errors.</span></span> <span data-ttu-id="29b90-151">Például használja az hello jelzőt "-r" (ahelyett, hogy "-f"), és adja át a következő szűrők hello tartalmazó json-fájlt hello helye:</span><span class="sxs-lookup"><span data-stu-id="29b90-151">For example, use hello flag "-r" (instead of "-f") and pass hello location of a json file containing hello following filters:</span></span>

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


<span data-ttu-id="29b90-152">hello alábbi példa egy hello várható kimenet hello futtatását `network watcher packet-capture create` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="29b90-152">hello following example is hello expected output from running hello `network watcher packet-capture create` cmdlet.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="29b90-153">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="29b90-153">Get a packet capture</span></span>

<span data-ttu-id="29b90-154">Hello futtató `network watcher packet-capture show` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="29b90-154">Running hello `network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

<span data-ttu-id="29b90-155">hello alábbi példa: hello hello kimenete `network watcher packet-capture show` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="29b90-155">hello following example is hello output from hello `network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="29b90-156">hello következő példa egy hello rögzítési befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="29b90-156">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="29b90-157">hello PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="29b90-157">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="29b90-158">Ezt az értéket jeleníti meg, hogy hello csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="29b90-158">This value shows that hello packet capture was successful and ran its time.</span></span>

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes tooCapture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="29b90-159">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="29b90-159">Stop a packet capture</span></span>

<span data-ttu-id="29b90-160">Hello futtatásával `network watcher packet-capture stop` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="29b90-160">By running hello `network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="29b90-161">hello parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="29b90-161">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="29b90-162">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="29b90-162">Delete a packet capture</span></span>

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> <span data-ttu-id="29b90-163">A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="29b90-163">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="29b90-164">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="29b90-164">Download a packet capture</span></span>

<span data-ttu-id="29b90-165">A csomag rögzítési munkamenet befejezése után hello rögzítési fájl lehet feltöltött tooblob tooa vagy a helyi fájl a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="29b90-165">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="29b90-166">hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="29b90-166">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="29b90-167">Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="29b90-167">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="29b90-168">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="29b90-168">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="29b90-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29b90-169">Next steps</span></span>

<span data-ttu-id="29b90-170">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="29b90-170">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="29b90-171">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="29b90-171">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
