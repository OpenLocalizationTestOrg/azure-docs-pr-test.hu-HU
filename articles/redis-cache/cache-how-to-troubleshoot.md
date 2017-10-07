---
title: Azure Redis Cache aaaHow tootroubleshoot |} Microsoft Docs
description: "Ismerje meg, hogyan tooresolve közös Azure Redis Cache érintő problémákat."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: 4e736fce2b6d5200a2a8d802f3f1384b63458cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-azure-redis-cache"></a>Hogyan tootroubleshoot Azure Redis Cache-gyorsítótár
Ez a cikk a következő kategóriák Azure Redis Cache problémák hello iránymutatásokat tartalmaz.

* [Ügyfél oldal hibaelhárítási](#client-side-troubleshooting) – Ez a szakasz útmutatást nyújt azonosítására és csatlakozás tooAzure Redis Cache hello alkalmazás által okozott problémák megoldása.
* [Kiszolgáló oldal hibaelhárítási](#server-side-troubleshooting) – Ez a szakasz útmutatást nyújt azonosítására és okozott problémák megoldása a hello Azure Redis Cache kiszolgálóoldali.
* [Időtúllépési kivételek StackExchange.Redis](#stackexchangeredis-timeout-exceptions) – Ez a szakasz tájékoztatást nyújt hello StackExchange.Redis ügyfél használatával kapcsolatos hibák elhárítása.

> [!NOTE]
> Utasításokat toorun Redis parancsokat, és figyelje a teljesítménymutatók hello hibaelhárítási lépések útmutatóban számos tartalmaznak. További információt és útmutatást lásd: hello hello cikkek [további információt](#additional-information) szakasz.
> 
> 

## <a name="client-side-troubleshooting"></a>Ügyfél oldali hibaelhárítása
Ez a szakasz ismerteti a hibaelhárítási problémák, amelyek miatt hello ügyfélalkalmazás vonatkozó feltétellel.

* [Memóriaprobléma hello ügyfélen](#memory-pressure-on-the-client)
* [A forgalom kapacitásnövelés](#burst-of-traffic)
* [Magas ügyfél CPU-használat](#high-client-cpu-usage)
* [Ügyfél oldali sávszélesség túllépve](#client-side-bandwidth-exceeded)
* [Nagy méretű kérelem/válasz mérete](#large-requestresponse-size)
* [A Redis toomy történt adatok?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-hello-client"></a>Memóriaprobléma hello ügyfélen
#### <a name="problem"></a>Probléma
Memóriaprobléma hello ügyfélszámítógépen vezet, hogy késedelem nélkül hello Redis-példány által küldött adatok feldolgozása késleltetheti-e teljesítményproblémákat tooall típusú. Amikor Memóriaterhelést találatok, hello a rendszer általában van toopage adatokat a fizikai memória toovirtual memóriából, amely a lemezen. Ez *lap hibás* okok hello rendszer tooslow le jelentősen.

#### <a name="measurement"></a>Mérési
1. Gép toomake meg arról, hogy azt nem haladja meg a rendelkezésre álló memória figyelése. 
2. A figyelő hello `Page Faults/Sec` teljesítményszámláló. Rendszerek többsége fog néhány laphibák normál működés során, akkor is, a teljesítményt a lapon hibák teljesítményszámláló időtúllépések, amely így figyelendő.

#### <a name="resolution"></a>Megoldás:
Frissítse az ügyfél tooa nagyobb ügyfél Virtuálisgép-méretet több memória, vagy a memória használati minták tooreduce memória consuption a dig.

### <a name="burst-of-traffic"></a>A forgalom kapacitásnövelés
#### <a name="problem"></a>Probléma
A forgalom felszakadásáig gyenge együtt `ThreadPool` beállítások adatfeldolgozás már hello Redis-kiszolgáló által küldött, de még nem használt hello ügyféloldalon késést okozhat.

#### <a name="measurement"></a>Mérési
A figyelő hogyan a `ThreadPool` statisztika kóddal időbeli változását [, például a](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Is megtalálhatja hello `TimeoutException` StackExchange.Redis üzenetét. Íme egy példa:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

A fenti üzenet hello van néhány problémákra, amelyek a érdekes:

1. Figyelje meg, hogy a hello `IOCP` szakasz és hello `WORKER` szakasz rendelkezik egy `Busy` érték, amely nagyobb, mint hello `Min` érték. Ez azt jelenti, hogy a `ThreadPool` beállításait kell hangolását.
2. Azt is láthatja, `in: 64221`. Ez azt jelzi, hogy 64211 bájt hello kernel szoftvercsatorna-réteg nem érkezett, de még nem hello alkalmazás (pl. StackExchange.Redis) beolvasva. Ez általában azt jelenti, hogy az alkalmazás nem adatok beolvasása hello hálózati gyorsan hello kiszolgáló küld az tooyou.

#### <a name="resolution"></a>Megoldás:
Konfigurálja a [ThreadPool beállítások](https://gist.github.com/JonCole/e65411214030f0d823cb) meg arról, hogy a szálkészlet gyorsan, a rendszer növelheti toomake kapacitásnövelés forgatókönyvek.

### <a name="high-client-cpu-usage"></a>Magas ügyfél CPU-használat
#### <a name="problem"></a>Probléma
Magas CPU-használat a hello ügyfél jelzi, hogy hello rendszer nem tud lépést tartani, hogy kérték tooperform hello munkával. Ennek következménye, hogy az ügyfél hello tooprocess Redis időben választ annak ellenére, hogy a Redis nagyon gyorsan hello választ küldött.

#### <a name="measurement"></a>Mérési
A figyelő hello rendszer nagy CPU-használat hello vagy hello Azure portálon keresztül kapcsolódó teljesítményszámláló. Legyen óvatos nem toomonitor *folyamat* CPU mivel egyetlen folyamat alacsony CPU-használat: hello azonos időt, hogy a rendszer általános CPU magas lehet. A CPU-használat teljesítményt, amelyek megfelelnek a időtúllépések figyelje. Magas CPU miatt is találkozhat magas `in: XXX` értékei `TimeoutException` hibaüzenet hello leírtak [forgalom kapacitásnövelés](#burst-of-traffic) szakasz.

> [!NOTE]
> StackExchange.Redis 1.1.603 és később is hello `local-cpu` a metrika `TimeoutException` hibaüzenetek. Győződjön meg arról, hogy hello hello legújabb verzióját használja [StackExchange.Redis NuGet-csomag](https://www.nuget.org/packages/StackExchange.Redis/). Nincsenek hibák folyamatosan alatt kijavította a hello kód toomake robusztusabb tootimeouts hello legújabb verzióját, akkor fontos.
> 
> 

#### <a name="resolution"></a>Megoldás:
Frissítse a virtuális gép méretének tooa további CPU-kapacitással rendelkező átjáróeszközt, vagy vizsgálja meg, mi okozza a CPU-teljesítményt. 

### <a name="client-side-bandwidth-exceeded"></a>Ügyfél oldali sávszélesség túllépve
#### <a name="problem"></a>Probléma
Különböző méretű ügyfélgépek korlátozásokkal rendelkezik mekkora hálózati sávszélességre gyakorolt rendelkeznek érhető el. Ha hello ügyfél meghaladja a rendelkezésre álló sávszélesség hello, majd adatok nem fogja feldolgozni hello ügyféloldalon gyorsan hello server küld. Ennek eredményeképpen előfordulhat tootimeouts.

#### <a name="measurement"></a>Mérési
Figyelje meg, hogyan a sávszélesség-használat idővel kód használatával módosítani [, például a](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Vegye figyelembe, hogy ez a kód nem futtatható az sikeresen bizonyos környezetekben (például az Azure webhelyek) korlátozott engedélyekkel.

#### <a name="resolution"></a>Megoldás:
Növelje az ügyfél virtuális gép méretét, vagy csökkentse a hálózati sávszélességet.

### <a name="large-requestresponse-size"></a>Nagy méretű kérelem/válasz mérete
#### <a name="problem"></a>Probléma
A nagy méretű kérelem/válasz időtúllépések okozhat. Tegyük fel tegyük fel, hogy az időkorlát az ügyfélhez konfigurált értéke 1 másodperc. Az alkalmazás (pl. kér két kulcs "A" és "B"): hello egy időben (azonos fizikai hálózati kapcsolat hello használata). A legtöbb ügyfél támogatja a "Pipelining" kérelmek, úgy, hogy mindkét kérések "A" és "B" érkeznek hello vezetékes toohello kiszolgálón egy hello után más hello válaszok várakozás nélkül. hello server küldi hello válaszokhoz hello az azonos sorrendje. Ha a válasz "A" nagy elég azt is keleti-afrikai hello időtúllépés további kérelmeknél leggyakrabban. 

hello a következő példa bemutatja, ebben a forgatókönyvben. Ebben a forgatókönyvben "A" és "B" küldött gyorsan, hello kiszolgáló elkezdi gyors küldése válaszok "A" és "B", de adatátviteli idők, mert a "B" mögött elakadnak kérelem hello más kérés és az időpontokat, annak ellenére, hogy hello kiszolgáló válasza gyorsan.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Mérési
Ez az egy nehéz egy toomeasure. Alapvetően, hogy tooinstrument az ügyfél kód tootrack nagy kérelmeit és válaszait. 

#### <a name="resolution"></a>Megoldás:
1. A redis nagyszámú kis értékek ahelyett, hogy néhány nagy értékek van optimalizálva. hello elsődleges megoldás toobreak másolatot az adatairól a kapcsolódó a kisebb értékek. Lásd: hello [hello ideális méretét értéktartománya redis újdonságai? Túl nagy érték 100KB? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) utáni körül Miért ajánlott a kisebb értékek részleteiről.
2. Hello méretének növelése a virtuális gép (az ügyfél és a Redis gyorsítótár-kiszolgáló), tooget nagyobb sávszélesség képességeit, csökkentve az adatok átvitele a nagyobb válaszok alkalommal. Vegye figyelembe, hogy első nagyobb sávszélességet csak hello kiszolgálón, vagy csak a hello ügyfél nem elegendő. A sávszélesség mérését, és összehasonlítják azt toohello képességek hello méretű VM-tal rendelkezik.
3. Az hello számának növeléséhez `ConnectionMultiplexer` meg különböző kapcsolatokon keresztül objektumokat használatát és ciklikus multiplexelés kérelmeket.

### <a name="what-happened-toomy-data-in-redis"></a>A Redis toomy történt adatok?
#### <a name="problem"></a>Probléma
Várt bizonyos adatok toobe a saját Azure Redis Cache példányt, de ez nem tűnik toobe van.

#### <a name="resolution"></a>Megoldás:
Lásd: [a Redis toomy történt adatok?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) a lehetséges okokért és megoldásokért.

## <a name="server-side-troubleshooting"></a>Kiszolgáló oldalán hibaelhárítása
Ez a szakasz ismerteti a hibaelhárítási problémák, amelyek miatt hello gyorsítótár-kiszolgálóra vonatkozó feltétellel.

* [Memóriaprobléma hello kiszolgálón](#memory-pressure-on-the-server)
* [Magas CPU-használatot / Server betöltése](#high-cpu-usage-server-load)
* [Túllépte a kiszolgáló ügyféloldali sávszélesség](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-hello-server"></a>Memóriaprobléma hello kiszolgálón
#### <a name="problem"></a>Probléma
Memóriaprobléma hello kiszolgáló oldalán, amely késleltetheti-e a kérelmek feldolgozásának teljesítményproblémákat tooall típusú vezet. Amikor Memóriaterhelést találatok, hello a rendszer általában van toopage adatokat a fizikai memória toovirtual memóriából, amely a lemezen. Ez *lap hibás* okok hello rendszer tooslow le jelentősen. Nincsenek a memóriaterhelése lehetséges okai: 

1. Hello gyorsítótár toofull kapacitás töltötte adatokkal. 
2. Redis kapja a nagy a memória töredezettségét - leggyakrabban okozta nagy objektumok tárolására (Redis úgy optimalizálták, hogy egy kis objektumok – Lásd: hello [hello ideális méretét értéktartománya redis újdonságai? Túl nagy érték 100KB? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) részletes post). 

#### <a name="measurement"></a>Mérési
Redis mutatja meg, amelyek segítenek azonosítani azokat a probléma két metrikákat. hello első az `used_memory` és egyéb hello `used_memory_rss`. [A metrikák](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) hello Azure portál vagy hello azon keresztül elérhető [Redis INFO](http://redis.io/commands/info) parancsot.

#### <a name="resolution"></a>Megoldás:
Van több lehetséges végrehajtott változtatásokat is toohelp megtartása memóriahasználat kifogástalan:

1. [Memória-házirendet konfigurálhat](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) és a kulcsok lejárati idejének beállítására. Előfordulhat, hogy ez nem elegendő, ha töredezettsége rendelkezik.
2. [Maxmemory fenntartott érték](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) a memória töredezettségét, amely elég nagy toocompensate.
3. Szakítsa meg a nagy a gyorsítótárazott objektumok kisebb kapcsolódó objektumot.
4. [Skála](cache-how-to-scale.md) tooa nagyobb gyorsítótár méretét.
5. Ha használ egy [prémium gyorsítótár engedélyezve van a Redis-fürt](cache-how-to-premium-clustering.md) is [a szilánkok hello értéknek a növelésével](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Magas CPU-használatot / Server betöltése
#### <a name="problem"></a>Probléma
Magas CPU-használatot azt, hogy hello ügyféloldali meghiúsulhatnak tooprocess Redis időben választ, annak ellenére, hogy a Redis nagyon gyorsan hello választ küldött.

#### <a name="measurement"></a>Mérési
A figyelő hello rendszer nagy CPU-használat hello vagy hello Azure portálon keresztül kapcsolódó teljesítményszámláló. Legyen óvatos nem toomonitor *folyamat* CPU mivel egyetlen folyamat alacsony CPU-használat: hello azonos időt, hogy a rendszer általános CPU magas lehet. A CPU-használat teljesítményt, amelyek megfelelnek a időtúllépések figyelje.

#### <a name="resolution"></a>Megoldás:
[Skála](cache-how-to-scale.md) tooa nagyobb gyorsítótár réteg további CPU-kapacitással rendelkező átjáróeszközt, vagy vizsgálja meg, mi okozza a CPU-teljesítményt. 

### <a name="server-side-bandwidth-exceeded"></a>Túllépte a kiszolgáló ügyféloldali sávszélesség
#### <a name="problem"></a>Probléma
Különböző méretű gyorsítótár célpéldánynál korlátozások mekkora hálózati sávszélességre gyakorolt rendelkeznek érhető el. Ha hello server meghaladja a rendelkezésre álló sávszélesség hello, majd adatok nem küldhetők toohello ügyfél leggyorsabban. Ennek eredményeképpen előfordulhat tootimeouts.

#### <a name="measurement"></a>Mérési
Figyelheti a hello `Cache Read` metrika, amely hello hello megadott jelentéskészítési időszakban gyorsítótárból hello megabájtban (MB/s) másodpercenként beolvasott adatok mennyiségét. Ezt az értéket használják ezt a gyorsítótárat toohello sávszélesség felel meg. Ha a kiszolgáló oldalán hálózati sávszélesség korlátja szeretne figyelmeztetéseket tooset, létrehozhat azokat a `Cache Read` számláló. A mérési hello értékekkel összehasonlítja [ezt a táblázatot](cache-faq.md#cache-performance) a megfigyelt sávszélességkorlátok különböző árképzési szinteket, és méretű gyorsítótár hello.

#### <a name="resolution"></a>Megoldás:
Ha következetesen figyelhető meg az árképzési szint és a gyorsítótár méretének maximális sávszélessége hello közelében, fontolja meg [skálázás](cache-how-to-scale.md) tooa árképzési szint vagy mérete nagyobb hálózati sávszélesség álljon, a hello értékekkel [ezt a táblázatot](cache-faq.md#cache-performance) útmutatóként.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis időtúllépési kivétel
StackExchange.Redis nevű konfigurációs beállítást használja `synctimeout` szinkron műveletekhez, amely 1000 ms alapértelmezett értéke. Ha egy aszinkron hívás nem fejezi be a hello meghatározott idő, hello StackExchange.Redis ügyfél jelez egy időtúllépési hiba hasonló toohello a következő példa.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Ez a hibaüzenet tartalmazza, amely segíthet a pont, akkor hello probléma okát és lehetséges megoldás toohello metrikákat. hello alábbi táblázat részleteket tartalmaz a hello hiba üzenet metrikákat.

| Hiba történt a üzenet metrika | Részletek |
| --- | --- |
| INST |Az utolsó időszelet hello: 0 parancsok kiállították. |
| kezelője |hello szoftvercsatorna manager hajt végre `socket.select` ami azt jelenti, hogy azt kéri, az operációs rendszer hello tooindicate, amely rendelkezik egy szoftvercsatorna toodo; alapvetően: hello olvasó van nem aktívan olvasása hello hálózatból, mert az nem gondolja, hogy van toodo |
| Várólista |73 összes folyamatban lévő műveletek |
| qu |6 hello folyamatban lévő műveletek hello elküldetlen várólistájuk, és nem még készült toohello kimenő hálózati |
| QS |67-es helykiszolgálójához folyamatban lévő műveletek elküldött toohello server, de a választ még nem érhető el. hello válasz lehet `Not yet sent by hello server` vagy`sent by hello server but not yet processed by hello client.` |
| QC |hello folyamatban lévő műveletek 0 választ láthatta, de nem még jelölt miatt el a hello befejezési hurok toowaiting |
| wR |Nincs egy aktív írási (azaz hello 6 el nem küldött kérelmek nem lesznek mellőzve) bájt/activewriters |
| A |Nincs aktív olvasó és nulla bájt elérhető toobe, olvassa el a hálózati bájtok/activereaders hello |

### <a name="steps-tooinvestigate"></a>Lépéseket tooinvestigate
1. Gondoskodjon arról, hogy a legjobb hello StackExchange.Redis ügyfél használata esetén a következő mintát tooconnect hello használ.

    ```c#
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    További információkért lásd: [toohello gyorsítótár StackExchange.Redis használatával csatlakozzon](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Győződjön meg arról, hogy az Azure Redis Cache és hello ügyfélalkalmazás hello Azure-ban ugyanabban a régióban. Például akkor lehet, hogy lehet első időtúllépések Ha a gyorsítótár van USA keleti régiója, de hello ügyfél USA nyugati régiója és hello kérelem nem fejezi be hello belül `synctimeout` időköz, vagy előfordulhat, hogy lehet első időtúllépések Ha meg vannak hibakereséshez a következőből: a helyi fejlesztési számítógépén. 
   
    Toohave hello gyorsítótár lehetőleg rendelkezik, és a hello ügyfelével hello azonos Azure-régiót. Ha olyan forgatókönyvekben, amelyek több régióban hívások tartalmaz, célszerű hello `synctimeout` intervallum tooa értéke nagyobb, mint hello alapértelmezett 1000 ms-ot egy `synctimeout` tulajdonság hello kapcsolati karakterláncban. hello alábbi példa bemutatja a StackExchange.Redis gyorsítótár kapcsolati karakterlánc részlet rendelkező egy `synctimeout` 2000 MS.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Győződjön meg arról, hogy hello hello legújabb verzióját használja [StackExchange.Redis NuGet-csomag](https://www.nuget.org/packages/StackExchange.Redis/). Nincsenek hibák folyamatosan alatt kijavította a hello kód toomake robusztusabb tootimeouts hello legújabb verzióját, akkor fontos.
3. Ha nincs sávszélességgel kapcsolatos korlátozásai hello kiszolgálón vagy az ügyfél által első kötött, tovább tart a számukra toocomplete, és ekkor időtúllépések. toosee Ha az időtúllépés miatt toonetwork sávszélesség hello kiszolgálón lásd [túllépte a kiszolgáló ügyféloldali sávszélesség](#server-side-bandwidth-exceeded). Ha az időtúllépés miatt tooclient hálózati sávszélesség, toosee lásd: [ügyfél oldalán sávszélesség túllépte](#client-side-bandwidth-exceeded).
4. Ön az első CPU kötött hello kiszolgálón vagy hello ügyfél?
   
   * Ellenőrizze, hogy Ön első kötött processzor az ügyfélen, így hello kérelem toonot hello végrehajtva `synctimeout` időköz, ami az időtúllépés miatt. Hello terhelés érkezhetnek vagy tooa ügyfél méretének áthelyezése segítségével toocontrol ez. 
   * Hello figyelésével hello kiszolgálón kötött jelölőnégyzetet, ha a CPU `CPU` [teljesítmény metrika gyorsítótár](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). CPU kötött okozhat, amelyek a Redis pedig várható kérelmek tootimeout kérelmek. Ez hello terjeszthető betölteni a prémium szintű gyorsítótár több szilánkok között, vagy frissítse a tooa nagyobb méretű vagy IP-címek tooaddress. További információkért lásd: [Server ügyféloldali sávszélesség túllépését](#server-side-bandwidth-exceeded).
5. Vannak-e véve a hosszú idő tooprocess hello kiszolgálón parancsok? Hosszú ideig futó parancsokat, amelyek hosszú ideig tooprocess hello redis-kiszolgáló tart időtúllépések okozhatják. Néhány példa a hosszú ideig futó parancsok `mget` kulcsok, ha sok felhasználóval rendelkező `keys *` vagy rosszul írt lua parancsfájlok. Csatlakozás tooyour Azure Redis Cache példány hello redis-cli ügyfélprogrammal, vagy hello használata [Redis konzol](cache-configure.md#redis-console) és futtatási hello [SlowLog](http://redis.io/commands/slowlog) toosee parancs, ha a kérelem a vártnál tovább tart. A redis-kiszolgáló és a StackExchange.Redis kevesebb nagy kérelmek helyett a sok kisméretű kérelem vannak optimalizálva. Az adatok felosztása kisebb adattömbökbe javíthatja a dolgok itt. 
   
    A redis-cli és stunnel toohello Azure Redis Cache SSL végpont csatlakozó információkért lásd: hello [bejelentése ASP.NET munkamenetállapot-szolgáltatóját a Redis előzetes](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzést. További információkért lásd: [SlowLog](http://redis.io/commands/slowlog).
6. Magas Redis-kiszolgáló terhelése okozhatja időtúllépések. Hello kiszolgálóterhelés figyelheti hello figyelésével `Redis Server Load` [teljesítmény metrika gyorsítótár](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). A kiszolgáló terhelését, 100 (maximális érték) azt jelzi, hogy adott hello redis megtörtént a kiszolgáló elfoglalt, nem üresjárati idő kérelmek feldolgozásához. toosee bizonyos kérések végzése összes hello képessége, ha paranccsal hello SlowLog, hello előző bekezdésben ismertetett módon. További információkért lásd: [magas CPU-használat / Server betöltési](#high-cpu-usage-server-load).
7. Történt más esemény egy hálózati blip okozhatta hello ügyféloldalon? Tanulmányozza az hello ügyfél (web-, feldolgozói szerepkör vagy egy infrastruktúra-szolgáltatási virtuális Gépre), ha hiba történt az esemény, például az ügyfél-példányok száma hello skálázás felfelé vagy lefelé, illetve hello ügyfél új verziójának telepítése, vagy automatikus skálázása engedélyezve van? A találtunk, amelyeknek, hogy az automatikus skálázás vagy fel/le skálázás okozhat tesztelés során kimenő hálózati kapcsolat elveszhet néhány másodpercig. StackExchange.Redis kód rugalmas toosuch eseményeket, és újra fognak csatlakozni. Ebben az időszakban újbóli kapcsolat hello várólistában lévő minden kérést is túllépi az időkorlátot.
8. Történt több kis kérelmek toohello Redis gyorsítótár, amely túllépte az időkorlátot megelőző nagy kérelmet? hello paraméter `qs` hello hibás üzenet közli, hogy hány kérésnek hello ügyfél toohello kiszolgálóról küldött, de még nem dolgozott választ. Ez az érték lehet egyre több adatra mert StackExchange.Redis egyetlen TCP-kapcsolatot használ, és csak olvasható egy válasz egyszerre. Annak ellenére, hogy hello első művelete túllépte az időkorlátot, akkor az nem hello küldött adatok mennyisége hello kiszolgáló és a, és más kérelmek le vannak tiltva, amíg ez befejeződött, ami időtúllépések. Egyik megoldást időtúllépések toominimize hello esélyét, hogy annak biztosítása, hogy a gyorsítótár elég nagy a terhelés, és nagy értékek felosztása kisebb adattömbökbe. Egy másik lehetséges megoldás, toouse készletét `ConnectionMultiplexer` az ügyfél objektumokat, majd válassza ki a legalább betöltött hello `ConnectionMultiplexer` új kérelem küldésekor. Egyetlen időtúllépés kell emiatt más kérelmek tooalso időtúllépés miatt.
9. Ha használ `RedisSessionStateprovider`, győződjön meg arról, hello újrapróbálkozási időtúllépés helyesen van beállítva. `retrytimeoutInMilliseconds`nagyobbnak kell lennie `operationTimeoutinMilliseonds`, ellenkező esetben nem ismételt próbálkozás történik. Az alábbi példa hello `retrytimeoutInMilliseconds` too3000 van beállítva. További információkért lásd: [ASP.NET munkamenetállapot-szolgáltatóját az Azure Redis Cache](cache-aspnet-session-state-provider.md) és [hogyan toouse hello konfigurációs paraméterek munkamenetállapot-szolgáltatóját, és a kimeneti gyorsítótár-szolgáltató](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Ellenőrizze a memóriahasználat a hello Azure Redis gyorsítótár-kiszolgáló [figyelési](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` és `Used Memory`. Egy kiürítés házirend van beállítva, ha kezdődik-e a Redis való kulcsok mikor `Used_Memory` eléri hello gyorsítótár méretét. Ideális esetben `Used Memory RSS` csak kell valamivel nagyobb, mint `Used memory`. A nagy különbség jelenti, hogy a memória töredezettségét (belső vagy külső. Ha `Used Memory RSS` értéke kisebb, mint `Used Memory`, azt jelenti, hogy a gyorsítótár-memória hello részét rendelkezik lett cserélve hello operációs rendszer. Ez akkor fordul elő, ha várhatóan néhány jelentős késések fordulnak elő. Mert Redis nincs vezérlő keresztül hogyan a hozzárendelések leképezve toomemory lapok, magas `Used Memory RSS` gyakran egy csúcs az igények memóriahasználat hello eredménye van. Amikor Redis felszabadítja a memória, hello memória vissza toohello foglaló kap, és nem hello foglaló előfordulhat, hogy nem adhat meg hello memória hátsó toohello rendszer. Lehet eltérést között hello `Used Memory` érték és a memória-felhasználás hello operációs rendszer által jelentett módon. Elképzelhető, hogy a esedékes toohello tény használt és a Redis, de nem adott vissza toohello rendszer, amely a memória. toohelp hajthat végre a lépéseket követve hello memória problémák elhárítása érdekében.
   
   * Frissítés hello tooa nagyobb gyorsítótárméret, hogy nem futtatja találkoznak memóriakorlátozások hello rendszeren.
   * Hello kulcsok lejárati idejének beállítására, hogy a régebbi értékek proaktív ki vannak zárva.
   * A figyelő hello hello `used_memory_rss` metrika gyorsítótárazza. Ez az érték megközelíti a gyorsítótár méretének hello, valószínűleg toostart teljesítmény problémákba elkülönítésével. Ha prémium szintű gyorsítótárat használ, vagy frissíti a tooa a gyorsítótár mérete nagyobb elosztása több szegmensben osztják hello adatokat.
   
   További információkért lásd: [Memóriaterhelést hello kiszolgálón](#memory-pressure-on-the-server).

## <a name="additional-information"></a>További információ
* [Melyik Redis Cache-ajánlatot és -méretet használjam?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Hogyan elvégez egy teljesítménytesztet és tesztelése a gyorsítótár teljesítményének hello?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Hogyan futtathatom Redis parancsok?](cache-faq.md#how-can-i-run-redis-commands)
* [Hogyan toomonitor Azure Redis Cache-gyorsítótár](cache-how-to-monitor.md)

