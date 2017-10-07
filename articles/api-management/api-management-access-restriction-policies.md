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
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="8f43b-103">Az API Management hozzáférés szoftverkorlátozó házirendek</span><span class="sxs-lookup"><span data-stu-id="8f43b-103">API Management access restriction policies</span></span>
<span data-ttu-id="8f43b-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="8f43b-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="8f43b-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="8f43b-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="8f43b-106"><a name="AccessRestrictionPolicies"></a>Hozzáférés a szoftverkorlátozó házirendek</span><span class="sxs-lookup"><span data-stu-id="8f43b-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="8f43b-107">[Ellenőrizze a HTTP-fejléc](api-management-access-restriction-policies.md#CheckHTTPHeader) -érvénybe lépteti a létezését és/vagy a HTTP-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="8f43b-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="8f43b-108">[Előfizetési határértéket hívás arányt a](api-management-access-restriction-policies.md#LimitCallRate) -megakadályozza, hogy API-használati napra hívás arány / előfizetés alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="8f43b-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8f43b-109">[Korlát hívás arányt a kulcs](#LimitCallRateByKey) -megakadályozza, hogy API-használati napra hívás arány / kulcs alapon korlátozásával.</span><span class="sxs-lookup"><span data-stu-id="8f43b-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8f43b-110">[A hívó IP-címek korlátozása](api-management-access-restriction-policies.md#RestrictCallerIPs) -szűrők (engedélyezi vagy megtagadja) hívást bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="8f43b-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="8f43b-111">[Set memóriahasználati kvóta előfizetéssel](api-management-access-restriction-policies.md#SetUsageQuota) -lehetővé teszi egy előfizetés alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="8f43b-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8f43b-112">[Set memóriahasználati kvóta kulcs által](#SetUsageQuotaByKey) -lehetővé teszi egy kulcs alapon tooenforce egy megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvótához.</span><span class="sxs-lookup"><span data-stu-id="8f43b-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8f43b-113">[Ellenőrizze a JWT](api-management-access-restriction-policies.md#ValidateJWT) -érvénybe lépteti a létezését és a jwt-t vagy a megadott HTTP-fejléc, vagy a megadott lekérdezési paraméter kinyert érvényességét.</span><span class="sxs-lookup"><span data-stu-id="8f43b-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="8f43b-114"><a name="CheckHTTPHeader"></a>Ellenőrizze a HTTP-fejléc</span><span class="sxs-lookup"><span data-stu-id="8f43b-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="8f43b-115">Használjon hello `check-header` házirend tooenforce, hogy egy kérelem rendelkezik-e a megadott HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="8f43b-115">Use hello `check-header` policy tooenforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="8f43b-116">Toosee opcionálisan ellenőrizheti, ha hello fejléce rendelkezik egy adott érték vagy az engedélyezett értéktartományon tartomány ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="8f43b-116">You can optionally check toosee if hello header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="8f43b-117">Ha a hello az ellenőrzés sikertelen, a hello HTTP kód és a hiba állapotüzenetet hello házirend által megadott értéket, és hello házirend kérelem feldolgozása leáll.</span><span class="sxs-lookup"><span data-stu-id="8f43b-117">If hello check fails, hello policy terminates request processing and returns hello HTTP status code and error message specified by hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-118">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-119">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="8f43b-120">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-120">Elements</span></span>  
  
|<span data-ttu-id="8f43b-121">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-121">Name</span></span>|<span data-ttu-id="8f43b-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-122">Description</span></span>|<span data-ttu-id="8f43b-123">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-124">ellenőrzés-fejléc</span><span class="sxs-lookup"><span data-stu-id="8f43b-124">check-header</span></span>|<span data-ttu-id="8f43b-125">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-125">Root element.</span></span>|<span data-ttu-id="8f43b-126">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-126">Yes</span></span>|  
|<span data-ttu-id="8f43b-127">érték</span><span class="sxs-lookup"><span data-stu-id="8f43b-127">value</span></span>|<span data-ttu-id="8f43b-128">Engedélyezett HTTP-fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="8f43b-128">Allowed HTTP header value.</span></span> <span data-ttu-id="8f43b-129">Ha több érték elem meg van adva, a hello ellenőrizze akkor tekinthető sikeres, ha bármely hello értékek egyik egyezés.</span><span class="sxs-lookup"><span data-stu-id="8f43b-129">When multiple value elements are specified, hello check is considered a success if any one of hello values is a match.</span></span>|<span data-ttu-id="8f43b-130">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-131">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-131">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-132">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-132">Name</span></span>|<span data-ttu-id="8f43b-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-133">Description</span></span>|<span data-ttu-id="8f43b-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-134">Required</span></span>|<span data-ttu-id="8f43b-135">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-136">nem sikerült – jelölőnégyzet-hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="8f43b-136">failed-check-error-message</span></span>|<span data-ttu-id="8f43b-137">Hiba üzenet tooreturn hello HTTP válasz törzsében Ha hello fejléc nem létezik vagy érvénytelen értékkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f43b-137">Error message tooreturn in hello HTTP response body if hello header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="8f43b-138">Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="8f43b-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8f43b-139">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-139">Yes</span></span>|<span data-ttu-id="8f43b-140">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-140">N/A</span></span>|  
|<span data-ttu-id="8f43b-141">nem sikerült – jelölőnégyzet-HTTP-kód</span><span class="sxs-lookup"><span data-stu-id="8f43b-141">failed-check-httpcode</span></span>|<span data-ttu-id="8f43b-142">HTTP-állapot kódját tooreturn, ha hello fejléc nem létezik vagy érvénytelen értékkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f43b-142">HTTP Status code tooreturn if hello header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="8f43b-143">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-143">Yes</span></span>|<span data-ttu-id="8f43b-144">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-144">N/A</span></span>|  
|<span data-ttu-id="8f43b-145">fejléc-neve</span><span class="sxs-lookup"><span data-stu-id="8f43b-145">header-name</span></span>|<span data-ttu-id="8f43b-146">HTTP-fejléc toocheck hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="8f43b-146">hello name of hello HTTP Header toocheck.</span></span>|<span data-ttu-id="8f43b-147">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-147">Yes</span></span>|<span data-ttu-id="8f43b-148">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-148">N/A</span></span>|  
|<span data-ttu-id="8f43b-149">esetben figyelmen kívül hagyása</span><span class="sxs-lookup"><span data-stu-id="8f43b-149">ignore-case</span></span>|<span data-ttu-id="8f43b-150">Állíthat be tooTrue vagy HAMIS eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="8f43b-150">Can be set tooTrue or False.</span></span> <span data-ttu-id="8f43b-151">Ha a set tooTrue esetben a rendszer figyelmen kívül hagyja, amikor hello elfogadható értékhalmazt összehasonlítja hello fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="8f43b-151">If set tooTrue case is ignored when hello header value is compared against hello set of acceptable values.</span></span>|<span data-ttu-id="8f43b-152">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-152">Yes</span></span>|<span data-ttu-id="8f43b-153">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-154">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-154">Usage</span></span>  
 <span data-ttu-id="8f43b-155">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-155">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-156">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="8f43b-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="8f43b-157">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8f43b-158"><a name="LimitCallRate"></a>Előfizetés hívás arányt a korlát</span><span class="sxs-lookup"><span data-stu-id="8f43b-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="8f43b-159">Hello `rate-limit` házirend megakadályozza, hogy az API-használati igényeiben jelentkező hello korlátozásával / előfizetés alapon hívja arány tooa megadott számú egy megadott időszak.</span><span class="sxs-lookup"><span data-stu-id="8f43b-159">hello `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8f43b-160">Ez a házirend kiváltásakor hello hívó kap egy `429 Too Many Requests` válasz állapotkódja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-160">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8f43b-161">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="8f43b-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8f43b-162">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="8f43b-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-163">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-164">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-164">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8f43b-165">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-165">Elements</span></span>  
  
|<span data-ttu-id="8f43b-166">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-166">Name</span></span>|<span data-ttu-id="8f43b-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-167">Description</span></span>|<span data-ttu-id="8f43b-168">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-169">korlát beállítása</span><span class="sxs-lookup"><span data-stu-id="8f43b-169">set-limit</span></span>|<span data-ttu-id="8f43b-170">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-170">Root element.</span></span>|<span data-ttu-id="8f43b-171">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-171">Yes</span></span>|  
|<span data-ttu-id="8f43b-172">api-t</span><span class="sxs-lookup"><span data-stu-id="8f43b-172">api</span></span>|<span data-ttu-id="8f43b-173">Adjon hozzá egy vagy több ezen elemek tooimpose hívás sávszélesség-korlátjának az API-k hello terméken belül.</span><span class="sxs-lookup"><span data-stu-id="8f43b-173">Add one  or more of these elements tooimpose a call rate limit on APIs within hello product.</span></span> <span data-ttu-id="8f43b-174">Termék és API hívása sebesség egymástól függetlenül korlátokat alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f43b-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="8f43b-175">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-175">No</span></span>|  
|<span data-ttu-id="8f43b-176">művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-176">operation</span></span>|<span data-ttu-id="8f43b-177">Adjon hozzá egy vagy több ezen elemek tooimpose hívás sávszélesség-korlátjának műveleten belül az API-k.</span><span class="sxs-lookup"><span data-stu-id="8f43b-177">Add one  or more of these elements tooimpose a call rate limit on operations within an API.</span></span> <span data-ttu-id="8f43b-178">Termék API és művelet hívása sebesség korlátok egymástól függetlenül alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f43b-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="8f43b-179">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-180">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-180">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-181">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-181">Name</span></span>|<span data-ttu-id="8f43b-182">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-182">Description</span></span>|<span data-ttu-id="8f43b-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-183">Required</span></span>|<span data-ttu-id="8f43b-184">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-185">név</span><span class="sxs-lookup"><span data-stu-id="8f43b-185">name</span></span>|<span data-ttu-id="8f43b-186">hello neve hello API mely tooapply hello sávszélesség-korlátjának.</span><span class="sxs-lookup"><span data-stu-id="8f43b-186">hello name of hello API for which tooapply hello rate limit.</span></span>|<span data-ttu-id="8f43b-187">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-187">Yes</span></span>|<span data-ttu-id="8f43b-188">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-188">N/A</span></span>|  
|<span data-ttu-id="8f43b-189">hívások</span><span class="sxs-lookup"><span data-stu-id="8f43b-189">calls</span></span>|<span data-ttu-id="8f43b-190">hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-190">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-191">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-191">Yes</span></span>|<span data-ttu-id="8f43b-192">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-192">N/A</span></span>|  
|<span data-ttu-id="8f43b-193">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="8f43b-193">renewal-period</span></span>|<span data-ttu-id="8f43b-194">hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-194">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8f43b-195">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-195">Yes</span></span>|<span data-ttu-id="8f43b-196">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-197">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-197">Usage</span></span>  
 <span data-ttu-id="8f43b-198">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-198">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-199">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-200">**Házirend hatókörök:** termék</span><span class="sxs-lookup"><span data-stu-id="8f43b-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8f43b-201"><a name="LimitCallRateByKey"></a>Korlát hívás arányt a kulcs</span><span class="sxs-lookup"><span data-stu-id="8f43b-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="8f43b-202">Hello `rate-limit-by-key` házirend megakadályozza, hogy az API-használati igényeiben jelentkező hello korlátozásával / kulcs alapon hívja arány tooa megadott számú egy megadott időszak.</span><span class="sxs-lookup"><span data-stu-id="8f43b-202">hello `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8f43b-203">hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="8f43b-203">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8f43b-204">Választható növekmény feltételt kell számolni, amelyekhez a hello határérték felé számolnak toospecify adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="8f43b-204">Optional increment condition can be added toospecify which requests should be counted towards hello limit.</span></span> <span data-ttu-id="8f43b-205">Ez a házirend kiváltásakor hello hívó kap egy `429 Too Many Requests` válasz állapotkódja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-205">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="8f43b-206">További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8f43b-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8f43b-207">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="8f43b-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-208">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-209">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-209">Example</span></span>  
 <span data-ttu-id="8f43b-210">A következő példa hello sávszélesség-korlátjának hello hello hívó IP-cím szerinti kulccsal definiált.</span><span class="sxs-lookup"><span data-stu-id="8f43b-210">In hello following example, hello rate limit is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8f43b-211">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-211">Elements</span></span>  
  
|<span data-ttu-id="8f43b-212">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-212">Name</span></span>|<span data-ttu-id="8f43b-213">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-213">Description</span></span>|<span data-ttu-id="8f43b-214">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-215">korlát beállítása</span><span class="sxs-lookup"><span data-stu-id="8f43b-215">set-limit</span></span>|<span data-ttu-id="8f43b-216">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-216">Root element.</span></span>|<span data-ttu-id="8f43b-217">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-218">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-218">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-219">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-219">Name</span></span>|<span data-ttu-id="8f43b-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-220">Description</span></span>|<span data-ttu-id="8f43b-221">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-221">Required</span></span>|<span data-ttu-id="8f43b-222">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-223">hívások</span><span class="sxs-lookup"><span data-stu-id="8f43b-223">calls</span></span>|<span data-ttu-id="8f43b-224">hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-224">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-225">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-225">Yes</span></span>|<span data-ttu-id="8f43b-226">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-226">N/A</span></span>|  
|<span data-ttu-id="8f43b-227">másik kulcs</span><span class="sxs-lookup"><span data-stu-id="8f43b-227">counter-key</span></span>|<span data-ttu-id="8f43b-228">hello kulcs toouse hello arány korlát házirend.</span><span class="sxs-lookup"><span data-stu-id="8f43b-228">hello key toouse for hello rate limit policy.</span></span>|<span data-ttu-id="8f43b-229">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-229">Yes</span></span>|<span data-ttu-id="8f43b-230">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-230">N/A</span></span>|  
|<span data-ttu-id="8f43b-231">növekvő-feltétel</span><span class="sxs-lookup"><span data-stu-id="8f43b-231">increment-condition</span></span>|<span data-ttu-id="8f43b-232">Adja meg, ha hello kérelem kell számolni, felé hello kvóta hello logikai kifejezés (`true`).</span><span class="sxs-lookup"><span data-stu-id="8f43b-232">hello boolean expression specifying if hello request should be counted towards hello quota (`true`).</span></span>|<span data-ttu-id="8f43b-233">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-233">No</span></span>|<span data-ttu-id="8f43b-234">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-234">N/A</span></span>|  
|<span data-ttu-id="8f43b-235">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="8f43b-235">renewal-period</span></span>|<span data-ttu-id="8f43b-236">hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-236">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8f43b-237">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-237">Yes</span></span>|<span data-ttu-id="8f43b-238">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-239">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-239">Usage</span></span>  
 <span data-ttu-id="8f43b-240">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-240">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-241">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-242">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8f43b-243"><a name="RestrictCallerIPs"></a>A hívó IP-címek korlátozása</span><span class="sxs-lookup"><span data-stu-id="8f43b-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="8f43b-244">Hello `ip-filter` házirend szűrők (engedélyezi vagy megtagadja) hívásait bizonyos IP-címeket és/vagy címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="8f43b-244">hello `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-245">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-246">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="8f43b-247">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-247">Elements</span></span>  
  
|<span data-ttu-id="8f43b-248">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-248">Name</span></span>|<span data-ttu-id="8f43b-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-249">Description</span></span>|<span data-ttu-id="8f43b-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-251">IP-szűrő</span><span class="sxs-lookup"><span data-stu-id="8f43b-251">ip-filter</span></span>|<span data-ttu-id="8f43b-252">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-252">Root element.</span></span>|<span data-ttu-id="8f43b-253">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-253">Yes</span></span>|  
|<span data-ttu-id="8f43b-254">Cím</span><span class="sxs-lookup"><span data-stu-id="8f43b-254">address</span></span>|<span data-ttu-id="8f43b-255">Mely toofilter egy egyetlen IP-cím megadása</span><span class="sxs-lookup"><span data-stu-id="8f43b-255">Specifies a single IP address on which toofilter.</span></span>|<span data-ttu-id="8f43b-256">Legalább egy `address` vagy `address-range` elemet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="8f43b-257">címtartományt, az "address" = "address" =</span><span class="sxs-lookup"><span data-stu-id="8f43b-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="8f43b-258">Mely toofilter egy adott IP-cím megadása</span><span class="sxs-lookup"><span data-stu-id="8f43b-258">Specifies a range of IP address on which toofilter.</span></span>|<span data-ttu-id="8f43b-259">Legalább egy `address` vagy `address-range` elemet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-260">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-260">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-261">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-261">Name</span></span>|<span data-ttu-id="8f43b-262">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-262">Description</span></span>|<span data-ttu-id="8f43b-263">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-263">Required</span></span>|<span data-ttu-id="8f43b-264">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-265">címtartományt, az "address" = "address" =</span><span class="sxs-lookup"><span data-stu-id="8f43b-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="8f43b-266">Egy adott IP-címek tooallow vagy megtagadja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="8f43b-266">A range of IP addresses tooallow or deny access for.</span></span>|<span data-ttu-id="8f43b-267">Kötelező, ha hello `address-range` elem szolgál.</span><span class="sxs-lookup"><span data-stu-id="8f43b-267">Required when hello `address-range` element is used.</span></span>|<span data-ttu-id="8f43b-268">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-268">N/A</span></span>|  
|<span data-ttu-id="8f43b-269">IP-szűrési művelet = "engedélyezése &#124; megtiltják"</span><span class="sxs-lookup"><span data-stu-id="8f43b-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="8f43b-270">Megadja, hogy hívások engedélyezni kell-e, vagy nem hello a megadott IP-címek és tartományok.</span><span class="sxs-lookup"><span data-stu-id="8f43b-270">Specifies whether calls should be allowed or not for hello specified IP addresses and ranges.</span></span>|<span data-ttu-id="8f43b-271">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-271">Yes</span></span>|<span data-ttu-id="8f43b-272">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-273">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-273">Usage</span></span>  
 <span data-ttu-id="8f43b-274">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-275">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-276">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8f43b-277"><a name="SetUsageQuota"></a>Set memóriahasználati kvóta-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="8f43b-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="8f43b-278">Hello `quota` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy előfizetés alapon.</span><span class="sxs-lookup"><span data-stu-id="8f43b-278">hello `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8f43b-279">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="8f43b-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8f43b-280">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="8f43b-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-281">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-282">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-282">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8f43b-283">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-283">Elements</span></span>  
  
|<span data-ttu-id="8f43b-284">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-284">Name</span></span>|<span data-ttu-id="8f43b-285">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-285">Description</span></span>|<span data-ttu-id="8f43b-286">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-287">kvóta</span><span class="sxs-lookup"><span data-stu-id="8f43b-287">quota</span></span>|<span data-ttu-id="8f43b-288">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-288">Root element.</span></span>|<span data-ttu-id="8f43b-289">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-289">Yes</span></span>|  
|<span data-ttu-id="8f43b-290">api-t</span><span class="sxs-lookup"><span data-stu-id="8f43b-290">api</span></span>|<span data-ttu-id="8f43b-291">Adjon hozzá egy vagy több ezen elemek tooimpose a kvóta az API-k hello terméken belül.</span><span class="sxs-lookup"><span data-stu-id="8f43b-291">Add one  or more of these elements tooimpose a quota on APIs within hello product.</span></span> <span data-ttu-id="8f43b-292">A termék és API-kvóták egymástól függetlenül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="8f43b-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="8f43b-293">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-293">No</span></span>|  
|<span data-ttu-id="8f43b-294">művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-294">operation</span></span>|<span data-ttu-id="8f43b-295">Adjon hozzá egy vagy több ezen elemek tooimpose kvóta műveleten belül az API-k.</span><span class="sxs-lookup"><span data-stu-id="8f43b-295">Add one  or more of these elements tooimpose a quota on operations within an API.</span></span> <span data-ttu-id="8f43b-296">Termék API és művelet kvóták egymástól függetlenül érvényesek.</span><span class="sxs-lookup"><span data-stu-id="8f43b-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="8f43b-297">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-298">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-298">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-299">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-299">Name</span></span>|<span data-ttu-id="8f43b-300">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-300">Description</span></span>|<span data-ttu-id="8f43b-301">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-301">Required</span></span>|<span data-ttu-id="8f43b-302">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-303">név</span><span class="sxs-lookup"><span data-stu-id="8f43b-303">name</span></span>|<span data-ttu-id="8f43b-304">hello neve hello API-t vagy a művelet, mely hello kvóta vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="8f43b-304">hello name of hello API or operation for which hello quota applies.</span></span>|<span data-ttu-id="8f43b-305">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-305">Yes</span></span>|<span data-ttu-id="8f43b-306">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-306">N/A</span></span>|  
|<span data-ttu-id="8f43b-307">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="8f43b-307">bandwidth</span></span>|<span data-ttu-id="8f43b-308">hello kilobájt hello hello megadott időtartam során engedélyezett maximális száma `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-308">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-309">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8f43b-310">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-310">N/A</span></span>|  
|<span data-ttu-id="8f43b-311">hívások</span><span class="sxs-lookup"><span data-stu-id="8f43b-311">calls</span></span>|<span data-ttu-id="8f43b-312">hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-312">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-313">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8f43b-314">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-314">N/A</span></span>|  
|<span data-ttu-id="8f43b-315">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="8f43b-315">renewal-period</span></span>|<span data-ttu-id="8f43b-316">hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-316">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8f43b-317">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-317">Yes</span></span>|<span data-ttu-id="8f43b-318">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-319">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-319">Usage</span></span>  
 <span data-ttu-id="8f43b-320">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-320">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-321">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-322">**Házirend hatókörök:** termék</span><span class="sxs-lookup"><span data-stu-id="8f43b-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8f43b-323"><a name="SetUsageQuotaByKey"></a>Set memóriahasználati kvóta gombot</span><span class="sxs-lookup"><span data-stu-id="8f43b-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="8f43b-324">Hello `quota-by-key` házirend érvénybe lépteti a megújítható vagy élettartama hívás mennyiségi és/vagy a sávszélesség kvóta, egy kulcs alapon.</span><span class="sxs-lookup"><span data-stu-id="8f43b-324">hello `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="8f43b-325">hello kulcs egy tetszőleges karakterlánc értéke lehet, és általában valósul meg a házirend-kifejezés használatával.</span><span class="sxs-lookup"><span data-stu-id="8f43b-325">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8f43b-326">Nem kötelező biztonsági feltétel kérések kell számolni, felé hello kvóta toospecify adhatók hozzá.</span><span class="sxs-lookup"><span data-stu-id="8f43b-326">Optional increment condition can be added toospecify which requests should be counted towards hello quota.</span></span>  
  
 <span data-ttu-id="8f43b-327">További tudnivalók és példák ezt a házirendet, lásd: [speciális kérelmet az Azure API Management-szabályozás](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8f43b-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8f43b-328">Ezzel a házirend-házirend dokumentumonként csak egyszer használható.</span><span class="sxs-lookup"><span data-stu-id="8f43b-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8f43b-329">[Házirend-kifejezések](api-management-policy-expressions.md) nem használható hello házirend attribútumokat a házirend.</span><span class="sxs-lookup"><span data-stu-id="8f43b-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-330">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8f43b-331">Példa</span><span class="sxs-lookup"><span data-stu-id="8f43b-331">Example</span></span>  
 <span data-ttu-id="8f43b-332">A következő példa hello hello kvóta hello hívó IP-cím szerinti kulccsal definiált.</span><span class="sxs-lookup"><span data-stu-id="8f43b-332">In hello following example, hello quota is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8f43b-333">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-333">Elements</span></span>  
  
|<span data-ttu-id="8f43b-334">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-334">Name</span></span>|<span data-ttu-id="8f43b-335">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-335">Description</span></span>|<span data-ttu-id="8f43b-336">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8f43b-337">kvóta</span><span class="sxs-lookup"><span data-stu-id="8f43b-337">quota</span></span>|<span data-ttu-id="8f43b-338">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-338">Root element.</span></span>|<span data-ttu-id="8f43b-339">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-340">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-340">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-341">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-341">Name</span></span>|<span data-ttu-id="8f43b-342">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-342">Description</span></span>|<span data-ttu-id="8f43b-343">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-343">Required</span></span>|<span data-ttu-id="8f43b-344">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-345">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="8f43b-345">bandwidth</span></span>|<span data-ttu-id="8f43b-346">hello kilobájt hello hello megadott időtartam során engedélyezett maximális száma `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-346">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-347">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8f43b-348">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-348">N/A</span></span>|  
|<span data-ttu-id="8f43b-349">hívások</span><span class="sxs-lookup"><span data-stu-id="8f43b-349">calls</span></span>|<span data-ttu-id="8f43b-350">hello hello megadott időtartam során engedélyezett hívások maximális száma hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-350">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8f43b-351">Vagy `calls`, `bandwidth`, vagy együttesen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8f43b-352">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-352">N/A</span></span>|  
|<span data-ttu-id="8f43b-353">másik kulcs</span><span class="sxs-lookup"><span data-stu-id="8f43b-353">counter-key</span></span>|<span data-ttu-id="8f43b-354">hello kulcs toouse hello kvóta házirend.</span><span class="sxs-lookup"><span data-stu-id="8f43b-354">hello key toouse for hello quota policy.</span></span>|<span data-ttu-id="8f43b-355">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-355">Yes</span></span>|<span data-ttu-id="8f43b-356">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-356">N/A</span></span>|  
|<span data-ttu-id="8f43b-357">növekvő-feltétel</span><span class="sxs-lookup"><span data-stu-id="8f43b-357">increment-condition</span></span>|<span data-ttu-id="8f43b-358">Adja meg, ha hello kérelem kell számolni, felé hello kvóta hello logikai kifejezés (`true`)</span><span class="sxs-lookup"><span data-stu-id="8f43b-358">hello boolean expression specifying if hello request should be counted towards hello quota (`true`)</span></span>|<span data-ttu-id="8f43b-359">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-359">No</span></span>|<span data-ttu-id="8f43b-360">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-360">N/A</span></span>|  
|<span data-ttu-id="8f43b-361">megújítási időszak</span><span class="sxs-lookup"><span data-stu-id="8f43b-361">renewal-period</span></span>|<span data-ttu-id="8f43b-362">hello időtartam másodperc elteltével hello kvóta alaphelyzetbe állítja.</span><span class="sxs-lookup"><span data-stu-id="8f43b-362">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8f43b-363">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-363">Yes</span></span>|<span data-ttu-id="8f43b-364">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-365">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-365">Usage</span></span>  
 <span data-ttu-id="8f43b-366">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-366">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-367">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-368">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8f43b-369"><a name="ValidateJWT"></a>JWT ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8f43b-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="8f43b-370">Hello `validate-jwt` házirend érvénybe lépteti a meglétét, és a jwt-t érvényességét kinyert vagy egy meghatározott HTTP-fejléc vagy a megadott lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="8f43b-370">hello `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8f43b-371">Hello `validate-jwt` házirendje megköveteli, hogy hello `exp` regisztrált nincs inlcuded hello JWT jogkivonat, kivéve, ha `require-expiration-time` attribútum be van megadva, túl`false`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-371">hello `validate-jwt` policy requires that hello `exp` registered claim is inlcuded in hello JWT token, unless `require-expiration-time` attribute is specified and set too`false`.</span></span>  
> <span data-ttu-id="8f43b-372">Hello `validate-jwt` házirend HS256 és RS256 aláírási algoritmust támogat.</span><span class="sxs-lookup"><span data-stu-id="8f43b-372">hello `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="8f43b-373">HS256 hello kulcsot meg kell adni beágyazott hello házirend hello base64-kódolású formában.</span><span class="sxs-lookup"><span data-stu-id="8f43b-373">For HS256 hello key must be provided inline within hello policy in hello base64 encoded form.</span></span> <span data-ttu-id="8f43b-374">RS256 hello kulcsot adjon meg azonosító nyissa meg a konfigurációs végpont keresztül toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f43b-374">For RS256 hello key has toobe provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8f43b-375">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="8f43b-375">Policy statement</span></span>  
  
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
  
### <a name="examples"></a><span data-ttu-id="8f43b-376">Példák</span><span class="sxs-lookup"><span data-stu-id="8f43b-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="8f43b-377">Az Azure Mobile Services jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8f43b-377">Azure Mobile Services token validation</span></span>  
  
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
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="8f43b-378">Az Azure Active Directory jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8f43b-378">Azure Active Directory token validation</span></span>  
  
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

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="8f43b-379">Az Azure Active Directory B2C jogkivonatok érvényesség-ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8f43b-379">Azure Active Directory B2C token validation</span></span>  
  
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
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a><span data-ttu-id="8f43b-380">A jogcímek jogkivonat-alapú hozzáférés toooperations engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8f43b-380">Authorize access toooperations based on token claims</span></span>  
 <span data-ttu-id="8f43b-381">A példa bemutatja, hogyan toouse hello [érvényesítése JWT](api-management-access-restriction-policies.md#ValidateJWT) házirend toopre-hozzáférés toooperations a jogcímek jogkivonat-alapú hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f43b-381">This example shows how toouse hello [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy toopre-authorize access toooperations based on token claims.</span></span> <span data-ttu-id="8f43b-382">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too13:50 előre.</span><span class="sxs-lookup"><span data-stu-id="8f43b-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="8f43b-383">Gyors toosee hello házirendeket a Helyicsoportházirend-szerkesztő hello az too15:00 továbbítja, és majd a művelet hívásának hello developer portálról vagy anélkül hello bemutatója too18:50 szükséges engedélyezési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="8f43b-383">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8f43b-384">Elemek</span><span class="sxs-lookup"><span data-stu-id="8f43b-384">Elements</span></span>  
  
|<span data-ttu-id="8f43b-385">Elem</span><span class="sxs-lookup"><span data-stu-id="8f43b-385">Element</span></span>|<span data-ttu-id="8f43b-386">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-386">Description</span></span>|<span data-ttu-id="8f43b-387">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="8f43b-388">jwt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8f43b-388">validate-jwt</span></span>|<span data-ttu-id="8f43b-389">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="8f43b-389">Root element.</span></span>|<span data-ttu-id="8f43b-390">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-390">Yes</span></span>|  
|<span data-ttu-id="8f43b-391">célcsoportok</span><span class="sxs-lookup"><span data-stu-id="8f43b-391">audiences</span></span>|<span data-ttu-id="8f43b-392">Elfogadható célközönség jogcímek használható hello jogkivonat listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f43b-392">Contains a list of acceptable audience claims that can be present on hello token.</span></span> <span data-ttu-id="8f43b-393">Ha több célközönség értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="8f43b-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="8f43b-394">Legalább egy célközönség meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="8f43b-394">At least one audience must be specified.</span></span>|<span data-ttu-id="8f43b-395">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-395">No</span></span>|  
|<span data-ttu-id="8f43b-396">Kiállítói aláírási kulcsok</span><span class="sxs-lookup"><span data-stu-id="8f43b-396">issuer-signing-keys</span></span>|<span data-ttu-id="8f43b-397">A Base64-kódolású biztonsági használt kulcsok toovalidate listáját aláírt jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="8f43b-397">A list of Base64-encoded security keys used toovalidate signed tokens.</span></span> <span data-ttu-id="8f43b-398">Ha több biztonsági kulcsok szerepelnek, akkor minden kulcs próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel (hasznos token kulcsváltás).</span><span class="sxs-lookup"><span data-stu-id="8f43b-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="8f43b-399">Fő elemekből kell egy nem kötelező `id` használt attribútum toomatch elleni `kid` jogcímek.</span><span class="sxs-lookup"><span data-stu-id="8f43b-399">Key elements have an optional `id` attribute used toomatch against `kid` claim.</span></span>|<span data-ttu-id="8f43b-400">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-400">No</span></span>|  
|<span data-ttu-id="8f43b-401">kibocsátók</span><span class="sxs-lookup"><span data-stu-id="8f43b-401">issuers</span></span>|<span data-ttu-id="8f43b-402">Hello jogkivonatot kibocsátó elfogadható résztvevők listájának.</span><span class="sxs-lookup"><span data-stu-id="8f43b-402">A list of acceptable principals that issued hello token.</span></span> <span data-ttu-id="8f43b-403">Ha több kibocsátó értékek találhatók, akkor minden egyes érték próbálkozik amíg újra nem indítják összes kimerültek (ebben az esetben az érvényesítés sikertelen), vagy amíg valamelyik nem jár sikerrel.</span><span class="sxs-lookup"><span data-stu-id="8f43b-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="8f43b-404">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-404">No</span></span>|  
|<span data-ttu-id="8f43b-405">openid-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8f43b-405">openid-config</span></span>|<span data-ttu-id="8f43b-406">hello elem segítségével megadhatja a megfelelő megnyitott azonosító konfigurációs végpont, amelyből aláíró kulcsok és a kiállító érhető el.</span><span class="sxs-lookup"><span data-stu-id="8f43b-406">hello element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="8f43b-407">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-407">No</span></span>|  
|<span data-ttu-id="8f43b-408">szükséges jogcímeket</span><span class="sxs-lookup"><span data-stu-id="8f43b-408">required-claims</span></span>|<span data-ttu-id="8f43b-409">Érvényes toobe várt jogcímek toobe az hello token megtalálható listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f43b-409">Contains a list of claims expected toobe present on hello token for it toobe considered valid.</span></span> <span data-ttu-id="8f43b-410">Ha hello `match` attribútum értéke túl`all` minden jogcím értékét hello házirend érvényesítési toosucceed hello jogkivonat jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-410">When hello `match` attribute is set too`all` every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8f43b-411">Ha hello `match` attribútum értéke túl`any` legalább egy jogcím érvényesítési toosucceed hello jogkivonat jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-411">When hello `match` attribute is set too`any` at least one claim must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8f43b-412">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-412">No</span></span>|  
|<span data-ttu-id="8f43b-413">zumo főkulcs</span><span class="sxs-lookup"><span data-stu-id="8f43b-413">zumo-master-key</span></span>|<span data-ttu-id="8f43b-414">Az Azure Mobile Services által kiállított jogkivonatokat főkulcs</span><span class="sxs-lookup"><span data-stu-id="8f43b-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="8f43b-415">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8f43b-416">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="8f43b-416">Attributes</span></span>  
  
|<span data-ttu-id="8f43b-417">Név</span><span class="sxs-lookup"><span data-stu-id="8f43b-417">Name</span></span>|<span data-ttu-id="8f43b-418">Leírás</span><span class="sxs-lookup"><span data-stu-id="8f43b-418">Description</span></span>|<span data-ttu-id="8f43b-419">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8f43b-419">Required</span></span>|<span data-ttu-id="8f43b-420">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8f43b-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8f43b-421">óraeltérés</span><span class="sxs-lookup"><span data-stu-id="8f43b-421">clock-skew</span></span>|<span data-ttu-id="8f43b-422">TimeSpan érték.</span><span class="sxs-lookup"><span data-stu-id="8f43b-422">Timespan.</span></span> <span data-ttu-id="8f43b-423">Néhány kis eltérést biztosít, abban az esetben hello jogkivonat lejárati jogcím hello lexikális elem szerepel, és már elmúlt hello jelenlegi dátum és idő.</span><span class="sxs-lookup"><span data-stu-id="8f43b-423">Provides some small leeway in case hello token's expiration claim is present in hello token and is past hello current date / time.</span></span>|<span data-ttu-id="8f43b-424">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-424">No</span></span>|<span data-ttu-id="8f43b-425">0 másodperc</span><span class="sxs-lookup"><span data-stu-id="8f43b-425">0 seconds</span></span>|  
|<span data-ttu-id="8f43b-426">nem sikerült – érvényesítési-hibaüzenetek</span><span class="sxs-lookup"><span data-stu-id="8f43b-426">failed-validation-error-message</span></span>|<span data-ttu-id="8f43b-427">Hiba üzenet tooreturn hello HTTP-válasz törzsében Ha hello JWT nem felel meg az érvényesítési.</span><span class="sxs-lookup"><span data-stu-id="8f43b-427">Error message tooreturn in hello HTTP response body if hello JWT does not pass validation.</span></span> <span data-ttu-id="8f43b-428">Ez az üzenet rendelkeznie kell a megfelelő escape-karaktersorozatot különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="8f43b-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8f43b-429">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-429">No</span></span>|<span data-ttu-id="8f43b-430">Alapértelmezett hibaüzenetet függ az érvényesítési hibát, például "JWT nem található."</span><span class="sxs-lookup"><span data-stu-id="8f43b-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="8f43b-431">nem sikerült – érvényesítési-HTTP-kód</span><span class="sxs-lookup"><span data-stu-id="8f43b-431">failed-validation-httpcode</span></span>|<span data-ttu-id="8f43b-432">HTTP-állapot kódját tooreturn, ha hello JWT nem teljesíti az ellenőrző.</span><span class="sxs-lookup"><span data-stu-id="8f43b-432">HTTP Status code tooreturn if hello JWT doesn't pass validation.</span></span>|<span data-ttu-id="8f43b-433">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-433">No</span></span>|<span data-ttu-id="8f43b-434">401</span><span class="sxs-lookup"><span data-stu-id="8f43b-434">401</span></span>|  
|<span data-ttu-id="8f43b-435">fejléc-neve</span><span class="sxs-lookup"><span data-stu-id="8f43b-435">header-name</span></span>|<span data-ttu-id="8f43b-436">hello hello token okozó hello HTTP-fejléc nevét.</span><span class="sxs-lookup"><span data-stu-id="8f43b-436">hello name of hello HTTP header holding hello token.</span></span>|<span data-ttu-id="8f43b-437">Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8f43b-438">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-438">N/A</span></span>|  
|<span data-ttu-id="8f43b-439">id</span><span class="sxs-lookup"><span data-stu-id="8f43b-439">id</span></span>|<span data-ttu-id="8f43b-440">Hello `id` hello attribútum `key` elem lehetővé teszi a toospecify hello karakterlánc, amely elleni megfeleltetésének `kid` hello token (ha van ilyen) toofind kimenő hello megfelelő kulcs toouse az aláírás-ellenőrzés a jogcímek.</span><span class="sxs-lookup"><span data-stu-id="8f43b-440">hello `id` attribute on hello `key` element allows you toospecify hello string that will be matched against `kid` claim in hello token (if present) toofind out hello appropriate key toouse for signature validation.</span></span>|<span data-ttu-id="8f43b-441">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-441">No</span></span>|<span data-ttu-id="8f43b-442">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-442">N/A</span></span>|  
|<span data-ttu-id="8f43b-443">Egyezés</span><span class="sxs-lookup"><span data-stu-id="8f43b-443">match</span></span>|<span data-ttu-id="8f43b-444">Hello `match` hello attribútum `claim` elem meghatározza, hogy minden hello házirend jogcím értékét kell a érvényesítési toosucceed hello lexikális elem szerepel.</span><span class="sxs-lookup"><span data-stu-id="8f43b-444">hello `match` attribute on hello `claim` element specifies whether every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8f43b-445">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="8f43b-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="8f43b-446">-                          `all`-hello házirend minden jogcím értékét érvényesítési toosucceed hello jogkivonat jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-446">-                          `all` - every claim value in hello policy must be present in hello token for validation toosucceed.</span></span><br /><br /> <span data-ttu-id="8f43b-447">-                          `any`-legalább egy jogcím értéke érvényesítési toosucceed hello jogkivonat jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-447">-                          `any` - at least one claim value must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8f43b-448">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-448">No</span></span>|<span data-ttu-id="8f43b-449">Minden</span><span class="sxs-lookup"><span data-stu-id="8f43b-449">all</span></span>|  
|<span data-ttu-id="8f43b-450">lekérdezés-paremeter-neve</span><span class="sxs-lookup"><span data-stu-id="8f43b-450">query-paremeter-name</span></span>|<span data-ttu-id="8f43b-451">hello hello lekérdezési paraméter hello token okozó hello neve.</span><span class="sxs-lookup"><span data-stu-id="8f43b-451">hello name of hello hello query parameter holding hello token.</span></span>|<span data-ttu-id="8f43b-452">Vagy `header-name` vagy `query-paremeter-name` megadott; de nem mindkettőn keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f43b-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8f43b-453">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-453">N/A</span></span>|  
|<span data-ttu-id="8f43b-454">igényelnek-lejárati-idő</span><span class="sxs-lookup"><span data-stu-id="8f43b-454">require-expiration-time</span></span>|<span data-ttu-id="8f43b-455">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="8f43b-455">Boolean.</span></span> <span data-ttu-id="8f43b-456">Meghatározza, hogy szükséges-e egy lejárati jogcím hello jogkivonatban.</span><span class="sxs-lookup"><span data-stu-id="8f43b-456">Specifies whether an expiration claim is required in hello token.</span></span>|<span data-ttu-id="8f43b-457">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-457">No</span></span>|<span data-ttu-id="8f43b-458">Igaz</span><span class="sxs-lookup"><span data-stu-id="8f43b-458">true</span></span>|
|<span data-ttu-id="8f43b-459">szükséges rendszer</span><span class="sxs-lookup"><span data-stu-id="8f43b-459">require-scheme</span></span>|<span data-ttu-id="8f43b-460">pl. hello hello token rendszer neve "Tulajdonos".</span><span class="sxs-lookup"><span data-stu-id="8f43b-460">hello name of hello token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="8f43b-461">Az attribútum van beállítva, amikor hello házirend biztosítja, hogy a megadott séma megtalálható-e hello engedélyezési fejléc értéke.</span><span class="sxs-lookup"><span data-stu-id="8f43b-461">When this attribute is set, hello policy will ensure that specified scheme is present in hello Authorization header value.</span></span>|<span data-ttu-id="8f43b-462">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-462">No</span></span>|<span data-ttu-id="8f43b-463">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-463">N/A</span></span>|
|<span data-ttu-id="8f43b-464">igényelnek-aláírt-tokenek</span><span class="sxs-lookup"><span data-stu-id="8f43b-464">require-signed-tokens</span></span>|<span data-ttu-id="8f43b-465">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="8f43b-465">Boolean.</span></span> <span data-ttu-id="8f43b-466">Megadja, hogy a jogkivonat aláírt szükséges toobe.</span><span class="sxs-lookup"><span data-stu-id="8f43b-466">Specifies whether a token is required toobe signed.</span></span>|<span data-ttu-id="8f43b-467">Nem</span><span class="sxs-lookup"><span data-stu-id="8f43b-467">No</span></span>|<span data-ttu-id="8f43b-468">Igaz</span><span class="sxs-lookup"><span data-stu-id="8f43b-468">true</span></span>|  
|<span data-ttu-id="8f43b-469">URL-címe</span><span class="sxs-lookup"><span data-stu-id="8f43b-469">url</span></span>|<span data-ttu-id="8f43b-470">Azonosító konfigurációs végponti URL-cím megnyitása ahol nyitott azonosító konfigurációs metaadatok érhető el.</span><span class="sxs-lookup"><span data-stu-id="8f43b-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="8f43b-471">Az Azure Active Directory használata a következő URL-cím hello: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` a directory-bérlő neve, pl. és `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="8f43b-471">For Azure Active Directory use hello following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="8f43b-472">Igen</span><span class="sxs-lookup"><span data-stu-id="8f43b-472">Yes</span></span>|<span data-ttu-id="8f43b-473">N/A</span><span class="sxs-lookup"><span data-stu-id="8f43b-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8f43b-474">Használat</span><span class="sxs-lookup"><span data-stu-id="8f43b-474">Usage</span></span>  
 <span data-ttu-id="8f43b-475">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8f43b-475">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8f43b-476">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="8f43b-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8f43b-477">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="8f43b-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="8f43b-478">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f43b-478">Next steps</span></span>
<span data-ttu-id="8f43b-479">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="8f43b-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
