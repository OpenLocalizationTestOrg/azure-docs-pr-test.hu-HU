---
title: Azure Redis Cache aaaHow tooScale |} Microsoft Docs
description: "Ismerje meg, hogyan tooscale az Azure Redis gyorsítótár-példányokon"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 8d7c015a539f872913056392aa080bf3f445bd03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-azure-redis-cache"></a><span data-ttu-id="466a7-103">Hogyan tooScale Azure Redis Cache-gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="466a7-103">How tooScale Azure Redis Cache</span></span>
<span data-ttu-id="466a7-104">Azure Redis Cache rendelkezik másik gyorsítótármappa ajánlatokat, amelyek hello választott gyorsítótár mérete és a szolgáltatások rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="466a7-104">Azure Redis Cache has different cache offerings, which provide flexibility in hello choice of cache size and features.</span></span> <span data-ttu-id="466a7-105">A gyorsítótár létrehozása után méretezheti hello méretét és hello hello gyorsítótár tarifacsomag, ha az alkalmazás hello követelményei megváltoznak.</span><span class="sxs-lookup"><span data-stu-id="466a7-105">After a cache is created, you can scale hello size and hello pricing tier of hello cache if hello requirements of your application change.</span></span> <span data-ttu-id="466a7-106">Ez a cikk bemutatja, hogyan tooscale hello Azure-portál és a használatához a gyorsítótár eszközök, például az Azure PowerShell és az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="466a7-106">This article shows you how tooscale your cache in both hello Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-tooscale"></a><span data-ttu-id="466a7-107">Ha tooscale</span><span class="sxs-lookup"><span data-stu-id="466a7-107">When tooscale</span></span>
<span data-ttu-id="466a7-108">Használhatja a hello [figyelési](cache-how-to-monitor.md) Azure Redis Cache toomonitor szolgáltatásai állapotának és teljesítményének a gyorsítótár hello és meghatározásához, hogy mikor tooscale hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-108">You can use hello [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache toomonitor hello health and performance of your cache and help determine when tooscale hello cache.</span></span> 

<span data-ttu-id="466a7-109">Figyelheti a hello következő metrikák toohelp határozza meg, ha tooscale szükséges.</span><span class="sxs-lookup"><span data-stu-id="466a7-109">You can monitor hello following metrics toohelp determine if you need tooscale.</span></span>

* <span data-ttu-id="466a7-110">A kiszolgálóterhelés redis</span><span class="sxs-lookup"><span data-stu-id="466a7-110">Redis Server Load</span></span>
* <span data-ttu-id="466a7-111">Memóriahasználat</span><span class="sxs-lookup"><span data-stu-id="466a7-111">Memory Usage</span></span>
* <span data-ttu-id="466a7-112">Hálózati sávszélesség</span><span class="sxs-lookup"><span data-stu-id="466a7-112">Network Bandwidth</span></span>
* <span data-ttu-id="466a7-113">CPU-használat</span><span class="sxs-lookup"><span data-stu-id="466a7-113">CPU Usage</span></span>

