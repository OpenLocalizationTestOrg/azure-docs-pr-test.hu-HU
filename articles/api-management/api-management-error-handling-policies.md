---
title: "az Azure API-felügyeleti házirendek kezelése aaaError |} Microsoft Docs"
description: "Ismerje meg, hogyan toorespond tooerror feltételek közben fellépő hello Azure API Management-ban található kérések feldolgozását."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 002c65f21e763fd644da61b6a11685ffd97488c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-api-management-policies"></a>Hiba történt az API-felügyeleti házirendek kezelése
Az Azure API Management lehetővé teszi, hogy közzétevők toorespond tooerror kérelmek toohello proxy hello feldolgozásakor fordulhat elő, adja meg a feltételeket a `ProxyError` objektum. Hello `ProxyError` objektum hello keresztül érhető el [a környezetben. Hiba](api-management-policy-expressions.md#ContextVariables) tulajdonság és hello szabályzatai által használható `on-error` házirend szakaszban. Ez a témakör nyújt a hello hiba kezelési képességek az Azure API Management.  
  
## <a name="error-handling-in-api-management"></a>Hiba történt az API Management kezelése  
 Az Azure API Management házirendek osztott `inbound`, `backend`, `outbound`, és `on-error` látható módon hello a következő példában szakaszok.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements toobe applied toohello request go here -->  
  </inbound>  
  <backend>  
    <!-- statements toobe applied before hello request is   
         forwarded toohello backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements toobe applied toohello response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements toobe applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
 A kérelem hello a feldolgozás során beépített lépéseit együtt hello kérelem hatókörébe házirendekben hajtja végre. Ha hiba lép fel, egyszer feldolgozása azonnal toohello `on-error` házirend szakaszban. Hello `on-error` házirend szakasz használható bármely hatókörű, és API-közzétevők konfigurálhatja a naplózás hello hiba tooevent hubok, vagy hozzon létre egy új válasz tooreturn toohello hívó például egyéni működését.  
  
> [!NOTE]
>  Hello `on-error` szakasz nincs jelen a házirendek alapértelmezés szerint. tooadd hello `on-error` tooa házirend szakaszt, keresse meg a szükséges toohello házirendek a Helyicsoportházirend-szerkesztő hello, és vegye fel azt. Házirendek konfigurálásával kapcsolatos további információkért lásd: [házirendek az API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Ha nincs `on-error` szakaszban hívóknak kap, 400 vagy 500 HTTP-válasz üzenetek hiba akkor fordul elő.  
  
### <a name="policies-allowed-in-on-error"></a>Hiba az engedélyezett házirendek  
 hello alábbi házirendek használható hello `on-error` házirend szakaszban.  
  
-   [Válassza a](api-management-advanced-policies.md#choose)  
  
-   [Set-változó](api-management-advanced-policies.md#set-variable)  
  
-   [keresése és cseréje](api-management-transformation-policies.md#Findandreplacestringinbody)  
  
-   [visszatérési-válasz](api-management-advanced-policies.md#ReturnResponse)  
  
-   [set-fejléc](api-management-transformation-policies.md#SetHTTPheader)  
  
-   [set-módszer](api-management-advanced-policies.md#SetRequestMethod)  
  
-   [állapot beállítása](api-management-advanced-policies.md#SetStatus)  
  
-   [küldési-kérelmek](api-management-advanced-policies.md#SendRequest)  
  
-   [küldési-egy-módon-kérelmek](api-management-advanced-policies.md#SendOneWayRequest)  
  
-   [napló-eventhub](api-management-advanced-policies.md#log-to-eventhub)  
  
-   [JSON-xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
  
-   [XML-json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>Hiba  
 Ha hiba lép fel, és a vezérlő egyszer toohello `on-error` hello hiba házirend szakaszban van tárolva [a környezetben. Hiba](api-management-policy-expressions.md#ContextVariables) tulajdonság, amely hozzáfér a hello házirendek `on-error` szakaszt, és rendelkezik a következő tulajdonságai hello.  
  
|Név|Típus|Leírás|Szükséges|  
|----------|----------|-----------------|--------------|  
|Forrás|Karakterlánc|Nevek hello elem ahol hello hiba történt. Lehet, vagy a házirend, vagy a beépített folyamat nevét.|Igen|  
|Ok|Karakterlánc|Gépet-barát hibakód, amely hibakezelés lesznek használva.|Nem|  
|Üzenet|Karakterlánc|Emberek számára olvasható hiba leírása.|Igen|  
|Hatókör|Karakterlánc|Ha hello hiba történt, ezért lehet egy "globális", "termék", "api" vagy "művelet" hello hatókör neve|Nem|  
|A szakasz|Karakterlánc|A szakasz nevét, ahol hiba történt, és sikerült valamelyik "bejövő", "Háttér", "kimenő" vagy "error".|Nem|  
|Elérési út|Karakterlánc|Meghatározza a beágyazott házirend, pl. "[3] válasszon / amikor [2]".|Nem|  
|PolicyId|Karakterlánc|Hello értékének `id` attribútumot, ha meg van adva hello ügyfél, ha hiba történt a hello házirend|Nem|  
  
> [!NOTE]
>  Összes házirend rendelkezik egy nem kötelező `id` attribútum toohello gyökérelemének hello házirend adható hozzá. Ha ez az attribútum szerepel egy házirend hibaállapotot esetén, hello attribútum értékének hello lekérhető hello segítségével `context.LastError.PolicyId` tulajdonság.  
  
## <a name="predefined-errors-for-built-in-steps"></a>A beépített lépéseket előre definiált hibák  
 hello következő hibák az előre beépített feldolgozási lépéseket hello értékelése során fellépő hibaállapotokat.  
  
|Forrás|Az állapot|Ok|Üzenet|  
|------------|---------------|------------|-------------|  
|konfiguráció|URI tooany Api vagy a művelet nem egyezik.|OperationNotFound|Nem lehet toomatch bejövő kérelem tooan műveletet.|  
|Engedélyezési|Nincs megadva előfizetés-kulcs|SubscriptionKeyNotFound|A hozzáférés toomissing előfizetés kulcs miatt megtagadva. Győződjön meg arról, hogy tooinclude előfizetés kulcs kérelmek toothis API meghozásakor.|  
|Engedélyezési|Előfizetés kulcs értéke érvénytelen.|SubscriptionKeyInvalid|A hozzáférés tooinvalid előfizetés kulcs miatt megtagadva. Győződjön meg arról, hogy tooprovide az aktív előfizetéssel az érvényes kulcsot.|  
  
## <a name="predefined-errors-for-policies"></a>Előre definiált hibák házirendek  
 hello következő hibák az előre házirend kiértékelése közben fellépő hibaállapotokat.  
  
|Forrás|Az állapot|Ok|Üzenet|  
|------------|---------------|------------|-------------|  
|a Sebességhatár|Sávszélesség-korlátjának túllépve|RateLimitExceeded|Sávszélesség-korlátjának túllépésekor|  
|kvóta|Túllépte a kvótát.|QuotaExceeded|A csomagba foglalt lebeszélhető percek elfogytak. Kvóta lesz tehát fel kell tölteni a xx:xx:xx. - e - sávszélesség kvótát. Kvóta lesz tehát fel kell tölteni a xx:xx:xx.|  
|jsonp|A visszahívási paraméter értéke érvénytelen (helytelen karaktereket tartalmaz.)|CallbackParameterInvalid|{Visszahívás-paraméter-neve} visszahívási paraméter értéke nem egy érvényes JavaScript-azonosító.|  
|IP-szűrő|Nem sikerült tooparse hívó IP kérelem|FailedToParseCallerIP|Nem sikerült hello hívó tooestablish IP-címet. A hozzáférés megtagadva.|  
|IP-szűrő|Hívó az IP-cím nem az engedélyezettek listájához|CallerIpNotAllowed|Hívó IP-cím {ip-cím} nem engedélyezett. A hozzáférés megtagadva.|  
|IP-szűrő|Hívó tiltólistán szereplő IP-cím|CallerIpBlocked|Hívó IP-cím le van tiltva. A hozzáférés megtagadva.|  
|ellenőrzés-fejléc|Nem jelenik meg a szükséges fejléc vagy -érték hiányzik|HeaderNotFound|{Fejlécnév} fejléc hello kérelem nem található. A hozzáférés megtagadva.|  
|ellenőrzés-fejléc|Nem jelenik meg a szükséges fejléc vagy -érték hiányzik|HeaderValueNotAllowed|{Fejlécérték} {fejlécnév} fejléc értéke nem engedélyezett. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Jwt jogkivonat nem található a kérelemben|TokenNotFound|A JWT hello kérelem nem található. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Aláírás érvényesítése nem sikerült|TokenSignatureInvalid|< jwt-könyvtárból üzenet\>. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Érvénytelen a célközönség|TokenAudienceNotAllowed|< jwt-könyvtárból üzenet\>. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Érvénytelen kibocsátót|TokenIssuerNotAllowed|< jwt-könyvtárból üzenet\>. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Token lejárt.|TokenExpired|< jwt-könyvtárból üzenet\>. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Aláírási kulcs nem lett feloldva-azonosító szerint|TokenSignatureKeyNotFound|< jwt-könyvtárból üzenet\>. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Szükséges jogcímeket a jogkivonatból hiányoznak|TokenClaimNotFound|Hiányzik a JWT jogkivonat hello jogcímek a következő: < c1\>, < c2\>,... A hozzáférés megtagadva.|  
|jwt ellenőrzése|Jogcím értékek eltérés|TokenClaimValueNotAllowed|{Jogcímérték} {jogcím-neve} jogcím értéke nem engedélyezett. A hozzáférés megtagadva.|  
|jwt ellenőrzése|Más érvényesítési hibák|JwtInvalid|< üzenet jwt-könyvtárból\>|

## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  