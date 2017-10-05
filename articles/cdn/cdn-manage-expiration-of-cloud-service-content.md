---
title: "Az Azure CDN webes tartalom lejáratának kezelése |} Microsoft Docs"
description: "Útmutató az Azure CDN Azure Web Apps/Cloud Services, az ASP.NET, vagy az IIS tartalom lejáratának kezelése."
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
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a><span data-ttu-id="25777-103">Az Azure CDN Azure Web Apps/Cloud Services, az ASP.NET, vagy az IIS tartalom lejáratának kezelése</span><span class="sxs-lookup"><span data-stu-id="25777-103">Manage expiration of Azure Web Apps/Cloud Services, ASP.NET, or IIS content in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="25777-104">Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS</span><span class="sxs-lookup"><span data-stu-id="25777-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="25777-105">Azure Storage-blob szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="25777-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="25777-106">Fájlok forráskiszolgálóról bármely nyilvánosan elérhető webkiszolgáló Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="25777-106">Files from any publicly accessible origin web server can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="25777-107">Az élettartam határozza meg a [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) az a HTTP-válasz a forráskiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="25777-107">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from the origin server.</span></span>  <span data-ttu-id="25777-108">Ez a cikk ismerteti, hogyan telepíthet `Cache-Control` fejlécek Azure Web Apps, Azure Cloud Services, az ASP.NET-alkalmazások és az Internet Information Services helyeket, amelyek hasonló módon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="25777-108">This article describes how to set `Cache-Control` headers for Azure Web Apps, Azure Cloud Services, ASP.NET applications, and Internet Information Services sites, all of which are configured similarly.</span></span>

> [!TIP]
> <span data-ttu-id="25777-109">Választhatja a fájl nincs TTL beállítása.</span><span class="sxs-lookup"><span data-stu-id="25777-109">You may choose to set no TTL on a file.</span></span>  <span data-ttu-id="25777-110">Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.</span><span class="sxs-lookup"><span data-stu-id="25777-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="25777-111">Fájlok és egyéb erőforrásokhoz való hozzáférés felgyorsítása érdekében Azure CDN szolgáltatás működésével kapcsolatos további információkért lásd: a [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="25777-111">For more information about how Azure CDN works to speed up access to files and other resources, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a><span data-ttu-id="25777-112">A beállítás a Cache-Control fejlécek konfigurációban</span><span class="sxs-lookup"><span data-stu-id="25777-112">Setting Cache-Control Headers in configuration</span></span>
<span data-ttu-id="25777-113">Statikus tartalom, lemezképek és a stíluslapok, például szabályozhatja a frissítés gyakoriságának módosítása a **applicationHost.config** vagy **web.config** a webes alkalmazás forrásfájljairól.</span><span class="sxs-lookup"><span data-stu-id="25777-113">For static content, such as images and style sheets, you can control the update frequency by modifying the **applicationHost.config** or **web.config** files for your web application.</span></span>  <span data-ttu-id="25777-114">A **system.webServer\staticContent\clientCache** állítja be a konfigurációs fájlban a `Cache-Control` fejlécet a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="25777-114">The **system.webServer\staticContent\clientCache** element in the configuration file will set the `Cache-Control` header for your content.</span></span> <span data-ttu-id="25777-115">A **web.config**, a konfigurációs beállítások hatással lesz minden mappában, és minden almappa, kivéve, ha a almappa szintjén felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="25777-115">For **web.config**, the configuration settings will affect everything in the folder and all subfolders, unless overridden at the subfolder level.</span></span>  <span data-ttu-id="25777-116">Például állítsa be alapértelmezett élő idő a legfelső szintű 3 napig az összes statikus tartalmat rendelkeznie, de rendelkezik egy almappát, a 6 órás gyorsítótár beállítással több változó tartalma.</span><span class="sxs-lookup"><span data-stu-id="25777-116">For example, you can set a default time-to-live at the root to have all static content cached for 3 days, but have a subfolder that has more variable content with a cache setting of 6 hours.</span></span>  <span data-ttu-id="25777-117">A **applicationHost.config**, a hely összes alkalmazás érinti, de felülbírálhatók **web.config** fájlok az alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="25777-117">For **applicationHost.config**, all applications on the site will be affected, but can be overridden in **web.config** files in the applications.</span></span>

<span data-ttu-id="25777-118">A következő XML-beállítás példáját mutatja be **clientCache** adhatja meg a 3 nap maximális élettartama:</span><span class="sxs-lookup"><span data-stu-id="25777-118">The following XML shows an example of setting **clientCache** to specify a maximum age of 3 days:</span></span>  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

