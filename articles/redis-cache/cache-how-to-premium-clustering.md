---
title: "aaaHow tooconfigure a fürtszolgáltatás egy prémium szintű Azure Redis Cache Redis |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a prémium csomagban fürtszolgáltatás Azure Redis Cache példány Redis kezelése"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 44d520facb9d1af145b69f1b58f082aabb655d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-redis-clustering-for-a-premium-azure-redis-cache"></a>Hogyan tooconfigure fürtözése a Premium Azure Redis Cache Redis-e
Azure Redis Cache rendelkezik másik gyorsítótármappa ajánlatokat, amelyek hello választott gyorsítótár mérete és a funkciót, beleértve a prémium réteg szolgáltatások, például a fürtszolgáltatás, az adatmegőrzésre és a virtuális hálózat támogatásának rugalmasságot biztosítanak. Ez a cikk ismerteti, hogyan tooconfigure fürtszolgáltatás egy prémium szintű Azure Redis Cache-gyorsítótár példány.

Más prémium gyorsítótár funkciókról további információért lásd: [bemutatása toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Mi az a fürt Redis?
Azure Redis Cache Redis-fürt kínál [Redis megvalósított](http://redis.io/topics/cluster-tutorial). Redis-fürthöz töltse le a következő előnyöket hello: 

* hello képességét tooautomatically ossza fel a dataset több csomópont között. 
* hello képességét toocontinue részhalmazát hello csomópontok hibákat tapasztal vagy megszerzésekor hello fürt többi hello nem toocommunicate. 
* További átviteli sebesség: átviteli lineárisan növeli a szilánkok hello számának növelésével. 
* További memória mérete: a szilánkok hello értéknek a növelésével lineárisan növekszik.  

Méret, átviteli sebesség és a prémium szintű gyorsítótárak sávszélesség kapcsolatos további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

Az Azure Redis-fürt érhető el egy elsődleges vagy replika modell, ahol minden shard rendelkezik egy elsődleges vagy replika pár replikációs ahol hello replikációs Azure Redis Cache szolgáltatás kezeli. 

## <a name="clustering"></a>Fürtszolgáltatás
Fürtszolgáltatás engedélyezve van a hello **új Redis Cache** panel gyorsítótár létrehozása során. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Fürtszolgáltatás konfigurálja a hello **Redis fürt** panelen.

![Fürtszolgáltatás][redis-cache-clustering]

Hello fürt too10 szilánkok fel lehet. Kattintson a **engedélyezve** és húzza a csúszkát hello, vagy írjon be egy számot 1 és 10 közötti **Shard száma** kattintson **OK**.

Minden shard egy elsődleges vagy replika gyorsítótár pár Azure kezeli, és hello hello gyorsítótár teljes mérete szorzata szilánkok hello száma által kiválasztott tarifacsomag hello hello gyorsítótárméret. 

![Fürtszolgáltatás][redis-cache-clustering-selected]

Hello gyorsítótár létrehozása után tooit csatlakoztatja, és használja az ebben az esetben, például egy nem fürtözött gyorsítótárat, és Redis osztja el a hello gyorsítótár szilánkok hello adatait. Ha diagnosztikai [engedélyezett](cache-how-to-monitor.md#enable-cache-diagnostics), metrikák külön-külön az egyes shard rögzíti és lehet [tekinthetők](cache-how-to-monitor.md) a hello Redis gyorsítótár paneljét. 

> [!NOTE]
> 
> Van néhány kisebb különbség az kötelező az ügyfél-alkalmazásban, ha a fürtszolgáltatás van beállítva. További információkért lásd: [van toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Mintakód hello StackExchange.Redis ügyfél fürtszolgáltatásra kezelését, lásd: hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) hello része [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta.

<a name="cluster-size"></a>

## <a name="change-hello-cluster-size-on-a-running-premium-cache"></a>A futó hello fürt méretének módosítása prémium gyorsítótár
a futó prémium toochange hello fürtméret fürtözési engedélyezve van, kattintson a gyorsítótár **fürtméret Redis** hello a **erőforrás menü**.

> [!NOTE]
> Hello Azure Redis Cache prémium szint lett, amely a rendelkezésre állási tooGeneral, amíg hello fürtméret Redis funkció jelenleg előzetes verzió.
> 
> 

![Redis foglalásiegység-méret][redis-cache-redis-cluster-size]

toochange hello fürtméret hello csúszkával, vagy írjon be egy számot 1 és 10 közötti hello **Shard száma** szöveges értéket, majd kattintson **OK** toosave.

> [!NOTE]
> Skálázás egy fürtben fut hello [ÁTTELEPÍTÉSE](https://redis.io/commands/migrate) parancs, amely egy drága parancs, ezért a gyakorolt minimális hatás mellett, fontolja meg a művelet során nem csúcsidőre futtatása. Hello áttelepítési folyamat során látni fogja a csúcs az igények kiszolgáló terhelését. A fürt skálázás folyamat fut egy hosszú, és hello szükséges idő mértékét kulcsok hello száma és mérete ezeknek a kulcsoknak társított hello értékek.
> 
> 

## <a name="clustering-faq"></a>Fürtszolgáltatás – gyakori kérdések
hello alábbi lista válaszok toocommonly Azure Redis Cache fürtszolgáltatással kapcsolatos kérdések.

* [Szükséges toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Hogyan el a fürt kulcsok?](#how-are-keys-distributed-in-a-cluster)
* [Mi az az hello a létrehozható legnagyobb gyorsítótár mérete?](#what-is-the-largest-cache-size-i-can-create)
* [Minden Redis-ügyfelek támogatják fürtszolgáltatás?](#do-all-redis-clients-support-clustering)
* [Csatlakozás toomy gyorsítótár Ha engedélyezve van a fürtszolgáltatás?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [I közvetlenül kapcsolódhatnak toohello egyedi szilánkok a gyorsítótár?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Beállítható, egy korábban létrehozott gyorsítótár fürtszolgáltatás?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Beállítható, alapszintű vagy standard gyorsítótár fürtszolgáltatás?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Használható hello Redis az ASP.NET munkamenet-állapot és a kimeneti gyorsítótár-szolgáltatók fürtszolgáltatásra?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Azért kapom áthelyezés kivételek, StackExchange.Redis használata esetén, és a fürtszolgáltatás, mi a teendő?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-toomake-any-changes-toomy-client-application-toouse-clustering"></a>Szükséges toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?
* Amikor a fürtszolgáltatás engedélyezve van, csak 0 adatbázis érhető el. Ha több adatbázist használ az ügyfélalkalmazás, és megkísérli tooread vagy írási tooa adatbázis 0 kivételével, hello következő kivétel történt. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch toodatabase: 6`
  
  További információkért lásd: [Redis betűcsoport-specifikációjában - megvalósított részhalmaza](http://redis.io/topics/cluster-spec#implemented-subset).
* Ha használ [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), 1.0.481 kell használnia, vagy később. Csatlakozás toohello gyorsítótár használatával hello azonos [végpontok, portokat és a kulcsok](cache-configure.md#properties) , amelyeket tooa gyorsítótár, amely nem rendelkezik engedélyezett fürtszolgáltatás kapcsolódáskor használ. hello egyetlen különbség, hogy az összes olvasási és írási elvégzendő toodatabase 0.
  
  * Más ügyfelek követelményei lehetnek. Lásd: [Redis-ügyfelek támogatják fürtszolgáltatás?](#do-all-redis-clients-support-clustering)
* Ha az alkalmazás be egy parancs kötegelni több kulcsú műveletek, az összes kulcs kell elhelyezni, hello ugyanazt a shard. toolocate kulcsok a hello ugyanazt a shard, lásd: [kulcsok terjesztését ismertető fürtben?](#how-are-keys-distributed-in-a-cluster)
* A Redis ASP.NET munkamenetállapot-szolgáltató használata használnia kell a 2.0.1 vagy újabb verzióját. Lásd: [használható a fürtszolgáltatás hello Redis az ASP.NET munkamenet-állapot és a kimeneti gyorsítótár szolgáltatókkal?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Hogyan el a fürt kulcsok?
Egy hello Redis [kulcsok telepítési modell](http://redis.io/topics/cluster-spec#keys-distribution-model) dokumentáció: hello kulcsfontosságú terület oszlik 16384 üzembe helyezési ponti. Minden kulcs kivonat készül, és ezek a tárolóhelyek a hello hello fürt csomópontjai között elosztott tooone hozzárendelni. Beállíthatja, hogy mely hello kulcs része, amely több kulcs található kivonatolt tooensure hello ugyanazt a shard kivonatoló címkék használatával.

* Ha valamely része szerint hello kulcs szimpla címkével ellátott kivonat - kulcsok `{` és `}`, hello kulcs csak az része kivonatolja a hello hello kivonatoló tárolóhely kulcs meghatározása céljából. Például a következő 3 kulcsok hello volna található hello ugyanazt a shard: `{key}1`, `{key}2`, és `{key}3` óta csak hello `key` hello név egy részének kivonat készül. Kulcsok kivonatoló címke specifikációk teljes listáját lásd: [kulcsok kivonat-címkék](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Kulcsok nélkül egy ujjlenyomat - hello teljes kulcsnév kivonatoláshoz használt. Az eredmény egy még akkor is, statisztikailag terjesztési hello szilánkok hello gyorsítótár között.

A legjobb teljesítmény és a teljesítményt javasoljuk, hello kulcsok egyenletes elosztása. Kivonatoló címkével ellátott használ a kulcsok esetén hello alkalmazás felelősségi tooensure hello kulcsok vannak elosztva.

További információkért lásd: [kulcsok telepítési modell](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis fürt adatok horizontális](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), és [kulcsok kivonat-címkék](http://redis.io/topics/cluster-spec#keys-hash-tags).

A fürtszolgáltatás és a kulcsok keresése az hello használata példakód ugyanazt a shard hello StackExchange.Redis ügyféllel lásd: hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) hello része [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta.

### <a name="what-is-hello-largest-cache-size-i-can-create"></a>Mi az az hello a létrehozható legnagyobb gyorsítótár mérete?
hello legnagyobb prémium gyorsítótár mérete 53 GB. Másolatot felkínálva 530 GB maximális mérettel too10 szilánkok hozhat létre. Ha nagyobb méretű képes [kérelem több](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). További információkért lásd: [Azure Redis Cache szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Minden Redis-ügyfelek támogatják fürtszolgáltatás?
Hello: jelenleg nem minden ügyfél támogatja a Redis fürtszolgáltatás. StackExchange.Redis egyike, amelyek támogatják azt. A más ügyfelekkel további információkért lásd: hello [hello fürt játszik](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) hello szakasza [Redis-fürt oktatóanyag](http://redis.io/topics/cluster-tutorial).

> [!NOTE]
> Ha az ügyfél StackExchange.Redis használ, győződjön meg arról, hello legújabb verzióját használja [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 vagy újabb verzióját, a fürtszolgáltatás megfelelően toowork. Ha problémák merülnek fel a move-kivételek, olvassa el [helyezze át a kivételek](#move-exceptions) további információt.
> 
> 

### <a name="how-do-i-connect-toomy-cache-when-clustering-is-enabled"></a>Csatlakozás toomy gyorsítótár Ha engedélyezve van a fürtszolgáltatás?
Csatlakoztathatja tooyour gyorsítótár használatával hello azonos [végpontok](cache-configure.md#properties), [portok](cache-configure.md#properties), és [kulcsok](cache-configure.md#access-keys) , amelyeket tooa gyorsítótár, amely nem rendelkezik engedélyezett fürtszolgáltatás kapcsolódáskor használ. A redis kezeli a fürtszolgáltatás hello háttérkiszolgálón, így nem kell toomanage hello azt az ügyfél.

### <a name="can-i-directly-connect-toohello-individual-shards-of-my-cache"></a>I közvetlenül kapcsolódhatnak toohello egyedi szilánkok a gyorsítótár?
Ez hivatalosan nem támogatott. Az említett, minden egyes shard elsődleges vagy replika gyorsítótár párból áll, a gyorsítótárpéldány összefoglaló néven. Toothese gyorsítótár példányok hello hello redis-cli segédprogram használatával csatlakoztathatja [instabil](http://redis.io/download) fiókirodai hello Redis tárház a github webhelyen. Ebben a verzióban megvalósítja az alapszintű támogatás indításakor a hello `-c` váltani. További információ: [hello fürt játszik](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) a [http://redis.io](http://redis.io) a hello [Redis-fürt oktatóanyag](http://redis.io/topics/cluster-tutorial).

A nem ssl használja a következő parancsok hello.

    Redis-cli.exe –h <<cachename>> -p 13000 (tooconnect tooinstance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (tooconnect tooinstance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (tooconnect tooinstance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (tooconnect tooinstance N)

Az SSL-hez, cserélje le a `1300N` rendelkező `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Beállítható, egy korábban létrehozott gyorsítótár fürtszolgáltatás?
Jelenleg csak engedélyezheti a fürtszolgáltatás a gyorsítótár létrehozásakor. Hello fürtméret után hello gyorsítótár jön létre, de nem adja hozzá a csoportosítási tooa prémium gyorsítótár, vagy távolítsa el a fürtszolgáltatás prémium gyorsítótárában hello gyorsítótár létrehozása után módosíthatja. Fürtszolgáltatás engedélyezve van, és csak egy shard prémium gyorsítótár eltér a prémium szintű gyorsítótár azonos méretezés nélküli fürtözési hello.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Beállítható, alapszintű vagy standard gyorsítótár fürtszolgáltatás?
Fürtszolgáltatás csak érhető el prémium gyorsítótárak esetében.

### <a name="can-i-use-clustering-with-hello-redis-aspnet-session-state-and-output-caching-providers"></a>Használható hello Redis az ASP.NET munkamenet-állapot és a kimeneti gyorsítótár-szolgáltatók fürtszolgáltatásra?
* **Kimeneti gyorsítótár-szolgáltató redis** -nem igényelt módosításokat.
* **Redis munkamenetállapot-szolgáltató** -fürtszolgáltatás, toouse kell használnia [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 vagy magasabb vagy kivétel lépett fel. Ez az használhatatlanná tévő változást; További információ: [v2.0.0 Megtörje Változásrészletek](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Azért kapom áthelyezés kivételek, StackExchange.Redis használata esetén, és a fürtszolgáltatás, mi a teendő?
Ha a StackExchange.Redis használ, és fogadni `MOVE` kivételek fürtözés használata esetén győződjön meg arról, hogy használja [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) vagy újabb. A .NET-alkalmazások toouse StackExchange.Redis konfigurálása, lásd: [hello gyorsítótár-ügyfelek konfigurálása](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Következő lépések
Ismerje meg, hogyan több premium toouse gyorsítótár szolgáltatásokat.

* [Bevezetés toohello Azure Redis Cache prémium szintjének](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







