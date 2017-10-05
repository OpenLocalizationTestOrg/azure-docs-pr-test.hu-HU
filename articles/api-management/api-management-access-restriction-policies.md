---
title: "A szoftverkorlátozó házirendek az Azure API Management hozzáférés |} Microsoft Docs"
description: "További tudnivalók a hozzáférés szoftverkorlátozó házirendek az Azure API Management használható."
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
ms.openlocfilehash: 4c9991baf3fbcf3b8ea01f8dd573e2336db88b68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="4c811-103">Az API Management hozzáférés szoftverkorlátozó házirendek</span><span class="sxs-lookup"><span data-stu-id="4c811-103">API Management access restriction policies</span></span>
<span data-ttu-id="4c811-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="4c811-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="4c811-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="4c811-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="4c811-106"><a name="AccessRestrictionPolicies"></a>Hozzáférés a szoftverkorlátozó házirendek</span><span class="sxs-lookup"><span data-stu-id="4c811-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="4c811-107">[Ellenőrizze a HTTP-fejléc](api-management-access-restriction-policies.md#CheckHTTPHeader) -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="4c811-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="4c811-108">[Előfizetési határértéket hívás arányt a](api-management-access-restriction-policies.md#LimitCallRate) -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="4c811-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="4c811-109">[Korlát hívás arányt a kulcs](#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="4c811-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="4c811-110">[A hívó IP-címek korlátozása](api-management-access-restriction-policies.md#RestrictCallerIPs) -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="4c811-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="4c811-111">[Set memóriahasználati kvóta előfizetéssel](api-management-access-restriction-policies.md#SetUsageQuota) -lehetővé teszi egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótát, egy előfizetés alapon érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="4c811-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="4c811-112">[Set memóriahasználati kvóta kulcs által](#SetUsageQuotaByKey) -lehetővé teszi egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótát, egy kulcs alapon érvényesítését.</span><span class="sxs-lookup"><span data-stu-id="4c811-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="4c811-113">[Ellenőrizze a JWT](api-management-access-restriction-policies.md#ValidateJWT) -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.</span><span class="sxs-lookup"><span data-stu-id="4c811-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="4c811-114"><a name="CheckHTTPHeader"></a>Ellenőrizze a HTTP-fejléc</span><span class="sxs-lookup"><span data-stu-id="4c811-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="4c811-115">Használja a `check-header` házirend kényszerítéséhez, hogy egy kérelem rendelkezik-e a megadott HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="4c811-115">Use the `check-header` policy to enforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="4c811-116">Meg nem kötelezően ellenőrizze, hogy a fejléc vannak-e egy adott érték vagy az engedélyezett értéktartományon tartomány ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="4c811-116">You can optionally check to see if the header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="4c811-117">Ha az ellenőrzés sikertelen, a HTTP kód és a hiba állapotüzenetet a szabályzat által megadott értéket, és a házirend kérelem feldolgozása leáll.</span><span class="sxs-lookup"><span data-stu-id="4c811-117">If the check fails, the policy terminates request processing and returns the HTTP status code and error message specified by the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-118">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-119">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="4c811-120">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-120">Elements</span></span>  
  
|<span data-ttu-id="4c811-121">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-121">Name</span></span>|<span data-ttu-id="4c811-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-122">Description</span></span>|<span data-ttu-id="4c811-123">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-124">ellenőrzés-fejléc</span><span class="sxs-lookup"><span data-stu-id="4c811-124">check-header</span></span>|<span data-ttu-id="4c811-125">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-125">Root element.</span></span>|<span data-ttu-id="4c811-126">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-126">Yes</span></span>|  
|<span data-ttu-id="4c811-127">érték</span><span class="sxs-lookup"><span data-stu-id="4c811-127">value</span></span>|<span data-ttu-id="4c811-128">Engedélyezett HTTP-fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="4c811-128">Allowed HTTP header value.</span></span> <span data-ttu-id="4c811-129">Ha több érték elem meg van adva, a jelölőnégyzet akkor tekinthető sikeres, ha az értékek közül bármelyik esetben.</span><span class="sxs-lookup"><span data-stu-id="4c811-129">When multiple value elements are specified, the check is considered a success if any one of the values is a match.</span></span>|<span data-ttu-id="4c811-130">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-131">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-131">Attributes</span></span>  
  
|<span data-ttu-id="4c811-132">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-132">Name</span></span>|<span data-ttu-id="4c811-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-133">Description</span></span>|<span data-ttu-id="4c811-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-134">Required</span></span>|<span data-ttu-id="4c811-135">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-136">nem sikerült – jelölőnégyzet-hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="4c811-136">failed-check-error-message</span></span>|<span data-ttu-id="4c811-137">Ha a fejléc nem létezik vagy érvénytelen értéket adja vissza a HTTP-válasz törzsében hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="4c811-137">Error message to return in the HTTP response body if the header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="4c811-138">Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="4c811-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="4c811-139">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-139">Yes</span></span>|<span data-ttu-id="4c811-140">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-140">N/A</span></span>|  
|<span data-ttu-id="4c811-141">nem sikerült – jelölőnégyzet-HTTP-kód</span><span class="sxs-lookup"><span data-stu-id="4c811-141">failed-check-httpcode</span></span>|<span data-ttu-id="4c811-142">HTTP-állapotkód vissza, ha a fejléc nem létezik, vagy érvénytelen értékkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4c811-142">HTTP Status code to return if the header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="4c811-143">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-143">Yes</span></span>|<span data-ttu-id="4c811-144">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-144">N/A</span></span>|  
|<span data-ttu-id="4c811-145">fejléc-neve</span><span class="sxs-lookup"><span data-stu-id="4c811-145">header-name</span></span>|<span data-ttu-id="4c811-146">Ellenőrizze, hogy a HTTP-fejléc nevét.</span><span class="sxs-lookup"><span data-stu-id="4c811-146">The name of the HTTP Header to check.</span></span>|<span data-ttu-id="4c811-147">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-147">Yes</span></span>|<span data-ttu-id="4c811-148">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-148">N/A</span></span>|  
|<span data-ttu-id="4c811-149">esetben figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="4c811-149">ignore-case</span></span>|<span data-ttu-id="4c811-150">Állítható igaz vagy hamis.</span><span class="sxs-lookup"><span data-stu-id="4c811-150">Can be set to True or False.</span></span> <span data-ttu-id="4c811-151">Ha eset igaz értékre állítva a rendszer figyelmen kívül hagyja, ha a fejléc értékének a rendszer összehasonlítja a készlet az elfogadható értéktartományon.</span><span class="sxs-lookup"><span data-stu-id="4c811-151">If set to True case is ignored when the header value is compared against the set of acceptable values.</span></span>|<span data-ttu-id="4c811-152">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-152">Yes</span></span>|<span data-ttu-id="4c811-153">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-154">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-154">Usage</span></span>  
 <span data-ttu-id="4c811-155">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-155">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-156">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="4c811-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="4c811-157">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="4c811-158"><a name="LimitCallRate"></a>Előfizetés hívás arányt a korlát</span><span class="sxs-lookup"><span data-stu-id="4c811-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="4c811-159">A `rate-limit` házirend megakadályozza, hogy egy előfizetés alapon API használati igényeiben jelentkező, ha egy megadott számára egy megadott időszak hívás sebessége korlátozza.</span><span class="sxs-lookup"><span data-stu-id="4c811-159">The `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="4c811-160">Ez a házirend kiváltásakor a hívó kap egy `429 Too Many Requests` válasz állapotkódja.</span><span class="sxs-lookup"><span data-stu-id="4c811-160">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="4c811-161">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="4c811-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="4c811-162">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható a házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="4c811-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-163">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-164">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-164">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4c811-165">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-165">Elements</span></span>  
  
|<span data-ttu-id="4c811-166">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-166">Name</span></span>|<span data-ttu-id="4c811-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-167">Description</span></span>|<span data-ttu-id="4c811-168">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-169">korlát beállítása</span><span class="sxs-lookup"><span data-stu-id="4c811-169">set-limit</span></span>|<span data-ttu-id="4c811-170">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-170">Root element.</span></span>|<span data-ttu-id="4c811-171">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-171">Yes</span></span>|  
|<span data-ttu-id="4c811-172">api-t</span><span class="sxs-lookup"><span data-stu-id="4c811-172">api</span></span>|<span data-ttu-id="4c811-173">Vegyen fel legalább egy hívás sebessége korlátozza az API-k a terméken belüli ezen elemek.</span><span class="sxs-lookup"><span data-stu-id="4c811-173">Add one  or more of these elements to impose a call rate limit on APIs within the product.</span></span> <span data-ttu-id="4c811-174">Termék és API hívása sebesség egymástól függetlenül korlátokat alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="4c811-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="4c811-175">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-175">No</span></span>|  
|<span data-ttu-id="4c811-176">művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-176">operation</span></span>|<span data-ttu-id="4c811-177">Vegyen fel legalább egy műveleten belül az API-k hívása sebessége korlátozza az ezen elemek.</span><span class="sxs-lookup"><span data-stu-id="4c811-177">Add one  or more of these elements to impose a call rate limit on operations within an API.</span></span> <span data-ttu-id="4c811-178">Termék API és művelet hívása sebesség korlátok egymástól függetlenül alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="4c811-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="4c811-179">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-180">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-180">Attributes</span></span>  
  
|<span data-ttu-id="4c811-181">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-181">Name</span></span>|<span data-ttu-id="4c811-182">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-182">Description</span></span>|<span data-ttu-id="4c811-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-183">Required</span></span>|<span data-ttu-id="4c811-184">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-185">név</span><span class="sxs-lookup"><span data-stu-id="4c811-185">name</span></span>|<span data-ttu-id="4c811-186">Az API-t, amelyre alkalmazni a sávszélesség-korlátjának neve.</span><span class="sxs-lookup"><span data-stu-id="4c811-186">The name of the API for which to apply the rate limit.</span></span>|<span data-ttu-id="4c811-187">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-187">Yes</span></span>|<span data-ttu-id="4c811-188">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-188">N/A</span></span>|  
|<span data-ttu-id="4c811-189">hívások</span><span class="sxs-lookup"><span data-stu-id="4c811-189">calls</span></span>|<span data-ttu-id="4c811-190">A megadott időintervallumon engedélyezett hívások maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-190">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-191">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-191">Yes</span></span>|<span data-ttu-id="4c811-192">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-192">N/A</span></span>|  
|<span data-ttu-id="4c811-193">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="4c811-193">renewal-period</span></span>|<span data-ttu-id="4c811-194">Az adott időszakban másodpercben, amely után a kvóta visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="4c811-194">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="4c811-195">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-195">Yes</span></span>|<span data-ttu-id="4c811-196">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-197">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-197">Usage</span></span>  
 <span data-ttu-id="4c811-198">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-198">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-199">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-200">**Házirend hatókörök:** termék</span><span class="sxs-lookup"><span data-stu-id="4c811-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="4c811-201"><a name="LimitCallRateByKey"></a>Korlát hívás arányt a kulcs</span><span class="sxs-lookup"><span data-stu-id="4c811-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="4c811-202">A `rate-limit-by-key` házirend miatt egy kulcs alapon API használati igényeiben jelentkező, ha egy megadott számára egy megadott időszak hívás sebessége korlátozza.</span><span class="sxs-lookup"><span data-stu-id="4c811-202">The `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="4c811-203">A kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="4c811-203">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="4c811-204">Választható növekmény feltétel adhatók meg, hogy mely kérelmek kell számolni, a határérték felé számolnak.</span><span class="sxs-lookup"><span data-stu-id="4c811-204">Optional increment condition can be added to specify which requests should be counted towards the limit.</span></span> <span data-ttu-id="4c811-205">Ez a házirend kiváltásakor a hívó kap egy `429 Too Many Requests` válasz állapotkódja.</span><span class="sxs-lookup"><span data-stu-id="4c811-205">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="4c811-206">További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="4c811-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="4c811-207">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="4c811-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-208">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-209">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-209">Example</span></span>  
 <span data-ttu-id="4c811-210">A következő példában a sávszélesség-korlátjának a hívó IP-cím szerinti kulccsal definiált.</span><span class="sxs-lookup"><span data-stu-id="4c811-210">In the following example, the rate limit is keyed by the caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4c811-211">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-211">Elements</span></span>  
  
|<span data-ttu-id="4c811-212">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-212">Name</span></span>|<span data-ttu-id="4c811-213">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-213">Description</span></span>|<span data-ttu-id="4c811-214">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-215">korlát beállítása</span><span class="sxs-lookup"><span data-stu-id="4c811-215">set-limit</span></span>|<span data-ttu-id="4c811-216">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-216">Root element.</span></span>|<span data-ttu-id="4c811-217">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-218">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-218">Attributes</span></span>  
  
|<span data-ttu-id="4c811-219">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-219">Name</span></span>|<span data-ttu-id="4c811-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-220">Description</span></span>|<span data-ttu-id="4c811-221">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-221">Required</span></span>|<span data-ttu-id="4c811-222">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-223">hívások</span><span class="sxs-lookup"><span data-stu-id="4c811-223">calls</span></span>|<span data-ttu-id="4c811-224">A megadott időintervallumon engedélyezett hívások maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-224">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-225">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-225">Yes</span></span>|<span data-ttu-id="4c811-226">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-226">N/A</span></span>|  
|<span data-ttu-id="4c811-227">másik kulcs</span><span class="sxs-lookup"><span data-stu-id="4c811-227">counter-key</span></span>|<span data-ttu-id="4c811-228">A sebesség korlát házirend használandó kulcs.</span><span class="sxs-lookup"><span data-stu-id="4c811-228">The key to use for the rate limit policy.</span></span>|<span data-ttu-id="4c811-229">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-229">Yes</span></span>|<span data-ttu-id="4c811-230">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-230">N/A</span></span>|  
|<span data-ttu-id="4c811-231">növekvő-feltétel</span><span class="sxs-lookup"><span data-stu-id="4c811-231">increment-condition</span></span>|<span data-ttu-id="4c811-232">Adja meg, ha a kérelem kell számolni, felé a kvóta logikai kifejezés (`true`).</span><span class="sxs-lookup"><span data-stu-id="4c811-232">The boolean expression specifying if the request should be counted towards the quota (`true`).</span></span>|<span data-ttu-id="4c811-233">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-233">No</span></span>|<span data-ttu-id="4c811-234">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-234">N/A</span></span>|  
|<span data-ttu-id="4c811-235">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="4c811-235">renewal-period</span></span>|<span data-ttu-id="4c811-236">Az adott időszakban másodpercben, amely után a kvóta visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="4c811-236">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="4c811-237">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-237">Yes</span></span>|<span data-ttu-id="4c811-238">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-239">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-239">Usage</span></span>  
 <span data-ttu-id="4c811-240">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-240">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-241">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-242">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="4c811-243"><a name="RestrictCallerIPs"></a>A hívó IP-címek korlátozása</span><span class="sxs-lookup"><span data-stu-id="4c811-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="4c811-244">A `ip-filter` házirend szűrők (engedélyezi vagy megtagadja) hívásait bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="4c811-244">The `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-245">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-246">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="4c811-247">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-247">Elements</span></span>  
  
|<span data-ttu-id="4c811-248">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-248">Name</span></span>|<span data-ttu-id="4c811-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-249">Description</span></span>|<span data-ttu-id="4c811-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-251">IP-szűrő</span><span class="sxs-lookup"><span data-stu-id="4c811-251">ip-filter</span></span>|<span data-ttu-id="4c811-252">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-252">Root element.</span></span>|<span data-ttu-id="4c811-253">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-253">Yes</span></span>|  
|<span data-ttu-id="4c811-254">Cím</span><span class="sxs-lookup"><span data-stu-id="4c811-254">address</span></span>|<span data-ttu-id="4c811-255">Adja meg a szűrni kívánt egyetlen IP-címet.</span><span class="sxs-lookup"><span data-stu-id="4c811-255">Specifies a single IP address on which to filter.</span></span>|<span data-ttu-id="4c811-256">Legalább egy `address` vagy `address-range` elemet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="4c811-257">címtartományt, az "address" = "address" =</span><span class="sxs-lookup"><span data-stu-id="4c811-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="4c811-258">Szűrni kívánt az IP-címet ad meg.</span><span class="sxs-lookup"><span data-stu-id="4c811-258">Specifies a range of IP address on which to filter.</span></span>|<span data-ttu-id="4c811-259">Legalább egy `address` vagy `address-range` elemet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-260">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-260">Attributes</span></span>  
  
|<span data-ttu-id="4c811-261">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-261">Name</span></span>|<span data-ttu-id="4c811-262">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-262">Description</span></span>|<span data-ttu-id="4c811-263">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-263">Required</span></span>|<span data-ttu-id="4c811-264">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-265">címtartományt, az "address" = "address" =</span><span class="sxs-lookup"><span data-stu-id="4c811-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="4c811-266">Engedélyezi vagy megtagadja a hozzáférést egy adott IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="4c811-266">A range of IP addresses to allow or deny access for.</span></span>|<span data-ttu-id="4c811-267">Szükséges, ha a `address-range` elem szolgál.</span><span class="sxs-lookup"><span data-stu-id="4c811-267">Required when the `address-range` element is used.</span></span>|<span data-ttu-id="4c811-268">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-268">N/A</span></span>|  
|<span data-ttu-id="4c811-269">IP-szűrési művelet = "engedélyezése &#124; megtiltják"</span><span class="sxs-lookup"><span data-stu-id="4c811-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="4c811-270">Megadja, hogy hívások engedélyezni kell, vagy nem az a megadott IP-címek és tartományok.</span><span class="sxs-lookup"><span data-stu-id="4c811-270">Specifies whether calls should be allowed or not for the specified IP addresses and ranges.</span></span>|<span data-ttu-id="4c811-271">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-271">Yes</span></span>|<span data-ttu-id="4c811-272">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-273">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-273">Usage</span></span>  
 <span data-ttu-id="4c811-274">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-275">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-276">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="4c811-277"><a name="SetUsageQuota"></a>Set memóriahasználati kvóta-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="4c811-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="4c811-278">A `quota` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy előfizetés alapon.</span><span class="sxs-lookup"><span data-stu-id="4c811-278">The `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="4c811-279">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="4c811-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="4c811-280">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható a házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="4c811-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-281">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-282">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-282">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4c811-283">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-283">Elements</span></span>  
  
|<span data-ttu-id="4c811-284">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-284">Name</span></span>|<span data-ttu-id="4c811-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-285">Description</span></span>|<span data-ttu-id="4c811-286">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-287">kvóta</span><span class="sxs-lookup"><span data-stu-id="4c811-287">quota</span></span>|<span data-ttu-id="4c811-288">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-288">Root element.</span></span>|<span data-ttu-id="4c811-289">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-289">Yes</span></span>|  
|<span data-ttu-id="4c811-290">api-t</span><span class="sxs-lookup"><span data-stu-id="4c811-290">api</span></span>|<span data-ttu-id="4c811-291">Adjon hozzá egy vagy több ezeket az elemeket a kvóta az API-k a terméken belüli bevezetése.</span><span class="sxs-lookup"><span data-stu-id="4c811-291">Add one  or more of these elements to impose a quota on APIs within the product.</span></span> <span data-ttu-id="4c811-292">A termék és API-kvóták egymástól függetlenül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="4c811-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="4c811-293">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-293">No</span></span>|  
|<span data-ttu-id="4c811-294">művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-294">operation</span></span>|<span data-ttu-id="4c811-295">Vegyen fel legalább egy, a kvóta műveleten belül az API-k bevezetése ezeket az elemeket.</span><span class="sxs-lookup"><span data-stu-id="4c811-295">Add one  or more of these elements to impose a quota on operations within an API.</span></span> <span data-ttu-id="4c811-296">Termék API és művelet kvóták egymástól függetlenül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="4c811-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="4c811-297">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-298">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-298">Attributes</span></span>  
  
|<span data-ttu-id="4c811-299">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-299">Name</span></span>|<span data-ttu-id="4c811-300">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-300">Description</span></span>|<span data-ttu-id="4c811-301">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-301">Required</span></span>|<span data-ttu-id="4c811-302">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-303">név</span><span class="sxs-lookup"><span data-stu-id="4c811-303">name</span></span>|<span data-ttu-id="4c811-304">Az API-t vagy a művelet, amelynek a kvóta vonatkozik neve.</span><span class="sxs-lookup"><span data-stu-id="4c811-304">The name of the API or operation for which the quota applies.</span></span>|<span data-ttu-id="4c811-305">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-305">Yes</span></span>|<span data-ttu-id="4c811-306">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-306">N/A</span></span>|  
|<span data-ttu-id="4c811-307">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="4c811-307">bandwidth</span></span>|<span data-ttu-id="4c811-308">A megadott időintervallumon engedélyezett kilobájt maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-308">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-309">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="4c811-310">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-310">N/A</span></span>|  
|<span data-ttu-id="4c811-311">hívások</span><span class="sxs-lookup"><span data-stu-id="4c811-311">calls</span></span>|<span data-ttu-id="4c811-312">A megadott időintervallumon engedélyezett hívások maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-312">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-313">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="4c811-314">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-314">N/A</span></span>|  
|<span data-ttu-id="4c811-315">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="4c811-315">renewal-period</span></span>|<span data-ttu-id="4c811-316">Az adott időszakban másodpercben, amely után a kvóta visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="4c811-316">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="4c811-317">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-317">Yes</span></span>|<span data-ttu-id="4c811-318">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-319">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-319">Usage</span></span>  
 <span data-ttu-id="4c811-320">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-320">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-321">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-322">**Házirend hatókörök:** termék</span><span class="sxs-lookup"><span data-stu-id="4c811-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="4c811-323"><a name="SetUsageQuotaByKey"></a>Set memóriahasználati kvóta gombot</span><span class="sxs-lookup"><span data-stu-id="4c811-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="4c811-324">A `quota-by-key` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy kulcs alapon.</span><span class="sxs-lookup"><span data-stu-id="4c811-324">The `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="4c811-325">A kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="4c811-325">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="4c811-326">Választható növekmény feltétel adhatók meg, hogy mely kérelmek kell számolni, a kvóta felé.</span><span class="sxs-lookup"><span data-stu-id="4c811-326">Optional increment condition can be added to specify which requests should be counted towards the quota.</span></span>  
  
 <span data-ttu-id="4c811-327">További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="4c811-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="4c811-328">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="4c811-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="4c811-329">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható a házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="4c811-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-330">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="4c811-331">Példa</span><span class="sxs-lookup"><span data-stu-id="4c811-331">Example</span></span>  
 <span data-ttu-id="4c811-332">A következő példában a kvóta a hívó IP-cím szerinti kulccsal definiált.</span><span class="sxs-lookup"><span data-stu-id="4c811-332">In the following example, the quota is keyed by the caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4c811-333">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-333">Elements</span></span>  
  
|<span data-ttu-id="4c811-334">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-334">Name</span></span>|<span data-ttu-id="4c811-335">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-335">Description</span></span>|<span data-ttu-id="4c811-336">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="4c811-337">kvóta</span><span class="sxs-lookup"><span data-stu-id="4c811-337">quota</span></span>|<span data-ttu-id="4c811-338">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-338">Root element.</span></span>|<span data-ttu-id="4c811-339">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-340">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-340">Attributes</span></span>  
  
|<span data-ttu-id="4c811-341">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-341">Name</span></span>|<span data-ttu-id="4c811-342">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-342">Description</span></span>|<span data-ttu-id="4c811-343">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-343">Required</span></span>|<span data-ttu-id="4c811-344">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-345">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="4c811-345">bandwidth</span></span>|<span data-ttu-id="4c811-346">A megadott időintervallumon engedélyezett kilobájt maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-346">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-347">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="4c811-348">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-348">N/A</span></span>|  
|<span data-ttu-id="4c811-349">hívások</span><span class="sxs-lookup"><span data-stu-id="4c811-349">calls</span></span>|<span data-ttu-id="4c811-350">A megadott időintervallumon engedélyezett hívások maximális száma a `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="4c811-350">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="4c811-351">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4c811-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="4c811-352">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-352">N/A</span></span>|  
|<span data-ttu-id="4c811-353">másik kulcs</span><span class="sxs-lookup"><span data-stu-id="4c811-353">counter-key</span></span>|<span data-ttu-id="4c811-354">A kvóta házirend használandó kulcs.</span><span class="sxs-lookup"><span data-stu-id="4c811-354">The key to use for the quota policy.</span></span>|<span data-ttu-id="4c811-355">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-355">Yes</span></span>|<span data-ttu-id="4c811-356">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-356">N/A</span></span>|  
|<span data-ttu-id="4c811-357">növekvő-feltétel</span><span class="sxs-lookup"><span data-stu-id="4c811-357">increment-condition</span></span>|<span data-ttu-id="4c811-358">Adja meg, ha a kérelem kell számolni, felé a kvóta logikai kifejezés (`true`)</span><span class="sxs-lookup"><span data-stu-id="4c811-358">The boolean expression specifying if the request should be counted towards the quota (`true`)</span></span>|<span data-ttu-id="4c811-359">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-359">No</span></span>|<span data-ttu-id="4c811-360">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-360">N/A</span></span>|  
|<span data-ttu-id="4c811-361">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="4c811-361">renewal-period</span></span>|<span data-ttu-id="4c811-362">Az adott időszakban másodpercben, amely után a kvóta visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="4c811-362">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="4c811-363">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-363">Yes</span></span>|<span data-ttu-id="4c811-364">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-365">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-365">Usage</span></span>  
 <span data-ttu-id="4c811-366">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-366">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-367">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-368">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="4c811-369"><a name="ValidateJWT"></a>JWT ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4c811-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="4c811-370">A `validate-jwt` házirend érvénybe lépteti a meglétét, és a jwt-t érvényességét kinyert vagy egy meghatározott HTTP-fejléc vagy a megadott lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="4c811-370">The `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="4c811-371">A `validate-jwt` házirend megköveteli, hogy a `exp` regisztrált nincs inlcuded a JWT jogkivonat, kivéve, ha `require-expiration-time` attribútum van megadva, és beállítása `false`.</span><span class="sxs-lookup"><span data-stu-id="4c811-371">The `validate-jwt` policy requires that the `exp` registered claim is inlcuded in the JWT token, unless `require-expiration-time` attribute is specified and set to `false`.</span></span>  
> <span data-ttu-id="4c811-372">A `validate-jwt` házirend HS256 és RS256 aláírási algoritmust támogat.</span><span class="sxs-lookup"><span data-stu-id="4c811-372">The `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="4c811-373">HS256 a kulcsot meg kell adni a base64-kódolású képernyőn a szabályzaton belüli beágyazott.</span><span class="sxs-lookup"><span data-stu-id="4c811-373">For HS256 the key must be provided inline within the policy in the base64 encoded form.</span></span> <span data-ttu-id="4c811-374">A kulcs RS256 keresztül nyitott azonosító konfigurációs végpontok biztosításához rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4c811-374">For RS256 the key has to be provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4c811-375">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="4c811-375">Policy statement</span></span>  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
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
    <claim name="name of the claim as it appears in the token" match="all|any">  
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="4c811-376">Példák</span><span class="sxs-lookup"><span data-stu-id="4c811-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="4c811-377">Az Azure Mobile Services jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4c811-377">Azure Mobile Services token validation</span></span>  
  
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
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="4c811-378">Az Azure Active Directory jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4c811-378">Azure Active Directory token validation</span></span>  
  
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

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="4c811-379">Az Azure Active Directory B2C jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4c811-379">Azure Active Directory B2C token validation</span></span>  
  
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
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a><span data-ttu-id="4c811-380">Műveletek a jogcímek jogkivonat-alapú hozzáférés hitelesítése</span><span class="sxs-lookup"><span data-stu-id="4c811-380">Authorize access to operations based on token claims</span></span>  
 <span data-ttu-id="4c811-381">A példa bemutatja, hogyan használható a [érvényesítése JWT](api-management-access-restriction-policies.md#ValidateJWT) házirend előre a műveletek hozzáférés hitelesítése a token jogcímei alapján.</span><span class="sxs-lookup"><span data-stu-id="4c811-381">This example shows how to use the [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy to pre-authorize access to operations based on token claims.</span></span> <span data-ttu-id="4c811-382">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és előretekerés 13:50.</span><span class="sxs-lookup"><span data-stu-id="4c811-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 13:50.</span></span> <span data-ttu-id="4c811-383">Gyors továbbítsa 15:00, a házirendek a Helyicsoportházirend-szerkesztő konfigurált megjelenítéséhez, majd a művelet hívása a developer portálról, és a szükséges engedélyezési jogkivonat anélkül bemutatója 18:50.</span><span class="sxs-lookup"><span data-stu-id="4c811-383">Fast forward to 15:00 to see the policies configured in the policy editor and then to 18:50 for a demonstration of calling an operation from the developer portal both with and without the required authorization token.</span></span>  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
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
  
### <a name="elements"></a><span data-ttu-id="4c811-384">Elemek</span><span class="sxs-lookup"><span data-stu-id="4c811-384">Elements</span></span>  
  
|<span data-ttu-id="4c811-385">Elem</span><span class="sxs-lookup"><span data-stu-id="4c811-385">Element</span></span>|<span data-ttu-id="4c811-386">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-386">Description</span></span>|<span data-ttu-id="4c811-387">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4c811-388">jwt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4c811-388">validate-jwt</span></span>|<span data-ttu-id="4c811-389">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="4c811-389">Root element.</span></span>|<span data-ttu-id="4c811-390">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-390">Yes</span></span>|  
|<span data-ttu-id="4c811-391">célcsoportok</span><span class="sxs-lookup"><span data-stu-id="4c811-391">audiences</span></span>|<span data-ttu-id="4c811-392">Lehet, hogy a jogkivonat jelenlegi elfogadható célközönség jogcímeket listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4c811-392">Contains a list of acceptable audience claims that can be present on the token.</span></span> <span data-ttu-id="4c811-393">Ha több célközönség értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="4c811-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="4c811-394">Legalább egy célközönség meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="4c811-394">At least one audience must be specified.</span></span>|<span data-ttu-id="4c811-395">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-395">No</span></span>|  
|<span data-ttu-id="4c811-396">Kiállítói aláírási kulcsok</span><span class="sxs-lookup"><span data-stu-id="4c811-396">issuer-signing-keys</span></span>|<span data-ttu-id="4c811-397">Aláírt jogkivonatokat érvényesítéséhez használt Base64-kódolású biztonsági kulcsok listáját.</span><span class="sxs-lookup"><span data-stu-id="4c811-397">A list of Base64-encoded security keys used to validate signed tokens.</span></span> <span data-ttu-id="4c811-398">Ha több biztonsági kulcsok szerepelnek, akkor minden kulcs próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel (hasznos token kulcsváltás).</span><span class="sxs-lookup"><span data-stu-id="4c811-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="4c811-399">Fő elemekből kell egy nem kötelező `id` az egyeztetéshez használt attribútum `kid` jogcímek.</span><span class="sxs-lookup"><span data-stu-id="4c811-399">Key elements have an optional `id` attribute used to match against `kid` claim.</span></span>|<span data-ttu-id="4c811-400">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-400">No</span></span>|  
|<span data-ttu-id="4c811-401">kibocsátók</span><span class="sxs-lookup"><span data-stu-id="4c811-401">issuers</span></span>|<span data-ttu-id="4c811-402">A jogkivonat kiállító elfogadható résztvevők listájának.</span><span class="sxs-lookup"><span data-stu-id="4c811-402">A list of acceptable principals that issued the token.</span></span> <span data-ttu-id="4c811-403">Ha több kibocsátó értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="4c811-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="4c811-404">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-404">No</span></span>|  
|<span data-ttu-id="4c811-405">openid-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4c811-405">openid-config</span></span>|<span data-ttu-id="4c811-406">Az elem, amelyből aláíró kulcsok és a kiállító érhető el megfelelő megnyitott azonosító konfigurációs végpont megadására használt.</span><span class="sxs-lookup"><span data-stu-id="4c811-406">The element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="4c811-407">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-407">No</span></span>|  
|<span data-ttu-id="4c811-408">szükséges jogcímeket</span><span class="sxs-lookup"><span data-stu-id="4c811-408">required-claims</span></span>|<span data-ttu-id="4c811-409">A jogcímek kellene lennie ahhoz, hogy a nem érvényes jogkivonat megtalálható listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4c811-409">Contains a list of claims expected to be present on the token for it to be considered valid.</span></span> <span data-ttu-id="4c811-410">Ha a `match` attribútum van beállítva `all` minden jogcím értékét a házirend a jogkivonat az érvényesítés sikeres jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-410">When the `match` attribute is set to `all` every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="4c811-411">Ha a `match` attribútum van beállítva `any` legalább egy jogcímet a jogkivonat az érvényesítés sikeres jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-411">When the `match` attribute is set to `any` at least one claim must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="4c811-412">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-412">No</span></span>|  
|<span data-ttu-id="4c811-413">zumo főkulcs</span><span class="sxs-lookup"><span data-stu-id="4c811-413">zumo-master-key</span></span>|<span data-ttu-id="4c811-414">Az Azure Mobile Services által kiállított jogkivonatokat főkulcs</span><span class="sxs-lookup"><span data-stu-id="4c811-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="4c811-415">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4c811-416">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="4c811-416">Attributes</span></span>  
  
|<span data-ttu-id="4c811-417">Név</span><span class="sxs-lookup"><span data-stu-id="4c811-417">Name</span></span>|<span data-ttu-id="4c811-418">Leírás</span><span class="sxs-lookup"><span data-stu-id="4c811-418">Description</span></span>|<span data-ttu-id="4c811-419">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4c811-419">Required</span></span>|<span data-ttu-id="4c811-420">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="4c811-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="4c811-421">óraeltérés</span><span class="sxs-lookup"><span data-stu-id="4c811-421">clock-skew</span></span>|<span data-ttu-id="4c811-422">TimeSpan érték.</span><span class="sxs-lookup"><span data-stu-id="4c811-422">Timespan.</span></span> <span data-ttu-id="4c811-423">Néhány kis eltérést biztosít, abban az esetben a jogkivonat lejárati jogcímek a lexikális elem szerepel, és az aktuális dátum utáni és idő.</span><span class="sxs-lookup"><span data-stu-id="4c811-423">Provides some small leeway in case the token's expiration claim is present in the token and is past the current date / time.</span></span>|<span data-ttu-id="4c811-424">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-424">No</span></span>|<span data-ttu-id="4c811-425">0 másodperc</span><span class="sxs-lookup"><span data-stu-id="4c811-425">0 seconds</span></span>|  
|<span data-ttu-id="4c811-426">nem sikerült – érvényesítési-hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="4c811-426">failed-validation-error-message</span></span>|<span data-ttu-id="4c811-427">A HTTP-válasz törzsében adja vissza, ha a jwt-t nem felel meg az érvényesítési hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="4c811-427">Error message to return in the HTTP response body if the JWT does not pass validation.</span></span> <span data-ttu-id="4c811-428">Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="4c811-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="4c811-429">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-429">No</span></span>|<span data-ttu-id="4c811-430">Alapértelmezett hibaüzenetet függ az érvényesítési hibát, például "JWT nem található."</span><span class="sxs-lookup"><span data-stu-id="4c811-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="4c811-431">nem sikerült – érvényesítési-HTTP-kód</span><span class="sxs-lookup"><span data-stu-id="4c811-431">failed-validation-httpcode</span></span>|<span data-ttu-id="4c811-432">HTTP-állapotkód vissza, ha a jwt-t nem teljesíti az ellenőrző.</span><span class="sxs-lookup"><span data-stu-id="4c811-432">HTTP Status code to return if the JWT doesn't pass validation.</span></span>|<span data-ttu-id="4c811-433">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-433">No</span></span>|<span data-ttu-id="4c811-434">401</span><span class="sxs-lookup"><span data-stu-id="4c811-434">401</span></span>|  
|<span data-ttu-id="4c811-435">fejléc-neve</span><span class="sxs-lookup"><span data-stu-id="4c811-435">header-name</span></span>|<span data-ttu-id="4c811-436">A HTTP-fejlécnek a tokent tároló neve.</span><span class="sxs-lookup"><span data-stu-id="4c811-436">The name of the HTTP header holding the token.</span></span>|<span data-ttu-id="4c811-437">Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="4c811-438">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-438">N/A</span></span>|  
|<span data-ttu-id="4c811-439">id</span><span class="sxs-lookup"><span data-stu-id="4c811-439">id</span></span>|<span data-ttu-id="4c811-440">A `id` attribútuma a `key` elem lehetővé teszi a karakterláncot, amely elleni megfeleltetésének `kid` tudja meg a megfelelő kulcsot az aláírás-ellenőrzés használata (ha van ilyen) a jogkivonat jogcímek.</span><span class="sxs-lookup"><span data-stu-id="4c811-440">The `id` attribute on the `key` element allows you to specify the string that will be matched against `kid` claim in the token (if present) to find out the appropriate key to use for signature validation.</span></span>|<span data-ttu-id="4c811-441">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-441">No</span></span>|<span data-ttu-id="4c811-442">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-442">N/A</span></span>|  
|<span data-ttu-id="4c811-443">Egyezés</span><span class="sxs-lookup"><span data-stu-id="4c811-443">match</span></span>|<span data-ttu-id="4c811-444">A `match` attribútuma a `claim` elem meghatározza, hogy a házirend minden jogcím értékét kell a jogkivonat az érvényesítés sikeres szerepel.</span><span class="sxs-lookup"><span data-stu-id="4c811-444">The `match` attribute on the `claim` element specifies whether every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="4c811-445">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="4c811-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="4c811-446">-                          `all`– a szabályzat minden jogcím értékét a jogkivonat az érvényesítés sikeres jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-446">-                          `all` - every claim value in the policy must be present in the token for validation to succeed.</span></span><br /><br /> <span data-ttu-id="4c811-447">-                          `any`-legalább egy jogcím értékét a jogkivonat az érvényesítés sikeres jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-447">-                          `any` - at least one claim value must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="4c811-448">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-448">No</span></span>|<span data-ttu-id="4c811-449">Minden</span><span class="sxs-lookup"><span data-stu-id="4c811-449">all</span></span>|  
|<span data-ttu-id="4c811-450">lekérdezés-paremeter-neve</span><span class="sxs-lookup"><span data-stu-id="4c811-450">query-paremeter-name</span></span>|<span data-ttu-id="4c811-451">Neve az a következő lekérdezésparaméter a tokent tároló.</span><span class="sxs-lookup"><span data-stu-id="4c811-451">The name of the the query parameter holding the token.</span></span>|<span data-ttu-id="4c811-452">Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4c811-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="4c811-453">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-453">N/A</span></span>|  
|<span data-ttu-id="4c811-454">igényelnek-lejárati-idő</span><span class="sxs-lookup"><span data-stu-id="4c811-454">require-expiration-time</span></span>|<span data-ttu-id="4c811-455">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4c811-455">Boolean.</span></span> <span data-ttu-id="4c811-456">Meghatározza, hogy szükséges-e egy lejárati jogcímet a tokenben.</span><span class="sxs-lookup"><span data-stu-id="4c811-456">Specifies whether an expiration claim is required in the token.</span></span>|<span data-ttu-id="4c811-457">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-457">No</span></span>|<span data-ttu-id="4c811-458">Igaz</span><span class="sxs-lookup"><span data-stu-id="4c811-458">true</span></span>|
|<span data-ttu-id="4c811-459">szükséges rendszer</span><span class="sxs-lookup"><span data-stu-id="4c811-459">require-scheme</span></span>|<span data-ttu-id="4c811-460">A token neve sémáját, pl. "Tulajdonos".</span><span class="sxs-lookup"><span data-stu-id="4c811-460">The name of the token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="4c811-461">Az attribútum van beállítva, ha a házirend biztosítja, hogy a megadott séma szerepel az engedélyezési fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="4c811-461">When this attribute is set, the policy will ensure that specified scheme is present in the Authorization header value.</span></span>|<span data-ttu-id="4c811-462">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-462">No</span></span>|<span data-ttu-id="4c811-463">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-463">N/A</span></span>|
|<span data-ttu-id="4c811-464">igényelnek-aláírt-tokenek</span><span class="sxs-lookup"><span data-stu-id="4c811-464">require-signed-tokens</span></span>|<span data-ttu-id="4c811-465">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4c811-465">Boolean.</span></span> <span data-ttu-id="4c811-466">Megadja, hogy egy jogkivonatot kell aláírni.</span><span class="sxs-lookup"><span data-stu-id="4c811-466">Specifies whether a token is required to be signed.</span></span>|<span data-ttu-id="4c811-467">Nem</span><span class="sxs-lookup"><span data-stu-id="4c811-467">No</span></span>|<span data-ttu-id="4c811-468">Igaz</span><span class="sxs-lookup"><span data-stu-id="4c811-468">true</span></span>|  
|<span data-ttu-id="4c811-469">URL-címe</span><span class="sxs-lookup"><span data-stu-id="4c811-469">url</span></span>|<span data-ttu-id="4c811-470">Azonosító konfigurációs végponti URL-cím megnyitása ahol nyitott azonosító konfigurációs metaadatok érhető el.</span><span class="sxs-lookup"><span data-stu-id="4c811-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="4c811-471">Az Azure Active Directory használata a következő URL-cím: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` a directory-bérlő neve, pl. és `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="4c811-471">For Azure Active Directory use the following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="4c811-472">Igen</span><span class="sxs-lookup"><span data-stu-id="4c811-472">Yes</span></span>|<span data-ttu-id="4c811-473">N/A</span><span class="sxs-lookup"><span data-stu-id="4c811-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4c811-474">Használat</span><span class="sxs-lookup"><span data-stu-id="4c811-474">Usage</span></span>  
 <span data-ttu-id="4c811-475">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4c811-475">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4c811-476">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="4c811-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4c811-477">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="4c811-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="4c811-478">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c811-478">Next steps</span></span>
<span data-ttu-id="4c811-479">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="4c811-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
