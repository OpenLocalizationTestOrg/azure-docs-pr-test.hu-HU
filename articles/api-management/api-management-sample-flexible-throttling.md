---
title: "az Azure API Management-szabályozás aaaAdvanced kérelem"
description: "Megtudhatja, hogyan toocreate és rugalmas kvóta és sebessége korlátozza az Azure API-felügyeleti házirendek vonatkoznak."
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
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="cfc8b-103">Az Azure API Management-szabályozás Speciális kérelem</span><span class="sxs-lookup"><span data-stu-id="cfc8b-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="cfc8b-104">Az Azure API Management kulcsfontosságú szerepet képes toothrottle bejövő kérelmek folyamatban.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-104">Being able toothrottle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="cfc8b-105">Vagy ellenőrző hello gyakorisága a kéréseket, és hello teljes kérelmek/adatokat továbbít, az API Management lehetővé teszi, hogy API szolgáltatók tooprotect az API-kat visszaélés, és hozzon létre külön API termék rétegekhez értékét.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-105">Either by controlling hello rate of requests or hello total requests/data transferred, API Management allows API providers tooprotect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="cfc8b-106">A termék alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-106">Product based throttling</span></span>
<span data-ttu-id="cfc8b-107">toodate, szabályozására hello sebessége korlátozott hatókörű toobeing tooa adott termék-előfizetéshez (lényegében egy kulcs) lett, meghatározott hello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-107">toodate, hello rate throttling capabilities have been limited toobeing scoped tooa particular Product subscription (essentially a key), defined in hello API Management publisher portal.</span></span> <span data-ttu-id="cfc8b-108">Ez akkor hasznos, ha hello API szolgáltató tooapply vonatkozó korlátozások feliratkozott toouse API hello fejlesztőknek, azonban ez nem működik, például az egyes végfelhasználói hello API-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-108">This is useful for hello API provider tooapply limits on hello developers who have signed up toouse their API, however, it does not help, for example, in throttling individual end-users of hello API.</span></span> <span data-ttu-id="cfc8b-109">Lehetséges, hogy a hello fejlesztői alkalmazás tooconsume-egy felhasználóhoz hello teljes kvóta, majd előfordulhat, hogy más ügyfelek hello fejlesztő képes toouse hello alkalmazás folyamatban.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-109">It is possible that for single user of hello developer's application tooconsume hello entire quota and then prevent other customers of hello developer from being able toouse hello application.</span></span> <span data-ttu-id="cfc8b-110">Is több olyan ügyfelek, akik nagy mennyiségű kérést hozhat létre korlátozhatja a hozzáférést toooccasional felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-110">Also, several customers who might generate a high volume of requests may limit access toooccasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="cfc8b-111">Egyéni kulcs alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-111">Custom key based throttling</span></span>
<span data-ttu-id="cfc8b-112">új hello [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek biztosítják a megoldás jóval rugalmasabb tootraffic vezérlő.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-112">hello new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution tootraffic control.</span></span> <span data-ttu-id="cfc8b-113">Ezeket az új házirendeket toodefine kifejezések tooidentify hello kulcsok használt tootrack forgalom használati leendő engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-113">These new policies allow you toodefine expressions tooidentify hello keys that will be used tootrack traffic usage.</span></span> <span data-ttu-id="cfc8b-114">hello működésének ez easiest látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-114">hello way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="cfc8b-115">IP-cím sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-115">IP Address throttling</span></span>
<span data-ttu-id="cfc8b-116">hello alábbi házirendek korlátozása egyetlen ügyfél IP-cím tooonly 10 percenként összesen 1 000 000 hívások és 10 000 kilobájt havonta sávszélesség hívja.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-116">hello following policies restrict a single client IP address tooonly 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="cfc8b-117">Összes ügyfél a hello Internet egy egyedi IP-címet használja, ha ez a felhasználó használat korlátozása hatékony módszer lehet.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-117">If all clients on hello Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="cfc8b-118">Azonban ez nagyon valószínű, hogy több felhasználó fog-e a megosztás toothem elérése során hello Internet NAT-eszközön keresztül miatt egyetlen nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-118">However, it is quite likely that multiple users will sharing a single public IP address due toothem accessing hello Internet via a NAT device.</span></span> <span data-ttu-id="cfc8b-119">Annak ellenére, hogy az API-k esetében, amelyek lehetővé teszik a nem hitelesített hozzáférést hello `IpAddress` hello-érdemes lehet.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-119">Despite this, for APIs that allow unauthenticated access hello `IpAddress` might be hello best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="cfc8b-120">Felhasználói azonosító sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-120">User identity throttling</span></span>
<span data-ttu-id="cfc8b-121">Ha a felhasználó hitelesítése, akkor a sávszélesség-szabályozási kulcs hozható létre, amely egyedileg azonosít egy, amely adatok alapján felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="cfc8b-122">Ebben a példában a Microsoft hello Authorization fejlécet kiolvasni, alakítsa át a túl`JWT` objektumra és hello token tooidentify hello felhasználó hello tulajdonos, és használhassa a kulcs hello sebességével.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-122">In this example we extract hello Authorization header, convert it too`JWT` object and use hello subject of hello token tooidentify hello user and use that as hello rate limiting key.</span></span> <span data-ttu-id="cfc8b-123">Ha a hello hello felhasználói identitás `JWT` , más hello egyik jogcímek majd, hogy az érték volt használható helyette.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-123">If hello user identity is stored in hello `JWT` as one of hello other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="cfc8b-124">Kombinált házirendek</span><span class="sxs-lookup"><span data-stu-id="cfc8b-124">Combined policies</span></span>
<span data-ttu-id="cfc8b-125">Új szabályozása házirendekkel hello adja meg a szabályozási házirendek meglévő hello-nál nagyobb mértékben vezérelheti, bár nincs továbbra is érték kombinálásával mindkét szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-125">Although hello new throttling policies provide more control than hello existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="cfc8b-126">Termékkulcs előfizetés általi szabályozás ([korlát hívás arányt a előfizetés](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [Set memóriahasználati kvóta előfizetéssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) használati szintek alapján úgy egy API monetizing tooenable remek módja.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way tooenable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="cfc8b-127">hello eszközzel kombinálva felhasználóbarát nyomtatott, hogy a felhasználó képes toothrottle kiegészítő és megakadályozza, hogy egy felhasználó viselkedésének hello élménye egy másik terhelése.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-127">hello finer grained control of being able toothrottle by user is complementary and prevents one user's behavior from degrading hello experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="cfc8b-128">Ügyfél-alapú sávszélesség-szabályozás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-128">Client driven throttling</span></span>
<span data-ttu-id="cfc8b-129">Ha kulcs-szabályozás hello definiálva van a használatával egy [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx), akkor célszerű hello API szolgáltató, amely megválasztása a módját a sávszélesség-szabályozás hello hatókörébe tartozik-e.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-129">When hello throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is hello API provider that is choosing how hello throttling is scoped.</span></span> <span data-ttu-id="cfc8b-130">Azonban érdemes egy fejlesztő hogyan értékelje toocontrol korlátozzák a saját ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-130">However, a developer might want toocontrol how they rate limit their own customers.</span></span> <span data-ttu-id="cfc8b-131">Ez sikerült engedélyezni a hello API-szolgáltató egy egyéni fejlécet tooallow hello fejlesztői ügyfél alkalmazás toocommunicate hello kulcs toohello API szemben.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-131">This could be enabled by hello API provider by introducing a custom header tooallow hello developer's client application toocommunicate hello key toohello API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="cfc8b-132">Ez lehetővé teszi a fejlesztői hello ügyfél alkalmazás toochoose toocreate hello sebességével kulcs módját.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-132">This enables hello developer's client application toochoose how they want toocreate hello rate limiting key.</span></span> <span data-ttu-id="cfc8b-133">Egy kis nyújt az ügyfél a fejlesztők kulcsok toousers készleteinek hozzárendelésének és elforgatása hello kulcshasználat létrehozhat saját rétegek.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys toousers and rotating hello key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="cfc8b-134">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cfc8b-134">Summary</span></span>
<span data-ttu-id="cfc8b-135">Az Azure API Management sebesség és a sávszélesség-szabályozás tooboth ajánlat védelmét, és adja hozzá a tooyour API szolgáltatás biztosít.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-135">Azure API Management provides rate and quote throttling tooboth protect and add value tooyour API service.</span></span> <span data-ttu-id="cfc8b-136">új sávszélesség-szabályozás egyéni szabályának szabályzatok hello engedélyezi, akkor pontosabban megbízhatatlanná tudják befolyásolni ezen házirendek tooenable az ügyfelek toobuild még élvezetesebbé alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-136">hello new throttling policies with custom scoping rules allow you finer grained control over those policies tooenable your customers toobuild even better applications.</span></span> <span data-ttu-id="cfc8b-137">hello a cikkben szereplő példák bemutatják, ezeket az új házirendeket hello használata korlátozza az ügyfél IP-címeket, a felhasználói azonosító és a generált ügyfél értékek kulcsok gyártó szerint.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-137">hello examples in this article demonstrate hello use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="cfc8b-138">Vannak azonban sok más részei üdvözlőüzenetére, például a felhasználói ügynök, az URL-cím elérési út töredék, az üzenet mérete használható.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-138">However, there are many other parts of hello message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfc8b-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfc8b-139">Next steps</span></span>
<span data-ttu-id="cfc8b-140">Adja meg a visszajelzést hello disqus-beszélgetésben teheti szálon az ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-140">Please give us your feedback in hello Disqus thread for this topic.</span></span> <span data-ttu-id="cfc8b-141">Nagyszerű toohear kapcsolatos egyéb lehetséges értékek, amelyek egy logikai választás a forgatókönyvekben lettek volna.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-141">It would be great toohear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="cfc8b-142">A házirendek áttekintő videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="cfc8b-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="cfc8b-143">Hello bővebben [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) a cikkben szereplő házirendek adjon tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="cfc8b-143">For more information on hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

