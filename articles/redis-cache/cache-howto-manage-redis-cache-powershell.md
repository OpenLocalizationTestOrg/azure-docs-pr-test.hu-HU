---
title: "Azure Redis gyorsítótár Azure PowerShell kezelése |} Microsoft Docs"
description: "Útmutató az Azure PowerShell Azure Redis Cache felügyeleti feladatokat hajthat végre."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 0a5c95eab3fd01f611fc049e80c5c506857e0b81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a><span data-ttu-id="7ad23-103">Azure Redis gyorsítótár Azure PowerShell kezelése</span><span class="sxs-lookup"><span data-stu-id="7ad23-103">Manage Azure Redis Cache with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ad23-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ad23-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="7ad23-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7ad23-105">Azure CLI</span></span>](cache-manage-cli.md)
> 
> 

<span data-ttu-id="7ad23-106">Ez a témakör bemutatja, hogyan végrehajtásához gyakori feladatokat, mint létrehozása, frissítése, és a méret az Azure Redis Cache példányt, a tárelérési kulcsok újragenerálása, és tekintse meg a gyorsítótárak.</span><span class="sxs-lookup"><span data-stu-id="7ad23-106">This topic shows you how to perform common tasks such as create, update, and scale your Azure Redis Cache instances, how to regenerate access keys, and how to view information about your caches.</span></span> <span data-ttu-id="7ad23-107">Azure Redis Cache PowerShell-parancsmagok teljes listáját lásd: [Azure Redis Cache parancsmagok](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ad23-107">For a complete list of Azure Redis Cache PowerShell cmdlets, see [Azure Redis Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="7ad23-108">A klasszikus üzembe helyezési modellel kapcsolatos további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellek és az erőforrások állapotát](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span><span class="sxs-lookup"><span data-stu-id="7ad23-108">For more information about the classic deployment model, see [Azure Resource Manager vs. classic deployment: Understand deployment models and the state of your resources](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ad23-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7ad23-109">Prerequisites</span></span>
<span data-ttu-id="7ad23-110">Ha már telepítette az Azure PowerShell, rendelkeznie kell Azure PowerShell 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7ad23-110">If you have already installed Azure PowerShell, you must have Azure PowerShell version 1.0.0 or later.</span></span> <span data-ttu-id="7ad23-111">Ellenőrizheti az Azure PowerShell ezzel a paranccsal, az Azure PowerShell-parancssorba telepített verzióját.</span><span class="sxs-lookup"><span data-stu-id="7ad23-111">You can check the version of Azure PowerShell that you have installed with this command at the Azure PowerShell command prompt.</span></span>

    Get-Module azure | format-table version


<span data-ttu-id="7ad23-112">Először be kell jelentkezni Azure ezzel a paranccsal.</span><span class="sxs-lookup"><span data-stu-id="7ad23-112">First, you must log in to Azure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="7ad23-113">A Microsoft Azure bejelentkezési párbeszédpanelen adja meg az e-mail cím, az Azure-fiókjával, és a hozzá tartozó jelszó.</span><span class="sxs-lookup"><span data-stu-id="7ad23-113">Specify the email address of your Azure account and its password in the Microsoft Azure sign-in dialog.</span></span>

<span data-ttu-id="7ad23-114">Ezután ha több Azure-előfizetéssel rendelkezik, be kell az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="7ad23-114">Next, if you have multiple Azure subscriptions, you need to set your Azure subscription.</span></span> <span data-ttu-id="7ad23-115">Az aktuális előfizetések listájának megtekintéséhez futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-115">To see a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="7ad23-116">Adja meg az előfizetést, futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-116">To specify the subscription, run the following command.</span></span> <span data-ttu-id="7ad23-117">A következő példában az előfizetés neve: `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="7ad23-117">In the following example, the subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

<span data-ttu-id="7ad23-118">A Windows PowerShell használhatja az Azure Resource Manager, a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="7ad23-118">Before you can use Windows PowerShell with Azure Resource Manager, you need the following:</span></span>

* <span data-ttu-id="7ad23-119">Windows PowerShell 3.0 vagy 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="7ad23-119">Windows PowerShell, Version 3.0 or 4.0.</span></span> <span data-ttu-id="7ad23-120">A Windows PowerShell verziója található, írja be a következőt:`$PSVersionTable` , és ellenőrizze a értékének `PSVersion` 3.0 vagy 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="7ad23-120">To find the version of Windows PowerShell, type:`$PSVersionTable` and verify the value of `PSVersion` is 3.0 or 4.0.</span></span> <span data-ttu-id="7ad23-121">Telepítsen egy kompatibilis verziót, lásd: [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) vagy [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="7ad23-121">To install a compatible version, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

<span data-ttu-id="7ad23-122">Ebben az oktatóanyagban látja parancsmagokhoz részletes segítséget kérhet, használja a Get-Help parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-122">To get detailed help for any cmdlet you see in this tutorial, use the Get-Help cmdlet.</span></span>

    Get-Help <cmdlet-name> -Detailed

<span data-ttu-id="7ad23-123">Segítség a példában a `New-AzureRmRedisCache` parancsmag, típus:</span><span class="sxs-lookup"><span data-stu-id="7ad23-123">For example, to get help for the `New-AzureRmRedisCache` cmdlet, type:</span></span>

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a><span data-ttu-id="7ad23-124">Más felhők csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="7ad23-124">How to connect to other clouds</span></span>
<span data-ttu-id="7ad23-125">Az Azure alapértelmezés szerint a környezete `AzureCloud`, amely globális Azure felhőben példányt jelenti.</span><span class="sxs-lookup"><span data-stu-id="7ad23-125">By default the Azure environment is `AzureCloud`, which represents the global Azure cloud instance.</span></span> <span data-ttu-id="7ad23-126">Szeretne csatlakozni egy másik példányt, használja a `Add-AzureRmAccount` parancsot a `-Environment` vagy -`EnvironmentName` parancssori kapcsolóval a kívánt környezetre vagy a környezet neve.</span><span class="sxs-lookup"><span data-stu-id="7ad23-126">To connect to a different instance, use the `Add-AzureRmAccount` command with the `-Environment` or -`EnvironmentName` command line switch with the desired environment or environment name.</span></span>

<span data-ttu-id="7ad23-127">Rendelkezésre álló környezeteket listájának megtekintéséhez futtassa a `Get-AzureRmEnvironment` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-127">To see the list of available environments, run the `Get-AzureRmEnvironment` cmdlet.</span></span>

### <a name="to-connect-to-the-azure-government-cloud"></a><span data-ttu-id="7ad23-128">Az Azure Government felhőbe való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="7ad23-128">To connect to the Azure Government Cloud</span></span>
<span data-ttu-id="7ad23-129">Az Azure Government felhő csatlakozni, használja a következő parancsok egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-129">To connect to the Azure Government Cloud, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

<span data-ttu-id="7ad23-130">vagy</span><span class="sxs-lookup"><span data-stu-id="7ad23-130">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

<span data-ttu-id="7ad23-131">A gyorsítótár Azure Government felhőalapú létrehozásához használja az alábbi helyek egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-131">To create a cache in the Azure Government Cloud, use one of the following locations.</span></span>

* <span data-ttu-id="7ad23-132">USGov Virginia</span><span class="sxs-lookup"><span data-stu-id="7ad23-132">USGov Virginia</span></span>
* <span data-ttu-id="7ad23-133">USGov Iowa</span><span class="sxs-lookup"><span data-stu-id="7ad23-133">USGov Iowa</span></span>

<span data-ttu-id="7ad23-134">Az Azure Government felhő kapcsolatos további információkért lásd: [a Microsoft Azure Government](https://azure.microsoft.com/features/gov/) és [Microsoft Azure Government – útmutató fejlesztőknek](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="7ad23-134">For more information about the Azure Government Cloud, see [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) and [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>

### <a name="to-connect-to-the-azure-china-cloud"></a><span data-ttu-id="7ad23-135">Az Azure Kína felhőbe való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="7ad23-135">To connect to the Azure China Cloud</span></span>
<span data-ttu-id="7ad23-136">Az Azure Kína felhő csatlakozni, használja a következő parancsok egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-136">To connect to the Azure China Cloud, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

<span data-ttu-id="7ad23-137">vagy</span><span class="sxs-lookup"><span data-stu-id="7ad23-137">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

<span data-ttu-id="7ad23-138">A gyorsítótár Azure Kína felhőalapú létrehozásához használja az alábbi helyek egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-138">To create a cache in the Azure China Cloud, use one of the following locations.</span></span>

* <span data-ttu-id="7ad23-139">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="7ad23-139">China East</span></span>
* <span data-ttu-id="7ad23-140">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="7ad23-140">China North</span></span>

<span data-ttu-id="7ad23-141">Az Azure Kína felhő kapcsolatos további információkért lásd: [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="7ad23-141">For more information about the Azure China Cloud, see [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span>

### <a name="to-connect-to-microsoft-azure-germany"></a><span data-ttu-id="7ad23-142">Csatlakozni a Microsoft Azure-Németország</span><span class="sxs-lookup"><span data-stu-id="7ad23-142">To connect to Microsoft Azure Germany</span></span>
<span data-ttu-id="7ad23-143">A Microsoft Azure Németország csatlakozni, használja a következő parancsok egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-143">To connect to Microsoft Azure Germany, use one of the following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


<span data-ttu-id="7ad23-144">vagy</span><span class="sxs-lookup"><span data-stu-id="7ad23-144">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

<span data-ttu-id="7ad23-145">A Microsoft Azure Németországban a gyorsítótár létrehozásához használja az alábbi helyek egyikét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-145">To create a cache in Microsoft Azure Germany, use one of the following locations.</span></span>

* <span data-ttu-id="7ad23-146">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="7ad23-146">Germany Central</span></span>
* <span data-ttu-id="7ad23-147">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="7ad23-147">Germany Northeast</span></span>

<span data-ttu-id="7ad23-148">A Microsoft Azure Németország kapcsolatos további információkért lásd: [Microsoft Azure Németország](https://azure.microsoft.com/overview/clouds/germany/).</span><span class="sxs-lookup"><span data-stu-id="7ad23-148">For more information about Microsoft Azure Germany, see [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).</span></span>

### <a name="properties-used-for-azure-redis-cache-powershell"></a><span data-ttu-id="7ad23-149">Azure Redis Cache PowerShell használt tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="7ad23-149">Properties used for Azure Redis Cache PowerShell</span></span>
<span data-ttu-id="7ad23-150">A következő táblázat a tulajdonságok és a gyakran használt paraméterek létrehozása és kezelése az Azure Redis Cache példányt az Azure PowerShell leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ad23-150">The following table contains properties and descriptions for commonly used parameters when creating and managing your Azure Redis Cache instances using Azure PowerShell.</span></span>

| <span data-ttu-id="7ad23-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="7ad23-151">Parameter</span></span> | <span data-ttu-id="7ad23-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="7ad23-152">Description</span></span> | <span data-ttu-id="7ad23-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="7ad23-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7ad23-154">Név</span><span class="sxs-lookup"><span data-stu-id="7ad23-154">Name</span></span> |<span data-ttu-id="7ad23-155">A gyorsítótár neve</span><span class="sxs-lookup"><span data-stu-id="7ad23-155">Name of the cache</span></span> | |
| <span data-ttu-id="7ad23-156">Hely</span><span class="sxs-lookup"><span data-stu-id="7ad23-156">Location</span></span> |<span data-ttu-id="7ad23-157">A gyorsítótár helye</span><span class="sxs-lookup"><span data-stu-id="7ad23-157">Location of the cache</span></span> | |
| <span data-ttu-id="7ad23-158">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="7ad23-158">ResourceGroupName</span></span> |<span data-ttu-id="7ad23-159">Az erőforráscsoport neve, amelyben a gyorsítótár létrehozásához</span><span class="sxs-lookup"><span data-stu-id="7ad23-159">Resource group name in which to create the cache</span></span> | |
| <span data-ttu-id="7ad23-160">Méret</span><span class="sxs-lookup"><span data-stu-id="7ad23-160">Size</span></span> |<span data-ttu-id="7ad23-161">A gyorsítótár méretét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-161">The size of the cache.</span></span> <span data-ttu-id="7ad23-162">Érvényes értékek a következők: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1 GB-os, 2.5 GB, 6 GB, 13 GB, 26 GB, 53 GB</span><span class="sxs-lookup"><span data-stu-id="7ad23-162">Valid values are: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB</span></span> |<span data-ttu-id="7ad23-163">1 GB</span><span class="sxs-lookup"><span data-stu-id="7ad23-163">1GB</span></span> |
| <span data-ttu-id="7ad23-164">ShardCount</span><span class="sxs-lookup"><span data-stu-id="7ad23-164">ShardCount</span></span> |<span data-ttu-id="7ad23-165">A szilánkok létrehozása, ha egy prémium szintű gyorsítótár létrehozása fürtözési engedélyezett száma.</span><span class="sxs-lookup"><span data-stu-id="7ad23-165">The number of shards to create when creating a premium cache with clustering enabled.</span></span> <span data-ttu-id="7ad23-166">Érvényes értékek a következők: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span><span class="sxs-lookup"><span data-stu-id="7ad23-166">Valid values are: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span></span> | |
| <span data-ttu-id="7ad23-167">SKU</span><span class="sxs-lookup"><span data-stu-id="7ad23-167">SKU</span></span> |<span data-ttu-id="7ad23-168">Megadja a gyorsítótár a Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="7ad23-168">Specifies the SKU of the cache.</span></span> <span data-ttu-id="7ad23-169">Érvényes értékek a következők: Basic, Standard, Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-169">Valid values are: Basic, Standard, Premium</span></span> |<span data-ttu-id="7ad23-170">Standard</span><span class="sxs-lookup"><span data-stu-id="7ad23-170">Standard</span></span> |
| <span data-ttu-id="7ad23-171">RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="7ad23-171">RedisConfiguration</span></span> |<span data-ttu-id="7ad23-172">Határozza meg a Redis-konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7ad23-172">Specifies Redis configuration settings.</span></span> <span data-ttu-id="7ad23-173">Az egyes beállítások, lásd a következő [RedisConfiguration tulajdonságok](#redisconfiguration-properties) tábla.</span><span class="sxs-lookup"><span data-stu-id="7ad23-173">For details on each setting, see the following [RedisConfiguration properties](#redisconfiguration-properties) table.</span></span> | |
| <span data-ttu-id="7ad23-174">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="7ad23-174">EnableNonSslPort</span></span> |<span data-ttu-id="7ad23-175">Azt jelzi, hogy engedélyezve van-e a nem SSL port.</span><span class="sxs-lookup"><span data-stu-id="7ad23-175">Indicates whether the non-SSL port is enabled.</span></span> |<span data-ttu-id="7ad23-176">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="7ad23-176">False</span></span> |
| <span data-ttu-id="7ad23-177">MaxMemoryPolicy</span><span class="sxs-lookup"><span data-stu-id="7ad23-177">MaxMemoryPolicy</span></span> |<span data-ttu-id="7ad23-178">Ez a paraméter elavult – használja inkább a RedisConfiguration.</span><span class="sxs-lookup"><span data-stu-id="7ad23-178">This parameter has been deprecated - use RedisConfiguration instead.</span></span> | |
| <span data-ttu-id="7ad23-179">StaticIP</span><span class="sxs-lookup"><span data-stu-id="7ad23-179">StaticIP</span></span> |<span data-ttu-id="7ad23-180">Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím az alhálózat, a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="7ad23-180">When hosting your cache in a VNET, specifies a unique IP address in the subnet for the cache.</span></span> <span data-ttu-id="7ad23-181">Ha nem ad meg, egy választja meg az alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="7ad23-181">If not provided, one is chosen for you from the subnet.</span></span> | |
| <span data-ttu-id="7ad23-182">Alhálózat</span><span class="sxs-lookup"><span data-stu-id="7ad23-182">Subnet</span></span> |<span data-ttu-id="7ad23-183">Esetén a VNETEN belül a gyorsítótár, a neve, amelyben a gyorsítótár telepítendő alhálózat.</span><span class="sxs-lookup"><span data-stu-id="7ad23-183">When hosting your cache in a VNET, specifies the name of the subnet in which to deploy the cache.</span></span> | |
| <span data-ttu-id="7ad23-184">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="7ad23-184">VirtualNetwork</span></span> |<span data-ttu-id="7ad23-185">Esetén a VNETEN belül a gyorsítótár, a virtuális Hálózatot, amelyben a gyorsítótár telepítendő erőforrás Azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="7ad23-185">When hosting your cache in a VNET, specifies the resource ID of the VNET in which to deploy the cache.</span></span> | |
| <span data-ttu-id="7ad23-186">keyType</span><span class="sxs-lookup"><span data-stu-id="7ad23-186">KeyType</span></span> |<span data-ttu-id="7ad23-187">Megadja, mely újragenerálni a hozzáférési kulcsok megújításakor elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-187">Specifies which access key to regenerate when renewing access keys.</span></span> <span data-ttu-id="7ad23-188">Érvényes értékek a következők: elsődleges, másodlagos</span><span class="sxs-lookup"><span data-stu-id="7ad23-188">Valid values are: Primary, Secondary</span></span> | |

### <a name="redisconfiguration-properties"></a><span data-ttu-id="7ad23-189">RedisConfiguration tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7ad23-189">RedisConfiguration properties</span></span>
| <span data-ttu-id="7ad23-190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7ad23-190">Property</span></span> | <span data-ttu-id="7ad23-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="7ad23-191">Description</span></span> | <span data-ttu-id="7ad23-192">Árképzési szintek</span><span class="sxs-lookup"><span data-stu-id="7ad23-192">Pricing tiers</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7ad23-193">rekordadatbázis biztonsági mentés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="7ad23-193">rdb-backup-enabled</span></span> |<span data-ttu-id="7ad23-194">E [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="7ad23-194">Whether [Redis data persistence](cache-how-to-premium-persistence.md) is enabled</span></span> |<span data-ttu-id="7ad23-195">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="7ad23-195">Premium only</span></span> |
| <span data-ttu-id="7ad23-196">rekordadatbázis-storage-kapcsolat-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7ad23-196">rdb-storage-connection-string</span></span> |<span data-ttu-id="7ad23-197">A kapcsolati karakterláncot a tárfiók [Redis-adatmegőrzés](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="7ad23-197">The connection string to the storage account for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="7ad23-198">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="7ad23-198">Premium only</span></span> |
| <span data-ttu-id="7ad23-199">biztonsági mentés-gyakori rekordadatbázis</span><span class="sxs-lookup"><span data-stu-id="7ad23-199">rdb-backup-frequency</span></span> |<span data-ttu-id="7ad23-200">A biztonsági mentési gyakorisága [Redis-adatmegőrzés](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="7ad23-200">The backup frequency for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="7ad23-201">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="7ad23-201">Premium only</span></span> |
| <span data-ttu-id="7ad23-202">maxmemory fenntartott</span><span class="sxs-lookup"><span data-stu-id="7ad23-202">maxmemory-reserved</span></span> |<span data-ttu-id="7ad23-203">Konfigurálja a [fenntartott memória](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) nem gyorsítótár folyamatok</span><span class="sxs-lookup"><span data-stu-id="7ad23-203">Configures the [memory reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for non-cache processes</span></span> |<span data-ttu-id="7ad23-204">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-204">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-205">maxmemory-házirend</span><span class="sxs-lookup"><span data-stu-id="7ad23-205">maxmemory-policy</span></span> |<span data-ttu-id="7ad23-206">Konfigurálja a [kiürítés házirend](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) a gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="7ad23-206">Configures the [eviction policy](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for the cache</span></span> |<span data-ttu-id="7ad23-207">Minden tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="7ad23-207">All pricing tiers</span></span> |
| <span data-ttu-id="7ad23-208">értesítés-kulcstérértesítések használatával-események</span><span class="sxs-lookup"><span data-stu-id="7ad23-208">notify-keyspace-events</span></span> |<span data-ttu-id="7ad23-209">Konfigurálja az [kulcstérértesítések használatával értesítések](cache-configure.md#keyspace-notifications-advanced-settings)</span><span class="sxs-lookup"><span data-stu-id="7ad23-209">Configures [keyspace notifications](cache-configure.md#keyspace-notifications-advanced-settings)</span></span> |<span data-ttu-id="7ad23-210">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-210">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-211">kivonat-max-ziplist-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="7ad23-211">hash-max-ziplist-entries</span></span> |<span data-ttu-id="7ad23-212">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="7ad23-212">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="7ad23-213">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-213">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-214">kivonat-max-ziplist-értéke</span><span class="sxs-lookup"><span data-stu-id="7ad23-214">hash-max-ziplist-value</span></span> |<span data-ttu-id="7ad23-215">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="7ad23-215">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="7ad23-216">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-216">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-217">set-maximális-intset-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="7ad23-217">set-max-intset-entries</span></span> |<span data-ttu-id="7ad23-218">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="7ad23-218">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="7ad23-219">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-219">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-220">zset-maximális-ziplist-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="7ad23-220">zset-max-ziplist-entries</span></span> |<span data-ttu-id="7ad23-221">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="7ad23-221">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="7ad23-222">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-222">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-223">zset-maximális-ziplist-érték</span><span class="sxs-lookup"><span data-stu-id="7ad23-223">zset-max-ziplist-value</span></span> |<span data-ttu-id="7ad23-224">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="7ad23-224">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="7ad23-225">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-225">Standard and Premium</span></span> |
| <span data-ttu-id="7ad23-226">adatbázisok</span><span class="sxs-lookup"><span data-stu-id="7ad23-226">databases</span></span> |<span data-ttu-id="7ad23-227">Konfigurálja az adatbázisok számát.</span><span class="sxs-lookup"><span data-stu-id="7ad23-227">Configures the number of databases.</span></span> <span data-ttu-id="7ad23-228">Ez a tulajdonság csak a gyorsítótár létrehozásakor konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="7ad23-228">This property can be configured only at cache creation.</span></span> |<span data-ttu-id="7ad23-229">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="7ad23-229">Standard and Premium</span></span> |

## <a name="to-create-a-redis-cache"></a><span data-ttu-id="7ad23-230">A Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ad23-230">To create a Redis Cache</span></span>
<span data-ttu-id="7ad23-231">Új Azure Redis Cache példány használatával hozhatók létre a [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-231">New Azure Redis Cache instances are created using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad23-232">A Redis gyorsítótár hoz létre egy előfizetést az Azure-portált használja, először a portál regisztrálja a `Microsoft.Cache` adott előfizetéshez tartozó névtér.</span><span class="sxs-lookup"><span data-stu-id="7ad23-232">The first time you create a Redis cache in a subscription using the Azure portal, the portal registers the `Microsoft.Cache` namespace for that subscription.</span></span> <span data-ttu-id="7ad23-233">Ha az első Redis gyorsítótár létrehozása a PowerShell használatával előfizetés, először regisztrálnia kell, hogy a következő paranccsal; névtér például más parancsmagok `New-AzureRmRedisCache` és `Get-AzureRmRedisCache` sikertelen.</span><span class="sxs-lookup"><span data-stu-id="7ad23-233">If you attempt to create the first Redis cache in a subscription using PowerShell, you must first register that namespace using the following command; otherwise cmdlets such as `New-AzureRmRedisCache` and `Get-AzureRmRedisCache` fail.</span></span>
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

<span data-ttu-id="7ad23-234">A használható paramétereket, és azok leírásait tartalmazza a lista `New-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-234">To see a list of available parameters and their descriptions for `New-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-235">A gyorsítótár létrehozásához alapértelmezett paraméterekkel rendelkező, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-235">To create a cache with default parameters, run the following command.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

<span data-ttu-id="7ad23-236">`ResourceGroupName`, `Name`, és `Location` kötelező paraméterek tartoznak, de a többi opcionálisak, az alapértelmezett értékek szerint vannak.</span><span class="sxs-lookup"><span data-stu-id="7ad23-236">`ResourceGroupName`, `Name`, and `Location` are required parameters, but the rest are optional and have default values.</span></span> <span data-ttu-id="7ad23-237">Az előző parancs futtatása hoz létre egy Standard Termékváltozat Azure Redis Cache példány a megadott név, hely és erőforráscsoport, amely 1 GB méretű és a nem SSL port le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="7ad23-237">Running the previous command creates a Standard SKU Azure Redis Cache instance with the specified name, location, and resource group, that is 1 GB in size with the non-SSL port disabled.</span></span>

<span data-ttu-id="7ad23-238">A prémium gyorsítótár létrehozásához adjon meg egy mérete P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), vagy P4 (53 GB - 530 GB).</span><span class="sxs-lookup"><span data-stu-id="7ad23-238">To create a premium cache, specify a size of P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), or P4 (53 GB - 530 GB).</span></span> <span data-ttu-id="7ad23-239">Fürtszolgáltatás engedélyezéséhez használatával megad egy shard száma a `ShardCount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7ad23-239">To enable clustering, specify a shard count using the `ShardCount` parameter.</span></span> <span data-ttu-id="7ad23-240">A következő példa egy premium P1 gyorsítótár 3 szilánkok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7ad23-240">The following example creates a P1 premium cache with 3 shards.</span></span> <span data-ttu-id="7ad23-241">Premium P1 gyorsítótár 6 GB-nál, és mivel azt a három szilánkok adott teljes mérete 18 GB (3 x 6 GB).</span><span class="sxs-lookup"><span data-stu-id="7ad23-241">A P1 premium cache is 6 GB in size, and since we specified three shards the total size is 18 GB (3 x 6 GB).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

<span data-ttu-id="7ad23-242">Az értékek megadása a `RedisConfiguration` paraméter, tegye az értékek belül `{}` kulcs/érték párok, például `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span><span class="sxs-lookup"><span data-stu-id="7ad23-242">To specify values for the `RedisConfiguration` parameter, enclose the values inside `{}` as key/value pairs like `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span></span> <span data-ttu-id="7ad23-243">Az alábbi példakód létrehozza a szabványos 1 GB-os gyorsítótár `allkeys-random` maxmemory a beállított házirend- és kulcstérértesítések használatával értesítések `KEA`.</span><span class="sxs-lookup"><span data-stu-id="7ad23-243">The following example creates a standard 1 GB cache with `allkeys-random` maxmemory policy and keyspace notifications configured with `KEA`.</span></span> <span data-ttu-id="7ad23-244">További információkért lásd: [kulcstérértesítések használatával értesítések (Speciális beállítások)](cache-configure.md#keyspace-notifications-advanced-settings) és [memória házirendek](cache-configure.md#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="7ad23-244">For more information, see [Keyspace notifications (advanced settings)](cache-configure.md#keyspace-notifications-advanced-settings) and [Memory policies](cache-configure.md#memory-policies).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a><span data-ttu-id="7ad23-245">A gyorsítótár létrehozása során beállítás-adatbázisok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ad23-245">To configure the databases setting during cache creation</span></span>
<span data-ttu-id="7ad23-246">A `databases` beállítást csak a gyorsítótár létrehozása közben lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="7ad23-246">The `databases` setting can be configured only during cache creation.</span></span> <span data-ttu-id="7ad23-247">Az alábbi példakód létrehozza a prémium P3 (26 GB-os) gyorsítótárat használ 48 adatbázisok a [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-247">The following example creates a premium P3 (26 GB) cache with 48 databases using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

<span data-ttu-id="7ad23-248">További információ a `databases` tulajdonság, lásd: [Azure Redis Cache alapértelmezett kiszolgálókonfiguráció](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="7ad23-248">For more information on the `databases` property, see [Default Azure Redis Cache server configuration](cache-configure.md#default-redis-server-configuration).</span></span> <span data-ttu-id="7ad23-249">További létrehozásával kapcsolatos információkat a gyorsítótár használata a [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag, tekintse meg az előző [Redis gyorsítótár létrehozásához](#to-create-a-redis-cache) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7ad23-249">For more information on creating a cache using the [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, see the previous [To create a Redis Cache](#to-create-a-redis-cache) section.</span></span>

## <a name="to-update-a-redis-cache"></a><span data-ttu-id="7ad23-250">A Redis gyorsítótár frissítése</span><span class="sxs-lookup"><span data-stu-id="7ad23-250">To update a Redis cache</span></span>
<span data-ttu-id="7ad23-251">Azure Redis Cache példány frissítése használatával a [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-251">Azure Redis Cache instances are updated using the [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span></span>

<span data-ttu-id="7ad23-252">A használható paramétereket, és azok leírásait tartalmazza a lista `Set-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-252">To see a list of available parameters and their descriptions for `Set-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-253">A `Set-AzureRmRedisCache` parancsmag segítségével, mint tulajdonságainak frissítése `Size`, `Sku`, `EnableNonSslPort`, és a `RedisConfiguration` értékeket.</span><span class="sxs-lookup"><span data-stu-id="7ad23-253">The `Set-AzureRmRedisCache` cmdlet can be used to update properties such as `Size`, `Sku`, `EnableNonSslPort`, and the `RedisConfiguration` values.</span></span> 

