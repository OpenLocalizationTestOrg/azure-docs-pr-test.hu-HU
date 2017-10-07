---
title: Azure Redis Cache aaaHow tooUse |} Microsoft Docs
description: "Ismerje meg, hogyan tooimprove hello Azure Redis Cache segítségével az Azure alkalmazások teljesítményének"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 763d70c10972eec9a1885969e8da5bf1c4084727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache"></a><span data-ttu-id="eee75-103">Hogyan tooUse Azure Redis Cache-gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="eee75-103">How tooUse Azure Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="eee75-104">.NET</span><span class="sxs-lookup"><span data-stu-id="eee75-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="eee75-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eee75-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="eee75-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="eee75-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="eee75-107">Java</span><span class="sxs-lookup"><span data-stu-id="eee75-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="eee75-108">Python</span><span class="sxs-lookup"><span data-stu-id="eee75-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="eee75-109">Ez az útmutató bemutatja, hogyan tooget használatának **Azure Redis Cache**.</span><span class="sxs-lookup"><span data-stu-id="eee75-109">This guide shows you how tooget started using **Azure Redis Cache**.</span></span> <span data-ttu-id="eee75-110">A Microsoft Azure Redis Cache hello népszerű nyílt forráskódú Redis Cache alapul.</span><span class="sxs-lookup"><span data-stu-id="eee75-110">Microsoft Azure Redis Cache is based on hello popular open source Redis Cache.</span></span> <span data-ttu-id="eee75-111">Ez lehetővé teszi az tooa biztonságos, dedikált Redis gyorsítótár, a Microsoft által felügyelt eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="eee75-111">It gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="eee75-112">Az Azure Redis Cache használatával létrehozott gyorsítótárak a Microsoft Azure összes alkalmazásából elérhetőek.</span><span class="sxs-lookup"><span data-stu-id="eee75-112">A cache created using Azure Redis Cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="eee75-113">A Microsoft Azure Redis Cache rétegek a következő hello érhető el:</span><span class="sxs-lookup"><span data-stu-id="eee75-113">Microsoft Azure Redis Cache is available in hello following tiers:</span></span>

