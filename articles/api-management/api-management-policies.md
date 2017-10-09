---
title: "API-felügyeleti házirendek aaaAzure |} Microsoft Docs"
description: "További tudnivalók a hello házirendekről használható az Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 1cc460cb-8e67-41aa-bc76-bbafc1892798
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1c468ff37d73359f1dd694b91e20c2ca04f8934e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policies"></a><span data-ttu-id="c97c9-103">API Management házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-103">API Management policies</span></span>
<span data-ttu-id="c97c9-104">Ez a rész nyújt a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="c97c9-104">This section provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="c97c9-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c97c9-105">For information on adding and configuring policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
  
 <span data-ttu-id="c97c9-106">Házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség.</span><span class="sxs-lookup"><span data-stu-id="c97c9-106">Policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="c97c9-107">Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="c97c9-107">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="c97c9-108">Népszerű utasítások XML tooJSON formátumú konverzió tartalmazza, és hívja meg a fejlesztők a bejövő hívások toorestrict hello mennyisége sebességével.</span><span class="sxs-lookup"><span data-stu-id="c97c9-108">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="c97c9-109">Számos további házirendeket hello kezdő verzióról érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c97c9-109">Many more policies are available out of hello box.</span></span>  
  
 <span data-ttu-id="c97c9-110">Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg.</span><span class="sxs-lookup"><span data-stu-id="c97c9-110">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="c97c9-111">Egyes házirendek, például a hello [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) és [Set változó](api-management-advanced-policies.md#set-variable) házirendek a házirend-kifejezések alapulnak.</span><span class="sxs-lookup"><span data-stu-id="c97c9-111">Some policies such as hello [Control flow](api-management-advanced-policies.md#choose) and [Set variable](api-management-advanced-policies.md#set-variable) policies are based on policy expressions.</span></span> <span data-ttu-id="c97c9-112">További információkért lásd: [házirendek speciális](api-management-advanced-policies.md#AdvancedPolicies) és [házirend-kifejezések](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="c97c9-112">For more information, see [Advanced policies](api-management-advanced-policies.md#AdvancedPolicies) and [Policy expressions](api-management-policy-expressions.md).</span></span>  
  
##  <span data-ttu-id="c97c9-113"><a name="ProxyPolicies"></a>Házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-113"><a name="ProxyPolicies"></a> Policies</span></span>  
  
-   [<span data-ttu-id="c97c9-114">Hozzáférés-korlátozási házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-114">Access restriction policies</span></span>](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   <span data-ttu-id="c97c9-115">[Ellenőrizze a HTTP-fejléc](api-management-access-restriction-policies.md#CheckHTTPHeader) -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="c97c9-115">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
    -   <span data-ttu-id="c97c9-116">[Előfizetési határértéket hívás arányt a](api-management-access-restriction-policies.md#LimitCallRate) -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="c97c9-116">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="c97c9-117">[Korlát hívás arányt a kulcs](api-management-access-restriction-policies.md#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="c97c9-117">[Limit call rate by key](api-management-access-restriction-policies.md#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="c97c9-118">[A hívó IP-címek korlátozása](api-management-access-restriction-policies.md#RestrictCallerIPs) -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="c97c9-118">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
    -   <span data-ttu-id="c97c9-119">[Set memóriahasználati kvóta előfizetéssel](api-management-access-restriction-policies.md#SetUsageQuota) -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="c97c9-119">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="c97c9-120">[Set memóriahasználati kvóta kulcs által](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="c97c9-120">[Set usage quota by key](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="c97c9-121">[Ellenőrizze a JWT](api-management-access-restriction-policies.md#ValidateJWT) -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.</span><span class="sxs-lookup"><span data-stu-id="c97c9-121">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
-   [<span data-ttu-id="c97c9-122">Speciális házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-122">Advanced policies</span></span>](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   <span data-ttu-id="c97c9-123">[Folyamat szabályozása](api-management-advanced-policies.md#choose) - feltételesen hello értékelése logikai kifejezésen alapuló házirend-utasításoknál vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c97c9-123">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello evaluation of Boolean expressions.</span></span>  
  
    -   <span data-ttu-id="c97c9-124">[Kérés továbbítása a](api-management-advanced-policies.md#ForwardRequest) -hello kérelem toohello háttérszolgáltatást továbbítja.</span><span class="sxs-lookup"><span data-stu-id="c97c9-124">[Forward request](api-management-advanced-policies.md#ForwardRequest) - Forwards hello request toohello backend service.</span></span>  
  
    -   <span data-ttu-id="c97c9-125">[TooEvent központ naplófájl](api-management-advanced-policies.md#log-to-eventhub) -üzeneteket küld a hello megadott formátumban tooa üzenetcél határozzák meg a tranzakciónaplókat tartalmazó entitás.</span><span class="sxs-lookup"><span data-stu-id="c97c9-125">[Log tooEvent Hub](api-management-advanced-policies.md#log-to-eventhub) - Sends messages in hello specified format tooa message target defined by a Logger entity.</span></span>  
  
    -   <span data-ttu-id="c97c9-126">[Próbálja meg újra](api-management-advanced-policies.md#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig.</span><span class="sxs-lookup"><span data-stu-id="c97c9-126">[Retry](api-management-advanced-policies.md#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="c97c9-127">Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="c97c9-127">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
    -   <span data-ttu-id="c97c9-128">[Térjen vissza a válasz](api-management-advanced-policies.md#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="c97c9-128">[Return response](api-management-advanced-policies.md#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>  
  
    -   <span data-ttu-id="c97c9-129">[Egyirányú kérés küldése](api-management-advanced-policies.md#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="c97c9-129">[Send one way request](api-management-advanced-policies.md#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
    -   <span data-ttu-id="c97c9-130">[Kérés küldése](api-management-advanced-policies.md#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c97c9-130">[Send request](api-management-advanced-policies.md#SendRequest) - Sends a request toohello specified URL.</span></span>  
  
    -   <span data-ttu-id="c97c9-131">[Új érték](api-management-advanced-policies.md#set-variable) -megőrzéséhez a későbbi eléréshez elnevezett környezeti változóban értéket.</span><span class="sxs-lookup"><span data-stu-id="c97c9-131">[Set variable](api-management-advanced-policies.md#set-variable) - Persist a value in a named context variable for later access.</span></span>  
  
    -   <span data-ttu-id="c97c9-132">[Kérelem módszert állítja be](api-management-advanced-policies.md#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.</span><span class="sxs-lookup"><span data-stu-id="c97c9-132">[Set request method](api-management-advanced-policies.md#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
    -   <span data-ttu-id="c97c9-133">[Állítsa be. állapotkód:](api-management-advanced-policies.md#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.</span><span class="sxs-lookup"><span data-stu-id="c97c9-133">[Set status code](api-management-advanced-policies.md#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
    -   <span data-ttu-id="c97c9-134">[Nyomkövetési](api-management-advanced-policies.md#Trace) -karakterlánc hozzáadja hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c97c9-134">[Trace](api-management-advanced-policies.md#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
    -   <span data-ttu-id="c97c9-135">[Várjon, amíg](api-management-advanced-policies.md#Wait) -megvárja-e a zárójelek között [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), vagy [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek toocomplete a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="c97c9-135">[Wait](api-management-advanced-policies.md#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
-   [<span data-ttu-id="c97c9-136">Hitelesítési házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-136">Authentication policies</span></span>](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   <span data-ttu-id="c97c9-137">[Basic hitelesítés](api-management-authentication-policies.md#Basic) -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c97c9-137">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
    -   <span data-ttu-id="c97c9-138">[Az ügyféltanúsítvány hitelesítéséhez](api-management-authentication-policies.md#ClientCertificate) -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c97c9-138">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
-   [<span data-ttu-id="c97c9-139">Gyorsítótárazási házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-139">Caching policies</span></span>](api-management-caching-policies.md#CachingPolicies)  
  
    -   <span data-ttu-id="c97c9-140">[Gyorsítótár beszerezni](api-management-caching-policies.md#GetFromCache) -hajtsa végre a gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="c97c9-140">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached response when available.</span></span>  
  
    -   <span data-ttu-id="c97c9-141">[Tárolja a toocache](api-management-caching-policies.md#StoreToCache) -gyorsítótárak válasz toohello szerint megadott gyorsítótár-vezérlő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="c97c9-141">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches response according toohello specified cache control configuration.</span></span>  
  
    -   <span data-ttu-id="c97c9-142">[Lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c97c9-142">[Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="c97c9-143">[A gyorsítótárban tárolja a érték](api-management-caching-policies.md#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="c97c9-143">[Store value in cache](api-management-caching-policies.md#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="c97c9-144">[Távolítsa el az értéket a gyorsítótárból](api-management-caching-policies.md#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="c97c9-144">[Remove value from cache](api-management-caching-policies.md#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
-   [<span data-ttu-id="c97c9-145">Tartományközi házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-145">Cross domain policies</span></span>](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   <span data-ttu-id="c97c9-146">[Lehetővé teszi a tartományok közötti hívások](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="c97c9-146">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
    -   <span data-ttu-id="c97c9-147">[A CORS](api-management-cross-domain-policies.md#CORS) -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.</span><span class="sxs-lookup"><span data-stu-id="c97c9-147">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
    -   <span data-ttu-id="c97c9-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.</span><span class="sxs-lookup"><span data-stu-id="c97c9-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
-   [<span data-ttu-id="c97c9-149">Átalakítási házirendek</span><span class="sxs-lookup"><span data-stu-id="c97c9-149">Transformation policies</span></span>](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   <span data-ttu-id="c97c9-150">[Alakítsa át a JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - konvertálja kérés vagy válasz JSON tooXML a body.</span><span class="sxs-lookup"><span data-stu-id="c97c9-150">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
    -   <span data-ttu-id="c97c9-151">[Átalakítás XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - konvertálja kérés vagy válasz XML tooJSON a body.</span><span class="sxs-lookup"><span data-stu-id="c97c9-151">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
    -   <span data-ttu-id="c97c9-152">[Keresés és csere törzsében karakterlánc](api-management-transformation-policies.md#Findandreplacestringinbody) - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="c97c9-152">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
    -   <span data-ttu-id="c97c9-153">[A tartalom URL-címek maszkolnia](api-management-transformation-policies.md#MaskURLSContent) -átírja (maszkok) hivatkozások hello válaszul body, így a pontok toohello egyenértékű hivatkozás hello átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="c97c9-153">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
    -   <span data-ttu-id="c97c9-154">[Állítsa be háttérszolgáltatás](api-management-transformation-policies.md#SetBackendService) -hello háttérszolgáltatást egy bejövő kérelemhez változik.</span><span class="sxs-lookup"><span data-stu-id="c97c9-154">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
    -   <span data-ttu-id="c97c9-155">[Állítsa be a szervezet](api-management-transformation-policies.md#SetBody) – beállítja a bejövő és kimenő kérelmek hello az üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="c97c9-155">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
    -   <span data-ttu-id="c97c9-156">[Set HTTP-fejléc](api-management-transformation-policies.md#SetHTTPheader) - hozzárendel egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="c97c9-156">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
    -   <span data-ttu-id="c97c9-157">[Állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c97c9-157">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
    -   <span data-ttu-id="c97c9-158">[URL-cím újraírása](api-management-transformation-policies.md#RewriteURL) -nyilvános űrlap toohello formájukban hello webszolgáltatás által várt a kérelem URL-cím alakítja.</span><span class="sxs-lookup"><span data-stu-id="c97c9-158">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
    -   <span data-ttu-id="c97c9-159">[Átalakítás XSLT használatával XML](api-management-transformation-policies.md#XSLTransform) -egy XSL átalakítása tooXML hello kérés vagy válasz törzsében vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c97c9-159">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="c97c9-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c97c9-160">Next steps</span></span>
<span data-ttu-id="c97c9-161">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="c97c9-161">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
