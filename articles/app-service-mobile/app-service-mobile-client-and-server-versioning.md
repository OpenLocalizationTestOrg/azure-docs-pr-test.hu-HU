---
title: "a Mobile Apps és a Mobile Services SDK versioning aaaClient és a kiszolgáló |} Microsoft Docs"
description: "A Mobile Services és az Azure Mobile Apps server SDK verzióival való kompatibilitás és az ügyfél SDK-k listája"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a><span data-ttu-id="1d0f2-103">A Mobile Apps és a Mobile Services ügyfél és kiszolgáló versioning</span><span class="sxs-lookup"><span data-stu-id="1d0f2-103">Client and server versioning in Mobile Apps and Mobile Services</span></span>
<span data-ttu-id="1d0f2-104">Azure Mobile Services hello legújabb verziója hello **Mobile Apps** az Azure App Service szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-104">hello latest version of Azure Mobile Services is hello **Mobile Apps** feature of Azure App Service.</span></span>

<span data-ttu-id="1d0f2-105">hello Mobile Apps-ügyfél és kiszolgáló SDK-k a Mobile Services a eredetileg alapulnak, de azok *nem* kompatibilis egymással.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-105">hello Mobile Apps client and server SDKs are originally based on those in Mobile Services, but they are *not* compatible with each other.</span></span>
<span data-ttu-id="1d0f2-106">Ez azt jelenti, hogy kell használnia egy *Mobile Apps* ügyfél SDK rendelkező egy *Mobile Apps* server SDK és ehhez hasonlóan az *Mobile Services*.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-106">That is, you must use a *Mobile Apps* client SDK with a *Mobile Apps* server SDK and similarly for *Mobile Services*.</span></span> <span data-ttu-id="1d0f2-107">Ehhez a szerződéshez hello ügyfél és kiszolgáló SDK-k, által használt különleges fejléc értéke ki `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-107">This contract is enforced through a special header value used by hello client and server SDKs, `ZUMO-API-VERSION`.</span></span>

<span data-ttu-id="1d0f2-108">Megjegyzés: Ha ez a dokumentum hivatkozik tooa *Mobile Services* háttér, nem feltétlenül kell a Mobile Services üzemeltetett toobe.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-108">Note: whenever this document refers tooa *Mobile Services* backend, it does not necessarily need toobe hosted on Mobile Services.</span></span> <span data-ttu-id="1d0f2-109">Most már lehetséges toomigrate egy mobilszolgáltatás toorun az App Service kód módosítások nélkül, de hello szolgáltatás még mindig dolgozna *Mobile Services* SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-109">It is now possible toomigrate a mobile service toorun on App Service without any code changes, but hello service would still be using *Mobile Services*  SDK versions.</span></span>

<span data-ttu-id="1d0f2-110">További részletek toolearn tooApp szolgáltatás kód módosítások nélkül áttelepítése cikke hello [áttelepítése egy App Service Mobile Service tooAzure].</span><span class="sxs-lookup"><span data-stu-id="1d0f2-110">toolearn more about migrating tooApp Service without any code changes, see hello article [Migrate a Mobile Service tooAzure App Service].</span></span>

## <a name="header-specification"></a><span data-ttu-id="1d0f2-111">Fejléc meghatározása</span><span class="sxs-lookup"><span data-stu-id="1d0f2-111">Header specification</span></span>
<span data-ttu-id="1d0f2-112">hello kulcs `ZUMO-API-VERSION` hello HTTP-fejléc vagy a hello lekérdezési karakterlánc is megadható.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-112">hello key `ZUMO-API-VERSION` may be specified in either hello HTTP header or hello query string.</span></span> <span data-ttu-id="1d0f2-113">hello értéke egy verzió-karakterlánca hello űrlap **x.y.z**.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-113">hello value is a version string in hello form **x.y.z**.</span></span>

<span data-ttu-id="1d0f2-114">Példa:</span><span class="sxs-lookup"><span data-stu-id="1d0f2-114">For example:</span></span>

<span data-ttu-id="1d0f2-115">Https://service.azurewebsites.net/tables/TodoItem beolvasása</span><span class="sxs-lookup"><span data-stu-id="1d0f2-115">GET https://service.azurewebsites.net/tables/TodoItem</span></span>

<span data-ttu-id="1d0f2-116">FEJLÉCEK: ZUMO-API-VERZIÓ: 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-116">HEADERS: ZUMO-API-VERSION: 2.0.0</span></span>

<span data-ttu-id="1d0f2-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span></span>

