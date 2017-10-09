---
title: "az API Management-átalakítási csoportházirendek aaaAzure |} Microsoft Docs"
description: "Hello átalakítási csoportházirendek használható az Azure API Management megismerése."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2891cc52d0017b717b3c12a98bc4941b5fd7ea78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-transformation-policies"></a>Az API Management-átalakítási csoportházirendek
Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello. Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="TransformationPolicies"></a>Átalakítási csoportházirendek  
  
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
  
##  <a name="ConvertJSONtoXML"></a>Alakítsa át a JSON tooXML  
 Hello `json-to-xml` házirend kérés vagy válasz törzsében konvertálja a JSON tooXML.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|JSON-xml|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|alkalmazása|hello attribútumot a következő értékek hello tooone kell beállítani.<br /><br /> átalakítás - mindig - mindig érvényesek.<br />-tartalom típus-json - konvertálás csak akkor, ha a response Content-Type fejléc JSON jelenlétét jelzi.|Igen|N/A|  
|Vegye figyelembe-elfogadja-fejléc|hello attribútumot a következő értékek hello tooone kell beállítani.<br /><br /> átalakítás - igaz - alkalmazni, amennyiben JSON kérelem Accept fejlécet.<br />-hamis - átalakítás mindig érvényesek.|Nem|Igaz|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő, hiba  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="ConvertXMLtoJSON"></a>XML-tooJSON átalakítása  
 Hello `xml-to-json` házirend XML tooJSON kérés vagy válasz törzsében konvertál. Ez a házirend API-k alapján háttér csak XML-webszolgáltatások használt toomodernize lehet.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|XML-json|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|típusa|hello attribútumot a következő értékek hello tooone kell beállítani.<br /><br /> -javascript-barát - hello alakítja át a JSON egy űrlap felhasználóbarát tooJavaScript fejlesztők rendelkezik.<br />-közvetlen – hello konvertált JSON tükrözi hello eredeti XML-dokumentum struktúra.|Igen|N/A|  
|alkalmazása|hello attribútumot a következő értékek hello tooone kell beállítani.<br /><br /> -mindig - átalakítás mindig.<br />-tartalom típus-xml - konvertálás csak akkor, ha a response Content-Type fejléc XML jelenlétét jelzi.|Igen|N/A|  
|Vegye figyelembe-elfogadja-fejléc|hello attribútumot a következő értékek hello tooone kell beállítani.<br /><br /> átalakítás - igaz - alkalmazni, amennyiben XML Accept fejlécet kérés van szükség.<br />-hamis - átalakítás mindig érvényesek.|Nem|Igaz|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő, hiba  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="Findandreplacestringinbody"></a>Keresés és csere törzsében karakterlánc  
 Hello `find-and-replace` házirend kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|keresése és cseréje|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|A|hello karakterlánc toosearch számára.|Igen|N/A|  
|erre:|hello behelyettesítendő karakterláncot. Adjon meg egy nulla hosszúságú helyettesítő karakterlánc tooremove hello keresési karakterlánc.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="MaskURLSContent"></a>A tartalom maszk URL-címek  
 Hello `redirect-content-urls` házirend átírja hello válasz törzsében (maszkok) hivatkozásokat úgy, hogy azok pont toohello egyenértékű hivatkozás hello átjárón keresztül. A hello kimenő szakasz toore-írási válasz törzsében hivatkozások toomake őket pont toohello átjáró használatára. Hello használható bejövő ellentétes hatás szakasz.  
  
