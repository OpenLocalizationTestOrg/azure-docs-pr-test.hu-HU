---
title: "aaaAzure API Management tartományközi házirendjei |} Microsoft Docs"
description: "További információk a tartományközi házirendjei használható az Azure API Management hello."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: dd5ebfd65b92ebd0c1f589a2bac669a3928d40b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-cross-domain-policies"></a>Az API Management tartományközi házirendjei
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CrossDomainPolicies"></a>Kereszt-tartományi házirendek  
  
-   [Lehetővé teszi a tartományok közötti hívások](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.  
  
-   [A CORS](api-management-cross-domain-policies.md#CORS) -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.  
  
-   [JSONP](api-management-cross-domain-policies.md#JSONP) - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.  
  
##  <a name="AllowCrossDomainCalls"></a>Lehetővé teszi a tartományok közötti hívások  
 Használjon hello `cross-domain` házirend toomake hello API érhető el az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|tartományok közötti|A gyökérelem. Gyermekelemek meg kell felelnie a toohello [Adobe tartományok közötti házirend specifikációnak](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Igen|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** globális  
  
##  <a name="CORS"></a>A CORS  
 Hello `cors` házirendnek eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.  
  
 A CORS lehetővé teszi, hogy egy böngésző és egy kiszolgáló toointeract, és határozza meg, függetlenül attól, tooallow adott eltérő eredetű kérések (azaz XMLHttpRequests hívásoknak JavaScript egy weblap tooother tartományokban). Ez lehetővé teszi, hogy csak az azonos eredetű kérések engedélyezése-nál nagyobb rugalmasságot, de biztonságosabb, mint az összes eltérő eredetű kérések engedélyezése.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a>Példa  
 Ez a példa bemutatja, hogyan toosupport előtti repülési kér, például az egyéni fejlécek vagy nem GET vagy POST metódusok. toosupport egyéni fejlécek és további HTTP-műveletek használata hello `allowed-methods` és `allowed-headers` látható módon hello a következő példában szakaszok.  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|a cors|A gyökérelem.|Igen|N/A|  
|engedélyezett források|Tartalmaz `origin` engedélyezett eredetet a tartományok közötti kérelmek hello leíró elemek. `allowed-origins`vagy egyetlen tartalmazhat `origin` megadó elem `*` tooallow minden eredet, vagy egy vagy több `origin` egy URI-t tartalmazó elemek.|Igen|N/A|  
|Forrás|hello érték lehet `*` tooallow minden eredet vagy URI, amely meghatározza egy egyetlen forrás. hello URI tartalmaznia kell egy protokollt, a gazdagép és a port.|Igen|Ha hello port nincs megadva az URI, 80-as portot használja HTTP és a 443-as portot használja HTTPS.|  
|engedélyezett módszerek|Ez az elem szükség, ha a módszerek nem GET vagy POST engedélyezettek. Tartalmaz `method` elemei, amelyek megadják a hello támogatott HTTP-műveleteket.|Nem|Ha ez a szakasz nincs jelen, a GET és POST támogatott.|  
|Módszer|Adja meg a HTTP-műveletet.|Legalább egy `method` elemet kell megadni, ha hello `allowed-methods` szakaszt.|N/A|  
|engedélyezett fejlécek|Ez az elem tartalmaz `header` elemek hello fejlécek hello kérelemben szereplő nevének megadása.|Nem|N/A|  
|a fejlécek elérhetővé tétele|Ez az elem tartalmaz `header` elemek hello fejlécek hello ügyfél által hozzáférhető nevének megadása.|Nem|N/A|  
|header|Meghatározza a fejléc neve.|Legalább egy `header` elemnek kell szerepelnie a `allowed-headers` vagy `expose-headers` Ha hello szakaszban található.|N/A|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|hitelesítő adatok engedélyezése|Hello `Access-Control-Allow-Credentials` fejléc a következő hello elővizsgálati válaszok set toohello értéke ennek az attribútumnak és hello ügyfél képes toosubmit hitelesítő adatait a tartományok közötti kérelmek hatással.|Nem|hamis|  
|ellenőrzés-result-maximális-kor|Hello `Access-Control-Max-Age` fejléc a következő hello elővizsgálati válaszok set toohello értéke ennek az attribútumnak és hello felhasználói ügynök képes toocache előtti repülési válasz hatással.|Nem|0|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** API-művelet  
  
##  <a name="JSONP"></a>JSONP  
 Hello `jsonp` házirend JSON hozzáadja a kitöltési (JSONP) támogatási tooan művelet vagy egy API-tooallow tartományokon átívelő JavaScript-ügyfelekből böngészőalapú hívások. JSONP a JavaScript programok toorequest adatait egy másik tartományban lévő kiszolgálóhoz használt módszer. JSONP megkerüli hello korlátozás kikényszeríti a legtöbb webböngészővel, ahol tooweb lapok hello kell ugyanabban a tartományban.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 Hello visszahívási paraméter nélkül hello metódus hívásakor? cb = XXX egyszerű JSON (nélkül függvény hívása burkoló) ad vissza.  
  
 Ha hello visszahívási paraméter hozzáadása `?cb=XXX` JSONP eredményeként alkalmazásburkoló hello eredeti JSON eredmények körül hello visszahívási függvény például ad vissza`XYZ('<json result goes here>');`  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|jsonp|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|a visszahívási paraméternév|hello tartományokon átívelő JavaScript függvényhívást előtagú hello teljesen minősített tartománynevét hello függvény tároló.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** kimenő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  