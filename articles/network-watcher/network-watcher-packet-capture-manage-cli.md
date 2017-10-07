---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomag rögzítése funkció az Azure CLI 2.0 használatával hálózati figyelőt"
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
ms.openlocfilehash: d19cb7d0ca3b7a9bc0546859e07ef6d4df4f42d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="2d5cb-103">Csomag rögzítésekre kezelése az Azure CLI 2.0 verziót használja Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="2d5cb-103">Manage packet captures with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="2d5cb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2d5cb-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="2d5cb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2d5cb-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="2d5cb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2d5cb-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="2d5cb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2d5cb-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="2d5cb-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="2d5cb-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="2d5cb-109">Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="2d5cb-110">Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="2d5cb-111">Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="2d5cb-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="2d5cb-113">Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="2d5cb-114">Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-114">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="2d5cb-115">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="2d5cb-115">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

<span data-ttu-id="2d5cb-116">Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-116">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="2d5cb-117">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="2d5cb-117">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="2d5cb-118">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="2d5cb-118">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="2d5cb-119">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="2d5cb-119">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="2d5cb-120">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="2d5cb-120">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="2d5cb-121">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="2d5cb-121">Before you begin</span></span>

<span data-ttu-id="2d5cb-122">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="2d5cb-122">This article assumes you have hello following resources:</span></span>

- <span data-ttu-id="2d5cb-123">Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban</span><span class="sxs-lookup"><span data-stu-id="2d5cb-123">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="2d5cb-124">Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-124">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d5cb-125">Csomagrögzítéssel egy hello virtuális gépen futó ügynök toobe igényel.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-125">Packet capture requires an agent toobe running on hello virtual machine.</span></span> <span data-ttu-id="2d5cb-126">egy bővítmény hello ügynök van telepítve.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-126">hello Agent is installed as an extension.</span></span> <span data-ttu-id="2d5cb-127">Virtuálisgép-bővítmények útmutatásra van szüksége, látogasson el [virtuálisgép-bővítmények és a szolgáltatások](../virtual-machines/windows/extensions-features.md).</span><span class="sxs-lookup"><span data-stu-id="2d5cb-127">For instructions on VM extensions, visit [Virtual Machine extensions and features](../virtual-machines/windows/extensions-features.md).</span></span>

## <a name="install-vm-extension"></a><span data-ttu-id="2d5cb-128">Virtuálisgép-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="2d5cb-128">Install VM extension</span></span>

### <a name="step-1"></a><span data-ttu-id="2d5cb-129">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2d5cb-129">Step 1</span></span>

<span data-ttu-id="2d5cb-130">Futtassa a hello `az vm extension set` parancsmag tooinstall hello csomagok rögzítési ügynök hello Vendég virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-130">Run hello `az vm extension set` cmdlet tooinstall hello packet capture agent on hello guest virtual machine.</span></span>

<span data-ttu-id="2d5cb-131">A Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="2d5cb-131">For Windows virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentWindows --version 1.4
```

<span data-ttu-id="2d5cb-132">A Linux virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="2d5cb-132">For Linux virtual machines:</span></span>

```azurecli
az vm extension set --resource-group resourceGroupName --vm-name virtualMachineName --publisher Microsoft.Azure.NetworkWatcher --name NetworkWatcherAgentLinux--version 1.4
````

### <a name="step-2"></a><span data-ttu-id="2d5cb-133">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2d5cb-133">Step 2</span></span>

<span data-ttu-id="2d5cb-134">tooensure, amely hello ügynök van telepítve, futtassa hello `vm extension get` parancsmagot, és adja át hello erőforráscsoport és a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-134">tooensure that hello agent is installed, run hello `vm extension get` cmdlet and pass it hello resource group and virtual machine name.</span></span> <span data-ttu-id="2d5cb-135">Ellenőrizze, hogy hello eredményül kapott lista tooensure hello ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-135">Check hello resulting list tooensure hello agent is installed.</span></span>

```azurecli
az vm extension show -resource-group resourceGroupName --vm-name virtualMachineName --name NetworkWatcherAgentWindows
```

<span data-ttu-id="2d5cb-136">a következő minta hello hello válasz futtatását példája`az vm extension show`</span><span class="sxs-lookup"><span data-stu-id="2d5cb-136">hello following sample is an example of hello response from running `az vm extension show`</span></span>

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

## <a name="start-a-packet-capture"></a><span data-ttu-id="2d5cb-137">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="2d5cb-137">Start a packet capture</span></span>

<span data-ttu-id="2d5cb-138">Ha hello előző lépések befejeződött, hello csomagok rögzítési ügynök hello virtuális gépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-138">Once hello preceding steps are complete, hello packet capture agent is installed on hello virtual machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="2d5cb-139">1. lépés</span><span class="sxs-lookup"><span data-stu-id="2d5cb-139">Step 1</span></span>

<span data-ttu-id="2d5cb-140">hello tovább tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-140">hello next step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="2d5cb-141">Hello hálózati figyelőt Tnem neve átadása toohello `az network watcher show` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-141">TThe name of hello Network Watcher is passed toohello `az network watcher show` cmdlet in step 4.</span></span>

