---
title: "aaaAzure Redis gyorsítótár – gyakori kérdések |} Microsoft Docs"
description: "Ismerje meg, hello megválaszolja toocommon kérdéseket, mintákat és ajánlott eljárások Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 2c6ed2f65f755bd08f04857b7af31f520cf4f158
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-redis-cache-faq"></a>Azure Redis Cache – Gyakori kérdések
Ismerje meg, hello megválaszolja toocommon kérdéseket, a mintákat és ajánlott eljárások Azure Redis Cache.

## <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha a fentiekben itt nem választ?
Ha kérdését itt nem látható, ossza meg velünk, és segítünk talált választ.

* Ez a GYIK hello végén hello megjegyzések fel kérdést, és foglalkozó hello Azure Cache csoport és a Közösség más tagjaival kapcsolatos cikkben.
* egy szélesebb körű célközönség tooreach, vannak, beküldheti egy kérdést a hello [Azure Cache MSDN fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) és foglalkozó hello Azure Cache csapat és hello Közösség más tagjai.
* Ha azt szeretné, hogy a szolgáltatás kérése toomake, elküldheti a kérelmek és ötleteket túl[Azure Redis Cache User Voice](https://feedback.azure.com/forums/169382-cache).
* Elküldhetők e-mailt, toous [Azure Cache külső visszajelzés](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Azure Redis Cache alapjai
hello ebben a szakaszban – gyakori kérdések néhány Azure Redis Cache hello alapjait fedik le.

* [Mi az Azure Redis Cache?](#what-is-azure-redis-cache)
* [Hogyan lehet kezdjem el az Azure Redis Cache?](#how-can-i-get-started-with-azure-redis-cache)

hello következő gyakori kérdések főbb fogalmait és kifejezéseit és az Azure Redis Cache kapcsolatos kérdések és válaszok egyik hello más gyakran ismételt kérdések szakaszban.

* [Melyik Redis Cache-ajánlatot és -méretet használjam?](#what-redis-cache-offering-and-size-should-i-use)
* [Melyik Redis gyorsítótár-ügyfelek használható?](#what-redis-cache-clients-can-i-use)
* [Az Azure Redis Cache egy helyi emulátor van?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hogyan figyelhetek hello állapotának és teljesítményének a gyorsítótár?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Tervezési – gyakori kérdések
* [Melyik Redis Cache-ajánlatot és -méretet használjam?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Redis Cache-teljesítmény](#azure-redis-cache-performance)
* [Milyen régióban I, keresse meg a gyorsítótár?](#in-what-region-should-i-locate-my-cache)
* [Hogyan vagyok fizetni az Azure Redis Cache?](#how-am-i-billed-for-azure-redis-cache)
* [Azure Redis Cache használható Azure Government felhő, Azure Kína felhő vagy a Microsoft Azure Németország?](#can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Fejlesztési – gyakori kérdések
* [Mit tegye hello StackExchange.Redis konfigurációs beállítások?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Melyik Redis gyorsítótár-ügyfelek használható?](#what-redis-cache-clients-can-i-use)
* [Az Azure Redis Cache egy helyi emulátor van?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Hogyan futtathatom Redis parancsok?](#how-can-i-run-redis-commands)
* [Miért nem rendelkezik az MSDN osztály kódtár – dokumentáció például néhány más Azure-szolgáltatásokkal hello Azure Redis Cache?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Használhatok Azure Redis Cache PHP munkamenet-gyorsítótár?](#can-i-use-azure-redis-cache-as-a-php-session-cache)
* [Mik azok a Redis-adatbázisok?](#what-are-redis-databases)

## <a name="security-faqs"></a>Biztonság – gyakori kérdések
* [Ha engedélyezze hello nem SSL port tooRedis csatlakozni?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Éles – gyakori kérdések
* [Mik azok a termelési tanácsokat?](#what-are-some-production-best-practices)
* [Melyek azok hello szempontok közös Redis-parancsok használata esetén?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Hogyan elvégez egy teljesítménytesztet és tesztelése a gyorsítótár teljesítményének hello?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Fontos tudnivalók a szálkészlet növekedési](#important-details-about-threadpool-growth)
* [Engedélyezze a további átviteli hello ügyfélen server GC tooget StackExchange.Redis használata esetén](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Kapcsolatok körül teljesítménnyel kapcsolatos szempontok](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>Figyelés és hibaelhárítás – gyakori kérdések
hello – gyakori kérdések a a szakasz tartalma közös megfigyelést és a hibaelhárítási kérdések. Figyelés és hibaelhárítás céljából az Azure Redis Cache példány kapcsolatos további információkért lásd: [hogyan toomonitor Azure Redis Cache-gyorsítótár](cache-how-to-monitor.md) és [hogyan tootroubleshoot Azure Redis Cache-gyorsítótár](cache-how-to-troubleshoot.md).

* [Hogyan figyelhetek hello állapotának és teljesítményének a gyorsítótár?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Miért jelenik meg időtúllépések?](#why-am-i-seeing-timeouts)
* [Az ügyfél miért leválasztották hello gyorsítótár?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Előzetes Cache-ajánlatot – gyakori kérdések
* [Mely Azure Cache-ajánlatot az megfelelő a számomra?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-redis-cache"></a>Mi az az Azure Redis Cache?
Azure Redis Cache alapul hello népszerű nyílt forráskódú [Redis gyorsítótár](http://redis.io). Ez lehetővé teszi az alkalmazásból tooa biztonságos, dedikált Redis gyorsítótár, a Microsoft által felügyelt, és elérhető bármely Azure-ban. Részletesebb áttekintéséért lásd: hello [Azure Redis Cache](https://azure.microsoft.com/services/cache/) az Azure.com-on a termék oldalát.

### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Hogyan lehet kezdjem el az Azure Redis Cache?
Ismerkedés az Azure Redis Cache számos módja van.

* Megtekintheti az elérhető oktatóprogramok valamelyikét [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), és [Python](cache-python-get-started.md).
* Figyelheti az [hogyan tooBuild nagy teljesítményű alkalmazások használata a Microsoft Azure Redis Cache](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* A projekt toosee hogyan toouse Redis hello fejlesztési nyelvének megfelelő hello ügyfelek hello ügyfél dokumentációja megtekintheti. Nincsenek sok használható az Azure Redis Cache Redis-ügyfelek. Egy Redis-ügyfelek listája, [http://redis.io/clients](http://redis.io/clients).

Ha még nem rendelkezik Azure-fiókot, akkor a következőket teheti:

* [Nyisson egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Amelyek lehetnek kimenő használt tootry fizetős Azure-szolgáltatások jóváírásokat kap. Hello jóváírásokat el is használta, után is megtarthatja hello fiókot, és ingyenes Azure-szolgáltatások és funkciók használatára.
* [Aktiválja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Az MSDN-előfizetés minden hónapban biztosít Önnek krediteket, amelyekkel fizetős Azure-szolgáltatásokat használhat.

<a name="cache-size"></a>

### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Melyik Redis Cache-ajánlatot és -méretet használjam?
Minden Azure Redis Cache-ajánlatot biztosít a különböző szintű **mérete**, **sávszélesség**, **magas rendelkezésre állású**, és **SLA** beállítások.

Az alábbiakban hello Cache-ajánlatot kiválasztására vonatkozó szempontok.

* **Memória**: hello Basic és Standard rétegek ajánlat 250 MB-ra – 53 GB. hello prémium csomagban kínál fel too530 GB. További információkért lásd: [Azure Redis Cache szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/cache/).
* **Hálózati teljesítményt**: Ha egy munkaterhelés nagy teljesítményt igénylő, hello prémium csomagban nyújt további sávszélesség képest tooStandard vagy alapszintű kiadásra. Minden egyes réteg belül nagyobb méretű gyorsítótárak is nagyobb sávszélességet az alapul szolgáló virtuális gép, amelyen hello gyorsítótár hello miatt. Lásd: hello [tábla következő](#cache-performance) további információt.
* **Átviteli sebesség**: hello prémium csomagban kínál hello maximális elérhető átviteli sebesség. Ha hello gyorsítótár-kiszolgáló vagy az ügyfél eléri hello sávszélesség korlátja, hello ügyféloldali időtúllépése jelenhet meg. További információkért tekintse meg a következő táblázat hello.
* **Magas rendelkezésre állás/SLA**: Azure Redis Cache biztosítja, hogy a gyorsítótárat Standard vagy prémium elérhető legalább hello idő 99,9 %-át. toolearn az SLA-t, kapcsolatos további információkért lásd: [Azure Redis Cache szolgáltatás díjszabása](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). hello SLA vonatkozik kapcsolat toohello gyorsítótár végpontok. hello SLA nem fedi le adatvesztés elleni védelem. Azt javasoljuk, hogy hello Redis adatok megőrzését a funkciójával hello Premium szint tooincrease rugalmassági adatvesztés ellen.
* **Redis-adatmegőrzés**: hello prémium csomagban lehetővé teszi toopersist hello gyorsítótárazott adatokat az Azure Storage-fiók. A Basic vagy Standard-gyorsítótár összes hello adatok csak memóriában van tárolva. Ha nincsenek alapul szolgáló infrastrukturális problémára nincs esetleges adatvesztés lehet. Azt javasoljuk, hogy hello Redis adatok megőrzését a funkciójával hello Premium szint tooincrease rugalmassági adatvesztés ellen. Azure Redis Cache Rekordadatbázis és AOF (hamarosan elérhető) lehetőséget kínál a Redis-adatmegőrzés. További információkért lásd: [hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md).
* **Redis-fürt**: toocreate gyorsítótárazza nagyobb, mint 53 GB vagy tooshard adatok több Redis-csomópont között, fürtözéssel Redis, a hello prémium csomagban elérhető. Minden csomópont a magas rendelkezésre állású gyorsítótár elsődleges vagy replika párból áll. További információkért lásd: [hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md).
* **Fokozott biztonsági és hálózati elkülönítési**: Azure virtuális hálózatot (VNET) központi telepítés biztosít magasabb védelmet és elszigeteltséget az Azure Redis Cache, valamint a alhálózatok, hozzáférés-vezérlési házirendeket, és egyéb szolgáltatások toofurther korlátozhatja a hozzáférést. További információkért lásd: [hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).
* **Konfigurálja a Redis**: hello Standard és prémium csomag szükséges, konfigurálhatja a Redis kulcstérértesítések használatával az értesítésekhez.
* **Ügyfélkapcsolatok maximális számát**: hello prémium csomagban kínál hello csatlakozó ügyfelek is tooRedis, magasabb azonosítószámot nagyobb méretű gyorsítótárak a kapcsolatok maximális számát. További információkért lásd: [Azure Redis Cache árképzési](https://azure.microsoft.com/pricing/details/cache/).
* **A Redis Server Core dedikált**: hello prémium csomagban, az összes gyorsítótár méretének rendelkezik egy dedikált core Redis. Hello Basic vagy Standard rétegek, a hello C1 méretét, és újabb rendelkezik egy dedikált core Redis-kiszolgáló.
* **A redis az egyszálas** , kettőnél több maggal rendelkező nem biztosít további előnye képest csak két maggal rendelkező, de nagyobb Virtuálisgép-méretek általában rendelkeznek nagyobb sávszélesség-nál kisebb méretű. Ha hello gyorsítótár-kiszolgáló vagy az ügyfél eléri hello sávszélességkorlátok, majd kap hello ügyféloldali időtúllépése.
* **Teljesítménnyel kapcsolatos fejlesztések**: gyorsítótárakat a hello prémium csomagban vannak telepítve, amely gyorsabb processzorral rendelkezik hardver jogosultságot ad a jobb teljesítmény összehasonlított toohello Basic vagy Standard szint. Prémium szintű réteghez gyorsítótárainak rendelkezik nagyobb átviteli teljesítményt és kisebb késések fordulnak elő.

<a name="cache-performance"></a>

### <a name="azure-redis-cache-performance"></a>Azure Redis Cache-teljesítmény
hello következő táblázatban a különböző méretű standard tesztelése során hello maximális sávszélesség értéke, és a prémium szintű gyorsítótárazza a használatával `redis-benchmark.exe` egy infrastruktúra-szolgáltatási virtuális gép hello Azure Redis Cache végpont ellen. 

>[!NOTE] 
>Ezeket az értékeket nem garantált, és ezek nem SLA-számok, de az tipikus van. Be kell tölteni a saját alkalmazás toodetermine hello jobb gyorsítótár mérete az alkalmazás teszteléséhez.
>
>

Ebből a táblázatból azt is megrajzolásához hello következtetéseket a következő:

* Adatátviteli sebességét, amelyek azonos mérete nagyobb a hello hello gyorsítótárak prémium csomagban hello mint összehasonlított toohello Standard csomagra. Például 6 GB gyorsítótárával P1 átviteli sebességgel 180 000 RPS mint összehasonlított too49, 000 C3.
* Fürtszolgáltatás Redis átviteli nő lineárisan szilánkok (csomópontok) hello fürt hello számának növelésével. Például, ha a 10 szilánkok P4 fürt létrehozása, majd hello elérhető átviteli sebesség 400000 * 10 = 4 millió RPS.
* A kulcs mérete nagyobb átviteli sebesség hello Premium szint nagyobb, mint összehasonlított toohello Standard csomagra.

| Tarifacsomag | Méret | Processzormagok | Rendelkezésre álló sávszélesség | 1 KB-os méret |
| --- | --- | --- | --- | --- |
| **Standard gyorsítótár mérete** | | |**Megabit / másodperc (Mb/s) vagy megabájt / másodperc (MB/s)** |**Kérelmek / másodperc (RPS)** |
| C0 |250 MB |Közös |5 / 0.625 |600 |
| C1 |1 GB |1 |100 / 12.5 |12,200 |
| C2 |2.5 GB |2 |200 / 25 |24,000 |
| C3 |6 GB |4 |400 / 50 |49,000 |
| C4 |13 GB |2 |500 / 62.5 |61,000 |
| C5 |26 GB |4 |1,000 / 125 |115,000 |
| C6 |53 GB |8 |2,000 / 250 |150,000 |
| **Prémium szintű gyorsítótár mérete** | |**CPU-magokat shard száma** | **Megabit / másodperc (Mb/s) vagy megabájt / másodperc (MB/s)** |**Kérelmek / másodperc (RPS) shard száma** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |
| P2 |13 GB |4 |3,000 / 375 |360,000 |
| P3 |26 GB |4 |3,000 / 375 |360,000 |
| P4 |53 GB |8 |6,000 / 750 |400,000 |

A hello letöltésével kapcsolatban eszközök például a Redis `redis-benchmark.exe`, lásd: hello [hogyan futtathatom Redis parancsok?](#cache-commands) szakasz.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Milyen régióban I, keresse meg a gyorsítótár?
A legjobb teljesítmény és a legkisebb mértékű késleltetést, keresse meg az Azure Redis Cache hello és az ügyfélalkalmazás ugyanabban a régióban.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-redis-cache"></a>Hogyan vagyok fizetni az Azure Redis Cache?
Az Azure Redis Cache árképzési [Itt](https://azure.microsoft.com/pricing/details/cache/). hello árképzést ismertető oldalra sorolja fel, mint egy óradíjat árképzési. Gyorsítótárak számlázása kezdve hello hello gyorsítótár jön létre, amely a gyorsítótár törlése hello időpontig perc alapon. Nincs lehetőség az leállítása és a gyorsítótár hello számlázási felfüggesztése.

### <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Azure Redis Cache használható Azure Government felhő, Azure Kína felhő vagy a Microsoft Azure Németország?
Igen, Azure Redis Cache nem áll rendelkezésre Azure Government felhő, az Azure Kína felhő és a Microsoft Azure Németország. hello URL-címek elérésére, és az Azure Redis Cache kezelése az Azure nyilvános Felhőjében összehasonlítja a felhők eltérőek. 

| Felhő   | A Redis DNS-utótagja            |
|---------|---------------------------------|
| Nyilvános  | *. redis.cache.windows.net       |
| USA-beli államigazgatás  | *. redis.cache.usgovcloudapi.net |
| Németország | *. redis.cache.cloudapi.de       |
| Kína   | *. redis.cache.chinacloudapi.cn  |

Azure Redis Cache használatának és egyéb szempontjai további információkért tekintse meg a következő hivatkozások hello.

- [Az Azure Government adatbázisok - Azure Redis gyorsítótár](../azure-government/documentation-government-services-database.md#azure-redis-cache)
- [Az Azure Kína felhő - Azure Redis gyorsítótár](https://www.azure.cn/documentation/services/redis-cache/)
- [A Microsoft Azure-Németország](https://azure.microsoft.com/overview/clouds/germany/)

Az Azure Redis Cache használata Azure Government felhő, az Azure Kína felhőben és a Microsoft Azure Németország PowerShell információkért lásd: [hogyan tooconnect tooother felhők - Azure Redis Cache PowerShell](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-hello-stackexchangeredis-configuration-options-do"></a>Mit tegye hello StackExchange.Redis konfigurációs beállítások?
StackExchange.Redis számos lehetőség van. Ez a szakasz néhány általános beállítások hello beszél. Részletesebb információ a StackExchange.Redis lehetőségekről, lásd: [StackExchange.Redis konfigurációs](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Leírás | Ajánlás |
| --- | --- | --- |
| AbortOnConnectFail |Ha beállítása tootrue, hello kapcsolat nem csatlakozik újra egy hálózati hiba után. |Állítsa be a toofalse, és lehetővé teszik a StackExchange.Redis újra automatikusan. |
| ConnectRetry |hello ennyiszer toorepeat kapcsolódási kísérletek során kezdeti csatlakozáshoz. |Tekintse meg a következő útmutató hello. |
| ConnectTimeout |Időkorlátot adja meg az ms connect műveletek. |Tekintse meg a következő útmutató hello. |

Hello ügyfél hello alapértelmezett értékek általában elegendő. Úgy finomhangolhatja, a munkaterhelés alapuló hello-beállítások.

* **Újbóli próbálkozások**
  * ConnectRetry és ConnectTimeout általános útmutatást hello gyors toofail, és próbálja megismételni a műveletet. Ez az útmutató a számítási feladatok és mennyi idő átlagos azt vesz igénybe az ügyfél tooissue egy Redis parancs számára, és választ kapnak alapul.
  * Lehetővé teszik a StackExchange.Redis automatikusan újra kapcsolat állapotának ellenőrzése és újracsatlakozás saját maga helyett. **Kerülje a hello ConnectionMultiplexer.IsConnected tulajdonság**.
  * Snowballing - néha előfordulhat, hogy az a probléma hol vannak újrapróbálkozás hello snowball újrapróbálkozik, és soha nem állítja helyre a futtatása. Snowballing esetben érdemes, exponenciális leállítási újrapróbálkozási algoritmust használ, a [ismételje meg az általános útmutatást](../best-practices-retry-general.md) hello Microsoft Patterns & eljárások csoport tett közzé.
* **Időkorlát-értékei**
  * Érdemes lehet a munkaterhelés, és ennek megfelelően beállíthatja a hello értékeit. Ha nagy értékeket tárolja, hello tooa magasabb időkorlátjának beállítása.
  * Állítsa be `AbortOnConnectFail` toofalse és lehetővé teszik a StackExchange.Redis meg újra.
  * Használjon egyetlen ConnectionMultiplexer példányt hello alkalmazáshoz. Látható módon használhatja a LazyConnection toocreate a Connection tulajdonság által visszaadott egypéldányos [toohello gyorsítótár hello ConnectionMultiplexer osztály használatával csatlakozzon](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Set hello `ConnectionMultiplexer.ClientName` tulajdonság tooan példány egyedi alkalmazásnév diagnosztikai célokra.
  * Használjon több `ConnectionMultiplexer` egyéni munkaterhelések példányait.
      * Ha az alkalmazás különböző terhelés kövesse ezt a modellt. Példa:
      * Egy multiplexer nagy kulcsok kezelésére is.
      * Egy kis kulcsok kapcsolatos multiplexer lehet.
      * Kapcsolat időtúllépése különböző értékeinek beállítása, és minden ConnectionMultiplexer, amelyekkel az újrapróbálkozási logika.
      * Set hello `ClientName` minden diagnosztika a multiplexer toohelp tulajdonságát.
      * Ez az útmutató azt eredményezheti, leegyszerűsített toomore késés / `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Melyik Redis gyorsítótár-ügyfelek használható?
Hello kiváló dolgot Redis kapcsolatos egyik, hogy vannak-e számos különböző fejlesztési nyelveket támogató ügyfelek számát. Az ügyfelek aktuális listájának megtekintéséhez lásd: [Redis-ügyfelek](http://redis.io/clients). A cikk egy több különböző nyelv és az ügyfelek, lásd: [hogyan toouse Azure Redis Cache-gyorsítótár](cache-dotnet-how-to-use-azure-redis-cache.md) hello nyelvi kapcsoló hello cikk hello tetején hello kívánt nyelvet, majd.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Az Azure Redis Cache egy helyi emulátor van?
Az Azure Redis Cache nem helyi emulátor van, de futtathatja a redis-server.exe hello MSOpenTech verziója hello [parancssori eszközök Redis](https://github.com/MSOpenTech/redis/releases/) a helyi számítógéphez, és csatlakozzon a tooit tooget hasonló élményt tooa helyi gyorsítótár emulátor, ahogy az alábbi példa hello:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect tooa locally running instance of Redis toosimulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Konfigurálhatja a [redis.conf dokumentációját](http://redis.io/topics/config) fájl toomore közel hasonlóan hello [alapértelmezett gyorsítótár beállításai](cache-configure.md#default-redis-server-configuration) számára az online Azure Redis Cache igény.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Hogyan futtathatom Redis parancsok?
Bármelyik megtalálható a hello parancsok használható [parancsok Redis](http://redis.io/commands#) megtalálható a hello parancsok kivételével [parancsok nem támogatott az Azure Redis Cache Redis](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Lehetősége van több beállítások toorun Redis parancsok.

* Ha egy Standard vagy prémium gyorsítótár, futtathatja a Redis-parancsok használatával hello [Redis konzol](cache-configure.md#redis-console). hello Redis-konzol tartalmaz egy biztonságos módon toorun Redis-parancsok hello Azure-portálon.
* Hello Redis parancssori eszközöket használhatja. toouse, hajtsa végre hello a következő lépéseket:
* Töltse le a hello [parancssori eszközök Redis](https://github.com/MSOpenTech/redis/releases/).
* Csatlakozás toohello gyorsítótár használatával `redis-cli.exe`. Adjon át hello gyorsítótár végpontjához hello -h kapcsoló és hello kulcs használatával – ahogy az alábbi példa hello használata:
* `redis-cli -h <your cache="" name="">
  .redis.cache.windows.net -a <key>
  `

> [!NOTE]
> hello Redis parancssori eszközök nem használhatók hello SSL-port, de egy segédprogramot használhatja például a `stunnel` toosecurely hello hello utasításait követve csatlakoztassa a hello eszközök toohello SSL-port [bejelentése ASP.NET munkamenetállapot-szolgáltatóját a Előzetes redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzést.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-hello-other-azure-services"></a>Miért nem rendelkezik az MSDN osztály kódtár – dokumentáció például néhány más Azure-szolgáltatásokkal hello Azure Redis Cache?
A Microsoft Azure Redis Cache hello népszerű nyílt forráskódú Redis Cache alapul, és számos hozzáférhet [Redis-ügyfelek](http://redis.io/clients) számos programozási nyelv. Minden ügyfél rendelkezik saját API-t, hogy elérhetővé válnak meghívja a toohello Redis cache példány használt [parancsok Redis](http://redis.io/commands).

Mivel egyes ügyfelek különböző, nem egy központosított osztályhivatkozást az MSDN webhelyen van, és minden ügyfél fenn a saját ismertető dokumentációjában. Ezenkívül toohello dokumentáció, nincsenek több oktatóanyagok megjelenítő tooget indításának Azure Redis Cache segítségével több különböző nyelvet és a gyorsítótárazási ügyfelek számára. tooaccess ezek az oktatóanyagok lásd [hogyan toouse Azure Redis Cache-gyorsítótár](cache-dotnet-how-to-use-azure-redis-cache.md) hello nyelvi kapcsoló hello cikk hello tetején hello kívánt nyelvet, majd.

### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Használhatok Azure Redis Cache PHP munkamenet-gyorsítótár?
Igen, adja meg a toouse Azure Redis Cache PHP munkamenet-gyorsítótár, mint a hello kapcsolati karakterlánc tooyour Azure Redis Cache példány a `session.save_path`.

> [!IMPORTANT]
> Ha a Azure Redis Cache-t egy PHP munkamenet-gyorsítótár, a URL-címet kell kódolni hello biztonsági kulcs használt tooconnect toohello gyorsítótár, ahogy az alábbi példa hello:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Ha hello kulcs nem kódolt URL-cím, egy üzenet, például a kivétel jelenhet meg:`Failed tooparse session.save_path`
>
>

További információ a Redis Cache használata egy PHP munkamenet-gyorsítótár hello PhpRedis ügyféllel: [PHP munkamenet kezelő](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Mik azok a Redis-adatbázisok?

A redis-adatbázisok a következők hello belül csak egy logikai elkülönítése ugyanazt a Redis-példányt. hello gyorsítótár-memória megosztott összes hello adatbázisok között, és egy adott adatbázisnak tényleges memóriahasználatának hello kulcsok és-értékek tárolja, hogy az adatbázis függ. Egy C6 gyorsítótár például 53 GB memóriával rendelkezik. Dönthet tooput összes 53 GB egy adatbázisba vagy fel azt több adatbázis között. 

> [!NOTE]
> Prémium szintű Azure Redis Cache engedélyezve fürtözési használatakor csak adatbázis 0 érhető el. Ez a korlátozás egy belső Redis korlátozás, és nem adott tooAzure Redis Cache. További információkért lásd: [van toomake bármely módosítások toomy ügyfél alkalmazás toouse fürtszolgáltatás?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-hello-non-ssl-port-for-connecting-tooredis"></a>Ha engedélyezze hello nem SSL port tooRedis csatlakozni?
Redis kiszolgáló nem támogatja natív módon SSL azonban Azure Redis Cache nem. Ha tooAzure Redis Cache csatlakozik, és az ügyfél támogatja az SSL, például a StackExchange.Redis, akkor SSL kell használnia.

>[!NOTE]
>hello nem SSL port az új Azure Redis Cache példány alapértelmezés szerint le van tiltva. Ha az ügyfél nem támogatja az SSL, akkor engedélyeznie kell hello nem SSL port hello hello útmutatásait követve [hozzáférési portok](cache-configure.md#access-ports) hello szakasza [gyorsítótár konfigurálása az Azure Redis Cache](cache-configure.md) cikk.
>
>

Eszközök például a redis `redis-cli` hello SSL-port nem működik, de egy segédprogramot használhatja például a `stunnel` toosecurely hello hello utasításait követve csatlakoztassa a hello eszközök toohello SSL-port [bejelentése ASP.NET munkamenetállapot-szolgáltatóját az előzetes kiadás Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzést.

Hello letöltésével kapcsolatban Redis eszközök, a következő témakörben: hello [hogyan futtathatom Redis parancsok?](#cache-commands) szakasz.

### <a name="what-are-some-production-best-practices"></a>Mik azok a termelési tanácsokat?
* [StackExchange.Redis gyakorlati tanácsok](#stackexchangeredis-best-practices)
* [Konfigurációs és fogalmak](#configuration-and-concepts)
* [Teljesítménytesztelés](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis gyakorlati tanácsok
* Állítsa be `AbortConnect` toofalse, majd lehetővé teszik a hello ConnectionMultiplexer újra automatikusan. [Itt talál információt](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Hello ConnectionMultiplexer felhasználhatja – ne hozzon létre egy új egyes kérésekre vonatkozóan. Hello `Lazy<ConnectionMultiplexer>` mintát [itt látható](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) ajánlott.
* Redis works legjobban a kisebb értékek, ezért fontolja meg több kulcsokból nagyobb adatok aprító. A [Redis ismertető](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb nagy tekinthető. Olvasási [Ez a cikk](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) egy példa probléma, amely nagy értékek okozhatja.
* Konfigurálja a [ThreadPool beállítások](#important-details-about-threadpool-growth) tooavoid időtúllépések.
* Használjon legalább 5 másodperces alapértelmezett connectTimeout hello. Ezt az időközt ad StackExchange.Redis elegendő idő toore-hello kapcsolat esetén a hálózati blip létrehozásához.
* Ne feledje hello teljesítmény költségek társított különféle műveletekkel futtatja. Például hello `KEYS` parancs O(n) művelet, és el kell kerülni. Hello [redis.io hely](http://redis.io/commands/) hello idő összetettsége minden művelethez, hogy az támogatja-e körül részleteket tartalmaz. Kattintson az egyes parancs toosee hello összetettsége minden művelethez.

#### <a name="configuration-and-concepts"></a>Konfigurációs és fogalmak
* Használja a Standard vagy prémium csomagban éles rendszerek esetén. az alapszintű csomag hello egyetlen csomópont rendszer nincs adatreplikáció és nem az SLA-t. Ezenkívül legalább egy C1-gyorsítótárazás használatára. C0 gyorsítótárak az egyszerű szolgáltatásának fejlesztési/tesztelési általában használják.
* Ne feledje, hogy a Redis egy **memórián belüli** adattár. Olvasási [Ez a cikk](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , hogy tisztában lehet a esetben adatvesztés következhet be.
* A rendszer fejlesztése úgy, hogy a kapcsolat blips is képes kezelni [toopatching és a feladatátvétel miatt](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Teljesítménytesztelés
* A Start `redis-benchmark.exe` tooget abba lehetséges átviteli sebesség eléréséhez saját telj tesztek írása előtt. Mivel `redis-benchmark` nem támogatja az SSL, meg kell [hello Azure-portálon keresztül hello nem SSL port engedélyezéséhez](cache-configure.md#access-ports) hello vizsgálat futtatása előtt. Tekintse meg a [hogyan elvégez egy teljesítménytesztet és tesztelése a gyorsítótár teljesítményének hello?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* hello ügyfél teszteléshez használt virtuális gép hello kell megadni a Redis cache példány és ugyanabban a régióban.
* Azt javasoljuk, Dv2 Virtuálisgép-sorozat a ügyfél még jobban hardverrel rendelkezik, és adjon hello legjobb eredmények elérése érdekében.
* Győződjön meg arról, hogy az ügyfél úgy dönt, a virtuális gép rendelkezik legalább annyi számítási és a sávszélesség funkció, a tesztelni kívánt hello gyorsítótár.
* Ha a Windows engedélyezi a VRSS hello ügyfélszámítógépen. [Itt talál információt](https://technet.microsoft.com/library/dn383582.aspx).
* Mert jobb hardveren futtatnak a Processzor- és hálózati jobban rendelkezik prémium szint Redis példányok hálózati késés és a teljesítményt.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-hello-considerations-when-using-common-redis-commands"></a>Melyek azok hello szempontok közös Redis-parancsok használata esetén?
* Ne futtassa a bizonyos Redis-parancsok egy hosszú ideig toocomplete állapotba anélkül, hogy ezek a parancsok hello hatásának ismertetése.
  * Például nem fut a hello [kulcsok](http://redis.io/commands/keys) parancs éles környezetben, mint egy hosszú ideig tooreturn attól függően, hogy a kulcsok hello száma is igénybe vehet. A redis egy egyszálas kiszolgáló és egy időben feldolgozza a parancsokat. Ha más parancsok után kulcsok, akkor nem fogja feldolgozni amíg Redis feldolgozza hello kulcsok parancsot. Hello [redis.io hely](http://redis.io/commands/) hello idő összetettsége minden művelethez, hogy az támogatja-e körül részleteket tartalmaz. Kattintson az egyes parancs toosee hello összetettsége minden művelethez.
* Kulcsméretekkel - érdemes használni kis kulcs/érték vagy nagy kulcs/érték? Általában hello forgatókönyv függ. Ha adott esetben szükség van a nagyobb kulcsok, hello ConnectionTimeout módosítsa és próbálja meg újra az értékeket, és állítsa be az újrapróbálkozási logika. A Redis-kiszolgáló szempontjából a kisebb értékek figyelhetők toohave jobb teljesítmény érdekében.
* Ezeket a szempontokat nem jelenti azt, hogy nem tárolja a nagyobb értékek a Redis; a következő szempontok hello tisztában kell lennie. Késések nagyobb lesz. Ha egy adatkészletet, amely nagyobb, a másik kisebb, ConnectionMultiplexer több példányt is használhat, minden más-más időkorlát és újrapróbálási érték leírtak szerint konfigurálta előző hello [mi tegye hello Hajtsa végre a StackExchange.Redis-konfigurációs beállítások](#cache-configuration) szakasz.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-hello-performance-of-my-cache"></a>Hogyan elvégez egy teljesítménytesztet és tesztelése a gyorsítótár teljesítményének hello?
* [Gyorsítótár-diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) így [figyelő](cache-how-to-monitor.md) hello a gyorsítótár állapotát. Megtekintheti a hello Azure-portálon, és a metrikák is hello [töltse le, és tekintse át](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) őket az Ön által választott hello eszközökkel.
* A redis-benchmark.exe tooload teszt használhatja a Redis-kiszolgáló.
* Győződjön meg arról, hogy hello terhelés tesztelési ügyfél és a hello Redis gyorsítótár hello ugyanabban a régióban.
* A redis-cli.exe használja, és figyelje hello gyorsítótár hello INFO paranccsal.
* Ha a terhelést okoz nagy a memória töredezettségét, kell méretezni tooa nagyobb gyorsítótár méretét.
* Hello letöltésével kapcsolatban Redis eszközök, a következő témakörben: hello [hogyan futtathatom Redis parancsok?](#cache-commands) szakasz.

hello következő parancsok példát kell megadni a redis-benchmark.exe használatával. A pontos eredményeket, futtassa az alábbi parancsokat a hello VM és a gyorsítótárat ugyanabban a régióban.

* Egy 1 KB-os hasznos használó teszt futószalagos beállítása kérések

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Teszt futószalagos GET kérelmek használatával egy 1 KB-os hasznos.
  Megjegyzés: A fenti első toopopulate gyorsítótár hello-SET teszt futtatása

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Fontos tudnivalók a szálkészlet növekedési
hello CLR ThreadPool kétféle szálak - "Munkavégző" és "I/O végrehajtási Port" (más néven i/o végrehajtási port Megnyitásakor.%n) szálak.

* Munkavégző szál használható többek között a feldolgozás a `Task.Run(…)` vagy `ThreadPool.QueueUserWorkItem(…)` módszerek. Ezek a szálak is használják hello CLR a különböző összetevői, ha munkahelyi toohappen háttér szálban kell.
* I/o végrehajtási port Megnyitásakor.%n szálak aszinkron IO történik (pl. olvasásakor hello hálózati) használja.

hello szálkészlet lehetővé teszi új munkavégző szál, vagy i/o-befejezési szálak igény szerinti (nélkül a sávszélesség-szabályozás), amíg eléri az egyes szál hello "Minimális" beállítást. Alapértelmezés szerint hello minimális szálak száma értéke toohello a processzorok számát a rendszeren.

Egyszer meglévő (foglalt) szálak találatok hello "minimális" szálak száma hello száma, hello ThreadPool fog sávszélesség-szabályozási hello érték, amellyel új szálak tooone szál / 500 ezredmásodperc esetében. Általában Ha a rendszer lekérdezi az i/o végrehajtási port Megnyitásakor.%n szál kellene munka továbbítás, fogja feldolgozni munka nagyon gyorsan. Azonban ha hello kapacitásnövelés munka több mint hello "Minimális" beállítást, lesz némi késedelemmel feldolgozásakor egy része hello módon hello ThreadPool megvárja-e a toohappen két dolog egyikét.

1. Egy meglévő szál szabad tooprocess hello munkahelyi válik.
2. Nem létező szál válik 500ms, a szabad, egy új szál jön létre.

Alapvetően az azt jelenti, hogy ha hello foglalt szálak száma nagyobb, mint a minimális szálak, valószínűleg fizet 500ms késleltetést hello alkalmazás hálózati forgalom feldolgozása előtt. Is fontos, hogy amikor egy meglévő szál marad (mi emlékszem alapján) 15 másodpercnél hosszabb ideig inaktív toonote, akkor törlődnek, és ez a ciklus növekedési ütemet, és zsugorodás megismétlésével.

Ha úgy tekintünk, például hibaüzenetet a StackExchange.Redis (build 1.0.450 vagy újabb), látni fogja, hogy most jelenít ThreadPool statisztika (i/o végrehajtási port Megnyitásakor.%n és MUNKAVÉGZŐ részleteit lásd alább).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Hello előző példában láthatja, hogy az i/o végrehajtási port Megnyitásakor.%n szál nincsenek 6 foglalt szálak és hello rendszer konfigurált tooallow 4 minimális szálak. Ebben az esetben hello ügyfél volna valószínűleg láthatta két 500 ms késések mert 6 > 4.

Vegye figyelembe, hogy StackExchange.Redis időtúllépések is találati, ha az i/o végrehajtási port Megnyitásakor.%n vagy feldolgozó szálak növekedési halmozódni lekérdezi.

### <a name="recommendation"></a>Ajánlás
Ezt az információt kap, Határozottan javasoljuk, hogy az ügyfelek meg hello minimális konfigurációs érték az i/o végrehajtási port Megnyitásakor.%n és MUNKAVÉGZŐ szál toosomething hello alapértelmezett értéknél nagyobb. Felállításához nem létezik univerzális útmutatást milyen ezt az értéket kell mert hello megfelelő értéket egy alkalmazás fogja túl magas vagy alacsony egy másik alkalmazás. Ezt a beállítást is hatással lehet hello teljesítmény összetett alkalmazásokon, egyéb részeinek, minden ügyfél kell ezt a beállítást tootheir adott kell toofine hangolhatja. Jó kiindulópontot 200-as vagy 300-at, majd tesztelése, és szükség szerint módosíthatja.

Hogyan tooconfigure ezt a beállítást:

* Az ASP.NET, használja a hello ["minIoThreads" konfigurációs beállítás] [ "minIoThreads" configuration setting] alatt hello `<processModel>` konfigurációs elem a Web.config fájlban. Ha Azure Websitesra belül futtat, akkor ezt a beállítást nincs közzétéve, az hello konfigurációs beállítások. Azonban továbbra is ajánlott ezt a beállítást programozott módon képes tooconfigure lehet (lásd alább) a Application_Start metódus a global.asax.cs.

  > [!NOTE] 
  > hello meghatározott a konfigurációs elem értéke egy *-core* beállítást. Például ha egy 4 processzormagos számítógép, és szeretné, hogy a minIOThreads beállítás toobe 200 futásidőben, használhatja `<processModel minIoThreads="50"/>`.
  >

* Az ASP.NET kívül használja a hello [ThreadPool.SetMinThreads(...) ](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>

### <a name="enable-server-gc-tooget-more-throughput-on-hello-client-when-using-stackexchangeredis"></a>Engedélyezze a további átviteli hello ügyfélen server GC tooget StackExchange.Redis használata esetén
A globális Katalógus kiszolgáló engedélyezése hello ügyfél optimalizálása, és adja meg a jobb teljesítmény és átviteli StackExchange.Redis használatakor. További információ a Globáliskatalógus-kiszolgálón, és hogyan tooenable, tekintse meg a következő cikkek hello:

* [a globális Katalógus tooenable kiszolgáló](https://msdn.microsoft.com/library/ms229357.aspx)
* [A szemétgyűjtés alapjai](https://msdn.microsoft.com/library/ee787088.aspx)
* [A szemétgyűjtés és teljesítmény](https://msdn.microsoft.com/library/ee851764.aspx)


### <a name="performance-considerations-around-connections"></a>Kapcsolatok körül teljesítménnyel kapcsolatos szempontok

Minden tarifacsomag különböző korlátai ügyfélkapcsolatokat, a memória és a sávszélesség rendelkezik. Miközben lehetővé teszi, hogy minden egyes gyorsítótár méretének *legfeljebb* bizonyos számú kapcsolatok, minden kapcsolat tooRedis rendelkezik terhet társítva. Erre példa ilyen terheléssel lehet Processzor- és memóriahasználatról miatt a TLS/SSL-titkosítást. a megadott gyorsítótárméret hello maximális kapcsolathoz megadott korlátot azt feltételezi, hogy egy némileg betöltött gyorsítótár. Ha a kapcsolat terhelés betöltése *plus* Ügyfélműveletek a terhelés meghaladja a kapacitását hello rendszerhez, hello gyorsítótár kapacitás problémákat tapasztalhat, még akkor is, ha nem lépik túl a gyorsítótár jelenlegi mérete hello hello kapcsolathoz megadott korlátot.

Az egyes rétegekhez hello különböző kapcsolatok korlátok kapcsolatos további információkért lásd: [Azure Redis Cache árképzési](https://azure.microsoft.com/pricing/details/cache/). Kapcsolatok és más alapértelmezett konfigurációkkal kapcsolatos további információkért lásd: [kiszolgálókonfiguráció alapértelmezett Redis](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-hello-health-and-performance-of-my-cache"></a>Hogyan figyelhetek hello állapotának és teljesítményének a gyorsítótár?
A Microsoft Azure Redis Cache példány lesz figyelhető hello [Azure-portálon](https://portal.azure.com). Akkor megtekintheti metrikákat, PIN-kód metrikák diagramok toohello Kezdőpulton, hello dátum és idő tartomány diagramok figyelési testreszabása, hozzáadása és metrikák eltávolítása hello diagramok, és állítson be riasztásokat, bizonyos feltételek teljesülése esetén. További információkért lásd: [figyelő Azure Redis Cache](cache-how-to-monitor.md).

Redis gyorsítótár hello **erőforrás menü** figyelés és hibaelhárítás céljából a gyorsítótárak eszközöket is tartalmazza.

* **Hibáinak diagnosztizálásához és elhárításához** tájékoztatást ad azokról a gyakori problémák és stratégiák kapcsolatos őket.
* **Erőforrás állapota** az erőforrás figyeli, és jelzi, hogy ha a várt módon fut.. Hello Azure Resource health service kapcsolatos további információkért lásd: [Azure-erőforrás állapotának áttekintése](../resource-health/resource-health-overview.md).
* **Új támogatási kérelem** egy támogatási kérést beállítások tooopen biztosít a gyorsítótárhoz.

Ezek az eszközök akkor toomonitor hello állapotát az Azure Redis Cache példány engedélyezése, és a gyorsítótárazási alkalmazásokat kezeléséhez nyújt segítséget. További információkért lásd: hello "Támogatási és hibaelhárítási beállításai" szakaszának [hogyan tooconfigure Azure Redis Cache-gyorsítótár](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Miért jelenik meg időtúllépések?
Időtúllépések fordul elő hello ügyfél tootalk tooRedis használatát. A parancs toohello Redis-kiszolgáló küldött, hello parancs sorba van, és Redis-kiszolgáló végül szerzi be hello parancsot, és végrehajtja az. Azonban hello ügyfél is időtúllépés történt a folyamat során, és ha az kivétel nem jelenik meg, az ügyféloldali hívása hello. Időtúllépés problémák elhárításával kapcsolatos további információkért lásd: [ügyféloldali hibaelhárítási](cache-how-to-troubleshoot.md#client-side-troubleshooting) és [StackExchange.Redis időtúllépési kivétel](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-hello-cache"></a>Az ügyfél miért leválasztották hello gyorsítótár?
hello az alábbiakban néhány általános oka a gyorsítótár kapcsolata megszakad.

* Ügyféloldali okok
  * hello ügyfélalkalmazás lett újratelepítése.
  * hello ügyfélalkalmazás egy skálázási műveletet végrehajtani.
    * Cloud Services vagy a Web Apps hello esetben ennek oka lehet tooauto skálázást.
  * hello hálózati réteg hello ügyféloldalon megváltozott.
  * Átmeneti hiba történt a hello ügyfél vagy a hello hálózati csomópontok hello ügyfél és hello kiszolgáló között.
  * hello sávszélesség küszöbérték-határnak születtek.
  * CPU kötött műveletek toocomplete túl sokáig tartott.
* Kiszolgálóoldali okok
  * Hello szabványos gyorsítótár kínál, a hello Azure Redis Cache service feladatátvétel hello elsődleges csomópont toohello másodlagos csomópont által kezdeményezett.
  * Azure hello példány, ahol telepítve volt a hello gyorsítótár lett javítását.
    * A Redis-kiszolgáló frissítések vagy általános virtuális gép karbantartási is lehet.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Melyik Azure Gyorsítótárat használjam?
> [!IMPORTANT]
> Előző év adott [közlemény](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), Azure Managed Cache Service és az Azure szerepköralapú gyorsítótár szolgáltatás **rendelkezik visszavontuk** 2016 November 30. Azt javasoljuk, toouse [Azure Redis Cache](https://azure.microsoft.com/services/cache/). Áttelepítésével kapcsolatban további információkért lásd: [áttelepítése a Redis Cache Managed Cache Service tooAzure](cache-migrate-to-redis.md).
>
>

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis Cache általánosan elérhető mentése too53 GB méretű és 99,9 %-os SLA-elérhetőséget garantál. új hello [prémium csomagban](cache-premium-tier-intro.md) méretek too530 GB és a fürtszolgáltatás, a virtuális hálózat és az adatmegőrzést, a 99,9 %-os SLA-t kínál.

Azure Redis Cache által biztosított ügyfelek hello képességét toouse a biztonságos, dedikált Redis gyorsítótár, a Microsoft által felügyelt. Az ajánlat elérhetővé tooleverage hello gazdag szolgáltatáskészlet és az ökoszisztémákhoz Redis, megbízható üzemeltetéséhez kezelése és a Microsoft által biztosított.

Hagyományos gyorsítótárak csak foglalkozó kulcs-érték párok, ellentétben a Redis a magas performant adattípusok esetén. Redis is atomi műveletek, például a Hozzáfűzés tooa karakterlánc; ezek a típusok futtatást támogatja egy kivonatot; hello értéket növekvő előtte tooa lista; számítási set metszetének, a union és a különbség; vagy a legmagasabb prioritású egy rendezett készlet első hello tag. Egyéb szolgáltatások közé tartozik a tranzakciók, pub/sub, Lua parancsfájlok, kulcsok idő a működés közbeni korlátozott támogatása, és konfigurációs beállítások toomake Redis lehessen használni, például egy hagyományos gyorsítótár.

Egy másik kulcsfontosságú tooRedis sikere hello kifogástalan, felfedezésre vár nyílt forráskódú ökoszisztéma azt épül. Ez több nyelv megjelenik a rendelkezésre álló Redis-ügyfelek érdekében számos hello. A ökoszisztémától és az ügyfelek széles köre engedélyezése az Azure Redis Cache toobe szinte bármilyen munkaterhelést volna készít Azure belül használják.

Ismerkedés az Azure Redis Cache szolgáltatással kapcsolatos további információkért lásd: [hogyan tooUse Azure Redis Cache-gyorsítótár](cache-dotnet-how-to-use-azure-redis-cache.md) és [Azure Redis Cache dokumentáció](index.md).

### <a name="managed-cache-service"></a>Felügyelt gyorsítótár-szolgáltatás
[Managed Cache service lett visszavonva November 30 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

tooview archivált dokumentációjáért lásd: [archivált felügyelt gyorsítótár szolgáltatás dokumentációja](https://msdn.microsoft.com/library/azure/dn386094.aspx).

### <a name="in-role-cache"></a>Szerepköralapú gyorsítótár
[Szerepköralapú gyorsítótár November 30 2016 lett visszavonva.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

tooview archivált dokumentációjáért lásd: [archivált szerepköralapú gyorsítótár dokumentáció](https://msdn.microsoft.com/library/azure/dn386103.aspx).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
