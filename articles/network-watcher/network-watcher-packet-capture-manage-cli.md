---
title: "Csomag rögzíti az Azure hálózati figyelőt - Azure CLI 2.0 kezelése |} Microsoft Docs"
description: "Ezen a lapon ismerteti, hogyan kezelheti a hálózati figyelőt az Azure CLI 2.0 verziót használja a csomag rögzítési szolgáltatása"
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
ms.openlocfilehash: c94eb46f31f2f19b843ccd7bf77b8a39943a07d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="bc3c8-103">Csomag rögzítésekre kezelése az Azure CLI 2.0 verziót használja Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="bc3c8-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="bc3c8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bc3c8-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="bc3c8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bc3c8-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="bc3c8-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bc3c8-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="bc3c8-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bc3c8-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="bc3c8-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="bc3c8-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="bc3c8-109">Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek nyomon követéséhez forgalma és a virtuális gép létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="bc3c8-110">Annak érdekében, hogy csak a kívánt forgalom rögzíti a rögzítési munkamenet szűrők célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="bc3c8-111">Csomagrögzítéssel segít diagnosztizálni hálózati rendellenességeket proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="bc3c8-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, hálózati behatolások, ügyfél-kiszolgáló közötti kommunikációt, és még sok más hibakeresési információkat való összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="bc3c8-113">Őket távolról elindítása csomag rögzíti, ez a funkció megkönnyíti a csomagrögzítéssel fut, manuálisan, a kívánt számítógépet, amely értékes időt takaríthat meg okozta terheket.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="bc3c8-114">Ez a cikk használja a következő generációs CLI a erőforrás management üzembe helyezési modellel, Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-114">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="bc3c8-115">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="bc3c8-115">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="bc3c8-116">Ez a cikk végigvezeti Önt a különböző felügyeleti feladatok, amelyek jelenleg a csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-116">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="bc3c8-117">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="bc3c8-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="bc3c8-118">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="bc3c8-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="bc3c8-119">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="bc3c8-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="bc3c8-120">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="bc3c8-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="bc3c8-121">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="bc3c8-121">Before you begin</span></span>

<span data-ttu-id="bc3c8-122">Ez a cikk feltételezi, hogy rendelkezik-e a következőket:</span><span class="sxs-lookup"><span data-stu-id="bc3c8-122">This article assumes you have the following resources:</span></span>