<span data-ttu-id="7ad23-254">A következő parancsot a maxmemory-házirend a Redis gyorsítótár myCache nevű frissíti.</span><span class="sxs-lookup"><span data-stu-id="7ad23-254">The following command updates the maxmemory-policy for the Redis Cache named myCache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a><span data-ttu-id="7ad23-255">A Redis gyorsítótár méretezése</span><span class="sxs-lookup"><span data-stu-id="7ad23-255">To scale a Redis cache</span></span>
<span data-ttu-id="7ad23-256">`Set-AzureRmRedisCache`az Azure Redis Cache-gyorsítótár méretezési használható példány, ha a `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="7ad23-256">`Set-AzureRmRedisCache` can be used to scale an Azure Redis cache instance when the `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> 

> [!NOTE]
> <span data-ttu-id="7ad23-257">Skálázás a PowerShell használatával gyorsítótár az azonos korlátok és útmutatók méretezés a gyorsítótár Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="7ad23-257">Scaling a cache using PowerShell is subject to the same limits and guidelines as scaling a cache from the Azure portal.</span></span> <span data-ttu-id="7ad23-258">A következő korlátozásokkal egy másik tarifacsomagra méretezheti.</span><span class="sxs-lookup"><span data-stu-id="7ad23-258">You can scale to a different pricing tier with the following restrictions.</span></span>
> 
> * <span data-ttu-id="7ad23-259">Egy alacsonyabb tarifacsomagra, méretezhető nem a magasabb szintű tarifacsomagban használható.</span><span class="sxs-lookup"><span data-stu-id="7ad23-259">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
> * <span data-ttu-id="7ad23-260">Nem lehet méretezni a egy **prémium** le a gyorsítótár egy **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="7ad23-260">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
> * <span data-ttu-id="7ad23-261">Nem lehet méretezni a egy **szabványos** le a gyorsítótár egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="7ad23-261">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
> * <span data-ttu-id="7ad23-262">A méretezheti a **alapvető** gyorsítótárba egy **szabványos** gyorsítótár, de nem módosíthatja a méretét egyszerre.</span><span class="sxs-lookup"><span data-stu-id="7ad23-262">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="7ad23-263">Ha különböző méretű van szüksége, végezhet egy későbbi skálázási műveletet, hogy a kívánt méretet.</span><span class="sxs-lookup"><span data-stu-id="7ad23-263">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
> * <span data-ttu-id="7ad23-264">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül egy **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="7ad23-264">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="7ad23-265">Kell méretezni a **alapvető** való **szabványos** egy skálázási műveletet, majd a **szabványos** való **prémium** a későbbi skálázás a műveletet.</span><span class="sxs-lookup"><span data-stu-id="7ad23-265">You must scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
> * <span data-ttu-id="7ad23-266">A nagyobb méretű le nem lehet méretezni a **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-266">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>
> 
> <span data-ttu-id="7ad23-267">További információkért lásd: [Scale Azure Redis Cache hogyan](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="7ad23-267">For more information, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).</span></span>
> 
> 

