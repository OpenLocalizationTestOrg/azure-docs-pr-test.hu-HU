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
# <a name="how-toouse-azure-redis-cache"></a>Hogyan tooUse Azure Redis Cache-gyorsítótár
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Ez az útmutató bemutatja, hogyan tooget használatának **Azure Redis Cache**. A Microsoft Azure Redis Cache hello népszerű nyílt forráskódú Redis Cache alapul. Ez lehetővé teszi az tooa biztonságos, dedikált Redis gyorsítótár, a Microsoft által felügyelt eléréséhez. Az Azure Redis Cache használatával létrehozott gyorsítótárak a Microsoft Azure összes alkalmazásából elérhetőek.

A Microsoft Azure Redis Cache rétegek a következő hello érhető el:

* **Alapszintű** – Egyetlen csomópont. Több méretek too53 GB fel.
* **Standard** – Két csomópontból álló elsődleges/replika. Több méretek too53 GB fel. 99,9%-os SLA.
* **Prémium szintű** – két csomópontos elsődleges vagy replika too10 szilánkok fel. Több méretek 6 GB too530 GB. Tartalmazza a Standard szint összes szolgáltatását és továbbiakat, beleértve a [Redis-fürtök](cache-how-to-premium-clustering.md), a [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) és az [Azure Virtual Network](cache-how-to-premium-vnet.md) támogatását. 99,9%-os SLA.

Az egyes szintek szolgáltatási feltételei és díjszabása eltérő. További díjszabási információk: [Díjszabás – Redis Cache][Cache Pricing Details].

Ez az útmutató bemutatja, hogyan toouse hello [StackExchange.Redis] [ StackExchange.Redis] ügyfél C használatával\# kódot. hello tárgyalt forgatókönyvekben szerepel a **létrehozásáról és konfigurálásáról a gyorsítótár**, **gyorsítótár-ügyfelek konfigurálása**, és **hozzáadása és eltávolítása az objektumok hello gyorsítótárból**. Az Azure Redis Cache használatával kapcsolatos további információkat a [Következő lépések][Next Steps] című szakaszban talál. ASP.NET MVC webes alkalmazás a Redis Cache létrehozásának részletes oktatóanyaga, lásd: [hogyan toocreate a webes alkalmazás a Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Bevezetés az Azure Redis Cache használatába
Az első lépések az Azure Redis Cache-ben egyszerűek. tooget indult el, akkor kiépítéséhez, és állítsa be a gyorsítótárba. A következő hello gyorsítótár-ügyfelek konfigurálása, így hello gyorsítótár eléréséhez. Hello gyorsítótár-ügyfelek konfigurálása után megkezdheti a munkát őket.

