---
title: "az Azure CDN webes tartalom lejáratának aaaManage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage lejárati Azure CDN szolgáltatás használata az Azure Web Apps/Cloud Services, az ASP.NET, vagy az IIS tartalom."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a0154236c3893b627f4480e0688f555964862556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Az Azure CDN Azure Web Apps/Cloud Services, az ASP.NET, vagy az IIS tartalom lejáratának kezelése
> [!div class="op_single_selector"]
> * [Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage-blob szolgáltatás](cdn-manage-expiration-of-blob-content.md)
> 
> 

Fájlok forráskiszolgálóról bármely nyilvánosan elérhető webkiszolgáló Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.  hello TTL határozza meg hello [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello forráskiszolgálóról hello HTTP válaszként.  Ez a cikk ismerteti, hogyan tooset `Cache-Control` fejlécek Azure Web Apps, Azure Cloud Services, az ASP.NET-alkalmazások és az Internet Information Services helyeket, amelyek hasonló módon konfigurált.

> [!TIP]
> Úgy is dönthet, tooset nem TTL-t, a fájl.  Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.
> 
> Hozzáférés toofiles és egyéb erőforrások toospeed Azure CDN szolgáltatás működésével kapcsolatos további információkért lásd: hello [Azure CDN áttekintése](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>A beállítás a Cache-Control fejlécek konfigurációban
Statikus tartalom, lemezképek és a stíluslapok, például szabályozhatja hello frissítési gyakoriság hello módosításával **applicationHost.config** vagy **web.config** a webes alkalmazás forrásfájljairól.  Hello **system.webServer\staticContent\clientCache** hello konfigurációs fájlban állítja hello `Cache-Control` fejlécet a tartalomhoz. A **web.config**, hello konfigurációs beállításai hatással lesz minden hello mappában és annak összes almappája, kivéve, ha hello almappa szintjén felülbírálható.  Például beállíthat egy alapértelmezett élő idő: hello legfelső szintű toohave összes statikus tartalmat 3 napig, de egy almappát, amely rendelkezik a 6 órás gyorsítótár beállítással tartalom több változót kell.  A **applicationHost.config**, hello helyen található összes alkalmazás érinti, de felülbírálhatók **web.config** hello alkalmazások található fájlokat.

hello következő XML példáját mutatja be a beállítás **clientCache** toospecify a 3 nap maximális élettartama:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Adja meg **UseMaxAge** ad hozzá egy `Cache-Control: max-age=<nnn>` toohello válasz fejrészét alapján hello érték van megadva a hello **CacheControlMaxAge** attribútum. hello hello timespan formátuma a hello **cacheControlMaxAge** attribútum <days>.<hours>:<min>:<sec>. További információ a hello **clientCache** csomópont, lásd: [Ügyfélgyorsítótár <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>A kód a Cache-Control fejlécek beállítása
Az ASP.NET-alkalmazások is megadhat hello CDN gyorsítótár-viselkedést programozott módon hello beállítása **HttpResponse.Cache** tulajdonság. További információ a hello **HttpResponse.Cache** tulajdonság, lásd: [HttpResponse.Cache tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) és [HttpCachePolicy osztály](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Ha azt szeretné, hogy az ASP.NET tooprogrammatically gyorsítótár-alkalmazás tartalmát, győződjön meg arról, hogy hello tartalom van megjelölve, kérelmeznék HttpCacheability túl beállításával*nyilvános*. Emellett győződjön meg arról, hogy a gyorsítótár érvényesítő van-e állítva. hello gyorsítótár érvényesítő utolsó módosítás lehet SetLastModified, vagy az egy etag érték beállítása időbélyeg beállítása SetETag meghívásával. Másik lehetőségként azt is megadhatja a gyorsítótár lejárati ideje SetExpires meghívásával, vagy hello alapértelmezett gyorsítótár heurisztikus lásd a jelen dokumentum korábbi számíthat.  

Például egy órára toocache tartalom adja hozzá a hello következő:  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Következő lépések
* [Olvassa el a hello adatait **clientCache** elem](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Hello hello dokumentációjából **HttpResponse.Cache** tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Hello hello dokumentációjából **HttpCachePolicy osztály**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