<span data-ttu-id="25777-119">Adja meg **UseMaxAge** ad hozzá egy `Cache-Control: max-age=<nnn>` a válaszok fejlécének megadott érték alapján a **CacheControlMaxAge** attribútum.</span><span class="sxs-lookup"><span data-stu-id="25777-119">Specifying **UseMaxAge** adds a `Cache-Control: max-age=<nnn>` header to the response based on the value specified in the **CacheControlMaxAge** attribute.</span></span> <span data-ttu-id="25777-120">A timespan formátuma esetében az **cacheControlMaxAge** attribútum <days>.<hours>:<min>:<sec>.</span><span class="sxs-lookup"><span data-stu-id="25777-120">The format of the timespan is for the **cacheControlMaxAge** attribute is <days>.<hours>:<min>:<sec>.</span></span> <span data-ttu-id="25777-121">További információ a **clientCache** csomópont, lásd: [Ügyfélgyorsítótár <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span><span class="sxs-lookup"><span data-stu-id="25777-121">For more information on the **clientCache** node, see [Client Cache <clientCache>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).</span></span>  

## <a name="setting-cache-control-headers-in-code"></a><span data-ttu-id="25777-122">A kód a Cache-Control fejlécek beállítása</span><span class="sxs-lookup"><span data-stu-id="25777-122">Setting Cache-Control Headers in Code</span></span>
<span data-ttu-id="25777-123">Az ASP.NET-alkalmazások, a CDN-t úgy, hogy programozott módon gyorsítótárazásának állíthatja be a **HttpResponse.Cache** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="25777-123">For ASP.NET applications, you can set the CDN caching behavior programmatically by setting the **HttpResponse.Cache** property.</span></span> <span data-ttu-id="25777-124">További információ a **HttpResponse.Cache** tulajdonság, lásd: [HttpResponse.Cache tulajdonság](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) és [HttpCachePolicy osztály](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="25777-124">For more information on the **HttpResponse.Cache** property, see [HttpResponse.Cache Property](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) and [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

<span data-ttu-id="25777-125">Ha szeretné a programozott módon gyorsítótár alkalmazás tartalmát az ASP.NET, győződjön meg arról, hogy a tartalom van megjelölve, kérelmeznék HttpCacheability értékre állításával *nyilvános*.</span><span class="sxs-lookup"><span data-stu-id="25777-125">If you want to programmatically cache application content in ASP.NET, make sure that the content is marked as cacheable by setting HttpCacheability to *Public*.</span></span> <span data-ttu-id="25777-126">Emellett győződjön meg arról, hogy a gyorsítótár érvényesítő van-e állítva.</span><span class="sxs-lookup"><span data-stu-id="25777-126">Also, ensure that a cache validator is set.</span></span> <span data-ttu-id="25777-127">A gyorsítótár-érvényesítő utolsó módosítás lehet SetLastModified, vagy az egy etag érték beállítása időbélyeg beállítása SetETag meghívásával.</span><span class="sxs-lookup"><span data-stu-id="25777-127">The cache validator can be a Last Modified timestamp set by calling SetLastModified, or an etag value set by calling SetETag.</span></span> <span data-ttu-id="25777-128">Másik lehetőségként azt is megadhatja a gyorsítótár lejárati ideje SetExpires meghívásával, vagy a korábban a jelen dokumentumban ismertetett alapértelmezett gyorsítótár heurisztikus számíthat.</span><span class="sxs-lookup"><span data-stu-id="25777-128">Optionally, you can also specify a cache expiration time by calling SetExpires, or you can rely on the default cache heuristics described earlier in this document.</span></span>  

<span data-ttu-id="25777-129">Például a gyorsítótár tartalmát egy óra, vegye fel a következő:</span><span class="sxs-lookup"><span data-stu-id="25777-129">For example, to cache content for one hour, add the following:</span></span>  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a><span data-ttu-id="25777-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25777-130">Next Steps</span></span>
* [<span data-ttu-id="25777-131">Részletekért olvassa el a **clientCache** elem</span><span class="sxs-lookup"><span data-stu-id="25777-131">Read details about the **clientCache** element</span></span>](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [<span data-ttu-id="25777-132">Olvassa el a dokumentációt a **HttpResponse.Cache** tulajdonság</span><span class="sxs-lookup"><span data-stu-id="25777-132">Read the documentation for the **HttpResponse.Cache** Property</span></span>](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* <span data-ttu-id="25777-133">[Olvassa el a dokumentációt a **HttpCachePolicy osztály**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span><span class="sxs-lookup"><span data-stu-id="25777-133">[Read the documentation for the **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).</span></span>  