<span data-ttu-id="7ad23-268">A következő példa bemutatja, hogyan nevű gyorsítótár méretezési `myCache` 2,5 GB gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="7ad23-268">The following example shows how to scale a cache named `myCache` to a 2.5 GB cache.</span></span> <span data-ttu-id="7ad23-269">Vegye figyelembe, hogy ez a parancs az alapszintű vagy Standard gyorsítótár is működik-e.</span><span class="sxs-lookup"><span data-stu-id="7ad23-269">Note that this command works for both a Basic or a Standard cache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="7ad23-270">Ez a parancs kiadása után a gyorsítótár állapotának ad vissza (hívása hasonló `Get-AzureRmRedisCache`).</span><span class="sxs-lookup"><span data-stu-id="7ad23-270">After this command is issued, the status of the cache is returned (similar to calling `Get-AzureRmRedisCache`).</span></span> <span data-ttu-id="7ad23-271">Vegye figyelembe, hogy a `ProvisioningState` van `Scaling`.</span><span class="sxs-lookup"><span data-stu-id="7ad23-271">Note that the `ProvisioningState` is `Scaling`.</span></span>

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

<span data-ttu-id="7ad23-272">Ha a méretezési művelet befejeződött, a `ProvisioningState` vált `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="7ad23-272">When the scaling operation is complete, the `ProvisioningState` changes to `Succeeded`.</span></span> <span data-ttu-id="7ad23-273">Ha meg kell győződnie egy későbbi skálázási művelet, például a Standard módosítása az alapszintű és a méretet, majd módosítja meg kell várnia, amíg az előző művelet befejeződik, vagy az alábbihoz hasonló hibaüzenetet kap.</span><span class="sxs-lookup"><span data-stu-id="7ad23-273">If you need to make a subsequent scaling operation, such as changing from Basic to Standard and then changing the size, you must wait until the previous operation is complete or you receive an error similar to the following.</span></span>

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a><span data-ttu-id="7ad23-274">A Redis gyorsítótár kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="7ad23-274">To get information about a Redis cache</span></span>
<span data-ttu-id="7ad23-275">Információ a gyorsítótár kérheti le a [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-275">You can retrieve information about a cache using the [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span></span>

<span data-ttu-id="7ad23-276">A használható paramétereket, és azok leírásait tartalmazza a lista `Get-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-276">To see a list of available parameters and their descriptions for `Get-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-277">A jelenlegi előfizetés az összes gyorsítótárak vonatkozó adatokat ad vissza, futtassa a `Get-AzureRmRedisCache` paraméter nélkül.</span><span class="sxs-lookup"><span data-stu-id="7ad23-277">To return information about all caches in the current subscription, run `Get-AzureRmRedisCache` without any parameters.</span></span>

    Get-AzureRmRedisCache

