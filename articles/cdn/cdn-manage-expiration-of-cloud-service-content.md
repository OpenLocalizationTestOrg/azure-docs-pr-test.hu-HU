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
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="5ee47-103">Az Azure CDN Azure Web Apps/Cloud Services, az ASP.NET, vagy az IIS tartalom lejáratának kezelése</span><span class="sxs-lookup"><span data-stu-id="5ee47-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ee47-104">Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS</span><span class="sxs-lookup"><span data-stu-id="5ee47-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="5ee47-105">Azure Storage-blob szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5ee47-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="5ee47-106">Fájlok forráskiszolgálóról bármely nyilvánosan elérhető webkiszolgáló Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="5ee47-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="5ee47-107">hello TTL határozza meg hello [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello forráskiszolgálóról hello HTTP válaszként.</span><span class="sxs-lookup"><span data-stu-id="5ee47-107">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from hello origin server.</span></span>  <span data-ttu-id="5ee47-108">Ez a cikk ismerteti, hogyan tooset `Cache-Control` fejlécek Azure Web Apps, Azure Cloud Services, az ASP.NET-alkalmazások és az Internet Information Services helyeket, amelyek hasonló módon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="5ee47-108">This article describes how tooset `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="5ee47-109">Úgy is dönthet, tooset nem TTL-t, a fájl.</span><span class="sxs-lookup"><span data-stu-id="5ee47-109">You may choose tooset no TTL on a file.</span></span>  <span data-ttu-id="5ee47-110">Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.</span><span class="sxs-lookup"><span data-stu-id="5ee47-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="5ee47-111">Hozzáférés toofiles és egyéb erőforrások toospeed Azure CDN szolgáltatás működésével kapcsolatos további információkért lásd: hello [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5ee47-111">For more information about how Azure CDN works toospeed up access toofiles and other resources, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="5ee47-112">A beállítás a Cache-Control fejlécek konfigurációban</span><span class="sxs-lookup"><span data-stu-id="5ee47-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="5ee47-113">Statikus tartalom, lemezképek és a stíluslapok, például szabályozhatja hello frissítési gyakoriság hello módosításával **applicationHost.config** vagy **web.config** a webes alkalmazás forrásfájljairól.</span><span class="sxs-lookup"><span data-stu-id="5ee47-113">For static content, such as images and style sheets, you can control hello update frequency by modifying hello **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="5ee47-114">Hello **system.webServer\staticContent\clientCache** hello konfigurációs fájlban állítja hello `Cache-Control` fejlécet a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="5ee47-114">hello **system.webServer\staticContent\clientCache** element in hello configuration file will set hello `Cache-Control` header for your content.</span></span> <span data-ttu-id="5ee47-115">A **web.config**, hello konfigurációs beállításai hatással lesz minden hello mappában és annak összes almappája, kivéve, ha hello almappa szintjén felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="5ee47-115">For **web.config**, hello configuration settings will affect everything in hello folder and all subfolders, unless overridden at hello subfolder level.</span></span>  <span data-ttu-id="5ee47-116">Például beállíthat egy alapértelmezett élő idő: hello legfelső szintű toohave összes statikus tartalmat 3 napig, de egy almappát, amely rendelkezik a 6 órás gyorsítótár beállítással tartalom több változót kell.</span><span class="sxs-lookup"><span data-stu-id="5ee47-116">For example, you can set a default time-to-live at hello root toohave all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="5ee47-117">A **applicationHost.config**, hello helyen található összes alkalmazás érinti, de felülbírálhatók **web.config** hello alkalmazások található fájlokat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-117">For **applicationHost.config**, all applications on hello site will be affected, but can be overridden in **web.config** files in hello applications.</span></span>

<span data-ttu-id="5ee47-118">hello következő XML példáját mutatja be a beállítás **clientCache** toospecify a 3 nap maximális élettartama:</span><span class="sxs-lookup"><span data-stu-id="5ee47-118">hello following XML shows an example of setting **clientCache** toospecify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="5ee47-119">Adja meg **UseMaxAge** ad hozzá egy `Cache-Control: max-age=<nnn>` toohello válasz fejrészét alapján hello érték van megadva a hello **CacheControlMaxAge** attribútum.</span><span class="sxs-lookup"><span data-stu-id="5ee47-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header toohello response based on hello value specified in hello **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="5ee47-120">hello hello timespan formátuma a hello **cacheControlMaxAge** attribútum <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="5ee47-120">hello format of hello timespan is for hello **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="5ee47-121">További információ a hello **clientCache** csomópont, lásd: [Ügyfélgyorsítótár <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="5ee47-121">For more information on hello **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="5ee47-122">A kód a Cache-Control fejlécek beállítása</span><span class="sxs-lookup"><span data-stu-id="5ee47-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="5ee47-123">Az ASP.NET-alkalmazások is megadhat hello CDN gyorsítótár-viselkedést programozott módon hello beállítása **HttpResponse.Cache** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5ee47-123">For ASP.NET applications, you can set hello CDN caching behavior programmatically by setting hello **HttpResponse.Cache** property.</span></span> <span data-ttu-id="5ee47-124">További információ a hello **HttpResponse.Cache** tulajdonság, lásd: [HttpResponse.Cache tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) és [HttpCachePolicy osztály](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ee47-124">For more information on hello **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="5ee47-125">Ha azt szeretné, hogy az ASP.NET tooprogrammatically gyorsítótár-alkalmazás tartalmát, győződjön meg arról, hogy hello tartalom van megjelölve, kérelmeznék HttpCacheability túl beállításával*nyilvános*.</span><span class="sxs-lookup"><span data-stu-id="5ee47-125">If you want tooprogrammatically cache application content in ASP.NET, make sure that hello content is marked as cacheable by setting HttpCacheability too*Public*.</span></span> <span data-ttu-id="5ee47-126">Emellett győződjön meg arról, hogy a gyorsítótár érvényesítő van-e állítva.</span><span class="sxs-lookup"><span data-stu-id="5ee47-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="5ee47-127">hello gyorsítótár érvényesítő utolsó módosítás lehet SetLastModified, vagy az egy etag érték beállítása időbélyeg beállítása SetETag meghívásával.</span><span class="sxs-lookup"><span data-stu-id="5ee47-127">hello cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="5ee47-128">Másik lehetőségként azt is megadhatja a gyorsítótár lejárati ideje SetExpires meghívásával, vagy hello alapértelmezett gyorsítótár heurisztikus lásd a jelen dokumentum korábbi számíthat.</span><span class="sxs-lookup"><span data-stu-id="5ee47-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on hello default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="5ee47-129">Például egy órára toocache tartalom adja hozzá a hello következő:</span><span class="sxs-lookup"><span data-stu-id="5ee47-129">For example, toocache content for one hour, add hello following:</span></span>  

```csharp
// Set hello caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="5ee47-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ee47-130">Next Steps</span></span>
* [<span data-ttu-id="5ee47-131">Olvassa el a hello adatait **clientCache** elem</span><span class="sxs-lookup"><span data-stu-id="5ee47-131">Read details about hello **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="5ee47-132">Hello hello dokumentációjából **HttpResponse.Cache** tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5ee47-132">Read hello documentation for hello **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="5ee47-133">[Hello hello dokumentációjából **HttpCachePolicy osztály**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ee47-133">[Read hello documentation for hello **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

