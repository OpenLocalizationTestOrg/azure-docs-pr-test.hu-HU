---
title: "az ASP.NET kimeneti gyorsítótár-szolgáltató aaaCache"
description: "Megtudhatja, hogyan toocache az ASP.NET kimeneti Azure Redis Cache használata"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: sdanie
ms.openlocfilehash: fc38cc657604b351f55ad8febac383783ac29700
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>Az ASP.NET kimeneti gyorsítótár-szolgáltató az Azure Redis gyorsítótár
hello Redis kimeneti gyorsítótár-szolgáltató egy olyan folyamaton-tárolási mechanizmus a kimeneti gyorsítótár adatokhoz. Ezek az adatok kifejezetten a teljes HTTP-válaszok van (a kimeneti gyorsítótár oldalon). hello szolgáltató számítógépekbe hello új kimeneti gyorsítótár szolgáltató bővítési pontot az ASP.NET 4 lett bevezetve.

toouse hello Redis kimeneti gyorsítótár-szolgáltató, először konfigurálja a gyorsítótárhoz, és adja meg az ASP.NET-alkalmazás hello Redis kimeneti gyorsítótár szolgáltató NuGet csomag segítségével. Ez a témakör az alkalmazás toouse hello Redis kimeneti gyorsítótár-szolgáltató konfigurálása nyújt útmutatást. Létrehozásával és az Azure Redis Cache példány konfigurálásával kapcsolatos további információkért lásd: [gyorsítótár létrehozásához](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-hello-cache"></a>ASP.NET lap kimenete tárolása hello gyorsítótár
tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello Redis gyorsítótár munkamenet állapota NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **Eszközök** menü.

Futtatási hello következő parancsot a hello `Package Manager Console` ablak.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

hello Redis kimeneti gyorsítótár szolgáltató NuGet-csomag függőségi hello StackExchange.Redis.StrongName csomag kapcsolatban van. Hello StackExchange.Redis.StrongName csomag nincs jelen a projektben, ha telepíti a rendszer. Hello Redis kimeneti gyorsítótár szolgáltató NuGet-csomag kapcsolatos további információkért lásd: hello [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet lap.

>[!NOTE]
>Továbbá toohello erős névvel ellátott StackExchange.Redis.StrongName csomagban van is hello StackExchange.Redis nem-erős névvel ellátott verzió. Ha projektjéhez hello nem-erős névvel ellátott StackExchange.Redis verzióját el kell távolítani a használ, ellenkező esetben azt lekérése ütközése a projekt. Ezeket a csomagokat kapcsolatos további információkért lásd: [konfigurálása .NET-gyorsítótárazási ügyfelek számára](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

hello NuGet csomag tölti le, és hozzáadja a hello szerelvény hivatkozik, és hozzáadja a következő szakasz a web.config fájlba hello szükséges. Ez a szakasz az ASP.NET alkalmazás toouse hello Redis kimeneti gyorsítótár-szolgáltató hello kötelező beállítani.

```xml
<caching>
  <outputCachedefault Provider="MyRedisOutputCache">
    <providers>
      <!--
      <add name="MyRedisOutputCache"
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
  </outputCache>
</caching>
```

hello megjegyzésként szakasz egy példát hello attribútumokat és minta beállításait minden attribútum.

A gyorsítótár paneljét hello Microsoft Azure-portálon hello értékeivel hello attribútumainak beállítása és konfigurálása hello tetszés szerint más értékek. A gyorsítótár tulajdonságai elérése, lásd: [konfigurálása Redis gyorsítótár beállításainak](cache-configure.md#configure-redis-cache-settings).

* **állomás** – adja meg a gyorsítótár végpontjához.
* **port** – használja a nem SSL port vagy az SSL-port, attól függően, hogy hello ssl-beállítások.
* **accessKey** – a gyorsítótár vagy hello elsődleges vagy másodlagos kulcsot használja.
* **SSL** – igaz, ha azt szeretné, hogy toosecure gyorsítótár vagy ügyfél-kommunikáció SSL; ellenkező esetben hamis. Lehet, hogy toospecify hello portjának.
  * hello nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva. Adja meg ezt a beállítást toouse hello SSL-port igaz. Hello nem SSL port engedélyezésével kapcsolatos további információkért lásd: hello [hozzáférési portok](cache-configure.md#access-ports) hello szakasz [gyorsítótár konfigurálása](cache-configure.md) témakör.
* **databaseId** – megadott melyik adatbázis toouse gyorsítótár kimeneti adatokat. Ha nincs megadva, hello alapértelmezett érték 0 lesz érvényben.
* **applicationName** – kulcsok vannak tárolva, a redis `<AppName>_<SessionId>_Data`. Az elnevezési sémát lehetővé teszi, hogy több alkalmazások tooshare hello ugyanazzal a kulccsal. Ez a paraméter nem kötelező, és ha nem ad meg, egy alapértelmezett értéket használja.
* **connectionTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello connectTimeout hello StackExchange.Redis ügyfél-ban történő beállítását. Ha nincs megadva, hello alapértelmezett connectTimeout 5000 beállítással. További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello syncTimeout hello StackExchange.Redis ügyfél-ban történő beállítását. Ha nincs megadva, hello alapértelmezett syncTimeout 1000 beállítással. További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).

Adja hozzá az OutputCache direktíva tooeach lap, amelynek toocache hello kimeneti kívánja.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

Hello előző példában hello lap adatok marad hello gyorsítótár gyorsítótárazott 60 másodpercen keresztül, és minden paraméter kombináció gyorsítótárazza hello lap egy másik verziója. Hello OutputCache direktíva kapcsolatos további információkért lásd: [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

A lépések elvégzése után az alkalmazás konfigurált toouse hello Redis kimeneti gyorsítótár-szolgáltató.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a hello [ASP.NET munkamenetállapot-szolgáltatóját az Azure Redis Cache](cache-aspnet-session-state-provider.md).

