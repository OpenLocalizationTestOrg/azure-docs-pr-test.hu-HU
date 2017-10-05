---
title: "Az Azure API Management tartományközi házirendjei |} Microsoft Docs"
description: "További tudnivalók a tartományok közötti házirendek az Azure API Management használható."
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
ms.openlocfilehash: ddca9e35b44a21294abbb5eaa4418bcdb85494cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="61700-103">Az API Management tartományközi házirendjei</span><span class="sxs-lookup"><span data-stu-id="61700-103">API Management cross domain policies</span></span>
<span data-ttu-id="61700-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="61700-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="61700-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="61700-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="61700-106"><a name="CrossDomainPolicies"></a>Kereszt-tartományi házirendek</span><span class="sxs-lookup"><span data-stu-id="61700-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="61700-107">[Lehetővé teszi a tartományok közötti hívások](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -elérhetővé válnak az API-t az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="61700-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="61700-108">[A CORS](api-management-cross-domain-policies.md#CORS) -hozzáadja az eltérő eredetű erőforrások megosztása (CORS) a támogatási művelet vagy API lehetővé teszi a tartományok közötti hívások webböngésző-alapú ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="61700-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="61700-109">[JSONP](api-management-cross-domain-policies.md#JSONP) -JSON ad hozzá egy műveletet, vagy lehetővé teszi a tartományok közötti hívások JavaScript-ügyfelekből webböngésző-alapú API kitöltési (JSONP) támogatással.</span><span class="sxs-lookup"><span data-stu-id="61700-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="61700-110"><a name="AllowCrossDomainCalls"></a>Lehetővé teszi a tartományok közötti hívások</span><span class="sxs-lookup"><span data-stu-id="61700-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="61700-111">Használja a `cross-domain` házirend ahhoz, hogy elérhető az API-t az Adobe Flash és a Microsoft Silverlight webböngésző-alapú ügyfelektől.</span><span class="sxs-lookup"><span data-stu-id="61700-111">Use the `cross-domain` policy to make the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="61700-112">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="61700-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in the Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="61700-113">Példa</span><span class="sxs-lookup"><span data-stu-id="61700-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="61700-114">Elemek</span><span class="sxs-lookup"><span data-stu-id="61700-114">Elements</span></span>  
  
|<span data-ttu-id="61700-115">Név</span><span class="sxs-lookup"><span data-stu-id="61700-115">Name</span></span>|<span data-ttu-id="61700-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="61700-116">Description</span></span>|<span data-ttu-id="61700-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="61700-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="61700-118">tartományok közötti</span><span class="sxs-lookup"><span data-stu-id="61700-118">cross-domain</span></span>|<span data-ttu-id="61700-119">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="61700-119">Root element.</span></span> <span data-ttu-id="61700-120">Gyermekelemek meg kell felelnie a [Adobe tartományok közötti házirend specifikációnak](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="61700-120">Child elements must conform to the [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="61700-121">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="61700-122">Használat</span><span class="sxs-lookup"><span data-stu-id="61700-122">Usage</span></span>  
 <span data-ttu-id="61700-123">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="61700-123">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="61700-124">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="61700-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="61700-125">**Házirend hatókörök:** globális</span><span class="sxs-lookup"><span data-stu-id="61700-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="61700-126"><a name="CORS"></a>A CORS</span><span class="sxs-lookup"><span data-stu-id="61700-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="61700-127">A `cors` házirendnek eltérő eredetű erőforrások megosztása (CORS) a támogatási művelet vagy API lehetővé teszi a tartományok közötti hívások webböngésző-alapú ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="61700-127">The `cors` policy adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="61700-128">A CORS lehetővé teszi, hogy egy böngésző és a kiszolgáló működjön, és meghatározzák, hogy az adott eltérő eredetű kérések (azaz XMLHttpRequests hívásoknak JavaScript weblapon lévő más tartományokra) engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="61700-128">CORS allows a browser and a server to interact and determine whether or not to allow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page to other domains).</span></span> <span data-ttu-id="61700-129">Ez lehetővé teszi, hogy csak az azonos eredetű kérések engedélyezése-nál nagyobb rugalmasságot, de biztonságosabb, mint az összes eltérő eredetű kérések engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="61700-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="61700-130">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="61700-130">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="61700-131">Példa</span><span class="sxs-lookup"><span data-stu-id="61700-131">Example</span></span>  
 <span data-ttu-id="61700-132">Ez a példa bemutatja, hogyan előtti repülési kérések, például az egyéni fejlécek vagy nem GET vagy POST metódusok támogatására.</span><span class="sxs-lookup"><span data-stu-id="61700-132">This example demonstrates how to support pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="61700-133">Egyéni fejlécek és további HTTP-műveletek támogatásához használja a `allowed-methods` és `allowed-headers` szakasz a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="61700-133">To support custom headers and additional HTTP verbs, use the `allowed-methods` and `allowed-headers` sections as shown in the following example.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="61700-134">Elemek</span><span class="sxs-lookup"><span data-stu-id="61700-134">Elements</span></span>  
  
|<span data-ttu-id="61700-135">Név</span><span class="sxs-lookup"><span data-stu-id="61700-135">Name</span></span>|<span data-ttu-id="61700-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="61700-136">Description</span></span>|<span data-ttu-id="61700-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="61700-137">Required</span></span>|<span data-ttu-id="61700-138">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="61700-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="61700-139">a cors</span><span class="sxs-lookup"><span data-stu-id="61700-139">cors</span></span>|<span data-ttu-id="61700-140">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="61700-140">Root element.</span></span>|<span data-ttu-id="61700-141">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-141">Yes</span></span>|<span data-ttu-id="61700-142">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-142">N/A</span></span>|  
|<span data-ttu-id="61700-143">engedélyezett források</span><span class="sxs-lookup"><span data-stu-id="61700-143">allowed-origins</span></span>|<span data-ttu-id="61700-144">Tartalmaz `origin` elemek, amelyek a tartományok közötti kérelmek engedélyezett eredetet ismertetik.</span><span class="sxs-lookup"><span data-stu-id="61700-144">Contains `origin` elements that describe the allowed origins for cross-domain requests.</span></span> <span data-ttu-id="61700-145">`allowed-origins`vagy egyetlen tartalmazhat `origin` megadó elem `*` engedélyezéséhez a forrás vagy egy vagy több `origin` egy URI-t tartalmazó elemek.</span><span class="sxs-lookup"><span data-stu-id="61700-145">`allowed-origins` can contain either a single `origin` element that specifies `*` to allow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="61700-146">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-146">Yes</span></span>|<span data-ttu-id="61700-147">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-147">N/A</span></span>|  
|<span data-ttu-id="61700-148">Forrás</span><span class="sxs-lookup"><span data-stu-id="61700-148">origin</span></span>|<span data-ttu-id="61700-149">Az érték lehet `*` minden eredet vagy URI, amely meghatározza egy egyetlen forrás.</span><span class="sxs-lookup"><span data-stu-id="61700-149">The value can be either `*` to allow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="61700-150">Az URI tartalmaznia kell egy protokollt, a gazdagép és a port.</span><span class="sxs-lookup"><span data-stu-id="61700-150">The URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="61700-151">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-151">Yes</span></span>|<span data-ttu-id="61700-152">Ha a port nincs megadva az URI, 80-as portot használja HTTP és a 443-as portot használja HTTPS.</span><span class="sxs-lookup"><span data-stu-id="61700-152">If the port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="61700-153">engedélyezett módszerek</span><span class="sxs-lookup"><span data-stu-id="61700-153">allowed-methods</span></span>|<span data-ttu-id="61700-154">Ez az elem szükség, ha a módszerek nem GET vagy POST engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="61700-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="61700-155">Tartalmaz `method` elemeit, adja meg a támogatott HTTP-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="61700-155">Contains `method` elements that specify the supported HTTP verbs.</span></span>|<span data-ttu-id="61700-156">Nem</span><span class="sxs-lookup"><span data-stu-id="61700-156">No</span></span>|<span data-ttu-id="61700-157">Ha ez a szakasz nincs jelen, a GET és POST támogatott.</span><span class="sxs-lookup"><span data-stu-id="61700-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="61700-158">Módszer</span><span class="sxs-lookup"><span data-stu-id="61700-158">method</span></span>|<span data-ttu-id="61700-159">Adja meg a HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="61700-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="61700-160">Legalább egy `method` elemet kell megadni, ha a `allowed-methods` szakaszt.</span><span class="sxs-lookup"><span data-stu-id="61700-160">At least one `method` element is required if the `allowed-methods` section is present.</span></span>|<span data-ttu-id="61700-161">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-161">N/A</span></span>|  
|<span data-ttu-id="61700-162">engedélyezett fejlécek</span><span class="sxs-lookup"><span data-stu-id="61700-162">allowed-headers</span></span>|<span data-ttu-id="61700-163">Ez az elem tartalmaz `header` elemek a fejlécek, a kérelemben szereplő nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="61700-163">This element contains `header` elements specifying names of the headers that can be included in the request.</span></span>|<span data-ttu-id="61700-164">Nem</span><span class="sxs-lookup"><span data-stu-id="61700-164">No</span></span>|<span data-ttu-id="61700-165">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-165">N/A</span></span>|  
|<span data-ttu-id="61700-166">a fejlécek elérhetővé tétele</span><span class="sxs-lookup"><span data-stu-id="61700-166">expose-headers</span></span>|<span data-ttu-id="61700-167">Ez az elem tartalmaz `header` elemek a fejlécek, az ügyfél által hozzáférhető nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="61700-167">This element contains `header` elements specifying names of the headers that will be accessible by the client.</span></span>|<span data-ttu-id="61700-168">Nem</span><span class="sxs-lookup"><span data-stu-id="61700-168">No</span></span>|<span data-ttu-id="61700-169">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-169">N/A</span></span>|  
|<span data-ttu-id="61700-170">header</span><span class="sxs-lookup"><span data-stu-id="61700-170">header</span></span>|<span data-ttu-id="61700-171">Meghatározza a fejléc neve.</span><span class="sxs-lookup"><span data-stu-id="61700-171">Specifies a header name.</span></span>|<span data-ttu-id="61700-172">Legalább egy `header` elemnek kell szerepelnie a `allowed-headers` vagy `expose-headers` Ha a szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="61700-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if the section is present.</span></span>|<span data-ttu-id="61700-173">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="61700-174">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="61700-174">Attributes</span></span>  
  
|<span data-ttu-id="61700-175">Név</span><span class="sxs-lookup"><span data-stu-id="61700-175">Name</span></span>|<span data-ttu-id="61700-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="61700-176">Description</span></span>|<span data-ttu-id="61700-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="61700-177">Required</span></span>|<span data-ttu-id="61700-178">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="61700-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="61700-179">hitelesítő adatok engedélyezése</span><span class="sxs-lookup"><span data-stu-id="61700-179">allow-credentials</span></span>|<span data-ttu-id="61700-180">A `Access-Control-Allow-Credentials` fejléc a következő ellenőrzés válasz Ez az attribútum értékét úgy lesz beállítva, és hatással az ügyfél hitelesítő adatait, a tartományok közötti kérelmek elküldése.</span><span class="sxs-lookup"><span data-stu-id="61700-180">The `Access-Control-Allow-Credentials` header in the preflight response will be set to the value of this attribute and affect the client’s ability to submit credentials in cross-domain requests.</span></span>|<span data-ttu-id="61700-181">Nem</span><span class="sxs-lookup"><span data-stu-id="61700-181">No</span></span>|<span data-ttu-id="61700-182">hamis</span><span class="sxs-lookup"><span data-stu-id="61700-182">false</span></span>|  
|<span data-ttu-id="61700-183">ellenőrzés-result-maximális-kor</span><span class="sxs-lookup"><span data-stu-id="61700-183">preflight-result-max-age</span></span>|<span data-ttu-id="61700-184">A `Access-Control-Max-Age` fejléc a következő ellenőrzés válasz Ez az attribútum értékét úgy lesz beállítva, és a felhasználói ügynök képességét, gyorsítótári előtti repülési választ.</span><span class="sxs-lookup"><span data-stu-id="61700-184">The `Access-Control-Max-Age` header in the preflight response will be set to the value of this attribute and affect the user agent’s ability to cache pre-flight response.</span></span>|<span data-ttu-id="61700-185">Nem</span><span class="sxs-lookup"><span data-stu-id="61700-185">No</span></span>|<span data-ttu-id="61700-186">0</span><span class="sxs-lookup"><span data-stu-id="61700-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="61700-187">Használat</span><span class="sxs-lookup"><span data-stu-id="61700-187">Usage</span></span>  
 <span data-ttu-id="61700-188">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="61700-188">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="61700-189">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="61700-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="61700-190">**Házirend hatókörök:** API-művelet</span><span class="sxs-lookup"><span data-stu-id="61700-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="61700-191"><a name="JSONP"></a>JSONP</span><span class="sxs-lookup"><span data-stu-id="61700-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="61700-192">A `jsonp` házirend ad JSON (JSONP) kitöltési-támogatással rendelkező művelet vagy API lehetővé teszi a tartományok közötti hívások JavaScript-ügyfelekből webböngésző-alapú.</span><span class="sxs-lookup"><span data-stu-id="61700-192">The `jsonp` policy adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="61700-193">JSONP módszere a JavaScript programok kérelem adatai egy kiszolgálóról egy másik tartományban.</span><span class="sxs-lookup"><span data-stu-id="61700-193">JSONP is a method used in JavaScript programs to request data from a server in a different domain.</span></span> <span data-ttu-id="61700-194">JSONP megkerüli a korlátozás kikényszeríti a legtöbb webböngészővel, ahol a weblapok elérése ugyanabban a tartományban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="61700-194">JSONP bypasses the limitation enforced by most web browsers where access to web pages must be in the same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="61700-195">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="61700-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="61700-196">Példa</span><span class="sxs-lookup"><span data-stu-id="61700-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="61700-197">A visszahívási paraméter nélkül. a metódus hívásakor? cb = XXX egyszerű JSON (nélkül függvény hívása burkoló) ad vissza.</span><span class="sxs-lookup"><span data-stu-id="61700-197">If you call the method without the callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="61700-198">Ha a visszahívási paraméter hozzáadása `?cb=XXX` JSONP eredményt ad vissza, az eredményeket a visszahívási függvény körül, például az eredeti JSON alkalmazásburkoló`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="61700-198">If you add the callback parameter `?cb=XXX` it will return a JSONP result, wrapping the original JSON results around the callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="61700-199">Elemek</span><span class="sxs-lookup"><span data-stu-id="61700-199">Elements</span></span>  
  
|<span data-ttu-id="61700-200">Név</span><span class="sxs-lookup"><span data-stu-id="61700-200">Name</span></span>|<span data-ttu-id="61700-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="61700-201">Description</span></span>|<span data-ttu-id="61700-202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="61700-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="61700-203">jsonp</span><span class="sxs-lookup"><span data-stu-id="61700-203">jsonp</span></span>|<span data-ttu-id="61700-204">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="61700-204">Root element.</span></span>|<span data-ttu-id="61700-205">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="61700-206">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="61700-206">Attributes</span></span>  
  
|<span data-ttu-id="61700-207">Név</span><span class="sxs-lookup"><span data-stu-id="61700-207">Name</span></span>|<span data-ttu-id="61700-208">Leírás</span><span class="sxs-lookup"><span data-stu-id="61700-208">Description</span></span>|<span data-ttu-id="61700-209">Szükséges</span><span class="sxs-lookup"><span data-stu-id="61700-209">Required</span></span>|<span data-ttu-id="61700-210">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="61700-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="61700-211">a visszahívási paraméternév</span><span class="sxs-lookup"><span data-stu-id="61700-211">callback-parameter-name</span></span>|<span data-ttu-id="61700-212">A tartományokon átívelő JavaScript függvény hívása a teljesen minősített tartománynevét, amelyen a függvényt található előtagként.</span><span class="sxs-lookup"><span data-stu-id="61700-212">The cross-domain JavaScript function call prefixed with the fully qualified domain name where the function resides.</span></span>|<span data-ttu-id="61700-213">Igen</span><span class="sxs-lookup"><span data-stu-id="61700-213">Yes</span></span>|<span data-ttu-id="61700-214">N/A</span><span class="sxs-lookup"><span data-stu-id="61700-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="61700-215">Használat</span><span class="sxs-lookup"><span data-stu-id="61700-215">Usage</span></span>  
 <span data-ttu-id="61700-216">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="61700-216">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="61700-217">**Házirend szakaszok:** kimenő</span><span class="sxs-lookup"><span data-stu-id="61700-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="61700-218">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="61700-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="61700-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61700-219">Next steps</span></span>
<span data-ttu-id="61700-220">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="61700-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  