* <span data-ttu-id="eee75-114">**Alapszintű** – Egyetlen csomópont.</span><span class="sxs-lookup"><span data-stu-id="eee75-114">**Basic** – Single node.</span></span> <span data-ttu-id="eee75-115">Több méretek too53 GB fel.</span><span class="sxs-lookup"><span data-stu-id="eee75-115">Multiple sizes up too53 GB.</span></span>
* <span data-ttu-id="eee75-116">**Standard** – Két csomópontból álló elsődleges/replika.</span><span class="sxs-lookup"><span data-stu-id="eee75-116">**Standard** – Two-node Primary/Replica.</span></span> <span data-ttu-id="eee75-117">Több méretek too53 GB fel.</span><span class="sxs-lookup"><span data-stu-id="eee75-117">Multiple sizes up too53 GB.</span></span> <span data-ttu-id="eee75-118">99,9%-os SLA.</span><span class="sxs-lookup"><span data-stu-id="eee75-118">99.9% SLA.</span></span>
* <span data-ttu-id="eee75-119">**Prémium szintű** – két csomópontos elsődleges vagy replika too10 szilánkok fel.</span><span class="sxs-lookup"><span data-stu-id="eee75-119">**Premium** – Two-node Primary/Replica with up too10 shards.</span></span> <span data-ttu-id="eee75-120">Több méretek 6 GB too530 GB.</span><span class="sxs-lookup"><span data-stu-id="eee75-120">Multiple sizes from 6 GB too530 GB.</span></span> <span data-ttu-id="eee75-121">Tartalmazza a Standard szint összes szolgáltatását és továbbiakat, beleértve a [Redis-fürtök](cache-how-to-premium-clustering.md), a [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) és az [Azure Virtual Network](cache-how-to-premium-vnet.md) támogatását.</span><span class="sxs-lookup"><span data-stu-id="eee75-121">All Standard tier features and more including support for [Redis cluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="eee75-122">99,9%-os SLA.</span><span class="sxs-lookup"><span data-stu-id="eee75-122">99.9% SLA.</span></span>

<span data-ttu-id="eee75-123">Az egyes szintek szolgáltatási feltételei és díjszabása eltérő.</span><span class="sxs-lookup"><span data-stu-id="eee75-123">Each tier differs in terms of features and pricing.</span></span> <span data-ttu-id="eee75-124">További díjszabási információk: [Díjszabás – Redis Cache][Cache Pricing Details].</span><span class="sxs-lookup"><span data-stu-id="eee75-124">For information on pricing, see [Cache Pricing Details][Cache Pricing Details].</span></span>

<span data-ttu-id="eee75-125">Ez az útmutató bemutatja, hogyan toouse hello [StackExchange.Redis] [ StackExchange.Redis] ügyfél C használatával\# kódot.</span><span class="sxs-lookup"><span data-stu-id="eee75-125">This guide shows you how toouse hello [StackExchange.Redis][StackExchange.Redis] client using C\# code.</span></span> <span data-ttu-id="eee75-126">hello tárgyalt forgatókönyvekben szerepel a **létrehozásáról és konfigurálásáról a gyorsítótár**, **gyorsítótár-ügyfelek konfigurálása**, és **hozzáadása és eltávolítása az objektumok hello gyorsítótárból**.</span><span class="sxs-lookup"><span data-stu-id="eee75-126">hello scenarios covered include **creating and configuring a cache**, **configuring cache clients**, and **adding and removing objects from hello cache**.</span></span> <span data-ttu-id="eee75-127">Az Azure Redis Cache használatával kapcsolatos további információkat a [Következő lépések][Next Steps] című szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="eee75-127">For more information on using Azure Redis Cache, see [Next Steps][Next Steps].</span></span> <span data-ttu-id="eee75-128">ASP.NET MVC webes alkalmazás a Redis Cache létrehozásának részletes oktatóanyaga, lásd: [hogyan toocreate a webes alkalmazás a Redis Cache](cache-web-app-howto.md).</span><span class="sxs-lookup"><span data-stu-id="eee75-128">For a step-by-step tutorial of building an ASP.NET MVC web app with Redis Cache, see [How toocreate a Web App with Redis Cache](cache-web-app-howto.md).</span></span>

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a><span data-ttu-id="eee75-129">Bevezetés az Azure Redis Cache használatába</span><span class="sxs-lookup"><span data-stu-id="eee75-129">Get Started with Azure Redis Cache</span></span>
<span data-ttu-id="eee75-130">Az első lépések az Azure Redis Cache-ben egyszerűek.</span><span class="sxs-lookup"><span data-stu-id="eee75-130">Getting started with Azure Redis Cache is easy.</span></span> <span data-ttu-id="eee75-131">tooget indult el, akkor kiépítéséhez, és állítsa be a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="eee75-131">tooget started, you provision and configure a cache.</span></span> <span data-ttu-id="eee75-132">A következő hello gyorsítótár-ügyfelek konfigurálása, így hello gyorsítótár eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="eee75-132">Next, you configure hello cache clients so they can access hello cache.</span></span> <span data-ttu-id="eee75-133">Hello gyorsítótár-ügyfelek konfigurálása után megkezdheti a munkát őket.</span><span class="sxs-lookup"><span data-stu-id="eee75-133">Once hello cache clients are configured, you can begin working with them.</span></span>

* <span data-ttu-id="eee75-134">[Hello gyorsítótár létrehozása][Create hello cache]</span><span class="sxs-lookup"><span data-stu-id="eee75-134">[Create hello cache][Create hello cache]</span></span>
* <span data-ttu-id="eee75-135">[Hello gyorsítótár-ügyfelek konfigurálása][Configure hello cache clients]</span><span class="sxs-lookup"><span data-stu-id="eee75-135">[Configure hello cache clients][Configure hello cache clients]</span></span>

<a name="create-cache"></a>

## <a name="create-a-cache"></a><span data-ttu-id="eee75-136">Gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="eee75-136">Create a cache</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a><span data-ttu-id="eee75-137">a gyorsítótár azt követően tooaccess jön létre</span><span class="sxs-lookup"><span data-stu-id="eee75-137">tooaccess your cache after it's created</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="eee75-138">A gyorsítótár konfigurálásával kapcsolatos további információkért lásd: [hogyan tooconfigure Azure Redis Cache](cache-configure.md).</span><span class="sxs-lookup"><span data-stu-id="eee75-138">For more information about configuring your cache, see [How tooconfigure Azure Redis Cache](cache-configure.md).</span></span>

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a><span data-ttu-id="eee75-139">Hello gyorsítótár-ügyfelek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eee75-139">Configure hello cache clients</span></span>
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

<span data-ttu-id="eee75-140">Miután ügyfélprojektbe beállította a gyorsítótár, a következő szakaszok a gyorsítótárhoz való munkához hello ismertetett hello módszerek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="eee75-140">Once your client project is configured for caching, you can use hello techniques described in hello following sections for working with your cache.</span></span>

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a><span data-ttu-id="eee75-141">A gyorsítótárak használata</span><span class="sxs-lookup"><span data-stu-id="eee75-141">Working with Caches</span></span>
<span data-ttu-id="eee75-142">Ebben a szakaszban hello lépések bemutatják, hogyan tooperform gyakori feladatokat, a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="eee75-142">hello steps in this section describe how tooperform common tasks with Cache.</span></span>

* <span data-ttu-id="eee75-143">[Csatlakozás toohello gyorsítótár][Connect toohello cache]</span><span class="sxs-lookup"><span data-stu-id="eee75-143">[Connect toohello cache][Connect toohello cache]</span></span>
* <span data-ttu-id="eee75-144">[Adja hozzá és hello gyorsítótárból objektumok beolvasása][Add and retrieve objects from hello cache]</span><span class="sxs-lookup"><span data-stu-id="eee75-144">[Add and retrieve objects from hello cache][Add and retrieve objects from hello cache]</span></span>
* [<span data-ttu-id="eee75-145">.NET-objektumok hello gyorsítótárban</span><span class="sxs-lookup"><span data-stu-id="eee75-145">Work with .NET objects in hello cache</span></span>](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a><span data-ttu-id="eee75-146">Csatlakozás toohello gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="eee75-146">Connect toohello cache</span></span>
<span data-ttu-id="eee75-147">tooprogrammatically munkahelyi gyorsítótárával, egy hivatkozási toohello gyorsítótár szüksége.</span><span class="sxs-lookup"><span data-stu-id="eee75-147">tooprogrammatically work with a cache, you need a reference toohello cache.</span></span> <span data-ttu-id="eee75-148">Adja hozzá a következő minden olyan fájlt, amelyből kívánt toouse hello StackExchange.Redis ügyfél tooaccess Azure Redis Cache toohello felső hello.</span><span class="sxs-lookup"><span data-stu-id="eee75-148">Add hello following toohello top of any file from which you want toouse hello StackExchange.Redis client tooaccess an Azure Redis Cache.</span></span>

    using StackExchange.Redis;

> [!NOTE]
> <span data-ttu-id="eee75-149">hello StackExchange.Redis ügyfél .NET-keretrendszer 4-es vagy újabb szükséges.</span><span class="sxs-lookup"><span data-stu-id="eee75-149">hello StackExchange.Redis client requires .NET Framework 4 or higher.</span></span>
> 
> 

<span data-ttu-id="eee75-150">Azure Redis Cache hello kezeli kapcsolat toohello hello `ConnectionMultiplexer` osztály.</span><span class="sxs-lookup"><span data-stu-id="eee75-150">hello connection toohello Azure Redis Cache is managed by hello `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="eee75-151">Ez az osztály kell osztható meg, és használja fel újra az ügyfélalkalmazást teljes, és nem kell toobe / művelet alapja létre.</span><span class="sxs-lookup"><span data-stu-id="eee75-151">This class should be shared and reused throughout your client application, and does not need toobe created on a per operation basis.</span></span> 

<span data-ttu-id="eee75-152">Azure Redis Cache tooconnect tooan, és visszahelyezi a csatlakoztatott példányának `ConnectionMultiplexer`, hívás hello statikus `Connect` metódus és hello adjon át a gyorsítótár-végpont és a kulcs.</span><span class="sxs-lookup"><span data-stu-id="eee75-152">tooconnect tooan Azure Redis Cache and be returned an instance of a connected `ConnectionMultiplexer`, call hello static `Connect` method and pass in hello cache endpoint and key.</span></span> <span data-ttu-id="eee75-153">Hello kulcs használata jönnek létre hello Azure-portálon hello jelszó paraméterként.</span><span class="sxs-lookup"><span data-stu-id="eee75-153">Use hello key generated from hello Azure portal as hello password parameter.</span></span>

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> <span data-ttu-id="eee75-154">Figyelmeztetés: Soha ne tároljon hitelesítő adatokat a forráskódban.</span><span class="sxs-lookup"><span data-stu-id="eee75-154">Warning: Never store credentials in source code.</span></span> <span data-ttu-id="eee75-155">tookeep a minta egyszerű I vagyok megjelenítő őket hello forráskódot.</span><span class="sxs-lookup"><span data-stu-id="eee75-155">tookeep this sample simple, I’m showing them in hello source code.</span></span> <span data-ttu-id="eee75-156">Lásd: [hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok munkahelyi] [ How Application Strings and Connection Strings Work] kapcsolatos információk toostore hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="eee75-156">See [How Application Strings and Connection Strings Work][How Application Strings and Connection Strings Work] for information on how toostore credentials.</span></span>
> 
> 

<span data-ttu-id="eee75-157">Ha nem szeretné toouse SSL, vagy állítsa be `ssl=false` , vagy hagyja el a hello `ssl` paraméter.</span><span class="sxs-lookup"><span data-stu-id="eee75-157">If you don't want toouse SSL, either set `ssl=false` or omit hello `ssl` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="eee75-158">hello nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="eee75-158">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="eee75-159">Hello nem SSL port engedélyezésével kapcsolatos útmutatásért lásd: [hozzáférési portok](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="eee75-159">For instructions on enabling hello non-SSL port, see [Access Ports](cache-configure.md#access-ports).</span></span>
> 
> 

<span data-ttu-id="eee75-160">Egyik módszer toosharing egy `ConnectionMultiplexer` toohave csatlakoztatott példány, a következő példa hasonló toohello visszaadó statikus tulajdonság példány az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="eee75-160">One approach toosharing a `ConnectionMultiplexer` instance in your application is toohave a static property that returns a connected instance, similar toohello following example.</span></span> <span data-ttu-id="eee75-161">Ez a megközelítés biztosítja egy szálbiztos módon tooinitialize egyetlen csatlakoztatott `ConnectionMultiplexer` példány.</span><span class="sxs-lookup"><span data-stu-id="eee75-161">This approach provides a thread-safe way tooinitialize only a single connected `ConnectionMultiplexer` instance.</span></span> <span data-ttu-id="eee75-162">Ezekben a példákban `abortConnect` van beállítva toofalse, ami azt jelenti, hogy hello kötés akkor is, ha a kapcsolat toohello Azure Redis Cache nem jön létre.</span><span class="sxs-lookup"><span data-stu-id="eee75-162">In these examples `abortConnect` is set toofalse, which means that hello call succeeds even if a connection toohello Azure Redis Cache is not established.</span></span> <span data-ttu-id="eee75-163">Egy alapfunkciója `ConnectionMultiplexer` , hogy automatikusan helyreállítja kapcsolat toohello gyorsítótár után hello hálózati hiba vagy más ok megoldották.</span><span class="sxs-lookup"><span data-stu-id="eee75-163">One key feature of `ConnectionMultiplexer` is that it automatically restores connectivity toohello cache once hello network issue or other causes are resolved.</span></span>

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

<span data-ttu-id="eee75-164">További információt a speciális kapcsolatkonfigurációs lehetőségekről a [StackExchange.Redis konfigurációs modellt][StackExchange.Redis configuration model] ismertető cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="eee75-164">For more information on advanced connection configuration options, see [StackExchange.Redis configuration model][StackExchange.Redis configuration model].</span></span>

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<span data-ttu-id="eee75-165">Hello kapcsolat létrejötte után vissza a hivatkozás toohello redis gyorsítótár adatbázisában: hívó hello `ConnectionMultiplexer.GetDatabase` metódust.</span><span class="sxs-lookup"><span data-stu-id="eee75-165">Once hello connection is established, return a reference toohello redis cache database by calling hello `ConnectionMultiplexer.GetDatabase` method.</span></span> <span data-ttu-id="eee75-166">hello hello által visszaadott `GetDatabase` metódus egy egyszerűsített csatlakoztatott objektum, és nem kell toobe tárolja.</span><span class="sxs-lookup"><span data-stu-id="eee75-166">hello object returned from hello `GetDatabase` method is a lightweight pass-through object and does not need toobe stored.</span></span>

    // Connection refers tooa property that returns a ConnectionMultiplexer
    // as shown in hello previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using hello cache object...
    // Simple put of integral data types into hello cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from hello cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

<span data-ttu-id="eee75-167">Azure Redis gyorsítótár-adatbázisok (16-os alapértelmezett), amely a Redis gyorsítótár használt toologically külön hello adatainak konfigurálható több van.</span><span class="sxs-lookup"><span data-stu-id="eee75-167">Azure Redis caches have a configurable number of databases (default of 16) that can be used toologically separate hello data within a Redis cache.</span></span> <span data-ttu-id="eee75-168">További információ: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases) és [A Redis-kiszolgáló alapértelmezett konfigurációja](cache-configure.md#default-redis-server-configuration).</span><span class="sxs-lookup"><span data-stu-id="eee75-168">For more information, see [What are Redis databases?](cache-faq.md#what-are-redis-databases) and [Default Redis server configuration](cache-configure.md#default-redis-server-configuration).</span></span>

<span data-ttu-id="eee75-169">Most, hogy tudja, hogyan tooconnect tooan Azure Redis Cache példányt, és térjen vissza a hivatkozás toohello gyorsítótár adatbázis, nézzük hello gyorsítótár használata.</span><span class="sxs-lookup"><span data-stu-id="eee75-169">Now that you know how tooconnect tooan Azure Redis Cache instance and return a reference toohello cache database, let's look at working with hello cache.</span></span>

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a><span data-ttu-id="eee75-170">Adja hozzá és hello gyorsítótárból objektumok beolvasása</span><span class="sxs-lookup"><span data-stu-id="eee75-170">Add and retrieve objects from hello cache</span></span>
<span data-ttu-id="eee75-171">Elemek tárolja, és a gyorsítótár lekért hello segítségével `StringSet` és `StringGet` módszerek.</span><span class="sxs-lookup"><span data-stu-id="eee75-171">Items can be stored in and retrieved from a cache by using hello `StringSet` and `StringGet` methods.</span></span>

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

<span data-ttu-id="eee75-172">Redis Redis karakterláncok, de ezek a karakterláncok legtöbb adatok is szerepelhetnek számos különböző típusú adatok, beleértve a szerializált bináris adatait, amely is használható, ha .NET tárolása objektumokat hello gyorsítótárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="eee75-172">Redis stores most data as Redis strings, but these strings can contain many types of data, including serialized binary data, which can be used when storing .NET objects in hello cache.</span></span>

<span data-ttu-id="eee75-173">Meghívásakor `StringGet`, ha a hello objektum létezik, akkor adja vissza, és ha nem létezik, `null` adja vissza.</span><span class="sxs-lookup"><span data-stu-id="eee75-173">When calling `StringGet`, if hello object exists, it is returned, and if it does not, `null` is returned.</span></span> <span data-ttu-id="eee75-174">Ha `null` lett visszaadva, akkor hello kívánt adatforrás lekérje hello értéket, és a későbbi használatra hello gyorsítótárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="eee75-174">If `null` is returned, you can retrieve hello value from hello desired data source and store it in hello cache for subsequent use.</span></span> <span data-ttu-id="eee75-175">A használati mód hello gyorsítótár-tartalékoljon mintát néven ismert.</span><span class="sxs-lookup"><span data-stu-id="eee75-175">This usage pattern is known as hello cache-aside pattern.</span></span>

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

<span data-ttu-id="eee75-176">Is `RedisValue`, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="eee75-176">You can also use `RedisValue`, as shown in hello following example.</span></span> <span data-ttu-id="eee75-177">A `RedisValue` implicit operátorokkal rendelkezik az egész szám típusú adatokkal történő munkavégzéshez, ami akkor lehet hasznos, ha a `null` egy gyorsítótárazott elem várt értéke.</span><span class="sxs-lookup"><span data-stu-id="eee75-177">`RedisValue` has implicit operators for working with integral data types, and can be useful if `null` is an expected value for a cached item.</span></span>


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


<span data-ttu-id="eee75-178">egy elem gyorsítótárában hello használata hello toospecify hello lejáratának `TimeSpan` paramétere `StringSet`.</span><span class="sxs-lookup"><span data-stu-id="eee75-178">toospecify hello expiration of an item in hello cache, use hello `TimeSpan` parameter of `StringSet`.</span></span>

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a><span data-ttu-id="eee75-179">.NET-objektumok hello gyorsítótárban</span><span class="sxs-lookup"><span data-stu-id="eee75-179">Work with .NET objects in hello cache</span></span>
<span data-ttu-id="eee75-180">Az Azure Redis Cache .NET-objektumokat és primitív adattípusokat is képes gyorsítótárazni, de a .NET-objektumokat a gyorsítótárazásuk előtt szerializálni kell.</span><span class="sxs-lookup"><span data-stu-id="eee75-180">Azure Redis Cache can cache both .NET objects and primitive data types, but before a .NET object can be cached it must be serialized.</span></span> <span data-ttu-id="eee75-181">A .NET-objektum szerializálás hello alkalmazásfejlesztő hello felelőssége, és hello fejlesztői rugalmasságot biztosít a hello szerializáló hello választott.</span><span class="sxs-lookup"><span data-stu-id="eee75-181">This .NET object serialization is hello responsibility of hello application developer, and gives hello developer flexibility in hello choice of hello serializer.</span></span>

<span data-ttu-id="eee75-182">Egy egyszerű módon tooserialize objektumokat toouse hello `JsonConvert` szerializálási metódusokon [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) és tooand JSON formátumból szerializálni.</span><span class="sxs-lookup"><span data-stu-id="eee75-182">One simple way tooserialize objects is toouse hello `JsonConvert` serialization methods in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) and serialize tooand from JSON.</span></span> <span data-ttu-id="eee75-183">hello következő példa bemutatja a get és set használatával egy `Employee` objektumpéldány.</span><span class="sxs-lookup"><span data-stu-id="eee75-183">hello following example shows a get and set using an `Employee` object instance.</span></span>

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store toocache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a><span data-ttu-id="eee75-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eee75-184">Next Steps</span></span>
<span data-ttu-id="eee75-185">Most, hogy megismerte hello alapjait, kövesse a további információk az Azure Redis Cache hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="eee75-185">Now that you've learned hello basics, follow these links toolearn more about Azure Redis Cache.</span></span>

* <span data-ttu-id="eee75-186">Tekintse meg a hello Azure Redis Cache ASP.NET-szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="eee75-186">Check out hello ASP.NET providers for Azure Redis Cache.</span></span>
  * [<span data-ttu-id="eee75-187">Az Azure Redis munkamenetállapot-szolgáltatója</span><span class="sxs-lookup"><span data-stu-id="eee75-187">Azure Redis Session State Provider</span></span>](cache-aspnet-session-state-provider.md)
  * [<span data-ttu-id="eee75-188">Az Azure Redis Cache ASP.NET kimenetigyorsítótár-szolgáltatója</span><span class="sxs-lookup"><span data-stu-id="eee75-188">Azure Redis Cache ASP.NET Output Cache Provider</span></span>](cache-aspnet-output-cache-provider.md)
* <span data-ttu-id="eee75-189">[Gyorsítótár-diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) így [figyelő](cache-how-to-monitor.md) hello a gyorsítótár állapotát.</span><span class="sxs-lookup"><span data-stu-id="eee75-189">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span> <span data-ttu-id="eee75-190">Megtekintheti a hello Azure-portálon, és a metrikák is hello [töltse le, és tekintse át](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) őket az Ön által választott hello eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="eee75-190">You can view hello metrics in hello Azure portal and you can also [download and review](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) them using hello tools of your choice.</span></span>
* <span data-ttu-id="eee75-191">Tekintse meg a hello [StackExchange.Redis gyorsítótár ügyfél dokumentáció][StackExchange.Redis cache client documentation].</span><span class="sxs-lookup"><span data-stu-id="eee75-191">Check out hello [StackExchange.Redis cache client documentation][StackExchange.Redis cache client documentation].</span></span>
  * <span data-ttu-id="eee75-192">Az Azure Redis Cache számos Redis-ügyfélből és fejlesztési nyelvből elérhető.</span><span class="sxs-lookup"><span data-stu-id="eee75-192">Azure Redis Cache can be accessed from many Redis clients and development languages.</span></span> <span data-ttu-id="eee75-193">További információ: [http://redis.io/clients][http://redis.io/clients].</span><span class="sxs-lookup"><span data-stu-id="eee75-193">For more information, see [http://redis.io/clients][http://redis.io/clients].</span></span>
* <span data-ttu-id="eee75-194">Az Azure Redis Cache olyan harmadik féltől származó szolgáltatásokkal és eszközökkel is használható, mint a Redsmin vagy a Redis Destkop Manager.</span><span class="sxs-lookup"><span data-stu-id="eee75-194">Azure Redis Cache can also be used with third-party services and tools such as Redsmin and Redis Desktop Manager.</span></span>
  * <span data-ttu-id="eee75-195">Redsmin kapcsolatos további információkért lásd: [hogyan tooretrieve az Azure Redis kapcsolódási karakterláncot, és használatához a Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span><span class="sxs-lookup"><span data-stu-id="eee75-195">For more information about Redsmin, see [How tooretrieve an Azure Redis connection string and use it with Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span></span>
  * <span data-ttu-id="eee75-196">Az Azure Redis Cache-ben tárolt adatait a [RedisDesktopManagert](https://github.com/uglide/RedisDesktopManager) használó grafikus felhasználó felületen érheti el és vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="eee75-196">Access and inspect your data in Azure Redis Cache with a GUI using [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).</span></span>
* <span data-ttu-id="eee75-197">Lásd: hello [redis] [ redis] dokumentációk és információk a [adattípusok redis] [ redis data types] és [egy 15 perces bemutatása tooRedis adattípusok][a fifteen minute introduction tooRedis data types].</span><span class="sxs-lookup"><span data-stu-id="eee75-197">See hello [redis][redis] documentation and read about [redis data types][redis data types] and [a fifteen minute introduction tooRedis data types][a fifteen minute introduction tooRedis data types].</span></span>

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction tooAzure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project tooUse Azure Caching]: #prepare-vs
[Configure Your Application tooUse Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create hello cache]: #create-cache
[Configure hello cache]: #enable-caching
[Configure hello cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect toohello cache]: #connect-to-cache
[Add and retrieve objects from hello cache]: #add-object
[Specify hello expiration of an object in hello cache]: #specify-expiration
[Store ASP.NET session state in hello cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How tooretrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How tooConfigure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set hello Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in hello cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate tooAzure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups toomanage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction tooRedis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


