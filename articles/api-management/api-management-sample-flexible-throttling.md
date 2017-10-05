---
title: "Az Azure API Management-szabályozás Speciális kérelem"
description: "Megtudhatja, hogyan hozhat létre és alkalmazhat a rugalmas kvóta és korlátozza az Azure API-felügyeleti szabályzatok alapján."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 35375e599891a9443a91c4c3a8657e8c9c48c7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="ab748-103">Az Azure API Management-szabályozás Speciális kérelem</span><span class="sxs-lookup"><span data-stu-id="ab748-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="ab748-104">Az Azure API Management kulcsfontosságú szerepet igényt bejövő kérelmek szabályozás.</span><span class="sxs-lookup"><span data-stu-id="ab748-104">Being able to throttle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="ab748-105">Lehet, hogy a kéréseket, és a teljes kérelmek/átvitt adatok mértékű vezérlése, API-kezelés lehetővé teszi a API-szolgáltatókat az API-való visszaélés elleni védelmében, és hozzon létre külön API termék rétegekhez értékét.</span><span class="sxs-lookup"><span data-stu-id="ab748-105">Either by controlling the rate of requests or the total requests/data transferred, API Management allows API providers to protect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="ab748-106">A termék alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="ab748-106">Product based throttling</span></span>
<span data-ttu-id="ab748-107">Naprakész, a sebesség szabályozása képességekkel rendelkezik volt korlátozva, éppen hatókörű egy adott termék-előfizetéshez (lényegében egy kulcs), az API Management publisher portálon meghatározott.</span><span class="sxs-lookup"><span data-stu-id="ab748-107">To date, the rate throttling capabilities have been limited to being scoped to a particular Product subscription (essentially a key), defined in the API Management publisher portal.</span></span> <span data-ttu-id="ab748-108">Ez akkor hasznos, ha az API-szolgáltató szűkítheti a regisztráltak-e az API használatához a fejlesztőknek, azonban nem segítséget, például a szabályozás, az API-t egyéni végfelhasználók.</span><span class="sxs-lookup"><span data-stu-id="ab748-108">This is useful for the API provider to apply limits on the developers who have signed up to use their API, however, it does not help, for example, in throttling individual end-users of the API.</span></span> <span data-ttu-id="ab748-109">Előfordulhat, hogy az egyetlen teljes kvóta fogadni, és más ügyfelek a fejlesztő megakadályozza az alkalmazás használata a fejlesztői alkalmazás felhasználója.</span><span class="sxs-lookup"><span data-stu-id="ab748-109">It is possible that for single user of the developer's application to consume the entire quota and then prevent other customers of the developer from being able to use the application.</span></span> <span data-ttu-id="ab748-110">Nagy mennyiségű kérést hozhat létre több felhasználókat is, korlátozhatja az alkalmi felhasználók hozzáférésének.</span><span class="sxs-lookup"><span data-stu-id="ab748-110">Also, several customers who might generate a high volume of requests may limit access to occasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="ab748-111">Egyéni kulcs alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="ab748-111">Custom key based throttling</span></span>
<span data-ttu-id="ab748-112">Az új [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek a forgalmi ellenőrzési jóval rugalmasabb megoldást nyújt.</span><span class="sxs-lookup"><span data-stu-id="ab748-112">The new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution to traffic control.</span></span> <span data-ttu-id="ab748-113">Ezeket az új házirendeket engedélyezi annak meghatározását-kifejezések helyesen azonosítani a kulcsokhoz, a forgalom használat nyomon követésére használható.</span><span class="sxs-lookup"><span data-stu-id="ab748-113">These new policies allow you to define expressions to identify the keys that will be used to track traffic usage.</span></span> <span data-ttu-id="ab748-114">Ez a megfelelő működésének easiest látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="ab748-114">The way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="ab748-115">IP-cím sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="ab748-115">IP Address throttling</span></span>
<span data-ttu-id="ab748-116">A következő házirendek korlátozása egyetlen ügyfél IP-címnek hívások csak 10 percenként 1 000 000 hívások összesen és 10 000 kilobájt havonta sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="ab748-116">The following policies restrict a single client IP address to only 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="ab748-117">Ha az interneten lévő összes ügyfél egy egyedi IP-címet használ, ez lehet hatékony módszer a felhasználó által használat korlátozása.</span><span class="sxs-lookup"><span data-stu-id="ab748-117">If all clients on the Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="ab748-118">Azonban ez nagyon valószínű, hogy több felhasználó fog-e a megosztás azokat az interneten keresztül egy NAT-eszköz használata miatt egyetlen nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ab748-118">However, it is quite likely that multiple users will sharing a single public IP address due to them accessing the Internet via a NAT device.</span></span> <span data-ttu-id="ab748-119">Annak ellenére, hogy az API-k esetében, amelyek lehetővé teszik a nem hitelesített hozzáférést a `IpAddress` előfordulhat, hogy a legjobb lehetőség.</span><span class="sxs-lookup"><span data-stu-id="ab748-119">Despite this, for APIs that allow unauthenticated access the `IpAddress` might be the best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="ab748-120">Felhasználói azonosító sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="ab748-120">User identity throttling</span></span>
<span data-ttu-id="ab748-121">Ha a felhasználó hitelesítése, akkor a sávszélesség-szabályozási kulcs hozható létre, amely egyedileg azonosít egy, amely adatok alapján felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ab748-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="ab748-122">Ebben a példában azt az Authorization fejlécet kiolvasni, alakítsa át a következőre `JWT` objektumra, és a tulajdonos a jogkivonat segítségével azonosíthatja a felhasználót, és használja, amely a sebesség korlátozása kulcs.</span><span class="sxs-lookup"><span data-stu-id="ab748-122">In this example we extract the Authorization header, convert it to `JWT` object and use the subject of the token to identify the user and use that as the rate limiting key.</span></span> <span data-ttu-id="ab748-123">Ha a felhasználói azonosító található a `JWT` , egy másik jogcímek majd, hogy érték helyére lesznek használva.</span><span class="sxs-lookup"><span data-stu-id="ab748-123">If the user identity is stored in the `JWT` as one of the other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="ab748-124">Kombinált házirendek</span><span class="sxs-lookup"><span data-stu-id="ab748-124">Combined policies</span></span>
<span data-ttu-id="ab748-125">Az új sávszélesség-szabályozási házirendek biztosítják nagyobb mértékben vezérelheti, mint a meglévő szabályozó házirendeket, bár nincs továbbra is érték kombinálásával mindkét szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ab748-125">Although the new throttling policies provide more control than the existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="ab748-126">Termékkulcs előfizetés általi szabályozás ([előfizetési határértéket hívás arányt a](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [Set memóriahasználati kvóta előfizetéssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) engedélyezéséhez a úgy használati szintek alapján, az API-k monetizing remek módja.</span><span class="sxs-lookup"><span data-stu-id="ab748-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way to enable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="ab748-127">Az eszközzel kombinálva felhasználóbarát nyomtatott irányítását képesek a felhasználó által szabályozás kiegészíti, és megakadályozza, hogy egy felhasználó viselkedését egy másik élmény terhelése.</span><span class="sxs-lookup"><span data-stu-id="ab748-127">The finer grained control of being able to throttle by user is complementary and prevents one user's behavior from degrading the experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="ab748-128">Ügyfél-alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="ab748-128">Client driven throttling</span></span>
<span data-ttu-id="ab748-129">Ha a sávszélesség-szabályozási kulcs használatával van megadva egy [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx), akkor célszerű az API-szolgáltatót, amelyet a sávszélesség-szabályozás hatóköre hogyan van választva.</span><span class="sxs-lookup"><span data-stu-id="ab748-129">When the throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is the API provider that is choosing how the throttling is scoped.</span></span> <span data-ttu-id="ab748-130">Azonban a fejlesztő előfordulhat, hogy kívánják vezérelni hogyan azok értékelje korlát a saját ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="ab748-130">However, a developer might want to control how they rate limit their own customers.</span></span> <span data-ttu-id="ab748-131">Ez sikerült engedélyezni az API-szolgáltató egy egyéni fejlécet bevezetésével ahhoz, hogy a fejlesztő ügyfél alkalmazás való kommunikációhoz az API-t a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ab748-131">This could be enabled by the API provider by introducing a custom header to allow the developer's client application to communicate the key to the API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="ab748-132">Ez lehetővé teszi a fejlesztői ügyfélalkalmazás számára adja meg, hogy azok a sebesség korlátozása a kulcs létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ab748-132">This enables the developer's client application to choose how they want to create the rate limiting key.</span></span> <span data-ttu-id="ab748-133">Egy kis nyújt az ügyfél a fejlesztők kulcsok készleteinek kiosztása a felhasználók számára, és a kulcshasználati elforgatása létrehozhat saját rétegek.</span><span class="sxs-lookup"><span data-stu-id="ab748-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys to users and rotating the key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="ab748-134">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ab748-134">Summary</span></span>
<span data-ttu-id="ab748-135">Az Azure API Management sebessége és ajánlat használatával történő védelméhez és értékkel adja hozzá az API-szolgáltatás is biztosít.</span><span class="sxs-lookup"><span data-stu-id="ab748-135">Azure API Management provides rate and quote throttling to both protect and add value to your API service.</span></span> <span data-ttu-id="ab748-136">Az egyéni szabályának új sávszélesség-szabályozási szabályzatok lehetővé teszik eszközzel kombinálva felhasználóbarát nyomtatott szabályozhatják, ezek a házirendek ahhoz, hogy az ügyfelek még élvezetesebbé alkalmazásokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="ab748-136">The new throttling policies with custom scoping rules allow you finer grained control over those policies to enable your customers to build even better applications.</span></span> <span data-ttu-id="ab748-137">Ebben a cikkben szereplő példák bemutatják, ezeket az új házirendeket használata korlátozza az ügyfél IP-címeket, a felhasználói azonosító és a generált ügyfél értékek kulcsok gyártó szerint.</span><span class="sxs-lookup"><span data-stu-id="ab748-137">The examples in this article demonstrate the use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="ab748-138">Van azonban az üzenetet, például a felhasználói ügynök, az URL-cím elérési út töredék, az üzenet mérete használható sok többi részével.</span><span class="sxs-lookup"><span data-stu-id="ab748-138">However, there are many other parts of the message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab748-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab748-139">Next steps</span></span>
<span data-ttu-id="ab748-140">Adja meg a visszajelzést a disqus-beszélgetésben teheti szál az ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="ab748-140">Please give us your feedback in the Disqus thread for this topic.</span></span> <span data-ttu-id="ab748-141">Más lehetséges értékek, amelyek egy logikai választás a forgatókönyvekben lettek szóló értesítésekre nagyszerű lenne.</span><span class="sxs-lookup"><span data-stu-id="ab748-141">It would be great to hear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="ab748-142">A házirendek áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="ab748-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="ab748-143">További információ a [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) a cikkben szereplő házirendek adja a következő videó megtekintése.</span><span class="sxs-lookup"><span data-stu-id="ab748-143">For more information on the [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

