---
title: "A Mobile Apps és a Mobile Services SDK versioning ügyfél és kiszolgáló |} Microsoft Docs"
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
ms.openlocfilehash: f79e819b1547f81498ea213858faf3c75e374782
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a><span data-ttu-id="ee4b5-103">A Mobile Apps és a Mobile Services ügyfél és kiszolgáló versioning</span><span class="sxs-lookup"><span data-stu-id="ee4b5-103">Client and server versioning in Mobile Apps and Mobile Services</span></span>
<span data-ttu-id="ee4b5-104">Azure Mobile Services legújabb verziója a **Mobile Apps** az Azure App Service szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-104">The latest version of Azure Mobile Services is the **Mobile Apps** feature of Azure App Service.</span></span>

<span data-ttu-id="ee4b5-105">A Mobile Apps-ügyfél és kiszolgáló SDK a Mobile Services az eredetileg alapulnak, de azok *nem* kompatibilis egymással.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-105">The Mobile Apps client and server SDKs are originally based on those in Mobile Services, but they are *not* compatible with each other.</span></span>
<span data-ttu-id="ee4b5-106">Ez azt jelenti, hogy kell használnia egy *Mobile Apps* ügyfél SDK rendelkező egy *Mobile Apps* server SDK és ehhez hasonlóan az *Mobile Services*.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-106">That is, you must use a *Mobile Apps* client SDK with a *Mobile Apps* server SDK and similarly for *Mobile Services*.</span></span> <span data-ttu-id="ee4b5-107">Ehhez a szerződéshez használják az ügyfél és kiszolgáló SDK-k, különleges fejléc értéke ki `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-107">This contract is enforced through a special header value used by the client and server SDKs, `ZUMO-API-VERSION`.</span></span>

<span data-ttu-id="ee4b5-108">Megjegyzés: Ha ez a dokumentum hivatkozik egy *Mobile Services* háttér, nem feltétlenül kell a Mobile Services is működtetnek.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-108">Note: whenever this document refers to a *Mobile Services* backend, it does not necessarily need to be hosted on Mobile Services.</span></span> <span data-ttu-id="ee4b5-109">Már lehetséges a futtatásához az App Service kód módosítás nélkül mobilszolgáltatás áttelepítése, de a szolgáltatás továbbra is dolgozna *Mobile Services* SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-109">It is now possible to migrate a mobile service to run on App Service without any code changes, but the service would still be using *Mobile Services*  SDK versions.</span></span>