<span data-ttu-id="466a7-114">Ha azt állapítja meg, hogy a gyorsítótár már nem teljesíti-e az alkalmazás követelményeinek, IP-címek számára az alkalmazás megfelelő tooa kisebb vagy nagyobb gyorsítótár méretezheti.</span><span class="sxs-lookup"><span data-stu-id="466a7-114">If you determine that your cache is no longer meeting your application's requirements, you can scale tooa larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="466a7-115">Annak meghatározására, amely gyorsítótár árképzési szint toouse további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="466a7-115">For more information on determining which cache pricing tier toouse, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="466a7-116">A gyorsítótár méretezése</span><span class="sxs-lookup"><span data-stu-id="466a7-116">Scale a cache</span></span>
<span data-ttu-id="466a7-117">tooscale a gyorsítótár [toohello gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello [Azure-portálon](https://portal.azure.com) kattintson **méretezési** a hello **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="466a7-117">tooscale your cache, [browse toohello cache](cache-configure.md#configure-redis-cache-settings) in hello [Azure portal](https://portal.azure.com) and click **Scale** from hello **Resource menu**.</span></span>

![Méretezés](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="466a7-119">SELECT hello szükséges hello a tarifacsomag **válassza ki az IP-címek** panel megnyitásához, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="466a7-119">Select hello desired pricing tier from hello **Select pricing tier** blade and click **Select**.</span></span>

![Tarifacsomag][redis-cache-pricing-tier-blade]


<span data-ttu-id="466a7-121">Másik tarifacsomagra vált a következő korlátozások hello tooa méretezheti:</span><span class="sxs-lookup"><span data-stu-id="466a7-121">You can scale tooa different pricing tier with hello following restrictions:</span></span>

* <span data-ttu-id="466a7-122">A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="466a7-122">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="466a7-123">Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-123">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="466a7-124">Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-124">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="466a7-125">A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="466a7-125">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="466a7-126">Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.</span><span class="sxs-lookup"><span data-stu-id="466a7-126">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="466a7-127">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-127">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="466a7-128">Kell méretezni a **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** a későbbi skálázás a műveletet.</span><span class="sxs-lookup"><span data-stu-id="466a7-128">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="466a7-129">Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="466a7-129">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="466a7-130">Hello gyorsítótár méretezése folyik toohello új IP-címek, miközben egy **méretezés** állapota megjelenik hello **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="466a7-130">While hello cache is scaling toohello new pricing tier, a **Scaling** status is displayed in hello **Redis Cache** blade.</span></span>

![Méretezés][redis-cache-scaling]

<span data-ttu-id="466a7-132">Skálázás befejeződése után hello állapota a **méretezés** túl**futtató**.</span><span class="sxs-lookup"><span data-stu-id="466a7-132">When scaling is complete, hello status changes from **Scaling** too**Running**.</span></span>

## <a name="how-tooautomate-a-scaling-operation"></a><span data-ttu-id="466a7-133">Hogyan tooautomate egy skálázási műveletet</span><span class="sxs-lookup"><span data-stu-id="466a7-133">How tooautomate a scaling operation</span></span>
<span data-ttu-id="466a7-134">A hozzáadása tooscaling a gyorsítótár-példány hello Azure-portálon, PowerShell-parancsmagokkal, az Azure parancssori felület, méretezhető, és hello segítségével a Microsoft Azure felügyeleti függvénytárak (MAML).</span><span class="sxs-lookup"><span data-stu-id="466a7-134">In addition tooscaling your cache instances in hello Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using hello Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="466a7-135">A skála PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="466a7-136">Scale Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="466a7-137">A skála MAML használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="466a7-138">A skála PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-138">Scale using PowerShell</span></span>
<span data-ttu-id="466a7-139">A PowerShell használatával az Azure Redis Cache példány méretezheti hello segítségével [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmagot, ha hello `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="466a7-139">You can scale your Azure Redis Cache instances with PowerShell by using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="466a7-140">hello következő példa bemutatja, hogyan tooscale gyorsítótár nevű `myCache` tooa 2,5 GB gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-140">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="466a7-141">A skálázás a PowerShell használatával további információkért lásd: [tooscale egy Redis gyorsítótár Powershell-lel](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="466a7-141">For more information on scaling with PowerShell, see [tooscale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="466a7-142">Scale Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-142">Scale using Azure CLI</span></span>
<span data-ttu-id="466a7-143">tooscale az Azure parancssori felület használatával Azure Redis Cache példány hívás hello `azure rediscache set` parancs és pass hello szükséges konfigurációs módosítások, amely egy új méretét, a termékváltozat, illetve a fürt méretét, attól függően, hogy hello skálázási művelet szükséges.</span><span class="sxs-lookup"><span data-stu-id="466a7-143">tooscale your Azure Redis Cache instances using Azure CLI, call hello `azure rediscache set` command and pass in hello desired configuration changes that include a new size, sku, or cluster size, depending on hello desired scaling operation.</span></span>

<span data-ttu-id="466a7-144">Az Azure parancssori felülettel skálázás további információkért lásd: [egy meglévő Redis gyorsítótár beállításainak módosítása](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="466a7-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="466a7-145">A skála MAML használatával</span><span class="sxs-lookup"><span data-stu-id="466a7-145">Scale using MAML</span></span>
<span data-ttu-id="466a7-146">tooscale a hello használó Azure Redis Cache-példányokat [Microsoft Azure felügyeleti függvénytárak (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), hívás hello `IRedisOperations.CreateOrUpdate` metódus, és új méretét hello hello adjon át `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="466a7-146">tooscale your Azure Redis Cache instances using hello [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call hello `IRedisOperations.CreateOrUpdate` method and pass in hello new size for hello `RedisProperties.SKU.Capacity`.</span></span>

    static void Main(string[] args)
    {
        // For instructions on getting hello access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // tooscale, set a new size for hello redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

<span data-ttu-id="466a7-147">További információkért lásd: hello [kezelése Redis Cache segítségével MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) minta.</span><span class="sxs-lookup"><span data-stu-id="466a7-147">For more information, see hello [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="466a7-148">Méretezési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="466a7-148">Scaling FAQ</span></span>
<span data-ttu-id="466a7-149">hello alábbi lista válaszok toocommonly Azure Redis Cache méretezésével kapcsolatos kérdések.</span><span class="sxs-lookup"><span data-stu-id="466a7-149">hello following list contains answers toocommonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="466a7-150">A, vagy a prémium szintű gyorsítótár is méretezhető?</span><span class="sxs-lookup"><span data-stu-id="466a7-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="466a7-151">Skálázás, után kell toochange a gyorsítótár neve vagy a hozzáférési kulcsainak?</span><span class="sxs-lookup"><span data-stu-id="466a7-151">After scaling, do I have toochange my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="466a7-152">Skálázás működése</span><span class="sxs-lookup"><span data-stu-id="466a7-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="466a7-153">I adat elvész a gyorsítótárból skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="466a7-154">Az egyéni adatbázisok állít érintett skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="466a7-155">A gyorsítótár elérhető lesz skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="466a7-156">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="466a7-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="466a7-157">Mennyi időt skálázás igénybe?</span><span class="sxs-lookup"><span data-stu-id="466a7-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="466a7-158">Hogyan állapítható meg, hogy ha skálázás befejeződött?</span><span class="sxs-lookup"><span data-stu-id="466a7-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="466a7-159">A, vagy a prémium szintű gyorsítótár is méretezhető?</span><span class="sxs-lookup"><span data-stu-id="466a7-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="466a7-160">Nem lehet méretezni a egy **prémium** gyorsítótár tooa le **alapvető** vagy **szabványos** tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="466a7-160">You can't scale from a **Premium** cache down tooa **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="466a7-161">Az egyik méretezheti **prémium** árképzési szint tooanother gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-161">You can scale from one **Premium** cache pricing tier tooanother.</span></span>
* <span data-ttu-id="466a7-162">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-162">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="466a7-163">Először át kell méretezni **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** egy későbbi skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="466a7-163">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="466a7-164">Ha engedélyezte a fürtszolgáltatás létrehozásakor a **prémium** gyorsítótárában, akkor [hello fürt méretének módosítása](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="466a7-164">If you enabled clustering when you created your **Premium** cache, you can [change hello cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="466a7-165">Ha a gyorsítótár fürtszolgáltatás engedélyezése nélkül hozták létre, akkor nem konfigurálhatja a fürtözést egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="466a7-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="466a7-166">További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="466a7-166">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a><span data-ttu-id="466a7-167">Skálázás, után kell toochange a gyorsítótár neve vagy a hozzáférési kulcsainak?</span><span class="sxs-lookup"><span data-stu-id="466a7-167">After scaling, do I have toochange my cache name or access keys?</span></span>
<span data-ttu-id="466a7-168">Nem, a gyorsítótár neve, valamint a kulcsok nem változik a méretezési művelet során.</span><span class="sxs-lookup"><span data-stu-id="466a7-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="466a7-169">Skálázás működése</span><span class="sxs-lookup"><span data-stu-id="466a7-169">How does scaling work?</span></span>
* <span data-ttu-id="466a7-170">Ha egy **alapvető** gyorsítótár méretezett tooa különböző méretű le van állítva, és új gyorsítótárat regisztráltak hello új méret használatával.</span><span class="sxs-lookup"><span data-stu-id="466a7-170">When a **Basic** cache is scaled tooa different size, it is shut down and a new cache is provisioned using hello new size.</span></span> <span data-ttu-id="466a7-171">Ebben az időszakban hello gyorsítótár nem érhető el, és a hello gyorsítótárban minden adat elveszik.</span><span class="sxs-lookup"><span data-stu-id="466a7-171">During this time, hello cache is unavailable and all data in hello cache is lost.</span></span>
* <span data-ttu-id="466a7-172">Ha egy **alapvető** gyorsítótár méretezett tooa **szabványos** gyorsítótár, a replika gyorsítótár ki van építve, és hello elsődleges gyorsítótár toohello replika gyorsítótár hello adatok másolásakor.</span><span class="sxs-lookup"><span data-stu-id="466a7-172">When a **Basic** cache is scaled tooa **Standard** cache, a replica cache is provisioned and hello data is copied from hello primary cache toohello replica cache.</span></span> <span data-ttu-id="466a7-173">hello gyorsítótár hello folyamat méretezés közben elérhető marad.</span><span class="sxs-lookup"><span data-stu-id="466a7-173">hello cache remains available during hello scaling process.</span></span>
* <span data-ttu-id="466a7-174">Ha egy **szabványos** gyorsítótár méretezett tooa különböző méretű vagy tooa **prémium** gyorsítótár, egyik hello replikák le van állítva, és újra üzembe a toohello új méretének és hello átvitt adatok keresztül, és más hello a replika hajt végre, a feladatátvétel előtt újra ki van építve, hasonló toohello folyamat során hello gyorsítótár-csomópontok közül az egyik hiba fordul elő.</span><span class="sxs-lookup"><span data-stu-id="466a7-174">When a **Standard** cache is scaled tooa different size or tooa **Premium** cache, one of hello replicas is shut down and re-provisioned toohello new size and hello data transferred over, and then hello other replica performs a failover before it is re-provisioned, similar toohello process that occurs during a failure of one of hello cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="466a7-175">I adat elvész a gyorsítótárból skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="466a7-176">Ha egy **alapvető** gyorsítótár méretezett tooa új méret, minden adat elveszik és hello gyorsítótár nem érhető el hello skálázás művelet során.</span><span class="sxs-lookup"><span data-stu-id="466a7-176">When a **Basic** cache is scaled tooa new size, all data is lost and hello cache is unavailable during hello scaling operation.</span></span>
* <span data-ttu-id="466a7-177">Ha egy **alapvető** gyorsítótár méretezett tooa **szabványos** gyorsítótárazza, hello hello gyorsítótárban lévő adatok általában megőrződik.</span><span class="sxs-lookup"><span data-stu-id="466a7-177">When a **Basic** cache is scaled tooa **Standard** cache, hello data in hello cache is typically preserved.</span></span>
* <span data-ttu-id="466a7-178">Ha egy **szabványos** gyorsítótár méretezett tooa nagyobb méretű vagy a réteg, vagy egy **prémium** gyorsítótár méretezett tooa nagyobb méretű, minden adatot általában megőrződik.</span><span class="sxs-lookup"><span data-stu-id="466a7-178">When a **Standard** cache is scaled tooa larger size or tier, or a **Premium** cache is scaled tooa larger size, all data is typically preserved.</span></span> <span data-ttu-id="466a7-179">Amikor skálázás egy **szabványos** vagy **prémium** attól függően, hogy mennyi adatot hello gyorsítótárban gyorsítótár le tooa kisebb méretet, adatok elveszhetnek azt méretezett kapcsolatos toohello új méretét.</span><span class="sxs-lookup"><span data-stu-id="466a7-179">When scaling a **Standard** or **Premium** cache down tooa smaller size, data may be lost depending on how much data is in hello cache related toohello new size when it is scaled.</span></span> <span data-ttu-id="466a7-180">Adat elvész, amikor a skálázás, ha a kulcsok kizárt vannak-e hello segítségével [allkeys-lru](http://redis.io/topics/lru-cache) kiürítés házirend.</span><span class="sxs-lookup"><span data-stu-id="466a7-180">If data is lost when scaling down, keys are evicted using hello [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="466a7-181">Az egyéni adatbázisok állít érintett skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="466a7-182">Különböző tartalmaznak az egyes tarifacsomagok [korlátok adatbázisok](cache-configure.md#databases), úgy, hogy nincs szempontokat konfigurálásakor méretet Ha hello egyéni értéket `databases` beállítása a gyorsítótár létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="466a7-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="466a7-183">Ha skálázás tarifacsomag az alsó tooa `databases` hello jelenlegi rétegtől határa:</span><span class="sxs-lookup"><span data-stu-id="466a7-183">When scaling tooa pricing tier with a lower `databases` limit than hello current tier:</span></span>
  * <span data-ttu-id="466a7-184">Hello alapértelmezett számának használata `databases` ez 16 az összes árképzési szinteket, adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="466a7-184">If you are using hello default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="466a7-185">Egyéni számos használata `databases` hello réteg toowhich van folyamatban, tartozik, amely hello határértékek ez `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="466a7-185">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="466a7-186">Egyéni számos használata `databases` , amely meghaladja hello új réteget, hello korlátai által megszabott hello `databases` beállítás süllyesztett toohello korlátok hello új réteg és hello eltávolított adatbázisok az összes adat elvész.</span><span class="sxs-lookup"><span data-stu-id="466a7-186">If you are using a custom number of `databases` that exceeds hello limits of hello new tier, hello `databases` setting is lowered toohello limits of hello new tier and all data in hello removed databases is lost.</span></span>
* <span data-ttu-id="466a7-187">Ha skálázás tarifacsomag hello az azonos vagy újabb tooa `databases` hello jelenlegi rétegtől határa a `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="466a7-187">When scaling tooa pricing tier with hello same or higher `databases` limit than hello current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="466a7-188">Vegye figyelembe, hogy míg a Standard és Premium gyorsítótárak egy 99,9 %-os SLA-t a rendelkezésre állás érdekében, az adatvesztés nélküli SLA.</span><span class="sxs-lookup"><span data-stu-id="466a7-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="466a7-189">A gyorsítótár elérhető lesz skálázás során?</span><span class="sxs-lookup"><span data-stu-id="466a7-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="466a7-190">**Standard** és **prémium** gyorsítótárak elérhetők maradnak hello skálázás művelet során.</span><span class="sxs-lookup"><span data-stu-id="466a7-190">**Standard** and **Premium** caches remain available during hello scaling operation.</span></span>
* <span data-ttu-id="466a7-191">**Alapszintű** gyorsítótárak műveletek tooa különböző méretű skálázás során offline módban, de elérhetők maradnak, ha a méretezés **alapvető** túl**szabványos**.</span><span class="sxs-lookup"><span data-stu-id="466a7-191">**Basic** caches are offline during scaling operations tooa different size, but remain available when scaling from **Basic** too**Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="466a7-192">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="466a7-192">Operations that are not supported</span></span>
* <span data-ttu-id="466a7-193">A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="466a7-193">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="466a7-194">Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-194">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="466a7-195">Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-195">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="466a7-196">A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="466a7-196">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="466a7-197">Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.</span><span class="sxs-lookup"><span data-stu-id="466a7-197">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="466a7-198">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="466a7-198">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="466a7-199">Először át kell méretezni **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** egy későbbi skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="466a7-199">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="466a7-200">Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="466a7-200">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>

<span data-ttu-id="466a7-201">A méretezési művelet sikertelen lesz, ha hello szolgáltatás megpróbál toorevert hello műveletet, és hello gyorsítótár visszaáll az eredeti méret toohello.</span><span class="sxs-lookup"><span data-stu-id="466a7-201">If a scaling operation fails, hello service will try toorevert hello operation and hello cache will revert toohello original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="466a7-202">Mennyi időt skálázás igénybe?</span><span class="sxs-lookup"><span data-stu-id="466a7-202">How long does scaling take?</span></span>
<span data-ttu-id="466a7-203">Skálázás vesz körülbelül 20 percet, attól függően, hogy mennyi adatot hello gyorsítótárában.</span><span class="sxs-lookup"><span data-stu-id="466a7-203">Scaling takes approximately 20 minutes, depending on how much data is in hello cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="466a7-204">Hogyan állapítható meg, hogy ha skálázás befejeződött?</span><span class="sxs-lookup"><span data-stu-id="466a7-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="466a7-205">Hello Azure-portálon a folyamatban lévő művelet skálázás hello látható.</span><span class="sxs-lookup"><span data-stu-id="466a7-205">In hello Azure portal you can see hello scaling operation in progress.</span></span> <span data-ttu-id="466a7-206">Skálázás befejeződése után hello hello gyorsítótár módosítások állapotának túl**futtató**.</span><span class="sxs-lookup"><span data-stu-id="466a7-206">When scaling is complete, hello status of hello cache changes too**Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