<span data-ttu-id="7ad23-278">Egy adott erőforráscsoportban található összes gyorsítótárak vonatkozó adatokat ad vissza, futtassa a `Get-AzureRmRedisCache` rendelkező a `ResourceGroupName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7ad23-278">To return information about all caches in a specific resource group, run `Get-AzureRmRedisCache` with the `ResourceGroupName` parameter.</span></span>

    Get-AzureRmRedisCache -ResourceGroupName myGroup

<span data-ttu-id="7ad23-279">Egy adott gyorsítótár vonatkozó adatokat ad vissza, futtassa a `Get-AzureRmRedisCache` rendelkező a `Name` a nevét, a gyorsítótár tartalmazó paraméter és a `ResourceGroupName` , hogy a gyorsítótár tartalmazó erőforráscsoportot paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="7ad23-279">To return information about a specific cache, run `Get-AzureRmRedisCache` with the `Name` parameter containing the name of the cache, and the `ResourceGroupName` parameter with the resource group containing that cache.</span></span>

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a><span data-ttu-id="7ad23-280">A Redis gyorsítótár elérési kulcsainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ad23-280">To retrieve the access keys for a Redis cache</span></span>
<span data-ttu-id="7ad23-281">A gyorsítótár elérési kulcsainak lekéréséhez használja a [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-281">To retrieve the access keys for your cache, you can use the [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span></span>

<span data-ttu-id="7ad23-282">A használható paramétereket, és azok leírásait tartalmazza a lista `Get-AzureRmRedisCacheKey`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-282">To see a list of available parameters and their descriptions for `Get-AzureRmRedisCacheKey`, run the following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-283">A gyorsítótár kulcsok lekéréséhez hívja meg a `Get-AzureRmRedisCacheKey` parancsmag, és adja át a gyorsítótár nevében, amely tartalmazza a gyorsítótár az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="7ad23-283">To retrieve the keys for your cache, call the `Get-AzureRmRedisCacheKey` cmdlet and pass in the name of your cache the name of the resource group that contains the cache.</span></span>

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a><span data-ttu-id="7ad23-284">A Redis gyorsítótár elérési kulcsainak újbóli</span><span class="sxs-lookup"><span data-stu-id="7ad23-284">To regenerate access keys for your Redis cache</span></span>
<span data-ttu-id="7ad23-285">A gyorsítótár elérési kulcsainak újragenerálása, használhatja a [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-285">To regenerate the access keys for your cache, you can use the [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span></span>

<span data-ttu-id="7ad23-286">A használható paramétereket, és azok leírásait tartalmazza a lista `New-AzureRmRedisCacheKey`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-286">To see a list of available parameters and their descriptions for `New-AzureRmRedisCacheKey`, run the following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-287">Újragenerálja az elsődleges vagy másodlagos kulcsot a gyorsítótárhoz, hívja meg a `New-AzureRmRedisCacheKey` parancsmag és a neve, az erőforráscsoport, adjon át, és adja meg `Primary` vagy `Secondary` a a `KeyType` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7ad23-287">To regenerate the primary or secondary key for your cache, call the `New-AzureRmRedisCacheKey` cmdlet and pass in the name, resource group, and specify either `Primary` or `Secondary` for the `KeyType` parameter.</span></span> <span data-ttu-id="7ad23-288">A következő példában a másodlagos hozzáférési kulcsot a gyorsítótár újbóli létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7ad23-288">In the following example, the secondary access key for a cache is regenerated.</span></span>

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a><span data-ttu-id="7ad23-289">A Redis gyorsítótár törlése</span><span class="sxs-lookup"><span data-stu-id="7ad23-289">To delete a Redis cache</span></span>
<span data-ttu-id="7ad23-290">A Redis gyorsítótár törléséhez használja a [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-290">To delete a Redis cache, use the [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span></span>

<span data-ttu-id="7ad23-291">A használható paramétereket, és azok leírásait tartalmazza a lista `Remove-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-291">To see a list of available parameters and their descriptions for `Remove-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="7ad23-292">A következő példában a gyorsítótár nevű `myCache` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="7ad23-292">In the following example, the cache named `myCache` is removed.</span></span>

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a><span data-ttu-id="7ad23-293">A Redis gyorsítótár importálása</span><span class="sxs-lookup"><span data-stu-id="7ad23-293">To import a Redis cache</span></span>
<span data-ttu-id="7ad23-294">Adatok importálása az Azure Redis Cache példány használatával a `Import-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-294">You can import data into an Azure Redis Cache instance using the `Import-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad23-295">Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="7ad23-295">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="7ad23-296">Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="7ad23-296">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="7ad23-297">A használható paramétereket, és azok leírásait tartalmazza a lista `Import-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-297">To see a list of available parameters and their descriptions for `Import-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="7ad23-298">A következő parancsot a blobból az Azure Redis Cache SAS URI-azonosítóval megadott importálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7ad23-298">The following command imports data from the blob specified by the SAS uri into Azure Redis Cache.</span></span>

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a><span data-ttu-id="7ad23-299">A Redis gyorsítótár exportálása</span><span class="sxs-lookup"><span data-stu-id="7ad23-299">To export a Redis cache</span></span>
<span data-ttu-id="7ad23-300">Adatok exportálása az Azure Redis Cache példány használatával a `Export-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-300">You can export data from an Azure Redis Cache instance using the `Export-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad23-301">Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="7ad23-301">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="7ad23-302">Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="7ad23-302">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="7ad23-303">A használható paramétereket, és azok leírásait tartalmazza a lista `Export-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-303">To see a list of available parameters and their descriptions for `Export-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="7ad23-304">A következő parancsot a tárolóba, a SAS URI azonosítója által megadott Azure Redis Cache-példányról exportálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7ad23-304">The following command exports data from an Azure Redis Cache instance into the container specified by the SAS uri.</span></span>

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a><span data-ttu-id="7ad23-305">Újraindítja a Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="7ad23-305">To reboot a Redis cache</span></span>
<span data-ttu-id="7ad23-306">Az Azure Redis Cache példány használt újraindíthatja a `Reset-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ad23-306">You can reboot your Azure Redis Cache instance using the `Reset-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad23-307">Újraindítás lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="7ad23-307">Reboot is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="7ad23-308">A gyorsítótár újraindítás kapcsolatos további információkért lásd: [felügyeleti gyorsítótár - újraindítás](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="7ad23-308">For more information about rebooting your cache, see [Cache administration - reboot](cache-administration.md#reboot).</span></span>
> 
> 

