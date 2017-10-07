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
# <a name="how-tooscale-azure-redis-cache"></a>Hogyan tooScale Azure Redis Cache-gyorsítótár
Azure Redis Cache rendelkezik másik gyorsítótármappa ajánlatokat, amelyek hello választott gyorsítótár mérete és a szolgáltatások rugalmasságot biztosítanak. A gyorsítótár létrehozása után méretezheti hello méretét és hello hello gyorsítótár tarifacsomag, ha az alkalmazás hello követelményei megváltoznak. Ez a cikk bemutatja, hogyan tooscale hello Azure-portál és a használatához a gyorsítótár eszközök, például az Azure PowerShell és az Azure parancssori felület.

## <a name="when-tooscale"></a>Ha tooscale
Használhatja a hello [figyelési](cache-how-to-monitor.md) Azure Redis Cache toomonitor szolgáltatásai állapotának és teljesítményének a gyorsítótár hello és meghatározásához, hogy mikor tooscale hello gyorsítótár. 

Figyelheti a hello következő metrikák toohelp határozza meg, ha tooscale szükséges.

* A kiszolgálóterhelés redis
* Memóriahasználat
* Hálózati sávszélesség
* CPU-használat

Ha azt állapítja meg, hogy a gyorsítótár már nem teljesíti-e az alkalmazás követelményeinek, IP-címek számára az alkalmazás megfelelő tooa kisebb vagy nagyobb gyorsítótár méretezheti. Annak meghatározására, amely gyorsítótár árképzési szint toouse további információkért lásd: [milyen Redis Cache-ajánlatot és méretet használjam](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>A gyorsítótár méretezése
tooscale a gyorsítótár [toohello gyorsítótár Tallózás](cache-configure.md#configure-redis-cache-settings) a hello [Azure-portálon](https://portal.azure.com) kattintson **méretezési** a hello **erőforrás menü**.

![Méretezés](./media/cache-how-to-scale/redis-cache-scale-menu.png)

SELECT hello szükséges hello a tarifacsomag **válassza ki az IP-címek** panel megnyitásához, és kattintson **válasszon**.

![Tarifacsomag][redis-cache-pricing-tier-blade]


Másik tarifacsomagra vált a következő korlátozások hello tooa méretezheti:

* A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.
  * Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.
  * Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.
* A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe. Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.
* Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár. Kell méretezni a **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** a későbbi skálázás a műveletet.
* Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.
 
Hello gyorsítótár méretezése folyik toohello új IP-címek, miközben egy **méretezés** állapota megjelenik hello **Redis Cache** panelen.

![Méretezés][redis-cache-scaling]

Skálázás befejeződése után hello állapota a **méretezés** túl**futtató**.

## <a name="how-tooautomate-a-scaling-operation"></a>Hogyan tooautomate egy skálázási műveletet
A hozzáadása tooscaling a gyorsítótár-példány hello Azure-portálon, PowerShell-parancsmagokkal, az Azure parancssori felület, méretezhető, és hello segítségével a Microsoft Azure felügyeleti függvénytárak (MAML). 

* [A skála PowerShell használatával](#scale-using-powershell)
* [Scale Azure parancssori felület használatával](#scale-using-azure-cli)
* [A skála MAML használatával](#scale-using-maml)

### <a name="scale-using-powershell"></a>A skála PowerShell használatával
A PowerShell használatával az Azure Redis Cache példány méretezheti hello segítségével [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmagot, ha hello `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be. hello következő példa bemutatja, hogyan tooscale gyorsítótár nevű `myCache` tooa 2,5 GB gyorsítótár. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

A skálázás a PowerShell használatával további információkért lásd: [tooscale egy Redis gyorsítótár Powershell-lel](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Scale Azure parancssori felület használatával
tooscale az Azure parancssori felület használatával Azure Redis Cache példány hívás hello `azure rediscache set` parancs és pass hello szükséges konfigurációs módosítások, amely egy új méretét, a termékváltozat, illetve a fürt méretét, attól függően, hogy hello skálázási művelet szükséges.

Az Azure parancssori felülettel skálázás további információkért lásd: [egy meglévő Redis gyorsítótár beállításainak módosítása](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>A skála MAML használatával
tooscale a hello használó Azure Redis Cache-példányokat [Microsoft Azure felügyeleti függvénytárak (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), hívás hello `IRedisOperations.CreateOrUpdate` metódus, és új méretét hello hello adjon át `RedisProperties.SKU.Capacity`.

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

További információkért lásd: hello [kezelése Redis Cache segítségével MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) minta.

## <a name="scaling-faq"></a>Méretezési – gyakori kérdések
hello alábbi lista válaszok toocommonly Azure Redis Cache méretezésével kapcsolatos kérdések.

* [A, vagy a prémium szintű gyorsítótár is méretezhető?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Skálázás, után kell toochange a gyorsítótár neve vagy a hozzáférési kulcsainak?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Skálázás működése](#how-does-scaling-work)
* [I adat elvész a gyorsítótárból skálázás során?](#will-i-lose-data-from-my-cache-during-scaling)
* [Az egyéni adatbázisok állít érintett skálázás során?](#is-my-custom-databases-setting-affected-during-scaling)
* [A gyorsítótár elérhető lesz skálázás során?](#will-my-cache-be-available-during-scaling)
* [Nem támogatott műveletek](#operations-that-are-not-supported)
* [Mennyi időt skálázás igénybe?](#how-long-does-scaling-take)
* [Hogyan állapítható meg, hogy ha skálázás befejeződött?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>A, vagy a prémium szintű gyorsítótár is méretezhető?
* Nem lehet méretezni a egy **prémium** gyorsítótár tooa le **alapvető** vagy **szabványos** tarifacsomagra vált.
* Az egyik méretezheti **prémium** árképzési szint tooanother gyorsítótár.
* Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár. Először át kell méretezni **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** egy későbbi skálázási művelet.
* Ha engedélyezte a fürtszolgáltatás létrehozásakor a **prémium** gyorsítótárában, akkor [hello fürt méretének módosítása](cache-how-to-premium-clustering.md#cluster-size). Ha a gyorsítótár fürtszolgáltatás engedélyezése nélkül hozták létre, akkor nem konfigurálhatja a fürtözést egy későbbi időpontban.
  
  További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a>Skálázás, után kell toochange a gyorsítótár neve vagy a hozzáférési kulcsainak?
Nem, a gyorsítótár neve, valamint a kulcsok nem változik a méretezési művelet során.

### <a name="how-does-scaling-work"></a>Skálázás működése
* Ha egy **alapvető** gyorsítótár méretezett tooa különböző méretű le van állítva, és új gyorsítótárat regisztráltak hello új méret használatával. Ebben az időszakban hello gyorsítótár nem érhető el, és a hello gyorsítótárban minden adat elveszik.
* Ha egy **alapvető** gyorsítótár méretezett tooa **szabványos** gyorsítótár, a replika gyorsítótár ki van építve, és hello elsődleges gyorsítótár toohello replika gyorsítótár hello adatok másolásakor. hello gyorsítótár hello folyamat méretezés közben elérhető marad.
* Ha egy **szabványos** gyorsítótár méretezett tooa különböző méretű vagy tooa **prémium** gyorsítótár, egyik hello replikák le van állítva, és újra üzembe a toohello új méretének és hello átvitt adatok keresztül, és más hello a replika hajt végre, a feladatátvétel előtt újra ki van építve, hasonló toohello folyamat során hello gyorsítótár-csomópontok közül az egyik hiba fordul elő.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>I adat elvész a gyorsítótárból skálázás során?
* Ha egy **alapvető** gyorsítótár méretezett tooa új méret, minden adat elveszik és hello gyorsítótár nem érhető el hello skálázás művelet során.
* Ha egy **alapvető** gyorsítótár méretezett tooa **szabványos** gyorsítótárazza, hello hello gyorsítótárban lévő adatok általában megőrződik.
* Ha egy **szabványos** gyorsítótár méretezett tooa nagyobb méretű vagy a réteg, vagy egy **prémium** gyorsítótár méretezett tooa nagyobb méretű, minden adatot általában megőrződik. Amikor skálázás egy **szabványos** vagy **prémium** attól függően, hogy mennyi adatot hello gyorsítótárban gyorsítótár le tooa kisebb méretet, adatok elveszhetnek azt méretezett kapcsolatos toohello új méretét. Adat elvész, amikor a skálázás, ha a kulcsok kizárt vannak-e hello segítségével [allkeys-lru](http://redis.io/topics/lru-cache) kiürítés házirend. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Az egyéni adatbázisok állít érintett skálázás során?
Különböző tartalmaznak az egyes tarifacsomagok [korlátok adatbázisok](cache-configure.md#databases), úgy, hogy nincs szempontokat konfigurálásakor méretet Ha hello egyéni értéket `databases` beállítása a gyorsítótár létrehozása közben.

* Ha skálázás tarifacsomag az alsó tooa `databases` hello jelenlegi rétegtől határa:
  * Hello alapértelmezett számának használata `databases` ez 16 az összes árképzési szinteket, adatok nem vesztek el.
  * Egyéni számos használata `databases` hello réteg toowhich van folyamatban, tartozik, amely hello határértékek ez `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.
  * Egyéni számos használata `databases` , amely meghaladja hello új réteget, hello korlátai által megszabott hello `databases` beállítás süllyesztett toohello korlátok hello új réteg és hello eltávolított adatbázisok az összes adat elvész.
* Ha skálázás tarifacsomag hello az azonos vagy újabb tooa `databases` hello jelenlegi rétegtől határa a `databases` beállítás akkor is megmarad, és az adatok nem vesztek el.

Vegye figyelembe, hogy míg a Standard és Premium gyorsítótárak egy 99,9 %-os SLA-t a rendelkezésre állás érdekében, az adatvesztés nélküli SLA.

### <a name="will-my-cache-be-available-during-scaling"></a>A gyorsítótár elérhető lesz skálázás során?
* **Standard** és **prémium** gyorsítótárak elérhetők maradnak hello skálázás művelet során.
* **Alapszintű** gyorsítótárak műveletek tooa különböző méretű skálázás során offline módban, de elérhetők maradnak, ha a méretezés **alapvető** túl**szabványos**.

### <a name="operations-that-are-not-supported"></a>Nem támogatott műveletek
* A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.
  * Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.
  * Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.
* A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe. Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.
* Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár. Először át kell méretezni **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** egy későbbi skálázási művelet.
* Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.

A méretezési művelet sikertelen lesz, ha hello szolgáltatás megpróbál toorevert hello műveletet, és hello gyorsítótár visszaáll az eredeti méret toohello.

### <a name="how-long-does-scaling-take"></a>Mennyi időt skálázás igénybe?
Skálázás vesz körülbelül 20 percet, attól függően, hogy mennyi adatot hello gyorsítótárában.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Hogyan állapítható meg, hogy ha skálázás befejeződött?
Hello Azure-portálon a folyamatban lévő művelet skálázás hello látható. Skálázás befejeződése után hello hello gyorsítótár módosítások állapotának túl**futtató**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