- <span data-ttu-id="bc3c8-123">A csomagrögzítéssel létrehozni kívánt hálózati figyelőt régióban példánya</span><span class="sxs-lookup"><span data-stu-id="bc3c8-123">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="bc3c8-124">A csomag rögzítési engedélyezett bővítményekhez virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-124">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc3c8-125">Csomagrögzítéssel megköveteli az ügynök a virtuális gépen kell futnia.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-125">Packet capture requires an agent to be running on the virtual machine.</span></span> <span data-ttu-id="bc3c8-126">Az ügynök telepítve van a kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-126">The Agent is installed as an extension.</span></span> <span data-ttu-id="bc3c8-127">Virtuálisgép-bővítmények útmutatásra van szüksége, látogasson el [virtuálisgép-bővítmények és a szolgáltatások](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="bc3c8-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="bc3c8-128">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="bc3c8-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="bc3c8-129">1. lépés</span><span class="sxs-lookup"><span data-stu-id="bc3c8-129">Step 1</span></span>

<span data-ttu-id="bc3c8-130">Futtassa a `az vm extension set` parancsmag a csomag rögzítési ügynök telepítéséhez a Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-130">Run the `az vm extension set` cmdlet to install the packet capture agent on the guest virtual machine.</span></span>

<span data-ttu-id="bc3c8-131">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="bc3c8-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="bc3c8-132">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="bc3c8-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="bc3c8-133">2. lépés</span><span class="sxs-lookup"><span data-stu-id="bc3c8-133">Step 2</span></span>

<span data-ttu-id="bc3c8-134">Győződjön meg arról, hogy az ügynök telepítve van-e, futtassa a `vm extension get` parancsmagot, és adja át az erőforrás-csoport és a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-134">To ensure that the agent is installed, run the `vm extension get` cmdlet and pass it the resource group and virtual machine name.</span></span> <span data-ttu-id="bc3c8-135">Ellenőrizze a megjelenő listában annak érdekében, hogy az ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-135">Check the resulting list to ensure the agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="bc3c8-136">Az alábbi minta futtatását a válasz példája`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="bc3c8-136">The following sample is an example of the response from running `az vm extension show`</span></span>

```json
{
  "autoUpgradeMinorVersion": true,
  "forceUpdateTag": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/extensions/NetworkWatcherAgentWindows",
  "instanceView": null,
  "location": "westcentralus",
  "name": "NetworkWatcherAgentWindows",
  "protectedSettings": null,
  "provisioningState": "Succeeded",
  "publisher": "Microsoft.Azure.NetworkWatcher",
  "resourceGroup": "{resourceGroupName}",
  "settings": null,
  "tags": null,
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "typeHandlerVersion": "1.4",
  "virtualMachineExtensionType": "NetworkWatcherAgentWindows"
}
```

## <a name="start-a-packet-capture"></a><span data-ttu-id="bc3c8-137">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="bc3c8-137">Start a packet capture</span></span>

<span data-ttu-id="bc3c8-138">Ha az előző lépések befejeződött, a csomag rögzítési ügynök telepítve van a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-138">Once the preceding steps are complete, the packet capture agent is installed on the virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="bc3c8-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="bc3c8-139">Step 1</span></span>

<span data-ttu-id="bc3c8-140">A következő lépésre hálózati figyelőt példányának lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-140">The next step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="bc3c8-141">A hálózati figyelőt Tnem neve átadott a `az network watcher show` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-141">TThe name of the Network Watcher is passed to the `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="bc3c8-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="bc3c8-142">Step 2</span></span>

<span data-ttu-id="bc3c8-143">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-143">Retrieve a storage account.</span></span> <span data-ttu-id="bc3c8-144">Ez a tárfiók a csomag rögzítési fájl tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-144">This storage account is used to store the packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="bc3c8-145">3. lépés</span><span class="sxs-lookup"><span data-stu-id="bc3c8-145">Step 3</span></span>

<span data-ttu-id="bc3c8-146">A csomagrögzítéssel által tárolt adatok szűrők használható.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-146">Filters can be used to limit the data that is stored by the packet capture.</span></span> <span data-ttu-id="bc3c8-147">Az alábbi példa állít be egy csomagrögzítéssel több szűrők.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-147">The following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="bc3c8-148">Az első három szűrők összegyűjtéséhez kimenő TCP-forgalom csak a helyi IP 10.0.0.3 célport 20, a 80-as és a 443-as.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-148">The first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 to destination ports 20, 80 and 443.</span></span>  <span data-ttu-id="bc3c8-149">Az utolsó szűrő gyűjt csak az UDP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-149">The last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="bc3c8-150">A következő példa egy futtatásának várható kimenete a `az network watcher packet-capture create` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-150">The following example is the expected output from running the `az network watcher packet-capture create` cmdlet.</span></span>

```json
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/pa
cketCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/p
roviders/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapture_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="get-a-packet-capture"></a><span data-ttu-id="bc3c8-151">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="bc3c8-151">Get a packet capture</span></span>

<span data-ttu-id="bc3c8-152">Fut a `az network watcher packet-capture show` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel állapotát.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-152">Running the `az network watcher packet-capture show` cmdlet, retrieves the status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="bc3c8-153">A következő példa egy kimenete a `az network watcher packet-capture show` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-153">The following example is the output from the `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="bc3c8-154">A következő példa a rögzítés befejezése után van.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-154">The following example is after the capture is complete.</span></span> <span data-ttu-id="bc3c8-155">A PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-155">The PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="bc3c8-156">Ezt az értéket jeleníti meg, hogy a csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-156">This value shows that the packet capture was successful and ran its time.</span></span>

```
{
  "bytesToCapturePerPacket": 0,
  "etag": "W/\"b8cf3528-2e14-45cb-a7f3-5712ffb687ac\"",
  "filters": [
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "20"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "80"
    },
    {
      "localIpAddress": "10.0.0.3",
      "localPort": "",
      "protocol": "TCP",
      "remoteIpAddress": "1.1.1.1-255.255.255",
      "remotePort": "443"
    },
    {
      "localIpAddress": "",
      "localPort": "",
      "protocol": "UDP",
      "remoteIpAddress": "",
      "remotePort": ""
    }
  ],
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/packetCaptureName",
  "name": "packetCaptureName",
  "provisioningState": "Succeeded",
  "resourceGroup": "NetworkWatcherRG",
  "storageLocation": {
    "filePath": null,
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/gwteststorage123abc",
    "storagePath": "https://gwteststorage123abc.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/{resourceGroupName}/providers/microsoft.compute/virtualmachines/{vmName}/2017/05/25/packetcapt
ure_16_22_34_630.cap"
  },
  "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}",
  "timeLimitInSeconds": 18000,
  "totalBytesPerSession": 1073741824
}
```

## <a name="stop-a-packet-capture"></a><span data-ttu-id="bc3c8-157">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="bc3c8-157">Stop a packet capture</span></span>

<span data-ttu-id="bc3c8-158">Futtassa a `az network watcher packet-capture stop` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-158">By running the `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="bc3c8-159">A parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-159">The cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="bc3c8-160">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="bc3c8-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="bc3c8-161">A csomagrögzítéssel törlése nem érinti a tárfiókban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-161">Deleting a packet capture does not delete the file in the storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="bc3c8-162">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="bc3c8-162">Download a packet capture</span></span>

<span data-ttu-id="bc3c8-163">A csomag rögzítési munkamenet befejezése után a rögzítési fájl is fel kell tölteni a blob storage vagy a virtuális gép helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-163">Once your packet capture session has completed, the capture file can be uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="bc3c8-164">A tárolási helye a csomagrögzítéssel definiálása a munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="bc3c8-164">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="bc3c8-165">Eszköz eléréséhez rögzítési-fájlokat egy tárfiókkal a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="bc3c8-165">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="bc3c8-166">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="bc3c8-166">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="bc3c8-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc3c8-167">Next steps</span></span>

<span data-ttu-id="bc3c8-168">Csomag rögzíti a virtuális gép a riasztások megtekintésével automatizálása [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="bc3c8-168">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="bc3c8-169">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="bc3c8-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
