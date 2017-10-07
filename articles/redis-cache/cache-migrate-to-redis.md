---
title: "Managed Cache Service-alkalmazások tooRedis - Azure aaaMigrate |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate Managed Cache Service és a szerepköralapú gyorsítótár alkalmazások tooAzure Redis gyorsítótár"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: sdanie
ms.openlocfilehash: bd81722820acf0d2637828fbb6100c723aafeba5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-managed-cache-service-tooazure-redis-cache"></a>Managed Cache Service tooAzure Redis Cache áttelepítése
Az Azure Managed Cache Service tooAzure Redis Cache használó alkalmazások áttelepítése alkalmazással minimális módosításait tooyour, attól függően, hogy a gyorsítótár alkalmazás által használt hello Managed Cache Service-szolgáltatások is elvégezhető. Míg hello API-k nem pontosan hello azonos hasonló, és nagy részét a meglévő kódot Managed Cache Service tooaccess gyorsítótárat használó újrahasználhatók minimális módosításait. Ez a témakör bemutatja, hogyan toomake hello szükséges konfigurációs és alkalmazás változik toomigrate a Managed Cache Service-alkalmazások toouse Azure Redis Cache, és bemutatja, hogyan lehet néhány hello szolgáltatást az Azure Redis Cache használt tooimplement hello funkció a Managed Cache Service gyorsítótár.

>[!NOTE]
>Felügyelt Cache Service és a szerepköralapú gyorsítótár [kivont](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) November 30 2016. Ha a megjeleníteni kívánt toomigrate tooAzure Redis Cache-hoz tartozó szerepköralapú gyorsítótár telepítések, lépésekkel hello ebben a cikkben.

## <a name="migration-steps"></a>Áttelepítés lépései
a lépéseket követve hello szükséges toomigrate egy Managed Cache Service-alkalmazás toouse Azure Redis Cache.

* Managed Cache Service-szolgáltatások tooAzure Redis Cache leképezése
* Válassza ki a Cache-ajánlatot
* A gyorsítótár létrehozása
* Hello gyorsítótár-ügyfelek konfigurálása
  * Távolítsa el a hello kezelt gyorsítótár szolgáltatás konfigurációja
  * Hello StackExchange.Redis NuGet-csomag használatával gyorsítótár-ügyfél konfigurálása
* Telepítse át a Managed Cache Service kódot
  * Csatlakozás toohello gyorsítótár hello ConnectionMultiplexer osztály használatával
  * Hozzáférés primitív adattípusokat hello gyorsítótárban
  * .NET-objektumok hello gyorsítótárban
* Az ASP.NET munkamenet-állapot és a kimeneti tooAzure Redis Cache-gyorsítótár áttelepítése 

## <a name="map-managed-cache-service-features-tooazure-redis-cache"></a>Managed Cache Service-szolgáltatások tooAzure Redis Cache leképezése
Az Azure Managed Cache Service és az Azure Redis Cache hasonló, de egyes szolgáltatásaik különböző módon végrehajtása. Ez a szakasz néhány hello különbségeket mutatja be, és útmutatást nyújt az Azure Redis Cache Managed Cache Service hello funkcióit végrehajtási.

