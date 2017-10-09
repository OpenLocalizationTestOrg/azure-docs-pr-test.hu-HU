---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - REST API |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomagok rögzítési szolgáltatása hálózati figyelőt Azure REST API használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 53fe0324-835f-4005-afc8-145eeb314aeb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7a531fbe796e85e94961bd192d171defb299be05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="aa32a-103">Csomag rögzítésekre kezelése Azure hálózati figyelőt Azure REST API használatával</span><span class="sxs-lookup"><span data-stu-id="aa32a-103">Manage packet captures with Azure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="aa32a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="aa32a-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="aa32a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa32a-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="aa32a-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="aa32a-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="aa32a-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="aa32a-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="aa32a-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="aa32a-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="aa32a-109">Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="aa32a-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="aa32a-110">Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti.</span><span class="sxs-lookup"><span data-stu-id="aa32a-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="aa32a-111">Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="aa32a-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="aa32a-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="aa32a-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="aa32a-113">Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="aa32a-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="aa32a-114">Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="aa32a-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="aa32a-115">**A csomagrögzítéssel beolvasása**</span><span class="sxs-lookup"><span data-stu-id="aa32a-115">**Get a packet capture**</span></span>](#get-a-packet-capture)
- [<span data-ttu-id="aa32a-116">**Összes csomag rögzítésekre felsorolása**</span><span class="sxs-lookup"><span data-stu-id="aa32a-116">**List all packet captures**</span></span>](#list-all-packet-captures)
- [<span data-ttu-id="aa32a-117">**A csomagrögzítéssel hello állapotának lekérdezése**</span><span class="sxs-lookup"><span data-stu-id="aa32a-117">**Query hello status of a packet capture**</span></span>](#query-packet-capture-status)
- [<span data-ttu-id="aa32a-118">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="aa32a-118">**Start a packet capture**</span></span>](#start-packet-capture)
- [<span data-ttu-id="aa32a-119">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="aa32a-119">**Stop a packet capture**</span></span>](#stop-packet-capture)
- [<span data-ttu-id="aa32a-120">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="aa32a-120">**Delete a packet capture**</span></span>](#delete-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="aa32a-121">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="aa32a-121">Before you begin</span></span>

<span data-ttu-id="aa32a-122">Ebben a forgatókönyvben hello hálózati figyelő Rest API toorun IP Flow ellenőrizze hívható meg.</span><span class="sxs-lookup"><span data-stu-id="aa32a-122">In this scenario, you call hello Network Watcher Rest API toorun IP Flow Verify.</span></span> <span data-ttu-id="aa32a-123">ARMclient használt toocall hello REST API használatával PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa32a-123">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="aa32a-124">ARMClient verziója van telepítve, chocolatey [a Chocolatey ARMClient](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="aa32a-124">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="aa32a-125">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="aa32a-125">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> <span data-ttu-id="aa32a-126">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="aa32a-126">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="aa32a-127">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="aa32a-127">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="aa32a-128">Jelentkezzen be ARMClient</span><span class="sxs-lookup"><span data-stu-id="aa32a-128">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="aa32a-129">A virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="aa32a-129">Retrieve a virtual machine</span></span>

<span data-ttu-id="aa32a-130">A következő parancsfájl tooreturn hello egy virtuális gép futtatásához.</span><span class="sxs-lookup"><span data-stu-id="aa32a-130">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="aa32a-131">Ezek az információk szükségesek a csomagrögzítéssel indításához.</span><span class="sxs-lookup"><span data-stu-id="aa32a-131">This information is needed for starting a packet capture.</span></span>

<span data-ttu-id="aa32a-132">a következő kódot kell változók hello:</span><span class="sxs-lookup"><span data-stu-id="aa32a-132">hello following code needs variables:</span></span>

- <span data-ttu-id="aa32a-133">**a subscriptionId** -hello előfizetés-azonosító is lehet beolvasni a hello **Get-AzureRMSubscription** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aa32a-133">**subscriptionId** - hello subscription id can also be retrieved with hello **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="aa32a-134">**resourceGroupName** – hello virtuális gépeket tartalmazó erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="aa32a-134">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="aa32a-135">Kimeneti hello alábbi hello azonosító hello virtuális gép hello a következő példában használják.</span><span class="sxs-lookup"><span data-stu-id="aa32a-135">From hello following output, hello id of hello virtual machine is used in hello next example.</span></span>

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


## <a name="get-a-packet-capture"></a><span data-ttu-id="aa32a-136">A csomagrögzítéssel beolvasása</span><span class="sxs-lookup"><span data-stu-id="aa32a-136">Get a packet capture</span></span>

<span data-ttu-id="aa32a-137">hello alábbi példa lekérdezi egy egyetlen csomagrögzítéssel hello állapota</span><span class="sxs-lookup"><span data-stu-id="aa32a-137">hello following example gets hello status of a single packet capture</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="aa32a-138">hello következő válaszok példák tipikus választ adott vissza, amikor egy csomagrögzítéssel hello állapotának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="aa32a-138">hello following responses are examples of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Running",
  "packetCaptureError": []
}
```

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded",
  "packetCaptureError": []
}
```

## <a name="list-all-packet-captures"></a><span data-ttu-id="aa32a-139">Összes csomag rögzítésekre felsorolása</span><span class="sxs-lookup"><span data-stu-id="aa32a-139">List all packet captures</span></span>

<span data-ttu-id="aa32a-140">a következő példa hello lekérdezi összes csomagot rögzítési munkamenet régióban.</span><span class="sxs-lookup"><span data-stu-id="aa32a-140">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

<span data-ttu-id="aa32a-141">hello következő válasz rögzíti egy példa egy tipikus válasza, az összes csomag beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="aa32a-141">hello following response is an example of a typical response returned when getting all packet captures</span></span>

```json
{
  "value": [
    {
      "name": "TestPacketCapture6",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Succeeded",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_19_53_056.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    },
    {
      "name": "TestPacketCapture7",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture7",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Failed",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_23_15_364.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    }
  ]
}
```

## <a name="query-packet-capture-status"></a><span data-ttu-id="aa32a-142">Lekérdezés csomag rögzítési állapota</span><span class="sxs-lookup"><span data-stu-id="aa32a-142">Query packet capture status</span></span>

<span data-ttu-id="aa32a-143">a következő példa hello lekérdezi összes csomagot rögzítési munkamenet régióban.</span><span class="sxs-lookup"><span data-stu-id="aa32a-143">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="aa32a-144">hello következő válasz példája egy tipikus válasz egy csomagrögzítéssel hello állapotának lekérdezése adott vissza.</span><span class="sxs-lookup"><span data-stu-id="aa32a-144">hello following response is an example of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
    "name": "vm1PacketCapture",     "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatche rName}/packetCaptures/{packetCaptureName}",
   "captureStartTime" : "9/7/2016 12:35:24PM",
   "packetCaptureStatus" : "Stopped",
   "stopReason" : "TimeExceeded"
   "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a><span data-ttu-id="aa32a-145">Indítsa el a csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="aa32a-145">Start packet capture</span></span>

<span data-ttu-id="aa32a-146">a következő példa hello egy csomagrögzítéssel egy virtuális gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aa32a-146">hello following example creates a packet capture on a virtual machine.</span></span>  <span data-ttu-id="aa32a-147">hello példája paraméteres tooallow létrehozása, például a rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="aa32a-147">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
$storageaccountname = "contosoexamplergdiag374"
$vmName = "ContosoVM"
$bytestoCaptureperPacket = "0"
$bytesPerSession = "1073741824"
$captureTimeinSeconds = "60"
$localIP = ""
$localPort = "" # Examples are: 80, or 80-120
$remoteIP = ""
$remotePort = "" # Examples are: 80, or 80-120
$protocol = "" # Valid values are TCP, UDP and Any.
$targetUri = "" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName
$storageId = "" # Example: "https://mytestaccountname.blob.core.windows.net/capture/vm1Capture.cap"
$storagePath = ""
$localFilePath = "c:\\temp\\packetcapture.cap" # Example: "d:\capture\vm1Capture.cap"

$requestBody = @"
{
    'properties':  {
                       'target':  '/${targetUri}',
                       'bytesToCapturePerPacket':  '${bytestoCaptureperPacket}',
                       'totalBytesPerSession':  '${bytesPerSession}',
                       'timeLimitinSeconds':  '${captureTimeinSeconds}',
                       'storageLocation':  {
                                               'storageId':  '${storageId}',
                                               'storagePath':  '${storagePath}',
                                               'filePath':  '${localFilePath}'
                                           },
                       'filters':  [
                                       {
                                           'protocol':  '${protocol}',
                                           'localIPAddress':  '${localIP}',
                                           'localPort':  '${localPort}',
                                           'remoteIPAddress':  '${remoteIP}',
                                           'remotePort':  '${remotePort}'
                                       }
                                   ]
                   }
}
"@

armclient PUT "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-07-01" $requestbody
```

## <a name="stop-packet-capture"></a><span data-ttu-id="aa32a-148">Állítsa le a csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="aa32a-148">Stop packet capture</span></span>

<span data-ttu-id="aa32a-149">a következő példa hello egy csomagrögzítéssel leállítja a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="aa32a-149">hello following example stops a packet capture on a virtual machine.</span></span>  <span data-ttu-id="aa32a-150">hello példája paraméteres tooallow létrehozása, például a rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="aa32a-150">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a><span data-ttu-id="aa32a-151">Csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="aa32a-151">Delete packet capture</span></span>

<span data-ttu-id="aa32a-152">a következő példa hello törli a csomagrögzítéssel virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="aa32a-152">hello following example deletes a packet capture on a virtual machine.</span></span>  <span data-ttu-id="aa32a-153">hello példája paraméteres tooallow létrehozása, például a rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="aa32a-153">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> <span data-ttu-id="aa32a-154">A csomagrögzítéssel törlése nem érinti hello tárfiókban hello fájl</span><span class="sxs-lookup"><span data-stu-id="aa32a-154">Deleting a packet capture does not delete hello file in hello storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa32a-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa32a-155">Next steps</span></span>

<span data-ttu-id="aa32a-156">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="aa32a-156">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="aa32a-157">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="aa32a-157">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="aa32a-158">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="aa32a-158">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

<span data-ttu-id="aa32a-159">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="aa32a-159">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>













