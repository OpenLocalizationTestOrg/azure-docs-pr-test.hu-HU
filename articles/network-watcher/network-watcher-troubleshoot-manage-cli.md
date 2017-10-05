---
title: "Az Azure virtuális hálózati átjáró és a kapcsolatok - Azure CLI 2.0 hibaelhárítása |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan Azure hálózati figyelőt Azure CLI 2.0 hibaelhárítása"
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
ms.openlocfilehash: e1d56317b10fa738a3d7089f6c4f357159fe2c2b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a><span data-ttu-id="f00b5-103">Virtuális hálózati átjáró és az Azure hálózati figyelő Azure CLI 2.0 verziót használja kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="f00b5-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f00b5-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f00b5-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="f00b5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f00b5-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="f00b5-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f00b5-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="f00b5-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f00b5-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="f00b5-108">REST API</span><span class="sxs-lookup"><span data-stu-id="f00b5-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="f00b5-109">Hálózati figyelőt sok képességeket biztosít, a hálózati erőforrások az Azure-ban ismertetése vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f00b5-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="f00b5-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="f00b5-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="f00b5-111">Erőforrások hibaelhárítása hívható a portálon, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="f00b5-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f00b5-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat állapotát, és visszaadja az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="f00b5-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="f00b5-113">Ez a cikk használja a következő generációs CLI a erőforrás management üzembe helyezési modellel, Azure CLI 2.0, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="f00b5-113">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="f00b5-114">Ebben a cikkben szereplő lépések végrehajtásához kell [telepítse az Azure parancssori felület Mac, Linux és Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f00b5-114">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f00b5-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f00b5-115">Before you begin</span></span>

<span data-ttu-id="f00b5-116">Ez a forgatókönyv azt feltételezi, hogy már követte lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) létrehozása egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="f00b5-116">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="f00b5-117">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="f00b5-117">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="f00b5-118">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f00b5-118">Overview</span></span>

<span data-ttu-id="f00b5-119">Erőforrás hibakeresési lehetővé teszi a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="f00b5-119">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="f00b5-120">A kérelem erőforrás hibáinak elhárításához, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni.</span><span class="sxs-lookup"><span data-stu-id="f00b5-120">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="f00b5-121">Ha vizsgálat befejeződött, a rendszer visszairányítja az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="f00b5-121">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="f00b5-122">Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, amely több percig is beletelhet visszaküldeni az eredményt.</span><span class="sxs-lookup"><span data-stu-id="f00b5-122">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="f00b5-123">A naplók hibaelhárítási egy tárolót, a megadott tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="f00b5-123">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="f00b5-124">A virtuális hálózati átjáró kapcsolat beolvasása</span><span class="sxs-lookup"><span data-stu-id="f00b5-124">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="f00b5-125">Ebben a példában erőforrás hibakeresési van folyamatban futtatott egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f00b5-125">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="f00b5-126">Is átadhatja azt a virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="f00b5-126">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="f00b5-127">A következő parancsmagot a vpn-kapcsolatok a erőforráscsoport sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="f00b5-127">The following cmdlet lists the vpn-connections in a resource group.</span></span>

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

<span data-ttu-id="f00b5-128">Miután a kapcsolat neve, futtathatja a parancsot az erőforrás-azonosítót:</span><span class="sxs-lookup"><span data-stu-id="f00b5-128">Once you have the name of the connection, you can run this command to get its resource Id:</span></span>

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a><span data-ttu-id="f00b5-129">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="f00b5-129">Create a storage account</span></span>

<span data-ttu-id="f00b5-130">Erőforrás hibakeresési erőforrás állapotával kapcsolatos adatokat ad vissza, a naplók tárfiókba vizsgálni is menti.</span><span class="sxs-lookup"><span data-stu-id="f00b5-130">Resource troubleshooting returns data about the health of the resource, it also saves logs to a storage account to be reviewed.</span></span> <span data-ttu-id="f00b5-131">Ebben a lépésben létrehozhatunk egy tárfiókot, ha létezik-e egy meglévő tárfiók használata.</span><span class="sxs-lookup"><span data-stu-id="f00b5-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="f00b5-132">A storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f00b5-132">Create the storage account</span></span>

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. <span data-ttu-id="f00b5-133">A tárfiók kulcsait beolvasása</span><span class="sxs-lookup"><span data-stu-id="f00b5-133">Get the storage account keys</span></span>

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. <span data-ttu-id="f00b5-134">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f00b5-134">Create the container</span></span>

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="f00b5-135">Futtassa a hálózati figyelőt erőforrás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="f00b5-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="f00b5-136">Hibaelhárítás az erőforrások a `az network watcher troubleshooting` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="f00b5-136">You troubleshoot resources with the `az network watcher troubleshooting` cmdlet.</span></span> <span data-ttu-id="f00b5-137">Azt át a parancsmag az erőforráscsoportot, a hálózati figyelőt, a kapcsolat, a tárfiók Id azonosítója neve és elérési útját a hibaelhárítás tárolni a blob eredményez.</span><span class="sxs-lookup"><span data-stu-id="f00b5-137">We pass the cmdlet the resource group, the name of the Network Watcher, the Id of the connection, the Id of the storage account, and the path to the blob to store the troubleshoot results in.</span></span>

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

<span data-ttu-id="f00b5-138">A parancsmag futtatása után hálózati figyelőt ellenőrzi, hogy az erőforrás állapotát ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="f00b5-138">Once you run the cmdlet, Network Watcher reviews the resource to verify the health.</span></span> <span data-ttu-id="f00b5-139">Az eredményt ad vissza. a rendszerhéj, és az eredmények naplók a megadott tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="f00b5-139">It returns the results to the shell and stores logs of the results in the storage account specified.</span></span>

## <a name="understanding-the-results"></a><span data-ttu-id="f00b5-140">Az eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="f00b5-140">Understanding the results</span></span>

<span data-ttu-id="f00b5-141">A művelet szöveg általános útmutatást biztosít a probléma megoldására.</span><span class="sxs-lookup"><span data-stu-id="f00b5-141">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="f00b5-142">Is művelet az a probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f00b5-142">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="f00b5-143">Abban az esetben nincs további útmutatás, ha a válasz biztosít nyissa meg a támogatási esetet URL-címét.</span><span class="sxs-lookup"><span data-stu-id="f00b5-143">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="f00b5-144">A válasz és tartalmát képező tulajdonságaival kapcsolatos további információkért látogasson el a [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f00b5-144">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="f00b5-145">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="f00b5-145">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="f00b5-146">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="f00b5-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="f00b5-147">Tártallózó további információt itt található: a következő hivatkozásra: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="f00b5-147">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f00b5-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f00b5-148">Next steps</span></span>

<span data-ttu-id="f00b5-149">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) nyomon követheti a hálózati biztonsági csoport és a biztonsági szabályok, amelyek lehet, hogy a szóban forgó.</span><span class="sxs-lookup"><span data-stu-id="f00b5-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
