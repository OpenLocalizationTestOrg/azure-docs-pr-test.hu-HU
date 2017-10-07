---
title: "aaaAzure Redis Cache minták |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Redis Cache-gyorsítótár"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: 5cf9287b577758b5d880d1ca3928c1bee643a8ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-redis-cache-samples"></a>Azure Redis Cache-minták
Ez a témakör az Azure Redis Cache mintavételt, például tooa gyorsítótár csatlakozás olvasása és írása adatok tooand a gyorsítótárból vagy hello ASP.NET Redis Cache-szolgáltatókat használ forgatókönyvek listája. Hello minták között letölthető projektek, és néhány részletes útmutatást és kódtöredékek tartalmazza, de ne csatolja tooa letölthető projekt.

## <a name="hello-world-samples"></a>Hello world – minták
Ebben a szakaszban hello minták megjelenítése kapcsolódás tooan Azure Redis Cache példány és a különböző nyelveken használó toohello adatgyorsítótár írásakor vagy olvasásakor hello alapjait, és Redis-ügyfelek.

Hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) bemutatja hogyan minta tooperform különböző gyorsítótár műveletekbe hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET ügyfél.

Ez a példa bemutatja, hogyan:

* Használjon különböző kapcsolati lehetőségek
* Olvasási és írási objektumok tooand szinkron és aszinkron műveletek használatával hello gyorsítótárból
* Redis MGET/MSET parancsok tooreturn értékek megadott kulcsok használata
* A Redis tranzakciós műveletek végrehajtása
* A Redis-listák és rendezett beállítása
* .NET-objektumok használatával JsonConvert objektumszerializáló tárolásához
* Használja a Redis beállítja tooimplement címkézés
* A Redis-fürt használata

További információkért lásd: hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentáció a githubon, és további használati forgatókönyvek esetén lásd: hello [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) egység teszteket.

[Hogyan toouse Azure Redis Cache-gyorsítótár Python](cache-python-get-started.md) bemutatja, hogyan tooget lépések segítségével a Python és hello Azure Redis Cache [redis-másolása](https://github.com/andymccurdy/redis-py) ügyfél.

[.NET-objektumok hello gyorsítótárában](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) mutat be, akkor egyirányú tooserialize .NET objektumokat, akkor is beírhatók tooand olvashatja őket az Azure Redis Cache példány. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Redis gyorsítótár használata egy Csatlakozópanel kibővítési ASP.NET SignalR
Hello [Redis Cache-használja, az ASP.NET SignalR Csatlakozópanel egy kibővítési](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) példa bemutatja, hogyan használható az Azure Redis Cache, egy SignalR csatlakozópanel. Csatlakozópanel kapcsolatos további információkért lásd: [SignalR Scaleout a redis gyorsítótárral](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis gyorsítótár ügyfél lekérdezés minta
Adatok elérése a gyorsítótárból, és az adatmegőrzési storage adatokhoz hozzáférő közötti hasonlítja össze teljesítményét mutatja be. Ez a minta két projektet tartalmaz.

* [Hogyan Redis Cache a jobb teljesítmény érdekében az adatokat bemutató](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Kezdőérték hello adatbázis és a gyorsítótár hello bemutató](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Az ASP.NET munkamenet-állapot és a kimeneti gyorsítótár
Hello [használata Azure Redis Cache toostore ASP.NET SessionState és OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) példa bemutatja, hogyan meg toouse Azure Redis Cache toostore az ASP.NET munkamenet és a kimeneti gyorsítótár használatával hello SessionState és OutputCache szolgáltatók A redis.

## <a name="manage-azure-redis-cache-with-maml"></a>Azure Redis gyorsítótár MAML kezelése
Hello [kezelése Azure Redis Cache használata Azure kezelési kódtárakat](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) minta bemutatja, hogyan lehet Azure kezelési kódtárakat toomanage - (létrehozása / frissítése / törlése) a gyorsítótárhoz. 

## <a name="custom-monitoring-sample"></a>Egyéni minta figyelése
Hello [Redis gyorsítótár-figyelés adatok](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) példa bemutatja, hogyan férhet hozzá a figyelési adatok az Azure Redis Cache hello Azure portálon kívül az.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>A PHP és a Redis használatával készítettek Twitter-stílusú másolat
Hello [Retwis](https://github.com/SyntaxC4-MSFT/retwis) minta Redis Hello World hello. A minimális Twitter-stílusú közösségi hálózati klónozott Redis és hello használata PHP használatával írt [Predis](https://github.com/nrk/predis) ügyfél. hello forráskód tervezett toobe nagyon egyszerű és: hello azonos időben tooshow különböző Redis-adatstruktúrák.

## <a name="bandwidth-monitor"></a>Sávszélesség-figyelő
Hello [sávszélesség figyelő](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) minta lehetővé teszi, hogy toomonitor hello sávszélesség hello ügyfél által használt. toomeasure hello sávszélesség, hello minta futtatásához hello gyorsítótár ügyfélszámítógépen, hívások toohello gyorsítótár ellenőrizze és hello sávszélesség figyelő minta által jelentett hello sávszélesség láthatja.

