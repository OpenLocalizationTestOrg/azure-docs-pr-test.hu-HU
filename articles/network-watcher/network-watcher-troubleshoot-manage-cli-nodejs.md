---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - Azure CLI 1.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: a0511689d773f9c7216b0fe5470e27f754eb600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a><span data-ttu-id="13abb-103">Virtuális hálózati átjáró és az Azure hálózati figyelő Azure CLI 1.0 használatával kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="13abb-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="13abb-104">Portál</span><span class="sxs-lookup"><span data-stu-id="13abb-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="13abb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="13abb-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="13abb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="13abb-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="13abb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="13abb-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="13abb-108">REST API</span><span class="sxs-lookup"><span data-stu-id="13abb-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="13abb-109">Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="13abb-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="13abb-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="13abb-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="13abb-111">Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="13abb-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="13abb-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="13abb-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="13abb-113">Ebben a cikkben az Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="13abb-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="13abb-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="13abb-114">Before you begin</span></span>

<span data-ttu-id="13abb-115">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="13abb-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="13abb-116">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="13abb-116">For a list of supported gateway types visit, [Supported Gateway types](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="13abb-117">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="13abb-117">Overview</span></span>

<span data-ttu-id="13abb-118">Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="13abb-118">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="13abb-119">A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni.</span><span class="sxs-lookup"><span data-stu-id="13abb-119">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="13abb-120">Ha vizsgálat befejeződött, hello eredmény akkor minősül.</span><span class="sxs-lookup"><span data-stu-id="13abb-120">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="13abb-121">Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt.</span><span class="sxs-lookup"><span data-stu-id="13abb-121">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="13abb-122">a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="13abb-122">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="13abb-123">A virtuális hálózati átjáró kapcsolat beolvasása</span><span class="sxs-lookup"><span data-stu-id="13abb-123">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="13abb-124">Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="13abb-124">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="13abb-125">Is átadhatja azt a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="13abb-125">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="13abb-126">hello következő parancsmag listája hello vpn-kapcsolatok erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="13abb-126">hello following cmdlet lists hello vpn-connections in a resource group.</span></span>

```azurecli
azure network vpn-connection list -g resourceGroupName
```

<span data-ttu-id="13abb-127">Is futtathatja hello parancs toosee hello kapcsolatok egy előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="13abb-127">You can also run hello command toosee hello connections in a subscription.</span></span>

```azurecli
azure network vpn-connection list -s subscription
```

<span data-ttu-id="13abb-128">Miután hello hello kapcsolat neve, futtathatja az erőforrás-azonosítót a parancs tooget:</span><span class="sxs-lookup"><span data-stu-id="13abb-128">Once you have hello name of hello connection, you can run this command tooget its resource Id:</span></span>

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a><span data-ttu-id="13abb-129">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="13abb-129">Create a storage account</span></span>

<span data-ttu-id="13abb-130">Hello hello erőforrás állapotának adatait erőforrás hibakeresési adja vissza, naplók tooa tárolási fiók toobe felül is menti.</span><span class="sxs-lookup"><span data-stu-id="13abb-130">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="13abb-131">Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.</span><span class="sxs-lookup"><span data-stu-id="13abb-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="13abb-132">Hello storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="13abb-132">Create hello storage account</span></span>

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. <span data-ttu-id="13abb-133">Hello tárfiókkulcsok beolvasása</span><span class="sxs-lookup"><span data-stu-id="13abb-133">Get hello storage account keys</span></span>

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. <span data-ttu-id="13abb-134">Hello tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="13abb-134">Create hello container</span></span>

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="13abb-135">Futtassa a hálózati figyelőt erőforrás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="13abb-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="13abb-136">Hibaelhárítás hello erőforrások `network watcher troubleshoot` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="13abb-136">You troubleshoot resources with hello `network watcher troubleshoot` cmdlet.</span></span> <span data-ttu-id="13abb-137">Hello parancsmag hello erőforráscsoport továbbítja azt, hello hello hálózati figyelőt, hello hello kapcsolat azonosítója neve hello hello tárfiók azonosítója és hello elérési toohello blob toostore hello hibaelhárításához eredményez.</span><span class="sxs-lookup"><span data-stu-id="13abb-137">We pass hello cmdlet hello resource group, hello name of hello Network Watcher, hello Id of hello connection, hello Id of hello storage account, and hello path toohello blob toostore hello troubleshoot results in.</span></span>

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

<span data-ttu-id="13abb-138">Hello parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy hello erőforrás tooverify hello állapota.</span><span class="sxs-lookup"><span data-stu-id="13abb-138">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="13abb-139">Toohello rendszerhéj hello eredményeket ad vissza, és hello eredmények naplók megadott hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="13abb-139">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="13abb-140">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="13abb-140">Understanding hello results</span></span>

<span data-ttu-id="13abb-141">hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást.</span><span class="sxs-lookup"><span data-stu-id="13abb-141">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="13abb-142">Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="13abb-142">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="13abb-143">Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="13abb-143">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="13abb-144">Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="13abb-144">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="13abb-145">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="13abb-145">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="13abb-146">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="13abb-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="13abb-147">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="13abb-147">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="13abb-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13abb-148">Next steps</span></span>

<span data-ttu-id="13abb-149">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="13abb-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
