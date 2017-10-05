---
title: "Az Azure API Management szabályzatainak ismertetése"
description: "Ismerje meg az API Management konfigurálható házirendeket."
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
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-policy-reference"></a><span data-ttu-id="b5896-103">Az Azure API Management szabályzatainak ismertetése</span><span class="sxs-lookup"><span data-stu-id="b5896-103">Azure API Management Policy Reference</span></span>
<span data-ttu-id="b5896-104">Ez a témakör az index a házirendek a [API-kezelési házirendjeihez][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="b5896-104">This section provides an index for the policies in the [API Management policy reference][API Management policy reference].</span></span> <span data-ttu-id="b5896-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management][Policies in API Management].</span><span class="sxs-lookup"><span data-stu-id="b5896-105">For information on adding and configuring policies, see [Policies in API Management][Policies in API Management].</span></span>

<span data-ttu-id="b5896-106">A házirend-kifejezéseket attribútumértékekként vagy szövegértékekként lehet használni bármelyik API Management házirendben, hacsak a házirend másként nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b5896-106">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="b5896-107">Egyes házirendek, például a [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak.</span><span class="sxs-lookup"><span data-stu-id="b5896-107">Some policies such as the [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="b5896-108">További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions]</span><span class="sxs-lookup"><span data-stu-id="b5896-108">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions]</span></span>

## <a name="policy-reference-index"></a><span data-ttu-id="b5896-109">Szabályzati segédlet indexe</span><span class="sxs-lookup"><span data-stu-id="b5896-109">Policy reference index</span></span>
* <span data-ttu-id="b5896-110">[Hozzáférés a szoftverkorlátozó házirendek][Access restriction policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-110">[Access restriction policies][Access restriction policies]</span></span>
  * <span data-ttu-id="b5896-111">[Ellenőrizze a HTTP-fejléc] [ Check HTTP header] -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="b5896-111">[Check HTTP header][Check HTTP header] - Enforces existence and/or value of a HTTP Header.</span></span>
  * <span data-ttu-id="b5896-112">[Előfizetési határértéket hívás arányt a] [ Limit call rate by subscription] -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="b5896-112">[Limit call rate by subscription][Limit call rate by subscription] - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>
  * <span data-ttu-id="b5896-113">[Korlát hívás arányt a kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="b5896-113">[Limit call rate by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>
  * <span data-ttu-id="b5896-114">[A hívó IP-címek korlátozása] [ Restrict caller IPs] -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="b5896-114">[Restrict caller IPs][Restrict caller IPs] - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>
  * <span data-ttu-id="b5896-115">[Set memóriahasználati kvóta előfizetéssel] [ Set usage quota by subscription] -lehetővé teszi egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótát, egy előfizetés alapon érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="b5896-115">[Set usage quota by subscription][Set usage quota by subscription] - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>
  * <span data-ttu-id="b5896-116">[Set memóriahasználati kvóta kulcs által](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -lehetővé teszi egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótát, egy kulcs alapon érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="b5896-116">[Set usage quota by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>
  * <span data-ttu-id="b5896-117">[Ellenőrizze a JWT] [ Validate JWT] -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.</span><span class="sxs-lookup"><span data-stu-id="b5896-117">[Validate JWT][Validate JWT] - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>
* <span data-ttu-id="b5896-118">[Speciális házirendek][Advanced policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-118">[Advanced policies][Advanced policies]</span></span>
  * <span data-ttu-id="b5896-119">[Folyamat szabályozása] [ Control flow] - feltételesen alkalmazza a házirend-utasításoknál logikai értékelése eredményei alapján [kifejezések][expressions].</span><span class="sxs-lookup"><span data-stu-id="b5896-119">[Control flow][Control flow] - Conditionally applies policy statements based on the results of the evaluation of Boolean [expressions][expressions].</span></span>
  * <span data-ttu-id="b5896-120">[Kérés továbbítása a] [ Forward request] -továbbítja a kérést a háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5896-120">[Forward request][Forward request] - Forwards the request to the backend service.</span></span>
  * <span data-ttu-id="b5896-121">[Az Event Hubs napló] [ Log to Event Hub] -üzeneteket küld egy üzenetcél határozzák meg a megadott formátumban egy [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitás.</span><span class="sxs-lookup"><span data-stu-id="b5896-121">[Log to Event Hub][Log to Event Hub] - Sends messages in the specified format to a message target defined by a [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span></span>
  * <span data-ttu-id="b5896-122">[Próbálja meg újra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -újrapróbálkozások zárt házirend utasítás végrehajtása, ha, és amíg a feltétel nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="b5896-122">[Retry](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="b5896-123">Végrehajtási ismételje meg a megadott időközönként, és a legfeljebb a megadott újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="b5896-123">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>
  * <span data-ttu-id="b5896-124">[Térjen vissza a válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -megszakításainak-feldolgozási folyamat végrehajtása és a közvetlenül a hívó adott választ.</span><span class="sxs-lookup"><span data-stu-id="b5896-124">[Return response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span>
  * <span data-ttu-id="b5896-125">[Egyirányú kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -kérést küld a megadott URL-cím a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="b5896-125">[Send one way request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>
  * <span data-ttu-id="b5896-126">[Kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -kérést küld a megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="b5896-126">[Send request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - Sends a request to the specified URL.</span></span>
  * <span data-ttu-id="b5896-127">[Kérelem módszert állítja be](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -módosíthatja a kérelem HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="b5896-127">[Set request method](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>
  * <span data-ttu-id="b5896-128">[Állapot beállítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) – a HTTP-állapotkód: a megadott értékre változik.</span><span class="sxs-lookup"><span data-stu-id="b5896-128">[Set status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Changes the HTTP status code to the specified value.</span></span>
  * <span data-ttu-id="b5896-129">[Új érték] [ Set variable] -értéket az elnevezett megőrzéséhez [környezetben] [ context] változó későbbi eléréshez.</span><span class="sxs-lookup"><span data-stu-id="b5896-129">[Set variable][Set variable] - Persist a value in a named [context][context] variable for later access.</span></span>
  * <span data-ttu-id="b5896-130">[Nyomkövetési](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -be egy karakterláncot ad a [API Inspector](api-management-howto-api-inspector.md) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="b5896-130">[Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Adds a string into the [API Inspector](api-management-howto-api-inspector.md) output.</span></span>
  * <span data-ttu-id="b5896-131">[Várjon, amíg](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -vár a zárt küldési kérelem a Get érték a gyorsítótárból, vagy vezérlési folyamata házirendek befejeződjön, mielőtt továbblép.</span><span class="sxs-lookup"><span data-stu-id="b5896-131">[Wait](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - Waits for enclosed Send request, Get value from cache, or Control flow policies to complete before proceeding.</span></span>
* <span data-ttu-id="b5896-132">[Hitelesítési házirendek][Authentication policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-132">[Authentication policies][Authentication policies]</span></span>
  * <span data-ttu-id="b5896-133">[Basic hitelesítés] [ Authenticate with Basic] -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="b5896-133">[Authenticate with Basic][Authenticate with Basic] - Authenticate with a backend service using Basic authentication.</span></span>
  * <span data-ttu-id="b5896-134">[Az ügyféltanúsítvány hitelesítéséhez] [ Authenticate with client certificate] -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="b5896-134">[Authenticate with client certificate][Authenticate with client certificate] - Authenticate with a backend service using client certificates.</span></span>
* <span data-ttu-id="b5896-135">[Házirendek gyorsítótárazása][Caching policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-135">[Caching policies][Caching policies]</span></span> 
  * <span data-ttu-id="b5896-136">[Gyorsítótár beszerezni] [ Get from cache] -hajtsa végre a gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="b5896-136">[Get from cache][Get from cache] - Perform cache look up and return a valid cached response when available.</span></span>
  * <span data-ttu-id="b5896-137">[Gyorsítótárazandó tároló] [ Store to cache] -gyorsítótárazza a választ megadott gyorsítótár-vezérlő konfigurációjának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b5896-137">[Store to cache][Store to cache] - Caches response according to the specified cache control configuration.</span></span>
  * <span data-ttu-id="b5896-138">[Lehet értéket kiolvasni a gyorsítótár](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b5896-138">[Get value from cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>
  * <span data-ttu-id="b5896-139">[A gyorsítótárban tárolja a érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -tárolja egy elemet a gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="b5896-139">[Store value in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store an item in the cache by key.</span></span>
  * <span data-ttu-id="b5896-140">[Távolítsa el az értéket a gyorsítótárból](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -a gyorsítótárban, kulcs által távolítani egy elemet.</span><span class="sxs-lookup"><span data-stu-id="b5896-140">[Remove value from cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>
* <span data-ttu-id="b5896-141">[Kereszt-tartományi házirendek][Cross domain policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-141">[Cross domain policies][Cross domain policies]</span></span> 
  * <span data-ttu-id="b5896-142">[Lehetővé teszi a tartományok közötti hívások] [ Allow cross-domain calls] -elérhetővé válnak az API-t az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="b5896-142">[Allow cross-domain calls][Allow cross-domain calls] - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>
  * <span data-ttu-id="b5896-143">[A CORS] [ CORS] -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) a támogatási művelet vagy API lehetővé teszi a tartományok közötti hívások webböngésző-alapú ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="b5896-143">[CORS][CORS] - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>
  * <span data-ttu-id="b5896-144">[JSONP] [ JSONP] -JSON ad hozzá egy műveletet, vagy lehetővé teszi a tartományok közötti hívások JavaScript-ügyfelekből webböngésző-alapú API kitöltési (JSONP) támogatással.</span><span class="sxs-lookup"><span data-stu-id="b5896-144">[JSONP][JSONP] - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>
* <span data-ttu-id="b5896-145">[Átalakítási csoportházirendek][Transformation policies]</span><span class="sxs-lookup"><span data-stu-id="b5896-145">[Transformation policies][Transformation policies]</span></span> 
  * <span data-ttu-id="b5896-146">[JSON átalakítása XML] [ Convert JSON to XML] - konvertálja kérés vagy válasz body JSON formátumból az XML.</span><span class="sxs-lookup"><span data-stu-id="b5896-146">[Convert JSON to XML][Convert JSON to XML] - Converts request or response body from JSON to XML.</span></span>
  * <span data-ttu-id="b5896-147">[XML konvertálása JSON] [ Convert XML to JSON] - konvertálja kérés vagy válasz body az XML-JSON.</span><span class="sxs-lookup"><span data-stu-id="b5896-147">[Convert XML to JSON][Convert XML to JSON] - Converts request or response body from XML to JSON.</span></span>
  * <span data-ttu-id="b5896-148">[Keresés és csere törzsében karakterlánc] [ Find and replace string in body] - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="b5896-148">[Find and replace string in body][Find and replace string in body] - Finds a request or response substring and replaces it with a different substring.</span></span>
  * <span data-ttu-id="b5896-149">[URL-címek maszkolja a tartalom] [ Mask URLs in content] -hivatkozások átírja (maszkok) a válaszban szereplő body, hogy azok az átjárón keresztül a megfelelő hivatkozásra mutat.</span><span class="sxs-lookup"><span data-stu-id="b5896-149">[Mask URLs in content][Mask URLs in content] - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>
  * <span data-ttu-id="b5896-150">[Állítsa be háttérszolgáltatás] [ Set backend service] -módosítja a háttérszolgáltatáshoz egy bejövő kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="b5896-150">[Set backend service][Set backend service] - Changes the backend service for an incoming request.</span></span>
  * <span data-ttu-id="b5896-151">[Állítsa be a szervezet] [ Set body] -beállítja az üzenettörzs, a bejövő és kimenő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="b5896-151">[Set body][Set body] - Sets the message body for incoming and outgoing requests.</span></span>
  * <span data-ttu-id="b5896-152">[Set HTTP-fejléc] [ Set HTTP header] - hozzárendel egy értéket egy meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="b5896-152">[Set HTTP header][Set HTTP header] - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>
  * <span data-ttu-id="b5896-153">[Állítsa be a lekérdezési karakterlánc paraméter] [ Set query string parameter] - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="b5896-153">[Set query string parameter][Set query string parameter] - Adds, replaces value of, or deletes request query string parameter.</span></span>
  * <span data-ttu-id="b5896-154">[URL-cím újraírása] [ Rewrite URL] – a kérelem URL-címe nyilvános formájukban a várt érték a webszolgáltatás által az űrlap alakítja.</span><span class="sxs-lookup"><span data-stu-id="b5896-154">[Rewrite URL][Rewrite URL] - Converts a request URL from its public form to the form expected by the web service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5896-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5896-155">Next steps</span></span>
<span data-ttu-id="b5896-156">Házirend-kifejezések további információkért tekintse meg a következő videó.</span><span class="sxs-lookup"><span data-stu-id="b5896-156">For more information on policy expressions, see the following video.</span></span>

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
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
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