<span data-ttu-id="7ad23-309">A használható paramétereket, és azok leírásait tartalmazza a lista `Reset-AzureRmRedisCache`, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="7ad23-309">To see a list of available parameters and their descriptions for `Reset-AzureRmRedisCache`, run the following command.</span></span>

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="7ad23-310">A következő parancsot a megadott gyorsítótár mindkét csomópont újraindul.</span><span class="sxs-lookup"><span data-stu-id="7ad23-310">The following command reboots both nodes of the specified cache.</span></span>

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a><span data-ttu-id="7ad23-311">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ad23-311">Next steps</span></span>
<span data-ttu-id="7ad23-312">Windows PowerShell használatával az Azure-ral kapcsolatos további tudnivalókért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="7ad23-312">To learn more about using Windows PowerShell with Azure, see the following resources:</span></span>

* [<span data-ttu-id="7ad23-313">Az Azure Redis Cache parancsmag dokumentációja az MSDN webhelyen</span><span class="sxs-lookup"><span data-stu-id="7ad23-313">Azure Redis Cache cmdlet documentation on MSDN</span></span>](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* <span data-ttu-id="7ad23-314">[Az Azure Resource Manager parancsmagjainak](http://go.microsoft.com/fwlink/?LinkID=394765): bemutatják, hogyan használhatja a parancsmagok az Azure erőforrás-kezelő modulban.</span><span class="sxs-lookup"><span data-stu-id="7ad23-314">[Azure Resource Manager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Learn to use the cmdlets in the Azure Resource Manager module.</span></span>
* <span data-ttu-id="7ad23-315">[Erőforráscsoportok használata az Azure-erőforrások kezeléséhez](../azure-resource-manager/resource-group-template-deploy-portal.md): megtudhatja, hogyan hozhatja létre és kezelheti az erőforráscsoportok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7ad23-315">[Using Resource groups to manage your Azure resources](../azure-resource-manager/resource-group-template-deploy-portal.md): Learn how to create and manage resource groups in the Azure portal.</span></span>
* <span data-ttu-id="7ad23-316">[Az Azure blog](http://blogs.msdn.com/windowsazure): az Azure-ban új szolgáltatásainak megismerése.</span><span class="sxs-lookup"><span data-stu-id="7ad23-316">[Azure blog](http://blogs.msdn.com/windowsazure): Learn about new features in Azure.</span></span>
* <span data-ttu-id="7ad23-317">[A Windows PowerShell blog](http://blogs.msdn.com/powershell): a Windows PowerShell új szolgáltatásainak megismerése.</span><span class="sxs-lookup"><span data-stu-id="7ad23-317">[Windows PowerShell blog](http://blogs.msdn.com/powershell): Learn about new features in Windows PowerShell.</span></span>
* <span data-ttu-id="7ad23-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): a Windows PowerShell-Közösség valós tippek és trükkök az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="7ad23-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Get real-world tips and tricks from the Windows PowerShell community.</span></span>

