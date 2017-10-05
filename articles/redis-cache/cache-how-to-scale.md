---
title: "Azure Redis Cache méretezése |} Microsoft Docs"
description: "További tudnivalók az Azure Redis Cache példány méretezése"
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
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-azure-redis-cache"></a><span data-ttu-id="9af41-103">Azure Redis Cache méretezése</span><span class="sxs-lookup"><span data-stu-id="9af41-103">How to Scale Azure Redis Cache</span></span>
<span data-ttu-id="9af41-104">Azure Redis Cache rendelkezik másik gyorsítótármappa ajánlatokat, amelyek gyorsítótár mérete és a szolgáltatások rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="9af41-104">Azure Redis Cache has different cache offerings, which provide flexibility in the choice of cache size and features.</span></span> <span data-ttu-id="9af41-105">A gyorsítótár létrehozása után méretezheti méretét és a gyorsítótár az árképzési szint Ha megváltoztatja az alkalmazás követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="9af41-105">After a cache is created, you can scale the size and the pricing tier of the cache if the requirements of your application change.</span></span> <span data-ttu-id="9af41-106">Ez a cikk bemutatja, hogyan méretezése a gyorsítótár az Azure-portálon és Azure PowerShell és az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="9af41-106">This article shows you how to scale your cache in both the Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-to-scale"></a><span data-ttu-id="9af41-107">Mikor érdemes méretezni</span><span class="sxs-lookup"><span data-stu-id="9af41-107">When to scale</span></span>
<span data-ttu-id="9af41-108">Használhatja a [figyelési](cache-how-to-monitor.md) állapotának és a gyorsítótár teljesítményének figyelésére, és segíthet meghatározni, mikor érdemes méretezni a gyorsítótár Azure Redis Cache-funkcióit.</span><span class="sxs-lookup"><span data-stu-id="9af41-108">You can use the [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache to monitor the health and performance of your cache and help determine when to scale the cache.</span></span> 

<span data-ttu-id="9af41-109">A következő metrikákat annak meghatározásához, ha skálázni szeretne figyelheti.</span><span class="sxs-lookup"><span data-stu-id="9af41-109">You can monitor the following metrics to help determine if you need to scale.</span></span>

* <span data-ttu-id="9af41-110">A kiszolgálóterhelés redis</span><span class="sxs-lookup"><span data-stu-id="9af41-110">Redis Server Load</span></span>
* <span data-ttu-id="9af41-111">Memóriahasználat</span><span class="sxs-lookup"><span data-stu-id="9af41-111">Memory Usage</span></span>
* <span data-ttu-id="9af41-112">Hálózati sávszélesség</span><span class="sxs-lookup"><span data-stu-id="9af41-112">Network Bandwidth</span></span>
* <span data-ttu-id="9af41-113">CPU-használat</span><span class="sxs-lookup"><span data-stu-id="9af41-113">CPU Usage</span></span>

<span data-ttu-id="9af41-114">Ha azt állapítja meg, hogy a gyorsítótár már nem teljesíti-e az alkalmazás követelményeinek, IP-címek számára az alkalmazás megfelelő kisebb vagy nagyobb gyorsítótárhoz méretezheti.</span><span class="sxs-lookup"><span data-stu-id="9af41-114">If you determine that your cache is no longer meeting your application's requirements, you can scale to a larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="9af41-115">További információ melyik gyorsítótár IP-címek használatára, lásd: [milyen Redis Cache-ajánlatot és méretet használjam](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="9af41-115">For more information on determining which cache pricing tier to use, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="9af41-116">A gyorsítótár méretezése</span><span class="sxs-lookup"><span data-stu-id="9af41-116">Scale a cache</span></span>
<span data-ttu-id="9af41-117">A gyorsítótár méretezési [keresse meg a gyorsítótár](cache-configure.md#configure-redis-cache-settings) a a [Azure-portálon](https://portal.azure.com) kattintson **méretezési** a a **erőforrás menü**.</span><span class="sxs-lookup"><span data-stu-id="9af41-117">To scale your cache, [browse to the cache](cache-configure.md#configure-redis-cache-settings) in the [Azure portal](https://portal.azure.com) and click **Scale** from the **Resource menu**.</span></span>

![Méretezés](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="9af41-119">Válassza ki a kívánt tarifacsomagot a **válassza ki az IP-címek** panel megnyitásához, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="9af41-119">Select the desired pricing tier from the **Select pricing tier** blade and click **Select**.</span></span>

![Tarifacsomag][redis-cache-pricing-tier-blade]


<span data-ttu-id="9af41-121">Méretezhető egy másik tarifacsomagra a következő korlátozásokkal:</span><span class="sxs-lookup"><span data-stu-id="9af41-121">You can scale to a different pricing tier with the following restrictions:</span></span>

* <span data-ttu-id="9af41-122">Egy alacsonyabb tarifacsomagra, méretezhető nem a magasabb szintű tarifacsomagban használható.</span><span class="sxs-lookup"><span data-stu-id="9af41-122">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="9af41-123">Nem lehet méretezni a egy **prémium** le a gyorsítótár egy **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-123">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="9af41-124">Nem lehet méretezni a egy **szabványos** le a gyorsítótár egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-124">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="9af41-125">A méretezheti a **alapvető** gyorsítótárba egy **szabványos** gyorsítótár, de nem módosíthatja a méretét egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9af41-125">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="9af41-126">Ha különböző méretű van szüksége, végezhet egy későbbi skálázási műveletet, hogy a kívánt méretet.</span><span class="sxs-lookup"><span data-stu-id="9af41-126">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="9af41-127">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül egy **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-127">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="9af41-128">Kell méretezni a **alapvető** való **szabványos** egy skálázási műveletet, majd a **szabványos** való **prémium** a későbbi skálázás a műveletet.</span><span class="sxs-lookup"><span data-stu-id="9af41-128">You must scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="9af41-129">A nagyobb méretű le nem lehet méretezni a **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="9af41-129">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="9af41-130">Amíg a gyorsítótár az új tarifacsomagra méretezése folyik a **méretezés** állapota megjelenik a **Redis Cache** panelen.</span><span class="sxs-lookup"><span data-stu-id="9af41-130">While the cache is scaling to the new pricing tier, a **Scaling** status is displayed in the **Redis Cache** blade.</span></span>

![Méretezés][redis-cache-scaling]

<span data-ttu-id="9af41-132">Skálázás befejeződése után állapota a **méretezés** való **futtató**.</span><span class="sxs-lookup"><span data-stu-id="9af41-132">When scaling is complete, the status changes from **Scaling** to **Running**.</span></span>

## <a name="how-to-automate-a-scaling-operation"></a><span data-ttu-id="9af41-133">A méretezési művelet automatizálása</span><span class="sxs-lookup"><span data-stu-id="9af41-133">How to automate a scaling operation</span></span>
<span data-ttu-id="9af41-134">A gyorsítótár-példányok méretezési az Azure-portálon, mellett méretezheti PowerShell-parancsmagok, az Azure parancssori felület használatával és a Microsoft Azure felügyeleti függvénytárak (MAML) használatával.</span><span class="sxs-lookup"><span data-stu-id="9af41-134">In addition to scaling your cache instances in the Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using the Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="9af41-135">A skála PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="9af41-136">Scale Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="9af41-137">A skála MAML használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="9af41-138">A skála PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-138">Scale using PowerShell</span></span>
<span data-ttu-id="9af41-139">Az Azure Redis Cache példány a PowerShell használatával méretezheti a [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmag amikor a `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="9af41-139">You can scale your Azure Redis Cache instances with PowerShell by using the [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when the `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="9af41-140">A következő példa bemutatja, hogyan nevű gyorsítótár méretezési `myCache` 2,5 GB gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="9af41-140">The following example shows how to scale a cache named `myCache` to a 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="9af41-141">A skálázás a PowerShell használatával további információkért lásd: [méretezése a Powershell használatával Redis gyorsítótár](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="9af41-141">For more information on scaling with PowerShell, see [To scale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="9af41-142">Scale Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-142">Scale using Azure CLI</span></span>
<span data-ttu-id="9af41-143">Az Azure parancssori felület használatával Azure Redis Cache példány méretezése, hívja meg a `azure rediscache set` parancsot, és adja át a kívánt konfigurációs módosításokat, amely egy új méretét, a termékváltozat, illetve a fürt méretét, attól függően, hogy a méretezési műveletet.</span><span class="sxs-lookup"><span data-stu-id="9af41-143">To scale your Azure Redis Cache instances using Azure CLI, call the `azure rediscache set` command and pass in the desired configuration changes that include a new size, sku, or cluster size, depending on the desired scaling operation.</span></span>

<span data-ttu-id="9af41-144">Az Azure parancssori felülettel skálázás további információkért lásd: [egy meglévő Redis gyorsítótár beállításainak módosítása](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="9af41-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="9af41-145">A skála MAML használatával</span><span class="sxs-lookup"><span data-stu-id="9af41-145">Scale using MAML</span></span>
<span data-ttu-id="9af41-146">Méretezési az Azure Redis Cache-példányok használata a [Microsoft Azure felügyeleti függvénytárak (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), hívja az `IRedisOperations.CreateOrUpdate` metódus, és új mérete adjon át a `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="9af41-146">To scale your Azure Redis Cache instances using the [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call the `IRedisOperations.CreateOrUpdate` method and pass in the new size for the `RedisProperties.SKU.Capacity`.</span></span>

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

<span data-ttu-id="9af41-147">További információkért lásd: a [kezelése Redis Cache segítségével MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) minta.</span><span class="sxs-lookup"><span data-stu-id="9af41-147">For more information, see the [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="9af41-148">Méretezési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="9af41-148">Scaling FAQ</span></span>
<span data-ttu-id="9af41-149">Az alábbi lista tartalmazza az Azure Redis Cache skálázás gyakran feltett kérdésekre adott válaszok.</span><span class="sxs-lookup"><span data-stu-id="9af41-149">The following list contains answers to commonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="9af41-150">A, vagy a prémium szintű gyorsítótár is méretezhető?</span><span class="sxs-lookup"><span data-stu-id="9af41-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="9af41-151">Skálázás, után kell módosítani a gyorsítótár neve vagy a hozzáférési kulcsainak?</span><span class="sxs-lookup"><span data-stu-id="9af41-151">After scaling, do I have to change my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="9af41-152">Skálázás működése</span><span class="sxs-lookup"><span data-stu-id="9af41-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="9af41-153">I adat elvész a gyorsítótárból skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="9af41-154">Az egyéni adatbázisok állít érintett skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="9af41-155">A gyorsítótár elérhető lesz skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="9af41-156">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="9af41-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="9af41-157">Mennyi időt skálázás igénybe?</span><span class="sxs-lookup"><span data-stu-id="9af41-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="9af41-158">Hogyan állapítható meg, hogy ha skálázás befejeződött?</span><span class="sxs-lookup"><span data-stu-id="9af41-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="9af41-159">A, vagy a prémium szintű gyorsítótár is méretezhető?</span><span class="sxs-lookup"><span data-stu-id="9af41-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="9af41-160">Nem lehet méretezni a egy **prémium** le a gyorsítótár egy **alapvető** vagy **szabványos** tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="9af41-160">You can't scale from a **Premium** cache down to a **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="9af41-161">Az egyik méretezheti **prémium** egy másik tarifacsomagban gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-161">You can scale from one **Premium** cache pricing tier to another.</span></span>
* <span data-ttu-id="9af41-162">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül egy **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-162">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="9af41-163">Először át kell méretezni **alapvető** való **szabványos** egy skálázási műveletet, majd a **szabványos** való **prémium** egy későbbi skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="9af41-163">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="9af41-164">Ha engedélyezte a fürtszolgáltatás létrehozásakor a **prémium** gyorsítótárában, akkor [a fürt méretének módosítása](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="9af41-164">If you enabled clustering when you created your **Premium** cache, you can [change the cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="9af41-165">Ha a gyorsítótár fürtszolgáltatás engedélyezése nélkül hozták létre, akkor nem konfigurálhatja a fürtözést egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="9af41-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="9af41-166">További információk: [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md) (Fürtözés konfigurálása prémium szintű Azure Redis Cache-gyorsítótárhoz).</span><span class="sxs-lookup"><span data-stu-id="9af41-166">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a><span data-ttu-id="9af41-167">Skálázás, után kell módosítani a gyorsítótár neve vagy a hozzáférési kulcsainak?</span><span class="sxs-lookup"><span data-stu-id="9af41-167">After scaling, do I have to change my cache name or access keys?</span></span>
<span data-ttu-id="9af41-168">Nem, a gyorsítótár neve, valamint a kulcsok nem változik a méretezési művelet során.</span><span class="sxs-lookup"><span data-stu-id="9af41-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="9af41-169">Skálázás működése</span><span class="sxs-lookup"><span data-stu-id="9af41-169">How does scaling work?</span></span>
* <span data-ttu-id="9af41-170">Ha egy **alapvető** különböző méretű gyorsítótár van méretezhető, le van állítva, és új gyorsítótárat ki van építve, új méret használatával.</span><span class="sxs-lookup"><span data-stu-id="9af41-170">When a **Basic** cache is scaled to a different size, it is shut down and a new cache is provisioned using the new size.</span></span> <span data-ttu-id="9af41-171">Ebben az időszakban a gyorsítótár nem érhető el, és a gyorsítótárban lévő összes adatot elvész.</span><span class="sxs-lookup"><span data-stu-id="9af41-171">During this time, the cache is unavailable and all data in the cache is lost.</span></span>
* <span data-ttu-id="9af41-172">Ha egy **alapvető** gyorsítótár méretezve, hogy egy **szabványos** gyorsítótár, a replika gyorsítótár ki van építve, és az adatok másolja az elsődleges gyorsítótár a replika gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="9af41-172">When a **Basic** cache is scaled to a **Standard** cache, a replica cache is provisioned and the data is copied from the primary cache to the replica cache.</span></span> <span data-ttu-id="9af41-173">A gyorsítótár a méretezés közben elérhető marad.</span><span class="sxs-lookup"><span data-stu-id="9af41-173">The cache remains available during the scaling process.</span></span>
* <span data-ttu-id="9af41-174">Ha egy **szabványos** gyorsítótár méretezett egy másik értékre, vagy egy **prémium** gyorsítótár, a replikák közül leáll, és újra létrehozni, hogy az új méretet és az átvitt adatokat, majd a másik replika a feladatátvétel hajt végre, ahhoz, hogy újra kiépített, a folyamat során. a gyorsítótár-csomópontok közül az egyik hibát, hasonló.</span><span class="sxs-lookup"><span data-stu-id="9af41-174">When a **Standard** cache is scaled to a different size or to a **Premium** cache, one of the replicas is shut down and re-provisioned to the new size and the data transferred over, and then the other replica performs a failover before it is re-provisioned, similar to the process that occurs during a failure of one of the cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="9af41-175">I adat elvész a gyorsítótárból skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="9af41-176">Ha egy **alapvető** gyorsítótár van méretezve, hogy az új méretét, az összes adat elvész, és a gyorsítótár nem érhető el, a méretezési művelet során.</span><span class="sxs-lookup"><span data-stu-id="9af41-176">When a **Basic** cache is scaled to a new size, all data is lost and the cache is unavailable during the scaling operation.</span></span>
* <span data-ttu-id="9af41-177">Ha egy **alapvető** gyorsítótár méretezve, hogy egy **szabványos** gyorsítótár, a gyorsítótárban lévő adatok általában megőrződik.</span><span class="sxs-lookup"><span data-stu-id="9af41-177">When a **Basic** cache is scaled to a **Standard** cache, the data in the cache is typically preserved.</span></span>
* <span data-ttu-id="9af41-178">Ha egy **szabványos** gyorsítótár van méretezve, hogy egy nagyobb méretű vagy a réteg, vagy egy **prémium** gyorsítótár méretezett nagyobb méretűre, általában megmaradjon az összes adatot.</span><span class="sxs-lookup"><span data-stu-id="9af41-178">When a **Standard** cache is scaled to a larger size or tier, or a **Premium** cache is scaled to a larger size, all data is typically preserved.</span></span> <span data-ttu-id="9af41-179">Amikor skálázás egy **szabványos** vagy **prémium** gyorsítótár le egy kisebb méretet, az adatok elveszhetnek attól függően, hogy mennyi adatot a gyorsítótárban során azt méretezett kapcsolatos új méretét.</span><span class="sxs-lookup"><span data-stu-id="9af41-179">When scaling a **Standard** or **Premium** cache down to a smaller size, data may be lost depending on how much data is in the cache related to the new size when it is scaled.</span></span> <span data-ttu-id="9af41-180">Adat elvész, amikor a skálázás, ha a kulcsok kizárt vannak-e használata a [allkeys-lru](http://redis.io/topics/lru-cache) kiürítés házirend.</span><span class="sxs-lookup"><span data-stu-id="9af41-180">If data is lost when scaling down, keys are evicted using the [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="9af41-181">Az egyéni adatbázisok állít érintett skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="9af41-182">Különböző tartalmaznak az egyes tarifacsomagok [korlátok adatbázisok](cache-configure.md#databases), úgy, hogy nincs szempontokat konfigurálásakor skálázás Ha, egyéni értéket a `databases` beállítása a gyorsítótár létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="9af41-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for the `databases` setting during cache creation.</span></span>

* <span data-ttu-id="9af41-183">Ha a tarifacsomag alsó méretezhetők `databases` a jelenlegi rétegtől határa:</span><span class="sxs-lookup"><span data-stu-id="9af41-183">When scaling to a pricing tier with a lower `databases` limit than the current tier:</span></span>
  * <span data-ttu-id="9af41-184">Alapértelmezett számának használata `databases` ez 16 az összes árképzési szinteket, adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="9af41-184">If you are using the default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="9af41-185">Ha egyéni számos használ `databases` , amely esik a határokon belül a réteget, amelyhez van folyamatban, ez a `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="9af41-185">If you are using a custom number of `databases` that falls within the limits for the tier to which you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="9af41-186">Ha egyéni számos használ `databases` , amely meghaladja a új réteg a `databases` beállítás van az új réteget határain szintűre csökkent, és az eltávolított adatbázisokat az összes adat elvész.</span><span class="sxs-lookup"><span data-stu-id="9af41-186">If you are using a custom number of `databases` that exceeds the limits of the new tier, the `databases` setting is lowered to the limits of the new tier and all data in the removed databases is lost.</span></span>
* <span data-ttu-id="9af41-187">Ha a tarifacsomagot az azonos vagy újabb méretezhetők `databases` korlátot, mint a jelenlegi rétegtől a `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.</span><span class="sxs-lookup"><span data-stu-id="9af41-187">When scaling to a pricing tier with the same or higher `databases` limit than the current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="9af41-188">Vegye figyelembe, hogy míg a Standard és Premium gyorsítótárak egy 99,9 %-os SLA-t a rendelkezésre állás érdekében, az adatvesztés nélküli SLA.</span><span class="sxs-lookup"><span data-stu-id="9af41-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="9af41-189">A gyorsítótár elérhető lesz skálázás során?</span><span class="sxs-lookup"><span data-stu-id="9af41-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="9af41-190">**Standard** és **prémium** gyorsítótárak elérhetők maradnak a méretezési művelet során.</span><span class="sxs-lookup"><span data-stu-id="9af41-190">**Standard** and **Premium** caches remain available during the scaling operation.</span></span>
* <span data-ttu-id="9af41-191">**Alapszintű** gyorsítótárak skálázás különböző méretű műveletek során offline módban, de elérhetők maradnak, ha a méretezés **alapvető** való **szabványos**.</span><span class="sxs-lookup"><span data-stu-id="9af41-191">**Basic** caches are offline during scaling operations to a different size, but remain available when scaling from **Basic** to **Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="9af41-192">Nem támogatott műveletek</span><span class="sxs-lookup"><span data-stu-id="9af41-192">Operations that are not supported</span></span>
* <span data-ttu-id="9af41-193">Egy alacsonyabb tarifacsomagra, méretezhető nem a magasabb szintű tarifacsomagban használható.</span><span class="sxs-lookup"><span data-stu-id="9af41-193">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="9af41-194">Nem lehet méretezni a egy **prémium** le a gyorsítótár egy **szabványos** vagy egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-194">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="9af41-195">Nem lehet méretezni a egy **szabványos** le a gyorsítótár egy **alapvető** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-195">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="9af41-196">A méretezheti a **alapvető** gyorsítótárba egy **szabványos** gyorsítótár, de nem módosíthatja a méretét egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9af41-196">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="9af41-197">Ha különböző méretű van szüksége, végezhet egy későbbi skálázási műveletet, hogy a kívánt méretet.</span><span class="sxs-lookup"><span data-stu-id="9af41-197">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="9af41-198">Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül egy **prémium** gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="9af41-198">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="9af41-199">Először át kell méretezni **alapvető** való **szabványos** egy skálázási műveletet, majd a **szabványos** való **prémium** egy későbbi skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="9af41-199">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="9af41-200">A nagyobb méretű le nem lehet méretezni a **C0 csomag (250 MB)** méretét.</span><span class="sxs-lookup"><span data-stu-id="9af41-200">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>

<span data-ttu-id="9af41-201">A méretezési művelet sikertelen lesz, ha a szolgáltatás megpróbál visszaállítani a műveletet, és a gyorsítótár visszaáll az eredeti méret.</span><span class="sxs-lookup"><span data-stu-id="9af41-201">If a scaling operation fails, the service will try to revert the operation and the cache will revert to the original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="9af41-202">Mennyi időt skálázás igénybe?</span><span class="sxs-lookup"><span data-stu-id="9af41-202">How long does scaling take?</span></span>
<span data-ttu-id="9af41-203">Skálázás vesz körülbelül 20 percet, attól függően, hogy mennyi adatot a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="9af41-203">Scaling takes approximately 20 minutes, depending on how much data is in the cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="9af41-204">Hogyan állapítható meg, hogy ha skálázás befejeződött?</span><span class="sxs-lookup"><span data-stu-id="9af41-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="9af41-205">Az Azure-portálon tekintheti meg a skálázási művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="9af41-205">In the Azure portal you can see the scaling operation in progress.</span></span> <span data-ttu-id="9af41-206">Ha skálázás befejeződött, a gyorsítótár állapota a **futtató**.</span><span class="sxs-lookup"><span data-stu-id="9af41-206">When scaling is complete, the status of the cache changes to **Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



