---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - PowerShell |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása a PowerShell-parancsmag"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: b7dbe6e44e0fa1ea0481223a84098d7a57c068a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="bb818-103">Virtuális hálózati átjáró és az Azure hálózati figyelő PowerShell kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="bb818-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="bb818-104">Portál</span><span class="sxs-lookup"><span data-stu-id="bb818-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="bb818-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bb818-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="bb818-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="bb818-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="bb818-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bb818-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="bb818-108">REST API</span><span class="sxs-lookup"><span data-stu-id="bb818-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="bb818-109">Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bb818-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="bb818-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="bb818-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="bb818-111">Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="bb818-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="bb818-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="bb818-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bb818-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="bb818-113">Before you begin</span></span>

<span data-ttu-id="bb818-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="bb818-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="bb818-115">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="bb818-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="bb818-116">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bb818-116">Overview</span></span>

<span data-ttu-id="bb818-117">Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="bb818-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="bb818-118">A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni.</span><span class="sxs-lookup"><span data-stu-id="bb818-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="bb818-119">Ha vizsgálat befejeződött, hello eredmény akkor minősül.</span><span class="sxs-lookup"><span data-stu-id="bb818-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="bb818-120">Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt.</span><span class="sxs-lookup"><span data-stu-id="bb818-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="bb818-121">a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="bb818-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="bb818-122">Hálózati figyelőt beolvasása</span><span class="sxs-lookup"><span data-stu-id="bb818-122">Retrieve Network Watcher</span></span>

<span data-ttu-id="bb818-123">hello első lépéseként tooretrieve hello hálózati figyelőt példány.</span><span class="sxs-lookup"><span data-stu-id="bb818-123">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="bb818-124">Hello `$networkWatcher` változó átadása toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag a 4. lépésben.</span><span class="sxs-lookup"><span data-stu-id="bb818-124">hello `$networkWatcher` variable is passed toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="bb818-125">A virtuális hálózati átjáró kapcsolat beolvasása</span><span class="sxs-lookup"><span data-stu-id="bb818-125">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="bb818-126">Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="bb818-126">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="bb818-127">Is átadhatja azt a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="bb818-127">You can also pass it a Virtual Network Gateway.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="bb818-128">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="bb818-128">Create a storage account</span></span>

<span data-ttu-id="bb818-129">Hello hello erőforrás állapotának adatait erőforrás hibakeresési adja vissza, naplók tooa tárolási fiók toobe felül is menti.</span><span class="sxs-lookup"><span data-stu-id="bb818-129">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="bb818-130">Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.</span><span class="sxs-lookup"><span data-stu-id="bb818-130">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="bb818-131">Futtassa a hálózati figyelőt erőforrás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="bb818-131">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="bb818-132">Hibaelhárítás hello erőforrások `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="bb818-132">You troubleshoot resources with hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span></span> <span data-ttu-id="bb818-133">Azt adja át a hello parancsmag hello hálózati figyelőt objektum, a hello hello kapcsolat azonosítója vagy a virtuális hálózati átjáró, a hello tárfiók azonosítója és a hello elérési toostore hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="bb818-133">We pass hello cmdlet hello Network Watcher object, hello Id of hello Connection or Virtual Network Gateway, hello storage account id, and hello path toostore hello results.</span></span>

> [!NOTE]
> <span data-ttu-id="bb818-134">Hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` parancsmag hosszú ideig futó és eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="bb818-134">hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet is long running and may take a few minutes toocomplete.</span></span>

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

<span data-ttu-id="bb818-135">Hello parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy hello erőforrás tooverify hello állapota.</span><span class="sxs-lookup"><span data-stu-id="bb818-135">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="bb818-136">Toohello rendszerhéj hello eredményeket ad vissza, és hello eredmények naplók megadott hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="bb818-136">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="bb818-137">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="bb818-137">Understanding hello results</span></span>

<span data-ttu-id="bb818-138">hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást.</span><span class="sxs-lookup"><span data-stu-id="bb818-138">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="bb818-139">Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="bb818-139">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="bb818-140">Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="bb818-140">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="bb818-141">Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="bb818-141">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="bb818-142">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="bb818-142">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="bb818-143">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="bb818-143">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="bb818-144">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="bb818-144">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb818-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb818-145">Next steps</span></span>

<span data-ttu-id="bb818-146">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="bb818-146">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
