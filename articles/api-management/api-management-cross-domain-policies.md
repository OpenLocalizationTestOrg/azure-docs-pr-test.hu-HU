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
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="adab5-103">Az API Management tartományközi házirendjei</span><span class="sxs-lookup"><span data-stu-id="adab5-103">API Management cross domain policies</span></span>
<span data-ttu-id="adab5-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="adab5-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="adab5-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="adab5-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="adab5-106"><a name="CrossDomainPolicies"></a>Kereszt-tartományi házirendek</span><span class="sxs-lookup"><span data-stu-id="adab5-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="adab5-107">[Lehetővé teszi a tartományok közötti hívások](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -hello API elérhető lesz az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="adab5-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="adab5-108">[A CORS](api-management-cross-domain-policies.md#CORS) -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.</span><span class="sxs-lookup"><span data-stu-id="adab5-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="adab5-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - JSON hozzáadása (JSONP) kitöltési támogatási tooan műveletet, vagy egy API tooallow tartományokon átívelő JavaScript-ügyfelekből webböngésző-alapú hívja.</span><span class="sxs-lookup"><span data-stu-id="adab5-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="adab5-110"><a name="AllowCrossDomainCalls"></a>Lehetővé teszi a tartományok közötti hívások</span><span class="sxs-lookup"><span data-stu-id="adab5-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="adab5-111">Használjon hello `cross-domain` házirend toomake hello API érhető el az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="adab5-111">Use hello `cross-domain` policy toomake hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="adab5-112">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="adab5-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="adab5-113">Példa</span><span class="sxs-lookup"><span data-stu-id="adab5-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="adab5-114">Elemek</span><span class="sxs-lookup"><span data-stu-id="adab5-114">Elements</span></span>  
  
|<span data-ttu-id="adab5-115">Név</span><span class="sxs-lookup"><span data-stu-id="adab5-115">Name</span></span>|<span data-ttu-id="adab5-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="adab5-116">Description</span></span>|<span data-ttu-id="adab5-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adab5-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="adab5-118">tartományok közötti</span><span class="sxs-lookup"><span data-stu-id="adab5-118">cross-domain</span></span>|<span data-ttu-id="adab5-119">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="adab5-119">Root element.</span></span> <span data-ttu-id="adab5-120">Gyermekelemek meg kell felelnie a toohello [Adobe tartományok közötti házirend specifikációnak](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="adab5-120">Child elements must conform toohello [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="adab5-121">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="adab5-122">Használat</span><span class="sxs-lookup"><span data-stu-id="adab5-122">Usage</span></span>  
 <span data-ttu-id="adab5-123">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="adab5-123">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="adab5-124">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="adab5-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="adab5-125">**Házirend hatókörök:** globális</span><span class="sxs-lookup"><span data-stu-id="adab5-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="adab5-126"><a name="CORS"></a>A CORS</span><span class="sxs-lookup"><span data-stu-id="adab5-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="adab5-127">Hello `cors` házirendnek eltérő eredetű erőforrások megosztása (CORS) támogatja a tooan műveletet, vagy egy API-tooallow-tartományok webböngésző-alapú ügyfelek hívásait.</span><span class="sxs-lookup"><span data-stu-id="adab5-127">hello `cors` policy adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="adab5-128">A CORS lehetővé teszi, hogy egy böngésző és egy kiszolgáló toointeract, és határozza meg, függetlenül attól, tooallow adott eltérő eredetű kérések (azaz XMLHttpRequests hívásoknak JavaScript egy weblap tooother tartományokban).</span><span class="sxs-lookup"><span data-stu-id="adab5-128">CORS allows a browser and a server toointeract and determine whether or not tooallow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page tooother domains).</span></span> <span data-ttu-id="adab5-129">Ez lehetővé teszi, hogy csak az azonos eredetű kérések engedélyezése-nál nagyobb rugalmasságot, de biztonságosabb, mint az összes eltérő eredetű kérések engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="adab5-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="adab5-130">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="adab5-130">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="adab5-131">Példa</span><span class="sxs-lookup"><span data-stu-id="adab5-131">Example</span></span>  
 <span data-ttu-id="adab5-132">Ez a példa bemutatja, hogyan toosupport előtti repülési kér, például az egyéni fejlécek vagy nem GET vagy POST metódusok.</span><span class="sxs-lookup"><span data-stu-id="adab5-132">This example demonstrates how toosupport pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="adab5-133">toosupport egyéni fejlécek és további HTTP-műveletek használata hello `allowed-methods` és `allowed-headers` látható módon hello a következő példában szakaszok.</span><span class="sxs-lookup"><span data-stu-id="adab5-133">toosupport custom headers and additional HTTP verbs, use hello `allowed-methods` and `allowed-headers` sections as shown in hello following example.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="adab5-134">Elemek</span><span class="sxs-lookup"><span data-stu-id="adab5-134">Elements</span></span>  
  
|<span data-ttu-id="adab5-135">Név</span><span class="sxs-lookup"><span data-stu-id="adab5-135">Name</span></span>|<span data-ttu-id="adab5-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="adab5-136">Description</span></span>|<span data-ttu-id="adab5-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adab5-137">Required</span></span>|<span data-ttu-id="adab5-138">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="adab5-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="adab5-139">a cors</span><span class="sxs-lookup"><span data-stu-id="adab5-139">cors</span></span>|<span data-ttu-id="adab5-140">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="adab5-140">Root element.</span></span>|<span data-ttu-id="adab5-141">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-141">Yes</span></span>|<span data-ttu-id="adab5-142">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-142">N/A</span></span>|  
|<span data-ttu-id="adab5-143">engedélyezett források</span><span class="sxs-lookup"><span data-stu-id="adab5-143">allowed-origins</span></span>|<span data-ttu-id="adab5-144">Tartalmaz `origin` engedélyezett eredetet a tartományok közötti kérelmek hello leíró elemek.</span><span class="sxs-lookup"><span data-stu-id="adab5-144">Contains `origin` elements that describe hello allowed origins for cross-domain requests.</span></span> <span data-ttu-id="adab5-145">`allowed-origins`vagy egyetlen tartalmazhat `origin` megadó elem `*` tooallow minden eredet, vagy egy vagy több `origin` egy URI-t tartalmazó elemek.</span><span class="sxs-lookup"><span data-stu-id="adab5-145">`allowed-origins` can contain either a single `origin` element that specifies `*` tooallow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="adab5-146">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-146">Yes</span></span>|<span data-ttu-id="adab5-147">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-147">N/A</span></span>|  
|<span data-ttu-id="adab5-148">Forrás</span><span class="sxs-lookup"><span data-stu-id="adab5-148">origin</span></span>|<span data-ttu-id="adab5-149">hello érték lehet `*` tooallow minden eredet vagy URI, amely meghatározza egy egyetlen forrás.</span><span class="sxs-lookup"><span data-stu-id="adab5-149">hello value can be either `*` tooallow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="adab5-150">hello URI tartalmaznia kell egy protokollt, a gazdagép és a port.</span><span class="sxs-lookup"><span data-stu-id="adab5-150">hello URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="adab5-151">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-151">Yes</span></span>|<span data-ttu-id="adab5-152">Ha hello port nincs megadva az URI, 80-as portot használja HTTP és a 443-as portot használja HTTPS.</span><span class="sxs-lookup"><span data-stu-id="adab5-152">If hello port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="adab5-153">engedélyezett módszerek</span><span class="sxs-lookup"><span data-stu-id="adab5-153">allowed-methods</span></span>|<span data-ttu-id="adab5-154">Ez az elem szükség, ha a módszerek nem GET vagy POST engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="adab5-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="adab5-155">Tartalmaz `method` elemei, amelyek megadják a hello támogatott HTTP-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="adab5-155">Contains `method` elements that specify hello supported HTTP verbs.</span></span>|<span data-ttu-id="adab5-156">Nem</span><span class="sxs-lookup"><span data-stu-id="adab5-156">No</span></span>|<span data-ttu-id="adab5-157">Ha ez a szakasz nincs jelen, a GET és POST támogatott.</span><span class="sxs-lookup"><span data-stu-id="adab5-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="adab5-158">Módszer</span><span class="sxs-lookup"><span data-stu-id="adab5-158">method</span></span>|<span data-ttu-id="adab5-159">Adja meg a HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="adab5-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="adab5-160">Legalább egy `method` elemet kell megadni, ha hello `allowed-methods` szakaszt.</span><span class="sxs-lookup"><span data-stu-id="adab5-160">At least one `method` element is required if hello `allowed-methods` section is present.</span></span>|<span data-ttu-id="adab5-161">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-161">N/A</span></span>|  
|<span data-ttu-id="adab5-162">engedélyezett fejlécek</span><span class="sxs-lookup"><span data-stu-id="adab5-162">allowed-headers</span></span>|<span data-ttu-id="adab5-163">Ez az elem tartalmaz `header` elemek hello fejlécek hello kérelemben szereplő nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="adab5-163">This element contains `header` elements specifying names of hello headers that can be included in hello request.</span></span>|<span data-ttu-id="adab5-164">Nem</span><span class="sxs-lookup"><span data-stu-id="adab5-164">No</span></span>|<span data-ttu-id="adab5-165">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-165">N/A</span></span>|  
|<span data-ttu-id="adab5-166">a fejlécek elérhetővé tétele</span><span class="sxs-lookup"><span data-stu-id="adab5-166">expose-headers</span></span>|<span data-ttu-id="adab5-167">Ez az elem tartalmaz `header` elemek hello fejlécek hello ügyfél által hozzáférhető nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="adab5-167">This element contains `header` elements specifying names of hello headers that will be accessible by hello client.</span></span>|<span data-ttu-id="adab5-168">Nem</span><span class="sxs-lookup"><span data-stu-id="adab5-168">No</span></span>|<span data-ttu-id="adab5-169">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-169">N/A</span></span>|  
|<span data-ttu-id="adab5-170">header</span><span class="sxs-lookup"><span data-stu-id="adab5-170">header</span></span>|<span data-ttu-id="adab5-171">Meghatározza a fejléc neve.</span><span class="sxs-lookup"><span data-stu-id="adab5-171">Specifies a header name.</span></span>|<span data-ttu-id="adab5-172">Legalább egy `header` elemnek kell szerepelnie a `allowed-headers` vagy `expose-headers` Ha hello szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="adab5-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if hello section is present.</span></span>|<span data-ttu-id="adab5-173">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="adab5-174">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="adab5-174">Attributes</span></span>  
  
|<span data-ttu-id="adab5-175">Név</span><span class="sxs-lookup"><span data-stu-id="adab5-175">Name</span></span>|<span data-ttu-id="adab5-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="adab5-176">Description</span></span>|<span data-ttu-id="adab5-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adab5-177">Required</span></span>|<span data-ttu-id="adab5-178">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="adab5-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="adab5-179">hitelesítő adatok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="adab5-179">allow-credentials</span></span>|<span data-ttu-id="adab5-180">Hello `Access-Control-Allow-Credentials` fejléc a következő hello elővizsgálati válaszok set toohello értéke ennek az attribútumnak és hello ügyfél képes toosubmit hitelesítő adatait a tartományok közötti kérelmek hatással.</span><span class="sxs-lookup"><span data-stu-id="adab5-180">hello `Access-Control-Allow-Credentials` header in hello preflight response will be set toohello value of this attribute and affect hello client’s ability toosubmit credentials in cross-domain requests.</span></span>|<span data-ttu-id="adab5-181">Nem</span><span class="sxs-lookup"><span data-stu-id="adab5-181">No</span></span>|<span data-ttu-id="adab5-182">hamis</span><span class="sxs-lookup"><span data-stu-id="adab5-182">false</span></span>|  
|<span data-ttu-id="adab5-183">ellenőrzés-result-maximális-kor</span><span class="sxs-lookup"><span data-stu-id="adab5-183">preflight-result-max-age</span></span>|<span data-ttu-id="adab5-184">Hello `Access-Control-Max-Age` fejléc a következő hello elővizsgálati válaszok set toohello értéke ennek az attribútumnak és hello felhasználói ügynök képes toocache előtti repülési válasz hatással.</span><span class="sxs-lookup"><span data-stu-id="adab5-184">hello `Access-Control-Max-Age` header in hello preflight response will be set toohello value of this attribute and affect hello user agent’s ability toocache pre-flight response.</span></span>|<span data-ttu-id="adab5-185">Nem</span><span class="sxs-lookup"><span data-stu-id="adab5-185">No</span></span>|<span data-ttu-id="adab5-186">0</span><span class="sxs-lookup"><span data-stu-id="adab5-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="adab5-187">Használat</span><span class="sxs-lookup"><span data-stu-id="adab5-187">Usage</span></span>  
 <span data-ttu-id="adab5-188">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="adab5-188">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="adab5-189">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="adab5-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="adab5-190">**Házirend hatókörök:** API-művelet</span><span class="sxs-lookup"><span data-stu-id="adab5-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="adab5-191"><a name="JSONP"></a>JSONP</span><span class="sxs-lookup"><span data-stu-id="adab5-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="adab5-192">Hello `jsonp` házirend JSON hozzáadja a kitöltési (JSONP) támogatási tooan művelet vagy egy API-tooallow tartományokon átívelő JavaScript-ügyfelekből böngészőalapú hívások.</span><span class="sxs-lookup"><span data-stu-id="adab5-192">hello `jsonp` policy adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="adab5-193">JSONP a JavaScript programok toorequest adatait egy másik tartományban lévő kiszolgálóhoz használt módszer.</span><span class="sxs-lookup"><span data-stu-id="adab5-193">JSONP is a method used in JavaScript programs toorequest data from a server in a different domain.</span></span> <span data-ttu-id="adab5-194">JSONP megkerüli hello korlátozás kikényszeríti a legtöbb webböngészővel, ahol tooweb lapok hello kell ugyanabban a tartományban.</span><span class="sxs-lookup"><span data-stu-id="adab5-194">JSONP bypasses hello limitation enforced by most web browsers where access tooweb pages must be in hello same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="adab5-195">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="adab5-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="adab5-196">Példa</span><span class="sxs-lookup"><span data-stu-id="adab5-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="adab5-197">Hello visszahívási paraméter nélkül hello metódus hívásakor? cb = XXX egyszerű JSON (nélkül függvény hívása burkoló) ad vissza.</span><span class="sxs-lookup"><span data-stu-id="adab5-197">If you call hello method without hello callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="adab5-198">Ha hello visszahívási paraméter hozzáadása `?cb=XXX` JSONP eredményeként alkalmazásburkoló hello eredeti JSON eredmények körül hello visszahívási függvény például ad vissza`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="adab5-198">If you add hello callback parameter `?cb=XXX` it will return a JSONP result, wrapping hello original JSON results around hello callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="adab5-199">Elemek</span><span class="sxs-lookup"><span data-stu-id="adab5-199">Elements</span></span>  
  
|<span data-ttu-id="adab5-200">Név</span><span class="sxs-lookup"><span data-stu-id="adab5-200">Name</span></span>|<span data-ttu-id="adab5-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="adab5-201">Description</span></span>|<span data-ttu-id="adab5-202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adab5-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="adab5-203">jsonp</span><span class="sxs-lookup"><span data-stu-id="adab5-203">jsonp</span></span>|<span data-ttu-id="adab5-204">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="adab5-204">Root element.</span></span>|<span data-ttu-id="adab5-205">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="adab5-206">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="adab5-206">Attributes</span></span>  
  
|<span data-ttu-id="adab5-207">Név</span><span class="sxs-lookup"><span data-stu-id="adab5-207">Name</span></span>|<span data-ttu-id="adab5-208">Leírás</span><span class="sxs-lookup"><span data-stu-id="adab5-208">Description</span></span>|<span data-ttu-id="adab5-209">Szükséges</span><span class="sxs-lookup"><span data-stu-id="adab5-209">Required</span></span>|<span data-ttu-id="adab5-210">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="adab5-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="adab5-211">a visszahívási paraméternév</span><span class="sxs-lookup"><span data-stu-id="adab5-211">callback-parameter-name</span></span>|<span data-ttu-id="adab5-212">hello tartományokon átívelő JavaScript függvényhívást előtagú hello teljesen minősített tartománynevét hello függvény tároló.</span><span class="sxs-lookup"><span data-stu-id="adab5-212">hello cross-domain JavaScript function call prefixed with hello fully qualified domain name where hello function resides.</span></span>|<span data-ttu-id="adab5-213">Igen</span><span class="sxs-lookup"><span data-stu-id="adab5-213">Yes</span></span>|<span data-ttu-id="adab5-214">N/A</span><span class="sxs-lookup"><span data-stu-id="adab5-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="adab5-215">Használat</span><span class="sxs-lookup"><span data-stu-id="adab5-215">Usage</span></span>  
 <span data-ttu-id="adab5-216">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="adab5-216">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="adab5-217">**Házirend szakaszok:** kimenő</span><span class="sxs-lookup"><span data-stu-id="adab5-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="adab5-218">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="adab5-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="adab5-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adab5-219">Next steps</span></span>
<span data-ttu-id="adab5-220">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="adab5-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  