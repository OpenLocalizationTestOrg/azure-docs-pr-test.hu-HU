---
title: "Gyorsítótárazás házirendek az Azure API Management |} Microsoft Docs"
description: "A gyorsítótárazási házirend használható az Azure API Management megismerése."
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
ms.openlocfilehash: 2a8f012e7e223ef5c1683c8a6c5ecf2f3e96bed8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-caching-policies"></a><span data-ttu-id="0ce52-103">Az API Management gyorsítótárazási házirendek</span><span class="sxs-lookup"><span data-stu-id="0ce52-103">API Management caching policies</span></span>
<span data-ttu-id="0ce52-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="0ce52-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="0ce52-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="0ce52-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="0ce52-106"><a name="CachingPolicies"></a>Házirendek gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="0ce52-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="0ce52-107">A válasz gyorsítótár házirendek</span><span class="sxs-lookup"><span data-stu-id="0ce52-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="0ce52-108">[Gyorsítótár beszerezni](api-management-caching-policies.md#GetFromCache) -hajtsa végre a gyorsítótár kereshet, és ha elérhető egy érvényes gyorsítótárazott választ adjon vissza.</span><span class="sxs-lookup"><span data-stu-id="0ce52-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="0ce52-109">[Gyorsítótárazandó tároló](api-management-caching-policies.md#StoreToCache) -gyorsítótárazza a válaszokat a gyorsítótármappa megadott vezérlő konfigurációjának megfelelően.</span><span class="sxs-lookup"><span data-stu-id="0ce52-109">[Store to cache](api-management-caching-policies.md#StoreToCache) - Caches responses according to the specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="0ce52-110">Házirendek gyorsítótárazás érték</span><span class="sxs-lookup"><span data-stu-id="0ce52-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="0ce52-111">[Lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) -kulcs által gyorsítótárazott elem beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0ce52-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="0ce52-112">[A gyorsítótárban tárolja a érték](#StoreToCacheByKey) -tárolja egy elemet a gyorsítótár gombot.</span><span class="sxs-lookup"><span data-stu-id="0ce52-112">[Store value in cache](#StoreToCacheByKey) - Store an item in the cache by key.</span></span>  
  
    -   <span data-ttu-id="0ce52-113">[Távolítsa el az értéket a gyorsítótárból](#RemoveCacheByKey) -a gyorsítótárban, kulcs által távolítani egy elemet.</span><span class="sxs-lookup"><span data-stu-id="0ce52-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>  
  
##  <span data-ttu-id="0ce52-114"><a name="GetFromCache"></a>Gyorsítótár beszerzése</span><span class="sxs-lookup"><span data-stu-id="0ce52-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="0ce52-115">Használja a `cache-lookup` házirend gyorsítótár végrehajtásához kereshet, és térjen vissza egy érvényes gyorsítótárazott választ, ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="0ce52-115">Use the `cache-lookup` policy to perform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="0ce52-116">Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="0ce52-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="0ce52-117">Válasz gyorsítótárazás csökkenti a sávszélesség, és a háttér web server, és csökkenti a várakozási API fogyasztó érzékelt kivetett szemben támasztott feldolgozási igényeket.</span><span class="sxs-lookup"><span data-stu-id="0ce52-117">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ce52-118">Ez a házirend rendelkeznie kell egy megfelelő [tároló, a gyorsítótár](api-management-caching-policies.md#StoreToCache) házirend.</span><span class="sxs-lookup"><span data-stu-id="0ce52-118">This policy must have a corresponding [Store to cache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ce52-119">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="0ce52-119">Policy statement</span></span>  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
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
  
### <a name="examples"></a><span data-ttu-id="0ce52-120">Példák</span><span class="sxs-lookup"><span data-stu-id="0ce52-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="0ce52-121">Példa</span><span class="sxs-lookup"><span data-stu-id="0ce52-121">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="0ce52-122">Példa házirend-kifejezések használatával</span><span class="sxs-lookup"><span data-stu-id="0ce52-122">Example using policy expressions</span></span>  
 <span data-ttu-id="0ce52-123">Ez a példa bemutatja, hogyan konfigurálhatja az API Management válasz gyorsítótár, amely megfelel a háttérszolgáltatáshoz, mint a válasz gyorsítótárazás időtartama adni a biztonsági másolat szolgáltatás `Cache-Control` direktívát.</span><span class="sxs-lookup"><span data-stu-id="0ce52-123">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="0ce52-124">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és a 25:25 előretekerés.</span><span class="sxs-lookup"><span data-stu-id="0ce52-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="0ce52-125">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="0ce52-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="0ce52-126">Elemek</span><span class="sxs-lookup"><span data-stu-id="0ce52-126">Elements</span></span>  
  
|<span data-ttu-id="0ce52-127">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-127">Name</span></span>|<span data-ttu-id="0ce52-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-128">Description</span></span>|<span data-ttu-id="0ce52-129">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ce52-130">gyorsítótár-keresés</span><span class="sxs-lookup"><span data-stu-id="0ce52-130">cache-lookup</span></span>|<span data-ttu-id="0ce52-131">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="0ce52-131">Root element.</span></span>|<span data-ttu-id="0ce52-132">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-132">Yes</span></span>|  
|<span data-ttu-id="0ce52-133">eltérő-által-fejléc</span><span class="sxs-lookup"><span data-stu-id="0ce52-133">vary-by-header</span></span>|<span data-ttu-id="0ce52-134">Kezdjék el gyorsítótárazni a válaszok száma érték a megadott fejléc, például elfogadás, Accept-Charset, elfogadás-kódolás, elfogadás-nyelv, hitelesítés, a várt, a gazdagép, If-Match.</span><span class="sxs-lookup"><span data-stu-id="0ce52-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="0ce52-135">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-135">No</span></span>|  
|<span data-ttu-id="0ce52-136">eltérő-által-lekérdezési-paraméter</span><span class="sxs-lookup"><span data-stu-id="0ce52-136">vary-by-query-parameter</span></span>|<span data-ttu-id="0ce52-137">Kezdjék el gyorsítótárazni a válaszok száma a megadott lekérdezési paraméterek értékének.</span><span class="sxs-lookup"><span data-stu-id="0ce52-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="0ce52-138">Adjon meg egy vagy több paramétert.</span><span class="sxs-lookup"><span data-stu-id="0ce52-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="0ce52-139">Használjon pontosvessző.</span><span class="sxs-lookup"><span data-stu-id="0ce52-139">Use semicolon as a separator.</span></span> <span data-ttu-id="0ce52-140">Ha nincs megadva, az összes lekérdezési paraméterek használhatók.</span><span class="sxs-lookup"><span data-stu-id="0ce52-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="0ce52-141">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ce52-142">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="0ce52-142">Attributes</span></span>  
  
|<span data-ttu-id="0ce52-143">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-143">Name</span></span>|<span data-ttu-id="0ce52-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-144">Description</span></span>|<span data-ttu-id="0ce52-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-145">Required</span></span>|<span data-ttu-id="0ce52-146">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0ce52-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ce52-147">lehetővé teszik privát-válasz-gyorsítótárazás</span><span class="sxs-lookup"><span data-stu-id="0ce52-147">allow-private-response-caching</span></span>|<span data-ttu-id="0ce52-148">Ha beállítása `true`, lehetővé teszi, hogy egy Authorization fejlécet tartalmazó kérelmek gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="0ce52-148">When set to `true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="0ce52-149">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-149">No</span></span>|<span data-ttu-id="0ce52-150">hamis</span><span class="sxs-lookup"><span data-stu-id="0ce52-150">false</span></span>|  
|<span data-ttu-id="0ce52-151">alsóbb rétegbeli gyorsítótárazás típusa</span><span class="sxs-lookup"><span data-stu-id="0ce52-151">downstream-caching-type</span></span>|<span data-ttu-id="0ce52-152">Ez az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="0ce52-152">This attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ce52-153">-none - alárendelt gyorsítótárazás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0ce52-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="0ce52-154">-titkos - alsóbb rétegbeli személyes engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="0ce52-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="0ce52-155">-nyilvános - titkos és megosztott alárendelt engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="0ce52-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="0ce52-156">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-156">No</span></span>|<span data-ttu-id="0ce52-157">Egyik sem</span><span class="sxs-lookup"><span data-stu-id="0ce52-157">none</span></span>|  
|<span data-ttu-id="0ce52-158">must-revalidate érték</span><span class="sxs-lookup"><span data-stu-id="0ce52-158">must-revalidate</span></span>|<span data-ttu-id="0ce52-159">Ha az alárendelt gyorsítótár engedélyezve van ez az attribútum be- vagy kikapcsolja a `must-revalidate` cache-control direktíva átjáró válaszokban.</span><span class="sxs-lookup"><span data-stu-id="0ce52-159">When downstream caching is enabled this attribute turns on or off  the `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="0ce52-160">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-160">No</span></span>|<span data-ttu-id="0ce52-161">Igaz</span><span class="sxs-lookup"><span data-stu-id="0ce52-161">true</span></span>|  
|<span data-ttu-id="0ce52-162">eltérő-által-fejlesztőknek</span><span class="sxs-lookup"><span data-stu-id="0ce52-162">vary-by-developer</span></span>|<span data-ttu-id="0ce52-163">Beállítása `true` a gyorsítótár-válaszok fejlesztői kulcs száma.</span><span class="sxs-lookup"><span data-stu-id="0ce52-163">Set to `true` to cache responses per developer key.</span></span>|<span data-ttu-id="0ce52-164">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-164">No</span></span>|<span data-ttu-id="0ce52-165">hamis</span><span class="sxs-lookup"><span data-stu-id="0ce52-165">false</span></span>|  
|<span data-ttu-id="0ce52-166">eltérő-által-developer-csoportok</span><span class="sxs-lookup"><span data-stu-id="0ce52-166">vary-by-developer-groups</span></span>|<span data-ttu-id="0ce52-167">Beállítása `true` gyorsítótár-válaszok száma felhasználói szerepkör számára.</span><span class="sxs-lookup"><span data-stu-id="0ce52-167">Set to `true` to cache responses per user role.</span></span>|<span data-ttu-id="0ce52-168">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-168">No</span></span>|<span data-ttu-id="0ce52-169">hamis</span><span class="sxs-lookup"><span data-stu-id="0ce52-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ce52-170">Használat</span><span class="sxs-lookup"><span data-stu-id="0ce52-170">Usage</span></span>  
 <span data-ttu-id="0ce52-171">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ce52-171">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ce52-172">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="0ce52-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="0ce52-173">**Házirend hatókörök:** API, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="0ce52-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="0ce52-174"><a name="StoreToCache"></a>Gyorsítótárazandó tárolásához</span><span class="sxs-lookup"><span data-stu-id="0ce52-174"><a name="StoreToCache"></a> Store to cache</span></span>  
 <span data-ttu-id="0ce52-175">A `cache-store` házirend gyorsítótárazza a választ a gyorsítótármappa megadott beállítások szerint.</span><span class="sxs-lookup"><span data-stu-id="0ce52-175">The `cache-store` policy caches responses according to the specified cache settings.</span></span> <span data-ttu-id="0ce52-176">Ez a házirend azokban az esetekben, ahol válasz tartalma statikus maradjon egy meghatározott időtartamra vonatkozóan lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="0ce52-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="0ce52-177">Válasz gyorsítótárazás csökkenti a sávszélesség, és a háttér web server, és csökkenti a várakozási API fogyasztó érzékelt kivetett szemben támasztott feldolgozási igényeket.</span><span class="sxs-lookup"><span data-stu-id="0ce52-177">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ce52-178">Ez a házirend rendelkeznie kell egy megfelelő [lekérni a gyorsítótár](api-management-caching-policies.md#GetFromCache) házirend.</span><span class="sxs-lookup"><span data-stu-id="0ce52-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ce52-179">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="0ce52-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="0ce52-180">Példák</span><span class="sxs-lookup"><span data-stu-id="0ce52-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="0ce52-181">Példa</span><span class="sxs-lookup"><span data-stu-id="0ce52-181">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="0ce52-182">Példa házirend-kifejezések használatával</span><span class="sxs-lookup"><span data-stu-id="0ce52-182">Example using policy expressions</span></span>  
 <span data-ttu-id="0ce52-183">Ez a példa bemutatja, hogyan konfigurálhatja az API Management válasz gyorsítótár, amely megfelel a háttérszolgáltatáshoz, mint a válasz gyorsítótárazás időtartama adni a biztonsági másolat szolgáltatás `Cache-Control` direktívát.</span><span class="sxs-lookup"><span data-stu-id="0ce52-183">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="0ce52-184">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és a 25:25 előretekerés.</span><span class="sxs-lookup"><span data-stu-id="0ce52-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="0ce52-185">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="0ce52-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="0ce52-186">Elemek</span><span class="sxs-lookup"><span data-stu-id="0ce52-186">Elements</span></span>  
  
|<span data-ttu-id="0ce52-187">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-187">Name</span></span>|<span data-ttu-id="0ce52-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-188">Description</span></span>|<span data-ttu-id="0ce52-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ce52-190">gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="0ce52-190">cache-store</span></span>|<span data-ttu-id="0ce52-191">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="0ce52-191">Root element.</span></span>|<span data-ttu-id="0ce52-192">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ce52-193">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="0ce52-193">Attributes</span></span>  
  
|<span data-ttu-id="0ce52-194">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-194">Name</span></span>|<span data-ttu-id="0ce52-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-195">Description</span></span>|<span data-ttu-id="0ce52-196">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-196">Required</span></span>|<span data-ttu-id="0ce52-197">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0ce52-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ce52-198">Időtartam</span><span class="sxs-lookup"><span data-stu-id="0ce52-198">duration</span></span>|<span data-ttu-id="0ce52-199">Másodpercben megadott idő élettartamát a gyorsítótárazott bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="0ce52-199">Time-to-live of the cached entries, specified in seconds.</span></span>|<span data-ttu-id="0ce52-200">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-200">Yes</span></span>|<span data-ttu-id="0ce52-201">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ce52-202">Használat</span><span class="sxs-lookup"><span data-stu-id="0ce52-202">Usage</span></span>  
 <span data-ttu-id="0ce52-203">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ce52-203">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ce52-204">**Házirend szakaszok:** kimenő</span><span class="sxs-lookup"><span data-stu-id="0ce52-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="0ce52-205">**Házirend hatókörök:** API, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="0ce52-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="0ce52-206"><a name="GetFromCacheByKey"></a>Lehet értéket kiolvasni a gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="0ce52-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="0ce52-207">Használja a `cache-lookup-value` házirend gyorsítótár keresést végrehajtani a gombot, és a gyorsítótárazott érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="0ce52-207">Use the `cache-lookup-value` policy to perform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="0ce52-208">A kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="0ce52-208">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ce52-209">Ez a házirend rendelkeznie kell egy megfelelő [érték tárolása gyorsítótár](#StoreToCacheByKey) házirend.</span><span class="sxs-lookup"><span data-stu-id="0ce52-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ce52-210">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="0ce52-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="0ce52-211">Példa</span><span class="sxs-lookup"><span data-stu-id="0ce52-211">Example</span></span>  
 <span data-ttu-id="0ce52-212">További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="0ce52-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="0ce52-213">Elemek</span><span class="sxs-lookup"><span data-stu-id="0ce52-213">Elements</span></span>  
  
|<span data-ttu-id="0ce52-214">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-214">Name</span></span>|<span data-ttu-id="0ce52-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-215">Description</span></span>|<span data-ttu-id="0ce52-216">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ce52-217">gyorsítótár-keresési-értéke</span><span class="sxs-lookup"><span data-stu-id="0ce52-217">cache-lookup-value</span></span>|<span data-ttu-id="0ce52-218">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="0ce52-218">Root element.</span></span>|<span data-ttu-id="0ce52-219">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ce52-220">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="0ce52-220">Attributes</span></span>  
  
|<span data-ttu-id="0ce52-221">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-221">Name</span></span>|<span data-ttu-id="0ce52-222">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-222">Description</span></span>|<span data-ttu-id="0ce52-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-223">Required</span></span>|<span data-ttu-id="0ce52-224">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0ce52-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ce52-225">alapértelmezett értékű</span><span class="sxs-lookup"><span data-stu-id="0ce52-225">default-value</span></span>|<span data-ttu-id="0ce52-226">Egy érték, amely rendeli hozzá a változó, ha a gyorsítótár kulcskeresési tévesztés eredményezett.</span><span class="sxs-lookup"><span data-stu-id="0ce52-226">A value that will be assigned to the variable if the cache key lookup resulted in a miss.</span></span> <span data-ttu-id="0ce52-227">Ha ez az attribútum nincs megadva, `null` hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="0ce52-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="0ce52-228">Nem</span><span class="sxs-lookup"><span data-stu-id="0ce52-228">No</span></span>|`null`|  
|<span data-ttu-id="0ce52-229">kulcs</span><span class="sxs-lookup"><span data-stu-id="0ce52-229">key</span></span>|<span data-ttu-id="0ce52-230">Gyorsítótár kulcsérték a keresés használatára.</span><span class="sxs-lookup"><span data-stu-id="0ce52-230">Cache key value to use in the lookup.</span></span>|<span data-ttu-id="0ce52-231">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-231">Yes</span></span>|<span data-ttu-id="0ce52-232">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-232">N/A</span></span>|  
|<span data-ttu-id="0ce52-233">változó-neve</span><span class="sxs-lookup"><span data-stu-id="0ce52-233">variable-name</span></span>|<span data-ttu-id="0ce52-234">Neve a [környezeti változó](api-management-policy-expressions.md#ContextVariables) a looked be értéket rendeli hozzá, ha a keresés sikeres.</span><span class="sxs-lookup"><span data-stu-id="0ce52-234">Name of the [context variable](api-management-policy-expressions.md#ContextVariables) the looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="0ce52-235">Keresési tévesztés eredményez, ha a változó kap-e értékét a `default-value` attribútum vagy `null`, ha a `default-value` attribútum hiányzik.</span><span class="sxs-lookup"><span data-stu-id="0ce52-235">If lookup results in a miss, the variable will be assigned the value of the `default-value` attribute or `null`, if the `default-value` attribute is omitted.</span></span>|<span data-ttu-id="0ce52-236">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-236">Yes</span></span>|<span data-ttu-id="0ce52-237">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ce52-238">Használat</span><span class="sxs-lookup"><span data-stu-id="0ce52-238">Usage</span></span>  
 <span data-ttu-id="0ce52-239">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ce52-239">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ce52-240">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="0ce52-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="0ce52-241">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="0ce52-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="0ce52-242"><a name="StoreToCacheByKey"></a>A gyorsítótárban tárolja a érték</span><span class="sxs-lookup"><span data-stu-id="0ce52-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="0ce52-243">A `cache-store-value` hajtja végre a gyorsítótár tárolási gombot.</span><span class="sxs-lookup"><span data-stu-id="0ce52-243">The `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="0ce52-244">A kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="0ce52-244">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ce52-245">Ez a házirend rendelkeznie kell egy megfelelő [lehet értéket kiolvasni a gyorsítótár](#GetFromCacheByKey) házirend.</span><span class="sxs-lookup"><span data-stu-id="0ce52-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ce52-246">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="0ce52-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="0ce52-247">Példa</span><span class="sxs-lookup"><span data-stu-id="0ce52-247">Example</span></span>  
 <span data-ttu-id="0ce52-248">További tudnivalók és példák ezt a házirendet, lásd: [egyéni gyorsítótárazása az Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="0ce52-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="0ce52-249">Elemek</span><span class="sxs-lookup"><span data-stu-id="0ce52-249">Elements</span></span>  
  
|<span data-ttu-id="0ce52-250">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-250">Name</span></span>|<span data-ttu-id="0ce52-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-251">Description</span></span>|<span data-ttu-id="0ce52-252">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ce52-253">gyorsítótár-tároló-értéke</span><span class="sxs-lookup"><span data-stu-id="0ce52-253">cache-store-value</span></span>|<span data-ttu-id="0ce52-254">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="0ce52-254">Root element.</span></span>|<span data-ttu-id="0ce52-255">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ce52-256">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="0ce52-256">Attributes</span></span>  
  
|<span data-ttu-id="0ce52-257">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-257">Name</span></span>|<span data-ttu-id="0ce52-258">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-258">Description</span></span>|<span data-ttu-id="0ce52-259">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-259">Required</span></span>|<span data-ttu-id="0ce52-260">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0ce52-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ce52-261">Időtartam</span><span class="sxs-lookup"><span data-stu-id="0ce52-261">duration</span></span>|<span data-ttu-id="0ce52-262">Érték a gyorsítótárba fognak kerülni a megadott típusú érték, másodpercben megadva.</span><span class="sxs-lookup"><span data-stu-id="0ce52-262">Value will be cached for the provided duration value, specified in seconds.</span></span>|<span data-ttu-id="0ce52-263">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-263">Yes</span></span>|<span data-ttu-id="0ce52-264">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-264">N/A</span></span>|  
|<span data-ttu-id="0ce52-265">kulcs</span><span class="sxs-lookup"><span data-stu-id="0ce52-265">key</span></span>|<span data-ttu-id="0ce52-266">A gyorsítótár kulcsának értékét tárolja.</span><span class="sxs-lookup"><span data-stu-id="0ce52-266">Cache key the value will be stored under.</span></span>|<span data-ttu-id="0ce52-267">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-267">Yes</span></span>|<span data-ttu-id="0ce52-268">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-268">N/A</span></span>|  
|<span data-ttu-id="0ce52-269">érték</span><span class="sxs-lookup"><span data-stu-id="0ce52-269">value</span></span>|<span data-ttu-id="0ce52-270">A gyorsítótárazható érték.</span><span class="sxs-lookup"><span data-stu-id="0ce52-270">The value to be cached.</span></span>|<span data-ttu-id="0ce52-271">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-271">Yes</span></span>|<span data-ttu-id="0ce52-272">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ce52-273">Használat</span><span class="sxs-lookup"><span data-stu-id="0ce52-273">Usage</span></span>  
 <span data-ttu-id="0ce52-274">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ce52-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ce52-275">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="0ce52-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="0ce52-276">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="0ce52-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="0ce52-277"><a name="RemoveCacheByKey"></a>A gyorsítótárból értékének eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0ce52-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="0ce52-278">A `cache-remove-value` egy gyorsítótárazott elem azonosítja a kulcs törlése.</span><span class="sxs-lookup"><span data-stu-id="0ce52-278">The             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="0ce52-279">A kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="0ce52-279">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="0ce52-280">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="0ce52-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="0ce52-281">Példa</span><span class="sxs-lookup"><span data-stu-id="0ce52-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="0ce52-282">Elemek</span><span class="sxs-lookup"><span data-stu-id="0ce52-282">Elements</span></span>  
  
|<span data-ttu-id="0ce52-283">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-283">Name</span></span>|<span data-ttu-id="0ce52-284">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-284">Description</span></span>|<span data-ttu-id="0ce52-285">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ce52-286">gyorsítótár-remove-értéke</span><span class="sxs-lookup"><span data-stu-id="0ce52-286">cache-remove-value</span></span>|<span data-ttu-id="0ce52-287">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="0ce52-287">Root element.</span></span>|<span data-ttu-id="0ce52-288">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="0ce52-289">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="0ce52-289">Attributes</span></span>  
  
|<span data-ttu-id="0ce52-290">Név</span><span class="sxs-lookup"><span data-stu-id="0ce52-290">Name</span></span>|<span data-ttu-id="0ce52-291">Leírás</span><span class="sxs-lookup"><span data-stu-id="0ce52-291">Description</span></span>|<span data-ttu-id="0ce52-292">Szükséges</span><span class="sxs-lookup"><span data-stu-id="0ce52-292">Required</span></span>|<span data-ttu-id="0ce52-293">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0ce52-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ce52-294">kulcs</span><span class="sxs-lookup"><span data-stu-id="0ce52-294">key</span></span>|<span data-ttu-id="0ce52-295">A korábban gyorsítótárazott érték eltávolítsa őket a gyorsítótár kulcsának.</span><span class="sxs-lookup"><span data-stu-id="0ce52-295">The key of the previously cached value to be removed from the cache.</span></span>|<span data-ttu-id="0ce52-296">Igen</span><span class="sxs-lookup"><span data-stu-id="0ce52-296">Yes</span></span>|<span data-ttu-id="0ce52-297">N/A</span><span class="sxs-lookup"><span data-stu-id="0ce52-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="0ce52-298">Használat</span><span class="sxs-lookup"><span data-stu-id="0ce52-298">Usage</span></span>  
 <span data-ttu-id="0ce52-299">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="0ce52-299">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="0ce52-300">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="0ce52-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="0ce52-301">**Házirend hatókörök:** globális, API-t, a művelet, a termék</span><span class="sxs-lookup"><span data-stu-id="0ce52-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="0ce52-302">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ce52-302">Next steps</span></span>
<span data-ttu-id="0ce52-303">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="0ce52-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  