## <a name="opting-out-of-version-checking"></a><span data-ttu-id="1d0f2-118">Meggátolható a verzió ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1d0f2-118">Opting out of version checking</span></span>
<span data-ttu-id="1d0f2-119">Verzióellenőrzés értékének beállításával lehet kikapcsolja **igaz** hello app beállítás **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-119">You can opt out of version checking by setting a value of **true** for hello app setting **MS_SkipVersionCheck**.</span></span> <span data-ttu-id="1d0f2-120">Adja meg, a web.config vagy hello hello Azure-portál alkalmazás beállítások területén.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-120">Specify this either in your web.config or in hello Application Settings section of hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="1d0f2-121">Számos viselkedésváltozások Mobile Services és a Mobile Apps, különösen a kapcsolat nélküli szinkronizálás, hitelesítés és leküldéses értesítések hello területek között.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-121">There are a number of behavior changes between Mobile Services and Mobile Apps, particularly in hello areas of offline sync, authentication, and push notifications.</span></span> <span data-ttu-id="1d0f2-122">Csak kikapcsolja kell verzióellenőrzés tesztelési tooensure befejezése után, hogy a viselkedés a módosítások nem törhetik az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-122">You should only opt out of version checking after complete testing tooensure that these behavioral changes do not break your app's functionality.</span></span>
>
>

## <a name="summary-of-compatibility-for-all-versions"></a><span data-ttu-id="1d0f2-123">Az összes verzió kompatibilitási összegzése</span><span class="sxs-lookup"><span data-stu-id="1d0f2-123">Summary of compatibility for all versions</span></span>
<span data-ttu-id="1d0f2-124">az alábbi hello diagram hello kompatibilitási összes ügyfél és kiszolgáló közötti jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-124">hello chart below shows hello compatibility between all client and server types.</span></span> <span data-ttu-id="1d0f2-125">A háttér vagy a Mobile lesz minősítve **szolgáltatások** vagy Mobile **alkalmazások** hello server SDK használt alapján.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-125">A backend is classified as either Mobile **Services** or Mobile **Apps** based on hello server SDK that it uses.</span></span>

|  | <span data-ttu-id="1d0f2-126">**Mobilszolgáltatások** Node.js vagy .NET</span><span class="sxs-lookup"><span data-stu-id="1d0f2-126">**Mobile Services** Node.js or .NET</span></span> | <span data-ttu-id="1d0f2-127">**Mobilalkalmazások** Node.js vagy .NET</span><span class="sxs-lookup"><span data-stu-id="1d0f2-127">**Mobile Apps** Node.js or .NET</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-128">[A Mobile Services ügyfelek]</span><span class="sxs-lookup"><span data-stu-id="1d0f2-128">[Mobile Services clients]</span></span> |<span data-ttu-id="1d0f2-129">oké</span><span class="sxs-lookup"><span data-stu-id="1d0f2-129">Ok</span></span> |<span data-ttu-id="1d0f2-130">Hiba történt\*</span><span class="sxs-lookup"><span data-stu-id="1d0f2-130">Error\*</span></span> |
| <span data-ttu-id="1d0f2-131">[Mobile Apps-ügyfelek]</span><span class="sxs-lookup"><span data-stu-id="1d0f2-131">[Mobile Apps clients]</span></span> |<span data-ttu-id="1d0f2-132">Hiba történt\*</span><span class="sxs-lookup"><span data-stu-id="1d0f2-132">Error\*</span></span> |<span data-ttu-id="1d0f2-133">oké</span><span class="sxs-lookup"><span data-stu-id="1d0f2-133">Ok</span></span> |

<span data-ttu-id="1d0f2-134">\*Ez is vezérelhető megadásával **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-134">\*This can be controlled by specifying **MS_SkipVersionCheck**.</span></span>

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <span data-ttu-id="1d0f2-135"><a name="1.0.0"></a>A Mobile Services-ügyfél és kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1d0f2-135"><a name="1.0.0"></a>Mobile Services client and server</span></span>
<span data-ttu-id="1d0f2-136">az alábbi táblázat hello hello ügyfél SDK-k kompatibilisek-e **Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-136">hello client SDKs in hello table below are compatible with **Mobile Services**.</span></span>

