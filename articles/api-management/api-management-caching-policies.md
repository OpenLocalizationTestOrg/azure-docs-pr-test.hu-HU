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
# <a name="api-management-caching-policies"></a><span data-ttu-id="8b9d1-103">Az API Management gyorsítótárazási házirendek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-103">API Management caching policies</span></span>
<span data-ttu-id="8b9d1-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="8b9d1-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="8b9d1-106"><a name="CachingPolicies"></a>Házirendek gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="8b9d1-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="8b9d1-107">A válasz gyorsítótár házirendek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="8b9d1-108">[Gyorsítótár beszerezni](api-management-caching-policies.md#GetFromCache) -hajtsa végre a gyorsítótár kereshet, és ha elérhető egy érvényes gyorsítótárazott választ adjon vissza.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="8b9d1-109">[Tárolja a toocache](api-management-caching-policies.md#StoreToCache) - gyorsítótárak válaszok szerint toohello megadott gyorsítótár-vezérlő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-109">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches responses according toohello specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="8b9d1-110">Házirendek gyorsítótárazás érték</span><span class="sxs-lookup"><span data-stu-id="8b9d1-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="8b9d1-111">[Lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="8b9d1-112">[A gyorsítótárban tárolja a érték](#StoreToCacheByKey) -elem tárolása hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-112">[Store value in cache](#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="8b9d1-113">[Távolítsa el az értéket a gyorsítótárból](#RemoveCacheByKey) -elem eltávolításához hello gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
##  <span data-ttu-id="8b9d1-114"><a name="GetFromCache"></a>Gyorsítótár beszerzése</span><span class="sxs-lookup"><span data-stu-id="8b9d1-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="8b9d1-115">Használjon hello `cache-lookup` házirend tooperform gyorsítótár kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-115">Use hello `cache-lookup` policy tooperform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="8b9d1-116">Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="8b9d1-117">Válasz gyorsítótárazás csökkenti a sávszélesség, és szemben támasztott feldolgozási igényeket kivetett hello háttér web server, és csökkenti a késés API fogyasztó érzékelt.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-117">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="8b9d1-118">Ez a házirend rendelkeznie kell egy megfelelő [tároló toocache](api-management-caching-policies.md#StoreToCache) házirend.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-118">This policy must have a corresponding [Store toocache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8b9d1-119">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-119">Policy statement</span></span>  
  
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
  
### <a name="examples"></a><span data-ttu-id="8b9d1-120">Példák</span><span class="sxs-lookup"><span data-stu-id="8b9d1-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="8b9d1-121">Példa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-121">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="8b9d1-122">Példa házirend-kifejezések használatával</span><span class="sxs-lookup"><span data-stu-id="8b9d1-122">Example using policy expressions</span></span>  
 <span data-ttu-id="8b9d1-123">Ez a példa bemutatja, hogyan tootooconfigure API Management válasz gyorsítótár időtartama, hogy megfelel hello válasz gyorsítótárazását hello háttérszolgáltatást hello által meghatározott biztonsági szolgáltatás `Cache-Control` direktívát.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-123">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="8b9d1-124">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too25:25 előre.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
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
  
 <span data-ttu-id="8b9d1-125">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="8b9d1-126">Elemek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-126">Elements</span></span>  
  
|<span data-ttu-id="8b9d1-127">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-127">Name</span></span>|<span data-ttu-id="8b9d1-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-128">Description</span></span>|<span data-ttu-id="8b9d1-129">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8b9d1-130">gyorsítótár-keresés</span><span class="sxs-lookup"><span data-stu-id="8b9d1-130">cache-lookup</span></span>|<span data-ttu-id="8b9d1-131">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-131">Root element.</span></span>|<span data-ttu-id="8b9d1-132">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-132">Yes</span></span>|  
|<span data-ttu-id="8b9d1-133">eltérő-által-fejléc</span><span class="sxs-lookup"><span data-stu-id="8b9d1-133">vary-by-header</span></span>|<span data-ttu-id="8b9d1-134">Kezdjék el gyorsítótárazni a válaszok száma érték a megadott fejléc, például elfogadás, Accept-Charset, elfogadás-kódolás, elfogadás-nyelv, hitelesítés, a várt, a gazdagép, If-Match.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="8b9d1-135">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-135">No</span></span>|  
|<span data-ttu-id="8b9d1-136">eltérő-által-lekérdezési-paraméter</span><span class="sxs-lookup"><span data-stu-id="8b9d1-136">vary-by-query-parameter</span></span>|<span data-ttu-id="8b9d1-137">Kezdjék el gyorsítótárazni a válaszok száma a megadott lekérdezési paraméterek értékének.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="8b9d1-138">Adjon meg egy vagy több paramétert.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="8b9d1-139">Használjon pontosvessző.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-139">Use semicolon as a separator.</span></span> <span data-ttu-id="8b9d1-140">Ha nincs megadva, az összes lekérdezési paraméterek használhatók.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="8b9d1-141">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8b9d1-142">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-142">Attributes</span></span>  
  
|<span data-ttu-id="8b9d1-143">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-143">Name</span></span>|<span data-ttu-id="8b9d1-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-144">Description</span></span>|<span data-ttu-id="8b9d1-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-145">Required</span></span>|<span data-ttu-id="8b9d1-146">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8b9d1-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8b9d1-147">lehetővé teszik privát-válasz-gyorsítótárazás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-147">allow-private-response-caching</span></span>|<span data-ttu-id="8b9d1-148">Ha értéke túl`true`, lehetővé teszi, hogy egy Authorization fejlécet tartalmazó kérelmek gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-148">When set too`true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="8b9d1-149">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-149">No</span></span>|<span data-ttu-id="8b9d1-150">hamis</span><span class="sxs-lookup"><span data-stu-id="8b9d1-150">false</span></span>|  
|<span data-ttu-id="8b9d1-151">alsóbb rétegbeli gyorsítótárazás típusa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-151">downstream-caching-type</span></span>|<span data-ttu-id="8b9d1-152">Ez az attribútum a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-152">This attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="8b9d1-153">-none - alárendelt gyorsítótárazás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="8b9d1-154">-titkos - alsóbb rétegbeli személyes engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="8b9d1-155">-nyilvános - titkos és megosztott alárendelt engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="8b9d1-156">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-156">No</span></span>|<span data-ttu-id="8b9d1-157">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-157">none</span></span>|  
|<span data-ttu-id="8b9d1-158">must-revalidate érték</span><span class="sxs-lookup"><span data-stu-id="8b9d1-158">must-revalidate</span></span>|<span data-ttu-id="8b9d1-159">Ha az alárendelt gyorsítótár engedélyezve van ez az attribútum a- vagy kikapcsolja a hello `must-revalidate` cache-control direktíva átjáró válaszokban.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-159">When downstream caching is enabled this attribute turns on or off  hello `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="8b9d1-160">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-160">No</span></span>|<span data-ttu-id="8b9d1-161">Igaz</span><span class="sxs-lookup"><span data-stu-id="8b9d1-161">true</span></span>|  
|<span data-ttu-id="8b9d1-162">eltérő-által-fejlesztőknek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-162">vary-by-developer</span></span>|<span data-ttu-id="8b9d1-163">Állítsa be a túl`true` toocache válaszok fejlesztői kulcs száma.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-163">Set too`true` toocache responses per developer key.</span></span>|<span data-ttu-id="8b9d1-164">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-164">No</span></span>|<span data-ttu-id="8b9d1-165">hamis</span><span class="sxs-lookup"><span data-stu-id="8b9d1-165">false</span></span>|  
|<span data-ttu-id="8b9d1-166">eltérő-által-developer-csoportok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-166">vary-by-developer-groups</span></span>|<span data-ttu-id="8b9d1-167">Állítsa be a túl`true` toocache válaszok száma felhasználói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-167">Set too`true` toocache responses per user role.</span></span>|<span data-ttu-id="8b9d1-168">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-168">No</span></span>|<span data-ttu-id="8b9d1-169">hamis</span><span class="sxs-lookup"><span data-stu-id="8b9d1-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8b9d1-170">Használat</span><span class="sxs-lookup"><span data-stu-id="8b9d1-170">Usage</span></span>  
 <span data-ttu-id="8b9d1-171">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-171">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8b9d1-172">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8b9d1-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8b9d1-173">**Házirend hatókörök:** API, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="8b9d1-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="8b9d1-174"><a name="StoreToCache"></a>Tároló toocache</span><span class="sxs-lookup"><span data-stu-id="8b9d1-174"><a name="StoreToCache"></a> Store toocache</span></span>  
 <span data-ttu-id="8b9d1-175">Hello `cache-store` házirend gyorsítótárak válaszok toohello szerint megadott gyorsítótár-beállítások.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-175">hello `cache-store` policy caches responses according toohello specified cache settings.</span></span> <span data-ttu-id="8b9d1-176">Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="8b9d1-177">Válasz gyorsítótárazás csökkenti a sávszélesség, és szemben támasztott feldolgozási igényeket kivetett hello háttér web server, és csökkenti a késés API fogyasztó érzékelt.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-177">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="8b9d1-178">Ez a házirend rendelkeznie kell egy megfelelő [lekérni a gyorsítótár](api-management-caching-policies.md#GetFromCache) házirend.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8b9d1-179">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="8b9d1-180">Példák</span><span class="sxs-lookup"><span data-stu-id="8b9d1-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="8b9d1-181">Példa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-181">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="8b9d1-182">Példa házirend-kifejezések használatával</span><span class="sxs-lookup"><span data-stu-id="8b9d1-182">Example using policy expressions</span></span>  
 <span data-ttu-id="8b9d1-183">Ez a példa bemutatja, hogyan tootooconfigure API Management válasz gyorsítótár időtartama, hogy megfelel hello válasz gyorsítótárazását hello háttérszolgáltatást hello által meghatározott biztonsági szolgáltatás `Cache-Control` direktívát.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-183">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="8b9d1-184">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too25:25 előre.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
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
  
 <span data-ttu-id="8b9d1-185">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="8b9d1-186">Elemek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-186">Elements</span></span>  
  
|<span data-ttu-id="8b9d1-187">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-187">Name</span></span>|<span data-ttu-id="8b9d1-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-188">Description</span></span>|<span data-ttu-id="8b9d1-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8b9d1-190">gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="8b9d1-190">cache-store</span></span>|<span data-ttu-id="8b9d1-191">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-191">Root element.</span></span>|<span data-ttu-id="8b9d1-192">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8b9d1-193">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-193">Attributes</span></span>  
  
|<span data-ttu-id="8b9d1-194">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-194">Name</span></span>|<span data-ttu-id="8b9d1-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-195">Description</span></span>|<span data-ttu-id="8b9d1-196">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-196">Required</span></span>|<span data-ttu-id="8b9d1-197">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8b9d1-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8b9d1-198">Időtartam</span><span class="sxs-lookup"><span data-stu-id="8b9d1-198">duration</span></span>|<span data-ttu-id="8b9d1-199">Bejegyzések, a másodpercben megadott idő élettartamát a hello gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-199">Time-to-live of hello cached entries, specified in seconds.</span></span>|<span data-ttu-id="8b9d1-200">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-200">Yes</span></span>|<span data-ttu-id="8b9d1-201">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8b9d1-202">Használat</span><span class="sxs-lookup"><span data-stu-id="8b9d1-202">Usage</span></span>  
 <span data-ttu-id="8b9d1-203">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-203">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8b9d1-204">**Házirend szakaszok:** kimenő</span><span class="sxs-lookup"><span data-stu-id="8b9d1-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="8b9d1-205">**Házirend hatókörök:** API, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="8b9d1-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="8b9d1-206"><a name="GetFromCacheByKey"></a>Lehet értéket kiolvasni a gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="8b9d1-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="8b9d1-207">Használjon hello `cache-lookup-value` házirend tooperform keresési gyorsítótár gombot, és a gyorsítótárazott érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-207">Use hello `cache-lookup-value` policy tooperform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="8b9d1-208">hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-208">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="8b9d1-209">Ez a házirend rendelkeznie kell egy megfelelő [érték tárolása gyorsítótár](#StoreToCacheByKey) házirend.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8b9d1-210">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="8b9d1-211">Példa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-211">Example</span></span>  
 <span data-ttu-id="8b9d1-212">További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="8b9d1-213">Elemek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-213">Elements</span></span>  
  
|<span data-ttu-id="8b9d1-214">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-214">Name</span></span>|<span data-ttu-id="8b9d1-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-215">Description</span></span>|<span data-ttu-id="8b9d1-216">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8b9d1-217">gyorsítótár-keresési-értéke</span><span class="sxs-lookup"><span data-stu-id="8b9d1-217">cache-lookup-value</span></span>|<span data-ttu-id="8b9d1-218">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-218">Root element.</span></span>|<span data-ttu-id="8b9d1-219">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8b9d1-220">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-220">Attributes</span></span>  
  
|<span data-ttu-id="8b9d1-221">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-221">Name</span></span>|<span data-ttu-id="8b9d1-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-222">Description</span></span>|<span data-ttu-id="8b9d1-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-223">Required</span></span>|<span data-ttu-id="8b9d1-224">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8b9d1-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8b9d1-225">alapértelmezett értékű</span><span class="sxs-lookup"><span data-stu-id="8b9d1-225">default-value</span></span>|<span data-ttu-id="8b9d1-226">Egy érték, amely rendeli toohello változó Ha hello gyorsítótár kulcskeresési tévesztés eredményezett.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-226">A value that will be assigned toohello variable if hello cache key lookup resulted in a miss.</span></span> <span data-ttu-id="8b9d1-227">Ha ez az attribútum nincs megadva, `null` hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="8b9d1-228">Nem</span><span class="sxs-lookup"><span data-stu-id="8b9d1-228">No</span></span>|`null`|  
|<span data-ttu-id="8b9d1-229">kulcs</span><span class="sxs-lookup"><span data-stu-id="8b9d1-229">key</span></span>|<span data-ttu-id="8b9d1-230">Kulcs értéke toouse hello keresési a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-230">Cache key value toouse in hello lookup.</span></span>|<span data-ttu-id="8b9d1-231">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-231">Yes</span></span>|<span data-ttu-id="8b9d1-232">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-232">N/A</span></span>|  
|<span data-ttu-id="8b9d1-233">változó-neve</span><span class="sxs-lookup"><span data-stu-id="8b9d1-233">variable-name</span></span>|<span data-ttu-id="8b9d1-234">Hello neve [környezeti változó](api-management-policy-expressions.md#ContextVariables) keresett érték hello rendeli hozzá, ha a keresés sikeres.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-234">Name of hello [context variable](api-management-policy-expressions.md#ContextVariables) hello looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="8b9d1-235">Ha keresési tévesztés eredményez, hello változó kap-e hello hello értékének `default-value` attribútum vagy `null`, ha hello `default-value` attribútum hiányzik.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-235">If lookup results in a miss, hello variable will be assigned hello value of hello `default-value` attribute or `null`, if hello `default-value` attribute is omitted.</span></span>|<span data-ttu-id="8b9d1-236">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-236">Yes</span></span>|<span data-ttu-id="8b9d1-237">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8b9d1-238">Használat</span><span class="sxs-lookup"><span data-stu-id="8b9d1-238">Usage</span></span>  
 <span data-ttu-id="8b9d1-239">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-239">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8b9d1-240">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="8b9d1-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="8b9d1-241">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="8b9d1-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="8b9d1-242"><a name="StoreToCacheByKey"></a>A gyorsítótárban tárolja a érték</span><span class="sxs-lookup"><span data-stu-id="8b9d1-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="8b9d1-243">Hello `cache-store-value` hajtja végre a gyorsítótár tárolási gombot.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-243">hello `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="8b9d1-244">hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-244">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="8b9d1-245">Ez a házirend rendelkeznie kell egy megfelelő [lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) házirend.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8b9d1-246">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="8b9d1-247">Példa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-247">Example</span></span>  
 <span data-ttu-id="8b9d1-248">További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="8b9d1-249">Elemek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-249">Elements</span></span>  
  
|<span data-ttu-id="8b9d1-250">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-250">Name</span></span>|<span data-ttu-id="8b9d1-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-251">Description</span></span>|<span data-ttu-id="8b9d1-252">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8b9d1-253">gyorsítótár-tároló-értéke</span><span class="sxs-lookup"><span data-stu-id="8b9d1-253">cache-store-value</span></span>|<span data-ttu-id="8b9d1-254">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-254">Root element.</span></span>|<span data-ttu-id="8b9d1-255">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8b9d1-256">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-256">Attributes</span></span>  
  
|<span data-ttu-id="8b9d1-257">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-257">Name</span></span>|<span data-ttu-id="8b9d1-258">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-258">Description</span></span>|<span data-ttu-id="8b9d1-259">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-259">Required</span></span>|<span data-ttu-id="8b9d1-260">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8b9d1-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8b9d1-261">Időtartam</span><span class="sxs-lookup"><span data-stu-id="8b9d1-261">duration</span></span>|<span data-ttu-id="8b9d1-262">Érték a gyorsítótárba fognak kerülni a megadott típusú érték, másodpercben megadva hello.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-262">Value will be cached for hello provided duration value, specified in seconds.</span></span>|<span data-ttu-id="8b9d1-263">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-263">Yes</span></span>|<span data-ttu-id="8b9d1-264">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-264">N/A</span></span>|  
|<span data-ttu-id="8b9d1-265">kulcs</span><span class="sxs-lookup"><span data-stu-id="8b9d1-265">key</span></span>|<span data-ttu-id="8b9d1-266">A gyorsítótár kulcs hello értékét fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-266">Cache key hello value will be stored under.</span></span>|<span data-ttu-id="8b9d1-267">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-267">Yes</span></span>|<span data-ttu-id="8b9d1-268">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-268">N/A</span></span>|  
|<span data-ttu-id="8b9d1-269">érték</span><span class="sxs-lookup"><span data-stu-id="8b9d1-269">value</span></span>|<span data-ttu-id="8b9d1-270">hello érték toobe gyorsítótárazva.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-270">hello value toobe cached.</span></span>|<span data-ttu-id="8b9d1-271">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-271">Yes</span></span>|<span data-ttu-id="8b9d1-272">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8b9d1-273">Használat</span><span class="sxs-lookup"><span data-stu-id="8b9d1-273">Usage</span></span>  
 <span data-ttu-id="8b9d1-274">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8b9d1-275">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="8b9d1-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="8b9d1-276">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="8b9d1-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="8b9d1-277"><a name="RemoveCacheByKey"></a>A gyorsítótárból értékének eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8b9d1-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="8b9d1-278">Hello `cache-remove-value` egy gyorsítótárazott elem azonosítja a kulcs törlése.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-278">hello             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="8b9d1-279">hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-279">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="8b9d1-280">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="8b9d1-281">Példa</span><span class="sxs-lookup"><span data-stu-id="8b9d1-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="8b9d1-282">Elemek</span><span class="sxs-lookup"><span data-stu-id="8b9d1-282">Elements</span></span>  
  
|<span data-ttu-id="8b9d1-283">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-283">Name</span></span>|<span data-ttu-id="8b9d1-284">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-284">Description</span></span>|<span data-ttu-id="8b9d1-285">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8b9d1-286">gyorsítótár-remove-értéke</span><span class="sxs-lookup"><span data-stu-id="8b9d1-286">cache-remove-value</span></span>|<span data-ttu-id="8b9d1-287">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-287">Root element.</span></span>|<span data-ttu-id="8b9d1-288">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="8b9d1-289">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8b9d1-289">Attributes</span></span>  
  
|<span data-ttu-id="8b9d1-290">Név</span><span class="sxs-lookup"><span data-stu-id="8b9d1-290">Name</span></span>|<span data-ttu-id="8b9d1-291">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b9d1-291">Description</span></span>|<span data-ttu-id="8b9d1-292">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b9d1-292">Required</span></span>|<span data-ttu-id="8b9d1-293">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8b9d1-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8b9d1-294">kulcs</span><span class="sxs-lookup"><span data-stu-id="8b9d1-294">key</span></span>|<span data-ttu-id="8b9d1-295">hello hello kulcsa előzőleg gyorsítótárazott érték toobe hello gyorsítótárból eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="8b9d1-295">hello key of hello previously cached value toobe removed from hello cache.</span></span>|<span data-ttu-id="8b9d1-296">Igen</span><span class="sxs-lookup"><span data-stu-id="8b9d1-296">Yes</span></span>|<span data-ttu-id="8b9d1-297">N/A</span><span class="sxs-lookup"><span data-stu-id="8b9d1-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="8b9d1-298">Használat</span><span class="sxs-lookup"><span data-stu-id="8b9d1-298">Usage</span></span>  
 <span data-ttu-id="8b9d1-299">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="8b9d1-299">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="8b9d1-300">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="8b9d1-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="8b9d1-301">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="8b9d1-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="8b9d1-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b9d1-302">Next steps</span></span>
<span data-ttu-id="8b9d1-303">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="8b9d1-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  