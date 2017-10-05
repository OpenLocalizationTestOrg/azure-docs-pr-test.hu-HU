---
title: "Speciális házirendek az Azure API Management |} Microsoft Docs"
description: "További tudnivalók az Azure API Management használható speciális házirendeket."
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
ms.openlocfilehash: 0c65ac74316421a0258f01143baa25ffecb5be3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="api-management-advanced-policies"></a><span data-ttu-id="bd900-103">Házirendek speciális API Management</span><span class="sxs-lookup"><span data-stu-id="bd900-103">API Management advanced policies</span></span>
<span data-ttu-id="bd900-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="bd900-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="bd900-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="bd900-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="bd900-106"><a name="AdvancedPolicies"></a>Speciális házirendek</span><span class="sxs-lookup"><span data-stu-id="bd900-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="bd900-107">[Folyamat szabályozása](api-management-advanced-policies.md#choose) - feltételesen alkalmazza a házirend-utasításoknál logikai értékelése eredményei alapján [kifejezések](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="bd900-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on the results of the evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="bd900-108">[Kérés továbbítása a](#ForwardRequest) -továbbítja a kérést a háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bd900-108">[Forward request](#ForwardRequest) - Forwards the request to the backend service.</span></span>

-   <span data-ttu-id="bd900-109">[Korlátozza a feldolgozási](#LimitConcurrency) -megakadályozza, hogy a házirendek egyszerre legfeljebb a megadott számú kérelem végrehajtása közé.</span><span class="sxs-lookup"><span data-stu-id="bd900-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than the specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="bd900-110">[Az Event Hubs napló](#log-to-eventhub) -üzeneteket küld egy Eseményközpontot, határozzák meg a tranzakciónaplókat tartalmazó entitás a megadott formátumban.</span><span class="sxs-lookup"><span data-stu-id="bd900-110">[Log to Event Hub](#log-to-eventhub) - Sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="bd900-111">[Válasz mock](#mock-response) -megszakításainak-feldolgozási folyamat végrehajtása és közvetlenül a hívó mocked választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bd900-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly to the caller.</span></span>
  
-   <span data-ttu-id="bd900-112">[Próbálja meg újra](#Retry) -újrapróbálkozások zárt házirend utasítás végrehajtása, ha, és amíg a feltétel nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="bd900-112">[Retry](#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="bd900-113">Végrehajtási ismételje meg a megadott időközönként, és a legfeljebb a megadott újrapróbálkozások száma.</span><span class="sxs-lookup"><span data-stu-id="bd900-113">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>  
  
-   <span data-ttu-id="bd900-114">[Térjen vissza a válasz](#ReturnResponse) -megszakításainak-feldolgozási folyamat végrehajtása és a közvetlenül a hívó adott választ.</span><span class="sxs-lookup"><span data-stu-id="bd900-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span> 
  
-   <span data-ttu-id="bd900-115">[Egyirányú kérés küldése](#SendOneWayRequest) -kérést küld a megadott URL-cím a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="bd900-115">[Send one way request](#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="bd900-116">[Kérés küldése](#SendRequest) -kérést küld a megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bd900-116">[Send request](#SendRequest) - Sends a request to the specified URL.</span></span>  

-   <span data-ttu-id="bd900-117">[Állítsa be a HTTP-proxy](#SetHttpProxy) -lehetővé teszi a továbbított kérelmek egy HTTP-proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="bd900-117">[Set HTTP proxy](#SetHttpProxy) - Allows you to route forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="bd900-118">[Kérelem módszert állítja be](#SetRequestMethod) -módosíthatja a kérelem HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="bd900-118">[Set request method](#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="bd900-119">[Állítsa be. állapotkód:](#SetStatus) – a HTTP-állapotkód: a megadott értékre változik.</span><span class="sxs-lookup"><span data-stu-id="bd900-119">[Set status code](#SetStatus) - Changes the HTTP status code to the specified value.</span></span>  
  
-   <span data-ttu-id="bd900-120">[Új érték](api-management-advanced-policies.md#set-variable) -továbbra is fennáll az elnevezett értéket [környezetben](api-management-policy-expressions.md#ContextVariables) változó későbbi eléréshez.</span><span class="sxs-lookup"><span data-stu-id="bd900-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="bd900-121">[Nyomkövetési](#Trace) -be egy karakterláncot ad a [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="bd900-121">[Trace](#Trace) - Adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="bd900-122">[Várjon, amíg](#Wait) -megvárja-e a zárójelek között [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), vagy [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendeket befejeződjön, mielőtt továbblép.</span><span class="sxs-lookup"><span data-stu-id="bd900-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies to complete before proceeding.</span></span>  
  
##  <span data-ttu-id="bd900-123"><a name="choose"></a>Folyamata</span><span class="sxs-lookup"><span data-stu-id="bd900-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="bd900-124">A `choose` házirend utasítások logikai kifejezésen, hasonló az if-majd-más vagy kapcsoló értékelése eredménye alapján a programozási nyelven összeállításához zárt házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="bd900-124">The `choose` policy applies enclosed policy statements based on the outcome of evaluation of Boolean expressions, similar to an if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="bd900-125"><a name="ChoosePolicyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 <span data-ttu-id="bd900-126">A vezérlő adatfolyam házirend tartalmaznia kell legalább egy `<when/>` elemet.</span><span class="sxs-lookup"><span data-stu-id="bd900-126">The control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="bd900-127">A `<otherwise/>` elem nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="bd900-127">The `<otherwise/>` element is optional.</span></span> <span data-ttu-id="bd900-128">A feltételek `<when/>` elemek kiértékelése sorrendben történik, illetve megjelenésük belül a házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-128">Conditions in `<when/>` elements are evaluated in order of their appearance within the policy.</span></span> <span data-ttu-id="bd900-129">Az első határolt házirend nyilatkozatuk `<when/>` feltétel attribútum egyenlő elem `true` lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="bd900-129">Policy statement(s) enclosed within the first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="bd900-130">Házirendek határolt a `<otherwise/>` elem, ha van ilyen, alkalmazza a rendszer, ha az összes, a `<when/>` elem feltétel attribútumok `false`.</span><span class="sxs-lookup"><span data-stu-id="bd900-130">Policies enclosed within the `<otherwise/>` element, if present, will be applied if all of the `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="bd900-131">Példák</span><span class="sxs-lookup"><span data-stu-id="bd900-131">Examples</span></span>  
  
####  <span data-ttu-id="bd900-132"><a name="ChooseExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="bd900-133">A következő példa bemutatja egy [set-változó](api-management-advanced-policies.md#set-variable) házirend és a két vezérlése áramlási szabályzatokkal.</span><span class="sxs-lookup"><span data-stu-id="bd900-133">The following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="bd900-134">A változó szabály beállítása a bejövő szakaszban, és létrehoz egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) változó értéke igaz, ha a `User-Agent` kérelem fejléc tartalmazza a szöveg `iPad` vagy `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="bd900-134">The set variable policy is in the inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="bd900-135">Az első ellenőrzési folyamata házirend is a bejövő szakaszban, és feltételesen vonatkozik két egyik [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) értékének attól a `isMobile` környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="bd900-135">The first control flow policy is also in the inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on the value of the `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="bd900-136">A második ellenőrzési folyamata házirend kimenő szakaszában és feltételesen alkalmazza a [XML konvertálása JSON formátumúvá](api-management-transformation-policies.md#ConvertXMLtoJSON) házirend amikor `isMobile` értéke `true`.</span><span class="sxs-lookup"><span data-stu-id="bd900-136">The second control flow policy is in the outbound section and conditionally applies the [Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set to `true`.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="bd900-137">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-137">Example</span></span>  
 <span data-ttu-id="bd900-138">A példa bemutatja, hogyan hajthat végre a tartalom alapján történő szűrés adatelemek eltávolítása a válasz érkezett a háttérszolgáltatás használata esetén a `Starter` termék.</span><span class="sxs-lookup"><span data-stu-id="bd900-138">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="bd900-139">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és a 34:30 előretekerés.</span><span class="sxs-lookup"><span data-stu-id="bd900-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="bd900-140">Megtekinthet egy áttekintést 31:50 kezdjék [a sötét égbolt előrejelzési API](https://developer.forecast.io/) ebben a bemutatóban használt.</span><span class="sxs-lookup"><span data-stu-id="bd900-140">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-141">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-141">Elements</span></span>  
  
|<span data-ttu-id="bd900-142">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-142">Element</span></span>|<span data-ttu-id="bd900-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-143">Description</span></span>|<span data-ttu-id="bd900-144">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-145">Válassza a</span><span class="sxs-lookup"><span data-stu-id="bd900-145">choose</span></span>|<span data-ttu-id="bd900-146">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-146">Root element.</span></span>|<span data-ttu-id="bd900-147">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-147">Yes</span></span>|  
|<span data-ttu-id="bd900-148">mikor</span><span class="sxs-lookup"><span data-stu-id="bd900-148">when</span></span>|<span data-ttu-id="bd900-149">Az állapotot, amelyet használni a `if` vagy `ifelse` részei a `choose` házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-149">The condition to use for the `if` or `ifelse` parts of the `choose` policy.</span></span> <span data-ttu-id="bd900-150">Ha a `choose` -házirendben engedélyezve van több `when` szakaszok, azok egymás után értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="bd900-150">If the `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="bd900-151">Egyszer a `condition` , hogy amikor egy elem értékelődik ki `true`, további `when` feltételek értékelésének.</span><span class="sxs-lookup"><span data-stu-id="bd900-151">Once the `condition` of a when element evaluates to `true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="bd900-152">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-152">Yes</span></span>|  
|<span data-ttu-id="bd900-153">Ellenkező esetben</span><span class="sxs-lookup"><span data-stu-id="bd900-153">otherwise</span></span>|<span data-ttu-id="bd900-154">A házirend-részlet használható, ha egyike sem tartalmazza a `when` feltételek értékelhetők `true`.</span><span class="sxs-lookup"><span data-stu-id="bd900-154">Contains the policy snippet to be used if none of the `when` conditions evaluate to `true`.</span></span>|<span data-ttu-id="bd900-155">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-156">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-156">Attributes</span></span>  
  
|<span data-ttu-id="bd900-157">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-157">Attribute</span></span>|<span data-ttu-id="bd900-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-158">Description</span></span>|<span data-ttu-id="bd900-159">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="bd900-160">feltétel = "logikai kifejezés &#124; Logikai állandó"</span><span class="sxs-lookup"><span data-stu-id="bd900-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="bd900-161">A logikai kifejezés vagy állandó értékeli ki, ha a tartalmazó `when` házirend-utasítás kiértékelése történik.</span><span class="sxs-lookup"><span data-stu-id="bd900-161">The Boolean expression or constant to evaluated when the containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="bd900-162">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-162">Yes</span></span>|  
  
###  <span data-ttu-id="bd900-163"><a name="ChooseUsage"></a>Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="bd900-164">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-164">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-165">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-166">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-167"><a name="ForwardRequest"></a>Kérés továbbítása</span><span class="sxs-lookup"><span data-stu-id="bd900-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="bd900-168">A `forward-request` házirend továbbítja a bejövő kérelem által a háttérszolgáltatáshoz a kérelemben megadott [környezet](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="bd900-168">The `forward-request` policy forwards the incoming request to the backend service specified in the request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="bd900-169">A háttérkiszolgáló URL-címe van megadva az API-t [beállítások](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) és módosítható a [be háttérszolgáltatás](api-management-transformation-policies.md) házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-169">The backend service URL is specified in the API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using the [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="bd900-170">A csoportházirend-eredményhez eltávolítása a kérelem nem lesznek továbbítva a háttér szolgáltatás és a kimenő szakaszban házirendek kiértékelése azonnal a bejövő szakaszban házirendek sikeres befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="bd900-170">Removing this policy results in the request not being forwarded to the backend service and the policies in the outbound section are evaluated immediately upon the successful completion of the policies in the inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-171">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="bd900-172">Példák</span><span class="sxs-lookup"><span data-stu-id="bd900-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="bd900-173">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-173">Example</span></span>  
 <span data-ttu-id="bd900-174">A következő API-szintű szabályzat továbbítja a háttérszolgáltatáshoz 60 másodperces időtúllépési időköz minden kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="bd900-174">The following API level policy forwards all requests to the backend service with a timeout interval of 60 seconds.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="bd900-175">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-175">Example</span></span>  
 <span data-ttu-id="bd900-176">Ez a művelet szintű szabályzat használja a `base` elem a háttérrendszer házirend örökli a szülő API-szintű hatókör.</span><span class="sxs-lookup"><span data-stu-id="bd900-176">This operation level policy uses the `base` element to inherit the backend policy from the parent API level scope.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="bd900-177">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-177">Example</span></span>  
 <span data-ttu-id="bd900-178">Ez a művelet szintű szabályzat explicit módon továbbítja a háttérszolgáltatáshoz 120 időtúllépés az összes kérelmet, és nem örökli a szülő API-szintű háttér házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-178">This operation level policy explicitly forwards all requests to the backend service with a timeout of 120 and does not inherit the parent API level backend policy.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="bd900-179">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-179">Example</span></span>  
 <span data-ttu-id="bd900-180">Ez a művelet szintű szabályzat nem továbbítja a kérelem által a háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bd900-180">This operation level policy does not forward requests to the backend service.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-181">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-181">Elements</span></span>  
  
|<span data-ttu-id="bd900-182">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-182">Element</span></span>|<span data-ttu-id="bd900-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-183">Description</span></span>|<span data-ttu-id="bd900-184">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-185">előre-kérés</span><span class="sxs-lookup"><span data-stu-id="bd900-185">forward-request</span></span>|<span data-ttu-id="bd900-186">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-186">Root element.</span></span>|<span data-ttu-id="bd900-187">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-188">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-188">Attributes</span></span>  
  
|<span data-ttu-id="bd900-189">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-189">Attribute</span></span>|<span data-ttu-id="bd900-190">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-190">Description</span></span>|<span data-ttu-id="bd900-191">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-191">Required</span></span>|<span data-ttu-id="bd900-192">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-193">Időtúllépés = "integer"</span><span class="sxs-lookup"><span data-stu-id="bd900-193">timeout="integer"</span></span>|<span data-ttu-id="bd900-194">Nem sikerül a időkorlátja, másodpercben. a háttérszolgáltatás hívása előtt.</span><span class="sxs-lookup"><span data-stu-id="bd900-194">The timeout interval in seconds before the call to the backend service fails.</span></span>|<span data-ttu-id="bd900-195">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-195">No</span></span>|<span data-ttu-id="bd900-196">Nincs időtúllépés</span><span class="sxs-lookup"><span data-stu-id="bd900-196">No timeout</span></span>|  
|<span data-ttu-id="bd900-197">hajtsa végre-átirányítások = "true &#124; FALSE"</span><span class="sxs-lookup"><span data-stu-id="bd900-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="bd900-198">Megadja, hogy a háttérszolgáltatáshoz átirányítása követi az átjáró vagy visszaérkezik a hívóhoz.</span><span class="sxs-lookup"><span data-stu-id="bd900-198">Specifies whether redirects from the backend service are followed by the gateway or returned to the caller.</span></span>|<span data-ttu-id="bd900-199">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-199">No</span></span>|<span data-ttu-id="bd900-200">hamis</span><span class="sxs-lookup"><span data-stu-id="bd900-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-201">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-201">Usage</span></span>  
 <span data-ttu-id="bd900-202">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-202">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-203">**Házirend szakaszok:** háttér</span><span class="sxs-lookup"><span data-stu-id="bd900-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="bd900-204">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-205"><a name="LimitConcurrency"></a>A feldolgozási korlátot</span><span class="sxs-lookup"><span data-stu-id="bd900-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="bd900-206">A `limit-concurrency` házirend megakadályozza, hogy a zárt házirendek egy adott időpontban legfeljebb a megadott számú kérelem végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="bd900-206">The `limit-concurrency` policy prevents enclosed policies from executing by more than the specified number of requests at a given time.</span></span> <span data-ttu-id="bd900-207">Új kérelmek alapján meghaladó, kerülnek be várólista, amíg a várólista maximális hossza érhető el.</span><span class="sxs-lookup"><span data-stu-id="bd900-207">Upon exceeding the threshold, new requests are added to a queue, until the maximum queue length is achieved.</span></span> <span data-ttu-id="bd900-208">Várólista Erőforrásfogyás esetén új kérelmek sikertelenek lesznek azonnal.</span><span class="sxs-lookup"><span data-stu-id="bd900-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="bd900-209"><a name="LimitConcurrencyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="bd900-210">Példák</span><span class="sxs-lookup"><span data-stu-id="bd900-210">Examples</span></span>  
  
####  <span data-ttu-id="bd900-211"><a name="ChooseExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="bd900-212">A következő példa bemutatja, hogyan legyen korlátozva egy környezeti változó értéke alapján háttérkiszolgálón továbbított kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="bd900-212">The following example demonstrates how to limit number of requests forwarded to a backend based on the value of a context variable.</span></span>
 
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

### <a name="elements"></a><span data-ttu-id="bd900-213">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-213">Elements</span></span>  
  
|<span data-ttu-id="bd900-214">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-214">Element</span></span>|<span data-ttu-id="bd900-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-215">Description</span></span>|<span data-ttu-id="bd900-216">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="bd900-217">határ-feldolgozási</span><span class="sxs-lookup"><span data-stu-id="bd900-217">limit-concurrency</span></span>|<span data-ttu-id="bd900-218">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-218">Root element.</span></span>|<span data-ttu-id="bd900-219">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-220">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-220">Attributes</span></span>  
  
|<span data-ttu-id="bd900-221">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-221">Attribute</span></span>|<span data-ttu-id="bd900-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-222">Description</span></span>|<span data-ttu-id="bd900-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-223">Required</span></span>|<span data-ttu-id="bd900-224">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="bd900-225">kulcs</span><span class="sxs-lookup"><span data-stu-id="bd900-225">key</span></span>|<span data-ttu-id="bd900-226">Egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bd900-226">A string.</span></span> <span data-ttu-id="bd900-227">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="bd900-227">Expression allowed.</span></span> <span data-ttu-id="bd900-228">Meghatározza azt a feldolgozási hatókört.</span><span class="sxs-lookup"><span data-stu-id="bd900-228">Specifies the concurrency scope.</span></span> <span data-ttu-id="bd900-229">Megoszthatók több házirendet.</span><span class="sxs-lookup"><span data-stu-id="bd900-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="bd900-230">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-230">Yes</span></span>|<span data-ttu-id="bd900-231">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-231">N/A</span></span>|  
|<span data-ttu-id="bd900-232">maximális száma</span><span class="sxs-lookup"><span data-stu-id="bd900-232">max-count</span></span>|<span data-ttu-id="bd900-233">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="bd900-233">An integer.</span></span> <span data-ttu-id="bd900-234">Megadja, hogy mely adja meg a házirend kérelmek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="bd900-234">Specifies a maximum number of requests that are allowed to enter the policy.</span></span>|<span data-ttu-id="bd900-235">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-235">Yes</span></span>|<span data-ttu-id="bd900-236">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-236">N/A</span></span>|  
|<span data-ttu-id="bd900-237">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="bd900-237">timeout</span></span>|<span data-ttu-id="bd900-238">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="bd900-238">An integer.</span></span> <span data-ttu-id="bd900-239">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="bd900-239">Expression allowed.</span></span> <span data-ttu-id="bd900-240">Adja meg másodpercben, egy kérelem várnia kell, mielőtt hibát jelentene, a hatókör megadását "403-as túl sok kérelem"</span><span class="sxs-lookup"><span data-stu-id="bd900-240">Specifies the number of seconds a request should wait to enter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="bd900-241">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-241">No</span></span>|<span data-ttu-id="bd900-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="bd900-242">Infinity</span></span>|  
|<span data-ttu-id="bd900-243">maximális hosszúságú várólista</span><span class="sxs-lookup"><span data-stu-id="bd900-243">max-queue-length</span></span>|<span data-ttu-id="bd900-244">Egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="bd900-244">An integer.</span></span> <span data-ttu-id="bd900-245">A kifejezés engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="bd900-245">Expression allowed.</span></span> <span data-ttu-id="bd900-246">A várólista maximális hosszát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="bd900-246">Specifies the maximum queue length.</span></span> <span data-ttu-id="bd900-247">A bejövő kérelem próbál meg adja meg a házirend befejeződik a "403-as túl sok kérelem" azonnal amikor a várólista kimerült.</span><span class="sxs-lookup"><span data-stu-id="bd900-247">Incoming requests trying to enter this policy will be terminated with “403 Too Many Requests” immediately when the queue is exhausted.</span></span>|<span data-ttu-id="bd900-248">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-248">No</span></span>|<span data-ttu-id="bd900-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="bd900-249">Infinity</span></span>|  
  
###  <span data-ttu-id="bd900-250"><a name="ChooseUsage"></a>Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="bd900-251">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-251">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-252">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-253">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="bd900-254"><a name="log-to-eventhub"></a>Az Event Hubs napló</span><span class="sxs-lookup"><span data-stu-id="bd900-254"><a name="log-to-eventhub"></a> Log to Event Hub</span></span>  
 <span data-ttu-id="bd900-255">A `log-to-eventhub` házirend üzeneteket küld a megadott formátumban határozzák meg a tranzakciónaplókat tartalmazó entitás Eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="bd900-255">The `log-to-eventhub` policy sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="bd900-256">A név azt jelenti, mert a házirend kijelölt kérés vagy válasz környezeti adatok mentése online vagy kapcsolat nélküli elemzéshez használható.</span><span class="sxs-lookup"><span data-stu-id="bd900-256">As its name implies, the policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="bd900-257">Részletes útmutató, az event hubs és a naplózási események konfigurálása, lásd: [útmutató az Azure Event Hubs API Management naplózása](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="bd900-257">For a step-by-step guide on configuring an event hub and logging events, see [How to log API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-258">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-259">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-259">Example</span></span>  
 <span data-ttu-id="bd900-260">Bármilyen karakterlánc értékeként használható Event Hubs be kell jelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="bd900-260">Any string can be used as the value to be logged in Event Hubs.</span></span> <span data-ttu-id="bd900-261">Ebben a példában a dátum és idő, telepítési szolgáltatás neve, kérelemazonosító, IP-cím és az összes bejövő hívás művelet nevét a rendszer naplózza az event hubs naplózó regisztrálva a `contoso-logger` azonosítója.</span><span class="sxs-lookup"><span data-stu-id="bd900-261">In this example the date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged to the event hub Logger registered with the `contoso-logger` id.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-262">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-262">Elements</span></span>  
  
|<span data-ttu-id="bd900-263">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-263">Element</span></span>|<span data-ttu-id="bd900-264">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-264">Description</span></span>|<span data-ttu-id="bd900-265">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-266">napló-eventhub</span><span class="sxs-lookup"><span data-stu-id="bd900-266">log-to-eventhub</span></span>|<span data-ttu-id="bd900-267">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-267">Root element.</span></span> <span data-ttu-id="bd900-268">Ez az elem értéke bejelentkezni az eseményközpont karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bd900-268">The value of this element is the string to log to your event hub.</span></span>|<span data-ttu-id="bd900-269">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-270">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-270">Attributes</span></span>  
  
|<span data-ttu-id="bd900-271">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-271">Attribute</span></span>|<span data-ttu-id="bd900-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-272">Description</span></span>|<span data-ttu-id="bd900-273">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="bd900-274">naplózó-azonosító</span><span class="sxs-lookup"><span data-stu-id="bd900-274">logger-id</span></span>|<span data-ttu-id="bd900-275">Az API Management szolgáltatásban regisztrált a naplózó azonosítója.</span><span class="sxs-lookup"><span data-stu-id="bd900-275">The id of the Logger registered with your API Management service.</span></span>|<span data-ttu-id="bd900-276">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-276">Yes</span></span>|  
|<span data-ttu-id="bd900-277">partícióazonosító-:</span><span class="sxs-lookup"><span data-stu-id="bd900-277">partition-id</span></span>|<span data-ttu-id="bd900-278">Meghatározza a partíció, ahol az üzenetek küldése történik az index.</span><span class="sxs-lookup"><span data-stu-id="bd900-278">Specifies the index of the partition where messages are sent.</span></span>|<span data-ttu-id="bd900-279">Választható.</span><span class="sxs-lookup"><span data-stu-id="bd900-279">Optional.</span></span> <span data-ttu-id="bd900-280">Ez az attribútum nem használható, ha `partition-key` szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="bd900-281">a partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="bd900-281">partition-key</span></span>|<span data-ttu-id="bd900-282">Meghatározza azt az értéket a partíció-hozzárendelés üzenettémakörbe küldött üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="bd900-282">Specifies the value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="bd900-283">Választható.</span><span class="sxs-lookup"><span data-stu-id="bd900-283">Optional.</span></span> <span data-ttu-id="bd900-284">Ez az attribútum nem használható, ha `partition-id` szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-285">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-285">Usage</span></span>  
 <span data-ttu-id="bd900-286">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-286">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-287">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-288">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="bd900-289"><a name="mock-response"></a>Utánzatait válasz</span><span class="sxs-lookup"><span data-stu-id="bd900-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="bd900-290">A `mock-response`, a névként azt jelenti, API-k és műveletek mock szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-290">The `mock-response`, as the name implies, is used to mock APIs and operations.</span></span> <span data-ttu-id="bd900-291">Normál feldolgozási sor végrehajtása megszakítja, és a hívó mocked választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bd900-291">It aborts normal pipeline execution and returns a mocked response to the caller.</span></span> <span data-ttu-id="bd900-292">A házirend mindig megpróbálja térjen vissza a legmagasabb hűség válaszokat.</span><span class="sxs-lookup"><span data-stu-id="bd900-292">The policy always tries to return responses of highest fidelity.</span></span> <span data-ttu-id="bd900-293">Válasz tartalom példák, amikor a rendelkezésre álló szívesebben.</span><span class="sxs-lookup"><span data-stu-id="bd900-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="bd900-294">Minta válaszok létrehozott sémák, a sémák találhatók, és többek között nem.</span><span class="sxs-lookup"><span data-stu-id="bd900-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="bd900-295">Ha a példák és sémák nem találhatók, nem tartalmú válaszok is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="bd900-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-296">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="bd900-297">Példák</span><span class="sxs-lookup"><span data-stu-id="bd900-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-298">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-298">Elements</span></span>  
  
|<span data-ttu-id="bd900-299">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-299">Element</span></span>|<span data-ttu-id="bd900-300">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-300">Description</span></span>|<span data-ttu-id="bd900-301">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-302">mock-válasz</span><span class="sxs-lookup"><span data-stu-id="bd900-302">mock-response</span></span>|<span data-ttu-id="bd900-303">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-303">Root element.</span></span>|<span data-ttu-id="bd900-304">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-305">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-305">Attributes</span></span>  
  
|<span data-ttu-id="bd900-306">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-306">Attribute</span></span>|<span data-ttu-id="bd900-307">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-307">Description</span></span>|<span data-ttu-id="bd900-308">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-308">Required</span></span>|<span data-ttu-id="bd900-309">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="bd900-310">állapotkód-:</span><span class="sxs-lookup"><span data-stu-id="bd900-310">status-code</span></span>|<span data-ttu-id="bd900-311">Válasz állapotkódja határozza meg, és válassza ki a megfelelő példa vagy séma használatával.</span><span class="sxs-lookup"><span data-stu-id="bd900-311">Specifies response status code and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="bd900-312">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-312">No</span></span>|<span data-ttu-id="bd900-313">200</span><span class="sxs-lookup"><span data-stu-id="bd900-313">200</span></span>|  
|<span data-ttu-id="bd900-314">tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="bd900-314">content-type</span></span>|<span data-ttu-id="bd900-315">Itt adhatja meg `Content-Type` válasz állomásfejléc-érték, és válassza ki a megfelelő példa vagy séma.</span><span class="sxs-lookup"><span data-stu-id="bd900-315">Specifies `Content-Type` response header value and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="bd900-316">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-316">No</span></span>|<span data-ttu-id="bd900-317">None</span><span class="sxs-lookup"><span data-stu-id="bd900-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-318">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-318">Usage</span></span>  
 <span data-ttu-id="bd900-319">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-319">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-320">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="bd900-321">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="bd900-322"><a name="Retry"></a>Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="bd900-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="bd900-323">A `retry` házirend egyszer végrehajtja az alárendelt házirendeket, és majd újrapróbálkozik a végrehajtás, amíg az újra gombra `condition` válik `false` , vagy próbálkozzon újra `count` kimerült.</span><span class="sxs-lookup"><span data-stu-id="bd900-323">The             `retry` policy executes its child policies once and then retries their execution until the retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-324">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-324">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="bd900-325">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-325">Example</span></span>  
 <span data-ttu-id="bd900-326">A következő példa egy kérelem a forewarding a rendszer ismét megkísérli legfeljebb algoritmus tízszeresének használatával exponenciális próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="bd900-326">In the following example request forewarding is retried up to ten times using exponential retry algorithm.</span></span> <span data-ttu-id="bd900-327">Mivel `first-fast-retry` beállítva hamis értékre, az összes újrapróbálkozások lépnek a exponsntial újrapróbálkozási algoritmust.</span><span class="sxs-lookup"><span data-stu-id="bd900-327">Since                    `first-fast-retry` is set to false, all retry attempts are subject to the exponsntial retry algorithm.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-328">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-328">Elements</span></span>  
  
|<span data-ttu-id="bd900-329">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-329">Element</span></span>|<span data-ttu-id="bd900-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-330">Description</span></span>|<span data-ttu-id="bd900-331">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-332">Próbálja meg újra</span><span class="sxs-lookup"><span data-stu-id="bd900-332">retry</span></span>|<span data-ttu-id="bd900-333">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-333">Root element.</span></span> <span data-ttu-id="bd900-334">Előfordulhat, hogy bármely egyéb házirendek gyermekelemeinek tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="bd900-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="bd900-335">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-336">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-336">Attributes</span></span>  
  
|<span data-ttu-id="bd900-337">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-337">Attribute</span></span>|<span data-ttu-id="bd900-338">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-338">Description</span></span>|<span data-ttu-id="bd900-339">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-339">Required</span></span>|<span data-ttu-id="bd900-340">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-341">Az állapot</span><span class="sxs-lookup"><span data-stu-id="bd900-341">condition</span></span>|<span data-ttu-id="bd900-342">Logikai szövegkonstans vagy [kifejezés](api-management-policy-expressions.md) adja meg, ha az újrapróbálkozások le kell állítani (`false`) vagy továbbra is (`true`).</span><span class="sxs-lookup"><span data-stu-id="bd900-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="bd900-343">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-343">Yes</span></span>|<span data-ttu-id="bd900-344">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-344">N/A</span></span>|  
|<span data-ttu-id="bd900-345">Száma</span><span class="sxs-lookup"><span data-stu-id="bd900-345">count</span></span>|<span data-ttu-id="bd900-346">Egy pozitív szám, amely megadja az újrapróbálkozások maximális számát kísérlet.</span><span class="sxs-lookup"><span data-stu-id="bd900-346">A positive number specifying the maximum number of retries to attempt.</span></span>|<span data-ttu-id="bd900-347">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-347">Yes</span></span>|<span data-ttu-id="bd900-348">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-348">N/A</span></span>|  
|<span data-ttu-id="bd900-349">időköz</span><span class="sxs-lookup"><span data-stu-id="bd900-349">interval</span></span>|<span data-ttu-id="bd900-350">Próbálja meg egy pozitív szám, a között az újrapróbálkozási időköz megadása másodpercben.</span><span class="sxs-lookup"><span data-stu-id="bd900-350">A positive number in seconds specifying the wait interval between the retry attempts.</span></span>|<span data-ttu-id="bd900-351">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-351">Yes</span></span>|<span data-ttu-id="bd900-352">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-352">N/A</span></span>|  
|<span data-ttu-id="bd900-353">maximális-időköz</span><span class="sxs-lookup"><span data-stu-id="bd900-353">max-interval</span></span>|<span data-ttu-id="bd900-354">Egy pozitív szám, a maximális megadása másodpercben Várjon, amíg az újrapróbálkozási kísérletek közötti időközt.</span><span class="sxs-lookup"><span data-stu-id="bd900-354">A positive number in seconds specifying the maximum wait interval between the retry attempts.</span></span> <span data-ttu-id="bd900-355">Az exponenciális újrapróbálkozási algoritmust végrehajtásához szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-355">It is used to implement an exponential retry algorithm.</span></span>|<span data-ttu-id="bd900-356">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-356">No</span></span>|<span data-ttu-id="bd900-357">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-357">N/A</span></span>|  
|<span data-ttu-id="bd900-358">különbözeti</span><span class="sxs-lookup"><span data-stu-id="bd900-358">delta</span></span>|<span data-ttu-id="bd900-359">Egy pozitív szám, a várakozási időközt növekmény megadása másodpercben.</span><span class="sxs-lookup"><span data-stu-id="bd900-359">A positive number in seconds specifying the wait interval increment.</span></span> <span data-ttu-id="bd900-360">A lineáris és exponenciális újrapróbálkozási algoritmust végrehajtására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-360">It is used to implement the linear and exponential retry algorithms.</span></span>|<span data-ttu-id="bd900-361">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-361">No</span></span>|<span data-ttu-id="bd900-362">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-362">N/A</span></span>|  
|<span data-ttu-id="bd900-363">első fast-újrapróbálkozási</span><span class="sxs-lookup"><span data-stu-id="bd900-363">first-fast-retry</span></span>|<span data-ttu-id="bd900-364">Ha beállítása `true` , az első újrapróbálkozások azonnal megtörténik.</span><span class="sxs-lookup"><span data-stu-id="bd900-364">If set to                                    `true` , the first retry attempt is performed immediately.</span></span>|<span data-ttu-id="bd900-365">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="bd900-366">Ha csak a `interval` meg van adva, **rögzített** időköz újrapróbálkozások hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="bd900-366">When only the `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="bd900-367">Ha csak a `interval` és `delta` vannak megadva, a **lineáris** időköz újrapróbálkozási algoritmust használnak, ahol az újrapróbálkozások közötti várakozási idő kiszámítása a következő képlet - szerint `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="bd900-367">When only the `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according the following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="bd900-368">Ha a `interval`, `max-interval` és `delta` vannak megadva, **exponenciális** időköz újrapróbálkozási algoritmust alkalmazza, ha a várakozási idő a próbálkozások közötti inkább exponenciálisan értéke `interval` számára az érték `max-interval` megfelelően a következő forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="bd900-368">When the `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where the wait time between the retries is growing exponentially from the value of `interval` to the value `max-interval` according to the following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="bd900-369">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-369">Usage</span></span>  
 <span data-ttu-id="bd900-370">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="bd900-370">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="bd900-371">Vegye figyelembe, hogy ez a házirend öröklik a gyermek házirend használattal kapcsolatos korlátozásokat.</span><span class="sxs-lookup"><span data-stu-id="bd900-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="bd900-372">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-373">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-374"><a name="ReturnResponse"></a>Válasz visszaadása</span><span class="sxs-lookup"><span data-stu-id="bd900-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="bd900-375">A `return-response` házirend megszakítja a feldolgozási sor végrehajtása, és egy alapértelmezett vagy egyéni reagálva a hívó adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bd900-375">The `return-response` policy aborts pipeline execution and returns either a default or custom response to the caller.</span></span> <span data-ttu-id="bd900-376">Alapértelmezett válasz `200 OK` nem szervezethez.</span><span class="sxs-lookup"><span data-stu-id="bd900-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="bd900-377">Egyéni válasz egy környezeti változó vagy házirend utasítások keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="bd900-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="bd900-378">Mindkét biztosított, ha a válasz a környezeti változó belül található módosul a a házirend-utasításoknál előtt alatt visszaérkezik a hívóhoz.</span><span class="sxs-lookup"><span data-stu-id="bd900-378">When both are provided, the response contained within the context variable is modified by the policy statements before being returned to the caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-379">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-380">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-381">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-381">Elements</span></span>  
  
|<span data-ttu-id="bd900-382">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-382">Element</span></span>|<span data-ttu-id="bd900-383">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-383">Description</span></span>|<span data-ttu-id="bd900-384">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-385">visszatérési-válasz</span><span class="sxs-lookup"><span data-stu-id="bd900-385">return-response</span></span>|<span data-ttu-id="bd900-386">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-386">Root element.</span></span>|<span data-ttu-id="bd900-387">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-387">Yes</span></span>|  
|<span data-ttu-id="bd900-388">set-fejléc</span><span class="sxs-lookup"><span data-stu-id="bd900-388">set-header</span></span>|<span data-ttu-id="bd900-389">A [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="bd900-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="bd900-390">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-390">No</span></span>|  
|<span data-ttu-id="bd900-391">set-szervezet</span><span class="sxs-lookup"><span data-stu-id="bd900-391">set-body</span></span>|<span data-ttu-id="bd900-392">A [set-törzsének](api-management-transformation-policies.md#SetBody) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="bd900-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="bd900-393">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-393">No</span></span>|  
|<span data-ttu-id="bd900-394">állapot beállítása</span><span class="sxs-lookup"><span data-stu-id="bd900-394">set-status</span></span>|<span data-ttu-id="bd900-395">A [állapotának beállítása](api-management-advanced-policies.md#SetStatus) házirend-utasítás.</span><span class="sxs-lookup"><span data-stu-id="bd900-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="bd900-396">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-397">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-397">Attributes</span></span>  
  
|<span data-ttu-id="bd900-398">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-398">Attribute</span></span>|<span data-ttu-id="bd900-399">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-399">Description</span></span>|<span data-ttu-id="bd900-400">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="bd900-401">válasz-változó-neve</span><span class="sxs-lookup"><span data-stu-id="bd900-401">response-variable-name</span></span>|<span data-ttu-id="bd900-402">A környezeti változó neve hivatkozni, például felsőbb rétegbeli [küldési-kérelmek](api-management-advanced-policies.md#SendRequest) házirend, és úgy, hogy egy `Response` objektum</span><span class="sxs-lookup"><span data-stu-id="bd900-402">The name of the context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="bd900-403">Választható.</span><span class="sxs-lookup"><span data-stu-id="bd900-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-404">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-404">Usage</span></span>  
 <span data-ttu-id="bd900-405">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-405">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-406">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-407">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-408"><a name="SendOneWayRequest"></a>Egyirányú kérés küldése</span><span class="sxs-lookup"><span data-stu-id="bd900-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="bd900-409">A `send-one-way-request` házirend a megadott kérést küld a megadott URL-cím a válaszra való várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="bd900-409">The `send-one-way-request` policy sends the provided request to the specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-410">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-411">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-411">Example</span></span>  
 <span data-ttu-id="bd900-412">Ez a minta-házirend használatával példáját mutatja be a `send-one-way-request` egy üzenetet küldeni a Slack Csevegés helyiségben, ha a HTTP válaszkódot nagyobb vagy egyenlő 500 házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-412">This sample policy shows an example of using the `send-one-way-request` policy to send a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="bd900-413">Ez a minta további információkért lásd: [használata az Azure API Management szolgáltatás a külső services](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="bd900-413">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-414">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-414">Elements</span></span>  
  
|<span data-ttu-id="bd900-415">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-415">Element</span></span>|<span data-ttu-id="bd900-416">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-416">Description</span></span>|<span data-ttu-id="bd900-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-418">küldési-egy-módon-kérelmek</span><span class="sxs-lookup"><span data-stu-id="bd900-418">send-one-way-request</span></span>|<span data-ttu-id="bd900-419">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-419">Root element.</span></span>|<span data-ttu-id="bd900-420">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-420">Yes</span></span>|  
|<span data-ttu-id="bd900-421">URL-címe</span><span class="sxs-lookup"><span data-stu-id="bd900-421">url</span></span>|<span data-ttu-id="bd900-422">A kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bd900-422">The URL of the request.</span></span>|<span data-ttu-id="bd900-423">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="bd900-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="bd900-424">Módszer</span><span class="sxs-lookup"><span data-stu-id="bd900-424">method</span></span>|<span data-ttu-id="bd900-425">A kérelem HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="bd900-425">The HTTP method for the request.</span></span>|<span data-ttu-id="bd900-426">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="bd900-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="bd900-427">header</span><span class="sxs-lookup"><span data-stu-id="bd900-427">header</span></span>|<span data-ttu-id="bd900-428">Kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="bd900-428">Request header.</span></span> <span data-ttu-id="bd900-429">Használjon több több kérelem fejlécek.</span><span class="sxs-lookup"><span data-stu-id="bd900-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="bd900-430">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-430">No</span></span>|  
|<span data-ttu-id="bd900-431">Törzs</span><span class="sxs-lookup"><span data-stu-id="bd900-431">body</span></span>|<span data-ttu-id="bd900-432">A kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="bd900-432">The request body.</span></span>|<span data-ttu-id="bd900-433">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-434">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-434">Attributes</span></span>  
  
|<span data-ttu-id="bd900-435">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-435">Attribute</span></span>|<span data-ttu-id="bd900-436">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-436">Description</span></span>|<span data-ttu-id="bd900-437">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-437">Required</span></span>|<span data-ttu-id="bd900-438">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-439">mód = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-439">mode="string"</span></span>|<span data-ttu-id="bd900-440">Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem egy példányát.</span><span class="sxs-lookup"><span data-stu-id="bd900-440">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="bd900-441">Kimenő módban mód = másolása nem sikerült a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="bd900-441">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="bd900-442">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-442">No</span></span>|<span data-ttu-id="bd900-443">új</span><span class="sxs-lookup"><span data-stu-id="bd900-443">New</span></span>|  
|<span data-ttu-id="bd900-444">név</span><span class="sxs-lookup"><span data-stu-id="bd900-444">name</span></span>|<span data-ttu-id="bd900-445">A neve a fejléc kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="bd900-445">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="bd900-446">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-446">Yes</span></span>|<span data-ttu-id="bd900-447">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-447">N/A</span></span>|  
|<span data-ttu-id="bd900-448">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="bd900-448">exists-action</span></span>|<span data-ttu-id="bd900-449">Megadja, milyen műveletet hajtson végre a fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="bd900-449">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="bd900-450">Ez az attribútum a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd900-450">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="bd900-451">-felülbírálás - lecseréli a meglévő fejléc értékének.</span><span class="sxs-lookup"><span data-stu-id="bd900-451">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="bd900-452">-skip – nem helyettesíti a meglévő-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="bd900-452">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="bd900-453">-hozzáfűzése - az érték hozzáfűzi a meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="bd900-453">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="bd900-454">-törlés - eltávolítja a fejlécet a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="bd900-454">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="bd900-455">Ha beállítása `override` történő besorolásakor ugyanazzal a névvel több bejegyzést eredményez az összes bejegyzés (amely jelennek meg több alkalommal) megfelelően történő beállítása fejléc; csak a listában szereplő értékek be lesznek állítva az eredményt.</span><span class="sxs-lookup"><span data-stu-id="bd900-455">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="bd900-456">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-456">No</span></span>|<span data-ttu-id="bd900-457">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="bd900-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-458">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-458">Usage</span></span>  
 <span data-ttu-id="bd900-459">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-459">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-460">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-461">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-462"><a name="SendRequest"></a>Kérés küldése</span><span class="sxs-lookup"><span data-stu-id="bd900-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="bd900-463">A `send-request` házirend a megadott kérést küld a megadott URL-cím nem tovább, mint a beállított időtúllépési értéket vár.</span><span class="sxs-lookup"><span data-stu-id="bd900-463">The `send-request` policy sends the provided request to the specified URL, waiting no longer than the set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-464">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-465">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-465">Example</span></span>  
 <span data-ttu-id="bd900-466">Ez a példa bemutatja a hivatkozás győződhet token az engedélyezési-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="bd900-466">This example shows one way to verify a reference token with an authorization server.</span></span> <span data-ttu-id="bd900-467">Ez a minta további információkért lásd: [használata az Azure API Management szolgáltatás a külső services](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="bd900-467">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request to Token Server to validate token (see RFC 7662) -->  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-468">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-468">Elements</span></span>  
  
|<span data-ttu-id="bd900-469">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-469">Element</span></span>|<span data-ttu-id="bd900-470">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-470">Description</span></span>|<span data-ttu-id="bd900-471">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-472">küldési-kérelmek</span><span class="sxs-lookup"><span data-stu-id="bd900-472">send-request</span></span>|<span data-ttu-id="bd900-473">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-473">Root element.</span></span>|<span data-ttu-id="bd900-474">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-474">Yes</span></span>|  
|<span data-ttu-id="bd900-475">URL-címe</span><span class="sxs-lookup"><span data-stu-id="bd900-475">url</span></span>|<span data-ttu-id="bd900-476">A kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bd900-476">The URL of the request.</span></span>|<span data-ttu-id="bd900-477">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="bd900-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="bd900-478">Módszer</span><span class="sxs-lookup"><span data-stu-id="bd900-478">method</span></span>|<span data-ttu-id="bd900-479">A kérelem HTTP-metódust.</span><span class="sxs-lookup"><span data-stu-id="bd900-479">The HTTP method for the request.</span></span>|<span data-ttu-id="bd900-480">Nincs if mód = másolási; Ellenkező esetben igen.</span><span class="sxs-lookup"><span data-stu-id="bd900-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="bd900-481">header</span><span class="sxs-lookup"><span data-stu-id="bd900-481">header</span></span>|<span data-ttu-id="bd900-482">Kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="bd900-482">Request header.</span></span> <span data-ttu-id="bd900-483">Használjon több több kérelem fejlécek.</span><span class="sxs-lookup"><span data-stu-id="bd900-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="bd900-484">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-484">No</span></span>|  
|<span data-ttu-id="bd900-485">Törzs</span><span class="sxs-lookup"><span data-stu-id="bd900-485">body</span></span>|<span data-ttu-id="bd900-486">A kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="bd900-486">The request body.</span></span>|<span data-ttu-id="bd900-487">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-488">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-488">Attributes</span></span>  
  
|<span data-ttu-id="bd900-489">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-489">Attribute</span></span>|<span data-ttu-id="bd900-490">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-490">Description</span></span>|<span data-ttu-id="bd900-491">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-491">Required</span></span>|<span data-ttu-id="bd900-492">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-493">mód = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-493">mode="string"</span></span>|<span data-ttu-id="bd900-494">Meghatározza, hogy ez egy új kérelmet vagy a jelenlegi kérelem egy példányát.</span><span class="sxs-lookup"><span data-stu-id="bd900-494">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="bd900-495">Kimenő módban mód = másolása nem sikerült a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="bd900-495">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="bd900-496">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-496">No</span></span>|<span data-ttu-id="bd900-497">új</span><span class="sxs-lookup"><span data-stu-id="bd900-497">New</span></span>|  
|<span data-ttu-id="bd900-498">válasz-változó-name = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-498">response-variable-name="string"</span></span>|<span data-ttu-id="bd900-499">Ha nincs jelen, `context.Response` szolgál.</span><span class="sxs-lookup"><span data-stu-id="bd900-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="bd900-500">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-500">No</span></span>|<span data-ttu-id="bd900-501">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-501">N/A</span></span>|  
|<span data-ttu-id="bd900-502">Időtúllépés = "integer"</span><span class="sxs-lookup"><span data-stu-id="bd900-502">timeout="integer"</span></span>|<span data-ttu-id="bd900-503">Nem sikerül a időkorlátja, másodpercben. az URL-cím hívása előtt.</span><span class="sxs-lookup"><span data-stu-id="bd900-503">The timeout interval in seconds before the call to the URL fails.</span></span>|<span data-ttu-id="bd900-504">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-504">No</span></span>|<span data-ttu-id="bd900-505">60</span><span class="sxs-lookup"><span data-stu-id="bd900-505">60</span></span>|  
|<span data-ttu-id="bd900-506">Hiba mellőzése</span><span class="sxs-lookup"><span data-stu-id="bd900-506">ignore-error</span></span>|<span data-ttu-id="bd900-507">Ha igaz, és hiba történt a kérelem eredményezi:</span><span class="sxs-lookup"><span data-stu-id="bd900-507">If true and the request results in an error:</span></span><br /><br /> <span data-ttu-id="bd900-508">-Ha a válasz-változó-név lett megadva, null értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bd900-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="bd900-509">-Ha a válasz-változó-neve nincs megadva, a környezetben. Kérés nem fog frissülni.</span><span class="sxs-lookup"><span data-stu-id="bd900-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="bd900-510">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-510">No</span></span>|<span data-ttu-id="bd900-511">hamis</span><span class="sxs-lookup"><span data-stu-id="bd900-511">false</span></span>|  
|<span data-ttu-id="bd900-512">név</span><span class="sxs-lookup"><span data-stu-id="bd900-512">name</span></span>|<span data-ttu-id="bd900-513">A neve a fejléc kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="bd900-513">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="bd900-514">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-514">Yes</span></span>|<span data-ttu-id="bd900-515">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-515">N/A</span></span>|  
|<span data-ttu-id="bd900-516">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="bd900-516">exists-action</span></span>|<span data-ttu-id="bd900-517">Megadja, milyen műveletet hajtson végre a fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="bd900-517">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="bd900-518">Ez az attribútum a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd900-518">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="bd900-519">-felülbírálás - lecseréli a meglévő fejléc értékének.</span><span class="sxs-lookup"><span data-stu-id="bd900-519">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="bd900-520">-skip – nem helyettesíti a meglévő-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="bd900-520">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="bd900-521">-hozzáfűzése - az érték hozzáfűzi a meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="bd900-521">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="bd900-522">-törlés - eltávolítja a fejlécet a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="bd900-522">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="bd900-523">Ha beállítása `override` történő besorolásakor ugyanazzal a névvel több bejegyzést eredményez az összes bejegyzés (amely jelennek meg több alkalommal) megfelelően történő beállítása fejléc; csak a listában szereplő értékek be lesznek állítva az eredményt.</span><span class="sxs-lookup"><span data-stu-id="bd900-523">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="bd900-524">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-524">No</span></span>|<span data-ttu-id="bd900-525">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="bd900-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-526">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-526">Usage</span></span>  
 <span data-ttu-id="bd900-527">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-527">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-528">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-529">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-530"><a name="SetHttpProxy"></a>HTTP-proxy beállítása</span><span class="sxs-lookup"><span data-stu-id="bd900-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="bd900-531">A `proxy` házirendje lehetővé teszi a kérelmek háttérkiszolgálókon egy HTTP-proxyn keresztül továbbítja.</span><span class="sxs-lookup"><span data-stu-id="bd900-531">The `proxy` policy allows you to route requests forwarded to backends via an HTTP proxy.</span></span> <span data-ttu-id="bd900-532">Csak a HTTP (nem HTTPS) az átjáró és a proxy között támogatott.</span><span class="sxs-lookup"><span data-stu-id="bd900-532">Only HTTP (not HTTPS) is supported between the gateway and the proxy.</span></span> <span data-ttu-id="bd900-533">Alapszintű, és csak az NTLM-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="bd900-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-534">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-535">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-535">Example</span></span>  
<span data-ttu-id="bd900-536">Vegye figyelembe a használatát [tulajdonságok](api-management-howto-properties.md) tartozó felhasználónevet és jelszót a bizalmas adatok tárolása a házirend-dokumentum elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="bd900-536">Note the use of [properties](api-management-howto-properties.md) as values of the username and password to avoid storing sensitive information in the policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-537">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-537">Elements</span></span>  
  
|<span data-ttu-id="bd900-538">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-538">Element</span></span>|<span data-ttu-id="bd900-539">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-539">Description</span></span>|<span data-ttu-id="bd900-540">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-541">Proxy</span><span class="sxs-lookup"><span data-stu-id="bd900-541">proxy</span></span>|<span data-ttu-id="bd900-542">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="bd900-542">Root element</span></span>|<span data-ttu-id="bd900-543">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="bd900-544">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-544">Attributes</span></span>  
  
|<span data-ttu-id="bd900-545">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-545">Attribute</span></span>|<span data-ttu-id="bd900-546">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-546">Description</span></span>|<span data-ttu-id="bd900-547">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-547">Required</span></span>|<span data-ttu-id="bd900-548">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-549">URL-cím = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-549">url="string"</span></span>|<span data-ttu-id="bd900-550">Proxy URL http://host:port formájában.</span><span class="sxs-lookup"><span data-stu-id="bd900-550">Proxy URL in the form of http://host:port.</span></span>|<span data-ttu-id="bd900-551">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-551">Yes</span></span>|<span data-ttu-id="bd900-552">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-552">N/A</span></span>|  
|<span data-ttu-id="bd900-553">felhasználónév = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-553">username="string"</span></span>|<span data-ttu-id="bd900-554">A proxy-nal való hitelesítéshez használt felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="bd900-554">Username to be used for authentication with the proxy.</span></span>|<span data-ttu-id="bd900-555">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-555">No</span></span>|<span data-ttu-id="bd900-556">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-556">N/A</span></span>|  
|<span data-ttu-id="bd900-557">jelszó = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-557">password="string"</span></span>|<span data-ttu-id="bd900-558">A proxy-hitelesítéshez használandó jelszó.</span><span class="sxs-lookup"><span data-stu-id="bd900-558">Password to be used for authentication with the proxy.</span></span>|<span data-ttu-id="bd900-559">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-559">No</span></span>|<span data-ttu-id="bd900-560">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="bd900-561">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-561">Usage</span></span>  
 <span data-ttu-id="bd900-562">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-562">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-563">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="bd900-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="bd900-564">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="bd900-565"><a name="SetRequestMethod"></a>Set kérés metódus</span><span class="sxs-lookup"><span data-stu-id="bd900-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="bd900-566">A `set-method` házirend lehetővé teszi a HTTP-kérési metódust egy kérelem módosítását.</span><span class="sxs-lookup"><span data-stu-id="bd900-566">The `set-method` policy allows you to change the HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-567">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-568">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-568">Example</span></span>  
 <span data-ttu-id="bd900-569">Ez a minta a házirend által használt a `set-method` házirend üzenetet küld a Slack Csevegés helyiségben, ha a HTTP válaszkódot nagyobb vagy egyenlő 500 példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="bd900-569">This sample policy that uses the `set-method` policy shows an example of sending a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="bd900-570">Ez a minta további információkért lásd: [használata az Azure API Management szolgáltatás a külső services](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="bd900-570">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-571">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-571">Elements</span></span>  
  
|<span data-ttu-id="bd900-572">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-572">Element</span></span>|<span data-ttu-id="bd900-573">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-573">Description</span></span>|<span data-ttu-id="bd900-574">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-575">set-módszer</span><span class="sxs-lookup"><span data-stu-id="bd900-575">set-method</span></span>|<span data-ttu-id="bd900-576">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-576">Root element.</span></span> <span data-ttu-id="bd900-577">Az elem értékét adja meg a HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="bd900-577">The value of the element specifies the HTTP method.</span></span>|<span data-ttu-id="bd900-578">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-579">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-579">Usage</span></span>  
 <span data-ttu-id="bd900-580">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-580">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-581">**Házirend szakaszok:** bejövő, hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="bd900-582">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-583"><a name="SetStatus"></a>Set-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="bd900-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="bd900-584">A `set-status` házirend értékűre állítja be a HTTP-állapotkód: a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="bd900-584">The `set-status` policy sets the HTTP status code to the specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-585">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-586">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-586">Example</span></span>  
 <span data-ttu-id="bd900-587">Ez a példa bemutatja, hogyan 401-es választ ad vissza, abban az esetben, ha az engedélyezési jogkivonat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="bd900-587">This example shows how to return a 401 response if the authorization token is invalid.</span></span> <span data-ttu-id="bd900-588">További információkért lásd: [külső szolgáltatások az Azure API Management szolgáltatás használata](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="bd900-588">For more information, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-589">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-589">Elements</span></span>  
  
|<span data-ttu-id="bd900-590">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-590">Element</span></span>|<span data-ttu-id="bd900-591">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-591">Description</span></span>|<span data-ttu-id="bd900-592">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-593">állapot beállítása</span><span class="sxs-lookup"><span data-stu-id="bd900-593">set-status</span></span>|<span data-ttu-id="bd900-594">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-594">Root element.</span></span>|<span data-ttu-id="bd900-595">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-596">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-596">Attributes</span></span>  
  
|<span data-ttu-id="bd900-597">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-597">Attribute</span></span>|<span data-ttu-id="bd900-598">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-598">Description</span></span>|<span data-ttu-id="bd900-599">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-599">Required</span></span>|<span data-ttu-id="bd900-600">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-601">kód = "integer"</span><span class="sxs-lookup"><span data-stu-id="bd900-601">code="integer"</span></span>|<span data-ttu-id="bd900-602">A HTTP-állapotkód való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="bd900-602">The HTTP status code to return.</span></span>|<span data-ttu-id="bd900-603">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-603">Yes</span></span>|<span data-ttu-id="bd900-604">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-604">N/A</span></span>|  
|<span data-ttu-id="bd900-605">oka = "karakterlánc"</span><span class="sxs-lookup"><span data-stu-id="bd900-605">reason="string"</span></span>|<span data-ttu-id="bd900-606">Az az oka az állapotkód: visszaadó leírását.</span><span class="sxs-lookup"><span data-stu-id="bd900-606">A description of the reason for returning the status code.</span></span>|<span data-ttu-id="bd900-607">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-607">Yes</span></span>|<span data-ttu-id="bd900-608">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-609">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-609">Usage</span></span>  
 <span data-ttu-id="bd900-610">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-610">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-611">**Házirend szakaszok:** kimenő, háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-612">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="bd900-613"><a name="set-variable"></a>Új érték</span><span class="sxs-lookup"><span data-stu-id="bd900-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="bd900-614">A `set-variable` házirend deklarál egy [környezetben](api-management-policy-expressions.md#ContextVariables) változó, és egy megadott értéket rendeli az [kifejezés](api-management-policy-expressions.md) vagy egy szöveges karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bd900-614">The `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="bd900-615">Ha a kifejezés olyan szövegkonstanst tartalmaz karakterláncra lesz konvertálva, és az érték típusa lesz `System.String`.</span><span class="sxs-lookup"><span data-stu-id="bd900-615">if the expression contains a literal it will be converted to a string and the type of the value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="bd900-616"><a name="set-variablePolicyStatement"></a>Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="bd900-617"><a name="set-variableExample"></a>Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="bd900-618">A következő példa bemutatja a változó szabály beállítása a bejövő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="bd900-618">The following example demonstrates a set variable policy in the inbound section.</span></span> <span data-ttu-id="bd900-619">A set változó házirend hoz létre egy `isMobile` logikai [környezetben](api-management-policy-expressions.md#ContextVariables) változó értéke igaz, ha a `User-Agent` kérelem fejléc tartalmazza a szöveg `iPad` vagy `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="bd900-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-620">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-620">Elements</span></span>  
  
|<span data-ttu-id="bd900-621">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-621">Element</span></span>|<span data-ttu-id="bd900-622">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-622">Description</span></span>|<span data-ttu-id="bd900-623">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-624">Set-változó</span><span class="sxs-lookup"><span data-stu-id="bd900-624">set-variable</span></span>|<span data-ttu-id="bd900-625">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-625">Root element.</span></span>|<span data-ttu-id="bd900-626">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-627">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-627">Attributes</span></span>  
  
|<span data-ttu-id="bd900-628">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-628">Attribute</span></span>|<span data-ttu-id="bd900-629">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-629">Description</span></span>|<span data-ttu-id="bd900-630">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="bd900-631">név</span><span class="sxs-lookup"><span data-stu-id="bd900-631">name</span></span>|<span data-ttu-id="bd900-632">Annak a változónak a nevét.</span><span class="sxs-lookup"><span data-stu-id="bd900-632">The name of the variable.</span></span>|<span data-ttu-id="bd900-633">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-633">Yes</span></span>|  
|<span data-ttu-id="bd900-634">érték</span><span class="sxs-lookup"><span data-stu-id="bd900-634">value</span></span>|<span data-ttu-id="bd900-635">A változó értékét.</span><span class="sxs-lookup"><span data-stu-id="bd900-635">The value of the variable.</span></span> <span data-ttu-id="bd900-636">Ez lehet egy kifejezést, vagy konstansérték.</span><span class="sxs-lookup"><span data-stu-id="bd900-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="bd900-637">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-638">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-638">Usage</span></span>  
 <span data-ttu-id="bd900-639">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-639">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-640">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-641">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="bd900-642"><a name="set-variableAllowedTypes"></a>Engedélyezett típusokkal</span><span class="sxs-lookup"><span data-stu-id="bd900-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="bd900-643">A használt kifejezések a `set-variable` házirendet a következő alapvető típusok valamelyikét kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="bd900-643">Expressions used in the `set-variable` policy must return one of the following basic types.</span></span>  
  
-   <span data-ttu-id="bd900-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="bd900-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="bd900-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="bd900-645">System.SByte</span></span>  
  
-   <span data-ttu-id="bd900-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="bd900-646">System.Byte</span></span>  
  
-   <span data-ttu-id="bd900-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="bd900-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="bd900-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="bd900-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="bd900-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="bd900-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="bd900-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="bd900-650">System.Int16</span></span>  
  
-   <span data-ttu-id="bd900-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="bd900-651">System.Int32</span></span>  
  
-   <span data-ttu-id="bd900-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="bd900-652">System.Int64</span></span>  
  
-   <span data-ttu-id="bd900-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="bd900-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="bd900-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="bd900-654">System.Single</span></span>  
  
-   <span data-ttu-id="bd900-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="bd900-655">System.Double</span></span>  
  
-   <span data-ttu-id="bd900-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="bd900-656">System.Guid</span></span>  
  
-   <span data-ttu-id="bd900-657">System.String</span><span class="sxs-lookup"><span data-stu-id="bd900-657">System.String</span></span>  
  
-   <span data-ttu-id="bd900-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="bd900-658">System.Char</span></span>  
  
-   <span data-ttu-id="bd900-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="bd900-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="bd900-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="bd900-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="bd900-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="bd900-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="bd900-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="bd900-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="bd900-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="bd900-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="bd900-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="bd900-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="bd900-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="bd900-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="bd900-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="bd900-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="bd900-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="bd900-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="bd900-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="bd900-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="bd900-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="bd900-669">System.Single?</span></span>  
  
-   <span data-ttu-id="bd900-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="bd900-670">System.Double?</span></span>  
  
-   <span data-ttu-id="bd900-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="bd900-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="bd900-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="bd900-672">System.String?</span></span>  
  
-   <span data-ttu-id="bd900-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="bd900-673">System.Char?</span></span>  
  
-   <span data-ttu-id="bd900-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="bd900-674">System.DateTime?</span></span>  

##  <span data-ttu-id="bd900-675"><a name="Trace"></a>Nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="bd900-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="bd900-676">A `trace` házirend hozzáadja a karakterlánc a [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) kimeneti.</span><span class="sxs-lookup"><span data-stu-id="bd900-676">The             `trace` policy adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="bd900-677">A házirend hajtja végre, csak ha nyomkövetés akkor váltódik ki, azaz `Ocp-Apim-Trace` fejléc jelen, és állítsa be a `true` és `Ocp-Apim-Subscription-Key` fejléc szerepel, valamint a rendszergazdai fiókhoz társított érvényes kulcs.</span><span class="sxs-lookup"><span data-stu-id="bd900-677">The policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set to `true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with the admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-678">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="bd900-679">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-679">Elements</span></span>  
  
|<span data-ttu-id="bd900-680">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-680">Element</span></span>|<span data-ttu-id="bd900-681">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-681">Description</span></span>|<span data-ttu-id="bd900-682">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-683">Nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="bd900-683">trace</span></span>|<span data-ttu-id="bd900-684">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-684">Root element.</span></span>|<span data-ttu-id="bd900-685">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-686">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-686">Attributes</span></span>  
  
|<span data-ttu-id="bd900-687">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-687">Attribute</span></span>|<span data-ttu-id="bd900-688">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-688">Description</span></span>|<span data-ttu-id="bd900-689">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-689">Required</span></span>|<span data-ttu-id="bd900-690">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-691">Forrás</span><span class="sxs-lookup"><span data-stu-id="bd900-691">source</span></span>|<span data-ttu-id="bd900-692">A literál karakterlánc kifejező a trace viewer, és adja meg az üzenet forrása.</span><span class="sxs-lookup"><span data-stu-id="bd900-692">String literal meaningful to the trace viewer and specifying the source of the message.</span></span>|<span data-ttu-id="bd900-693">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-693">Yes</span></span>|<span data-ttu-id="bd900-694">N/A</span><span class="sxs-lookup"><span data-stu-id="bd900-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-695">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-695">Usage</span></span>  
 <span data-ttu-id="bd900-696">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="bd900-696">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="bd900-697">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="bd900-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="bd900-698">**Házirend hatókörök:** minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="bd900-699"><a name="Wait"></a>várj</span><span class="sxs-lookup"><span data-stu-id="bd900-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="bd900-700">A `wait` házirend hajt végre annak közvetlenül alárendelt házirendjei párhuzamosan, és megvárja-e az összes vagy annak befejezését, mielőtt ő maga befejeződne közvetlenül alárendelt házirendek egyikét.</span><span class="sxs-lookup"><span data-stu-id="bd900-700">The `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies to complete before it completes.</span></span> <span data-ttu-id="bd900-701">A várakozási házirend lehetnek azonnali gyermek házirendjeit [küldési kérelmek](api-management-advanced-policies.md#SendRequest), [lehet értéket kiolvasni a gyorsítótár](api-management-caching-policies.md#GetFromCacheByKey), és [folyamatot szabályozhatja](api-management-advanced-policies.md#choose) házirendek.</span><span class="sxs-lookup"><span data-stu-id="bd900-701">The wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="bd900-702">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="bd900-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="bd900-703">Példa</span><span class="sxs-lookup"><span data-stu-id="bd900-703">Example</span></span>  
 <span data-ttu-id="bd900-704">A következő példa kétféle `choose` házirendek közvetlenül alárendelt házirendek, mint a `wait` házirend.</span><span class="sxs-lookup"><span data-stu-id="bd900-704">In the following example there are two `choose` policies as immediate child policies of the `wait` policy.</span></span> <span data-ttu-id="bd900-705">Ezek mindegyikének `choose` házirendek hajt végre párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="bd900-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="bd900-706">Minden egyes `choose` házirend megpróbálja gyorsítótárazott értéke.</span><span class="sxs-lookup"><span data-stu-id="bd900-706">Each `choose` policy attempts to retrieve a cached value.</span></span> <span data-ttu-id="bd900-707">Ha a gyorsítótár-tévesztései, háttérszolgáltatás nevezik adni az értékét.</span><span class="sxs-lookup"><span data-stu-id="bd900-707">If there is a cache miss, a backend service is called to provide the value.</span></span> <span data-ttu-id="bd900-708">Ebben a példában a `wait` házirend végrehajtásához összes közvetlenül alárendelt házirendjeit végrehajtani, mert a `for` attribútum van beállítva `all`.</span><span class="sxs-lookup"><span data-stu-id="bd900-708">In this example the `wait` policy does not complete until all of its immediate child policies complete, because the `for` attribute is set to `all`.</span></span>   <span data-ttu-id="bd900-709">Ebben a példában a környezeti változók (`execute-branch-one`, `value-one`, `execute-branch-two`, és `value-two`) Ez a példa házirend kereteit deklarált.</span><span class="sxs-lookup"><span data-stu-id="bd900-709">In this example the context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of the scope of this example policy.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="bd900-710">Elemek</span><span class="sxs-lookup"><span data-stu-id="bd900-710">Elements</span></span>  
  
|<span data-ttu-id="bd900-711">Elem</span><span class="sxs-lookup"><span data-stu-id="bd900-711">Element</span></span>|<span data-ttu-id="bd900-712">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-712">Description</span></span>|<span data-ttu-id="bd900-713">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="bd900-714">várj</span><span class="sxs-lookup"><span data-stu-id="bd900-714">wait</span></span>|<span data-ttu-id="bd900-715">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="bd900-715">Root element.</span></span> <span data-ttu-id="bd900-716">Tartalmazhat, mint a gyermekelemek csak `send-request`, `cache-lookup-value`, és `choose` házirendek.</span><span class="sxs-lookup"><span data-stu-id="bd900-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="bd900-717">Igen</span><span class="sxs-lookup"><span data-stu-id="bd900-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="bd900-718">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="bd900-718">Attributes</span></span>  
  
|<span data-ttu-id="bd900-719">Attribútum</span><span class="sxs-lookup"><span data-stu-id="bd900-719">Attribute</span></span>|<span data-ttu-id="bd900-720">Leírás</span><span class="sxs-lookup"><span data-stu-id="bd900-720">Description</span></span>|<span data-ttu-id="bd900-721">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bd900-721">Required</span></span>|<span data-ttu-id="bd900-722">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="bd900-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="bd900-723">A</span><span class="sxs-lookup"><span data-stu-id="bd900-723">for</span></span>|<span data-ttu-id="bd900-724">Meghatározza, hogy a `wait` házirend megvárja-e a minden közvetlenül alárendelt házirendek befejezett vagy egyszerűen lesz.</span><span class="sxs-lookup"><span data-stu-id="bd900-724">Determines whether the `wait` policy waits for all immediate child policies to be completed or just one.</span></span> <span data-ttu-id="bd900-725">Engedélyezett értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="bd900-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="bd900-726">-   `all`-Várjon, amíg befejeződik az összes közvetlenül alárendelt házirendek</span><span class="sxs-lookup"><span data-stu-id="bd900-726">-   `all` - wait for all immediate child policies to complete</span></span><br /><span data-ttu-id="bd900-727">-a - Várjon, amíg egy-egy közvetlen alárendelt házirend befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bd900-727">-   any - wait for any immediate child policy to complete.</span></span> <span data-ttu-id="bd900-728">Az első közvetlenül alárendelt házirend befejezése után a `wait` házirend befejeződött, és bármely más közvetlenül alárendelt házirendek végrehajtása megszakadt.</span><span class="sxs-lookup"><span data-stu-id="bd900-728">Once the first immediate child policy has completed, the `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="bd900-729">Nem</span><span class="sxs-lookup"><span data-stu-id="bd900-729">No</span></span>|<span data-ttu-id="bd900-730">Minden</span><span class="sxs-lookup"><span data-stu-id="bd900-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="bd900-731">Használat</span><span class="sxs-lookup"><span data-stu-id="bd900-731">Usage</span></span>  
 <span data-ttu-id="bd900-732">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="bd900-732">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="bd900-733">**Házirend szakaszok:** bejövő, kimenő háttér</span><span class="sxs-lookup"><span data-stu-id="bd900-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="bd900-734">**Házirend hatókörök:**minden hatókör</span><span class="sxs-lookup"><span data-stu-id="bd900-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="bd900-735">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd900-735">Next steps</span></span>
<span data-ttu-id="bd900-736">Házirendek használata további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="bd900-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="bd900-737">Az API Management házirendek</span><span class="sxs-lookup"><span data-stu-id="bd900-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="bd900-738">Házirend-kifejezések</span><span class="sxs-lookup"><span data-stu-id="bd900-738">Policy expressions</span></span>](api-management-policy-expressions.md)