<span data-ttu-id="1d0f2-137">Megjegyzés: a Mobile Services ügyfél SDK-k hello *nem* fejléc értéke küldési `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-137">Note: hello Mobile Services client SDKs *do not* send a header value for `ZUMO-API-VERSION`.</span></span> <span data-ttu-id="1d0f2-138">Ha hello szolgáltatás a fejléc vagy a lekérdezési karakterláncokra vonatkozó értéket kap olyan hibaüzenetet küld, kivéve, ha rendelkezik explicit módon választotta ki a fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="1d0f2-138">If hello service receives this header or query string value, an error will be returned, unless you have explicitly opted out as described above.</span></span>

### <span data-ttu-id="1d0f2-139"><a name="MobileServicesClients"></a>Mobile *szolgáltatások* ügyfél SDK-k</span><span class="sxs-lookup"><span data-stu-id="1d0f2-139"><a name="MobileServicesClients"></a> Mobile *Services* client SDKs</span></span>
| <span data-ttu-id="1d0f2-140">Ügyfélplatform</span><span class="sxs-lookup"><span data-stu-id="1d0f2-140">Client platform</span></span> | <span data-ttu-id="1d0f2-141">Verzió</span><span class="sxs-lookup"><span data-stu-id="1d0f2-141">Version</span></span> | <span data-ttu-id="1d0f2-142">A verziófejléc-érték</span><span class="sxs-lookup"><span data-stu-id="1d0f2-142">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-143">Felügyelt ügyfél (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-143">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="1d0f2-144">1.3.2</span><span class="sxs-lookup"><span data-stu-id="1d0f2-144">1.3.2</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |<span data-ttu-id="1d0f2-145">n/a</span><span class="sxs-lookup"><span data-stu-id="1d0f2-145">n/a</span></span> |
| <span data-ttu-id="1d0f2-146">iOS</span><span class="sxs-lookup"><span data-stu-id="1d0f2-146">iOS</span></span> |[<span data-ttu-id="1d0f2-147">2.2.2</span><span class="sxs-lookup"><span data-stu-id="1d0f2-147">2.2.2</span></span>](http://aka.ms/gc6fex) |<span data-ttu-id="1d0f2-148">n/a</span><span class="sxs-lookup"><span data-stu-id="1d0f2-148">n/a</span></span> |
| <span data-ttu-id="1d0f2-149">Android</span><span class="sxs-lookup"><span data-stu-id="1d0f2-149">Android</span></span> |[<span data-ttu-id="1d0f2-150">2.0.3</span><span class="sxs-lookup"><span data-stu-id="1d0f2-150">2.0.3</span></span>](https://go.microsoft.com/fwLink/?LinkID=280126) |<span data-ttu-id="1d0f2-151">n/a</span><span class="sxs-lookup"><span data-stu-id="1d0f2-151">n/a</span></span> |
| <span data-ttu-id="1d0f2-152">HTML</span><span class="sxs-lookup"><span data-stu-id="1d0f2-152">HTML</span></span> |[<span data-ttu-id="1d0f2-153">1.2.7</span><span class="sxs-lookup"><span data-stu-id="1d0f2-153">1.2.7</span></span>](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |<span data-ttu-id="1d0f2-154">n/a</span><span class="sxs-lookup"><span data-stu-id="1d0f2-154">n/a</span></span> |

### <a name="mobile-services-server-sdks"></a><span data-ttu-id="1d0f2-155">Mobile *szolgáltatások* server SDK-k</span><span class="sxs-lookup"><span data-stu-id="1d0f2-155">Mobile *Services* server SDKs</span></span>
| <span data-ttu-id="1d0f2-156">Kiszolgáló platform</span><span class="sxs-lookup"><span data-stu-id="1d0f2-156">Server platform</span></span> | <span data-ttu-id="1d0f2-157">Verzió</span><span class="sxs-lookup"><span data-stu-id="1d0f2-157">Version</span></span> | <span data-ttu-id="1d0f2-158">Elfogadott verzió fejléc</span><span class="sxs-lookup"><span data-stu-id="1d0f2-158">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-159">.NET</span><span class="sxs-lookup"><span data-stu-id="1d0f2-159">.NET</span></span> |[<span data-ttu-id="1d0f2-160">WindowsAzure.MobileServices.Backend.* verzió 1.0.x</span><span class="sxs-lookup"><span data-stu-id="1d0f2-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |<span data-ttu-id="1d0f2-161">** Nem verziófejléc **</span><span class="sxs-lookup"><span data-stu-id="1d0f2-161">**No version header **</span></span> |
| <span data-ttu-id="1d0f2-162">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d0f2-162">Node.js</span></span> |<span data-ttu-id="1d0f2-163">(hamarosan elérhető)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-163">(coming soon)</span></span> |<span data-ttu-id="1d0f2-164">**Nincs verzió fejléc**</span><span class="sxs-lookup"><span data-stu-id="1d0f2-164">**No version header**</span></span> |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a><span data-ttu-id="1d0f2-165">A Mobile Services háttérkiszolgálókon viselkedése</span><span class="sxs-lookup"><span data-stu-id="1d0f2-165">Behavior of Mobile Services backends</span></span>
| <span data-ttu-id="1d0f2-166">ZUMO-API-VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="1d0f2-166">ZUMO-API-VERSION</span></span> | <span data-ttu-id="1d0f2-167">MS_SkipVersionCheck értéke</span><span class="sxs-lookup"><span data-stu-id="1d0f2-167">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="1d0f2-168">Válasz</span><span class="sxs-lookup"><span data-stu-id="1d0f2-168">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-169">Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="1d0f2-169">Not specified</span></span> |<span data-ttu-id="1d0f2-170">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="1d0f2-170">Any</span></span> |<span data-ttu-id="1d0f2-171">200 - OK</span><span class="sxs-lookup"><span data-stu-id="1d0f2-171">200 - OK</span></span> |
| <span data-ttu-id="1d0f2-172">Bármely érték</span><span class="sxs-lookup"><span data-stu-id="1d0f2-172">Any value</span></span> |<span data-ttu-id="1d0f2-173">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-173">True</span></span> |<span data-ttu-id="1d0f2-174">200 - OK</span><span class="sxs-lookup"><span data-stu-id="1d0f2-174">200 - OK</span></span> |
| <span data-ttu-id="1d0f2-175">Bármely érték</span><span class="sxs-lookup"><span data-stu-id="1d0f2-175">Any value</span></span> |<span data-ttu-id="1d0f2-176">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="1d0f2-176">False/Not Specified</span></span> |<span data-ttu-id="1d0f2-177">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="1d0f2-177">400 - Bad Request</span></span> |

## <span data-ttu-id="1d0f2-178"><a name="2.0.0"></a>Az Azure Mobile Apps-ügyfél és kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1d0f2-178"><a name="2.0.0"></a>Azure Mobile Apps client and server</span></span>
### <span data-ttu-id="1d0f2-179"><a name="MobileAppsClients"></a>Mobile *alkalmazások* ügyfél SDK-k</span><span class="sxs-lookup"><span data-stu-id="1d0f2-179"><a name="MobileAppsClients"></a> Mobile *Apps* client SDKs</span></span>
<span data-ttu-id="1d0f2-180">Verzióellenőrzés jelent a következő hello ügyfél SDK verziói hello kezdve a **Azure Mobile Apps**:</span><span class="sxs-lookup"><span data-stu-id="1d0f2-180">Version checking was introduced starting with hello following versions of hello client SDK for **Azure Mobile Apps**:</span></span>

| <span data-ttu-id="1d0f2-181">Ügyfélplatform</span><span class="sxs-lookup"><span data-stu-id="1d0f2-181">Client platform</span></span> | <span data-ttu-id="1d0f2-182">Verzió</span><span class="sxs-lookup"><span data-stu-id="1d0f2-182">Version</span></span> | <span data-ttu-id="1d0f2-183">A verziófejléc-érték</span><span class="sxs-lookup"><span data-stu-id="1d0f2-183">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-184">Felügyelt ügyfél (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-184">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="1d0f2-185">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-185">2.0.0</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |<span data-ttu-id="1d0f2-186">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-186">2.0.0</span></span> |
| <span data-ttu-id="1d0f2-187">iOS</span><span class="sxs-lookup"><span data-stu-id="1d0f2-187">iOS</span></span> |[<span data-ttu-id="1d0f2-188">3.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-188">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=529823) |<span data-ttu-id="1d0f2-189">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-189">2.0.0</span></span> |
| <span data-ttu-id="1d0f2-190">Android</span><span class="sxs-lookup"><span data-stu-id="1d0f2-190">Android</span></span> |[<span data-ttu-id="1d0f2-191">3.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-191">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |<span data-ttu-id="1d0f2-192">3.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-192">3.0.0</span></span> |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a><span data-ttu-id="1d0f2-193">Mobile *alkalmazások* server SDK-k</span><span class="sxs-lookup"><span data-stu-id="1d0f2-193">Mobile *Apps* server SDKs</span></span>
<span data-ttu-id="1d0f2-194">Verzióellenőrzés szerepel a kiszolgáló SDK verzió a következő:</span><span class="sxs-lookup"><span data-stu-id="1d0f2-194">Version checking is included in following server SDK versions:</span></span>

| <span data-ttu-id="1d0f2-195">Kiszolgáló platform</span><span class="sxs-lookup"><span data-stu-id="1d0f2-195">Server platform</span></span> | <span data-ttu-id="1d0f2-196">SDK</span><span class="sxs-lookup"><span data-stu-id="1d0f2-196">SDK</span></span> | <span data-ttu-id="1d0f2-197">Elfogadott verzió fejléc</span><span class="sxs-lookup"><span data-stu-id="1d0f2-197">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-198">.NET</span><span class="sxs-lookup"><span data-stu-id="1d0f2-198">.NET</span></span> |[<span data-ttu-id="1d0f2-199">Microsoft.Azure.Mobile.Server</span><span class="sxs-lookup"><span data-stu-id="1d0f2-199">Microsoft.Azure.Mobile.Server</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |<span data-ttu-id="1d0f2-200">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-200">2.0.0</span></span> |
| <span data-ttu-id="1d0f2-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d0f2-201">Node.js</span></span> |[<span data-ttu-id="1d0f2-202">Azure-mobileszköz-alkalmazások)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-202">azure-mobile-apps)</span></span>](https://www.npmjs.com/package/azure-mobile-apps) |<span data-ttu-id="1d0f2-203">2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d0f2-203">2.0.0</span></span> |

### <a name="behavior-of-mobile-apps-backends"></a><span data-ttu-id="1d0f2-204">Mobile Apps háttérkiszolgálókon viselkedése</span><span class="sxs-lookup"><span data-stu-id="1d0f2-204">Behavior of Mobile Apps backends</span></span>
| <span data-ttu-id="1d0f2-205">ZUMO-API-VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="1d0f2-205">ZUMO-API-VERSION</span></span> | <span data-ttu-id="1d0f2-206">MS_SkipVersionCheck értéke</span><span class="sxs-lookup"><span data-stu-id="1d0f2-206">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="1d0f2-207">Válasz</span><span class="sxs-lookup"><span data-stu-id="1d0f2-207">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1d0f2-208">NULL értékű vagy x.y.z</span><span class="sxs-lookup"><span data-stu-id="1d0f2-208">x.y.z or Null</span></span> |<span data-ttu-id="1d0f2-209">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="1d0f2-209">True</span></span> |<span data-ttu-id="1d0f2-210">200 - OK</span><span class="sxs-lookup"><span data-stu-id="1d0f2-210">200 - OK</span></span> |
| <span data-ttu-id="1d0f2-211">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="1d0f2-211">Null</span></span> |<span data-ttu-id="1d0f2-212">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="1d0f2-212">False/Not Specified</span></span> |<span data-ttu-id="1d0f2-213">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="1d0f2-213">400 - Bad Request</span></span> |
| <span data-ttu-id="1d0f2-214">1.x.y</span><span class="sxs-lookup"><span data-stu-id="1d0f2-214">1.x.y</span></span> |<span data-ttu-id="1d0f2-215">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="1d0f2-215">False/Not Specified</span></span> |<span data-ttu-id="1d0f2-216">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="1d0f2-216">400 - Bad Request</span></span> |
| <span data-ttu-id="1d0f2-217">2.0.0-2.x.y</span><span class="sxs-lookup"><span data-stu-id="1d0f2-217">2.0.0-2.x.y</span></span> |<span data-ttu-id="1d0f2-218">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="1d0f2-218">False/Not Specified</span></span> |<span data-ttu-id="1d0f2-219">200 - OK</span><span class="sxs-lookup"><span data-stu-id="1d0f2-219">200 - OK</span></span> |
| <span data-ttu-id="1d0f2-220">3.0.0-3.x.y</span><span class="sxs-lookup"><span data-stu-id="1d0f2-220">3.0.0-3.x.y</span></span> |<span data-ttu-id="1d0f2-221">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="1d0f2-221">False/Not Specified</span></span> |<span data-ttu-id="1d0f2-222">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="1d0f2-222">400 - Bad Request</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d0f2-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d0f2-223">Next Steps</span></span>
* <span data-ttu-id="1d0f2-224">[áttelepítése egy App Service Mobile Service tooAzure]</span><span class="sxs-lookup"><span data-stu-id="1d0f2-224">[Migrate a Mobile Service tooAzure App Service]</span></span>

[A Mobile Services ügyfelek]: #MobileServicesClients
[Mobile Apps-ügyfelek]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[áttelepítése egy App Service Mobile Service tooAzure]: app-service-mobile-migrating-from-mobile-services.md
