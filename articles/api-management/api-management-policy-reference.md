---
title: "aaaAzure API-felügyeleti szabályzatainak ismertetése"
description: "További információk a hello házirendek elérhető tooconfigure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7ee194f4d6f172bf836c789cbe8ab732e18a7e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-policy-reference"></a><span data-ttu-id="60199-103">Az Azure API Management szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="60199-103">Azure API Management Policy Reference</span></span>
<span data-ttu-id="60199-104">Ez a témakör az index hello házirendek a hello [API-kezelési házirendjeihez][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="60199-104">This section provides an index for hello policies in hello [API Management policy reference][API Management policy reference].</span></span> <span data-ttu-id="60199-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management][Policies in API Management].</span><span class="sxs-lookup"><span data-stu-id="60199-105">For information on adding and configuring policies, see [Policies in API Management][Policies in API Management].</span></span>

<span data-ttu-id="60199-106">Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg.</span><span class="sxs-lookup"><span data-stu-id="60199-106">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="60199-107">Egyes házirendek, például a hello [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak.</span><span class="sxs-lookup"><span data-stu-id="60199-107">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="60199-108">További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions]</span><span class="sxs-lookup"><span data-stu-id="60199-108">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions]</span></span>

## <a name="policy-reference-index"></a><span data-ttu-id="60199-109">Szabályzati segédlet indexe</span><span class="sxs-lookup"><span data-stu-id="60199-109">Policy reference index</span></span>
* <span data-ttu-id="60199-110">[Hozzáférés a szoftverkorlátozó házirendek][Access restriction policies]</span><span class="sxs-lookup"><span data-stu-id="60199-110">[Access restriction policies][Access restriction policies]</span></span>
  * <span data-ttu-id="60199-111">[Ellenőrizze a HTTP-fejléc] [ Check HTTP header] -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="60199-111">[Check HTTP header][Check HTTP header] - Enforces existence and/or value of a HTTP Header.</span></span>
  * <span data-ttu-id="60199-112">[Előfizetési határértéket hívás arányt a] [ Limit call rate by subscription] -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="60199-112">[Limit call rate by subscription][Limit call rate by subscription] - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>
  * <span data-ttu-id="60199-113">[Korlát hívás arányt a kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="60199-113">[Limit call rate by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>
  * <span data-ttu-id="60199-114">[A hívó IP-címek korlátozása] [ Restrict caller IPs] -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="60199-114">[Restrict caller IPs][Restrict caller IPs] - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>
  * <span data-ttu-id="60199-115">[Set memóriahasználati kvóta előfizetéssel] [ Set usage quota by subscription] -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="60199-115">[Set usage quota by subscription][Set usage quota by subscription] - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>
  * <span data-ttu-id="60199-116">[Set memóriahasználati kvóta kulcs által](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="60199-116">[Set usage quota by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>
  * <span data-ttu-id="60199-117">[Ellenőrizze a JWT] [ Validate JWT] -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.</span><span class="sxs-lookup"><span data-stu-id="60199-117">[Validate JWT][Validate JWT] - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>
* <span data-ttu-id="60199-118">[Speciális házirendek][Advanced policies]</span><span class="sxs-lookup"><span data-stu-id="60199-118">[Advanced policies][Advanced policies]</span></span>
  * <span data-ttu-id="60199-119">[Folyamat szabályozása] [ Control flow] - feltételesen alkalmazza a házirend-utasításoknál logikai hello értékelése hello eredményei alapján [kifejezések][expressions].</span><span class="sxs-lookup"><span data-stu-id="60199-119">[Control flow][Control flow] - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions][expressions].</span></span>
  * <span data-ttu-id="60199-120">[Kérés továbbítása a] [ Forward request] -hello kérelem toohello háttérszolgáltatást továbbítja.</span><span class="sxs-lookup"><span data-stu-id="60199-120">[Forward request][Forward request] - Forwards hello request toohello backend service.</span></span>
  * <span data-ttu-id="60199-121">[TooEvent központ naplófájl] [ Log tooEvent Hub] -üzeneteket küld a hello megadott formátumban tooa üzenetcél határozzák meg a [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitás.</span><span class="sxs-lookup"><span data-stu-id="60199-121">[Log tooEvent Hub][Log tooEvent Hub] - Sends messages in hello specified format tooa message target defined by a [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span></span>
  * <span data-ttu-id="60199-122">[Próbálja meg újra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig.</span><span class="sxs-lookup"><span data-stu-id="60199-122">[Retry](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="60199-123">Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="60199-123">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>
  * <span data-ttu-id="60199-124">[Térjen vissza a válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="60199-124">[Return response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>
  * <span data-ttu-id="60199-125">[Egyirányú kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="60199-125">[Send one way request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>
  * <span data-ttu-id="60199-126">[Kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="60199-126">[Send request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - Sends a request toohello specified URL.</span></span>
  * <span data-ttu-id="60199-127">[Kérelem módszert állítja be](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.</span><span class="sxs-lookup"><span data-stu-id="60199-127">[Set request method](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>
  * <span data-ttu-id="60199-128">[Állapot beállítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.</span><span class="sxs-lookup"><span data-stu-id="60199-128">[Set status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>
  * <span data-ttu-id="60199-129">[Új érték] [ Set variable] -értéket az elnevezett megőrzéséhez [környezetben] [ context] változó későbbi eléréshez.</span><span class="sxs-lookup"><span data-stu-id="60199-129">[Set variable][Set variable] - Persist a value in a named [context][context] variable for later access.</span></span>
  * <span data-ttu-id="60199-130">[Nyomkövetési](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -karakterlánc hozzáadja hello [API Inspector](api-management-howto-api-inspector.md) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="60199-130">[Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Adds a string into hello [API Inspector](api-management-howto-api-inspector.md) output.</span></span>
  * <span data-ttu-id="60199-131">[Várjon, amíg](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - vár a zárt küldési kérelmek, lehet értéket kiolvasni a gyorsítótár, vagy szabályozhatja a folyamat házirendek toocomplete a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="60199-131">[Wait](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - Waits for enclosed Send request, Get value from cache, or Control flow policies toocomplete before proceeding.</span></span>
* <span data-ttu-id="60199-132">[Hitelesítési házirendek][Authentication policies]</span><span class="sxs-lookup"><span data-stu-id="60199-132">[Authentication policies][Authentication policies]</span></span>
  * <span data-ttu-id="60199-133">[Basic hitelesítés] [ Authenticate with Basic] -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="60199-133">[Authenticate with Basic][Authenticate with Basic] - Authenticate with a backend service using Basic authentication.</span></span>
  * <span data-ttu-id="60199-134">[Az ügyféltanúsítvány hitelesítéséhez] [ Authenticate with client certificate] -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="60199-134">[Authenticate with client certificate][Authenticate with client certificate] - Authenticate with a backend service using client certificates.</span></span>
* <span data-ttu-id="60199-135">[Házirendek gyorsítótárazása][Caching policies]</span><span class="sxs-lookup"><span data-stu-id="60199-135">[Caching policies][Caching policies]</span></span> 
  * <span data-ttu-id="60199-136">[Gyorsítótár beszerezni] [ Get from cache] -hajtsa végre a gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="60199-136">[Get from cache][Get from cache] - Perform cache look up and return a valid cached response when available.</span></span>
  * <span data-ttu-id="60199-137">[Tárolja a toocache] [ Store toocache] -gyorsítótárak válasz toohello szerint megadott gyorsítótár-vezérlő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="60199-137">[Store toocache][Store toocache] - Caches response according toohello specified cache control configuration.</span></span>
  * <span data-ttu-id="60199-138">[Lehet értéket kiolvasni a gyorsítótár](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="60199-138">[Get value from cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>
  * <span data-ttu-id="60199-139">[A gyorsítótárban tárolja a érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="60199-139">[Store value in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>
  * <span data-ttu-id="60199-140">[Távolítsa el az értéket a gyorsítótárból](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="60199-140">[Remove value from cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>
* <span data-ttu-id="60199-141">[Kereszt-tartományi házirendek][Cross domain policies]</span><span class="sxs-lookup"><span data-stu-id="60199-141">[Cross domain policies][Cross domain policies]</span></span> 
  * <span data-ttu-id="60199-142">[Lehetővé teszi a tartományok közötti hívások] [ Allow cross-domain calls] -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="60199-142">[Allow cross-domain calls][Allow cross-domain calls] - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>
  * <span data-ttu-id="60199-143">[A CORS] [ CORS] -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.</span><span class="sxs-lookup"><span data-stu-id="60199-143">[CORS][CORS] - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>
  * <span data-ttu-id="60199-144">[JSONP] [ JSONP] - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.</span><span class="sxs-lookup"><span data-stu-id="60199-144">[JSONP][JSONP] - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>
* <span data-ttu-id="60199-145">[Átalakítási csoportházirendek][Transformation policies]</span><span class="sxs-lookup"><span data-stu-id="60199-145">[Transformation policies][Transformation policies]</span></span> 
  * <span data-ttu-id="60199-146">[Alakítsa át a JSON tooXML] [ Convert JSON tooXML] - konvertálja kérés vagy válasz JSON tooXML a body.</span><span class="sxs-lookup"><span data-stu-id="60199-146">[Convert JSON tooXML][Convert JSON tooXML] - Converts request or response body from JSON tooXML.</span></span>
  * <span data-ttu-id="60199-147">[Átalakítás XML tooJSON] [ Convert XML tooJSON] - konvertálja kérés vagy válasz XML tooJSON a body.</span><span class="sxs-lookup"><span data-stu-id="60199-147">[Convert XML tooJSON][Convert XML tooJSON] - Converts request or response body from XML tooJSON.</span></span>
  * <span data-ttu-id="60199-148">[Keresés és csere törzsében karakterlánc] [ Find and replace string in body] - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="60199-148">[Find and replace string in body][Find and replace string in body] - Finds a request or response substring and replaces it with a different substring.</span></span>
  * <span data-ttu-id="60199-149">[A tartalom URL-címek maszkolnia] [ Mask URLs in content] -átírja (maszkok) hivatkozások hello válaszul body, így a pontok toohello egyenértékű hivatkozás hello átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="60199-149">[Mask URLs in content][Mask URLs in content] - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>
  * <span data-ttu-id="60199-150">[Állítsa be háttérszolgáltatás] [ Set backend service] -hello háttérszolgáltatást egy bejövő kérelemhez változik.</span><span class="sxs-lookup"><span data-stu-id="60199-150">[Set backend service][Set backend service] - Changes hello backend service for an incoming request.</span></span>
  * <span data-ttu-id="60199-151">[Állítsa be a szervezet] [ Set body] – beállítja a bejövő és kimenő kérelmek hello az üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="60199-151">[Set body][Set body] - Sets hello message body for incoming and outgoing requests.</span></span>
  * <span data-ttu-id="60199-152">[Set HTTP-fejléc] [ Set HTTP header] - hozzárendel egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="60199-152">[Set HTTP header][Set HTTP header] - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>
  * <span data-ttu-id="60199-153">[Állítsa be a lekérdezési karakterlánc paraméter] [ Set query string parameter] - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="60199-153">[Set query string parameter][Set query string parameter] - Adds, replaces value of, or deletes request query string parameter.</span></span>
  * <span data-ttu-id="60199-154">[URL-cím újraírása] [ Rewrite URL] -nyilvános űrlap toohello formájukban hello webszolgáltatás által várt a kérelem URL-cím alakítja.</span><span class="sxs-lookup"><span data-stu-id="60199-154">[Rewrite URL][Rewrite URL] - Converts a request URL from its public form toohello form expected by hello web service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60199-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60199-155">Next steps</span></span>
<span data-ttu-id="60199-156">Házirend-kifejezések további információkért tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="60199-156">For more information on policy expressions, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log tooEvent Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store toocache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON tooXML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML tooJSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


