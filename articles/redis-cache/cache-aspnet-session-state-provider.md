---
title: "ASP.NET munkamenetállapot-szolgáltatóját aaaCache |} Microsoft Docs"
description: "Megtudhatja, hogyan toostore az ASP.NET munkamenet állapot Azure Redis Cache használata"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: 192f384c-836a-479a-bb65-8c3e6d6522bb
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/01/2017
ms.author: sdanie
ms.openlocfilehash: 9ea84cf67b9314b15dce696f596d399921194510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>Az Azure Redis Cache ASP.NET munkamenetállapot-szolgáltatója
Azure Redis Cache biztosít a munkamenetállapot-szolgáltatóját, hogy toostore használhatja a munkamenet-állapot gyorsítótár helyett a memória, vagy egy SQL Server-adatbázis. toouse hello gyorsítótárazási munkamenetállapot-szolgáltatóját, először konfigurálja a gyorsítótárhoz, és konfigurálja az ASP.NET-alkalmazás gyorsítótár hello Redis gyorsítótár munkamenet állapota NuGet csomag segítségével.

Nincs gyakran egy valós cloud app tooavoid állapot egyaránt működik valamilyen formában tárolja a felhasználói munkamenet a gyakorlati, de néhány megközelítések hatással lehet a teljesítmény és méretezhetőség több mint mások. Toostore állapotban van, ha a hello legjobban megfelelő megoldást állapot kis mennyiségű tookeep hello, és a cookie-kban tárolja. Amely nem megvalósítható, hello következő legjobban megfelelő megoldást akkor toouse az ASP.NET munkamenet-állapot elosztott, memóriában lévő gyorsítótárhoz szolgáltatóhoz. hello legrosszabb megoldás teljesítményére és méretezhetőségére szempontból toouse egy adatbázis biztonsági munkamenetállapot-szolgáltatóját. Ez a témakör hello ASP.NET munkamenetállapot-szolgáltatóját használja az Azure Redis Cache nyújt útmutatást. A más munkamenet-állapotra vonatkozó információkért lásd: [az ASP.NET munkamenet-állapot beállításai](#aspnet-session-state-options).

## <a name="store-aspnet-session-state-in-hello-cache"></a>Az ASP.NET munkamenet-állapot tárolása hello gyorsítótárban
tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello Redis gyorsítótár munkamenet állapota NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **Eszközök** menü.

Futtatási hello következő parancsot a hello `Package Manager Console` ablak.
    
```
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Hello prémium csomagban a fürtszolgáltatáshoz hello használatakor használjon [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 vagy magasabb vagy kivétel lépett fel. Too2.0.1 áthelyezése vagy magasabb használhatatlanná tévő változást; További információkért lásd: [v2.0.0 Megtörje Változásrészletek](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details). A cikk frissítés hello időpontban hello aktuális Ez a csomag verziószáma 2.2.3.
> 
> 

hello Redis munkamenet állapota szolgáltató NuGet-csomag függőségi hello StackExchange.Redis.StrongName csomag kapcsolatban van. Hello StackExchange.Redis.StrongName csomag nincs jelen a projektben, ha telepíti a rendszer.

>[!NOTE]
>Továbbá toohello erős névvel ellátott StackExchange.Redis.StrongName csomagban van is hello StackExchange.Redis nem-erős névvel ellátott verzió. Ha projektjéhez hello nem-erős névvel ellátott StackExchange.Redis verzióját el kell távolítani a használ, ellenkező esetben azt lekérése ütközése a projekt. Ezeket a csomagokat kapcsolatos további információkért lásd: [konfigurálása .NET-gyorsítótárazási ügyfelek számára](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

hello NuGet csomag tölti le, és hozzáadja a hello szerelvény hivatkozik, és hozzáadja a következő szakasz a web.config fájlba hello szükséges. Ez a szakasz az ASP.NET alkalmazás toouse hello Redis gyorsítótár munkamenetállapot-szolgáltatóját hello kötelező beállítani.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
    <!--
    <add name="MySessionStateStore"
           host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "0" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "5000" [number]
    />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
    </providers>
</sessionState>
```

hello megjegyzésként szakasz egy példát hello attribútumokat és minta beállításait minden attribútum.

A gyorsítótár paneljét hello Microsoft Azure-portálon hello értékeivel hello attribútumainak beállítása és konfigurálása hello tetszés szerint más értékek. A gyorsítótár tulajdonságai elérése, lásd: [konfigurálása Redis gyorsítótár beállításainak](cache-configure.md#configure-redis-cache-settings).

* **állomás** – adja meg a gyorsítótár végpontjához.
* **port** – használja a nem SSL port vagy az SSL-port, attól függően, hogy hello ssl-beállítások.
* **accessKey** – a gyorsítótár vagy hello elsődleges vagy másodlagos kulcsot használja.
* **SSL** – igaz, ha azt szeretné, hogy toosecure gyorsítótár vagy ügyfél-kommunikáció SSL; ellenkező esetben hamis. Lehet, hogy toospecify hello portjának.
  * hello nem SSL port az új gyorsítótárakhoz alapértelmezés szerint le van tiltva. Adja meg ezt a beállítást toouse hello SSL-port igaz. Hello nem SSL port engedélyezésével kapcsolatos további információkért lásd: hello [hozzáférési portok](cache-configure.md#access-ports) hello szakasz [gyorsítótár konfigurálása](cache-configure.md) témakör.
* **throwOnError** – igaz, ha azt szeretné, hogy egy kivétel toobe vált ki, ha van hiba vagy hamis értéket, ha azt szeretné, hello művelet toofail beavatkozás nélkül. Hello statikus Microsoft.Web.Redis.RedisSessionStateProvider.LastException tulajdonság ellenőrizheti hibája miatt. hello alapértelmezett értéke true.
* **retryTimeoutInMilliseconds** – sikertelen műveletek ismétlődnek, ezen időszakban, ezredmásodpercben megadva. hello próbálkozik újra 20 ezredmásodperc után következik be, és majd próbálkozások másodpercenként eléréséig hello retryTimeoutInMilliseconds időköz. Az intervallum után azonnal hello műveletet a rendszer ismét megkísérli egy befejező időpontja. Ha továbbra sem sikerül hello művelet, hello kivétel történt vissza toohello hívó, attól függően, hogy hello throwOnError beállítást. hello alapértelmezett értéke 0, ami azt jelenti, hogy nincs újrapróbálás.
* **databaseId** – adja meg, melyik adatbázis toouse gyorsítótár kimeneti adatokat. Ha nincs megadva, hello alapértelmezett érték 0 lesz érvényben.
* **applicationName** – kulcsok vannak tárolva, a redis `{<Application Name>_<Session ID>}_Data`. Az elnevezési sémát lehetővé teszi, hogy a több-alkalmazása tooshare hello ugyanazt a Redis-példányt. Ez a paraméter nem kötelező, és ha nem ad meg, egy alapértelmezett értéket használja.
* **connectionTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello connectTimeout hello StackExchange.Redis ügyfél-ban történő beállítását. Ha nincs megadva, hello alapértelmezett connectTimeout 5000 beállítással. További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Ez a beállítás lehetővé teszi toooverride hello syncTimeout hello StackExchange.Redis ügyfél-ban történő beállítását. Ha nincs megadva, hello alapértelmezett syncTimeout 1000 beállítással. További információkért lásd: [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705).

Ezek a Tulajdonságok kapcsolatos további információkért lásd: hello eredeti blogbejegyzésben található: [bejelentése ASP.NET munkamenetállapot-szolgáltatóját a Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).

Ne feledje toocomment kimenő hello szabványos InProc munkamenet állapota szolgáltató szakasz a Web.config fájlban.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

A lépések elvégzése után az alkalmazás konfigurált toouse hello Redis gyorsítótár munkamenetállapot-szolgáltatóját. A munkamenet-állapot az alkalmazás használatakor Azure Redis Cache példány tárolódik.

> [!IMPORTANT]
> Hello gyorsítótárban tárolt adatok kell szerializálható, eltérően hello tárolt adatok hello alapértelmezett memórián belüli ASP.NET munkamenetállapot-szolgáltatóját. Hello munkamenetállapot-szolgáltatóját a Redis használata esetén lehet, hogy a munkamenet-állapot tárolt adattípusokat hello szerializálható.
> 
> 

## <a name="aspnet-session-state-options"></a>Az ASP.NET munkamenet-állapot beállításai
* Ez a szolgáltató memória munkamenetállapot-szolgáltató - hello munkamenet-állapot a memóriában tárolja. Ez a szolgáltató használatának előnye hello gyorsan és egyszerűen. A webalkalmazások azonban használata a memória-szolgáltató nem elosztott óta nem méretezhető.
* SQL Server munkamenetállapot-szolgáltató - szolgáltató Sql Server munkamenet-állapot hello tárolja. Használja ezt a szolgáltatót, ha azt szeretné, toostore hello munkamenet-állapot az állandó tároló. A webalkalmazás méretezheti, de használata az Sql Server munkamenet teljesítmény hatást gyakorol a webalkalmazás.
* Elosztott a memória munkamenetállapot-szolgáltatóját például a Redis gyorsítótár munkamenetállapot-szolgáltatóját – a szolgáltató által biztosított, akkor mindkét világot legjobb hello. A webes alkalmazás lehet egy egyszerű, gyors és méretezhető munkamenetállapot-szolgáltatóját. Mert a szolgáltató tárolók hello a gyorsítótár, az alkalmazás munkamenet-állapot tootake veszi figyelembe az összes kapcsolódó, amikor tooa memória-gyorsítótárban, például az átmeneti hálózati hibák elosztott van szó, jellemzőkkel hello. Gyakorlati tanácsok a gyorsítótár használatával, lásd: [útmutatást gyorsítótárazás](../best-practices-caching.md) Microsoft Patterns & eljárások [Azure Cloud alkalmazás tervezési és megvalósítási útmutatást](https://github.com/mspnp/azure-guidance).

További információ a munkamenet-állapot és egyéb bevált gyakorlatokat: [webes fejlesztési gyakorlati tanácsok (épület valós felhőalapú alkalmazásokat az Azure-ral)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices).

## <a name="next-steps"></a>Következő lépések
Tekintse meg a hello [az ASP.NET kimeneti gyorsítótár-szolgáltató Azure Redis Cache](cache-aspnet-output-cache-provider.md).

