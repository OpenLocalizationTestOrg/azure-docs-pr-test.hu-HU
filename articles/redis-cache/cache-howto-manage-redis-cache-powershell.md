---
title: Azure Redis Cache az Azure PowerShell aaaManage |} Microsoft Docs
description: "Megtudhatja, hogyan tooperform felügyeleti feladatokat az Azure Redis Cache Azure PowerShell használatával."
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
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a><span data-ttu-id="5b37b-103">Azure Redis gyorsítótár Azure PowerShell kezelése</span><span class="sxs-lookup"><span data-stu-id="5b37b-103">Manage Azure Redis Cache with Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5b37b-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5b37b-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="5b37b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5b37b-105">Azure CLI</span></span>](cache-manage-cli.md)
> 
> 

<span data-ttu-id="5b37b-106">Ez a témakör bemutatja, hogyan tooperform gyakori feladatokat, például egy létrehozása, frissítése és az Azure Redis Cache példány méretezése hogyan tooregenerate kulcsot, és hogyan a gyorsítótárak tooview információt.</span><span class="sxs-lookup"><span data-stu-id="5b37b-106">This topic shows you how tooperform common tasks such as create, update, and scale your Azure Redis Cache instances, how tooregenerate access keys, and how tooview information about your caches.</span></span> <span data-ttu-id="5b37b-107">Azure Redis Cache PowerShell-parancsmagok teljes listáját lásd: [Azure Redis Cache parancsmagok](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b37b-107">For a complete list of Azure Redis Cache PowerShell cmdlets, see [Azure Redis Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="5b37b-108">Hello klasszikus üzembe helyezési modellel kapcsolatos további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span><span class="sxs-lookup"><span data-stu-id="5b37b-108">For more information about hello classic deployment model, see [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b37b-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5b37b-109">Prerequisites</span></span>
<span data-ttu-id="5b37b-110">Ha már telepítette az Azure PowerShell, rendelkeznie kell Azure PowerShell 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5b37b-110">If you have already installed Azure PowerShell, you must have Azure PowerShell version 1.0.0 or later.</span></span> <span data-ttu-id="5b37b-111">Ellenőrizheti a hello Azure PowerShell ezzel a paranccsal hello Azure PowerShell parancssorban telepített verzióját.</span><span class="sxs-lookup"><span data-stu-id="5b37b-111">You can check hello version of Azure PowerShell that you have installed with this command at hello Azure PowerShell command prompt.</span></span>

    Get-Module azure | format-table version


<span data-ttu-id="5b37b-112">Először be kell jelentkeznie tooAzure ezzel a paranccsal.</span><span class="sxs-lookup"><span data-stu-id="5b37b-112">First, you must log in tooAzure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="5b37b-113">A Microsoft Azure-bejelentkezés hello párbeszédpanel hello e-mail címet az Azure-fiókjával, és a hozzá tartozó jelszó megadása</span><span class="sxs-lookup"><span data-stu-id="5b37b-113">Specify hello email address of your Azure account and its password in hello Microsoft Azure sign-in dialog.</span></span>

<span data-ttu-id="5b37b-114">Ezután ha több Azure-előfizetéssel rendelkezik, meg kell tooset az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5b37b-114">Next, if you have multiple Azure subscriptions, you need tooset your Azure subscription.</span></span> <span data-ttu-id="5b37b-115">az aktuális előfizetések listája toosee futtassa ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-115">toosee a list of your current subscriptions, run this command.</span></span>

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

<span data-ttu-id="5b37b-116">toospecify hello előfizetés, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="5b37b-116">toospecify hello subscription, run hello following command.</span></span> <span data-ttu-id="5b37b-117">A következő példa hello, hello előfizetés neve: `ContosoSubscription`.</span><span class="sxs-lookup"><span data-stu-id="5b37b-117">In hello following example, hello subscription name is `ContosoSubscription`.</span></span>

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

<span data-ttu-id="5b37b-118">A Windows PowerShell használhatja az Azure Resource Manager eszközzel, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5b37b-118">Before you can use Windows PowerShell with Azure Resource Manager, you need hello following:</span></span>

* <span data-ttu-id="5b37b-119">Windows PowerShell 3.0 vagy 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="5b37b-119">Windows PowerShell, Version 3.0 or 4.0.</span></span> <span data-ttu-id="5b37b-120">a Windows Powershellt, írja be a toofind hello verziója:`$PSVersionTable` , és ellenőrizze a hello értékének `PSVersion` 3.0 vagy 4.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="5b37b-120">toofind hello version of Windows PowerShell, type:`$PSVersionTable` and verify hello value of `PSVersion` is 3.0 or 4.0.</span></span> <span data-ttu-id="5b37b-121">tooinstall kompatibilitását, lásd: [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) vagy [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="5b37b-121">tooinstall a compatible version, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

<span data-ttu-id="5b37b-122">tooget részletes a jelen oktatóanyag esetében használja a Get-Help parancsmagot hello látni a parancsmag súgóját.</span><span class="sxs-lookup"><span data-stu-id="5b37b-122">tooget detailed help for any cmdlet you see in this tutorial, use hello Get-Help cmdlet.</span></span>

    Get-Help <cmdlet-name> -Detailed

<span data-ttu-id="5b37b-123">Hello például tooget súgóját `New-AzureRmRedisCache` parancsmag, típus:</span><span class="sxs-lookup"><span data-stu-id="5b37b-123">For example, tooget help for hello `New-AzureRmRedisCache` cmdlet, type:</span></span>

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a><span data-ttu-id="5b37b-124">Hogyan tooconnect tooother felhők</span><span class="sxs-lookup"><span data-stu-id="5b37b-124">How tooconnect tooother clouds</span></span>
<span data-ttu-id="5b37b-125">Alapértelmezett hello Azure környezetben az `AzureCloud`, amely jelöli hello globális Azure felhőben példány.</span><span class="sxs-lookup"><span data-stu-id="5b37b-125">By default hello Azure environment is `AzureCloud`, which represents hello global Azure cloud instance.</span></span> <span data-ttu-id="5b37b-126">tooconnect tooa másik példányt, használjon hello `Add-AzureRmAccount` hello parancsot `-Environment` vagy -`EnvironmentName` hello kívánt környezet vagy a környezet nevű parancssori kapcsolóval.</span><span class="sxs-lookup"><span data-stu-id="5b37b-126">tooconnect tooa different instance, use hello `Add-AzureRmAccount` command with hello `-Environment` or -`EnvironmentName` command line switch with hello desired environment or environment name.</span></span>

<span data-ttu-id="5b37b-127">rendelkezésre álló környezetekben, futtassa a hello toosee hello listája `Get-AzureRmEnvironment` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-127">toosee hello list of available environments, run hello `Get-AzureRmEnvironment` cmdlet.</span></span>

### <a name="tooconnect-toohello-azure-government-cloud"></a><span data-ttu-id="5b37b-128">tooconnect toohello Azure Government felhő</span><span class="sxs-lookup"><span data-stu-id="5b37b-128">tooconnect toohello Azure Government Cloud</span></span>
<span data-ttu-id="5b37b-129">tooconnect toohello Azure Government felhő, hello a következő parancsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-129">tooconnect toohello Azure Government Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

<span data-ttu-id="5b37b-130">vagy</span><span class="sxs-lookup"><span data-stu-id="5b37b-130">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

<span data-ttu-id="5b37b-131">hello Azure Government felhőbe, a gyorsítótárhoz toocreate hello alábbi helyek egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-131">toocreate a cache in hello Azure Government Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="5b37b-132">USGov Virginia</span><span class="sxs-lookup"><span data-stu-id="5b37b-132">USGov Virginia</span></span>
* <span data-ttu-id="5b37b-133">USGov Iowa</span><span class="sxs-lookup"><span data-stu-id="5b37b-133">USGov Iowa</span></span>

<span data-ttu-id="5b37b-134">Hello Azure Government felhő kapcsolatos további információkért lásd: [a Microsoft Azure Government](https://azure.microsoft.com/features/gov/) és [Microsoft Azure Government – útmutató fejlesztőknek](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="5b37b-134">For more information about hello Azure Government Cloud, see [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) and [Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>

### <a name="tooconnect-toohello-azure-china-cloud"></a><span data-ttu-id="5b37b-135">tooconnect toohello Azure Kína felhő</span><span class="sxs-lookup"><span data-stu-id="5b37b-135">tooconnect toohello Azure China Cloud</span></span>
<span data-ttu-id="5b37b-136">tooconnect toohello Azure Kína felhő, hello a következő parancsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-136">tooconnect toohello Azure China Cloud, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

<span data-ttu-id="5b37b-137">vagy</span><span class="sxs-lookup"><span data-stu-id="5b37b-137">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

<span data-ttu-id="5b37b-138">hello Azure Kína felhőbe, a gyorsítótárhoz toocreate hello alábbi helyek egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-138">toocreate a cache in hello Azure China Cloud, use one of hello following locations.</span></span>

* <span data-ttu-id="5b37b-139">Kelet-Kína</span><span class="sxs-lookup"><span data-stu-id="5b37b-139">China East</span></span>
* <span data-ttu-id="5b37b-140">Észak-Kína</span><span class="sxs-lookup"><span data-stu-id="5b37b-140">China North</span></span>

<span data-ttu-id="5b37b-141">Hello Azure Kína felhő kapcsolatos további információkért lásd: [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="5b37b-141">For more information about hello Azure China Cloud, see [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span>

### <a name="tooconnect-toomicrosoft-azure-germany"></a><span data-ttu-id="5b37b-142">tooconnect tooMicrosoft Azure-Németország</span><span class="sxs-lookup"><span data-stu-id="5b37b-142">tooconnect tooMicrosoft Azure Germany</span></span>
<span data-ttu-id="5b37b-143">tooconnect tooMicrosoft németországi Azure hello a következő parancsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-143">tooconnect tooMicrosoft Azure Germany, use one of hello following commands.</span></span>

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


<span data-ttu-id="5b37b-144">vagy</span><span class="sxs-lookup"><span data-stu-id="5b37b-144">or</span></span>

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

<span data-ttu-id="5b37b-145">egy Microsoft Azure Németországban gyorsítótár toocreate hello alábbi helyek egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b37b-145">toocreate a cache in Microsoft Azure Germany, use one of hello following locations.</span></span>

* <span data-ttu-id="5b37b-146">Közép-Németország</span><span class="sxs-lookup"><span data-stu-id="5b37b-146">Germany Central</span></span>
* <span data-ttu-id="5b37b-147">Északkelet-Németország</span><span class="sxs-lookup"><span data-stu-id="5b37b-147">Germany Northeast</span></span>

<span data-ttu-id="5b37b-148">A Microsoft Azure Németország kapcsolatos további információkért lásd: [Microsoft Azure Németország](https://azure.microsoft.com/overview/clouds/germany/).</span><span class="sxs-lookup"><span data-stu-id="5b37b-148">For more information about Microsoft Azure Germany, see [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).</span></span>

### <a name="properties-used-for-azure-redis-cache-powershell"></a><span data-ttu-id="5b37b-149">Azure Redis Cache PowerShell használt tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="5b37b-149">Properties used for Azure Redis Cache PowerShell</span></span>
<span data-ttu-id="5b37b-150">a következő táblázat hello tulajdonságok és a gyakran használt paraméterek létrehozása és kezelése az Azure Redis Cache példányt az Azure PowerShell leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5b37b-150">hello following table contains properties and descriptions for commonly used parameters when creating and managing your Azure Redis Cache instances using Azure PowerShell.</span></span>

| <span data-ttu-id="5b37b-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5b37b-151">Parameter</span></span> | <span data-ttu-id="5b37b-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="5b37b-152">Description</span></span> | <span data-ttu-id="5b37b-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="5b37b-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5b37b-154">Név</span><span class="sxs-lookup"><span data-stu-id="5b37b-154">Name</span></span> |<span data-ttu-id="5b37b-155">Hello gyorsítótár neve</span><span class="sxs-lookup"><span data-stu-id="5b37b-155">Name of hello cache</span></span> | |
| <span data-ttu-id="5b37b-156">Hely</span><span class="sxs-lookup"><span data-stu-id="5b37b-156">Location</span></span> |<span data-ttu-id="5b37b-157">Hello gyorsítótár helye</span><span class="sxs-lookup"><span data-stu-id="5b37b-157">Location of hello cache</span></span> | |
| <span data-ttu-id="5b37b-158">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="5b37b-158">ResourceGroupName</span></span> |<span data-ttu-id="5b37b-159">Az erőforráscsoport neve mely toocreate hello gyorsítótárban</span><span class="sxs-lookup"><span data-stu-id="5b37b-159">Resource group name in which toocreate hello cache</span></span> | |
| <span data-ttu-id="5b37b-160">Méret</span><span class="sxs-lookup"><span data-stu-id="5b37b-160">Size</span></span> |<span data-ttu-id="5b37b-161">hello hello gyorsítótár méretét.</span><span class="sxs-lookup"><span data-stu-id="5b37b-161">hello size of hello cache.</span></span> <span data-ttu-id="5b37b-162">Érvényes értékek a következők: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1 GB-os, 2.5 GB, 6 GB, 13 GB, 26 GB, 53 GB</span><span class="sxs-lookup"><span data-stu-id="5b37b-162">Valid values are: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB</span></span> |<span data-ttu-id="5b37b-163">1 GB</span><span class="sxs-lookup"><span data-stu-id="5b37b-163">1GB</span></span> |
| <span data-ttu-id="5b37b-164">ShardCount</span><span class="sxs-lookup"><span data-stu-id="5b37b-164">ShardCount</span></span> |<span data-ttu-id="5b37b-165">a prémium szintű gyorsítótár engedélyezve fürtözési létrehozásakor szilánkok toocreate hello száma.</span><span class="sxs-lookup"><span data-stu-id="5b37b-165">hello number of shards toocreate when creating a premium cache with clustering enabled.</span></span> <span data-ttu-id="5b37b-166">Érvényes értékek a következők: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span><span class="sxs-lookup"><span data-stu-id="5b37b-166">Valid values are: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10</span></span> | |
| <span data-ttu-id="5b37b-167">SKU</span><span class="sxs-lookup"><span data-stu-id="5b37b-167">SKU</span></span> |<span data-ttu-id="5b37b-168">Megadja a hello hello gyorsítótár Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="5b37b-168">Specifies hello SKU of hello cache.</span></span> <span data-ttu-id="5b37b-169">Érvényes értékek a következők: Basic, Standard, Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-169">Valid values are: Basic, Standard, Premium</span></span> |<span data-ttu-id="5b37b-170">Standard</span><span class="sxs-lookup"><span data-stu-id="5b37b-170">Standard</span></span> |
| <span data-ttu-id="5b37b-171">RedisConfiguration</span><span class="sxs-lookup"><span data-stu-id="5b37b-171">RedisConfiguration</span></span> |<span data-ttu-id="5b37b-172">Határozza meg a Redis-konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="5b37b-172">Specifies Redis configuration settings.</span></span> <span data-ttu-id="5b37b-173">Minden beállítás a részletekért lásd: hello következő [RedisConfiguration tulajdonságok](#redisconfiguration-properties) tábla.</span><span class="sxs-lookup"><span data-stu-id="5b37b-173">For details on each setting, see hello following [RedisConfiguration properties](#redisconfiguration-properties) table.</span></span> | |
| <span data-ttu-id="5b37b-174">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="5b37b-174">EnableNonSslPort</span></span> |<span data-ttu-id="5b37b-175">Azt jelzi, hogy engedélyezve van-e hello nem SSL port.</span><span class="sxs-lookup"><span data-stu-id="5b37b-175">Indicates whether hello non-SSL port is enabled.</span></span> |<span data-ttu-id="5b37b-176">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="5b37b-176">False</span></span> |
| <span data-ttu-id="5b37b-177">MaxMemoryPolicy</span><span class="sxs-lookup"><span data-stu-id="5b37b-177">MaxMemoryPolicy</span></span> |<span data-ttu-id="5b37b-178">Ez a paraméter elavult – használja inkább a RedisConfiguration.</span><span class="sxs-lookup"><span data-stu-id="5b37b-178">This parameter has been deprecated - use RedisConfiguration instead.</span></span> | |
| <span data-ttu-id="5b37b-179">StaticIP</span><span class="sxs-lookup"><span data-stu-id="5b37b-179">StaticIP</span></span> |<span data-ttu-id="5b37b-180">Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím hello gyorsítótár hello IP-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-180">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="5b37b-181">Ha nem ad meg, egy választja meg hello alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="5b37b-181">If not provided, one is chosen for you from hello subnet.</span></span> | |
| <span data-ttu-id="5b37b-182">Alhálózat</span><span class="sxs-lookup"><span data-stu-id="5b37b-182">Subnet</span></span> |<span data-ttu-id="5b37b-183">Esetén a VNETEN belül a gyorsítótárhoz, mely toodeploy hello gyorsítótárban a hello alhálózati hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="5b37b-183">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="5b37b-184">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="5b37b-184">VirtualNetwork</span></span> |<span data-ttu-id="5b37b-185">Ha a gyorsítótár a VNETEN belül üzemeltető hello erőforrás Azonosítóját adja meg hello virtuális Hálózatot, mely toodeploy hello gyorsítótárában.</span><span class="sxs-lookup"><span data-stu-id="5b37b-185">When hosting your cache in a VNET, specifies hello resource ID of hello VNET in which toodeploy hello cache.</span></span> | |
| <span data-ttu-id="5b37b-186">keyType</span><span class="sxs-lookup"><span data-stu-id="5b37b-186">KeyType</span></span> |<span data-ttu-id="5b37b-187">Meghatározza, mely a hozzáférési kulcsot tooregenerate hívóbetűk megújításakor.</span><span class="sxs-lookup"><span data-stu-id="5b37b-187">Specifies which access key tooregenerate when renewing access keys.</span></span> <span data-ttu-id="5b37b-188">Érvényes értékek a következők: elsődleges, másodlagos</span><span class="sxs-lookup"><span data-stu-id="5b37b-188">Valid values are: Primary, Secondary</span></span> | |

### <a name="redisconfiguration-properties"></a><span data-ttu-id="5b37b-189">RedisConfiguration tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="5b37b-189">RedisConfiguration properties</span></span>
| <span data-ttu-id="5b37b-190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5b37b-190">Property</span></span> | <span data-ttu-id="5b37b-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="5b37b-191">Description</span></span> | <span data-ttu-id="5b37b-192">Árképzési szintek</span><span class="sxs-lookup"><span data-stu-id="5b37b-192">Pricing tiers</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5b37b-193">rekordadatbázis biztonsági mentés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="5b37b-193">rdb-backup-enabled</span></span> |<span data-ttu-id="5b37b-194">E [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="5b37b-194">Whether [Redis data persistence](cache-how-to-premium-persistence.md) is enabled</span></span> |<span data-ttu-id="5b37b-195">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="5b37b-195">Premium only</span></span> |
| <span data-ttu-id="5b37b-196">rekordadatbázis-storage-kapcsolat-karakterlánc</span><span class="sxs-lookup"><span data-stu-id="5b37b-196">rdb-storage-connection-string</span></span> |<span data-ttu-id="5b37b-197">kapcsolati karakterlánc toohello tárfiók hello [Redis-adatmegőrzés](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="5b37b-197">hello connection string toohello storage account for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="5b37b-198">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="5b37b-198">Premium only</span></span> |
| <span data-ttu-id="5b37b-199">biztonsági mentés-gyakori rekordadatbázis</span><span class="sxs-lookup"><span data-stu-id="5b37b-199">rdb-backup-frequency</span></span> |<span data-ttu-id="5b37b-200">biztonsági mentési gyakorisága hello [Redis-adatmegőrzés](cache-how-to-premium-persistence.md)</span><span class="sxs-lookup"><span data-stu-id="5b37b-200">hello backup frequency for [Redis data persistence](cache-how-to-premium-persistence.md)</span></span> |<span data-ttu-id="5b37b-201">Csak a prémium</span><span class="sxs-lookup"><span data-stu-id="5b37b-201">Premium only</span></span> |
| <span data-ttu-id="5b37b-202">maxmemory fenntartott</span><span class="sxs-lookup"><span data-stu-id="5b37b-202">maxmemory-reserved</span></span> |<span data-ttu-id="5b37b-203">Konfigurálja a hello [fenntartott memória](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) nem gyorsítótár folyamatok</span><span class="sxs-lookup"><span data-stu-id="5b37b-203">Configures hello [memory reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for non-cache processes</span></span> |<span data-ttu-id="5b37b-204">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-204">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-205">maxmemory-házirend</span><span class="sxs-lookup"><span data-stu-id="5b37b-205">maxmemory-policy</span></span> |<span data-ttu-id="5b37b-206">Konfigurálja a hello [kiürítés házirend](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) hello gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="5b37b-206">Configures hello [eviction policy](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) for hello cache</span></span> |<span data-ttu-id="5b37b-207">Minden tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="5b37b-207">All pricing tiers</span></span> |
| <span data-ttu-id="5b37b-208">értesítés-kulcstérértesítések használatával-események</span><span class="sxs-lookup"><span data-stu-id="5b37b-208">notify-keyspace-events</span></span> |<span data-ttu-id="5b37b-209">Konfigurálja az [kulcstérértesítések használatával értesítések](cache-configure.md#keyspace-notifications-advanced-settings)</span><span class="sxs-lookup"><span data-stu-id="5b37b-209">Configures [keyspace notifications](cache-configure.md#keyspace-notifications-advanced-settings)</span></span> |<span data-ttu-id="5b37b-210">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-210">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-211">kivonat-max-ziplist-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="5b37b-211">hash-max-ziplist-entries</span></span> |<span data-ttu-id="5b37b-212">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="5b37b-212">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="5b37b-213">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-213">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-214">kivonat-max-ziplist-értéke</span><span class="sxs-lookup"><span data-stu-id="5b37b-214">hash-max-ziplist-value</span></span> |<span data-ttu-id="5b37b-215">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="5b37b-215">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="5b37b-216">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-216">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-217">set-maximális-intset-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="5b37b-217">set-max-intset-entries</span></span> |<span data-ttu-id="5b37b-218">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="5b37b-218">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="5b37b-219">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-219">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-220">zset-maximális-ziplist-bejegyzések</span><span class="sxs-lookup"><span data-stu-id="5b37b-220">zset-max-ziplist-entries</span></span> |<span data-ttu-id="5b37b-221">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="5b37b-221">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="5b37b-222">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-222">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-223">zset-maximális-ziplist-érték</span><span class="sxs-lookup"><span data-stu-id="5b37b-223">zset-max-ziplist-value</span></span> |<span data-ttu-id="5b37b-224">Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében</span><span class="sxs-lookup"><span data-stu-id="5b37b-224">Configures [memory optimization](http://redis.io/topics/memory-optimization) for small aggregate data types</span></span> |<span data-ttu-id="5b37b-225">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-225">Standard and Premium</span></span> |
| <span data-ttu-id="5b37b-226">adatbázisok</span><span class="sxs-lookup"><span data-stu-id="5b37b-226">databases</span></span> |<span data-ttu-id="5b37b-227">Konfigurálja az adatbázisok hello számát.</span><span class="sxs-lookup"><span data-stu-id="5b37b-227">Configures hello number of databases.</span></span> <span data-ttu-id="5b37b-228">Ez a tulajdonság csak a gyorsítótár létrehozásakor konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="5b37b-228">This property can be configured only at cache creation.</span></span> |<span data-ttu-id="5b37b-229">Standard és Premium</span><span class="sxs-lookup"><span data-stu-id="5b37b-229">Standard and Premium</span></span> |

## <a name="toocreate-a-redis-cache"></a><span data-ttu-id="5b37b-230">egy Redis gyorsítótárhoz toocreate</span><span class="sxs-lookup"><span data-stu-id="5b37b-230">toocreate a Redis Cache</span></span>
<span data-ttu-id="5b37b-231">Új Azure Redis Cache példány hello használatával hozhatók létre [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-231">New Azure Redis Cache instances are created using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b37b-232">hello első alkalommal hoz létre a Redis gyorsítótár hello Azure-portál használatával előfizetés hello portal regisztrálja hello `Microsoft.Cache` adott előfizetéshez tartozó névtér.</span><span class="sxs-lookup"><span data-stu-id="5b37b-232">hello first time you create a Redis cache in a subscription using hello Azure portal, hello portal registers hello `Microsoft.Cache` namespace for that subscription.</span></span> <span data-ttu-id="5b37b-233">Ha úgy próbálja toocreate hello először Redis gyorsítótár a PowerShell használatával előfizetés, először regisztrálnia kell a névtér a következő parancs; hello használata például más parancsmagok `New-AzureRmRedisCache` és `Get-AzureRmRedisCache` sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5b37b-233">If you attempt toocreate hello first Redis cache in a subscription using PowerShell, you must first register that namespace using hello following command; otherwise cmdlets such as `New-AzureRmRedisCache` and `Get-AzureRmRedisCache` fail.</span></span>
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

<span data-ttu-id="5b37b-234">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `New-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-234">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCache`, run hello following command.</span></span>

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
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-235">a gyorsítótár alapértelmezett paraméterek, futtassa a következő parancs hello toocreate.</span><span class="sxs-lookup"><span data-stu-id="5b37b-235">toocreate a cache with default parameters, run hello following command.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

<span data-ttu-id="5b37b-236">`ResourceGroupName`, `Name`, és `Location` kötelező paraméterek tartoznak, de hello rest opcionálisak, az alapértelmezett értékek szerint vannak.</span><span class="sxs-lookup"><span data-stu-id="5b37b-236">`ResourceGroupName`, `Name`, and `Location` are required parameters, but hello rest are optional and have default values.</span></span> <span data-ttu-id="5b37b-237">Hello előző parancs futtatása hoz létre egy Standard Termékváltozat Azure Redis Cache példány hello megadott nevét, helyét és erőforráscsoport, 1 GB méretű hello nem SSL port le van tiltva van.</span><span class="sxs-lookup"><span data-stu-id="5b37b-237">Running hello previous command creates a Standard SKU Azure Redis Cache instance with hello specified name, location, and resource group, that is 1 GB in size with hello non-SSL port disabled.</span></span>

<span data-ttu-id="5b37b-238">a prémium szintű gyorsítótár toocreate P1 (6 GB - 60 GB), (13 GB - 130 GB), P2 méretet adjon meg P3 (26 GB - 260 GB), vagy P4 (53 GB - 530 GB).</span><span class="sxs-lookup"><span data-stu-id="5b37b-238">toocreate a premium cache, specify a size of P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), or P4 (53 GB - 530 GB).</span></span> <span data-ttu-id="5b37b-239">Fürtszolgáltatás, tooenable hello segítségével a shard számot megadnia `ShardCount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b37b-239">tooenable clustering, specify a shard count using hello `ShardCount` parameter.</span></span> <span data-ttu-id="5b37b-240">hello alábbi példa hoz létre egy premium P1 gyorsítótár 3 szilánkok.</span><span class="sxs-lookup"><span data-stu-id="5b37b-240">hello following example creates a P1 premium cache with 3 shards.</span></span> <span data-ttu-id="5b37b-241">Premium P1 gyorsítótár 6 GB-nál, és mivel azt a három szilánkok hello teljes mérete adott 18 GB (3 x 6 GB).</span><span class="sxs-lookup"><span data-stu-id="5b37b-241">A P1 premium cache is 6 GB in size, and since we specified three shards hello total size is 18 GB (3 x 6 GB).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

<span data-ttu-id="5b37b-242">hello toospecify értékeinek `RedisConfiguration` paraméter, tegye a hello értékek belül `{}` kulcs/érték párok, például `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span><span class="sxs-lookup"><span data-stu-id="5b37b-242">toospecify values for hello `RedisConfiguration` parameter, enclose hello values inside `{}` as key/value pairs like `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`.</span></span> <span data-ttu-id="5b37b-243">hello alábbi példakód létrehozza szabványos 1 GB-os gyorsítótár `allkeys-random` maxmemory a beállított házirend- és kulcstérértesítések használatával értesítések `KEA`.</span><span class="sxs-lookup"><span data-stu-id="5b37b-243">hello following example creates a standard 1 GB cache with `allkeys-random` maxmemory policy and keyspace notifications configured with `KEA`.</span></span> <span data-ttu-id="5b37b-244">További információkért lásd: [kulcstérértesítések használatával értesítések (Speciális beállítások)](cache-configure.md#keyspace-notifications-advanced-settings) és [memória házirendek](cache-configure.md#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="5b37b-244">For more information, see [Keyspace notifications (advanced settings)](cache-configure.md#keyspace-notifications-advanced-settings) and [Memory policies](cache-configure.md#memory-policies).</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a><span data-ttu-id="5b37b-245">tooconfigure hello adatbázisok beállítása a gyorsítótár létrehozása közben</span><span class="sxs-lookup"><span data-stu-id="5b37b-245">tooconfigure hello databases setting during cache creation</span></span>
<span data-ttu-id="5b37b-246">Hello `databases` beállítást csak a gyorsítótár létrehozása közben lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="5b37b-246">hello `databases` setting can be configured only during cache creation.</span></span> <span data-ttu-id="5b37b-247">hello alábbi példa létrehoz egy prémium P3 (26 GB-os) gyorsítótárat hello segítségével 48 adatbázisok [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-247">hello following example creates a premium P3 (26 GB) cache with 48 databases using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet.</span></span>

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

<span data-ttu-id="5b37b-248">További információ a hello `databases` tulajdonság, lásd: [Azure Redis Cache alapértelmezett kiszolgálókonfiguráció](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="5b37b-248">For more information on hello `databases` property, see [Default Azure Redis Cache server configuration](cache-configure.md#default-redis-server-configuration).</span></span> <span data-ttu-id="5b37b-249">További létrehozásával kapcsolatos információkat hello gyorsítótár [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag, tekintse meg az előző hello [toocreate egy Redis gyorsítótárhoz](#to-create-a-redis-cache) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5b37b-249">For more information on creating a cache using hello [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet, see hello previous [toocreate a Redis Cache](#to-create-a-redis-cache) section.</span></span>

## <a name="tooupdate-a-redis-cache"></a><span data-ttu-id="5b37b-250">a Redis gyorsítótár tooupdate</span><span class="sxs-lookup"><span data-stu-id="5b37b-250">tooupdate a Redis cache</span></span>
<span data-ttu-id="5b37b-251">Azure Redis Cache példány frissítése hello segítségével [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-251">Azure Redis Cache instances are updated using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet.</span></span>

<span data-ttu-id="5b37b-252">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Set-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-252">toosee a list of available parameters and their descriptions for `Set-AzureRmRedisCache`, run hello following command.</span></span>

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
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-253">Hello `Set-AzureRmRedisCache` parancsmag lehetnek többek között használt tooupdate tulajdonságok `Size`, `Sku`, `EnableNonSslPort`, és hello `RedisConfiguration` értékeket.</span><span class="sxs-lookup"><span data-stu-id="5b37b-253">hello `Set-AzureRmRedisCache` cmdlet can be used tooupdate properties such as `Size`, `Sku`, `EnableNonSslPort`, and hello `RedisConfiguration` values.</span></span> 

<span data-ttu-id="5b37b-254">hello frissítések hello hello Redis Cache-maxmemory-házirend a következő parancs nevű myCache.</span><span class="sxs-lookup"><span data-stu-id="5b37b-254">hello following command updates hello maxmemory-policy for hello Redis Cache named myCache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a><span data-ttu-id="5b37b-255">a Redis gyorsítótár tooscale</span><span class="sxs-lookup"><span data-stu-id="5b37b-255">tooscale a Redis cache</span></span>
<span data-ttu-id="5b37b-256">`Set-AzureRmRedisCache`az Azure Redis cache példány amikor hello lehet használt tooscale `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="5b37b-256">`Set-AzureRmRedisCache` can be used tooscale an Azure Redis cache instance when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> 

> [!NOTE]
> <span data-ttu-id="5b37b-257">Skálázás a PowerShell használatával gyorsítótár tulajdonos toohello azonos korlátok és útmutatók a gyorsítótár, a méretezés hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5b37b-257">Scaling a cache using PowerShell is subject toohello same limits and guidelines as scaling a cache from hello Azure portal.</span></span> <span data-ttu-id="5b37b-258">Másik tarifacsomagra vált a következő korlátozások hello tooa méretezheti.</span><span class="sxs-lookup"><span data-stu-id="5b37b-258">You can scale tooa different pricing tier with hello following restrictions.</span></span>
> 
> * <span data-ttu-id="5b37b-259">A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="5b37b-259">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
> * <span data-ttu-id="5b37b-260">Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b37b-260">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
> * <span data-ttu-id="5b37b-261">Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b37b-261">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
> * <span data-ttu-id="5b37b-262">A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5b37b-262">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="5b37b-263">Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.</span><span class="sxs-lookup"><span data-stu-id="5b37b-263">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
> * <span data-ttu-id="5b37b-264">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b37b-264">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="5b37b-265">Kell méretezni a **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** a későbbi skálázás a műveletet.</span><span class="sxs-lookup"><span data-stu-id="5b37b-265">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
> * <span data-ttu-id="5b37b-266">Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="5b37b-266">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
> 
> <span data-ttu-id="5b37b-267">További információkért lásd: [hogyan tooScale Azure Redis Cache-gyorsítótár](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="5b37b-267">For more information, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>
> 
> 

<span data-ttu-id="5b37b-268">hello következő példa bemutatja, hogyan tooscale gyorsítótár nevű `myCache` tooa 2,5 GB gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b37b-268">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> <span data-ttu-id="5b37b-269">Vegye figyelembe, hogy ez a parancs az alapszintű vagy Standard gyorsítótár is működik-e.</span><span class="sxs-lookup"><span data-stu-id="5b37b-269">Note that this command works for both a Basic or a Standard cache.</span></span>

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="5b37b-270">Ez a parancs kiadása után hello gyorsítótár hello állapotának ad vissza (hasonló toocalling `Get-AzureRmRedisCache`).</span><span class="sxs-lookup"><span data-stu-id="5b37b-270">After this command is issued, hello status of hello cache is returned (similar toocalling `Get-AzureRmRedisCache`).</span></span> <span data-ttu-id="5b37b-271">Vegye figyelembe, hogy hello `ProvisioningState` van `Scaling`.</span><span class="sxs-lookup"><span data-stu-id="5b37b-271">Note that hello `ProvisioningState` is `Scaling`.</span></span>

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

<span data-ttu-id="5b37b-272">Hello skálázás művelet befejeződése után hello `ProvisioningState` túl változik`Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="5b37b-272">When hello scaling operation is complete, hello `ProvisioningState` changes too`Succeeded`.</span></span> <span data-ttu-id="5b37b-273">Ha egy későbbi skálázási művelet, például módosítása az alapvető tooStandard és hello méretét, majd módosítja toomake kell várnia kell addig, amíg hello korábbi művelet befejeződik, vagy egy hiba hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="5b37b-273">If you need toomake a subsequent scaling operation, such as changing from Basic tooStandard and then changing hello size, you must wait until hello previous operation is complete or you receive an error similar toohello following.</span></span>

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a><span data-ttu-id="5b37b-274">a Redis gyorsítótár tooget információ</span><span class="sxs-lookup"><span data-stu-id="5b37b-274">tooget information about a Redis cache</span></span>
<span data-ttu-id="5b37b-275">Információ a gyorsítótár hello kérheti le [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-275">You can retrieve information about a cache using hello [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet.</span></span>

<span data-ttu-id="5b37b-276">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Get-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-276">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-277">hello az aktuális előfizetést, minden gyorsítótárak tooreturn információ futtatása `Get-AzureRmRedisCache` paraméter nélkül.</span><span class="sxs-lookup"><span data-stu-id="5b37b-277">tooreturn information about all caches in hello current subscription, run `Get-AzureRmRedisCache` without any parameters.</span></span>

    Get-AzureRmRedisCache

<span data-ttu-id="5b37b-278">egy adott erőforráscsoportban található összes gyorsítótárak tooreturn információ futtatása `Get-AzureRmRedisCache` a hello `ResourceGroupName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b37b-278">tooreturn information about all caches in a specific resource group, run `Get-AzureRmRedisCache` with hello `ResourceGroupName` parameter.</span></span>

    Get-AzureRmRedisCache -ResourceGroupName myGroup

<span data-ttu-id="5b37b-279">tooreturn információk egy adott gyorsítótárral, futtassa `Get-AzureRmRedisCache` a hello `Name` hello nevét, valamint hello gyorsítótár hello tartalmazó paraméter `ResourceGroupName` , hogy a gyorsítótár tartalmazó hello erőforráscsoport paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="5b37b-279">tooreturn information about a specific cache, run `Get-AzureRmRedisCache` with hello `Name` parameter containing hello name of hello cache, and hello `ResourceGroupName` parameter with hello resource group containing that cache.</span></span>

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

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a><span data-ttu-id="5b37b-280">tooretrieve hello a Redis gyorsítótár elérési kulcsainak</span><span class="sxs-lookup"><span data-stu-id="5b37b-280">tooretrieve hello access keys for a Redis cache</span></span>
<span data-ttu-id="5b37b-281">tooretrieve hello hozzáférési kulcsainak listázása a gyorsítótár, a hello használhatja [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-281">tooretrieve hello access keys for your cache, you can use hello [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet.</span></span>

<span data-ttu-id="5b37b-282">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Get-AzureRmRedisCacheKey`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-282">toosee a list of available parameters and their descriptions for `Get-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-283">a gyorsítótár, a hívás hello kulcsok tooretrieve hello `Get-AzureRmRedisCacheKey` parancsmag és a gyorsítótár hello nevű fázis hello hello gyorsítótár tartalmazó hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="5b37b-283">tooretrieve hello keys for your cache, call hello `Get-AzureRmRedisCacheKey` cmdlet and pass in hello name of your cache hello name of hello resource group that contains hello cache.</span></span>

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a><span data-ttu-id="5b37b-284">a Redis gyorsítótár elérési kulcsainak tooregenerate</span><span class="sxs-lookup"><span data-stu-id="5b37b-284">tooregenerate access keys for your Redis cache</span></span>
<span data-ttu-id="5b37b-285">tooregenerate hello hozzáférési kulcsainak listázása a gyorsítótár, a hello használhatja [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-285">tooregenerate hello access keys for your cache, you can use hello [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet.</span></span>

<span data-ttu-id="5b37b-286">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `New-AzureRmRedisCacheKey`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-286">toosee a list of available parameters and their descriptions for `New-AzureRmRedisCacheKey`, run hello following command.</span></span>

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-287">tooregenerate hello elsődleges vagy másodlagos kulcsot a gyorsítótár, a hívás hello `New-AzureRmRedisCacheKey` parancsmag és a hello adjon át name, erőforráscsoportot, majd adja meg `Primary` vagy `Secondary` a hello `KeyType` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b37b-287">tooregenerate hello primary or secondary key for your cache, call hello `New-AzureRmRedisCacheKey` cmdlet and pass in hello name, resource group, and specify either `Primary` or `Secondary` for hello `KeyType` parameter.</span></span> <span data-ttu-id="5b37b-288">A következő példa hello hello másodlagos hozzáférési kulcsot a gyorsítótár újbóli létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5b37b-288">In hello following example, hello secondary access key for a cache is regenerated.</span></span>

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a><span data-ttu-id="5b37b-289">a Redis gyorsítótár toodelete</span><span class="sxs-lookup"><span data-stu-id="5b37b-289">toodelete a Redis cache</span></span>
<span data-ttu-id="5b37b-290">a Redis gyorsítótár toodelete hello használata [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-290">toodelete a Redis cache, use hello [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet.</span></span>

<span data-ttu-id="5b37b-291">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Remove-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-291">toosee a list of available parameters and their descriptions for `Remove-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

<span data-ttu-id="5b37b-292">A következő példa hello, hello nevű gyorsítótár `myCache` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="5b37b-292">In hello following example, hello cache named `myCache` is removed.</span></span>

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a><span data-ttu-id="5b37b-293">a Redis gyorsítótár tooimport</span><span class="sxs-lookup"><span data-stu-id="5b37b-293">tooimport a Redis cache</span></span>
<span data-ttu-id="5b37b-294">Adatok importálása az Azure Redis Cache példány hello segítségével `Import-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-294">You can import data into an Azure Redis Cache instance using hello `Import-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b37b-295">Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="5b37b-295">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="5b37b-296">Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="5b37b-296">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="5b37b-297">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Import-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-297">toosee a list of available parameters and their descriptions for `Import-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="5b37b-298">hello következő parancs importál adatokat hello blob az Azure Redis Cache hello SAS URI megadva.</span><span class="sxs-lookup"><span data-stu-id="5b37b-298">hello following command imports data from hello blob specified by hello SAS uri into Azure Redis Cache.</span></span>

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a><span data-ttu-id="5b37b-299">a Redis gyorsítótár tooexport</span><span class="sxs-lookup"><span data-stu-id="5b37b-299">tooexport a Redis cache</span></span>
<span data-ttu-id="5b37b-300">Az Azure Redis Cache példány hello segítségével exportálhatja az adatokat `Export-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-300">You can export data from an Azure Redis Cache instance using hello `Export-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b37b-301">Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="5b37b-301">Import/Export is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="5b37b-302">Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="5b37b-302">For more information about Import/Export, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

<span data-ttu-id="5b37b-303">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Export-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-303">toosee a list of available parameters and their descriptions for `Export-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="5b37b-304">hello következő parancs exportál adatokat az Azure Redis Cache példány hello SAS URI azonosítója által megadott hello tárolóba.</span><span class="sxs-lookup"><span data-stu-id="5b37b-304">hello following command exports data from an Azure Redis Cache instance into hello container specified by hello SAS uri.</span></span>

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a><span data-ttu-id="5b37b-305">a Redis gyorsítótár tooreboot</span><span class="sxs-lookup"><span data-stu-id="5b37b-305">tooreboot a Redis cache</span></span>
<span data-ttu-id="5b37b-306">Az Azure Redis Cache példány hello segítségével újraindíthatja `Reset-AzureRmRedisCache` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5b37b-306">You can reboot your Azure Redis Cache instance using hello `Reset-AzureRmRedisCache` cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b37b-307">Újraindítás lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="5b37b-307">Reboot is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span> <span data-ttu-id="5b37b-308">A gyorsítótár újraindítás kapcsolatos további információkért lásd: [felügyeleti gyorsítótár - újraindítás](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="5b37b-308">For more information about rebooting your cache, see [Cache administration - reboot](cache-administration.md#reboot).</span></span>
> 
> 

<span data-ttu-id="5b37b-309">toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Reset-AzureRmRedisCache`- ben futtassa hello következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="5b37b-309">toosee a list of available parameters and their descriptions for `Reset-AzureRmRedisCache`, run hello following command.</span></span>

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


<span data-ttu-id="5b37b-310">hello parancsban újraindulása mindkét csomópontjának megadott hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="5b37b-310">hello following command reboots both nodes of hello specified cache.</span></span>

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a><span data-ttu-id="5b37b-311">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b37b-311">Next steps</span></span>
<span data-ttu-id="5b37b-312">További információ a Windows PowerShell használatával az Azure-toolearn a következő erőforrások hello lásd:</span><span class="sxs-lookup"><span data-stu-id="5b37b-312">toolearn more about using Windows PowerShell with Azure, see hello following resources:</span></span>

* [<span data-ttu-id="5b37b-313">Az Azure Redis Cache parancsmag dokumentációja az MSDN webhelyen</span><span class="sxs-lookup"><span data-stu-id="5b37b-313">Azure Redis Cache cmdlet documentation on MSDN</span></span>](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* <span data-ttu-id="5b37b-314">[Az Azure Resource Manager parancsmagjainak](http://go.microsoft.com/fwlink/?LinkID=394765): hello Azure Resource Manager modul toouse hello használható parancsmagok megismerése.</span><span class="sxs-lookup"><span data-stu-id="5b37b-314">[Azure Resource Manager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Learn toouse hello cmdlets in hello Azure Resource Manager module.</span></span>
* <span data-ttu-id="5b37b-315">[Erőforrás segítségével csoportosítja toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-template-deploy-portal.md): megtudhatja, hogyan toocreate és kezelheti a hello Azure-portálon az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="5b37b-315">[Using Resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-template-deploy-portal.md): Learn how toocreate and manage resource groups in hello Azure portal.</span></span>
* <span data-ttu-id="5b37b-316">[Az Azure blog](http://blogs.msdn.com/windowsazure): az Azure-ban új szolgáltatásainak megismerése.</span><span class="sxs-lookup"><span data-stu-id="5b37b-316">[Azure blog](http://blogs.msdn.com/windowsazure): Learn about new features in Azure.</span></span>
* <span data-ttu-id="5b37b-317">[A Windows PowerShell blog](http://blogs.msdn.com/powershell): a Windows PowerShell új szolgáltatásainak megismerése.</span><span class="sxs-lookup"><span data-stu-id="5b37b-317">[Windows PowerShell blog](http://blogs.msdn.com/powershell): Learn about new features in Windows PowerShell.</span></span>
* <span data-ttu-id="5b37b-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): hello Windows PowerShell-Közösség valós tippek és trükkök az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="5b37b-318">["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Get real-world tips and tricks from hello Windows PowerShell community.</span></span>