<span data-ttu-id="ee4b5-110">Az App Service kód módosítás nélkül áttelepítéssel kapcsolatos további tudnivalókért tekintse meg a cikket [Mobile szolgáltatás áttelepítése az Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="ee4b5-110">To learn more about migrating to App Service without any code changes, see the article [Migrate a Mobile Service to Azure App Service].</span></span>

## <a name="header-specification"></a><span data-ttu-id="ee4b5-111">Fejléc meghatározása</span><span class="sxs-lookup"><span data-stu-id="ee4b5-111">Header specification</span></span>
<span data-ttu-id="ee4b5-112">A kulcs `ZUMO-API-VERSION` vagy a HTTP-fejléc, vagy a lekérdezési karakterlánc is megadható.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-112">The key `ZUMO-API-VERSION` may be specified in either the HTTP header or the query string.</span></span> <span data-ttu-id="ee4b5-113">Az űrlap egy verzió-karakterlánca értéke **x.y.z**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-113">The value is a version string in the form **x.y.z**.</span></span>

<span data-ttu-id="ee4b5-114">Példa:</span><span class="sxs-lookup"><span data-stu-id="ee4b5-114">For example:</span></span>

<span data-ttu-id="ee4b5-115">Https://service.azurewebsites.net/tables/TodoItem beolvasása</span><span class="sxs-lookup"><span data-stu-id="ee4b5-115">GET https://service.azurewebsites.net/tables/TodoItem</span></span>

<span data-ttu-id="ee4b5-116">FEJLÉCEK: ZUMO-API-VERZIÓ: 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-116">HEADERS: ZUMO-API-VERSION: 2.0.0</span></span>

<span data-ttu-id="ee4b5-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span></span>

## <a name="opting-out-of-version-checking"></a><span data-ttu-id="ee4b5-118">Meggátolható a verzió ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="ee4b5-118">Opting out of version checking</span></span>
<span data-ttu-id="ee4b5-119">Verzióellenőrzés értékének beállításával lehet kikapcsolja **igaz** az alkalmazás-beállítás **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-119">You can opt out of version checking by setting a value of **true** for the app setting **MS_SkipVersionCheck**.</span></span> <span data-ttu-id="ee4b5-120">Adja meg, vagy a Web.config fájlban, vagy az Azure-portálon Alkalmazásbeállítások szakaszában.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-120">Specify this either in your web.config or in the Application Settings section of the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="ee4b5-121">Számos viselkedésváltozások Mobile Services és a Mobile Apps, különösen a kapcsolat nélküli szinkronizálás, hitelesítés és leküldéses értesítések között.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-121">There are a number of behavior changes between Mobile Services and Mobile Apps, particularly in the areas of offline sync, authentication, and push notifications.</span></span> <span data-ttu-id="ee4b5-122">Csak kikapcsolja kell verzió ellenőrzése, hogy a viselkedés a módosítások nem törhetik az alkalmazás funkcióinak biztosításához teszt befejezése után.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-122">You should only opt out of version checking after complete testing to ensure that these behavioral changes do not break your app's functionality.</span></span>
>
>

## <a name="summary-of-compatibility-for-all-versions"></a><span data-ttu-id="ee4b5-123">Az összes verzió kompatibilitási összegzése</span><span class="sxs-lookup"><span data-stu-id="ee4b5-123">Summary of compatibility for all versions</span></span>
<span data-ttu-id="ee4b5-124">Az alábbi táblázatban látható az összes ügyfél és kiszolgáló közötti kompatibilitást.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-124">The chart below shows the compatibility between all client and server types.</span></span> <span data-ttu-id="ee4b5-125">A háttér vagy a Mobile lesz minősítve **szolgáltatások** vagy Mobile **alkalmazások** a kiszolgálón használt SDK-alapú.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-125">A backend is classified as either Mobile **Services** or Mobile **Apps** based on the server SDK that it uses.</span></span>

|  | <span data-ttu-id="ee4b5-126">**Mobilszolgáltatások** Node.js vagy .NET</span><span class="sxs-lookup"><span data-stu-id="ee4b5-126">**Mobile Services** Node.js or .NET</span></span> | <span data-ttu-id="ee4b5-127">**Mobilalkalmazások** Node.js vagy .NET</span><span class="sxs-lookup"><span data-stu-id="ee4b5-127">**Mobile Apps** Node.js or .NET</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-128">[A Mobile Services ügyfelek]</span><span class="sxs-lookup"><span data-stu-id="ee4b5-128">[Mobile Services clients]</span></span> |<span data-ttu-id="ee4b5-129">oké</span><span class="sxs-lookup"><span data-stu-id="ee4b5-129">Ok</span></span> |<span data-ttu-id="ee4b5-130">Hiba történt\*</span><span class="sxs-lookup"><span data-stu-id="ee4b5-130">Error\*</span></span> |
| <span data-ttu-id="ee4b5-131">[Mobile Apps-ügyfelek]</span><span class="sxs-lookup"><span data-stu-id="ee4b5-131">[Mobile Apps clients]</span></span> |<span data-ttu-id="ee4b5-132">Hiba történt\*</span><span class="sxs-lookup"><span data-stu-id="ee4b5-132">Error\*</span></span> |<span data-ttu-id="ee4b5-133">oké</span><span class="sxs-lookup"><span data-stu-id="ee4b5-133">Ok</span></span> |

<span data-ttu-id="ee4b5-134">\*Ez is vezérelhető megadásával **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-134">\*This can be controlled by specifying **MS_SkipVersionCheck**.</span></span>

<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <span data-ttu-id="ee4b5-135"><a name="1.0.0"></a>A Mobile Services-ügyfél és kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ee4b5-135"><a name="1.0.0"></a>Mobile Services client and server</span></span>
<span data-ttu-id="ee4b5-136">Az alábbi táblázat az ügyfél SDK-k kompatibilisek-e **Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-136">The client SDKs in the table below are compatible with **Mobile Services**.</span></span>

<span data-ttu-id="ee4b5-137">Megjegyzés: a Mobile Services-ügyfél SDK-k *nem* fejléc értéke küldési `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-137">Note: the Mobile Services client SDKs *do not* send a header value for `ZUMO-API-VERSION`.</span></span> <span data-ttu-id="ee4b5-138">Ha a szolgáltatás a fejléc vagy a lekérdezési karakterláncokra vonatkozó értéket kap olyan hibaüzenetet küld, kivéve, ha rendelkezik explicit módon választotta ki a fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-138">If the service receives this header or query string value, an error will be returned, unless you have explicitly opted out as described above.</span></span>

### <span data-ttu-id="ee4b5-139"><a name="MobileServicesClients"></a>Mobile *szolgáltatások* ügyfél SDK-k</span><span class="sxs-lookup"><span data-stu-id="ee4b5-139"><a name="MobileServicesClients"></a> Mobile *Services* client SDKs</span></span>
| <span data-ttu-id="ee4b5-140">Ügyfélplatform</span><span class="sxs-lookup"><span data-stu-id="ee4b5-140">Client platform</span></span> | <span data-ttu-id="ee4b5-141">Verzió</span><span class="sxs-lookup"><span data-stu-id="ee4b5-141">Version</span></span> | <span data-ttu-id="ee4b5-142">A verziófejléc-érték</span><span class="sxs-lookup"><span data-stu-id="ee4b5-142">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-143">Felügyelt ügyfél (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-143">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="ee4b5-144">1.3.2</span><span class="sxs-lookup"><span data-stu-id="ee4b5-144">1.3.2</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |<span data-ttu-id="ee4b5-145">n/a</span><span class="sxs-lookup"><span data-stu-id="ee4b5-145">n/a</span></span> |
| <span data-ttu-id="ee4b5-146">iOS</span><span class="sxs-lookup"><span data-stu-id="ee4b5-146">iOS</span></span> |[<span data-ttu-id="ee4b5-147">2.2.2</span><span class="sxs-lookup"><span data-stu-id="ee4b5-147">2.2.2</span></span>](http://aka.ms/gc6fex) |<span data-ttu-id="ee4b5-148">n/a</span><span class="sxs-lookup"><span data-stu-id="ee4b5-148">n/a</span></span> |
| <span data-ttu-id="ee4b5-149">Android</span><span class="sxs-lookup"><span data-stu-id="ee4b5-149">Android</span></span> |[<span data-ttu-id="ee4b5-150">2.0.3</span><span class="sxs-lookup"><span data-stu-id="ee4b5-150">2.0.3</span></span>](https://go.microsoft.com/fwLink/?LinkID=280126) |<span data-ttu-id="ee4b5-151">n/a</span><span class="sxs-lookup"><span data-stu-id="ee4b5-151">n/a</span></span> |
| <span data-ttu-id="ee4b5-152">HTML</span><span class="sxs-lookup"><span data-stu-id="ee4b5-152">HTML</span></span> |[<span data-ttu-id="ee4b5-153">1.2.7</span><span class="sxs-lookup"><span data-stu-id="ee4b5-153">1.2.7</span></span>](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |<span data-ttu-id="ee4b5-154">n/a</span><span class="sxs-lookup"><span data-stu-id="ee4b5-154">n/a</span></span> |

### <a name="mobile-services-server-sdks"></a><span data-ttu-id="ee4b5-155">Mobile *szolgáltatások* server SDK-k</span><span class="sxs-lookup"><span data-stu-id="ee4b5-155">Mobile *Services* server SDKs</span></span>
| <span data-ttu-id="ee4b5-156">Kiszolgáló platform</span><span class="sxs-lookup"><span data-stu-id="ee4b5-156">Server platform</span></span> | <span data-ttu-id="ee4b5-157">Verzió</span><span class="sxs-lookup"><span data-stu-id="ee4b5-157">Version</span></span> | <span data-ttu-id="ee4b5-158">Elfogadott verzió fejléc</span><span class="sxs-lookup"><span data-stu-id="ee4b5-158">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-159">.NET</span><span class="sxs-lookup"><span data-stu-id="ee4b5-159">.NET</span></span> |[<span data-ttu-id="ee4b5-160">WindowsAzure.MobileServices.Backend.* verzió 1.0.x</span><span class="sxs-lookup"><span data-stu-id="ee4b5-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |<span data-ttu-id="ee4b5-161">** Nem verziófejléc **</span><span class="sxs-lookup"><span data-stu-id="ee4b5-161">**No version header **</span></span> |
| <span data-ttu-id="ee4b5-162">Node.js</span><span class="sxs-lookup"><span data-stu-id="ee4b5-162">Node.js</span></span> |<span data-ttu-id="ee4b5-163">(hamarosan elérhető)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-163">(coming soon)</span></span> |<span data-ttu-id="ee4b5-164">**Nincs verzió fejléc**</span><span class="sxs-lookup"><span data-stu-id="ee4b5-164">**No version header**</span></span> |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a><span data-ttu-id="ee4b5-165">A Mobile Services háttérkiszolgálókon viselkedése</span><span class="sxs-lookup"><span data-stu-id="ee4b5-165">Behavior of Mobile Services backends</span></span>
| <span data-ttu-id="ee4b5-166">ZUMO-API-VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="ee4b5-166">ZUMO-API-VERSION</span></span> | <span data-ttu-id="ee4b5-167">MS_SkipVersionCheck értéke</span><span class="sxs-lookup"><span data-stu-id="ee4b5-167">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="ee4b5-168">Válasz</span><span class="sxs-lookup"><span data-stu-id="ee4b5-168">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-169">Nincs megadva</span><span class="sxs-lookup"><span data-stu-id="ee4b5-169">Not specified</span></span> |<span data-ttu-id="ee4b5-170">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="ee4b5-170">Any</span></span> |<span data-ttu-id="ee4b5-171">200 - OK</span><span class="sxs-lookup"><span data-stu-id="ee4b5-171">200 - OK</span></span> |
| <span data-ttu-id="ee4b5-172">Bármely érték</span><span class="sxs-lookup"><span data-stu-id="ee4b5-172">Any value</span></span> |<span data-ttu-id="ee4b5-173">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-173">True</span></span> |<span data-ttu-id="ee4b5-174">200 - OK</span><span class="sxs-lookup"><span data-stu-id="ee4b5-174">200 - OK</span></span> |
| <span data-ttu-id="ee4b5-175">Bármely érték</span><span class="sxs-lookup"><span data-stu-id="ee4b5-175">Any value</span></span> |<span data-ttu-id="ee4b5-176">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="ee4b5-176">False/Not Specified</span></span> |<span data-ttu-id="ee4b5-177">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="ee4b5-177">400 - Bad Request</span></span> |

## <span data-ttu-id="ee4b5-178"><a name="2.0.0"></a>Az Azure Mobile Apps-ügyfél és kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ee4b5-178"><a name="2.0.0"></a>Azure Mobile Apps client and server</span></span>
### <span data-ttu-id="ee4b5-179"><a name="MobileAppsClients"></a>Mobile *alkalmazások* ügyfél SDK-k</span><span class="sxs-lookup"><span data-stu-id="ee4b5-179"><a name="MobileAppsClients"></a> Mobile *Apps* client SDKs</span></span>
<span data-ttu-id="ee4b5-180">Az SDK-ügyfél a következő verziójú indítása verzióellenőrzés jelent a **Azure Mobile Apps**:</span><span class="sxs-lookup"><span data-stu-id="ee4b5-180">Version checking was introduced starting with the following versions of the client SDK for **Azure Mobile Apps**:</span></span>

| <span data-ttu-id="ee4b5-181">Ügyfélplatform</span><span class="sxs-lookup"><span data-stu-id="ee4b5-181">Client platform</span></span> | <span data-ttu-id="ee4b5-182">Verzió</span><span class="sxs-lookup"><span data-stu-id="ee4b5-182">Version</span></span> | <span data-ttu-id="ee4b5-183">A verziófejléc-érték</span><span class="sxs-lookup"><span data-stu-id="ee4b5-183">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-184">Felügyelt ügyfél (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-184">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="ee4b5-185">2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-185">2.0.0</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |<span data-ttu-id="ee4b5-186">2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-186">2.0.0</span></span> |
| <span data-ttu-id="ee4b5-187">iOS</span><span class="sxs-lookup"><span data-stu-id="ee4b5-187">iOS</span></span> |[<span data-ttu-id="ee4b5-188">3.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-188">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=529823) |<span data-ttu-id="ee4b5-189">2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-189">2.0.0</span></span> |
| <span data-ttu-id="ee4b5-190">Android</span><span class="sxs-lookup"><span data-stu-id="ee4b5-190">Android</span></span> |[<span data-ttu-id="ee4b5-191">3.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-191">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |<span data-ttu-id="ee4b5-192">3.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-192">3.0.0</span></span> |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a><span data-ttu-id="ee4b5-193">Mobile *alkalmazások* server SDK-k</span><span class="sxs-lookup"><span data-stu-id="ee4b5-193">Mobile *Apps* server SDKs</span></span>
<span data-ttu-id="ee4b5-194">Verzióellenőrzés szerepel a kiszolgáló SDK verzió a következő:</span><span class="sxs-lookup"><span data-stu-id="ee4b5-194">Version checking is included in following server SDK versions:</span></span>

| <span data-ttu-id="ee4b5-195">Kiszolgáló platform</span><span class="sxs-lookup"><span data-stu-id="ee4b5-195">Server platform</span></span> | <span data-ttu-id="ee4b5-196">SDK</span><span class="sxs-lookup"><span data-stu-id="ee4b5-196">SDK</span></span> | <span data-ttu-id="ee4b5-197">Elfogadott verzió fejléc</span><span class="sxs-lookup"><span data-stu-id="ee4b5-197">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-198">.NET</span><span class="sxs-lookup"><span data-stu-id="ee4b5-198">.NET</span></span> |[<span data-ttu-id="ee4b5-199">Microsoft.Azure.Mobile.Server</span><span class="sxs-lookup"><span data-stu-id="ee4b5-199">Microsoft.Azure.Mobile.Server</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |<span data-ttu-id="ee4b5-200">2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-200">2.0.0</span></span> |
| <span data-ttu-id="ee4b5-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="ee4b5-201">Node.js</span></span> |[<span data-ttu-id="ee4b5-202">Azure-mobileszköz-alkalmazások)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-202">azure-mobile-apps)</span></span>](https://www.npmjs.com/package/azure-mobile-apps) |<span data-ttu-id="ee4b5-203">2.0.0</span><span class="sxs-lookup"><span data-stu-id="ee4b5-203">2.0.0</span></span> |

### <a name="behavior-of-mobile-apps-backends"></a><span data-ttu-id="ee4b5-204">Mobile Apps háttérkiszolgálókon viselkedése</span><span class="sxs-lookup"><span data-stu-id="ee4b5-204">Behavior of Mobile Apps backends</span></span>
| <span data-ttu-id="ee4b5-205">ZUMO-API-VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="ee4b5-205">ZUMO-API-VERSION</span></span> | <span data-ttu-id="ee4b5-206">MS_SkipVersionCheck értéke</span><span class="sxs-lookup"><span data-stu-id="ee4b5-206">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="ee4b5-207">Válasz</span><span class="sxs-lookup"><span data-stu-id="ee4b5-207">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee4b5-208">NULL értékű vagy x.y.z</span><span class="sxs-lookup"><span data-stu-id="ee4b5-208">x.y.z or Null</span></span> |<span data-ttu-id="ee4b5-209">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="ee4b5-209">True</span></span> |<span data-ttu-id="ee4b5-210">200 - OK</span><span class="sxs-lookup"><span data-stu-id="ee4b5-210">200 - OK</span></span> |
| <span data-ttu-id="ee4b5-211">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="ee4b5-211">Null</span></span> |<span data-ttu-id="ee4b5-212">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="ee4b5-212">False/Not Specified</span></span> |<span data-ttu-id="ee4b5-213">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="ee4b5-213">400 - Bad Request</span></span> |
| <span data-ttu-id="ee4b5-214">1.x.y</span><span class="sxs-lookup"><span data-stu-id="ee4b5-214">1.x.y</span></span> |<span data-ttu-id="ee4b5-215">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="ee4b5-215">False/Not Specified</span></span> |<span data-ttu-id="ee4b5-216">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="ee4b5-216">400 - Bad Request</span></span> |
| <span data-ttu-id="ee4b5-217">2.0.0-2.x.y</span><span class="sxs-lookup"><span data-stu-id="ee4b5-217">2.0.0-2.x.y</span></span> |<span data-ttu-id="ee4b5-218">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="ee4b5-218">False/Not Specified</span></span> |<span data-ttu-id="ee4b5-219">200 - OK</span><span class="sxs-lookup"><span data-stu-id="ee4b5-219">200 - OK</span></span> |
| <span data-ttu-id="ee4b5-220">3.0.0-3.x.y</span><span class="sxs-lookup"><span data-stu-id="ee4b5-220">3.0.0-3.x.y</span></span> |<span data-ttu-id="ee4b5-221">A megadott FALSE/nem</span><span class="sxs-lookup"><span data-stu-id="ee4b5-221">False/Not Specified</span></span> |<span data-ttu-id="ee4b5-222">400 - Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="ee4b5-222">400 - Bad Request</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ee4b5-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee4b5-223">Next Steps</span></span>
* <span data-ttu-id="ee4b5-224">[Mobile szolgáltatás áttelepítése az Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="ee4b5-224">[Migrate a Mobile Service to Azure App Service]</span></span>

<span data-ttu-id="ee4b5-225">[A Mobile Services ügyfelek]: #MobileServicesClients</span><span class="sxs-lookup"><span data-stu-id="ee4b5-225">[Mobile Services clients]: #MobileServicesClients</span></span>
<span data-ttu-id="ee4b5-226">[Mobile Apps-ügyfelek]: #MobileAppsClients</span><span class="sxs-lookup"><span data-stu-id="ee4b5-226">[Mobile Apps clients]: #MobileAppsClients</span></span>


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
<span data-ttu-id="ee4b5-227">[Mobile szolgáltatás áttelepítése az Azure App Service]: app-service-mobile-migrating-from-mobile-services.md</span><span class="sxs-lookup"><span data-stu-id="ee4b5-227">[Migrate a Mobile Service to Azure App Service]: app-service-mobile-migrating-from-mobile-services.md</span></span>
