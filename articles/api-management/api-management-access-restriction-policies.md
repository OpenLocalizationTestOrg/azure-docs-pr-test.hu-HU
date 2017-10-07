---
title: "aaaAzure API Management hozzáférés szoftverkorlátozó házirendek |} Microsoft Docs"
description: "További tudnivalók hello hozzáférés szoftverkorlátozó házirendek az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 0ef368c2781d9a5cf9eaaa41a47489c904ed3198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-access-restriction-policies"></a>Az API Management hozzáférés szoftverkorlátozó házirendek
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a>Hozzáférés a szoftverkorlátozó házirendek  
  
-   [Ellenőrizze a HTTP-fejléc](api-management-access-restriction-policies.md#CheckHTTPHeader) -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.  
  
-   [Előfizetési határértéket hívás arányt a](api-management-access-restriction-policies.md#LimitCallRate) -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.  
  
-   [Korlát hívás arányt a kulcs](#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.  
  
-   [A hívó IP-címek korlátozása](api-management-access-restriction-policies.md#RestrictCallerIPs) -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.  
  
-   [Set memóriahasználati kvóta előfizetéssel](api-management-access-restriction-policies.md#SetUsageQuota) -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.  
  
-   [Set memóriahasználati kvóta kulcs által](#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.  
  
-   [Ellenőrizze a JWT](api-management-access-restriction-policies.md#ValidateJWT) -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.  
  
##  <a name="CheckHTTPHeader"></a>Ellenőrizze a HTTP-fejléc  
 Használjon hello `check-header` házirend tooenforce, hogy egy kérelem rendelkezik-e a megadott HTTP-fejléc. Toosee opcionálisan ellenőrizheti, ha hello fejléce rendelkezik egy adott érték vagy az engedélyezett értéktartományon tartomány ellenőrzése. Ha a hello az ellenőrzés sikertelen, a hello HTTP kód és a hiba állapotüzenetet hello házirend által megadott értéket, és hello házirend kérelem feldolgozása leáll.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|ellenőrzés-fejléc|A gyökérelem.|Igen|  
|érték|Engedélyezett HTTP-fejléc értéke. Ha több érték elem meg van adva, a hello ellenőrizze akkor tekinthető sikeres, ha bármely hello értékek egyik egyezés.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|nem sikerült – jelölőnégyzet-hibaüzenetek|Hiba üzenet tooreturn hello HTTP válasz törzsében Ha hello fejléc nem létezik vagy érvénytelen értékkel rendelkezik. Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.|Igen|N/A|  
|nem sikerült – jelölőnégyzet-HTTP-kód|HTTP-állapot kódját tooreturn, ha hello fejléc nem létezik vagy érvénytelen értékkel rendelkezik.|Igen|N/A|  
|fejléc-neve|HTTP-fejléc toocheck hello hello neve.|Igen|N/A|  
|esetben figyelmen kívül hagyása|Állíthat be tooTrue vagy HAMIS eredményt ad. Ha a set tooTrue esetben a rendszer figyelmen kívül hagyja, amikor hello elfogadható értékhalmazt összehasonlítja hello fejléc értékét.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="LimitCallRate"></a>Előfizetés hívás arányt a korlát  
 Hello `rate-limit` házirend megakadályozza, hogy az API-használati igényeiben jelentkező hello korlátozásával / előfizetés alapon hívja arány tooa megadott számú egy megadott időszak. Ez a házirend kiváltásakor hello hívó kap egy `429 Too Many Requests` válasz állapotkódja.  
  
> [!IMPORTANT]
>  Ezzel a házirend-házirend dokumentumonként csak egyszer használható.  
>   
>  [Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|korlát beállítása|A gyökérelem.|Igen|  
|api-t|Adjon hozzá egy vagy több ezen elemek tooimpose hívás sávszélesség-korlátjának az API-k hello terméken belül. Termék és API hívása sebesség egymástól függetlenül korlátokat alkalmazza.|Nem|  
|művelet|Adjon hozzá egy vagy több ezen elemek tooimpose hívás sávszélesség-korlátjának műveleten belül az API-k. Termék API és művelet hívása sebesség korlátok egymástól függetlenül alkalmazza.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|név|hello neve hello API mely tooapply hello sávszélesség-korlátjának.|Igen|N/A|  
|hívások|hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.|Igen|N/A|  
|megújítási időszak|hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** termék  
  
##  <a name="LimitCallRateByKey"></a>Korlát hívás arányt a kulcs  
 Hello `rate-limit-by-key` házirend megakadályozza, hogy az API-használati igényeiben jelentkező hello korlátozásával / kulcs alapon hívja arány tooa megadott számú egy megadott időszak. hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával. Választható növekmény feltételt kell számolni, amelyekhez a hello határérték felé számolnak toospecify adhatók hozzá. Ez a házirend kiváltásakor hello hívó kap egy `429 Too Many Requests` válasz állapotkódja.  
  
 További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Ezzel a házirend-házirend dokumentumonként csak egyszer használható.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Példa  
 A következő példa hello sávszélesség-korlátjának hello hello hívó IP-cím szerinti kulccsal definiált.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|korlát beállítása|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|hívások|hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.|Igen|N/A|  
|másik kulcs|hello kulcs toouse hello arány korlát házirend.|Igen|N/A|  
|növekvő-feltétel|Adja meg, ha hello kérelem kell számolni, felé hello kvóta hello logikai kifejezés (`true`).|Nem|N/A|  
|megújítási időszak|hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="RestrictCallerIPs"></a>A hívó IP-címek korlátozása  
 Hello `ip-filter` házirend szűrők (engedélyezi vagy megtagadja) hívásait bizonyos IP-címeket és/vagy címtartományokat.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|IP-szűrő|A gyökérelem.|Igen|  
|Cím|Mely toofilter egy egyetlen IP-cím megadása|Legalább egy `address` vagy `address-range` elemet kell megadni.|  
|címtartományt, az "address" = "address" =|Mely toofilter egy adott IP-cím megadása|Legalább egy `address` vagy `address-range` elemet kell megadni.|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|címtartományt, az "address" = "address" =|Egy adott IP-címek tooallow vagy megtagadja a hozzáférést.|Kötelező, ha hello `address-range` elem szolgál.|N/A|  
|IP-szűrési művelet = "engedélyezése &#124; megtiltják"|Megadja, hogy hívások engedélyezni kell-e, vagy nem hello a megadott IP-címek és tartományok.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="SetUsageQuota"></a>Set memóriahasználati kvóta-előfizetéssel  
 Hello `quota` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy előfizetés alapon.  
  
> [!IMPORTANT]
>  Ezzel a házirend-házirend dokumentumonként csak egyszer használható.  
>   
>  [Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|kvóta|A gyökérelem.|Igen|  
|api-t|Adjon hozzá egy vagy több ezen elemek tooimpose a kvóta az API-k hello terméken belül. A termék és API-kvóták egymástól függetlenül érvényesek.|Nem|  
|művelet|Adjon hozzá egy vagy több ezen elemek tooimpose kvóta műveleten belül az API-k. Termék API és művelet kvóták egymástól függetlenül érvényesek.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|név|hello neve hello API-t vagy a művelet, mely hello kvóta vonatkozik.|Igen|N/A|  
|Sávszélesség|hello kilobájt hello hello megadott időtartam során engedélyezett maximális száma `renewal-period`.|Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.|N/A|  
|hívások|hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.|Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.|N/A|  
|megújítási időszak|hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** termék  
  
##  <a name="SetUsageQuotaByKey"></a>Set memóriahasználati kvóta gombot  
 Hello `quota-by-key` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy kulcs alapon. hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával. Nem kötelező biztonsági feltétel kérések kell számolni, felé hello kvóta toospecify adhatók hozzá.  
  
 További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Ezzel a házirend-házirend dokumentumonként csak egyszer használható.  
>   
>  [Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Példa  
 A következő példa hello hello kvóta hello hívó IP-cím szerinti kulccsal definiált.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|kvóta|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|Sávszélesség|hello kilobájt hello hello megadott időtartam során engedélyezett maximális száma `renewal-period`.|Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.|N/A|  
|hívások|hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.|Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.|N/A|  
|másik kulcs|hello kulcs toouse hello kvóta házirend.|Igen|N/A|  
|növekvő-feltétel|Adja meg, ha hello kérelem kell számolni, felé hello kvóta hello logikai kifejezés (`true`)|Nem|N/A|  
|megújítási időszak|hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="ValidateJWT"></a>JWT ellenőrzése  
 Hello `validate-jwt` házirend érvénybe lépteti a meglétét, és a jwt-t érvényességét kinyert vagy egy meghatározott HTTP-fejléc vagy a megadott lekérdezési paraméter.  
  
> [!IMPORTANT]
>  Hello `validate-jwt` házirendje megköveteli, hogy hello `exp` regisztrált nincs inlcuded hello JWT jogkivonat, kivéve, ha `require-expiration-time` attribútum be van megadva, túl`false`.  
> Hello `validate-jwt` házirend HS256 és RS256 aláírási algoritmust támogat. HS256 hello kulcsot meg kell adni beágyazott hello házirend hello base64-kódolású formában. RS256 hello kulcsot adjon meg azonosító nyissa meg a konfigurációs végpont keresztül toobe rendelkezik.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<validate-jwt   
    header-name="name of http header containing hello token (use query-parameter-name attribute if hello token is passed in hello URL)"   
    failed-validation-httpcode="http status code tooreturn on failure"   
    failed-validation-error-message="error message tooreturn on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of hello claim as it appears in hello token" match="all|any">  
      <value>claim value as it is expected tooappear in hello token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of hello configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="azure-mobile-services-token-validation"></a>Az Azure Mobile Services jogkivonatok érvényesség-ellenőrzése  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Az Azure Active Directory jogkivonatok érvényesség-ellenőrzése  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Az Azure Active Directory B2C jogkivonatok érvényesség-ellenőrzése  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a>A jogcímek jogkivonat-alapú hozzáférés toooperations engedélyezése  
 A példa bemutatja, hogyan toouse hello [érvényesítése JWT](api-management-access-restriction-policies.md#ValidateJWT) házirend toopre-hozzáférés toooperations a jogcímek jogkivonat-alapú hitelesítéséhez. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too13:50 előre. Gyors toosee hello házirendeket a Helyicsoportházirend-szerkesztő hello az too15:00 továbbítja, és majd a művelet hívásának hello developer portálról vagy anélkül hello bemutatója too18:50 szükséges engedélyezési jogkivonat.  
  
```xml  
<!-- Copy hello following snippet into hello inbound section at hello api (or higher) level toopre-authorize access toooperations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|jwt ellenőrzése|A gyökérelem.|Igen|  
|célcsoportok|Elfogadható célközönség jogcímek használható hello jogkivonat listáját tartalmazza. Ha több célközönség értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel. Legalább egy célközönség meg kell adni.|Nem|  
|Kiállítói aláírási kulcsok|A Base64-kódolású biztonsági használt kulcsok toovalidate listáját aláírt jogkivonatokat. Ha több biztonsági kulcsok szerepelnek, akkor minden kulcs próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel (hasznos token kulcsváltás). Fő elemekből kell egy nem kötelező `id` használt attribútum toomatch elleni `kid` jogcímek.|Nem|  
|kibocsátók|Hello jogkivonatot kibocsátó elfogadható résztvevők listájának. Ha több kibocsátó értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel.|Nem|  
|openid-konfiguráció|hello elem segítségével megadhatja a megfelelő megnyitott azonosító konfigurációs végpont, amelyből aláíró kulcsok és a kiállító érhető el.|Nem|  
|szükséges jogcímeket|Érvényes toobe várt jogcímek toobe az hello token megtalálható listáját tartalmazza. Ha hello `match` attribútum értéke túl`all` minden jogcím értékét hello házirend érvényesítési toosucceed hello jogkivonat jelen kell lennie. Ha hello `match` attribútum értéke túl`any` legalább egy jogcím érvényesítési toosucceed hello jogkivonat jelen kell lennie.|Nem|  
|zumo főkulcs|Az Azure Mobile Services által kiállított jogkivonatokat főkulcs|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|óraeltérés|TimeSpan érték. Néhány kis eltérést biztosít, abban az esetben hello jogkivonat lejárati jogcím hello lexikális elem szerepel, és már elmúlt hello jelenlegi dátum és idő.|Nem|0 másodperc|  
|nem sikerült – érvényesítési-hibaüzenetek|Hiba üzenet tooreturn hello HTTP-válasz törzsében Ha hello JWT nem felel meg az érvényesítési. Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.|Nem|Alapértelmezett hibaüzenetet függ az érvényesítési hibát, például "JWT nem található."|  
|nem sikerült – érvényesítési-HTTP-kód|HTTP-állapot kódját tooreturn, ha hello JWT nem teljesíti az ellenőrző.|Nem|401|  
|fejléc-neve|hello hello token okozó hello HTTP-fejléc nevét.|Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.|N/A|  
|id|Hello `id` hello attribútum `key` elem lehetővé teszi a toospecify hello karakterlánc, amely elleni megfeleltetésének `kid` hello token (ha van ilyen) toofind kimenő hello megfelelő kulcs toouse az aláírás-ellenőrzés a jogcímek.|Nem|N/A|  
|Egyezés|Hello `match` hello attribútum `claim` elem meghatározza, hogy minden hello házirend jogcím értékét kell a érvényesítési toosucceed hello lexikális elem szerepel. Lehetséges értékek:<br /><br /> -                          `all`-hello házirend minden jogcím értékét érvényesítési toosucceed hello jogkivonat jelen kell lennie.<br /><br /> -                          `any`-legalább egy jogcím értéke érvényesítési toosucceed hello jogkivonat jelen kell lennie.|Nem|Minden|  
|lekérdezés-paremeter-neve|hello hello lekérdezési paraméter hello token okozó hello neve.|Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.|N/A|  
|igényelnek-lejárati-idő|Logikai érték. Meghatározza, hogy szükséges-e egy lejárati jogcím hello jogkivonatban.|Nem|Igaz|
|szükséges rendszer|pl. hello hello token rendszer neve "Tulajdonos". Az attribútum van beállítva, amikor hello házirend biztosítja, hogy a megadott séma megtalálható-e hello engedélyezési fejléc értéke.|Nem|N/A|
|igényelnek-aláírt-tokenek|Logikai érték. Megadja, hogy a jogkivonat aláírt szükséges toobe.|Nem|Igaz|  
|URL-címe|Azonosító konfigurációs végponti URL-cím megnyitása ahol nyitott azonosító konfigurációs metaadatok érhető el. Az Azure Active Directory használata a következő URL-cím hello: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` a directory-bérlő neve, pl. és `contoso.onmicrosoft.com`.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  