```azurecli
az network watcher show -resource-group resourceGroup -name networkWatcherName
```

### <a name="step-2"></a><span data-ttu-id="2d5cb-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="2d5cb-142">Step 2</span></span>

<span data-ttu-id="2d5cb-143">A storage-fiók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-143">Retrieve a storage account.</span></span> <span data-ttu-id="2d5cb-144">Ez a tárfiók használt toostore hello csomagok rögzítési fájl.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-144">This storage account is used toostore hello packet capture file.</span></span>

```azurecli
azure storage account list
```

### <a name="step-3"></a><span data-ttu-id="2d5cb-145">3. lépés</span><span class="sxs-lookup"><span data-stu-id="2d5cb-145">Step 3</span></span>

<span data-ttu-id="2d5cb-146">Szűrők használt toolimit hello hello csomagrögzítéssel által tárolt adatokat is lehet.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-146">Filters can be used toolimit hello data that is stored by hello packet capture.</span></span> <span data-ttu-id="2d5cb-147">hello alábbi példa állít be egy csomagrögzítéssel több szűrők.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-147">hello following example sets up a packet capture with several  filters.</span></span>  <span data-ttu-id="2d5cb-148">hello első három szűrők gyűjteni kimenő TCP-forgalom csak helyi IP 10.0.0.3 toodestination 20, a 80-as és a 443-as portot.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-148">hello first three filters collect outgoing TCP traffic only from local IP 10.0.0.3 toodestination ports 20, 80 and 443.</span></span>  <span data-ttu-id="2d5cb-149">hello utolsó szűrő csak a UDP-forgalom gyűjti.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-149">hello last filter collects only UDP traffic.</span></span>

```azurecli
az network watcher packet-capture create --resource-group {resoureceurceGroupName} --vm {vmName} --name packetCaptureName --storage-account gwteststorage123abc --filters "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

<span data-ttu-id="2d5cb-150">hello alábbi példa egy hello várható kimenet hello futtatását `az network watcher packet-capture create` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-150">hello following example is hello expected output from running hello `az network watcher packet-capture create` cmdlet.</span></span>

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

## <a name="get-a-packet-capture"></a><span data-ttu-id="2d5cb-151">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="2d5cb-151">Get a packet capture</span></span>

<span data-ttu-id="2d5cb-152">Hello futtató `az network watcher packet-capture show` parancsmag, beolvassa a jelenleg futó vagy befejezett csomagrögzítéssel hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-152">Running hello `az network watcher packet-capture show` cmdlet, retrieves hello status of a currently running, or completed packet capture.</span></span>

```azurecli
az network watcher packet-capture show --name packetCaptureName --location westcentralus
```

<span data-ttu-id="2d5cb-153">hello alábbi példa: hello hello kimenete `az network watcher packet-capture show` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-153">hello following example is hello output from hello `az network watcher packet-capture show` cmdlet.</span></span> <span data-ttu-id="2d5cb-154">hello következő példa egy hello rögzítési befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-154">hello following example is after hello capture is complete.</span></span> <span data-ttu-id="2d5cb-155">hello PacketCaptureStatus érték le van állítva, az egy TimeExceeded StopReason.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-155">hello PacketCaptureStatus value is Stopped, with a StopReason of TimeExceeded.</span></span> <span data-ttu-id="2d5cb-156">Ezt az értéket jeleníti meg, hogy hello csomagrögzítéssel sikeres volt-e, és az idő futott.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-156">This value shows that hello packet capture was successful and ran its time.</span></span>

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

## <a name="stop-a-packet-capture"></a><span data-ttu-id="2d5cb-157">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="2d5cb-157">Stop a packet capture</span></span>

<span data-ttu-id="2d5cb-158">Hello futtatásával `az network watcher packet-capture stop` parancsmagot, ha a rögzítési munkamenet folyamatban az le van állítva.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-158">By running hello `az network watcher packet-capture stop` cmdlet, if a capture session is in progress it is stopped.</span></span>

```azurecli
az network watcher packet-capture stop --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="2d5cb-159">hello parancsmag nem választ ad vissza. Ha a jelenleg futó rögzítési munkamenet, vagy már leállt a meglévő munkamenetekben futtatott.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-159">hello cmdlet returns no response when ran on a currently running capture session or an existing session that has already stopped.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="2d5cb-160">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="2d5cb-160">Delete a packet capture</span></span>

```azurecli
az network watcher packet-capture delete --name packetCaptureName --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="2d5cb-161">A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-161">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="2d5cb-162">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="2d5cb-162">Download a packet capture</span></span>

<span data-ttu-id="2d5cb-163">A csomag rögzítési munkamenet befejezése után hello rögzítési fájl lehet feltöltött tooblob tooa vagy a helyi fájl a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-163">Once your packet capture session has completed, hello capture file can be uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="2d5cb-164">hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="2d5cb-164">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="2d5cb-165">Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="2d5cb-165">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="2d5cb-166">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="2d5cb-166">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="2d5cb-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d5cb-167">Next steps</span></span>

<span data-ttu-id="2d5cb-168">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="2d5cb-168">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="2d5cb-169">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2d5cb-169">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
