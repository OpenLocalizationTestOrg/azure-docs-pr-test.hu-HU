---
title: "-Microsoft fenyegetések modellezése eszköz - kezelés Azure aaaException |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 247096c10deeca94ebb9b19df7ba60e442ca1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-exception-management--mitigations"></a><span data-ttu-id="00897-103">Biztonsági keret: Kivételek kezelése |} Megoldást</span><span class="sxs-lookup"><span data-stu-id="00897-103">Security Frame: Exception Management | Mitigations</span></span> 
| <span data-ttu-id="00897-104">A termék vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="00897-104">Product/Service</span></span> | <span data-ttu-id="00897-105">Cikk</span><span class="sxs-lookup"><span data-stu-id="00897-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="00897-106">**WCF**</span><span class="sxs-lookup"><span data-stu-id="00897-106">**WCF**</span></span> | <ul><li>[<span data-ttu-id="00897-107">WCF - ne szerepeljen az serviceDebug csomópont a konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="00897-107">WCF- Do not include serviceDebug node in configuration file</span></span>](#servicedebug)</li><li>[<span data-ttu-id="00897-108">WCF - ne szerepeljen az serviceMetadata csomópont a konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="00897-108">WCF- Do not include serviceMetadata node in configuration file</span></span>](#servicemetadata)</li></ul> |
| <span data-ttu-id="00897-109">**Webes API**</span><span class="sxs-lookup"><span data-stu-id="00897-109">**Web API**</span></span> | <ul><li>[<span data-ttu-id="00897-110">Győződjön meg arról, hogy a megfelelő kivételkezelést történik-e az ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="00897-110">Ensure that proper exception handling is done in ASP.NET Web API </span></span>](#exception)</li></ul> |
| <span data-ttu-id="00897-111">**Webalkalmazás**</span><span class="sxs-lookup"><span data-stu-id="00897-111">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="00897-112">Nem teszi közzé a látható a hibaüzenetekben biztonsági részletei</span><span class="sxs-lookup"><span data-stu-id="00897-112">Do not expose security details in error messages </span></span>](#messages)</li><li>[<span data-ttu-id="00897-113">Alapértelmezett hibakezelési lap végrehajtása</span><span class="sxs-lookup"><span data-stu-id="00897-113">Implement Default error handling page </span></span>](#default)</li><li>[<span data-ttu-id="00897-114">Az üzembe helyezési módszer tooRetail beállítása az IIS-ben</span><span class="sxs-lookup"><span data-stu-id="00897-114">Set Deployment Method tooRetail in IIS</span></span>](#deployment)</li><li>[<span data-ttu-id="00897-115">Kivételek biztonságosan kell-e sikertelen</span><span class="sxs-lookup"><span data-stu-id="00897-115">Exceptions should fail safely</span></span>](#fail)</li></ul> |

## <span data-ttu-id="00897-116"><a id="servicedebug"></a>WCF - ne szerepeljen az serviceDebug csomópont a konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="00897-116"><a id="servicedebug"></a>WCF- Do not include serviceDebug node in configuration file</span></span>

| <span data-ttu-id="00897-117">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-117">Title</span></span>                   | <span data-ttu-id="00897-118">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-118">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-119">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-119">**Component**</span></span>               | <span data-ttu-id="00897-120">WCF</span><span class="sxs-lookup"><span data-stu-id="00897-120">WCF</span></span> | 
| <span data-ttu-id="00897-121">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-121">**SDL Phase**</span></span>               | <span data-ttu-id="00897-122">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-122">Build</span></span> |  
| <span data-ttu-id="00897-123">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-123">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-124">Általános, NET-keretrendszer 3</span><span class="sxs-lookup"><span data-stu-id="00897-124">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="00897-125">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-125">**Attributes**</span></span>              | <span data-ttu-id="00897-126">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-126">N/A</span></span>  |
| <span data-ttu-id="00897-127">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-127">**References**</span></span>              | <span data-ttu-id="00897-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="00897-128">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="00897-129">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-129">**Steps**</span></span> | <span data-ttu-id="00897-130">A Windows kommunikációs keretrendszer (WCF) szolgáltatásait konfigurált tooexpose hibakeresési információ is lehet.</span><span class="sxs-lookup"><span data-stu-id="00897-130">Windows Communication Framework (WCF) services can be configured tooexpose debugging information.</span></span> <span data-ttu-id="00897-131">Hibakeresési információ nem használható üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="00897-131">Debug information should not be used in production environments.</span></span> <span data-ttu-id="00897-132">Hello `<serviceDebug>` címke határozza meg, hogy hello hibakeresési információ szolgáltatás engedélyezve van-e a WCF-szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="00897-132">hello `<serviceDebug>` tag defines whether hello debug information feature is enabled for a WCF service.</span></span> <span data-ttu-id="00897-133">Ha hello attribútum includeExceptionDetailInFaults tootrue, kivételek adatai hello alkalmazásból visszaadott tooclients.</span><span class="sxs-lookup"><span data-stu-id="00897-133">If hello attribute includeExceptionDetailInFaults is set tootrue, exception information from hello application will be returned tooclients.</span></span> <span data-ttu-id="00897-134">A támadók kihasználhatják a hello további információt a kimeneti toomount támadások megcélzott hello keretrendszer, adatbázisok vagy más hello alkalmazás által használt erőforrások hibakeresés kapnak.</span><span class="sxs-lookup"><span data-stu-id="00897-134">Attackers can leverage hello additional information they gain from debugging output toomount attacks targeted on hello framework, database, or other resources used by hello application.</span></span> |

### <a name="example"></a><span data-ttu-id="00897-135">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-135">Example</span></span>
<span data-ttu-id="00897-136">hello következő konfigurációs fájl tartalmaz hello `<serviceDebug>` címke:</span><span class="sxs-lookup"><span data-stu-id="00897-136">hello following configuration file includes hello `<serviceDebug>` tag:</span></span> 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
<span data-ttu-id="00897-137">Tiltsa le a hibakeresési adatok hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="00897-137">Disable debugging information in hello service.</span></span> <span data-ttu-id="00897-138">Hello eltávolításával ehhez `<serviceDebug>` címke az alkalmazás konfigurációs fájljából.</span><span class="sxs-lookup"><span data-stu-id="00897-138">This can be accomplished by removing hello `<serviceDebug>` tag from your application's configuration file.</span></span> 

## <span data-ttu-id="00897-139"><a id="servicemetadata"></a>WCF - ne szerepeljen az serviceMetadata csomópont a konfigurációs fájl</span><span class="sxs-lookup"><span data-stu-id="00897-139"><a id="servicemetadata"></a>WCF- Do not include serviceMetadata node in configuration file</span></span>

| <span data-ttu-id="00897-140">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-140">Title</span></span>                   | <span data-ttu-id="00897-141">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-141">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-142">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-142">**Component**</span></span>               | <span data-ttu-id="00897-143">WCF</span><span class="sxs-lookup"><span data-stu-id="00897-143">WCF</span></span> | 
| <span data-ttu-id="00897-144">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-144">**SDL Phase**</span></span>               | <span data-ttu-id="00897-145">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-145">Build</span></span> |  
| <span data-ttu-id="00897-146">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-146">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-147">Általános</span><span class="sxs-lookup"><span data-stu-id="00897-147">Generic</span></span> |
| <span data-ttu-id="00897-148">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-148">**Attributes**</span></span>              | <span data-ttu-id="00897-149">Általános, NET-keretrendszer 3</span><span class="sxs-lookup"><span data-stu-id="00897-149">Generic, NET Framework 3</span></span> |
| <span data-ttu-id="00897-150">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-150">**References**</span></span>              | <span data-ttu-id="00897-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [erősítse meg Királyság](https://vulncat.fortify.com/en/vulncat/index.html)</span><span class="sxs-lookup"><span data-stu-id="00897-151">[MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html)</span></span> |
| <span data-ttu-id="00897-152">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-152">**Steps**</span></span> | <span data-ttu-id="00897-153">Nyilvánosan kitettségének szolgáltatás információt biztosíthat támadók számára azok hogyan hello szolgáltatást is elegendő értékes betekintést.</span><span class="sxs-lookup"><span data-stu-id="00897-153">Publicly exposing information about a service can provide attackers with valuable insight into how they might exploit hello service.</span></span> <span data-ttu-id="00897-154">Hello `<serviceMetadata>` címke engedélyezi hello metaadatok közzétételi szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="00897-154">hello `<serviceMetadata>` tag enables hello metadata publishing feature.</span></span> <span data-ttu-id="00897-155">Szolgáltatás metaadatai tartalmazhatnak bizalmas adatokat, amelyeket nem kell nyilvánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="00897-155">Service metadata could contain sensitive information that should not be publicly accessible.</span></span> <span data-ttu-id="00897-156">Minimális csak a megbízható felhasználóknak engedélyezze tooaccess hello metaadatok, és győződjön meg arról, hogy a szükségtelen adatokat nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="00897-156">At a minimum, only allow trusted users tooaccess hello metadata and ensure that unnecessary information is not exposed.</span></span> <span data-ttu-id="00897-157">Még jobb teljes mértékben letiltása hello képességét toopublish metaadatok.</span><span class="sxs-lookup"><span data-stu-id="00897-157">Better yet, entirely disable hello ability toopublish metadata.</span></span> <span data-ttu-id="00897-158">A biztonságos WCF-konfiguráció nem tartalmaz hello `<serviceMetadata>` címke.</span><span class="sxs-lookup"><span data-stu-id="00897-158">A safe WCF configuration will not contain hello `<serviceMetadata>` tag.</span></span> |

## <span data-ttu-id="00897-159"><a id="exception"></a>Győződjön meg arról, hogy a megfelelő kivételkezelést történik-e az ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="00897-159"><a id="exception"></a>Ensure that proper exception handling is done in ASP.NET Web API</span></span>

| <span data-ttu-id="00897-160">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-160">Title</span></span>                   | <span data-ttu-id="00897-161">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-162">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-162">**Component**</span></span>               | <span data-ttu-id="00897-163">Webes API</span><span class="sxs-lookup"><span data-stu-id="00897-163">Web API</span></span> | 
| <span data-ttu-id="00897-164">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-164">**SDL Phase**</span></span>               | <span data-ttu-id="00897-165">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-165">Build</span></span> |  
| <span data-ttu-id="00897-166">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-166">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-167">MVC 5, 6 MVC</span><span class="sxs-lookup"><span data-stu-id="00897-167">MVC 5, MVC 6</span></span> |
| <span data-ttu-id="00897-168">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-168">**Attributes**</span></span>              | <span data-ttu-id="00897-169">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-169">N/A</span></span>  |
| <span data-ttu-id="00897-170">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-170">**References**</span></span>              | <span data-ttu-id="00897-171">[ASP.NET webes API-t a kivételkezelő](http://www.asp.net/web-api/overview/error-handling/exception-handling), [-ellenőrzés az ASP.NET Web API minta](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span><span class="sxs-lookup"><span data-stu-id="00897-171">[Exception Handling in ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model Validation in ASP.NET Web API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api)</span></span> |
| <span data-ttu-id="00897-172">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-172">**Steps**</span></span> | <span data-ttu-id="00897-173">Alapértelmezés szerint az ASP.NET Web API leginkább a nem kezelt kivételek lefordítását egy HTTP-válasz állapotkód:`500, Internal Server Error`</span><span class="sxs-lookup"><span data-stu-id="00897-173">By default, most uncaught exceptions in ASP.NET Web API are translated into an HTTP response with status code `500, Internal Server Error`</span></span>|

### <a name="example"></a><span data-ttu-id="00897-174">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-174">Example</span></span>
<span data-ttu-id="00897-175">toocontrol hello hello API-t által visszaadott állapotkód `HttpResponseException` alább látható módon használhatja:</span><span class="sxs-lookup"><span data-stu-id="00897-175">toocontrol hello status code returned by hello API, `HttpResponseException` can be used as shown below:</span></span> 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a><span data-ttu-id="00897-176">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-176">Example</span></span>
<span data-ttu-id="00897-177">Hello kivétel válasz további vezérlő hello `HttpResponseMessage` osztály használható a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="00897-177">For further control on hello exception response, hello `HttpResponseMessage` class can be used as shown below:</span></span> 
```C#
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
<span data-ttu-id="00897-178">toocatch kezeletlen kivételek fellépése, amelyek nincsenek hello típusú `HttpResponseException`, kivétel szűrők segítségével.</span><span class="sxs-lookup"><span data-stu-id="00897-178">toocatch unhandled exceptions that are not of hello type `HttpResponseException`, Exception Filters can be used.</span></span> <span data-ttu-id="00897-179">Kivétel szűrők megvalósítása hello `System.Web.Http.Filters.IExceptionFilter` felületet.</span><span class="sxs-lookup"><span data-stu-id="00897-179">Exception filters implement hello `System.Web.Http.Filters.IExceptionFilter` interface.</span></span> <span data-ttu-id="00897-180">hello legegyszerűbb módja toowrite egy kivételszűrőbe a hello tooderive `System.Web.Http.Filters.ExceptionFilterAttribute` osztályhoz, és írja felül hello OnException metódust.</span><span class="sxs-lookup"><span data-stu-id="00897-180">hello simplest way toowrite an exception filter is tooderive from hello `System.Web.Http.Filters.ExceptionFilterAttribute` class and override hello OnException method.</span></span> 

### <a name="example"></a><span data-ttu-id="00897-181">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-181">Example</span></span>
<span data-ttu-id="00897-182">Íme egy szűrő, amely átalakítja `NotImplementedException` kivételek be HTTP-állapotkód `501, Not Implemented`:</span><span class="sxs-lookup"><span data-stu-id="00897-182">Here is a filter that converts `NotImplementedException` exceptions into HTTP status code `501, Not Implemented`:</span></span> 
```C#
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

<span data-ttu-id="00897-183">Számos módon tooregister egy webes API kivételszűrőbe van:</span><span class="sxs-lookup"><span data-stu-id="00897-183">There are several ways tooregister a Web API exception filter:</span></span>
- <span data-ttu-id="00897-184">Művelet</span><span class="sxs-lookup"><span data-stu-id="00897-184">By action</span></span>
- <span data-ttu-id="00897-185">Vezérlő</span><span class="sxs-lookup"><span data-stu-id="00897-185">By controller</span></span>
- <span data-ttu-id="00897-186">Globálisan</span><span class="sxs-lookup"><span data-stu-id="00897-186">Globally</span></span>

### <a name="example"></a><span data-ttu-id="00897-187">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-187">Example</span></span>
<span data-ttu-id="00897-188">tooapply hello tooa bizonyos művelet szűréséhez hello szűrő attribútum toohello művelet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="00897-188">tooapply hello filter tooa specific action, add hello filter as an attribute toohello action:</span></span> 
```C#
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a><span data-ttu-id="00897-189">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-189">Example</span></span>
<span data-ttu-id="00897-190">tooapply hello szűrő tooall hello műveletek egy `controller`, adja hozzá hello szűrő egy attribútum toohello `controller` osztály:</span><span class="sxs-lookup"><span data-stu-id="00897-190">tooapply hello filter tooall of hello actions on a `controller`, add hello filter as an attribute toohello `controller` class:</span></span> 

```C#
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a><span data-ttu-id="00897-191">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-191">Example</span></span>
<span data-ttu-id="00897-192">tooapply hello szűrése globálisan tooall Web API tartományvezérlőket, adjon hozzá egy hello szűrő toohello `GlobalConfiguration.Configuration.Filters` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="00897-192">tooapply hello filter globally tooall Web API controllers, add an instance of hello filter toohello `GlobalConfiguration.Configuration.Filters` collection.</span></span> <span data-ttu-id="00897-193">A gyűjtemény kivételszűrők tooany Web API vezérlő műveletének alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="00897-193">Exception filters in this collection apply tooany Web API controller action.</span></span> 
```C#
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a><span data-ttu-id="00897-194">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-194">Example</span></span>
<span data-ttu-id="00897-195">A modell-ellenőrzéshez hello modell állapota átadhatók tooCreateErrorResponse metódus alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="00897-195">For model validation, hello model state can be passed tooCreateErrorResponse method as shown below:</span></span> 
```C#
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

<span data-ttu-id="00897-196">Hello references szakaszában kivételes kezelésére és a modell-ellenőrzés az ASP.Net Web API kapcsolatos további részletekért ellenőrizze hello hivatkozások</span><span class="sxs-lookup"><span data-stu-id="00897-196">Check hello links in hello references section for additional details about exceptional handling and model validation in ASP.Net Web API</span></span> 

## <span data-ttu-id="00897-197"><a id="messages"></a>Nem teszi közzé a látható a hibaüzenetekben biztonsági részletei</span><span class="sxs-lookup"><span data-stu-id="00897-197"><a id="messages"></a>Do not expose security details in error messages</span></span>

| <span data-ttu-id="00897-198">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-198">Title</span></span>                   | <span data-ttu-id="00897-199">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-199">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-200">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-200">**Component**</span></span>               | <span data-ttu-id="00897-201">Web Application</span><span class="sxs-lookup"><span data-stu-id="00897-201">Web Application</span></span> | 
| <span data-ttu-id="00897-202">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-202">**SDL Phase**</span></span>               | <span data-ttu-id="00897-203">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-203">Build</span></span> |  
| <span data-ttu-id="00897-204">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-204">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-205">Általános</span><span class="sxs-lookup"><span data-stu-id="00897-205">Generic</span></span> |
| <span data-ttu-id="00897-206">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-206">**Attributes**</span></span>              | <span data-ttu-id="00897-207">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-207">N/A</span></span>  |
| <span data-ttu-id="00897-208">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-208">**References**</span></span>              | <span data-ttu-id="00897-209">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-209">N/A</span></span>  |
| <span data-ttu-id="00897-210">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-210">**Steps**</span></span> | <p><span data-ttu-id="00897-211">Általános hiba üzenetek biztosított közvetlenül toohello felhasználó nélkül az alkalmazás bizalmas adatokat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="00897-211">Generic error messages are provided directly toohello user without including sensitive application data.</span></span> <span data-ttu-id="00897-212">Bizalmas adatok közé tartoznak például:</span><span class="sxs-lookup"><span data-stu-id="00897-212">Examples of sensitive data include:</span></span></p><ul><li><span data-ttu-id="00897-213">Kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="00897-213">Server names</span></span></li><li><span data-ttu-id="00897-214">Kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="00897-214">Connection strings</span></span></li><li><span data-ttu-id="00897-215">Felhasználónevek</span><span class="sxs-lookup"><span data-stu-id="00897-215">Usernames</span></span></li><li><span data-ttu-id="00897-216">Jelszavak</span><span class="sxs-lookup"><span data-stu-id="00897-216">Passwords</span></span></li><li><span data-ttu-id="00897-217">SQL-eljárások</span><span class="sxs-lookup"><span data-stu-id="00897-217">SQL procedures</span></span></li><li><span data-ttu-id="00897-218">A dinamikus SQL-hibák részletei</span><span class="sxs-lookup"><span data-stu-id="00897-218">Details of dynamic SQL failures</span></span></li><li><span data-ttu-id="00897-219">A veremkivonatot és sornyi kód</span><span class="sxs-lookup"><span data-stu-id="00897-219">Stack trace and lines of code</span></span></li><li><span data-ttu-id="00897-220">A memóriában tárolt változók</span><span class="sxs-lookup"><span data-stu-id="00897-220">Variables stored in memory</span></span></li><li><span data-ttu-id="00897-221">Meghajtó és mappa</span><span class="sxs-lookup"><span data-stu-id="00897-221">Drive and folder locations</span></span></li><li><span data-ttu-id="00897-222">Alkalmazás telepítése pontok</span><span class="sxs-lookup"><span data-stu-id="00897-222">Application install points</span></span></li><li><span data-ttu-id="00897-223">Gazdagép konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="00897-223">Host configuration settings</span></span></li><li><span data-ttu-id="00897-224">Egyéb részleteket a belső alkalmazás</span><span class="sxs-lookup"><span data-stu-id="00897-224">Other internal application details</span></span></li></ul><p><span data-ttu-id="00897-225">Az alkalmazáson belül az összes hiba túltöltés és nyújtó általános hibaüzenetek, valamint IIS belül az egyéni hibák engedélyezése segítségével információ felfedésének megakadályozására.</span><span class="sxs-lookup"><span data-stu-id="00897-225">Trapping all errors within an application and providing generic error messages, as well as enabling custom errors within IIS will help prevent information disclosure.</span></span> <span data-ttu-id="00897-226">SQL Server-adatbázis és a .NET kivételkezelő, más hibakezelési architektúrák mellett között is a különösen részletes és rendkívül hasznos tooa rosszindulatú felhasználó adatainak összegyűjtése az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00897-226">SQL Server database and .NET Exception handling, among other error handling architectures, are especially verbose and extremely useful tooa malicious user profiling your application.</span></span> <span data-ttu-id="00897-227">Akkor hajtsa végre, nem közvetlenül a megjelenítési hello osztály tartalmát hello .NET kivétel osztályból származó, és győződjön meg arról, hogy megfelelő kivételkezelést, hogy véletlenül nem váratlan kivétel kiváltása közvetlenül toohello felhasználói.</span><span class="sxs-lookup"><span data-stu-id="00897-227">Do not directly display hello contents of a class derived from hello .NET Exception class, and ensure that you have proper exception handling so that an unexpected exception isn't inadvertently raised directly toohello user.</span></span></p><ul><li><span data-ttu-id="00897-228">Adja meg, általános hibaüzenet közvetlenül található közvetlenül a hello kivétel/hibaüzenet jelenik meg rögtön részletes absztrakt toohello felhasználó</span><span class="sxs-lookup"><span data-stu-id="00897-228">Provide generic error messages directly toohello user that abstract away specific details found directly in hello exception/error message</span></span></li><li><span data-ttu-id="00897-229">Tegye nem megjelenítési hello tartalmát a .NET-kivételt osztályban közvetlenül toohello felhasználói</span><span class="sxs-lookup"><span data-stu-id="00897-229">Do not display hello contents of a .NET exception class directly toohello user</span></span></li><li><span data-ttu-id="00897-230">Feldolgozzák-e az összes hibaüzenet, és adott esetben tájékoztatják egy általános hiba üzenetet küldött toohello alkalmazás ügyfél hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="00897-230">Trap all error messages and if appropriate inform hello user via a generic error message sent toohello application client</span></span></li><li><span data-ttu-id="00897-231">Nem teszi közzé hello kivételosztály hello tartalmát közvetlenül toohello felhasználói, különösen akkor hello visszatérési érték a `.ToString()`, vagy hello üzenet vagy Veremkivonat tulajdonságok hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="00897-231">Do not expose hello contents of hello Exception class directly toohello user, especially hello return value from `.ToString()`, or hello values of hello Message or StackTrace properties.</span></span> <span data-ttu-id="00897-232">Biztonságosan jelentkezzen ezt az információt, és a több ártalmatlan üzenet toohello felhasználó megjelenítése</span><span class="sxs-lookup"><span data-stu-id="00897-232">Securely log this information and display a more innocuous message toohello user</span></span></li></ul>|

## <span data-ttu-id="00897-233"><a id="default"></a>Alapértelmezett hibakezelési lap végrehajtása</span><span class="sxs-lookup"><span data-stu-id="00897-233"><a id="default"></a>Implement Default error handling page</span></span>

| <span data-ttu-id="00897-234">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-234">Title</span></span>                   | <span data-ttu-id="00897-235">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-235">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-236">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-236">**Component**</span></span>               | <span data-ttu-id="00897-237">Web Application</span><span class="sxs-lookup"><span data-stu-id="00897-237">Web Application</span></span> | 
| <span data-ttu-id="00897-238">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-238">**SDL Phase**</span></span>               | <span data-ttu-id="00897-239">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-239">Build</span></span> |  
| <span data-ttu-id="00897-240">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-240">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-241">Általános</span><span class="sxs-lookup"><span data-stu-id="00897-241">Generic</span></span> |
| <span data-ttu-id="00897-242">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-242">**Attributes**</span></span>              | <span data-ttu-id="00897-243">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-243">N/A</span></span>  |
| <span data-ttu-id="00897-244">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-244">**References**</span></span>              | <span data-ttu-id="00897-245">[Az ASP.NET lapok beállításainak szerkesztése párbeszédpanel](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="00897-245">[Edit ASP.NET Error Pages Settings Dialog Box](https://technet.microsoft.com/library/dd569096(WS.10).aspx)</span></span> |
| <span data-ttu-id="00897-246">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-246">**Steps**</span></span> | <p><span data-ttu-id="00897-247">Az ASP.NET-alkalmazások sikertelen lesz, és HTTP/1.x 500 belső kiszolgáló hibát okoz, vagy egy kapcsolószolgáltatás-konfigurációja (például kérelmek szűrése) esetén a lap nem jelenik meg, amikor egy hibaüzenetet fog készülni.</span><span class="sxs-lookup"><span data-stu-id="00897-247">When an ASP.NET application fails and causes an HTTP/1.x 500 Internal Server Error, or a feature configuration (such as Request Filtering) prevents a page from being displayed, an error message will be generated.</span></span> <span data-ttu-id="00897-248">Rendszergazdák kiválaszthatják, függetlenül attól, hello alkalmazás egy barátságos üzenet toohello ügyfél, a hibával kapcsolatos részletes üzenet toohello ügyfél vagy a csak a hibával kapcsolatos részletes üzenet toolocalhost megjelenjen-e.</span><span class="sxs-lookup"><span data-stu-id="00897-248">Administrators can choose whether or not hello application should display a friendly message toohello client, detailed error message toohello client, or detailed error message toolocalhost only.</span></span> <span data-ttu-id="00897-249">Hello <customErrors> hello web.config-címke szolgáltatásnak három módja van:</span><span class="sxs-lookup"><span data-stu-id="00897-249">hello <customErrors> tag in hello web.config has three modes:</span></span></p><ul><li><span data-ttu-id="00897-250">**A:** határozza meg, hogy engedélyezve vannak-e az egyéni hibák.</span><span class="sxs-lookup"><span data-stu-id="00897-250">**On:** Specifies that custom errors are enabled.</span></span> <span data-ttu-id="00897-251">Ha nem defaultRedirect attribútum meg van adva, a felhasználók látni egy általános hiba.</span><span class="sxs-lookup"><span data-stu-id="00897-251">If no defaultRedirect attribute is specified, users see a generic error.</span></span> <span data-ttu-id="00897-252">hello az egyéni hibák toohello távoli ügyfelek és a helyi állomás toohello láthatók.</span><span class="sxs-lookup"><span data-stu-id="00897-252">hello custom errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="00897-253">**OFF:** határozza meg, hogy van-e tiltva az egyéni hibák.</span><span class="sxs-lookup"><span data-stu-id="00897-253">**Off:** Specifies that custom errors are disabled.</span></span> <span data-ttu-id="00897-254">hello részletes ASP.NET hibák jelennek meg toohello távoli ügyfelek és a helyi állomás toohello</span><span class="sxs-lookup"><span data-stu-id="00897-254">hello detailed ASP.NET errors are shown toohello remote clients and toohello local host</span></span></li><li><span data-ttu-id="00897-255">**RemoteOnly:** Megadja, hogy megjelennek-e az egyéni hibák csak toohello távoli ügyfelek és, hogy megjelennek-e az ASP.NET-hibákat toohello helyi állomás.</span><span class="sxs-lookup"><span data-stu-id="00897-255">**RemoteOnly:** Specifies that custom errors are shown only toohello remote clients, and that ASP.NET errors are shown toohello local host.</span></span> <span data-ttu-id="00897-256">Ez az alapértelmezett érték hello</span><span class="sxs-lookup"><span data-stu-id="00897-256">This is hello default value</span></span></li></ul><p><span data-ttu-id="00897-257">Nyissa meg hello `web.config` hello alkalmazás vagy hely fájlt, és ügyeljen arra, hogy hello címke vagy `<customErrors mode="RemoteOnly" />` vagy `<customErrors mode="On" />` definiálva.</span><span class="sxs-lookup"><span data-stu-id="00897-257">Open hello `web.config` file for hello application/site and ensure that hello tag has either `<customErrors mode="RemoteOnly" />` or `<customErrors mode="On" />` defined.</span></span></p>|

## <span data-ttu-id="00897-258"><a id="deployment"></a>Az üzembe helyezési módszer tooRetail beállítása az IIS-ben</span><span class="sxs-lookup"><span data-stu-id="00897-258"><a id="deployment"></a>Set Deployment Method tooRetail in IIS</span></span>

| <span data-ttu-id="00897-259">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-259">Title</span></span>                   | <span data-ttu-id="00897-260">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-260">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-261">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-261">**Component**</span></span>               | <span data-ttu-id="00897-262">Web Application</span><span class="sxs-lookup"><span data-stu-id="00897-262">Web Application</span></span> | 
| <span data-ttu-id="00897-263">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-263">**SDL Phase**</span></span>               | <span data-ttu-id="00897-264">Környezet</span><span class="sxs-lookup"><span data-stu-id="00897-264">Deployment</span></span> |  
| <span data-ttu-id="00897-265">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-265">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-266">Általános</span><span class="sxs-lookup"><span data-stu-id="00897-266">Generic</span></span> |
| <span data-ttu-id="00897-267">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-267">**Attributes**</span></span>              | <span data-ttu-id="00897-268">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-268">N/A</span></span>  |
| <span data-ttu-id="00897-269">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-269">**References**</span></span>              | <span data-ttu-id="00897-270">[központi telepítés elem (ASP.NET beállítási séma)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span><span class="sxs-lookup"><span data-stu-id="00897-270">[deployment Element (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx)</span></span> |
| <span data-ttu-id="00897-271">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-271">**Steps**</span></span> | <p><span data-ttu-id="00897-272">Hello `<deployment retail>` kapcsoló éles IIS-kiszolgálókkal való használatra készült.</span><span class="sxs-lookup"><span data-stu-id="00897-272">hello `<deployment retail>` switch is intended for use by production IIS servers.</span></span> <span data-ttu-id="00897-273">A kapcsoló használt toohelp alkalmazások hello legjobb lehetséges teljesítménnyel futnak, és legalább lehetséges biztonsági információk letiltásával szivárgást hello alkalmazás képes toogenerate nyomkövetés tiltása hello képességét toodisplay lapon a hiba részletei üzenetek tooend felhasználók és letiltását hello hibakeresési kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="00897-273">This switch is used toohelp applications run with hello best possible performance and least possible security information leakages by disabling hello application's ability toogenerate trace output on a page, disabling hello ability toodisplay detailed error messages tooend users, and disabling hello debug switch.</span></span></p><p><span data-ttu-id="00897-274">Gyakran, a kapcsolók és a kapcsolókról developer-arra irányul, például a sikertelen kérelmek nyomkövetésére vonatkozó és a hibakeresést, aktív fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="00897-274">Often times, switches and options that are developer-focused, such as failed request tracing and debugging, are enabled during active development.</span></span> <span data-ttu-id="00897-275">Javasoljuk, hogy bármely üzemi kiszolgálón az üzembe helyezési módszer hello tooretail állítható be.</span><span class="sxs-lookup"><span data-stu-id="00897-275">It is recommended that hello deployment method on any production server be set tooretail.</span></span> <span data-ttu-id="00897-276">Nyissa meg a hello machine.config fájl, és győződjön meg arról, hogy `<deployment retail="true" />` tootrue értéke.</span><span class="sxs-lookup"><span data-stu-id="00897-276">open hello machine.config file and ensure that `<deployment retail="true" />` remains set tootrue.</span></span></p>|

## <span data-ttu-id="00897-277"><a id="fail"></a>Kivételek biztonságosan kell-e sikertelen</span><span class="sxs-lookup"><span data-stu-id="00897-277"><a id="fail"></a>Exceptions should fail safely</span></span>

| <span data-ttu-id="00897-278">Cím</span><span class="sxs-lookup"><span data-stu-id="00897-278">Title</span></span>                   | <span data-ttu-id="00897-279">Részletek</span><span class="sxs-lookup"><span data-stu-id="00897-279">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="00897-280">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="00897-280">**Component**</span></span>               | <span data-ttu-id="00897-281">Web Application</span><span class="sxs-lookup"><span data-stu-id="00897-281">Web Application</span></span> | 
| <span data-ttu-id="00897-282">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="00897-282">**SDL Phase**</span></span>               | <span data-ttu-id="00897-283">Felépítés</span><span class="sxs-lookup"><span data-stu-id="00897-283">Build</span></span> |  
| <span data-ttu-id="00897-284">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="00897-284">**Applicable Technologies**</span></span> | <span data-ttu-id="00897-285">Általános</span><span class="sxs-lookup"><span data-stu-id="00897-285">Generic</span></span> |
| <span data-ttu-id="00897-286">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="00897-286">**Attributes**</span></span>              | <span data-ttu-id="00897-287">N/A</span><span class="sxs-lookup"><span data-stu-id="00897-287">N/A</span></span>  |
| <span data-ttu-id="00897-288">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="00897-288">**References**</span></span>              | [<span data-ttu-id="00897-289">Biztonságos helyen sikertelen</span><span class="sxs-lookup"><span data-stu-id="00897-289">Fail securely</span></span>](https://www.owasp.org/index.php/Fail_securely) |
| <span data-ttu-id="00897-290">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="00897-290">**Steps**</span></span> | <span data-ttu-id="00897-291">Alkalmazás biztonságosan sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="00897-291">Application should fail safely.</span></span> <span data-ttu-id="00897-292">Bármelyik módszert, amely visszaadja az egy logikai érték, mely bizonyos döntés, alapján gondosan létrehozott kivételblokk kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="00897-292">Any method that returns a Boolean value, based on which certain decision is made, should have exception block carefully created.</span></span> <span data-ttu-id="00897-293">Nincsenek nagy mennyiségű logikai hibák miatt toowhich biztonsági problémákat kötés a, ha hello kivételblokk carelessly ír.</span><span class="sxs-lookup"><span data-stu-id="00897-293">There are lot of logical errors due toowhich security issues creep in, when hello exception block is written carelessly.</span></span>|

### <a name="example"></a><span data-ttu-id="00897-294">Példa</span><span class="sxs-lookup"><span data-stu-id="00897-294">Example</span></span>
```C#
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check tooenable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
<span data-ttu-id="00897-295">hello fent metódus mindig ad vissza IGAZ, ha kivétel történik.</span><span class="sxs-lookup"><span data-stu-id="00897-295">hello above method will always return True, if some exception happens.</span></span> <span data-ttu-id="00897-296">Ha hello végfelhasználói biztosít egy nem megfelelően formázott URL-címet, amely hello böngésző tekintetben, de hello `Uri()` konstruktor nem, a rendszer kivételt jelez, és hello áldozata toohello érvényes, de nem megfelelően formázott URL-címet kell tenni.</span><span class="sxs-lookup"><span data-stu-id="00897-296">If hello end user provides a malformed URL, that hello browser respects, but hello `Uri()` constructor doesn't, this will throw an exception, and hello victim will be taken toohello valid but malformed URL.</span></span> 