> [!NOTE]
>  Ez a házirend nem változik, mint bármely térközkaraktert `Location` fejléceket. toochange fejléc értékei, használja a hello [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|az átirányítási-tartalom-URL-címek|A gyökérelem.|Igen|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="SetBackendService"></a>Háttér-szolgáltatás beállítása  
 Használjon hello `set-backend-service` házirend tooredirect egy bejövő kérelem tooa különböző háttér, mint egy megadott hello API-beállítások az adott műveletre vonatkozó hello. Ez a házirend hello háttér szolgáltatás alap URL-CÍMÉT hello bejövő kérelem toohello hello házirendben megadott változik.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <choose>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">  
                <set-backend-service base-url="http://contoso.com/api/8.2/" />  
            </when>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">  
                <set-backend-service base-url="http://contoso.com/api/9.1/" />  
            </when>  
        </choose>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
Ez a példa hello a háttér-szolgáltatás szabály beállítása továbbítja a kérelmet hello lekérdezési karakterlánc tooa különböző háttérszolgáltatást hello egy meghatározott hello API mint az átadott hello verzióértéket alapján.
  
Kezdetben hello háttér alap URL-címe, amelyből származik hello API-beállítások. Ezért a kérelem URL-címe hello `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` válik `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` ahol `http://contoso.com/api/10.4/` hello háttérkiszolgáló URL-címe a hello API-beállításaiban megadott.  
  
Ha hello [< válasszon\> ](api-management-advanced-policies.md#choose) alkalmazott házirend-utasítás hello háttér szolgáltatás alap URL-címet módosíthatja újra vagy túl`http://contoso.com/api/8.2` vagy `http://contoso.com/api/9.1`hello hello verzió kérelem lekérdezési paraméter értéke attól függően, hogy. Például ha hello értéke `"2013-15"` hello végső kérelem URL-cím lesz `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
Ha további hello kérelem átalakítása kívánt, más [átalakítási csoportházirendek](api-management-transformation-policies.md#TransformationPolicies) is használható. Például tooremove hello verzió lekérdezési paraméter most, hogy hello kérelem folyamatban van irányítva tooa verziót adott háttér hello [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) házirend lehet használt tooremove hello most redundáns version attribútum.  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" /> 
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
A a példa hello házirend útvonalak hello kérelem tooa service fabric háttérbeli hello userId lekérdezési karakterlánc hello partíciós kulcs használatával, és segítségével hello hello partíció elsődleges másodpéldány.  

### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|háttér-szolgáltatás beállítása|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|alap-URL-cím|Új háttér alap URL-címe.|Nem|N/A|  
|háttér-azonosító|Hello háttér tooroute az azonosítója.|Nem|N/A|  
|ú partíciókulcs|Csak érvényes hello háttér Service Fabric-szolgáltatás, és a "háttér-id" van megadva. Használt tooresolve egy adott partícióra hello name resolution szolgáltatást.|Nem|N/A|  
|ú-replika-típusa|Csak érvényes hello háttér Service Fabric-szolgáltatás, és a "háttér-id" van megadva. Szabályozza, hogy hello kérelem toohello elsődleges vagy másodlagos replikáján partíció kell lépjen. |Nem|N/A|    
|ú-resolve-feltétel|Csak érvényes hello háttér esetén a Service Fabric-szolgáltatás. A feltétel azonosítja Ha hello hívás tooService háló háttér rendelkezik új megoldás ismételni toobe.|Nem|N/A|    
|ú-példány-szolgáltatásnév|Csak érvényes hello háttér esetén a Service Fabric-szolgáltatás. Lehetővé teszi, hogy toochange szolgáltatáspéldány futásidőben. |Nem|N/A|    

### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, háttér  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="SetBody"></a>Set törzse  
 Használjon hello `set-body` házirend tooset hello üzenettörzs a bejövő és kimenő kérelmek. tooaccess hello hello is használhatja az üzenet törzse `context.Request.Body` tulajdonság vagy hello `context.Response.Body`, attól függően, hogy hello házirend van hello bejövő vagy kimenő szakasz.  
  
> [!IMPORTANT]
>  Vegye figyelembe, hogy amikor hozzáfér alapértelmezés szerint hello üzenet törzsének használatával `context.Request.Body` vagy `context.Response.Body`, hello eredeti üzenet törzsének elvész, és vissza hello kifejezésben hello törzs vissza kell állítani. tartalom, toopreserve hello törzs hello beállítása `preserveContent` paraméter túl`true` üdvözlőüzenetére való hozzáféréskor. Ha `preserveContent` értéke túl`true` és egy másik szervezet által visszaadott hello kifejezés hello visszaadott törzs szolgál.  
>   
>  Ne feledje hello használata esetén a következő szempontok hello `set-body` házirend.  
>   
>  -   Hello használata `set-body` házirend tooreturn tooset nem szükséges új vagy frissített törzs `preserveContent` túl`true` mert kifejezetten megadja hello új üzenettörzs tartalmát.  
> -   A válasz hello tartalom megőrzése hello bejövő folyamat értelmetlen, mert még nincs válasz.  
> -   A kérelem tartalma hello megőrzése hello kimenő folyamat értelmetlen mert hello kérelem már elküldtek toohello háttér ezen a ponton.  
> -   Ha ezt a házirendet használja, amikor nincs az üzenet törzse, például egy bejövő GET, a rendszer kivételt hoz létre.  
  
 További információkért lásd: hello `context.Request.Body`, `context.Response.Body`, és hello `IMessage` hello részeiben [környezeti változó](api-management-policy-expressions.md#ContextVariables) tábla.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="literal-text-example"></a>Szöveg – példa  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a>Példa hello törzs karakterláncként elérése. Vegye figyelembe, hogy hello eredeti kérelemtörzset, hogy a Microsoft-e hozzáférési engedélye hello folyamat későbbi szakaszában vannak megőrzi.
  
```xml  
<set-body>  
@{   
    string inBody = context.Request.Body.As<string>(preserveContent: true);   
    if (inBody[0] =='c') {   
        inBody[0] = 'm';   
    }   
    return inBody;   
}  
</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a>A példa egy JObject hello szervezettől elérése. Vegye figyelembe, hogy a beállítást, mivel jelenleg nem lefoglalja hello eredeti kérelemtörzset, egypéldányú hello folyamat későbbi szakaszában okoz kivételt.  
  
```xml  
<set-body>   
@{   
    JObject inBody = context.Request.Body.As<JObject>();   
    if (inBody.attribute == <tag>) {   
        inBody[0] = 'm';   
    }   
    return inBody.ToString();   
}   
</set-body>  
  
```  
  
#### <a name="filter-response-based-on-product"></a>A termék szűrő választ  
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

### <a name="using-liquid-templates-with-set-body"></a>A készlet szövegtörzzsel folyékony sablonokkal 
Hello `set-body` házirend lehet konfigurált toouse hello [folyékony](https://shopify.github.io/liquid/basics/introduction/) templating nyelvi tootransfom hello törzsének olyan kérésre vagy válaszra. Ez nagyon hatékony, ha az üzenet toocompletely átalakítás hello formátumát szüksége lehet.

> [!IMPORTANT]
> hello használt folyadék végrehajtásának hello `set-body` házirend úgy van konfigurálva, a "C# módban". Ez különösen fontos műveleteket, mint a szűrés során. Tegyük fel, a dátum szűrő használatához Pascal hello használata és nagybetűhasználatnak, valamint C# dátum formázás, például:
>
> {{body.foo.startDateTime| Dátum: "yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> A sorrend toocorrectly bind tooan XML-törzsére hello folyékony sablonnal, használja a `set-header` házirend tooset Content-Type tooeither application/xml, text/xml (vagy bármely típusú végződő + xml); a JSON-törzsére értéke lehet az application/json, text/json (vagy bármilyen típusú befejezése a + json).

#### <a name="convert-json-toosoap-using-a-liquid-template"></a>Alakítsa át a JSON tooSOAP folyékony sablon használatával
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="tranform-json-using-a-liquid-template"></a>Tranform JSON folyékony sablon használatával
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|set-szervezet|A gyökérelem. Hello szöveg vagy egy törzs visszaadó kifejezések tartalmaz.|Igen|  

### <a name="properties"></a>Tulajdonságok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|sablon|Törzs szabály beállítása hello használt toochange hello templating módban fog futni. Jelenleg csak a támogatott hello érték a következő:<br /><br />-folyékony - hello beállítása törzs házirend hello folyékony templating motor fogja használni. |Nem|folyadék|  

Hello kérelem-válasz információ eléréséhez, hello folyékony sablon az alábbi tulajdonságokkal hello köthető tooa környezeti objektumot: <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="SetHTTPheader"></a>HTTP-fejléc beállítása  
 Hello `set-header` házirendet hozzárendeli egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.  
  
 A HTTP-fejlécek listáját szúr be a HTTP üzenet. Ha egy bejövő folyamat, ez a házirend hello HTTP-fejlécek átadott toohello célszolgáltatás hello kérelem állítja be. Ha egy kimenő folyamat, ez a házirend hello HTTP-fejlécek toohello átjáró ügyfél küldött hello választ állítja be.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="example"></a>Példa  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Környezeti információkat toohello háttérszolgáltatást továbbítsa  
 Ez a példa bemutatja, hogyan hello API tooapply házirendje szinten toosupply környezetben információk toohello háttérszolgáltatáshoz. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too10:30 előre. 12:10 nincs a művelet hívásának hello developer portálon, ahol láthatja a munkahelyi hello házirend bemutató.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|set-fejléc|A gyökérelem.|Igen|  
|érték|Megadja a hello fejléc toobe set hello értékét. Hello fejléc több azonos nevű vegye fel az további `value` elemek.|Igen|  
  
### <a name="properties"></a>Tulajdonságok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|létezik-e művelet|Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva. Ez az attribútum hello a következő értékek egyikének kell lennie.<br /><br /> -felülbírálás - cserél hello értéke hello meglévő fejléc.<br />-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.<br />-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.<br />-törlés – hello kérelemből eltávolítja hello fejlécet.<br /><br /> Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.|Nem|felülbírálás|  
|név|Hello fejléc toobe készlet nevét adja meg.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő háttér,-hiba  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="SetQueryStringParameter"></a>Lekérdezési karakterlánc-paraméter beállítása  
 Hello `set-query-parameter` házirendnek, cserél értékét, vagy a törlések kérelem lekérdezési karakterláncot. Lehet használt toopass hello háttérszolgáltatást által várt lekérdezési paramétereket, amelyek választható vagy soha nem szerepelnek a hello kérelem.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a>Példák  
  
#### <a name="example"></a>Példa  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Környezeti információkat toohello háttérszolgáltatást továbbítsa  
 Ez a példa bemutatja, hogyan hello API tooapply házirendje szinten toosupply környezetben információk toohello háttérszolgáltatáshoz. A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too10:30 előre. 12:10 nincs a művelet hívásának hello developer portálon, ahol láthatja a munkahelyi hello házirend bemutató.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|a Set-lekérdezés-paraméter|A gyökérelem.|Igen|  
|érték|Megadja a hello lekérdezés paraméterhalmaz toobe hello értékét. Több lekérdezés paramétereket hello ugyanazt a nevet adja hozzá további `value` elemek.|Igen|  
  
### <a name="properties"></a>Tulajdonságok  
  
|Név|Leírás|Szükséges|Alapértelmezett|  
|----------|-----------------|--------------|-------------|  
|létezik-e művelet|Milyen műveletet tootake határozza meg, amikor hello lekérdezési paraméter már meg van adva. Ez az attribútum hello a következő értékek egyikének kell lennie.<br /><br /> -felülbírálás - cserél hello hello meglévő paraméter értékét.<br />-skip - függvény nem cseréli le hello meglévő lekérdezési paraméter értéke.<br />-hozzáfűzése - hozzáfűzi hello érték toohello meglévő lekérdezési paraméter értéke.<br />-törlés - eltávolítása hello kérelem hello lekérdezési paraméter.<br /><br /> Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello lekérdezési paraméter beállítása függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzés.|Nem|felülbírálás|  
|név|Hello lekérdezési paraméter toobe készlet nevét adja meg.|Igen|N/A|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, háttér  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
##  <a name="RewriteURL"></a>URL-cím újraírása  
 Hello `rewrite-uri` házirend a nyilvános űrlap toohello formájukban hello webszolgáltatás, amelyet várt alakítja a kérelem URL-CÍMÉT, ahogy az alábbi példa hello.  
  
-   Nyilvános URL-`http://api.example.com/storenumber/ordernumber`  
  
-   Kérelem URL-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Ez a házirend használhatja emberi és/vagy a böngésző-barát URL-címet kell alakul hello webszolgáltatás által várt hello URL-cím formátumba. Ez a házirend csak kell alkalmazni, amikor egy másik URL-cím formátumban, például a tiszta URL-címeket, a RESTful URL-címeket, a felhasználóbarát URL-címek vagy a Keresőmotor-barát URL-címek, amelyek tisztán strukturális URL-címek, amely nem tartalmazhat lekérdezési karakterláncot, és helyette a hello csak hello elérési tartalmaz ilyen toobe az erőforrás (után hello séma és a hello szolgáltató). Ez gyakran történik esztétikai, a használhatóság és a keresőmotor (keresőmotor-Optimalizáláshoz) optimalizálási céllal.  
  
> [!NOTE]
>  Csak a lekérdezési karakterlánc paraméterek hello házirend használatával adhat hozzá. További elérési Sablonparaméterek a hello újraírási URL-cím nem vehető fel.  

### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put?c=d -->
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" copy-unmatched-params="false" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|Módosítsa úgy-uri|A gyökérelem.|Igen|  
  
### <a name="attributes"></a>Attribútumok  
  
|Attribútum|Leírás|Szükséges|Alapértelmezett|  
|---------------|-----------------|--------------|-------------|  
|sablon|hello tényleges webes szolgáltatás URL-cím elé a lekérdezési karakterlánc paramétereket. Kifejezések használata esetén a hello egész értéknek kell lennie egy kifejezés.|Igen|N/A|  
|Másolás páratlan számú paraméterei|Megadja, hogy a lekérdezés-paraméterek a hello bejövő kérelem nem található meg a hello eredeti URL-cím sablon kerülnek, toohello hello által megadott URL-cím újbóli írása sablon|Nem|Igaz|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő  
  
-   **Házirend hatókörök:** termék, API-művelet  
  
##  <a name="XSLTransform"></a>Az XSLT-vel XML átalakítása  
 Hello `Transform XML using an XSLT` házirend vonatkozik egy XSL átalakítása tooXML hello kérés vagy válasz törzsében.  
  
### <a name="policy-statement"></a>Házirendutasítás  
  
```xml  
<xsl-transform>  
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
        <xsl:output method="xml" indent="yes" />  
        <xsl:param name="User-Agent" />  
        <xsl:template match="* | @* | node()">  
            <xsl:copy>  
                <xsl:if test="self::* and not(parent::*)">  
                    <xsl:attribute name="User-Agent">  
                        <xsl:value-of select="$User-Agent" />  
                    </xsl:attribute>  
                </xsl:if>  
                <xsl:apply-templates select="* | @* | node()" />  
            </xsl:copy>  
        </xsl:template>  
    </xsl:stylesheet>  
  </xsl-transform>  
```  
  
### <a name="example"></a>Példa  
  
```xml  
<policies>  
  <inbound>  
      <base />  
  </inbound>  
  <outbound>  
      <base />  
      <xsl-transform>  
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />  
            <!-- Copy all nodes directly-->  
            <xsl:template match="node()| @*|*">  
                <xsl:copy>  
                    <xsl:apply-templates select="@* | node()|*" />  
                </xsl:copy>  
            </xsl:template>  
        </xsl:stylesheet>  
    </xsl-transform>  
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemek  
  
|Név|Leírás|Szükséges|  
|----------|-----------------|--------------|  
|XSL-átalakítás|A gyökérelem.|Igen|  
|A paraméter|Hello átalakító használt használt toodefine változók|Nem|  
|XSL: stylesheet|Stíluslap gyökérelem. Minden elemek és attribútumok vannak meghatározva kövesse hello standard [XSLT-specifikációja](http://www.w3.org/TR/xslt)|Igen|  
  
### <a name="usage"></a>Használat  
 Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Házirend szakaszok:** bejövő, kimenő  
  
-   **Házirend hatókörök:** globális, termék, API-művelet  
  
## <a name="next-steps"></a>Következő lépések
Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).  
