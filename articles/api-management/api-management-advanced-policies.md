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
# <a name="api-management-advanced-policies"></a><span data-ttu-id="1029f-103">Házirendek speciális API Management</span><span class="sxs-lookup"><span data-stu-id="1029f-103">API Management advanced policies</span></span>
<span data-ttu-id="1029f-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="1029f-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="1029f-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="1029f-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="1029f-106"><a name="AdvancedPolicies"></a>Speciális házirendek</span><span class="sxs-lookup"><span data-stu-id="1029f-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="1029f-107">[Folyamat szabályozása](api-management-advanced-policies.md#choose) - feltételesen alkalmazza a házirend-utasításoknál logikai hello értékelése hello eredményei alapján [kifejezések](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="1029f-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="1029f-108">[Kérés továbbítása a](#ForwardRequest) -hello kérelem toohello háttérszolgáltatást továbbítja.</span><span class="sxs-lookup"><span data-stu-id="1029f-108">[Forward request](#ForwardRequest) - Forwards hello request toohello backend service.</span></span>

-   <span data-ttu-id="1029f-109">[Korlátozza a feldolgozási](#LimitConcurrency) -megakadályozza, hogy a házirendek által megadott számú kérelem hello egyszerre egynél több végrehajtása közé.</span><span class="sxs-lookup"><span data-stu-id="1029f-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than hello specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="1029f-110">[TooEvent központ naplófájl](#log-to-eventhub) -hello üzeneteket küld a megadott formátumban tooan Eseményközpont határozzák meg a tranzakciónaplókat tartalmazó entitás.</span><span class="sxs-lookup"><span data-stu-id="1029f-110">[Log tooEvent Hub](#log-to-eventhub) - Sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="1029f-111">[Válasz mock](#mock-response) -megszakításainak csővezeték-végrehajtási mocked választ ad vissza, és közvetlenül toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="1029f-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly toohello caller.</span></span>
  
-   <span data-ttu-id="1029f-112">[Próbálja meg újra](#Retry) -újrapróbálkozások végrehajtásának hello házirend-utasításoknál zárójelek között, ha és hello feltétel teljesüléséig.</span><span class="sxs-lookup"><span data-stu-id="1029f-112">[Retry](#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="1029f-113">Végrehajtási fog ismétlődő hello megadott időközök és mentése megadott toohello újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="1029f-113">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
-   <span data-ttu-id="1029f-114">[Térjen vissza a válasz](#ReturnResponse) -megszakításainak feldolgozási sor végrehajtása és visszaadja a hello megadott válasz közvetlenül toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="1029f-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span> 
  
-   <span data-ttu-id="1029f-115">[Egyirányú kérés küldése](#SendOneWayRequest) -elküld egy kérést toohello megadott URL-címet a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="1029f-115">[Send one way request](#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="1029f-116">[Kérés küldése](#SendRequest) -küld egy kérelem toohello megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1029f-116">[Send request](#SendRequest) - Sends a request toohello specified URL.</span></span>  

-   <span data-ttu-id="1029f-117">[Állítsa be a HTTP-proxy](#SetHttpProxy) -lehetővé teszi egy HTTP-proxyn keresztül továbbított tooroute kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="1029f-117">[Set HTTP proxy](#SetHttpProxy) - Allows you tooroute forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="1029f-118">[Kérelem módszert állítja be](#SetRequestMethod) -lehetővé teszi a kérelmek toochange hello HTTP metódus.</span><span class="sxs-lookup"><span data-stu-id="1029f-118">[Set request method](#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="1029f-119">[Állítsa be. állapotkód:](#SetStatus) -módosítások hello HTTP-állapot kód toohello megadott érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-119">[Set status code](#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
-   <span data-ttu-id="1029f-120">[Új érték](api-management-advanced-policies.md#set-variable) -továbbra is fennáll az elnevezett értéket [környezetben](api-management-policy-expressions.md#ContextVariables) változó későbbi eléréshez.</span><span class="sxs-lookup"><span data-stu-id="1029f-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="1029f-121">[Nyomkövetési](#Trace) -karakterlánc hozzáadja hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="1029f-121">[Trace](#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="1029f-122">[Várjon, amíg](#Wait) -megvárja-e a zárójelek között [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), vagy [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek toocomplete a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="1029f-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
##  <span data-ttu-id="1029f-123"><a name="choose"></a>Folyamata</span><span class="sxs-lookup"><span data-stu-id="1029f-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="1029f-124">Hello `choose` házirend utasítások logikai kifejezésen, ha-akkor más hasonló tooan vagy kapcsoló értékelése hello eredménye alapján a programozási nyelven összeállításához zárt házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1029f-124">hello `choose` policy applies enclosed policy statements based on hello outcome of evaluation of Boolean expressions, similar tooan if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="1029f-125"><a name="ChoosePolicyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
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
  
 <span data-ttu-id="1029f-126">hello vezérlő adatfolyam házirend tartalmaznia kell legalább egy `<when/>` elemet.</span><span class="sxs-lookup"><span data-stu-id="1029f-126">hello control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="1029f-127">Hello `<otherwise/>` elem nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="1029f-127">hello `<otherwise/>` element is optional.</span></span> <span data-ttu-id="1029f-128">A feltételek `<when/>` elemek kiértékelése sorrendben történik, illetve megjelenésük hello házirend.</span><span class="sxs-lookup"><span data-stu-id="1029f-128">Conditions in `<when/>` elements are evaluated in order of their appearance within hello policy.</span></span> <span data-ttu-id="1029f-129">Először határolt hello házirend nyilatkozatuk `<when/>` feltétel attribútum egyenlő elem `true` lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="1029f-129">Policy statement(s) enclosed within hello first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="1029f-130">Hello határolt házirendek `<otherwise/>` elem, ha van ilyen, alkalmazza a rendszer, ha az összes hello a `<when/>` elem feltétel attribútumok `false`.</span><span class="sxs-lookup"><span data-stu-id="1029f-130">Policies enclosed within hello `<otherwise/>` element, if present, will be applied if all of hello `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="1029f-131">Példák</span><span class="sxs-lookup"><span data-stu-id="1029f-131">Examples</span></span>  
  
####  <span data-ttu-id="1029f-132"><a name="ChooseExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="1029f-133">hello következő példa bemutatja egy [set-változó](api-management-advanced-policies.md#set-variable) házirend és a két vezérlése áramlási szabályzatokkal.</span><span class="sxs-lookup"><span data-stu-id="1029f-133">hello following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="1029f-134">változó szabály hello beállítása megtalálható hello bejövő szakaszt, és létrehoz egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) változó, amely tootrue van beállítva, ha hello `User-Agent` kérelem fejléc hello szöveget tartalmaz `iPad` vagy `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="1029f-134">hello set variable policy is in hello inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="1029f-135">hello első vezérlő adatfolyam házirend egyben a bejövő szakasz hello és feltételesen alkalmaz egy két [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) attól hello hello értékének `isMobile` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="1029f-135">hello first control flow policy is also in hello inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on hello value of hello `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="1029f-136">hello második ellenőrzési folyamata házirend hello kimenő szakaszban, és feltételesen vonatkozik hello [XML konvertálása tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) házirend amikor `isMobile` értéke túl`true`.</span><span class="sxs-lookup"><span data-stu-id="1029f-136">hello second control flow policy is in hello outbound section and conditionally applies hello [Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set too`true`.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="1029f-137">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-137">Example</span></span>  
 <span data-ttu-id="1029f-138">Ez a példa bemutatja, hogyan tooperform tartalom alapján történő szűrés adatelemek eltávolítása hello választ kapott hello háttérszolgáltatást hello használatakor `Starter` termék.</span><span class="sxs-lookup"><span data-stu-id="1029f-138">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="1029f-139">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too34:30 előre.</span><span class="sxs-lookup"><span data-stu-id="1029f-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="1029f-140">Áttekintést 31:50 toosee kezdjék [sötét égbolt előrejelzési API hello](https://developer.forecast.io/) ebben a bemutatóban használt.</span><span class="sxs-lookup"><span data-stu-id="1029f-140">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-141">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-141">Elements</span></span>  
  
|<span data-ttu-id="1029f-142">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-142">Element</span></span>|<span data-ttu-id="1029f-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-143">Description</span></span>|<span data-ttu-id="1029f-144">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-145">Válassza a</span><span class="sxs-lookup"><span data-stu-id="1029f-145">choose</span></span>|<span data-ttu-id="1029f-146">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-146">Root element.</span></span>|<span data-ttu-id="1029f-147">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-147">Yes</span></span>|  
|<span data-ttu-id="1029f-148">mikor</span><span class="sxs-lookup"><span data-stu-id="1029f-148">when</span></span>|<span data-ttu-id="1029f-149">a hello feltétel toouse hello `if` vagy `ifelse` hello részei `choose` házirend.</span><span class="sxs-lookup"><span data-stu-id="1029f-149">hello condition toouse for hello `if` or `ifelse` parts of hello `choose` policy.</span></span> <span data-ttu-id="1029f-150">Ha hello `choose` -házirendben engedélyezve van több `when` szakaszok, azok egymás után értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="1029f-150">If hello `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="1029f-151">Egyszer hello `condition` , hogy amikor egy elem értékeli ki túl`true`, további `when` feltételek értékelésének.</span><span class="sxs-lookup"><span data-stu-id="1029f-151">Once hello `condition` of a when element evaluates too`true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="1029f-152">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-152">Yes</span></span>|  
|<span data-ttu-id="1029f-153">Ellenkező esetben</span><span class="sxs-lookup"><span data-stu-id="1029f-153">otherwise</span></span>|<span data-ttu-id="1029f-154">Hello házirend részlet toobe használható, ha nincs hello tartalmaz `when` feltételek kiértékelése túl`true`.</span><span class="sxs-lookup"><span data-stu-id="1029f-154">Contains hello policy snippet toobe used if none of hello `when` conditions evaluate too`true`.</span></span>|<span data-ttu-id="1029f-155">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-156">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-156">Attributes</span></span>  
  
|<span data-ttu-id="1029f-157">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-157">Attribute</span></span>|<span data-ttu-id="1029f-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-158">Description</span></span>|<span data-ttu-id="1029f-159">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="1029f-160">feltétel = "logikai kifejezés &#124; Logikai állandó"</span><span class="sxs-lookup"><span data-stu-id="1029f-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="1029f-161">hello logikai kifejezés, vagy ha hello tartalmazó állandó tooevaluated `when` házirend-utasítás kiértékelése történik.</span><span class="sxs-lookup"><span data-stu-id="1029f-161">hello Boolean expression or constant tooevaluated when hello containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="1029f-162">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-162">Yes</span></span>|  
  
###  <span data-ttu-id="1029f-163"><a name="ChooseUsage"></a>Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="1029f-164">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-164">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-165">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-166">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-167"><a name="ForwardRequest"></a>Kérés továbbítása</span><span class="sxs-lookup"><span data-stu-id="1029f-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="1029f-168">Hello `forward-request` házirend továbbítja hello bejövő kérelem toohello háttérszolgáltatást hello kérelemben megadott [környezet](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="1029f-168">hello `forward-request` policy forwards hello incoming request toohello backend service specified in hello request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="1029f-169">hello háttérkiszolgáló URL-címe szerepel hello API [beállítások](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) és hello segítségével módosítható [be háttérszolgáltatás](api-management-transformation-policies.md) házirend.</span><span class="sxs-lookup"><span data-stu-id="1029f-169">hello backend service URL is specified in hello API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using hello [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="1029f-170">A csoportházirend-eredmények nem lesznek továbbítva toohello háttér szolgáltatás és hello hello kimenő szakaszban házirendek hello kérelem hello házirendek segítségével a hello hello sikeres befejezését követően azonnal kiértékelése eltávolítása bejövő szakasz.</span><span class="sxs-lookup"><span data-stu-id="1029f-170">Removing this policy results in hello request not being forwarded toohello backend service and hello policies in hello outbound section are evaluated immediately upon hello successful completion of hello policies in hello inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-171">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="1029f-172">Példák</span><span class="sxs-lookup"><span data-stu-id="1029f-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="1029f-173">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-173">Example</span></span>  
 <span data-ttu-id="1029f-174">hello következő API-szintű szabályzat továbbítja az összes kérelem toohello háttérszolgáltatást 60 másodperces időtúllépési időköz.</span><span class="sxs-lookup"><span data-stu-id="1029f-174">hello following API level policy forwards all requests toohello backend service with a timeout interval of 60 seconds.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="1029f-175">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-175">Example</span></span>  
 <span data-ttu-id="1029f-176">Ez a művelet szintű szabályzat használ hello `base` elem tooinherit hello háttér házirend hello szülő API szintű hatókörből.</span><span class="sxs-lookup"><span data-stu-id="1029f-176">This operation level policy uses hello `base` element tooinherit hello backend policy from hello parent API level scope.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="1029f-177">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-177">Example</span></span>  
 <span data-ttu-id="1029f-178">Ez a művelet szintű szabályzat explicit módon továbbítja az összes kérelem toohello háttérszolgáltatást egy időkorlát 120, és nem örököl a hello szülő API-szintű háttér házirend.</span><span class="sxs-lookup"><span data-stu-id="1029f-178">This operation level policy explicitly forwards all requests toohello backend service with a timeout of 120 and does not inherit hello parent API level backend policy.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="1029f-179">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-179">Example</span></span>  
 <span data-ttu-id="1029f-180">Ez a művelet szintű szabályzat nem továbbítja a kérelmek toohello háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="1029f-180">This operation level policy does not forward requests toohello backend service.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-181">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-181">Elements</span></span>  
  
|<span data-ttu-id="1029f-182">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-182">Element</span></span>|<span data-ttu-id="1029f-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-183">Description</span></span>|<span data-ttu-id="1029f-184">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-185">előre-kérés</span><span class="sxs-lookup"><span data-stu-id="1029f-185">forward-request</span></span>|<span data-ttu-id="1029f-186">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-186">Root element.</span></span>|<span data-ttu-id="1029f-187">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-188">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-188">Attributes</span></span>  
  
|<span data-ttu-id="1029f-189">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-189">Attribute</span></span>|<span data-ttu-id="1029f-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-190">Description</span></span>|<span data-ttu-id="1029f-191">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-191">Required</span></span>|<span data-ttu-id="1029f-192">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-193">Időtúllépés = "integer"</span><span class="sxs-lookup"><span data-stu-id="1029f-193">timeout="integer"</span></span>|<span data-ttu-id="1029f-194">hello időkorlátja másodpercben előtt hello hívás toohello háttérszolgáltatást sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="1029f-194">hello timeout interval in seconds before hello call toohello backend service fails.</span></span>|<span data-ttu-id="1029f-195">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-195">No</span></span>|<span data-ttu-id="1029f-196">Nincs időtúllépés</span><span class="sxs-lookup"><span data-stu-id="1029f-196">No timeout</span></span>|  
|<span data-ttu-id="1029f-197">hajtsa végre-átirányítások = "true &#124; FALSE"</span><span class="sxs-lookup"><span data-stu-id="1029f-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="1029f-198">Megadja, hogy hello háttérszolgáltatást átirányítása hello átjáró követ vagy toohello hívó adott vissza.</span><span class="sxs-lookup"><span data-stu-id="1029f-198">Specifies whether redirects from hello backend service are followed by hello gateway or returned toohello caller.</span></span>|<span data-ttu-id="1029f-199">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-199">No</span></span>|<span data-ttu-id="1029f-200">hamis</span><span class="sxs-lookup"><span data-stu-id="1029f-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-201">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-201">Usage</span></span>  
 <span data-ttu-id="1029f-202">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-202">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-203">**Házirend szakaszok:** háttér</span><span class="sxs-lookup"><span data-stu-id="1029f-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="1029f-204">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-205"><a name="LimitConcurrency"></a>A feldolgozási korlátot</span><span class="sxs-lookup"><span data-stu-id="1029f-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="1029f-206">Hello `limit-concurrency` házirend megakadályozza, hogy a zárt házirendek nagyobb, mint a kérések megadott számát egy adott időben hello végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="1029f-206">hello `limit-concurrency` policy prevents enclosed policies from executing by more than hello specified number of requests at a given time.</span></span> <span data-ttu-id="1029f-207">Alapján meghaladó hello küszöbértéket, új kérelmek kerülnek tooa várólista, amíg nem érhető el, hello várólista maximális hossza.</span><span class="sxs-lookup"><span data-stu-id="1029f-207">Upon exceeding hello threshold, new requests are added tooa queue, until hello maximum queue length is achieved.</span></span> <span data-ttu-id="1029f-208">Várólista Erőforrásfogyás esetén új kérelmek sikertelenek lesznek azonnal.</span><span class="sxs-lookup"><span data-stu-id="1029f-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="1029f-209"><a name="LimitConcurrencyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="1029f-210">Példák</span><span class="sxs-lookup"><span data-stu-id="1029f-210">Examples</span></span>  
  
####  <span data-ttu-id="1029f-211"><a name="ChooseExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="1029f-212">hello következő példa bemutatja, hogyan továbbítsa az toolimit kérelmek száma a tooa háttér hello egy környezeti változó értéke alapján.</span><span class="sxs-lookup"><span data-stu-id="1029f-212">hello following example demonstrates how toolimit number of requests forwarded tooa backend based on hello value of a context variable.</span></span>
 
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

### <a name="elements"></a><span data-ttu-id="1029f-213">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-213">Elements</span></span>  
  
|<span data-ttu-id="1029f-214">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-214">Element</span></span>|<span data-ttu-id="1029f-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-215">Description</span></span>|<span data-ttu-id="1029f-216">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="1029f-217">határ-feldolgozási</span><span class="sxs-lookup"><span data-stu-id="1029f-217">limit-concurrency</span></span>|<span data-ttu-id="1029f-218">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-218">Root element.</span></span>|<span data-ttu-id="1029f-219">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-220">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-220">Attributes</span></span>  
  
|<span data-ttu-id="1029f-221">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-221">Attribute</span></span>|<span data-ttu-id="1029f-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-222">Description</span></span>|<span data-ttu-id="1029f-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-223">Required</span></span>|<span data-ttu-id="1029f-224">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="1029f-225">kulcs</span><span class="sxs-lookup"><span data-stu-id="1029f-225">key</span></span>|<span data-ttu-id="1029f-226">Egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1029f-226">A string.</span></span> <span data-ttu-id="1029f-227">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1029f-227">Expression allowed.</span></span> <span data-ttu-id="1029f-228">Hello párhuzamossági hatókör határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1029f-228">Specifies hello concurrency scope.</span></span> <span data-ttu-id="1029f-229">Megoszthatók több házirendet.</span><span class="sxs-lookup"><span data-stu-id="1029f-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="1029f-230">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-230">Yes</span></span>|<span data-ttu-id="1029f-231">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-231">N/A</span></span>|  
|<span data-ttu-id="1029f-232">maximális száma</span><span class="sxs-lookup"><span data-stu-id="1029f-232">max-count</span></span>|<span data-ttu-id="1029f-233">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="1029f-233">An integer.</span></span> <span data-ttu-id="1029f-234">Itt adhatja meg, amelyek számára engedélyezett tooenter hello házirend kérelmek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="1029f-234">Specifies a maximum number of requests that are allowed tooenter hello policy.</span></span>|<span data-ttu-id="1029f-235">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-235">Yes</span></span>|<span data-ttu-id="1029f-236">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-236">N/A</span></span>|  
|<span data-ttu-id="1029f-237">timeout</span><span class="sxs-lookup"><span data-stu-id="1029f-237">timeout</span></span>|<span data-ttu-id="1029f-238">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="1029f-238">An integer.</span></span> <span data-ttu-id="1029f-239">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1029f-239">Expression allowed.</span></span> <span data-ttu-id="1029f-240">Hello kérelmet megvárja-e tooenter egy hatókört, mielőtt hibát jelentene, a másodpercek számát adja meg "403-as túl sok kérelem"</span><span class="sxs-lookup"><span data-stu-id="1029f-240">Specifies hello number of seconds a request should wait tooenter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="1029f-241">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-241">No</span></span>|<span data-ttu-id="1029f-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="1029f-242">Infinity</span></span>|  
|<span data-ttu-id="1029f-243">maximális hosszúságú várólista</span><span class="sxs-lookup"><span data-stu-id="1029f-243">max-queue-length</span></span>|<span data-ttu-id="1029f-244">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="1029f-244">An integer.</span></span> <span data-ttu-id="1029f-245">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1029f-245">Expression allowed.</span></span> <span data-ttu-id="1029f-246">Hello várólista maximális hosszát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1029f-246">Specifies hello maximum queue length.</span></span> <span data-ttu-id="1029f-247">Bejövő kérelmek tooenter közben ezzel a házirend-befejeződik a "403-as túl sok kérelem" azonnal amikor hello várólista kimerült.</span><span class="sxs-lookup"><span data-stu-id="1029f-247">Incoming requests trying tooenter this policy will be terminated with “403 Too Many Requests” immediately when hello queue is exhausted.</span></span>|<span data-ttu-id="1029f-248">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-248">No</span></span>|<span data-ttu-id="1029f-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="1029f-249">Infinity</span></span>|  
  
###  <span data-ttu-id="1029f-250"><a name="ChooseUsage"></a>Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="1029f-251">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-251">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-252">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-253">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="1029f-254"><a name="log-to-eventhub"></a>Napló tooEvent Hub</span><span class="sxs-lookup"><span data-stu-id="1029f-254"><a name="log-to-eventhub"></a> Log tooEvent Hub</span></span>  
 <span data-ttu-id="1029f-255">Hello `log-to-eventhub` hello üzenetek megadott formátum tooan határozzák meg a tranzakciónaplókat tartalmazó entitás Eseményközpont házirend küld.</span><span class="sxs-lookup"><span data-stu-id="1029f-255">hello `log-to-eventhub` policy sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="1029f-256">Mivel a név azt jelenti, hello házirend kijelölt kérés vagy válasz környezeti adatok mentése online vagy kapcsolat nélküli elemzéshez használható.</span><span class="sxs-lookup"><span data-stu-id="1029f-256">As its name implies, hello policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="1029f-257">Részletes útmutató, az event hubs és a naplózási események konfigurálása, lásd: [hogyan toolog API Management eseményeket az Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="1029f-257">For a step-by-step guide on configuring an event hub and logging events, see [How toolog API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-258">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-259">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-259">Example</span></span>  
 <span data-ttu-id="1029f-260">Bármilyen karakterlánc hello érték toobe jelentkezett be az Event Hubs használható.</span><span class="sxs-lookup"><span data-stu-id="1029f-260">Any string can be used as hello value toobe logged in Event Hubs.</span></span> <span data-ttu-id="1029f-261">Ebben a példában hello dátum és idő, telepítési szolgáltatás neve, kérelemazonosító, IP-cím és az összes bejövő hívások műveletnév naplózó regisztrált hello naplózott toohello eseményközpont `contoso-logger` azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1029f-261">In this example hello date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged toohello event hub Logger registered with hello `contoso-logger` id.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-262">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-262">Elements</span></span>  
  
|<span data-ttu-id="1029f-263">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-263">Element</span></span>|<span data-ttu-id="1029f-264">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-264">Description</span></span>|<span data-ttu-id="1029f-265">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-266">napló-eventhub</span><span class="sxs-lookup"><span data-stu-id="1029f-266">log-to-eventhub</span></span>|<span data-ttu-id="1029f-267">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-267">Root element.</span></span> <span data-ttu-id="1029f-268">hello Ez az elem értéke hello karakterlánc toolog tooyour eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="1029f-268">hello value of this element is hello string toolog tooyour event hub.</span></span>|<span data-ttu-id="1029f-269">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-270">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-270">Attributes</span></span>  
  
|<span data-ttu-id="1029f-271">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-271">Attribute</span></span>|<span data-ttu-id="1029f-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-272">Description</span></span>|<span data-ttu-id="1029f-273">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="1029f-274">naplózó-azonosító</span><span class="sxs-lookup"><span data-stu-id="1029f-274">logger-id</span></span>|<span data-ttu-id="1029f-275">hello hello azonosítója naplózó az API Management szolgáltatásban regisztrált.</span><span class="sxs-lookup"><span data-stu-id="1029f-275">hello id of hello Logger registered with your API Management service.</span></span>|<span data-ttu-id="1029f-276">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-276">Yes</span></span>|  
|<span data-ttu-id="1029f-277">partícióazonosító-:</span><span class="sxs-lookup"><span data-stu-id="1029f-277">partition-id</span></span>|<span data-ttu-id="1029f-278">Megadja a hello index hello partíció, ahol az üzenetek küldése történik.</span><span class="sxs-lookup"><span data-stu-id="1029f-278">Specifies hello index of hello partition where messages are sent.</span></span>|<span data-ttu-id="1029f-279">Választható.</span><span class="sxs-lookup"><span data-stu-id="1029f-279">Optional.</span></span> <span data-ttu-id="1029f-280">Ez az attribútum nem használható, ha `partition-key` szolgál.</span><span class="sxs-lookup"><span data-stu-id="1029f-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="1029f-281">a partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="1029f-281">partition-key</span></span>|<span data-ttu-id="1029f-282">Adja meg a használt partíció-hozzárendelés elküldött üzenetek hello érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-282">Specifies hello value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="1029f-283">Választható.</span><span class="sxs-lookup"><span data-stu-id="1029f-283">Optional.</span></span> <span data-ttu-id="1029f-284">Ez az attribútum nem használható, ha `partition-id` szolgál.</span><span class="sxs-lookup"><span data-stu-id="1029f-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-285">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-285">Usage</span></span>  
 <span data-ttu-id="1029f-286">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-286">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-287">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-288">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="1029f-289"><a name="mock-response"></a>Utánzatait válasz</span><span class="sxs-lookup"><span data-stu-id="1029f-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="1029f-290">Hello `mock-response`, hello neveként azt jelenti, toomock API-k és műveletek.</span><span class="sxs-lookup"><span data-stu-id="1029f-290">hello `mock-response`, as hello name implies, is used toomock APIs and operations.</span></span> <span data-ttu-id="1029f-291">Normál feldolgozási sor végrehajtása megszakítja, és egy mocked válasz toohello hívó adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1029f-291">It aborts normal pipeline execution and returns a mocked response toohello caller.</span></span> <span data-ttu-id="1029f-292">hello házirend mindig próbálja meg a legmagasabb hűség tooreturn válaszokat.</span><span class="sxs-lookup"><span data-stu-id="1029f-292">hello policy always tries tooreturn responses of highest fidelity.</span></span> <span data-ttu-id="1029f-293">Válasz tartalom példák, amikor a rendelkezésre álló szívesebben.</span><span class="sxs-lookup"><span data-stu-id="1029f-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="1029f-294">Minta válaszok létrehozott sémák, a sémák találhatók, és többek között nem.</span><span class="sxs-lookup"><span data-stu-id="1029f-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="1029f-295">Ha a példák és sémák nem találhatók, nem tartalmú válaszok is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="1029f-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-296">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="1029f-297">Példák</span><span class="sxs-lookup"><span data-stu-id="1029f-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="1029f-298">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-298">Elements</span></span>  
  
|<span data-ttu-id="1029f-299">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-299">Element</span></span>|<span data-ttu-id="1029f-300">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-300">Description</span></span>|<span data-ttu-id="1029f-301">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-302">mock-válasz</span><span class="sxs-lookup"><span data-stu-id="1029f-302">mock-response</span></span>|<span data-ttu-id="1029f-303">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-303">Root element.</span></span>|<span data-ttu-id="1029f-304">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-305">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-305">Attributes</span></span>  
  
|<span data-ttu-id="1029f-306">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-306">Attribute</span></span>|<span data-ttu-id="1029f-307">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-307">Description</span></span>|<span data-ttu-id="1029f-308">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-308">Required</span></span>|<span data-ttu-id="1029f-309">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="1029f-310">állapotkód-:</span><span class="sxs-lookup"><span data-stu-id="1029f-310">status-code</span></span>|<span data-ttu-id="1029f-311">Adja meg a válasz állapotkódja és használt tooselect megfelelő példa vagy séma.</span><span class="sxs-lookup"><span data-stu-id="1029f-311">Specifies response status code and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="1029f-312">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-312">No</span></span>|<span data-ttu-id="1029f-313">200</span><span class="sxs-lookup"><span data-stu-id="1029f-313">200</span></span>|  
|<span data-ttu-id="1029f-314">tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="1029f-314">content-type</span></span>|<span data-ttu-id="1029f-315">Itt adhatja meg `Content-Type` válasz állomásfejléc-érték és a használt tooselect megfelelő példa vagy séma van.</span><span class="sxs-lookup"><span data-stu-id="1029f-315">Specifies `Content-Type` response header value and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="1029f-316">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-316">No</span></span>|<span data-ttu-id="1029f-317">None</span><span class="sxs-lookup"><span data-stu-id="1029f-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-318">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-318">Usage</span></span>  
 <span data-ttu-id="1029f-319">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-319">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-320">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="1029f-321">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="1029f-322"><a name="Retry"></a>Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="1029f-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="1029f-323">Hello `retry` házirend egyszer végrehajtja az alárendelt házirendeket, és majd újrapróbálkozik a végrehajtási, amíg hello újrapróbálkozási `condition` válik `false` , vagy próbálkozzon újra `count` kimerült.</span><span class="sxs-lookup"><span data-stu-id="1029f-323">hello             `retry` policy executes its child policies once and then retries their execution until hello retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-324">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-324">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="1029f-325">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-325">Example</span></span>  
 <span data-ttu-id="1029f-326">A hello a következő példa kérelem forewarding a rendszer ismét megkísérli tooten időpontokban exponenciális újrapróbálkozási algoritmust használja fel.</span><span class="sxs-lookup"><span data-stu-id="1029f-326">In hello following example request forewarding is retried up tooten times using exponential retry algorithm.</span></span> <span data-ttu-id="1029f-327">Mivel a `first-fast-retry` értéke toofalse, minden újrapróbálkozások tulajdonos toohello exponsntial újrapróbálkozási algoritmust.</span><span class="sxs-lookup"><span data-stu-id="1029f-327">Since                    `first-fast-retry` is set toofalse, all retry attempts are subject toohello exponsntial retry algorithm.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-328">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-328">Elements</span></span>  
  
|<span data-ttu-id="1029f-329">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-329">Element</span></span>|<span data-ttu-id="1029f-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-330">Description</span></span>|<span data-ttu-id="1029f-331">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-332">retry</span><span class="sxs-lookup"><span data-stu-id="1029f-332">retry</span></span>|<span data-ttu-id="1029f-333">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-333">Root element.</span></span> <span data-ttu-id="1029f-334">Előfordulhat, hogy bármely egyéb házirendek gyermekelemeinek tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="1029f-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="1029f-335">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-336">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-336">Attributes</span></span>  
  
|<span data-ttu-id="1029f-337">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-337">Attribute</span></span>|<span data-ttu-id="1029f-338">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-338">Description</span></span>|<span data-ttu-id="1029f-339">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-339">Required</span></span>|<span data-ttu-id="1029f-340">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-341">Az állapot</span><span class="sxs-lookup"><span data-stu-id="1029f-341">condition</span></span>|<span data-ttu-id="1029f-342">Logikai szövegkonstans vagy [kifejezés](api-management-policy-expressions.md) adja meg, ha az újrapróbálkozások le kell állítani (`false`) vagy továbbra is (`true`).</span><span class="sxs-lookup"><span data-stu-id="1029f-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="1029f-343">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-343">Yes</span></span>|<span data-ttu-id="1029f-344">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-344">N/A</span></span>|  
|<span data-ttu-id="1029f-345">Száma</span><span class="sxs-lookup"><span data-stu-id="1029f-345">count</span></span>|<span data-ttu-id="1029f-346">Hello tooattempt az újrapróbálkozások maximális számát megadó pozitív szám.</span><span class="sxs-lookup"><span data-stu-id="1029f-346">A positive number specifying hello maximum number of retries tooattempt.</span></span>|<span data-ttu-id="1029f-347">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-347">Yes</span></span>|<span data-ttu-id="1029f-348">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-348">N/A</span></span>|  
|<span data-ttu-id="1029f-349">interval</span><span class="sxs-lookup"><span data-stu-id="1029f-349">interval</span></span>|<span data-ttu-id="1029f-350">Egy pozitív szám hello újrapróbálkozások között hello időköz megadása másodpercben.</span><span class="sxs-lookup"><span data-stu-id="1029f-350">A positive number in seconds specifying hello wait interval between hello retry attempts.</span></span>|<span data-ttu-id="1029f-351">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-351">Yes</span></span>|<span data-ttu-id="1029f-352">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-352">N/A</span></span>|  
|<span data-ttu-id="1029f-353">maximális-időköz</span><span class="sxs-lookup"><span data-stu-id="1029f-353">max-interval</span></span>|<span data-ttu-id="1029f-354">Egy pozitív szám hello maximális időtartamot hello újrapróbálkozások között megadása másodpercben.</span><span class="sxs-lookup"><span data-stu-id="1029f-354">A positive number in seconds specifying hello maximum wait interval between hello retry attempts.</span></span> <span data-ttu-id="1029f-355">Használt tooimplement exponenciális újrapróbálkozási algoritmust is.</span><span class="sxs-lookup"><span data-stu-id="1029f-355">It is used tooimplement an exponential retry algorithm.</span></span>|<span data-ttu-id="1029f-356">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-356">No</span></span>|<span data-ttu-id="1029f-357">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-357">N/A</span></span>|  
|<span data-ttu-id="1029f-358">különbözeti</span><span class="sxs-lookup"><span data-stu-id="1029f-358">delta</span></span>|<span data-ttu-id="1029f-359">Egy pozitív szám hello várakozási időközt növekmény megadása másodpercben.</span><span class="sxs-lookup"><span data-stu-id="1029f-359">A positive number in seconds specifying hello wait interval increment.</span></span> <span data-ttu-id="1029f-360">Használt tooimplement hello lineáris és exponenciális újrapróbálkozási algoritmust is.</span><span class="sxs-lookup"><span data-stu-id="1029f-360">It is used tooimplement hello linear and exponential retry algorithms.</span></span>|<span data-ttu-id="1029f-361">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-361">No</span></span>|<span data-ttu-id="1029f-362">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-362">N/A</span></span>|  
|<span data-ttu-id="1029f-363">első fast-újrapróbálkozási</span><span class="sxs-lookup"><span data-stu-id="1029f-363">first-fast-retry</span></span>|<span data-ttu-id="1029f-364">Ha túl beállítása `true` , hello első újrapróbálkozások azonnal megtörténik.</span><span class="sxs-lookup"><span data-stu-id="1029f-364">If set too                                   `true` , hello first retry attempt is performed immediately.</span></span>|<span data-ttu-id="1029f-365">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="1029f-366">Ha csak hello `interval` meg van adva, **rögzített** időköz újrapróbálkozások hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1029f-366">When only hello `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="1029f-367">Ha csak hello `interval` és `delta` vannak megadva, a **lineáris** időköz újrapróbálkozási algoritmust használnak, ahol várakozási idő között a számított függően hello a következő képlet - `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="1029f-367">When only hello `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according hello following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="1029f-368">Ha hello `interval`, `max-interval` és `delta` vannak megadva, **exponenciális** időköz újrapróbálkozási algoritmust alkalmazza a rendszer, ahol hello várakozási idő hello újrapróbálkozások között inkább exponenciálisan helloértékét`interval`toohello érték `max-interval` toohello következő megfelelően forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="1029f-368">When hello `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where hello wait time between hello retries is growing exponentially from hello value of `interval` toohello value `max-interval` according toohello following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="1029f-369">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-369">Usage</span></span>  
 <span data-ttu-id="1029f-370">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="1029f-370">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="1029f-371">Vegye figyelembe, hogy ez a házirend öröklik a gyermek házirend használattal kapcsolatos korlátozásokat.</span><span class="sxs-lookup"><span data-stu-id="1029f-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="1029f-372">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-373">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-374"><a name="ReturnResponse"></a>Válasz visszaadása</span><span class="sxs-lookup"><span data-stu-id="1029f-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="1029f-375">Hello `return-response` házirend feldolgozási sor végrehajtása megszakítása és egy alapértelmezett vagy egyéni válasz toohello hívó adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1029f-375">hello `return-response` policy aborts pipeline execution and returns either a default or custom response toohello caller.</span></span> <span data-ttu-id="1029f-376">Alapértelmezett válasz `200 OK` nem szervezethez.</span><span class="sxs-lookup"><span data-stu-id="1029f-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="1029f-377">Egyéni válasz egy környezeti változó vagy házirend utasítások keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="1029f-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="1029f-378">Mindkét kapnak, amikor hello válasz hello környezeti változó belül található módosul a hello házirend-utasításoknál előtt toohello hívó ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1029f-378">When both are provided, hello response contained within hello context variable is modified by hello policy statements before being returned toohello caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-379">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-380">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="1029f-381">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-381">Elements</span></span>  
  
|<span data-ttu-id="1029f-382">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-382">Element</span></span>|<span data-ttu-id="1029f-383">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-383">Description</span></span>|<span data-ttu-id="1029f-384">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-385">visszatérési-válasz</span><span class="sxs-lookup"><span data-stu-id="1029f-385">return-response</span></span>|<span data-ttu-id="1029f-386">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-386">Root element.</span></span>|<span data-ttu-id="1029f-387">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-387">Yes</span></span>|  
|<span data-ttu-id="1029f-388">set-fejléc</span><span class="sxs-lookup"><span data-stu-id="1029f-388">set-header</span></span>|<span data-ttu-id="1029f-389">A [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="1029f-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="1029f-390">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-390">No</span></span>|  
|<span data-ttu-id="1029f-391">set-szervezet</span><span class="sxs-lookup"><span data-stu-id="1029f-391">set-body</span></span>|<span data-ttu-id="1029f-392">A [set-törzsének](api-management-transformation-policies.md#SetBody) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="1029f-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="1029f-393">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-393">No</span></span>|  
|<span data-ttu-id="1029f-394">állapot beállítása</span><span class="sxs-lookup"><span data-stu-id="1029f-394">set-status</span></span>|<span data-ttu-id="1029f-395">A [állapotának beállítása](api-management-advanced-policies.md#SetStatus) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="1029f-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="1029f-396">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-397">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-397">Attributes</span></span>  
  
|<span data-ttu-id="1029f-398">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-398">Attribute</span></span>|<span data-ttu-id="1029f-399">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-399">Description</span></span>|<span data-ttu-id="1029f-400">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="1029f-401">válasz-változó-neve</span><span class="sxs-lookup"><span data-stu-id="1029f-401">response-variable-name</span></span>|<span data-ttu-id="1029f-402">hello hello környezeti változó neve hivatkozni, például felsőbb rétegbeli [küldési-kérelmek](api-management-advanced-policies.md#SendRequest) házirend, és úgy, hogy egy `Response` objektum</span><span class="sxs-lookup"><span data-stu-id="1029f-402">hello name of hello context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="1029f-403">Választható.</span><span class="sxs-lookup"><span data-stu-id="1029f-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-404">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-404">Usage</span></span>  
 <span data-ttu-id="1029f-405">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-405">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-406">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-407">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-408"><a name="SendOneWayRequest"></a>Egyirányú kérés küldése</span><span class="sxs-lookup"><span data-stu-id="1029f-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="1029f-409">Hello `send-one-way-request` házirend toohello megadott URL-címet a válaszra való várakozás nélkül megadott hello kérést küld.</span><span class="sxs-lookup"><span data-stu-id="1029f-409">hello `send-one-way-request` policy sends hello provided request toohello specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-410">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-411">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-411">Example</span></span>  
 <span data-ttu-id="1029f-412">Ez a minta-házirend használatával hello példáját mutatja be `send-one-way-request` házirend toosend egy üzenet tooa Slack Csevegés hely hello HTTP-válaszkód nagyobb vagy egyenlő too500 esetén.</span><span class="sxs-lookup"><span data-stu-id="1029f-412">This sample policy shows an example of using hello `send-one-way-request` policy toosend a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="1029f-413">Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="1029f-413">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-414">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-414">Elements</span></span>  
  
|<span data-ttu-id="1029f-415">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-415">Element</span></span>|<span data-ttu-id="1029f-416">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-416">Description</span></span>|<span data-ttu-id="1029f-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-418">küldési-egy-módon-kérelmek</span><span class="sxs-lookup"><span data-stu-id="1029f-418">send-one-way-request</span></span>|<span data-ttu-id="1029f-419">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-419">Root element.</span></span>|<span data-ttu-id="1029f-420">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-420">Yes</span></span>|  
|<span data-ttu-id="1029f-421">URL-címe</span><span class="sxs-lookup"><span data-stu-id="1029f-421">url</span></span>|<span data-ttu-id="1029f-422">hello hello kérelem URL-címe</span><span class="sxs-lookup"><span data-stu-id="1029f-422">hello URL of hello request.</span></span>|<span data-ttu-id="1029f-423">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="1029f-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="1029f-424">Módszer</span><span class="sxs-lookup"><span data-stu-id="1029f-424">method</span></span>|<span data-ttu-id="1029f-425">hello hello kérelem HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="1029f-425">hello HTTP method for hello request.</span></span>|<span data-ttu-id="1029f-426">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="1029f-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="1029f-427">header</span><span class="sxs-lookup"><span data-stu-id="1029f-427">header</span></span>|<span data-ttu-id="1029f-428">Kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="1029f-428">Request header.</span></span> <span data-ttu-id="1029f-429">Használjon több több kérelem fejlécek.</span><span class="sxs-lookup"><span data-stu-id="1029f-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="1029f-430">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-430">No</span></span>|  
|<span data-ttu-id="1029f-431">Törzs</span><span class="sxs-lookup"><span data-stu-id="1029f-431">body</span></span>|<span data-ttu-id="1029f-432">hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="1029f-432">hello request body.</span></span>|<span data-ttu-id="1029f-433">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-434">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-434">Attributes</span></span>  
  
|<span data-ttu-id="1029f-435">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-435">Attribute</span></span>|<span data-ttu-id="1029f-436">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-436">Description</span></span>|<span data-ttu-id="1029f-437">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-437">Required</span></span>|<span data-ttu-id="1029f-438">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-439">mód = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-439">mode="string"</span></span>|<span data-ttu-id="1029f-440">Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem hello.</span><span class="sxs-lookup"><span data-stu-id="1029f-440">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="1029f-441">Kimenő módban mód = másolása nem sikerült hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="1029f-441">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="1029f-442">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-442">No</span></span>|<span data-ttu-id="1029f-443">új</span><span class="sxs-lookup"><span data-stu-id="1029f-443">New</span></span>|  
|<span data-ttu-id="1029f-444">név</span><span class="sxs-lookup"><span data-stu-id="1029f-444">name</span></span>|<span data-ttu-id="1029f-445">Hello fejléc toobe set hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1029f-445">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="1029f-446">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-446">Yes</span></span>|<span data-ttu-id="1029f-447">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-447">N/A</span></span>|  
|<span data-ttu-id="1029f-448">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="1029f-448">exists-action</span></span>|<span data-ttu-id="1029f-449">Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="1029f-449">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="1029f-450">Ez az attribútum hello a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1029f-450">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="1029f-451">-felülbírálás - cserél hello értéke hello meglévő fejléc.</span><span class="sxs-lookup"><span data-stu-id="1029f-451">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="1029f-452">-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-452">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="1029f-453">-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-453">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="1029f-454">-törlés – hello kérelemből eltávolítja hello fejlécet.</span><span class="sxs-lookup"><span data-stu-id="1029f-454">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="1029f-455">Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="1029f-455">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="1029f-456">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-456">No</span></span>|<span data-ttu-id="1029f-457">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="1029f-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-458">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-458">Usage</span></span>  
 <span data-ttu-id="1029f-459">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-459">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-460">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-461">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-462"><a name="SendRequest"></a>Kérés küldése</span><span class="sxs-lookup"><span data-stu-id="1029f-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="1029f-463">Hello `send-request` házirend toohello megadott URL-címe, mint hello beállítása időtúllépés értéke már nem vár a megadott hello kérést küld.</span><span class="sxs-lookup"><span data-stu-id="1029f-463">hello `send-request` policy sends hello provided request toohello specified URL, waiting no longer than hello set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-464">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-465">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-465">Example</span></span>  
 <span data-ttu-id="1029f-466">Ez a példa bemutatja egyirányú tooverify hivatkozás-jogkivonat az engedélyezési-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="1029f-466">This example shows one way tooverify a reference token with an authorization server.</span></span> <span data-ttu-id="1029f-467">Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="1029f-467">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-468">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-468">Elements</span></span>  
  
|<span data-ttu-id="1029f-469">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-469">Element</span></span>|<span data-ttu-id="1029f-470">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-470">Description</span></span>|<span data-ttu-id="1029f-471">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-472">küldési-kérelmek</span><span class="sxs-lookup"><span data-stu-id="1029f-472">send-request</span></span>|<span data-ttu-id="1029f-473">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-473">Root element.</span></span>|<span data-ttu-id="1029f-474">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-474">Yes</span></span>|  
|<span data-ttu-id="1029f-475">URL-címe</span><span class="sxs-lookup"><span data-stu-id="1029f-475">url</span></span>|<span data-ttu-id="1029f-476">hello hello kérelem URL-címe</span><span class="sxs-lookup"><span data-stu-id="1029f-476">hello URL of hello request.</span></span>|<span data-ttu-id="1029f-477">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="1029f-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="1029f-478">Módszer</span><span class="sxs-lookup"><span data-stu-id="1029f-478">method</span></span>|<span data-ttu-id="1029f-479">hello hello kérelem HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="1029f-479">hello HTTP method for hello request.</span></span>|<span data-ttu-id="1029f-480">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="1029f-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="1029f-481">header</span><span class="sxs-lookup"><span data-stu-id="1029f-481">header</span></span>|<span data-ttu-id="1029f-482">Kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="1029f-482">Request header.</span></span> <span data-ttu-id="1029f-483">Használjon több több kérelem fejlécek.</span><span class="sxs-lookup"><span data-stu-id="1029f-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="1029f-484">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-484">No</span></span>|  
|<span data-ttu-id="1029f-485">Törzs</span><span class="sxs-lookup"><span data-stu-id="1029f-485">body</span></span>|<span data-ttu-id="1029f-486">hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="1029f-486">hello request body.</span></span>|<span data-ttu-id="1029f-487">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-488">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-488">Attributes</span></span>  
  
|<span data-ttu-id="1029f-489">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-489">Attribute</span></span>|<span data-ttu-id="1029f-490">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-490">Description</span></span>|<span data-ttu-id="1029f-491">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-491">Required</span></span>|<span data-ttu-id="1029f-492">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-493">mód = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-493">mode="string"</span></span>|<span data-ttu-id="1029f-494">Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem hello.</span><span class="sxs-lookup"><span data-stu-id="1029f-494">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="1029f-495">Kimenő módban mód = másolása nem sikerült hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="1029f-495">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="1029f-496">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-496">No</span></span>|<span data-ttu-id="1029f-497">új</span><span class="sxs-lookup"><span data-stu-id="1029f-497">New</span></span>|  
|<span data-ttu-id="1029f-498">válasz-változó-name = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-498">response-variable-name="string"</span></span>|<span data-ttu-id="1029f-499">Ha nincs jelen, `context.Response` szolgál.</span><span class="sxs-lookup"><span data-stu-id="1029f-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="1029f-500">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-500">No</span></span>|<span data-ttu-id="1029f-501">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-501">N/A</span></span>|  
|<span data-ttu-id="1029f-502">Időtúllépés = "integer"</span><span class="sxs-lookup"><span data-stu-id="1029f-502">timeout="integer"</span></span>|<span data-ttu-id="1029f-503">hello időkorlátja másodpercben hello hívása előtt toohello URL-cím nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="1029f-503">hello timeout interval in seconds before hello call toohello URL fails.</span></span>|<span data-ttu-id="1029f-504">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-504">No</span></span>|<span data-ttu-id="1029f-505">60</span><span class="sxs-lookup"><span data-stu-id="1029f-505">60</span></span>|  
|<span data-ttu-id="1029f-506">Hiba mellőzése</span><span class="sxs-lookup"><span data-stu-id="1029f-506">ignore-error</span></span>|<span data-ttu-id="1029f-507">Ha igaz, és hello kérik hibát eredményez:</span><span class="sxs-lookup"><span data-stu-id="1029f-507">If true and hello request results in an error:</span></span><br /><br /> <span data-ttu-id="1029f-508">-Ha a válasz-változó-név lett megadva, null értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1029f-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="1029f-509">-Ha a válasz-változó-neve nincs megadva, a környezetben. Kérés nem fog frissülni.</span><span class="sxs-lookup"><span data-stu-id="1029f-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="1029f-510">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-510">No</span></span>|<span data-ttu-id="1029f-511">hamis</span><span class="sxs-lookup"><span data-stu-id="1029f-511">false</span></span>|  
|<span data-ttu-id="1029f-512">név</span><span class="sxs-lookup"><span data-stu-id="1029f-512">name</span></span>|<span data-ttu-id="1029f-513">Hello fejléc toobe set hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1029f-513">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="1029f-514">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-514">Yes</span></span>|<span data-ttu-id="1029f-515">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-515">N/A</span></span>|  
|<span data-ttu-id="1029f-516">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="1029f-516">exists-action</span></span>|<span data-ttu-id="1029f-517">Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="1029f-517">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="1029f-518">Ez az attribútum hello a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1029f-518">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="1029f-519">-felülbírálás - cserél hello értéke hello meglévő fejléc.</span><span class="sxs-lookup"><span data-stu-id="1029f-519">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="1029f-520">-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-520">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="1029f-521">-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-521">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="1029f-522">-törlés – hello kérelemből eltávolítja hello fejlécet.</span><span class="sxs-lookup"><span data-stu-id="1029f-522">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="1029f-523">Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="1029f-523">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="1029f-524">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-524">No</span></span>|<span data-ttu-id="1029f-525">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="1029f-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-526">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-526">Usage</span></span>  
 <span data-ttu-id="1029f-527">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-527">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-528">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-529">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-530"><a name="SetHttpProxy"></a>HTTP-proxy beállítása</span><span class="sxs-lookup"><span data-stu-id="1029f-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="1029f-531">Hello `proxy` házirendje lehetővé teszi a tooroute továbbított kérelmek toobackends egy HTTP-proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="1029f-531">hello `proxy` policy allows you tooroute requests forwarded toobackends via an HTTP proxy.</span></span> <span data-ttu-id="1029f-532">Csak a HTTP (nem a HTTPS-) átjáró hello és hello proxy között támogatott.</span><span class="sxs-lookup"><span data-stu-id="1029f-532">Only HTTP (not HTTPS) is supported between hello gateway and hello proxy.</span></span> <span data-ttu-id="1029f-533">Alapszintű, és csak az NTLM-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="1029f-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-534">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-535">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-535">Example</span></span>  
<span data-ttu-id="1029f-536">Vegye figyelembe a hello használata [tulajdonságok](api-management-howto-properties.md) a bizalmas adatok tárolása hello házirend-dokumentum hello felhasználónév és jelszó tooavoid értékként.</span><span class="sxs-lookup"><span data-stu-id="1029f-536">Note hello use of [properties](api-management-howto-properties.md) as values of hello username and password tooavoid storing sensitive information in hello policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="1029f-537">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-537">Elements</span></span>  
  
|<span data-ttu-id="1029f-538">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-538">Element</span></span>|<span data-ttu-id="1029f-539">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-539">Description</span></span>|<span data-ttu-id="1029f-540">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-541">Proxy</span><span class="sxs-lookup"><span data-stu-id="1029f-541">proxy</span></span>|<span data-ttu-id="1029f-542">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="1029f-542">Root element</span></span>|<span data-ttu-id="1029f-543">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="1029f-544">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-544">Attributes</span></span>  
  
|<span data-ttu-id="1029f-545">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-545">Attribute</span></span>|<span data-ttu-id="1029f-546">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-546">Description</span></span>|<span data-ttu-id="1029f-547">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-547">Required</span></span>|<span data-ttu-id="1029f-548">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-549">URL-cím = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-549">url="string"</span></span>|<span data-ttu-id="1029f-550">Proxykiszolgáló URL-címet http://host:port hello formája.</span><span class="sxs-lookup"><span data-stu-id="1029f-550">Proxy URL in hello form of http://host:port.</span></span>|<span data-ttu-id="1029f-551">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-551">Yes</span></span>|<span data-ttu-id="1029f-552">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-552">N/A</span></span>|  
|<span data-ttu-id="1029f-553">felhasználónév = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-553">username="string"</span></span>|<span data-ttu-id="1029f-554">A hello proxy-hitelesítéshez használt felhasználónevet toobe.</span><span class="sxs-lookup"><span data-stu-id="1029f-554">Username toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="1029f-555">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-555">No</span></span>|<span data-ttu-id="1029f-556">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-556">N/A</span></span>|  
|<span data-ttu-id="1029f-557">jelszó = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-557">password="string"</span></span>|<span data-ttu-id="1029f-558">A hello proxy-hitelesítéshez használt jelszó toobe.</span><span class="sxs-lookup"><span data-stu-id="1029f-558">Password toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="1029f-559">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-559">No</span></span>|<span data-ttu-id="1029f-560">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="1029f-561">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-561">Usage</span></span>  
 <span data-ttu-id="1029f-562">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-562">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-563">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="1029f-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="1029f-564">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="1029f-565"><a name="SetRequestMethod"></a>Set kérés metódus</span><span class="sxs-lookup"><span data-stu-id="1029f-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="1029f-566">Hello `set-method` házirendje lehetővé teszi a toochange hello HTTP-kérési metódust egy kérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-566">hello `set-method` policy allows you toochange hello HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-567">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-568">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-568">Example</span></span>  
 <span data-ttu-id="1029f-569">Ez a minta házirend hello használó `set-method` házirend egy üzenet tooa Slack Csevegés hely küldése, ha HTTP-válaszkód hello érték nagyobb, mint vagy egyenlő too500 példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="1029f-569">This sample policy that uses hello `set-method` policy shows an example of sending a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="1029f-570">Ez a minta további információkért lásd: [hello Azure API Management szolgáltatás a külső szolgáltatásokkal](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="1029f-570">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-571">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-571">Elements</span></span>  
  
|<span data-ttu-id="1029f-572">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-572">Element</span></span>|<span data-ttu-id="1029f-573">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-573">Description</span></span>|<span data-ttu-id="1029f-574">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-575">set-módszer</span><span class="sxs-lookup"><span data-stu-id="1029f-575">set-method</span></span>|<span data-ttu-id="1029f-576">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-576">Root element.</span></span> <span data-ttu-id="1029f-577">hello elem hello értéke határozza meg a hello HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="1029f-577">hello value of hello element specifies hello HTTP method.</span></span>|<span data-ttu-id="1029f-578">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-579">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-579">Usage</span></span>  
 <span data-ttu-id="1029f-580">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-580">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-581">**Házirend szakaszok:** bejövő, hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="1029f-582">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-583"><a name="SetStatus"></a>Set-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="1029f-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="1029f-584">Hello `set-status` házirend beállítása hello HTTP állapot kód toohello megadott érték.</span><span class="sxs-lookup"><span data-stu-id="1029f-584">hello `set-status` policy sets hello HTTP status code toohello specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-585">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-586">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-586">Example</span></span>  
 <span data-ttu-id="1029f-587">A példa bemutatja, hogyan tooreturn 401-es választ, ha hello engedélyezési jogkivonat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="1029f-587">This example shows how tooreturn a 401 response if hello authorization token is invalid.</span></span> <span data-ttu-id="1029f-588">További információkért lásd: [hello Azure API Management szolgáltatás a külső services használatával](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="1029f-588">For more information, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-589">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-589">Elements</span></span>  
  
|<span data-ttu-id="1029f-590">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-590">Element</span></span>|<span data-ttu-id="1029f-591">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-591">Description</span></span>|<span data-ttu-id="1029f-592">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-593">állapot beállítása</span><span class="sxs-lookup"><span data-stu-id="1029f-593">set-status</span></span>|<span data-ttu-id="1029f-594">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-594">Root element.</span></span>|<span data-ttu-id="1029f-595">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-596">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-596">Attributes</span></span>  
  
|<span data-ttu-id="1029f-597">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-597">Attribute</span></span>|<span data-ttu-id="1029f-598">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-598">Description</span></span>|<span data-ttu-id="1029f-599">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-599">Required</span></span>|<span data-ttu-id="1029f-600">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-601">kód = "integer"</span><span class="sxs-lookup"><span data-stu-id="1029f-601">code="integer"</span></span>|<span data-ttu-id="1029f-602">hello HTTP-állapot kódját tooreturn.</span><span class="sxs-lookup"><span data-stu-id="1029f-602">hello HTTP status code tooreturn.</span></span>|<span data-ttu-id="1029f-603">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-603">Yes</span></span>|<span data-ttu-id="1029f-604">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-604">N/A</span></span>|  
|<span data-ttu-id="1029f-605">oka = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="1029f-605">reason="string"</span></span>|<span data-ttu-id="1029f-606">A leírás hello hello állapotkód visszaküldésére használatos.</span><span class="sxs-lookup"><span data-stu-id="1029f-606">A description of hello reason for returning hello status code.</span></span>|<span data-ttu-id="1029f-607">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-607">Yes</span></span>|<span data-ttu-id="1029f-608">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-609">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-609">Usage</span></span>  
 <span data-ttu-id="1029f-610">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-610">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-611">**Házirend szakaszok:** kimenő, háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-612">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="1029f-613"><a name="set-variable"></a>Új érték</span><span class="sxs-lookup"><span data-stu-id="1029f-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="1029f-614">Hello `set-variable` házirend deklarál egy [környezetben](api-management-policy-expressions.md#ContextVariables) változó, és egy megadott értéket rendeli az [kifejezés](api-management-policy-expressions.md) vagy egy szöveges karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1029f-614">hello `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="1029f-615">Ha hello kifejezés tartalmaz lesz konvertálva szövegkonstans tooa karakterlánc és hello: hello érték lesz `System.String`.</span><span class="sxs-lookup"><span data-stu-id="1029f-615">if hello expression contains a literal it will be converted tooa string and hello type of hello value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="1029f-616"><a name="set-variablePolicyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="1029f-617"><a name="set-variableExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="1029f-618">hello következő példa bemutatja a változó szabály beállítása a hello bejövő szakasz.</span><span class="sxs-lookup"><span data-stu-id="1029f-618">hello following example demonstrates a set variable policy in hello inbound section.</span></span> <span data-ttu-id="1029f-619">A set változó házirend létrehoz egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) tootrue van beállítva, ha hello változó `User-Agent` kérelem fejléc hello szöveget tartalmaz `iPad` vagy `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="1029f-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="1029f-620">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-620">Elements</span></span>  
  
|<span data-ttu-id="1029f-621">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-621">Element</span></span>|<span data-ttu-id="1029f-622">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-622">Description</span></span>|<span data-ttu-id="1029f-623">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-624">Set-változó</span><span class="sxs-lookup"><span data-stu-id="1029f-624">set-variable</span></span>|<span data-ttu-id="1029f-625">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-625">Root element.</span></span>|<span data-ttu-id="1029f-626">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-627">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-627">Attributes</span></span>  
  
|<span data-ttu-id="1029f-628">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-628">Attribute</span></span>|<span data-ttu-id="1029f-629">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-629">Description</span></span>|<span data-ttu-id="1029f-630">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="1029f-631">név</span><span class="sxs-lookup"><span data-stu-id="1029f-631">name</span></span>|<span data-ttu-id="1029f-632">hello hello változó neve.</span><span class="sxs-lookup"><span data-stu-id="1029f-632">hello name of hello variable.</span></span>|<span data-ttu-id="1029f-633">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-633">Yes</span></span>|  
|<span data-ttu-id="1029f-634">érték</span><span class="sxs-lookup"><span data-stu-id="1029f-634">value</span></span>|<span data-ttu-id="1029f-635">hello hello változó értékét.</span><span class="sxs-lookup"><span data-stu-id="1029f-635">hello value of hello variable.</span></span> <span data-ttu-id="1029f-636">Ez lehet egy kifejezést, vagy konstansérték.</span><span class="sxs-lookup"><span data-stu-id="1029f-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="1029f-637">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-638">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-638">Usage</span></span>  
 <span data-ttu-id="1029f-639">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-639">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-640">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-641">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="1029f-642"><a name="set-variableAllowedTypes"></a>Engedélyezett típusokkal</span><span class="sxs-lookup"><span data-stu-id="1029f-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="1029f-643">Hello használt kifejezések `set-variable` házirendet a következő típus hello valamelyikét kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="1029f-643">Expressions used in hello `set-variable` policy must return one of hello following basic types.</span></span>  
  
-   <span data-ttu-id="1029f-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="1029f-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="1029f-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="1029f-645">System.SByte</span></span>  
  
-   <span data-ttu-id="1029f-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="1029f-646">System.Byte</span></span>  
  
-   <span data-ttu-id="1029f-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="1029f-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="1029f-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="1029f-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="1029f-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="1029f-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="1029f-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="1029f-650">System.Int16</span></span>  
  
-   <span data-ttu-id="1029f-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="1029f-651">System.Int32</span></span>  
  
-   <span data-ttu-id="1029f-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="1029f-652">System.Int64</span></span>  
  
-   <span data-ttu-id="1029f-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="1029f-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="1029f-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="1029f-654">System.Single</span></span>  
  
-   <span data-ttu-id="1029f-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="1029f-655">System.Double</span></span>  
  
-   <span data-ttu-id="1029f-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="1029f-656">System.Guid</span></span>  
  
-   <span data-ttu-id="1029f-657">System.String</span><span class="sxs-lookup"><span data-stu-id="1029f-657">System.String</span></span>  
  
-   <span data-ttu-id="1029f-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="1029f-658">System.Char</span></span>  
  
-   <span data-ttu-id="1029f-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="1029f-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="1029f-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="1029f-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="1029f-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="1029f-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="1029f-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="1029f-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="1029f-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="1029f-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="1029f-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="1029f-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="1029f-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="1029f-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="1029f-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="1029f-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="1029f-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="1029f-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="1029f-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="1029f-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="1029f-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="1029f-669">System.Single?</span></span>  
  
-   <span data-ttu-id="1029f-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="1029f-670">System.Double?</span></span>  
  
-   <span data-ttu-id="1029f-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="1029f-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="1029f-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="1029f-672">System.String?</span></span>  
  
-   <span data-ttu-id="1029f-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="1029f-673">System.Char?</span></span>  
  
-   <span data-ttu-id="1029f-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="1029f-674">System.DateTime?</span></span>  

##  <span data-ttu-id="1029f-675"><a name="Trace"></a>Nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="1029f-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="1029f-676">Hello `trace` házirend hozzáadja egy karakterlánc hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="1029f-676">hello             `trace` policy adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="1029f-677">hello házirend sorozatot hajthat végre, csak ha nyomkövetés akkor váltódik ki, azaz `Ocp-Apim-Trace` fejléc jelen, és a beállított túl`true` és `Ocp-Apim-Subscription-Key` fejléc szerepel, valamint hello rendszergazdai fiókhoz társított érvényes kulcs.</span><span class="sxs-lookup"><span data-stu-id="1029f-677">hello policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set too`true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with hello admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-678">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="1029f-679">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-679">Elements</span></span>  
  
|<span data-ttu-id="1029f-680">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-680">Element</span></span>|<span data-ttu-id="1029f-681">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-681">Description</span></span>|<span data-ttu-id="1029f-682">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-683">Nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="1029f-683">trace</span></span>|<span data-ttu-id="1029f-684">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-684">Root element.</span></span>|<span data-ttu-id="1029f-685">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-686">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-686">Attributes</span></span>  
  
|<span data-ttu-id="1029f-687">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-687">Attribute</span></span>|<span data-ttu-id="1029f-688">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-688">Description</span></span>|<span data-ttu-id="1029f-689">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-689">Required</span></span>|<span data-ttu-id="1029f-690">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-691">Forrás</span><span class="sxs-lookup"><span data-stu-id="1029f-691">source</span></span>|<span data-ttu-id="1029f-692">Karakterlánc literális jelentéssel bíró toohello trace viewer és üdvözlőüzenetére megadását hello forrását.</span><span class="sxs-lookup"><span data-stu-id="1029f-692">String literal meaningful toohello trace viewer and specifying hello source of hello message.</span></span>|<span data-ttu-id="1029f-693">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-693">Yes</span></span>|<span data-ttu-id="1029f-694">N/A</span><span class="sxs-lookup"><span data-stu-id="1029f-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-695">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-695">Usage</span></span>  
 <span data-ttu-id="1029f-696">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="1029f-696">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="1029f-697">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="1029f-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="1029f-698">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="1029f-699"><a name="Wait"></a>várj</span><span class="sxs-lookup"><span data-stu-id="1029f-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="1029f-700">Hello `wait` házirend hajt végre annak közvetlenül alárendelt házirendjei párhuzamosan, és az összes vagy egy annak közvetlenül alárendelt házirendek toocomplete vár befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="1029f-700">hello `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies toocomplete before it completes.</span></span> <span data-ttu-id="1029f-701">hello várakozási házirend lehetnek azonnali gyermek házirendjeit [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), és [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek.</span><span class="sxs-lookup"><span data-stu-id="1029f-701">hello wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1029f-702">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1029f-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="1029f-703">Példa</span><span class="sxs-lookup"><span data-stu-id="1029f-703">Example</span></span>  
 <span data-ttu-id="1029f-704">A következő példa hello nincsenek két `choose` közvetlenül alárendelt házirendek a hello házirendekben `wait` házirend.</span><span class="sxs-lookup"><span data-stu-id="1029f-704">In hello following example there are two `choose` policies as immediate child policies of hello `wait` policy.</span></span> <span data-ttu-id="1029f-705">Ezek mindegyikének `choose` házirendek hajt végre párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="1029f-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="1029f-706">Minden egyes `choose` házirend kísérletek tooretrieve gyorsítótárazott értéket.</span><span class="sxs-lookup"><span data-stu-id="1029f-706">Each `choose` policy attempts tooretrieve a cached value.</span></span> <span data-ttu-id="1029f-707">Ha a gyorsítótár-tévesztései, háttérszolgáltatás tooprovide hello érték neve.</span><span class="sxs-lookup"><span data-stu-id="1029f-707">If there is a cache miss, a backend service is called tooprovide hello value.</span></span> <span data-ttu-id="1029f-708">Az ebben a példában hello `wait` házirend végrehajtásához összes közvetlenül alárendelt házirendjeit végrehajtani, mert hello `for` attribútum értéke túl`all`.</span><span class="sxs-lookup"><span data-stu-id="1029f-708">In this example hello `wait` policy does not complete until all of its immediate child policies complete, because hello `for` attribute is set too`all`.</span></span>   <span data-ttu-id="1029f-709">A példa hello környezeti változók (`execute-branch-one`, `value-one`, `execute-branch-two`, és `value-two`) deklarált hello kereteit ezt például a házirendet.</span><span class="sxs-lookup"><span data-stu-id="1029f-709">In this example hello context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of hello scope of this example policy.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="1029f-710">Elemek</span><span class="sxs-lookup"><span data-stu-id="1029f-710">Elements</span></span>  
  
|<span data-ttu-id="1029f-711">Elem</span><span class="sxs-lookup"><span data-stu-id="1029f-711">Element</span></span>|<span data-ttu-id="1029f-712">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-712">Description</span></span>|<span data-ttu-id="1029f-713">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="1029f-714">várj</span><span class="sxs-lookup"><span data-stu-id="1029f-714">wait</span></span>|<span data-ttu-id="1029f-715">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1029f-715">Root element.</span></span> <span data-ttu-id="1029f-716">Tartalmazhat, mint a gyermekelemek csak `send-request`, `cache-lookup-value`, és `choose` házirendek.</span><span class="sxs-lookup"><span data-stu-id="1029f-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="1029f-717">Igen</span><span class="sxs-lookup"><span data-stu-id="1029f-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1029f-718">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1029f-718">Attributes</span></span>  
  
|<span data-ttu-id="1029f-719">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1029f-719">Attribute</span></span>|<span data-ttu-id="1029f-720">Leírás</span><span class="sxs-lookup"><span data-stu-id="1029f-720">Description</span></span>|<span data-ttu-id="1029f-721">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1029f-721">Required</span></span>|<span data-ttu-id="1029f-722">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1029f-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="1029f-723">A</span><span class="sxs-lookup"><span data-stu-id="1029f-723">for</span></span>|<span data-ttu-id="1029f-724">Meghatározza, hogy hello `wait` házirend megvárja-e a minden közvetlenül alárendelt házirendek toobe befejeződött vagy csak egy.</span><span class="sxs-lookup"><span data-stu-id="1029f-724">Determines whether hello `wait` policy waits for all immediate child policies toobe completed or just one.</span></span> <span data-ttu-id="1029f-725">Engedélyezett értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="1029f-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="1029f-726">-   `all`-Várjon, amíg az összes közvetlenül alárendelt házirendek toocomplete</span><span class="sxs-lookup"><span data-stu-id="1029f-726">-   `all` - wait for all immediate child policies toocomplete</span></span><br /><span data-ttu-id="1029f-727">-a - bármely közvetlenül alárendelt házirend toocomplete várja.</span><span class="sxs-lookup"><span data-stu-id="1029f-727">-   any - wait for any immediate child policy toocomplete.</span></span> <span data-ttu-id="1029f-728">Hello első közvetlenül alárendelt házirend befejezése után hello `wait` házirend befejeződött, és bármely más közvetlenül alárendelt házirendek végrehajtása megszakadt.</span><span class="sxs-lookup"><span data-stu-id="1029f-728">Once hello first immediate child policy has completed, hello `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="1029f-729">Nem</span><span class="sxs-lookup"><span data-stu-id="1029f-729">No</span></span>|<span data-ttu-id="1029f-730">Minden</span><span class="sxs-lookup"><span data-stu-id="1029f-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1029f-731">Használat</span><span class="sxs-lookup"><span data-stu-id="1029f-731">Usage</span></span>  
 <span data-ttu-id="1029f-732">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1029f-732">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1029f-733">**Házirend szakaszok:** bejövő, kimenő háttér</span><span class="sxs-lookup"><span data-stu-id="1029f-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="1029f-734">**Házirend hatókörök:**minden hatókör</span><span class="sxs-lookup"><span data-stu-id="1029f-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="1029f-735">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1029f-735">Next steps</span></span>
<span data-ttu-id="1029f-736">Házirendek használata további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="1029f-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="1029f-737">Az API Management házirendek</span><span class="sxs-lookup"><span data-stu-id="1029f-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="1029f-738">Házirend-kifejezések</span><span class="sxs-lookup"><span data-stu-id="1029f-738">Policy expressions</span></span>](api-management-policy-expressions.md)