* [Hello gyorsítótár létrehozása][Create hello cache]
* [Hello gyorsítótár-ügyfelek konfigurálása][Configure hello cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Gyorsítótár létrehozása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a>a gyorsítótár azt követően tooaccess jön létre
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

A gyorsítótár konfigurálásával kapcsolatos további információkért lásd: [hogyan tooconfigure Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a>Hello gyorsítótár-ügyfelek konfigurálása
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Miután ügyfélprojektbe beállította a gyorsítótár, a következő szakaszok a gyorsítótárhoz való munkához hello ismertetett hello módszerek is használhatja.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>A gyorsítótárak használata
Ebben a szakaszban hello lépések bemutatják, hogyan tooperform gyakori feladatokat, a gyorsítótár.

* [Csatlakozás toohello gyorsítótár][Connect toohello cache]
* [Adja hozzá és hello gyorsítótárból objektumok beolvasása][Add and retrieve objects from hello cache]
* [.NET-objektumok hello gyorsítótárban](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a>Csatlakozás toohello gyorsítótár
tooprogrammatically munkahelyi gyorsítótárával, egy hivatkozási toohello gyorsítótár szüksége. Adja hozzá a következő minden olyan fájlt, amelyből kívánt toouse hello StackExchange.Redis ügyfél tooaccess Azure Redis Cache toohello felső hello.

    using StackExchange.Redis;

> [!NOTE]
> hello StackExchange.Redis ügyfél .NET-keretrendszer 4-es vagy újabb szükséges.
> 
> 

Azure Redis Cache hello kezeli kapcsolat toohello hello `ConnectionMultiplexer` osztály. Ez az osztály kell osztható meg, és használja fel újra az ügyfélalkalmazást teljes, és nem kell toobe / művelet alapja létre. 

Azure Redis Cache tooconnect tooan, és visszahelyezi a csatlakoztatott példányának `ConnectionMultiplexer`, hívás hello statikus `Connect` metódus és hello adjon át a gyorsítótár-végpont és a kulcs. Hello kulcs használata jönnek létre hello Azure-portálon hello jelszó paraméterként.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Figyelmeztetés: Soha ne tároljon hitelesítő adatokat a forráskódban. tookeep a minta egyszerű I vagyok megjelenítő őket hello forráskódot. Lásd: [hogyan alkalmazás karakterláncok és a kapcsolati karakterláncok munkahelyi] [ How Application Strings and Connection Strings Work] kapcsolatos információk toostore hitelesítő adatokat.
> 
> 

Ha nem szeretné toouse SSL, vagy állítsa be `ssl=false` , vagy hagyja el a hello `ssl` paraméter.

> [!NOTE]
> hello nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva. Hello nem SSL port engedélyezésével kapcsolatos útmutatásért lásd: [hozzáférési portok](cache-configure.md#access-ports).
> 
> 

Egyik módszer toosharing egy `ConnectionMultiplexer` toohave csatlakoztatott példány, a következő példa hasonló toohello visszaadó statikus tulajdonság példány az alkalmazásban. Ez a megközelítés biztosítja egy szálbiztos módon tooinitialize egyetlen csatlakoztatott `ConnectionMultiplexer` példány. Ezekben a példákban `abortConnect` van beállítva toofalse, ami azt jelenti, hogy hello kötés akkor is, ha a kapcsolat toohello Azure Redis Cache nem jön létre. Egy alapfunkciója `ConnectionMultiplexer` , hogy automatikusan helyreállítja kapcsolat toohello gyorsítótár után hello hálózati hiba vagy más ok megoldották.

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

További információt a speciális kapcsolatkonfigurációs lehetőségekről a [StackExchange.Redis konfigurációs modellt][StackExchange.Redis configuration model] ismertető cikkben talál.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Hello kapcsolat létrejötte után vissza a hivatkozás toohello redis gyorsítótár adatbázisában: hívó hello `ConnectionMultiplexer.GetDatabase` metódust. hello hello által visszaadott `GetDatabase` metódus egy egyszerűsített csatlakoztatott objektum, és nem kell toobe tárolja.

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

Azure Redis gyorsítótár-adatbázisok (16-os alapértelmezett), amely a Redis gyorsítótár használt toologically külön hello adatainak konfigurálható több van. További információ: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases) és [A Redis-kiszolgáló alapértelmezett konfigurációja](cache-configure.md#default-redis-server-configuration).

Most, hogy tudja, hogyan tooconnect tooan Azure Redis Cache példányt, és térjen vissza a hivatkozás toohello gyorsítótár adatbázis, nézzük hello gyorsítótár használata.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a>Adja hozzá és hello gyorsítótárból objektumok beolvasása
Elemek tárolja, és a gyorsítótár lekért hello segítségével `StringSet` és `StringGet` módszerek.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis Redis karakterláncok, de ezek a karakterláncok legtöbb adatok is szerepelhetnek számos különböző típusú adatok, beleértve a szerializált bináris adatait, amely is használható, ha .NET tárolása objektumokat hello gyorsítótárban tárolja.

Meghívásakor `StringGet`, ha a hello objektum létezik, akkor adja vissza, és ha nem létezik, `null` adja vissza. Ha `null` lett visszaadva, akkor hello kívánt adatforrás lekérje hello értéket, és a későbbi használatra hello gyorsítótárban tárolja. A használati mód hello gyorsítótár-tartalékoljon mintát néven ismert.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Is `RedisValue`, ahogy az alábbi példa hello. A `RedisValue` implicit operátorokkal rendelkezik az egész szám típusú adatokkal történő munkavégzéshez, ami akkor lehet hasznos, ha a `null` egy gyorsítótárazott elem várt értéke.


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


egy elem gyorsítótárában hello használata hello toospecify hello lejáratának `TimeSpan` paramétere `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a>.NET-objektumok hello gyorsítótárban
Az Azure Redis Cache .NET-objektumokat és primitív adattípusokat is képes gyorsítótárazni, de a .NET-objektumokat a gyorsítótárazásuk előtt szerializálni kell. A .NET-objektum szerializálás hello alkalmazásfejlesztő hello felelőssége, és hello fejlesztői rugalmasságot biztosít a hello szerializáló hello választott.

Egy egyszerű módon tooserialize objektumokat toouse hello `JsonConvert` szerializálási metódusokon [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) és tooand JSON formátumból szerializálni. hello következő példa bemutatja a get és set használatával egy `Employee` objektumpéldány.

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte hello alapjait, kövesse a további információk az Azure Redis Cache hivatkozások toolearn.

* Tekintse meg a hello Azure Redis Cache ASP.NET-szolgáltatók.
  * [Az Azure Redis munkamenetállapot-szolgáltatója](cache-aspnet-session-state-provider.md)
  * [Az Azure Redis Cache ASP.NET kimenetigyorsítótár-szolgáltatója](cache-aspnet-output-cache-provider.md)
* [Gyorsítótár-diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) így [figyelő](cache-how-to-monitor.md) hello a gyorsítótár állapotát. Megtekintheti a hello Azure-portálon, és a metrikák is hello [töltse le, és tekintse át](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) őket az Ön által választott hello eszközökkel.
* Tekintse meg a hello [StackExchange.Redis gyorsítótár ügyfél dokumentáció][StackExchange.Redis cache client documentation].
  * Az Azure Redis Cache számos Redis-ügyfélből és fejlesztési nyelvből elérhető. További információ: [http://redis.io/clients][http://redis.io/clients].
* Az Azure Redis Cache olyan harmadik féltől származó szolgáltatásokkal és eszközökkel is használható, mint a Redsmin vagy a Redis Destkop Manager.
  * Redsmin kapcsolatos további információkért lásd: [hogyan tooretrieve az Azure Redis kapcsolódási karakterláncot, és használatához a Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].
  * Az Azure Redis Cache-ben tárolt adatait a [RedisDesktopManagert](https://github.com/uglide/RedisDesktopManager) használó grafikus felhasználó felületen érheti el és vizsgálhatja meg.
* Lásd: hello [redis] [ redis] dokumentációk és információk a [adattípusok redis] [ redis data types] és [egy 15 perces bemutatása tooRedis adattípusok][a fifteen minute introduction tooRedis data types].

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


