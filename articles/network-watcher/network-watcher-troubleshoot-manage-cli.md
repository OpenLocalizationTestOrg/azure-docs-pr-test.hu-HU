---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása Azure CLI 2.0"
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
ms.openlocfilehash: 389e86f247722412a1d0e2e722dbf80bb7cff291
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a><span data-ttu-id="f0783-103">Virtuális hálózati átjáró és az Azure hálózati figyelő Azure CLI 2.0 verziót használja kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="f0783-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f0783-104">Portál</span><span class="sxs-lookup"><span data-stu-id="f0783-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="f0783-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0783-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="f0783-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f0783-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="f0783-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f0783-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="f0783-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f0783-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="f0783-109">Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f0783-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="f0783-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="f0783-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="f0783-111">Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="f0783-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f0783-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="f0783-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="f0783-113">Ez a cikk használja a következő generációs CLI hello erőforrás management üzembe helyezési modelljével Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="f0783-113">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="f0783-114">tooperform hello ebben a cikkben ismertetett visszaállítási lépésekkel, túl kell[hello Azure parancssori felület Mac, Linux és Windows (Azure CLI) telepítése](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f0783-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f0783-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f0783-115">Before you begin</span></span>

<span data-ttu-id="f0783-116">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="f0783-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="f0783-117">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="f0783-117">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="f0783-118">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f0783-118">Overview</span></span>

<span data-ttu-id="f0783-119">Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="f0783-119">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="f0783-120">A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni.</span><span class="sxs-lookup"><span data-stu-id="f0783-120">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="f0783-121">Ha vizsgálat befejeződött, hello eredmény akkor minősül.</span><span class="sxs-lookup"><span data-stu-id="f0783-121">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="f0783-122">Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt.</span><span class="sxs-lookup"><span data-stu-id="f0783-122">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="f0783-123">a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="f0783-123">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="f0783-124">A virtuális hálózati átjáró kapcsolat beolvasása</span><span class="sxs-lookup"><span data-stu-id="f0783-124">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="f0783-125">Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f0783-125">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="f0783-126">Is átadhatja azt a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="f0783-126">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="f0783-127">hello következő parancsmag listája hello vpn-kapcsolatok erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="f0783-127">hello following cmdlet lists hello vpn-connections in a resource group.</span></span>

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

<span data-ttu-id="f0783-128">Miután hello hello kapcsolat neve, futtathatja az erőforrás-azonosítót a parancs tooget:</span><span class="sxs-lookup"><span data-stu-id="f0783-128">Once you have hello name of hello connection, you can run this command tooget its resource Id:</span></span>

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a><span data-ttu-id="f0783-129">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="f0783-129">Create a storage account</span></span>

<span data-ttu-id="f0783-130">Hello hello erőforrás állapotának adatait erőforrás hibakeresési adja vissza, naplók tooa tárolási fiók toobe felül is menti.</span><span class="sxs-lookup"><span data-stu-id="f0783-130">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="f0783-131">Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.</span><span class="sxs-lookup"><span data-stu-id="f0783-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="f0783-132">Hello storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0783-132">Create hello storage account</span></span>

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. <span data-ttu-id="f0783-133">Hello tárfiókkulcsok beolvasása</span><span class="sxs-lookup"><span data-stu-id="f0783-133">Get hello storage account keys</span></span>

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. <span data-ttu-id="f0783-134">Hello tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0783-134">Create hello container</span></span>

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="f0783-135">Futtassa a hálózati figyelőt erőforrás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="f0783-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="f0783-136">Hibaelhárítás hello erőforrások `az network watcher troubleshooting` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f0783-136">You troubleshoot resources with hello `az network watcher troubleshooting` cmdlet.</span></span> <span data-ttu-id="f0783-137">Hello parancsmag hello erőforráscsoport továbbítja azt, hello hello hálózati figyelőt, hello hello kapcsolat azonosítója neve hello hello tárfiók azonosítója és hello elérési toohello blob toostore hello hibaelhárításához eredményez.</span><span class="sxs-lookup"><span data-stu-id="f0783-137">We pass hello cmdlet hello resource group, hello name of hello Network Watcher, hello Id of hello connection, hello Id of hello storage account, and hello path toohello blob toostore hello troubleshoot results in.</span></span>

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

<span data-ttu-id="f0783-138">Hello parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy hello erőforrás tooverify hello állapota.</span><span class="sxs-lookup"><span data-stu-id="f0783-138">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="f0783-139">Toohello rendszerhéj hello eredményeket ad vissza, és hello eredmények naplók megadott hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="f0783-139">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="f0783-140">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="f0783-140">Understanding hello results</span></span>

<span data-ttu-id="f0783-141">hello művelet szöveg hogyan tooresolve hello probléma nyújt általános útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f0783-141">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="f0783-142">Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f0783-142">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="f0783-143">Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="f0783-143">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="f0783-144">Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f0783-144">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="f0783-145">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="f0783-145">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="f0783-146">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="f0783-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="f0783-147">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="f0783-147">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0783-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0783-148">Next steps</span></span>

<span data-ttu-id="f0783-149">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="f0783-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