| Felügyelt Cache Service szolgáltatás | Felügyelt gyorsítótár szolgáltatás támogatása | Azure Redis Cache-támogatás |
| --- | --- | --- |
| Elnevezett gyorsítótárak |Egy alapértelmezett gyorsítótár van beállítva, és a hello Standard és Premium gyorsítótár ajánlatokat, fel további toonine gyorsítótárak nevű beállítható úgy, ha szükséges. |Azure Redis gyorsítótár-adatbázisok (16-os alapértelmezett), amely egy hasonló funkciókat toonamed gyorsítótárazza a használt tooimplement konfigurálható több van. További információ: [Mik azok a Redis-adatbázisok?](cache-faq.md#what-are-redis-databases) és [A Redis-kiszolgáló alapértelmezett konfigurációja](cache-configure.md#default-redis-server-configuration). |
| Magas rendelkezésre állás |Magas rendelkezésre állást biztosít a Standard és Premium hello gyorsítótár ajánlatok hello gyorsítótár elemeinek. Ha a cikkeket tooa hiba miatt megszakadt, hello gyorsítótár hello elemek biztonsági másolatok továbbra is elérhetők. Írja a toohello másodlagos gyorsítótár szinkron módon történik. |Magas rendelkezésre állású hello Standard és Premium gyorsítótár ajánlatok, amely két csomópont elsődleges vagy replika konfigurációt (a Premium-gyorsítótár egyes shard rendelkezik egy elsődleges vagy replika pár) érhető el. Írás toohello replika aszinkron módon történik. További információkért lásd: [Azure Redis Cache árképzési](https://azure.microsoft.com/pricing/details/cache/). |
| Értesítések |Lehetővé teszi, hogy az ügyfelek tooreceive aszinkron értesítések küldése, ha a gyorsítótár-műveletekhez különböző fordulhat elő, egy elnevezett gyorsítótár. |Ügyfélalkalmazások használhatja a Redis pub/sub vagy [kulcstérértesítések használatával értesítések](cache-configure.md#keyspace-notifications-advanced-settings) tooachieve egy hasonló funkciókat toonotifications. |
| Helyi gyorsítótár |Hello ügyfél extra gyors hozzáférés a helyileg tárolja a gyorsítótárazott objektum egy példányát. |Ügyfélalkalmazások tooimplement kellene ezt a funkciót a szótár vagy hasonló adatstruktúra. |
| A kiürítési házirend |Nincs vagy LRU. hello alapértelmezett házirend LRU. |Azure Redis Cache támogatja a következő kiürítési házirendek hello: ideiglenes-lru, allkeys-lru, ideiglenes véletlenszerű, allkeys-véletlen ideiglenes ttl, noeviction. hello alapértelmezett házirend ideiglenes-lru. További információkért lásd: [kiszolgálókonfiguráció alapértelmezett Redis](cache-configure.md#default-redis-server-configuration). |
| Elévülési szabályzatának |hello alapértelmezett elévülési szabályzatának abszolút és hello alapértelmezett lejárati időköz tíz perc. A késleltetett és Never házirendek is elérhetők. |Alapértelmezés szerint hello gyorsítótárban elemek nem jár le, de egy lejárati gyorsítótár set túlterhelés használatával / írási szinten konfigurálható. További információkért lásd: [hozzáadása és lekérése objektumok hello gyorsítótárból](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache). |
| Régiók és címkézés |Régiók a következők elemek gyorsítótárazott alcsoportok. Régiók is támogatják a gyorsítótárazott elemek hello jegyzet további leíró karakterláncok címkéket. Régiók hello képességét tooperform keresési műveletek címkézett elemek támogatja az adott régióban. Régión belül minden elem hello gyorsítótár fürt egyetlen csomópont belül találhatók. |A Redis gyorsítótár áll egyetlen csomópont (kivéve, ha engedélyezve van a Redis-fürt), Managed Cache Service régiók hello fogalma nem érvényes. Redis támogatja a keresést és a helyettesítő művelet kulcsok beolvasása közben, ezért leíró címkék hello kulcsnevek belül lehet beágyazni, és később használt tooretrieve hello elemek. Egy Redis használó címkézési megoldás végrehajtási példáért lásd: [címkézés – Redis gyorsítótár végrehajtási](http://stackify.com/implementing-cache-tagging-redis/). |
| Szerializálási |Felügyelt gyorsítótár NetDataContractSerializer, BinaryFormatter és egyéni objektumszerializáló hello használatát támogatja. hello alapértelmezett érték a NetDataContractSerializer. |Feladata hello hello ügyfél tooserialize .NET alkalmazásobjektumok előtt hello szerializáló mentése toohello ügyfél alkalmazásfejlesztő hello választható hello gyorsítótárba helyezi őket. További információk és mintakód [.NET objektumok hello gyorsítótárában](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Gyorsítótár-emulátor |Felügyelt gyorsítótár biztosít egy helyi gyorsítótár emulátor. |Azure Redis Cache-gyorsítótár nem rendelkezik az emulátor, azonban úgy is [futtassa helyileg a redis-server.exe hello MSOpenTech build](cache-faq.md#cache-emulator) tooprovide emulátor élményt. |

## <a name="choose-a-cache-offering"></a>Válassza ki a Cache-ajánlatot
A Microsoft Azure Redis Cache rétegek a következő hello érhető el:

* **Alapszintű** – Egyetlen csomópont. Több méretek too53 GB fel.
* **Standard** – Két csomópontból álló elsődleges/replika. Több méretek too53 GB fel. 99,9%-os SLA.
* **Prémium szintű** – két csomópontos elsődleges vagy replika too10 szilánkok fel. Több méretek 6 GB too530 GB. Tartalmazza a Standard szint összes szolgáltatását és továbbiakat, beleértve a [Redis-fürtök](cache-how-to-premium-clustering.md), a [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) és az [Azure Virtual Network](cache-how-to-premium-vnet.md) támogatását. 99,9%-os SLA.

Az egyes szintek szolgáltatási feltételei és díjszabása eltérő. hello szolgáltatások tartoznak, és az árakkal kapcsolatos további információ az útmutató későbbi részében, lásd: [gyorsítótár díjszabás](https://azure.microsoft.com/pricing/details/cache/).

Áttelepítési kiindulási pontot, amely megfelel az előző Managed Cache Service gyorsítótár méretének hello toopick hello mérete, és a felfelé vagy lefelé majd méret attól függően, hogy az alkalmazás hello követelményeinek. Hello jobb Azure Redis Cache-ajánlatot kiválasztásáról További útmutatóért lásd: [milyen Redis Cache-ajánlatot és méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>A gyorsítótár létrehozása
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-hello-cache-clients"></a>Hello gyorsítótár-ügyfelek konfigurálása
Hello gyorsítótár létrehozása és konfigurálása után hello tovább tooremove hello Managed Cache Service-konfiguráció, és adja hozzá a hello hello Azure Redis Cache konfigurációs és hivatkozások hozzáadása, úgy, hogy a gyorsítótár-ügyfelek hozzáférhetnek a hello gyorsítótár.

* Távolítsa el a hello kezelt gyorsítótár szolgáltatás konfigurációja
* Hello StackExchange.Redis NuGet-csomag használatával gyorsítótár-ügyfél konfigurálása

### <a name="remove-hello-managed-cache-service-configuration"></a>Távolítsa el a hello kezelt gyorsítótár szolgáltatás konfigurációja
Előtt hello Azure Redis Cache, hello meglévő Managed Cache Service configuration ügyfélalkalmazások konfigurálhatók, és szerelvényhivatkozások hello Managed Cache Service NuGet-csomag eltávolításával el kell távolítani.

toouninstall hello Managed Cache Service NuGet-csomagot, kattintson a jobb gombbal a hello ügyfélprojekt **Megoldáskezelőben** válassza **NuGet-csomagok kezelése**. Jelölje be hello **telepített csomag** csomópont, és írja be W**indowsAzure.Caching** hello történő keresés telepítve csomagok mezőben. Válassza ki **Windows** **Azure Cache** (vagy **Windows** **Azure gyorsítótárazás** hello NuGet csomag hello verziójától függően), kattintson  **Távolítsa el**, és kattintson a **Bezárás**.

![Távolítsa el az Azure Managed Cache Service NuGet-csomag](./media/cache-migrate-to-redis/IC757666.jpg)

Eltávolítását hello Managed Cache Service NuGet csomag hello app.config vagy a web.config hello ügyfélalkalmazás hello Managed Cache Service szerelvények és hello Managed Cache Service bejegyzések távolítja el. Hello NuGet-csomag eltávolításakor néhány testre szabott beállításokat nem lehet eltávolítani, mert nyissa meg a web.config vagy az App.config fájlt, és győződjön meg arról, hogy a következő elemek hello teljesen el vannak távolítva.

Győződjön meg arról, hogy hello `dataCacheClients` bejegyzés törlődik hello `configSections` elemet. Ne távolítsa el az hello teljes `configSections` elem; csak eltávolítása hello `dataCacheClients` bejegyzést, ha telepítve.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Győződjön meg arról, hogy hello `dataCacheClients` szakaszban a rendszer eltávolítja. Hello `dataCacheClients` lesz a szakasz a következő példa hasonló toohello.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--toouse hello in-role flavor of Azure Cache, set identifier toobe hello cache cluster role name -->
    <!--toouse hello Azure Managed Cache Service, set identifier toobe hello endpoint of hello cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section toospecify security settings for connecting tooyour cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Hello Managed Cache Service configuration eltávolítást követően a hello gyorsítótárügyfél hello a következő szakaszban leírtak szerint konfigurálhatja.

### <a name="configure-a-cache-client-using-hello-stackexchangeredis-nuget-package"></a>Hello StackExchange.Redis NuGet-csomag használatával gyorsítótár-ügyfél konfigurálása
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Telepítse át a Managed Cache Service kódot
hello API-t a hello StackExchange.Redis gyorsítótárügyfél hasonló toohello Managed Cache Service. Ez a szakasz áttekintést hello eltéréseket.

### <a name="connect-toohello-cache-using-hello-connectionmultiplexer-class"></a>Csatlakozás toohello gyorsítótár hello ConnectionMultiplexer osztály használatával
A Managed Cache Service kapcsolatok toohello gyorsítótár kezelt hello `DataCacheFactory` és `DataCache` osztályok. Az Azure Redis Cache, ezek a kapcsolatok hello által kezelt `ConnectionMultiplexer` osztály.

Adja hozzá a következő hello minden olyan fájlt, amelyből el kívánja tooaccess hello gyorsítótár toohello tetején utasítás használatával.

```c#
using StackExchange.Redis
```

Ha ez a névtér nem oldja meg, ne feledje hozzáadásának hello StackExchange.Redis NuGet-csomagot a [hello gyorsítótár-ügyfelek konfigurálása](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

> [!NOTE]
> Vegye figyelembe, hogy hello StackExchange.Redis-ügyfél igényli a .NET-keretrendszer 4-es vagy újabb verzióját.
> 
> 

tooconnect tooan Azure Redis Cache példányt, hívás hello statikus `ConnectionMultiplexer.Connect` metódus és hello végpont és a kulcs adjon át. Egyik módszer toosharing egy `ConnectionMultiplexer` toohave csatlakoztatott példány, a következő példa hasonló toohello visszaadó statikus tulajdonság példány az alkalmazásban. Ez biztosítja a szálbiztos módon tooinitialize egyetlen csatlakoztatott `ConnectionMultiplexer` példány. Ebben a példában `abortConnect` van beállítva toofalse, ami azt jelenti, hogy hello hívás sikeres lesz, még akkor is, ha a kapcsolat toohello gyorsítótár nem jön létre. Egy alapfunkciója `ConnectionMultiplexer` , hogy kapcsolatot toohello gyorsítótár automatikusan azt fog épülni, miután hello hálózati hiba vagy más ok fakadó problémák megoldásával.

```c#
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
```

hello gyorsítótár végpontjához, a kulcsok és a portok szerezhető hello **Redis Cache** a gyorsítótárpéldány panelen. További információkért lásd: [Redis Cache-gyorsítótár tulajdonságai](cache-configure.md#properties).

Hello kapcsolat létrejötte után vissza a hivatkozás toohello Redis gyorsítótár adatbázisában: hívó hello `ConnectionMultiplexer.GetDatabase` metódust. hello hello által visszaadott `GetDatabase` metódus egy egyszerűsített csatlakoztatott objektum, és nem kell toobe tárolja.

```c#
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using hello cache object...
// Simple put of integral data types into hello cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from hello cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

hello StackExchange.Redis ügyfélnél hello `RedisKey` és `RedisValue` típusok elérésére, és az elemek tárolására hello gyorsítótárában. Ezek a típusok alakzatot leginkább egyszerű nyelvi típusok, beleértve a karakterláncot, hozzárendelését, gyakran nem használhatók közvetlenül. Redis karakterláncok hello legalapvetőbb Redis érték típusa, és számos különböző típusú adatok, beleértve a szerializált bináris adatfolyamokat, és tartalmazhat hello típus közvetlenül nem használható, amíg Ön tartalmazó módszert használja `String` hello nevében. A legtöbb primitív adattípusokat tároljuk, és az elemek beolvasása a hello gyorsítótárból hello segítségével `StringSet` és `StringGet` módszerek, kivéve, ha tárolja gyűjtemények és más Redis adattípusok hello gyorsítótár. 

`StringSet`és `StringGet` nagyon hasonló toohello Managed Cache Service `Put` és `Get` módszerek, az egyik fő különbség, hogy a hello gyorsítótárba egy .NET-objektum beolvasása és beállítása előtt meg kell (MUST) azt először. 

Meghívásakor `StringGet`, ha hello objektum létezik, akkor adja vissza, és ha nem, null ad vissza. Ebben az esetben hello érték lekérése hello kívánt adatforrás és a későbbi használatra hello gyorsítótárban tárolja. Ez az úgynevezett hello gyorsítótár-tartalékoljon mintát.

egy elem gyorsítótárában hello használata hello toospecify hello lejáratának `TimeSpan` paramétere `StringSet`.

```c#
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis Cache .NET objektumok, valamint a primitív adattípusokat együttműködik, de előtt egy .NET-objektum gyorsítótárazható következően szerializálni kell. Ez a hello alkalmazásfejlesztő hello felelőssége. A hello szerializáló hello választott így hello fejlesztői rugalmasságot. További információk és mintakód [.NET objektumok hello gyorsítótárában](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-tooazure-redis-cache"></a>Az ASP.NET munkamenet-állapot és a kimeneti tooAzure Redis Cache-gyorsítótár áttelepítése
Azure Redis Cache van az ASP.NET munkamenet-állapot és a lap kimeneti gyorsítótár-szolgáltatók. toomigrate az alkalmazás által használt hello Managed Cache Service verziói ezen szolgáltatók esetében először el kell távolítani hello meglévő szakaszokat a Web.config fájlban, és adja meg hello szolgáltatók hello Azure Redis Cache verzióit. Használatával hello Azure Redis Cache ASP.NET-szolgáltatók, lásd: [ASP.NET munkamenetállapot-szolgáltatóját az Azure Redis Cache](cache-aspnet-session-state-provider.md) és [az ASP.NET kimeneti gyorsítótár-szolgáltató Azure Redis Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Következő lépések
Hello felfedezés [Azure Redis Cache dokumentáció](https://azure.microsoft.com/documentation/services/cache/) az oktatóanyagok, példák, videók és több.

