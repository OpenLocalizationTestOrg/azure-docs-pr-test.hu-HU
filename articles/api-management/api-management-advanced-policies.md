---
title: "API-felügyeleti házirendek speciális aaaAzure |} Microsoft Docs"
description: "További tudnivalók az Azure API Management használható házirendek speciális hello."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8a13348b-7856-428f-8e35-9e4273d94323
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 8245e7a4c9d432b7b4d362192e357829fcabad55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-advanced-policies"></a>Házirendek speciális API Management
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AdvancedPolicies"></a>Speciális házirendek  
  
-   [Folyamat szabályozása](api-management-advanced-policies.md#choose) - feltételesen alkalmazza a házirend-utasításoknál logikai hello értékelése hello eredményei alapján [kifejezések](api-management-policy-expressions.md).  
  
-   [Kérés továbbítása a](#ForwardRequest) -hello kérelem toohello háttérszolgáltatást továbbítja.

-   [Korlátozza a feldolgozási](#LimitConcurrency) -megakadályozza, hogy a házirendek által megadott számú kérelem hello egyszerre egynél több végrehajtása közé.
  
-   [TooEvent központ naplófájl](#log-to-eventhub) -hello üzeneteket küld a megadott formátumban tooan Eseményközpont határozzák meg a tranzakciónaplókat tartalmazó entitás. 

-   [Válasz mock](#mock-response) -megszakításainak csővezeték-végrehajtási mocked választ ad vissza, és közvetlenül toohello hívó.
  
-   [Próbálja meg újra](#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig. Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.  
  
-   [Térjen vissza a válasz](#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó. 
  
-   [Egyirányú kérés küldése](#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.  
  
-   [Kérés küldése](#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.  

-   [Állítsa be a HTTP-proxy](#SetHttpProxy) -lehetővé teszi egy HTTP-proxyn keresztül továbbított tooroute kérelmeket.  

-   [Kérelem módszert állítja be](#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.  
  
-   [Állítsa be. állapotkód:](#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.  
  
-   [Új érték](api-management-advanced-policies.md#set-variable) -továbbra is fennáll az elnevezett értéket [környezetben](api-management-policy-expressions.md#ContextVariables) változó későbbi eléréshez.  

-   [Nyomkövetési](#Trace) -karakterlánc hozzáadja hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.  
  
-   [Várjon, amíg](#Wait) -megvárja-e a zárójelek között [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), vagy [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek toocomplete a folytatás előtt.  
  
##  <a name="choose"></a>Folyamata  
 Hello `choose` házirend utasítások logikai kifejezésen, ha-akkor más hasonló tooan vagy kapcsoló értékelése hello eredménye alapján a programozási nyelven összeállításához zárt házirend vonatkozik.  
  
###  <a name="ChoosePolicyStatement"></a>Házirendutasítás  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements toobe applied if none of hello above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 hello vezérlő adatfolyam házirend tartalmaznia kell legalább egy `<when/>` elemet. Hello `<otherwise/>` elem nem kötelező megadni. A feltételek `<when/>` elemek kiértékelése sorrendben történik, illetve megjelenésük hello házirend. Először határolt hello házirend nyilatkozatuk `<when/>` feltétel attribútum egyenlő elem `true` lépnek érvénybe. Hello határolt házirendek `<otherwise/>` elem, ha van ilyen, alkalmazza a rendszer, ha az összes hello a `<when/>` elem feltétel attribútumok `false`.  
  
### <a name="examples"></a>Példák  
  
####  <a name="ChooseExample"></a>Példa  
 hello következő példa bemutatja egy [set-változó](api-management-advanced-policies.md#set-variable) házirend és a két vezérlése áramlási szabályzatokkal.  
  
 változó szabály hello beállítása megtalálható hello bejövő szakaszt, és létrehoz egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) változó, amely tootrue van beállítva, ha hello `User-Agent` kérelem fejléc hello szöveget tartalmaz `iPad` vagy `iPhone`.  
  
 hello első vezérlő adatfolyam házirend egyben a bejövő szakasz hello és feltételesen alkalmaz egy két [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) attól hello hello értékének `isMobile` környezeti változó.  
  
 hello második ellenőrzési folyamata házirend hello kimenő szakaszban, és feltételesen vonatkozik hello [XML konvertálása tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) házirend amikor `isMobile` értéke túl`true`.  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a>Példa  
 Ez a példa bemutatja, hogyan tooperform tartalom alapján történő szűrés adatelemek eltávolítása hello választ kapott hello háttérszolgáltatást hello használatakor `Starter` termék. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too34:30 előre. Áttekintést 31:50 toosee kezdjék [sötét égbolt előrejelzési API hello](https://developer.forecast.io/) ebben a bemutatóban használt.  
  
```xml  
<!-- Copy this snippet into hello outbound section tooremove a number of data elements from hello response received from hello backend service based on hello name of hello api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|Válassza a|A gyökérelem.|Igen|  
|mikor|a hello feltétel toouse hello `if` vagy `ifelse` hello részei `choose` házirend. Ha hello `choose` -házirendben engedélyezve van több `when` szakaszok, azok egymás után értékeli ki. Egyszer hello `condition` , hogy amikor egy elem értékeli ki túl`true`, további `when` feltételek értékelésének.|Igen|  
|Ellenkező esetben|Hello házirend részlet toobe használható, ha nincs hello tartalmaz `when` feltételek kiértékelése túl`true`.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|  
|---------------|-----------------|--------------|  
|feltétel = "logikai kifejezés &#124; Logikai állandó"|hello logikai kifejezés, vagy ha hello tartalmazó állandó tooevaluated `when` házirend-utasítás kiértékelése történik.|Igen|  
  
###  <a name="ChooseUsage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="ForwardRequest"></a>Kérés továbbítása  
 Hello `forward-request` házirend továbbítja hello bejövő kérelem toohello háttérszolgáltatást hello kérelemben megadott [környezet](api-management-policy-expressions.md#ContextVariables). hello háttérkiszolgáló URL-címe szerepel hello API [beállítások](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) és hello segítségével módosítható [be háttérszolgáltatás](api-management-transformation-policies.md) házirend.  
  
> [!NOTE]
>  A csoportházirend-eredmények nem lesznek továbbítva toohello háttér szolgáltatás és hello hello kimenő szakaszban házirendek hello kérelem hello házirendek segítségével a hello hello sikeres befejezését követően azonnal kiértékelése eltávolítása bejövő szakasz.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="example"></a>Példa  
 hello következő API-szintű szabályzat továbbítja az összes kérelem toohello háttérszolgáltatást 60 másodperces időtúllépési időköz.  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Példa  
 Ez a művelet szintű szabályzat használ hello `base` elem tooinherit hello háttér házirend hello szülő API szintű hatókörből.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Példa  
 Ez a művelet szintű szabályzat explicit módon továbbítja az összes kérelem toohello háttérszolgáltatást egy időkorlát 120, és nem örököl a hello szülő API-szintű háttér házirend.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note hello absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Példa  
 Ez a művelet szintű szabályzat nem továbbítja a kérelmek toohello háttérszolgáltatáshoz.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding toobackend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|előre-kérés|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|Időtúllépés = "integer"|hello időkorlátja másodpercben előtt hello hívás toohello háttérszolgáltatást sikertelen lesz.|Nem|Nincs időtúllépés|  
|hajtsa végre-átirányítások = "true &#124; FALSE"|Megadja, hogy hello háttérszolgáltatást átirányítása hello átjáró követ vagy toohello hívó adott vissza.|Nem|hamis|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** háttér  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="LimitConcurrency"></a>A feldolgozási korlátot  
 Hello `limit-concurrency` házirend megakadályozza, hogy a zárt házirendek nagyobb, mint a kérések megadott számát egy adott időben hello végrehajtása. Alapján meghaladó hello küszöbértéket, új kérelmek kerülnek tooa várólista, amíg nem érhető el, hello várólista maximális hossza. Várólista Erőforrásfogyás esetén új kérelmek sikertelenek lesznek azonnal.
  
###  <a name="LimitConcurrencyStatement"></a>Házirendutasítás  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Példák  
  
####  <a name="ChooseExample"></a>Példa  
 hello következő példa bemutatja, hogyan továbbítsa az toolimit kérelmek száma a tooa háttér hello egy környezeti változó értéke alapján.
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3" timeout="60">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|    
|határ-feldolgozási|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|--------------|  
|kulcs|Egy karakterláncot. A kifejezés engedélyezett. Hello párhuzamossági hatókör határozza meg. Megoszthatók több házirendet.|Igen|N/A|  
|maximális száma|Egy egész számot. Itt adhatja meg, amelyek számára engedélyezett tooenter hello házirend kérelmek maximális számát.|Igen|N/A|  
|timeout|Egy egész számot. A kifejezés engedélyezett. Hello kérelmet megvárja-e tooenter egy hatókört, mielőtt hibát jelentene, a másodpercek számát adja meg "403-as túl sok kérelem"|Nem|Infinity|  
|maximális hosszúságú várólista|Egy egész számot. A kifejezés engedélyezett. Hello várólista maximális hosszát határozza meg. Bejövő kérelmek tooenter közben ezzel a házirend-befejeződik a "403-as túl sok kérelem" azonnal amikor hello várólista kimerült.|Nem|Infinity|  
  
###  <a name="ChooseUsage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  

##  <a name="log-to-eventhub"></a>Napló tooEvent Hub  
 Hello `log-to-eventhub` hello üzenetek megadott formátum tooan határozzák meg a tranzakciónaplókat tartalmazó entitás Eseményközpont házirend küld. Mivel a név azt jelenti, hello házirend kijelölt kérés vagy válasz környezeti adatok mentése online vagy kapcsolat nélküli elemzéshez használható.  
  
> [!NOTE]
>  Részletes útmutató, az event hubs és a naplózási események konfigurálása, lásd: [hogyan toolog API Management eseményeket az Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Példa  
 Bármilyen karakterlánc hello érték toobe jelentkezett be az Event Hubs használható. Ebben a példában hello dátum és idő, telepítési szolgáltatás neve, kérelemazonosító, IP-cím és az összes bejövő hívások műveletnév naplózó regisztrált hello naplózott toohello eseményközpont `contoso-logger` azonosítója.  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|napló-eventhub|A gyökérelem. hello Ez az elem értéke hello karakterlánc toolog tooyour eseményközpontot.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|  
|---------------|-----------------|--------------|  
|naplózó-azonosító|hello hello azonosítója naplózó az API Management szolgáltatásban regisztrált.|Igen|  
|partícióazonosító-:|Megadja a hello index hello partíció, ahol az üzenetek küldése történik.|Választható. Ez az attribútum nem használható, ha `partition-key` szolgál.|  
|a partíciókulcs|Adja meg a használt partíció-hozzárendelés elküldött üzenetek hello érték.|Választható. Ez az attribútum nem használható, ha `partition-id` szolgál.|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  

##  <a name="mock-response"></a>Utánzatait válasz  
Hello `mock-response`, hello neveként azt jelenti, toomock API-k és műveletek. Normál feldolgozási sor végrehajtása megszakítja, és egy mocked válasz toohello hívó adja vissza. hello házirend mindig próbálja meg a legmagasabb hűség tooreturn válaszokat. Válasz tartalom példák, amikor a rendelkezésre álló szívesebben. Minta válaszok létrehozott sémák, a sémák találhatók, és többek között nem. Ha a példák és sémák nem találhatók, nem tartalmú válaszok is megjelennek.
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Példák  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|mock-válasz|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|--------------|  
|állapotkód-:|Adja meg a válasz állapotkódja és használt tooselect megfelelő példa vagy séma.|Nem|200|  
|tartalomtípus|Itt adhatja meg `Content-Type` válasz állomásfejléc-érték és a használt tooselect megfelelő példa vagy séma van.|Nem|None|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő, hiba  
  
-   **Házirend hatókörök:** minden hatókör

##  <a name="Retry"></a>Próbálja meg újra  
 Hello `retry` házirend egyszer végrehajtja az alárendelt házirendeket, és majd újrapróbálkozik a végrehajtási, amíg hello újrapróbálkozási `condition` válik `false` , vagy próbálkozzon újra `count` kimerült.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a>Példa  
 A hello a következő példa kérelem forewarding a rendszer ismét megkísérli tooten időpontokban exponenciális újrapróbálkozási algoritmust használja fel. Mivel a `first-fast-retry` értéke toofalse, minden újrapróbálkozások tulajdonos toohello exponsntial újrapróbálkozási algoritmust.  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|retry|A gyökérelem. Előfordulhat, hogy bármely egyéb házirendek gyermekelemeinek tartalmazni.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|Az állapot|Logikai szövegkonstans vagy [kifejezés](api-management-policy-expressions.md) adja meg, ha az újrapróbálkozások le kell állítani (`false`) vagy továbbra is (`true`).|Igen|N/A|  
|Száma|Hello tooattempt az újrapróbálkozások maximális számát megadó pozitív szám.|Igen|N/A|  
|interval|Egy pozitív szám hello újrapróbálkozások között hello időköz megadása másodpercben.|Igen|N/A|  
|maximális-időköz|Egy pozitív szám hello maximális időtartamot hello újrapróbálkozások között megadása másodpercben. Használt tooimplement exponenciális újrapróbálkozási algoritmust is.|Nem|N/A|  
|különbözeti|Egy pozitív szám hello várakozási időközt növekmény megadása másodpercben. Használt tooimplement hello lineáris és exponenciális újrapróbálkozási algoritmust is.|Nem|N/A|  
|első fast-újrapróbálkozási|Ha túl beállítása `true` , hello első újrapróbálkozások azonnal megtörténik.|Nem|`false`|  
  
> [!NOTE]
>  Ha csak hello `interval` meg van adva, **rögzített** időköz újrapróbálkozások hajtja végre.  
>  Ha csak hello `interval` és `delta` vannak megadva, a **lineáris** időköz újrapróbálkozási algoritmust használnak, ahol várakozási idő között a számított függően hello a következő képlet - `interval + (count - 1)*delta`.  
>  Ha hello `interval`, `max-interval` és `delta` vannak megadva, **exponenciális** időköz újrapróbálkozási algoritmust alkalmazza a rendszer, ahol hello várakozási idő hello újrapróbálkozások között inkább exponenciálisan helloértékét`interval`toohello érték `max-interval` toohello következő megfelelően forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Vegye figyelembe, hogy ez a házirend öröklik a gyermek házirend használattal kapcsolatos korlátozásokat.  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="ReturnResponse"></a>Válasz visszaadása  
 Hello `return-response` házirend feldolgozási sor végrehajtása megszakítása és egy alapértelmezett vagy egyéni válasz toohello hívó adja vissza. Alapértelmezett válasz `200 OK` nem szervezethez. Egyéni válasz egy környezeti változó vagy házirend utasítások keresztül adható meg. Mindkét kapnak, amikor hello válasz hello környezeti változó belül található módosul a hello házirend-utasításoknál előtt toohello hívó ad vissza.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|visszatérési-válasz|A gyökérelem.|Igen|  
|set-fejléc|A [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend-utasítás.|Nem|  
|set-szervezet|A [set-törzsének](api-management-transformation-policies.md#SetBody) házirend-utasítás.|Nem|  
|állapot beállítása|A [állapotának beállítása](api-management-advanced-policies.md#SetStatus) házirend-utasítás.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|  
|---------------|-----------------|--------------|  
|válasz-változó-neve|hello hello környezeti változó neve hivatkozni, például felsőbb rétegbeli [küldési-kérelmek](api-management-advanced-policies.md#SendRequest) házirend, és úgy, hogy egy `Response` objektum|Választható.|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="SendOneWayRequest"></a>Egyirányú kérés küldése  
 Hello `send-one-way-request` házirend toohello megadott URL-címet a válaszra való várakozás nélkül megadott hello kérést küld.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Példa  
 Ez a minta-házirend használatával hello példáját mutatja be `send-one-way-request` házirend toosend egy üzenet tooa Slack Csevegés hely hello HTTP-válaszkód nagyobb vagy egyenlő too500 esetén. Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|küldési-egy-módon-kérelmek|A gyökérelem.|Igen|  
|URL-címe|hello hello kérelem URL-címe|Nincs if mód = másolási; Ellenkező esetben igen.|  
|Módszer|hello hello kérelem HTTP-metódus.|Nincs if mód = másolási; Ellenkező esetben igen.|  
|header|Kérelem fejléce. Használjon több több kérelem fejlécek.|Nem|  
|Törzs|hello kérés törzsében.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|mód = "karakterlánc"|Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem hello. Kimenő módban mód = másolása nem sikerült hello kérés törzsében.|Nem|új|  
|név|Hello fejléc toobe set hello nevét adja meg.|Igen|N/A|  
|létezik-e művelet|Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva. Ez az attribútum hello a következő értékek egyikének kell lennie.<br /><br /> -felülbírálás - cserél hello értéke hello meglévő fejléc.<br />-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.<br />-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.<br />-törlés – hello kérelemből eltávolítja hello fejlécet.<br /><br /> Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.|Nem|felülbírálás|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="SendRequest"></a>Kérés küldése  
 Hello `send-request` házirend toohello megadott URL-címe, mint hello beállítása időtúllépés értéke már nem vár a megadott hello kérést küld.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Példa  
 Ez a példa bemutatja egyirányú tooverify hivatkozás-jogkivonat az engedélyezési-kiszolgálóval. Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request tooToken Server toovalidate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|küldési-kérelmek|A gyökérelem.|Igen|  
|URL-címe|hello hello kérelem URL-címe|Nincs if mód = másolási; Ellenkező esetben igen.|  
|Módszer|hello hello kérelem HTTP-metódus.|Nincs if mód = másolási; Ellenkező esetben igen.|  
|header|Kérelem fejléce. Használjon több több kérelem fejlécek.|Nem|  
|Törzs|hello kérés törzsében.|Nem|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|mód = "karakterlánc"|Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem hello. Kimenő módban mód = másolása nem sikerült hello kérés törzsében.|Nem|új|  
|válasz-változó-name = "karakterlánc"|Ha nincs jelen, `context.Response` szolgál.|Nem|N/A|  
|Időtúllépés = "integer"|hello időkorlátja másodpercben hello hívása előtt toohello URL-cím nem sikerül.|Nem|60|  
|Hiba mellőzése|Ha igaz, és hello kérik hibát eredményez:<br /><br /> -Ha a válasz-változó-név lett megadva, null értéket tartalmaz.<br />-Ha a válasz-változó-neve nincs megadva, a környezetben. Kérés nem fog frissülni.|Nem|hamis|  
|név|Hello fejléc toobe set hello nevét adja meg.|Igen|N/A|  
|létezik-e művelet|Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva. Ez az attribútum hello a következő értékek egyikének kell lennie.<br /><br /> -felülbírálás - cserél hello értéke hello meglévő fejléc.<br />-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.<br />-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.<br />-törlés – hello kérelemből eltávolítja hello fejlécet.<br /><br /> Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.|Nem|felülbírálás|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="SetHttpProxy"></a>HTTP-proxy beállítása  
 Hello `proxy` házirendje lehetővé teszi a tooroute továbbított kérelmek toobackends egy HTTP-proxyn keresztül. Csak a HTTP (nem a HTTPS-) átjáró hello és hello proxy között támogatott. Alapszintű, és csak az NTLM-hitelesítés.
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Példa  
Vegye figyelembe a hello használata [tulajdonságok](api-management-howto-properties.md) a bizalmas adatok tárolása hello házirend-dokumentum hello felhasználónév és jelszó tooavoid értékként.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|Proxy|Legfelső szintű elem|Igen|  

### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|URL-cím = "karakterlánc"|Proxykiszolgáló URL-címet http://host:port hello formája.|Igen|N/A|  
|felhasználónév = "karakterlánc"|A hello proxy-hitelesítéshez használt felhasználónevet toobe.|Nem|N/A|  
|jelszó = "karakterlánc"|A hello proxy-hitelesítéshez használt jelszó toobe.|Nem|N/A|  

### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** minden hatókör  

##  <a name="SetRequestMethod"></a>Set kérés metódus  
 Hello `set-method` házirendje lehetővé teszi a toochange hello HTTP-kérési metódust egy kérelem.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Példa  
 Ez a minta házirend hello használó `set-method` házirend egy üzenet tooa Slack Csevegés hely küldése, ha HTTP-válaszkód hello érték nagyobb, mint vagy egyenlő too500 példáját mutatja be. Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|set-módszer|A gyökérelem. hello elem hello értéke határozza meg a hello HTTP-metódus.|Igen|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="SetStatus"></a>Set-állapotkód:  
 Hello `set-status` házirend beállítása hello HTTP állapot kód toohello megadott érték.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Példa  
 A példa bemutatja, hogyan tooreturn 401-es választ, ha hello engedélyezési jogkivonat érvénytelen. További információkért lásd: [hello Azure API Management szolgáltatás a külső services használatával](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|állapot beállítása|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|kód = "integer"|hello HTTP-állapot kódját tooreturn.|Igen|N/A|  
|oka = "karakterlánc"|A leírás hello hello állapotkód visszaküldésére használatos.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** kimenő, háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  

##  <a name="set-variable"></a>Új érték  
 Hello `set-variable` házirend deklarál egy [környezetben](api-management-policy-expressions.md#ContextVariables) változó, és egy megadott értéket rendeli az [kifejezés](api-management-policy-expressions.md) vagy egy szöveges karakterlánc. Ha hello kifejezés tartalmaz lesz konvertálva szövegkonstans tooa karakterlánc és hello: hello érték lesz `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a>Házirendutasítás  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a>Példa  
 hello következő példa bemutatja a változó szabály beállítása a hello bejövő szakasz. A set változó házirend létrehoz egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) tootrue van beállítva, ha hello változó `User-Agent` kérelem fejléc hello szöveget tartalmaz `iPad` vagy `iPhone`.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|Set-változó|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|  
|---------------|-----------------|--------------|  
|név|hello hello változó neve.|Igen|  
|érték|hello hello változó értékét. Ez lehet egy kifejezést, vagy konstansérték.|Igen|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
###  <a name="set-variableAllowedTypes"></a>Engedélyezett típusokkal  
 Hello használt kifejezések `set-variable` házirendet a következő típus hello valamelyikét kell visszaadnia.  
  
-   System.Boolean  
  
-   System.SByte  
  
-   System.Byte  
  
-   System.UInt16  
  
-   System.UInt32  
  
-   System.UInt64  
  
-   System.Int16  
  
-   System.Int32  
  
-   System.Int64  
  
-   System.Decimal  
  
-   System.Single  
  
-   System.Double  
  
-   System.Guid  
  
-   System.String  
  
-   System.Char  
  
-   System.DateTime  
  
-   System.TimeSpan  
  
-   System.Byte?  
  
-   System.UInt16?  
  
-   System.UInt32?  
  
-   System.UInt64?  
  
-   System.Int16?  
  
-   System.Int32?  
  
-   System.Int64?  
  
-   System.Decimal?  
  
-   System.Single?  
  
-   System.Double?  
  
-   System.Guid?  
  
-   System.String?  
  
-   System.Char?  
  
-   System.DateTime?  

##  <a name="Trace"></a>Nyomkövetési  
 Hello `trace` házirend hozzáadja egy karakterlánc hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti. hello házirend sorozatot hajthat végre, csak ha nyomkövetés akkor váltódik ki, azaz `Ocp-Apim-Trace` fejléc jelen, és a beállított túl`true` és `Ocp-Apim-Subscription-Key` fejléc szerepel, valamint hello rendszergazdai fiókhoz társított érvényes kulcs.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|Nyomkövetési|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|Forrás|Karakterlánc literális jelentéssel bíró toohello trace viewer és üdvözlőüzenetére megadását hello forrását.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** minden hatókör  
  
##  <a name="Wait"></a>várj  
 Hello `wait` házirend hajt végre annak közvetlenül alárendelt házirendjei párhuzamosan, és az összes vagy egy annak közvetlenül alárendelt házirendek toocomplete vár befejeződése után. hello várakozási házirend lehetnek azonnali gyermek házirendjeit [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), és [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Példa  
 A következő példa hello nincsenek két `choose` közvetlenül alárendelt házirendek a hello házirendekben `wait` házirend. Ezek mindegyikének `choose` házirendek hajt végre párhuzamosan. Minden egyes `choose` házirend kísérletek tooretrieve gyorsítótárazott értéket. Ha a gyorsítótár-tévesztései, háttérszolgáltatás tooprovide hello érték neve. Az ebben a példában hello `wait` házirend végrehajtásához összes közvetlenül alárendelt házirendjeit végrehajtani, mert hello `for` attribútum értéke túl`all`.   A példa hello környezeti változók (`execute-branch-one`, `value-one`, `execute-branch-two`, és `value-two`) deklarált hello kereteit ezt például a házirendet.  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a>Elemek  
  
|Elem|Leírás|Szükséges|  
|-------------|-----------------|--------------|  
|várj|A gyökérelem. Tartalmazhat, mint a gyermekelemek csak `send-request`, `cache-lookup-value`, és `choose` házirendek.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|A|Meghatározza, hogy hello `wait` házirend megvárja-e a minden közvetlenül alárendelt házirendek toobe befejeződött vagy csak egy. Engedélyezett értékek a következők:<br /><br /> -   `all`-Várjon, amíg az összes közvetlenül alárendelt házirendek toocomplete<br />-a - bármely közvetlenül alárendelt házirend toocomplete várja. Hello első közvetlenül alárendelt házirend befejezése után hello `wait` házirend befejeződött, és bármely más közvetlenül alárendelt házirendek végrehajtása megszakadt.|Nem|Minden|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér  
  
-   **Házirend hatókörök:**minden hatókör  
  
## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd:
-   [Az API Management házirendek](api-management-howto-policies.md) 
-   [Házirend-kifejezések](api-management-policy-expressions.md)
