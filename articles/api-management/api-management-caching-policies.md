---
title: "az API Management gyorsítótárazási házirendek aaaAzure |} Microsoft Docs"
description: "További információk a házirendek az Azure API Management használható gyorsítótárazás hello."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: bd6b0721945609b28dbf6e7ef0631979c08c8c65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-caching-policies"></a>Az API Management gyorsítótárazási házirendek
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a>Házirendek gyorsítótárazása  
  
-   A válasz gyorsítótár házirendek  
  
    -   [Gyorsítótár beszerezni](api-management-caching-policies.md#GetFromCache) -hajtsa végre a gyorsítótár kereshet, és ha elérhető egy érvényes gyorsítótárazott választ adjon vissza.  
  
    -   [Tárolja a toocache](api-management-caching-policies.md#StoreToCache) - gyorsítótárak válaszok szerint toohello megadott gyorsítótár-vezérlő konfigurációját.  
  
-   Házirendek gyorsítótárazás érték  
  
    -   [Lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.  
  
    -   [A gyorsítótárban tárolja a érték](#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.  
  
    -   [Távolítsa el az értéket a gyorsítótárból](#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.  
  
##  <a name="GetFromCache"></a>Gyorsítótár beszerzése  
 Használjon hello `cache-lookup` házirend tooperform gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető. Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni. Válasz gyorsítótárazás csökkenti a sávszélesség, és szemben támasztott feldolgozási igényeket kivetett hello háttér web server, és csökkenti a késés API fogyasztó érzékelt.  
  
> [!NOTE]
>  Ez a házirend rendelkeznie kell egy megfelelő [tároló toocache](api-management-caching-policies.md#StoreToCache) házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression tooevaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-authorized-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Példa házirend-kifejezések használatával  
 Ez a példa bemutatja, hogyan tootooconfigure API Management válasz gyorsítótár időtartama, hogy megfelel hello válasz gyorsítótárazását hello háttérszolgáltatást hello által meghatározott biztonsági szolgáltatás `Cache-Control` direktívát. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too25:25 előre.  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|gyorsítótár-keresés|A gyökérelem.|Igen|  
|eltérő-által-fejléc|Kezdjék el gyorsítótárazni a válaszok száma érték a megadott fejléc, például elfogadás, Accept-Charset, elfogadás-kódolás, elfogadás-nyelv, hitelesítés, a várt, a gazdagép, If-Match.|Nem|  
|eltérő-által-lekérdezési-paraméter|Kezdjék el gyorsítótárazni a válaszok száma a megadott lekérdezési paraméterek értékének. Adjon meg egy vagy több paramétert. Használjon pontosvessző. Ha nincs megadva, az összes lekérdezési paraméterek használhatók.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|lehetővé teszik privát-válasz-gyorsítótárazás|Ha értéke túl`true`, lehetővé teszi, hogy egy Authorization fejlécet tartalmazó kérelmek gyorsítótárazását.|Nem|hamis|  
|alsóbb rétegbeli gyorsítótárazás típusa|Ez az attribútum a következő értékek hello tooone kell beállítani.<br /><br /> -none - alárendelt gyorsítótárazás nem engedélyezett.<br />-titkos - alsóbb rétegbeli személyes engedélyezve van a gyorsítótárazás.<br />-nyilvános - titkos és megosztott alárendelt engedélyezve van a gyorsítótárazás.|Nem|Egyik sem|  
|must-revalidate érték|Ha az alárendelt gyorsítótár engedélyezve van ez az attribútum a- vagy kikapcsolja a hello `must-revalidate` cache-control direktíva átjáró válaszokban.|Nem|Igaz|  
|eltérő-által-fejlesztőknek|Állítsa be a túl`true` toocache válaszok fejlesztői kulcs száma.|Nem|hamis|  
|eltérő-által-developer-csoportok|Állítsa be a túl`true` toocache válaszok száma felhasználói szerepkör.|Nem|hamis|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** API, a művelet, a termék  
  
##  <a name="StoreToCache"></a>Tároló toocache  
 Hello `cache-store` házirend gyorsítótárak válaszok toohello szerint megadott gyorsítótár-beállítások. Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni. Válasz gyorsítótárazás csökkenti a sávszélesség, és szemben támasztott feldolgozási igényeket kivetett hello háttér web server, és csökkenti a késés API fogyasztó érzékelt.  
  
> [!NOTE]
>  Ez a házirend rendelkeznie kell egy megfelelő [lekérni a gyorsítótár](api-management-caching-policies.md#GetFromCache) házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Példa házirend-kifejezések használatával  
 Ez a példa bemutatja, hogyan tootooconfigure API Management válasz gyorsítótár időtartama, hogy megfelel hello válasz gyorsítótárazását hello háttérszolgáltatást hello által meghatározott biztonsági szolgáltatás `Cache-Control` direktívát. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too25:25 előre.  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|gyorsítótár|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|Időtartam|Bejegyzések, a másodpercben megadott idő élettartamát a hello gyorsítótárazza.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** kimenő  
  
-   **Házirend hatókörök:** API, a művelet, a termék  
  
##  <a name="GetFromCacheByKey"></a>Lehet értéket kiolvasni a gyorsítótár  
 Használjon hello `cache-lookup-value` házirend tooperform keresési gyorsítótár gombot, és a gyorsítótárazott érték visszaadása. hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.  
  
> [!NOTE]
>  Ez a házirend rendelkeznie kell egy megfelelő [érték tárolása gyorsítótár](#StoreToCacheByKey) házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Példa  
 További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|gyorsítótár-keresési-értéke|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|alapértelmezett értékű|Egy érték, amely rendeli toohello változó Ha hello gyorsítótár kulcskeresési tévesztés eredményezett. Ha ez az attribútum nincs megadva, `null` hozzá van rendelve.|Nem|`null`|  
|kulcs|Kulcs értéke toouse hello keresési a gyorsítótárba.|Igen|N/A|  
|változó-neve|Hello neve [környezeti változó](api-management-policy-expressions.md#ContextVariables) keresett érték hello rendeli hozzá, ha a keresés sikeres. Ha keresési tévesztés eredményez, hello változó kap-e hello hello értékének `default-value` attribútum vagy `null`, ha hello `default-value` attribútum hiányzik.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** globális, API-t, a művelet, a termék  
  
##  <a name="StoreToCacheByKey"></a>A gyorsítótárban tárolja a érték  
 Hello `cache-store-value` hajtja végre a gyorsítótár tárolási gombot. hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.  
  
> [!NOTE]
>  Ez a házirend rendelkeznie kell egy megfelelő [lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a>Példa  
 További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|gyorsítótár-tároló-értéke|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|Időtartam|Érték a gyorsítótárba fognak kerülni a megadott típusú érték, másodpercben megadva hello.|Igen|N/A|  
|kulcs|A gyorsítótár kulcs hello értékét fogja tárolni.|Igen|N/A|  
|érték|hello érték toobe gyorsítótárazva.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** globális, API-t, a művelet, a termék  
  
###  <a name="RemoveCacheByKey"></a>A gyorsítótárból értékének eltávolítása  
 Hello `cache-remove-value` egy gyorsítótárazott elem azonosítja a kulcs törlése. hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.  
  
#### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Példa  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|gyorsítótár-remove-értéke|A gyökérelem.|Igen|  
  
#### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|kulcs|hello hello kulcsa előzőleg gyorsítótárazott érték toobe hello gyorsítótárból eltávolítva.|Igen|N/A|  
  
#### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** globális, API-t, a művelet, a termék  
  

## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  