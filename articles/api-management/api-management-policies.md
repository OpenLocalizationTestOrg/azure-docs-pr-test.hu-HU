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
# <a name="api-management-policies"></a>API Management házirendek
Ez a rész nyújt a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](api-management-howto-policies.md).  
  
 Házirendek, amelyek lehetővé teszik a hello publisher toochange hello viselkedését hello API konfigurálással hello rendszer hatékony képesség. Házirendek olyan hello kérésre egymás után végrehajtott utasítások gyűjteménye vagy egy API-t adott válaszokat. Népszerű utasítások XML tooJSON formátumú konverzió tartalmazza, és hívja meg a fejlesztők a bejövő hívások toorestrict hello mennyisége sebességével. Számos további házirendeket hello kezdő verzióról érhetők el.  
  
 Házirend-kifejezések használható attribútumértékek vagy szöveges értékek bármely hello API-felügyeleti házirendek, kivéve, ha hello házirend ellenkező esetben adja meg. Egyes házirendek, például a hello [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) és [Set változó](api-management-advanced-policies.md#set-variable) házirendek a házirend-kifejezések alapulnak. További információkért lásd: [házirendek speciális](api-management-advanced-policies.md#AdvancedPolicies) és [házirend-kifejezések](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a>Házirendek  
  
-   [Hozzáférés-korlátozási házirendek](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   [Ellenőrizze a HTTP-fejléc](api-management-access-restriction-policies.md#CheckHTTPHeader) -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.  
  
    -   [Előfizetési határértéket hívás arányt a](api-management-access-restriction-policies.md#LimitCallRate) -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.  
  
    -   [Korlát hívás arányt a kulcs](api-management-access-restriction-policies.md#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.  
  
    -   [A hívó IP-címek korlátozása](api-management-access-restriction-policies.md#RestrictCallerIPs) -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.  
  
    -   [Set memóriahasználati kvóta előfizetéssel](api-management-access-restriction-policies.md#SetUsageQuota) -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.  
  
    -   [Set memóriahasználati kvóta kulcs által](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.  
  
    -   [Ellenőrizze a JWT](api-management-access-restriction-policies.md#ValidateJWT) -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.  
  
-   [Speciális házirendek](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   [Folyamat szabályozása](api-management-advanced-policies.md#choose) - feltételesen hello értékelése logikai kifejezésen alapuló házirend-utasításoknál vonatkozik.  
  
    -   [Kérés továbbítása a](api-management-advanced-policies.md#ForwardRequest) -hello kérelem toohello háttérszolgáltatást továbbítja.  
  
    -   [TooEvent központ naplófájl](api-management-advanced-policies.md#log-to-eventhub) -üzeneteket küld a hello megadott formátumban tooa üzenetcél határozzák meg a tranzakciónaplókat tartalmazó entitás.  
  
    -   [Próbálja meg újra](api-management-advanced-policies.md#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig. Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.  
  
    -   [Térjen vissza a válasz](api-management-advanced-policies.md#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó.  
  
    -   [Egyirányú kérés küldése](api-management-advanced-policies.md#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.  
  
    -   [Kérés küldése](api-management-advanced-policies.md#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.  
  
    -   [Új érték](api-management-advanced-policies.md#set-variable) -megőrzéséhez a későbbi eléréshez elnevezett környezeti változóban értéket.  
  
    -   [Kérelem módszert állítja be](api-management-advanced-policies.md#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.  
  
    -   [Állítsa be. állapotkód:](api-management-advanced-policies.md#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.  
  
    -   [Nyomkövetési](api-management-advanced-policies.md#Trace) -karakterlánc hozzáadja hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.  
  
    -   [Várjon, amíg](api-management-advanced-policies.md#Wait) -megvárja-e a zárójelek között [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), vagy [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek toocomplete a folytatás előtt.  
  
-   [Hitelesítési házirendek](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   [Basic hitelesítés](api-management-authentication-policies.md#Basic) -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.  
  
    -   [Az ügyféltanúsítvány hitelesítéséhez](api-management-authentication-policies.md#ClientCertificate) -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.  
  
-   [Gyorsítótárazási házirendek](api-management-caching-policies.md#CachingPolicies)  
  
    -   [Gyorsítótár beszerezni](api-management-caching-policies.md#GetFromCache) -hajtsa végre a gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.  
  
    -   [Tárolja a toocache](api-management-caching-policies.md#StoreToCache) -gyorsítótárak válasz toohello szerint megadott gyorsítótár-vezérlő konfigurációját.  
  
    -   [Lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.  
  
    -   [A gyorsítótárban tárolja a érték](api-management-caching-policies.md#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.  
  
    -   [Távolítsa el az értéket a gyorsítótárból](api-management-caching-policies.md#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.  
  
-   [Tartományközi házirendek](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   [Lehetővé teszi a tartományok közötti hívások](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.  
  
    -   [A CORS](api-management-cross-domain-policies.md#CORS) -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.  
  
    -   [JSONP](api-management-cross-domain-policies.md#JSONP) - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.  
  
-   [Átalakítási házirendek](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   [Alakítsa át a JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - konvertálja kérés vagy válasz JSON tooXML a body.  
  
    -   [Átalakítás XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - konvertálja kérés vagy válasz XML tooJSON a body.  
  
    -   [Keresés és csere törzsében karakterlánc](api-management-transformation-policies.md#Findandreplacestringinbody) - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.  
  
    -   [A tartalom URL-címek maszkolnia](api-management-transformation-policies.md#MaskURLSContent) -átírja (maszkok) hivatkozások hello válaszul body, így a pontok toohello egyenértékű hivatkozás hello átjárón keresztül.  
  
    -   [Állítsa be háttérszolgáltatás](api-management-transformation-policies.md#SetBackendService) -hello háttérszolgáltatást egy bejövő kérelemhez változik.  
  
    -   [Állítsa be a szervezet](api-management-transformation-policies.md#SetBody) – beállítja a bejövő és kimenő kérelmek hello az üzenet törzse.  
  
    -   [Set HTTP-fejléc](api-management-transformation-policies.md#SetHTTPheader) - hozzárendel egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.  
  
    -   [Állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.  
  
    -   [URL-cím újraírása](api-management-transformation-policies.md#RewriteURL) -nyilvános űrlap toohello formájukban hello webszolgáltatás által várt a kérelem URL-cím alakítja.  
  
    -   [Átalakítás XSLT használatával XML](api-management-transformation-policies.md#XSLTransform) -egy XSL átalakítása tooXML hello kérés vagy válasz törzsében vonatkozik.  
  
## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  
