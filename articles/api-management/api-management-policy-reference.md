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
# <a name="azure-api-management-policy-reference"></a>Az Azure API Management szabályzatainak ismertetése
Ez a témakör az index hello házirendek a hello [API-kezelési házirendjeihez][API Management policy reference]. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management][Policies in API Management].

Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg. Egyes házirendek, például a hello [folyamatot szabályozhatja] [ Control flow] és [Set változó] [ Set variable] házirendek a házirend-kifejezések alapulnak. További információkért lásd: [házirendek speciális] [ Advanced policies] és [házirend-kifejezések][Policy expressions]

## <a name="policy-reference-index"></a>Szabályzati segédlet indexe
* [Hozzáférés a szoftverkorlátozó házirendek][Access restriction policies]
  * [Ellenőrizze a HTTP-fejléc] [ Check HTTP header] -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.
  * [Előfizetési határértéket hívás arányt a] [ Limit call rate by subscription] -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.
  * [Korlát hívás arányt a kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.
  * [A hívó IP-címek korlátozása] [ Restrict caller IPs] -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.
  * [Set memóriahasználati kvóta előfizetéssel] [ Set usage quota by subscription] -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.
  * [Set memóriahasználati kvóta kulcs által](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.
  * [Ellenőrizze a JWT] [ Validate JWT] -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.
* [Speciális házirendek][Advanced policies]
  * [Folyamat szabályozása] [ Control flow] - feltételesen alkalmazza a házirend-utasításoknál logikai hello értékelése hello eredményei alapján [kifejezések][expressions].
  * [Kérés továbbítása a] [ Forward request] -hello kérelem toohello háttérszolgáltatást továbbítja.
  * [TooEvent központ naplófájl] [ Log tooEvent Hub] -üzeneteket küld a hello megadott formátumban tooa üzenetcél határozzák meg a [naplózó](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitás.
  * [Próbálja meg újra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig. Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.
  * [Térjen vissza a válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó.
  * [Egyirányú kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.
  * [Kérés küldése](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.
  * [Kérelem módszert állítja be](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.
  * [Állapot beállítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.
  * [Új érték] [ Set variable] -értéket az elnevezett megőrzéséhez [környezetben] [ context] változó későbbi eléréshez.
  * [Nyomkövetési](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -karakterlánc hozzáadja hello [API Inspector](api-management-howto-api-inspector.md) kimeneti.
  * [Várjon, amíg](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - vár a zárt küldési kérelmek, lehet értéket kiolvasni a gyorsítótár, vagy szabályozhatja a folyamat házirendek toocomplete a folytatás előtt.
* [Hitelesítési házirendek][Authentication policies]
  * [Basic hitelesítés] [ Authenticate with Basic] -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.
  * [Az ügyféltanúsítvány hitelesítéséhez] [ Authenticate with client certificate] -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.
* [Házirendek gyorsítótárazása][Caching policies] 
  * [Gyorsítótár beszerezni] [ Get from cache] -hajtsa végre a gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.
  * [Tárolja a toocache] [ Store toocache] -gyorsítótárak válasz toohello szerint megadott gyorsítótár-vezérlő konfigurációját.
  * [Lehet értéket kiolvasni a gyorsítótár](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.
  * [A gyorsítótárban tárolja a érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.
  * [Távolítsa el az értéket a gyorsítótárból](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.
* [Kereszt-tartományi házirendek][Cross domain policies] 
  * [Lehetővé teszi a tartományok közötti hívások] [ Allow cross-domain calls] -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.
  * [A CORS] [ CORS] -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.
  * [JSONP] [ JSONP] - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.
* [Átalakítási csoportházirendek][Transformation policies] 
  * [Alakítsa át a JSON tooXML] [ Convert JSON tooXML] - konvertálja kérés vagy válasz JSON tooXML a body.
  * [Átalakítás XML tooJSON] [ Convert XML tooJSON] - konvertálja kérés vagy válasz XML tooJSON a body.
  * [Keresés és csere törzsében karakterlánc] [ Find and replace string in body] - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.
  * [A tartalom URL-címek maszkolnia] [ Mask URLs in content] -átírja (maszkok) hivatkozások hello válaszul body, így a pontok toohello egyenértékű hivatkozás hello átjárón keresztül.
  * [Állítsa be háttérszolgáltatás] [ Set backend service] -hello háttérszolgáltatást egy bejövő kérelemhez változik.
  * [Állítsa be a szervezet] [ Set body] – beállítja a bejövő és kimenő kérelmek hello az üzenet törzse.
  * [Set HTTP-fejléc] [ Set HTTP header] - hozzárendel egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.
  * [Állítsa be a lekérdezési karakterlánc paraméter] [ Set query string parameter] - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.
  * [URL-cím újraírása] [ Rewrite URL] -nyilvános űrlap toohello formájukban hello webszolgáltatás által várt a kérelem URL-cím alakítja.

## <a name="next-steps"></a>Következő lépések
Házirend-kifejezések további információkért tekintse meg a következő videó hello.

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


