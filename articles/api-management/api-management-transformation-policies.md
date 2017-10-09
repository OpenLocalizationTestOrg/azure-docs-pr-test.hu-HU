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
# <a name="api-management-transformation-policies"></a><span data-ttu-id="aedb9-103">Az API Management-átalakítási csoportházirendek</span><span class="sxs-lookup"><span data-stu-id="aedb9-103">API Management transformation policies</span></span>
<span data-ttu-id="aedb9-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="aedb9-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="aedb9-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="aedb9-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="aedb9-106"><a name="TransformationPolicies"></a>Átalakítási csoportházirendek</span><span class="sxs-lookup"><span data-stu-id="aedb9-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="aedb9-107">[Alakítsa át a JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - konvertálja kérés vagy válasz JSON tooXML a body.</span><span class="sxs-lookup"><span data-stu-id="aedb9-107">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
-   <span data-ttu-id="aedb9-108">[Átalakítás XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - konvertálja kérés vagy válasz XML tooJSON a body.</span><span class="sxs-lookup"><span data-stu-id="aedb9-108">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
-   <span data-ttu-id="aedb9-109">[Keresés és csere törzsében karakterlánc](api-management-transformation-policies.md#Findandreplacestringinbody) - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="aedb9-110">[A tartalom URL-címek maszkolnia](api-management-transformation-policies.md#MaskURLSContent) -átírja (maszkok) hivatkozások hello válaszul body, így a pontok toohello egyenértékű hivatkozás hello átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="aedb9-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
-   <span data-ttu-id="aedb9-111">[Állítsa be háttérszolgáltatás](api-management-transformation-policies.md#SetBackendService) -hello háttérszolgáltatást egy bejövő kérelemhez változik.</span><span class="sxs-lookup"><span data-stu-id="aedb9-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="aedb9-112">[Állítsa be a szervezet](api-management-transformation-policies.md#SetBody) – beállítja a bejövő és kimenő kérelmek hello az üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="aedb9-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="aedb9-113">[Set HTTP-fejléc](api-management-transformation-policies.md#SetHTTPheader) - hozzárendel egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="aedb9-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="aedb9-114">[Állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="aedb9-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="aedb9-115">[URL-cím újraírása](api-management-transformation-policies.md#RewriteURL) -nyilvános űrlap toohello formájukban hello webszolgáltatás által várt a kérelem URL-cím alakítja.</span><span class="sxs-lookup"><span data-stu-id="aedb9-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
-   <span data-ttu-id="aedb9-116">[Átalakítás XSLT használatával XML](api-management-transformation-policies.md#XSLTransform) -egy XSL átalakítása tooXML hello kérés vagy válasz törzsében vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="aedb9-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
##  <span data-ttu-id="aedb9-117"><a name="ConvertJSONtoXML"></a>Alakítsa át a JSON tooXML</span><span class="sxs-lookup"><span data-stu-id="aedb9-117"><a name="ConvertJSONtoXML"></a> Convert JSON tooXML</span></span>  
 <span data-ttu-id="aedb9-118">Hello `json-to-xml` házirend kérés vagy válasz törzsében konvertálja a JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="aedb9-118">hello `json-to-xml` policy converts a request or response body from JSON tooXML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-119">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-120">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-120">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="aedb9-121">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-121">Elements</span></span>  
  
|<span data-ttu-id="aedb9-122">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-122">Name</span></span>|<span data-ttu-id="aedb9-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-123">Description</span></span>|<span data-ttu-id="aedb9-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-125">JSON-xml</span><span class="sxs-lookup"><span data-stu-id="aedb9-125">json-to-xml</span></span>|<span data-ttu-id="aedb9-126">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-126">Root element.</span></span>|<span data-ttu-id="aedb9-127">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="aedb9-128">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="aedb9-128">Attributes</span></span>  
  
|<span data-ttu-id="aedb9-129">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-129">Name</span></span>|<span data-ttu-id="aedb9-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-130">Description</span></span>|<span data-ttu-id="aedb9-131">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-131">Required</span></span>|<span data-ttu-id="aedb9-132">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-133">alkalmazása</span><span class="sxs-lookup"><span data-stu-id="aedb9-133">apply</span></span>|<span data-ttu-id="aedb9-134">hello attribútumot a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-134">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-135">átalakítás - mindig - mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="aedb9-136">-tartalom típus-json - konvertálás csak akkor, ha a response Content-Type fejléc JSON jelenlétét jelzi.</span><span class="sxs-lookup"><span data-stu-id="aedb9-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="aedb9-137">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-137">Yes</span></span>|<span data-ttu-id="aedb9-138">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-138">N/A</span></span>|  
|<span data-ttu-id="aedb9-139">Vegye figyelembe-elfogadja-fejléc</span><span class="sxs-lookup"><span data-stu-id="aedb9-139">consider-accept-header</span></span>|<span data-ttu-id="aedb9-140">hello attribútumot a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-140">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-141">átalakítás - igaz - alkalmazni, amennyiben JSON kérelem Accept fejlécet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="aedb9-142">-hamis - átalakítás mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="aedb9-143">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-143">No</span></span>|<span data-ttu-id="aedb9-144">Igaz</span><span class="sxs-lookup"><span data-stu-id="aedb9-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-145">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-145">Usage</span></span>  
 <span data-ttu-id="aedb9-146">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-146">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-147">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="aedb9-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="aedb9-148">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-149"><a name="ConvertXMLtoJSON"></a>XML-tooJSON átalakítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-149"><a name="ConvertXMLtoJSON"></a> Convert XML tooJSON</span></span>  
 <span data-ttu-id="aedb9-150">Hello `xml-to-json` házirend XML tooJSON kérés vagy válasz törzsében konvertál.</span><span class="sxs-lookup"><span data-stu-id="aedb9-150">hello `xml-to-json` policy converts a request or response body from XML tooJSON.</span></span> <span data-ttu-id="aedb9-151">Ez a házirend API-k alapján háttér csak XML-webszolgáltatások használt toomodernize lehet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-151">This policy can be used toomodernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-152">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-153">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-153">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="aedb9-154">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-154">Elements</span></span>  
  
|<span data-ttu-id="aedb9-155">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-155">Name</span></span>|<span data-ttu-id="aedb9-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-156">Description</span></span>|<span data-ttu-id="aedb9-157">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-158">XML-json</span><span class="sxs-lookup"><span data-stu-id="aedb9-158">xml-to-json</span></span>|<span data-ttu-id="aedb9-159">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-159">Root element.</span></span>|<span data-ttu-id="aedb9-160">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="aedb9-161">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="aedb9-161">Attributes</span></span>  
  
|<span data-ttu-id="aedb9-162">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-162">Name</span></span>|<span data-ttu-id="aedb9-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-163">Description</span></span>|<span data-ttu-id="aedb9-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-164">Required</span></span>|<span data-ttu-id="aedb9-165">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-166">típusa</span><span class="sxs-lookup"><span data-stu-id="aedb9-166">kind</span></span>|<span data-ttu-id="aedb9-167">hello attribútumot a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-167">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-168">-javascript-barát - hello alakítja át a JSON egy űrlap felhasználóbarát tooJavaScript fejlesztők rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="aedb9-168">-   javascript-friendly - hello converted JSON has a form friendly tooJavaScript developers.</span></span><br /><span data-ttu-id="aedb9-169">-közvetlen – hello konvertált JSON tükrözi hello eredeti XML-dokumentum struktúra.</span><span class="sxs-lookup"><span data-stu-id="aedb9-169">-   direct - hello converted JSON reflects hello original XML document's structure.</span></span>|<span data-ttu-id="aedb9-170">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-170">Yes</span></span>|<span data-ttu-id="aedb9-171">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-171">N/A</span></span>|  
|<span data-ttu-id="aedb9-172">alkalmazása</span><span class="sxs-lookup"><span data-stu-id="aedb9-172">apply</span></span>|<span data-ttu-id="aedb9-173">hello attribútumot a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-173">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-174">-mindig - átalakítás mindig.</span><span class="sxs-lookup"><span data-stu-id="aedb9-174">-   always - convert always.</span></span><br /><span data-ttu-id="aedb9-175">-tartalom típus-xml - konvertálás csak akkor, ha a response Content-Type fejléc XML jelenlétét jelzi.</span><span class="sxs-lookup"><span data-stu-id="aedb9-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="aedb9-176">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-176">Yes</span></span>|<span data-ttu-id="aedb9-177">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-177">N/A</span></span>|  
|<span data-ttu-id="aedb9-178">Vegye figyelembe-elfogadja-fejléc</span><span class="sxs-lookup"><span data-stu-id="aedb9-178">consider-accept-header</span></span>|<span data-ttu-id="aedb9-179">hello attribútumot a következő értékek hello tooone kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-179">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-180">átalakítás - igaz - alkalmazni, amennyiben XML Accept fejlécet kérés van szükség.</span><span class="sxs-lookup"><span data-stu-id="aedb9-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="aedb9-181">-hamis - átalakítás mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="aedb9-182">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-182">No</span></span>|<span data-ttu-id="aedb9-183">Igaz</span><span class="sxs-lookup"><span data-stu-id="aedb9-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-184">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-184">Usage</span></span>  
 <span data-ttu-id="aedb9-185">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-185">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-186">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="aedb9-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="aedb9-187">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-188"><a name="Findandreplacestringinbody"></a>Keresés és csere törzsében karakterlánc</span><span class="sxs-lookup"><span data-stu-id="aedb9-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="aedb9-189">Hello `find-and-replace` házirend kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-189">hello `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-190">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-191">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="aedb9-192">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-192">Elements</span></span>  
  
|<span data-ttu-id="aedb9-193">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-193">Name</span></span>|<span data-ttu-id="aedb9-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-194">Description</span></span>|<span data-ttu-id="aedb9-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-196">keresése és cseréje</span><span class="sxs-lookup"><span data-stu-id="aedb9-196">find-and-replace</span></span>|<span data-ttu-id="aedb9-197">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-197">Root element.</span></span>|<span data-ttu-id="aedb9-198">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="aedb9-199">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="aedb9-199">Attributes</span></span>  
  
|<span data-ttu-id="aedb9-200">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-200">Name</span></span>|<span data-ttu-id="aedb9-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-201">Description</span></span>|<span data-ttu-id="aedb9-202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-202">Required</span></span>|<span data-ttu-id="aedb9-203">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-204">A</span><span class="sxs-lookup"><span data-stu-id="aedb9-204">from</span></span>|<span data-ttu-id="aedb9-205">hello karakterlánc toosearch számára.</span><span class="sxs-lookup"><span data-stu-id="aedb9-205">hello string toosearch for.</span></span>|<span data-ttu-id="aedb9-206">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-206">Yes</span></span>|<span data-ttu-id="aedb9-207">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-207">N/A</span></span>|  
|<span data-ttu-id="aedb9-208">erre:</span><span class="sxs-lookup"><span data-stu-id="aedb9-208">to</span></span>|<span data-ttu-id="aedb9-209">hello behelyettesítendő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="aedb9-209">hello replacement string.</span></span> <span data-ttu-id="aedb9-210">Adjon meg egy nulla hosszúságú helyettesítő karakterlánc tooremove hello keresési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="aedb9-210">Specify a zero length replacement string tooremove hello search string.</span></span>|<span data-ttu-id="aedb9-211">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-211">Yes</span></span>|<span data-ttu-id="aedb9-212">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-213">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-213">Usage</span></span>  
 <span data-ttu-id="aedb9-214">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-214">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-215">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="aedb9-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="aedb9-216">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-217"><a name="MaskURLSContent"></a>A tartalom maszk URL-címek</span><span class="sxs-lookup"><span data-stu-id="aedb9-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="aedb9-218">Hello `redirect-content-urls` házirend átírja hello válasz törzsében (maszkok) hivatkozásokat úgy, hogy azok pont toohello egyenértékű hivatkozás hello átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="aedb9-218">hello `redirect-content-urls` policy re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span> <span data-ttu-id="aedb9-219">A hello kimenő szakasz toore-írási válasz törzsében hivatkozások toomake őket pont toohello átjáró használatára.</span><span class="sxs-lookup"><span data-stu-id="aedb9-219">Use in hello outbound section toore-write response body links toomake them point toohello gateway.</span></span> <span data-ttu-id="aedb9-220">Hello használható bejövő ellentétes hatás szakasz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-220">Use in hello inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="aedb9-221">Ez a házirend nem változik, mint bármely térközkaraktert `Location` fejléceket.</span><span class="sxs-lookup"><span data-stu-id="aedb9-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="aedb9-222">toochange fejléc értékei, használja a hello [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend.</span><span class="sxs-lookup"><span data-stu-id="aedb9-222">toochange header values, use hello [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-223">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-224">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="aedb9-225">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-225">Elements</span></span>  
  
|<span data-ttu-id="aedb9-226">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-226">Name</span></span>|<span data-ttu-id="aedb9-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-227">Description</span></span>|<span data-ttu-id="aedb9-228">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-229">az átirányítási-tartalom-URL-címek</span><span class="sxs-lookup"><span data-stu-id="aedb9-229">redirect-content-urls</span></span>|<span data-ttu-id="aedb9-230">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-230">Root element.</span></span>|<span data-ttu-id="aedb9-231">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-232">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-232">Usage</span></span>  
 <span data-ttu-id="aedb9-233">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-233">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-234">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="aedb9-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="aedb9-235">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-236"><a name="SetBackendService"></a>Háttér-szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="aedb9-237">Használjon hello `set-backend-service` házirend tooredirect egy bejövő kérelem tooa különböző háttér, mint egy megadott hello API-beállítások az adott műveletre vonatkozó hello.</span><span class="sxs-lookup"><span data-stu-id="aedb9-237">Use hello `set-backend-service` policy tooredirect an incoming request tooa different backend than hello one specified in hello API settings for that operation.</span></span> <span data-ttu-id="aedb9-238">Ez a házirend hello háttér szolgáltatás alap URL-CÍMÉT hello bejövő kérelem toohello hello házirendben megadott változik.</span><span class="sxs-lookup"><span data-stu-id="aedb9-238">This policy changes hello backend service base URL of hello incoming request toohello one specified in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-239">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-240">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-240">Example</span></span>  
  
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
<span data-ttu-id="aedb9-241">Ez a példa hello a háttér-szolgáltatás szabály beállítása továbbítja a kérelmet hello lekérdezési karakterlánc tooa különböző háttérszolgáltatást hello egy meghatározott hello API mint az átadott hello verzióértéket alapján.</span><span class="sxs-lookup"><span data-stu-id="aedb9-241">In this example hello set backend service policy routes requests based on hello version value passed in hello query string tooa different backend service than hello one specified in hello API.</span></span>
  
<span data-ttu-id="aedb9-242">Kezdetben hello háttér alap URL-címe, amelyből származik hello API-beállítások.</span><span class="sxs-lookup"><span data-stu-id="aedb9-242">Initially hello backend service base URL is derived from hello API settings.</span></span> <span data-ttu-id="aedb9-243">Ezért a kérelem URL-címe hello `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` válik `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` ahol `http://contoso.com/api/10.4/` hello háttérkiszolgáló URL-címe a hello API-beállításaiban megadott.</span><span class="sxs-lookup"><span data-stu-id="aedb9-243">So hello request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is hello backend service URL specified in hello API settings.</span></span>  
  
<span data-ttu-id="aedb9-244">Ha hello [< válasszon\> ](api-management-advanced-policies.md#choose) alkalmazott házirend-utasítás hello háttér szolgáltatás alap URL-címet módosíthatja újra vagy túl`http://contoso.com/api/8.2` vagy `http://contoso.com/api/9.1`hello hello verzió kérelem lekérdezési paraméter értéke attól függően, hogy.</span><span class="sxs-lookup"><span data-stu-id="aedb9-244">When hello [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied hello backend service base URL may change again either too`http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on hello value of hello version request query parameter.</span></span> <span data-ttu-id="aedb9-245">Például ha hello értéke `"2013-15"` hello végső kérelem URL-cím lesz `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="aedb9-245">For example, if hello value is `"2013-15"` hello final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="aedb9-246">Ha további hello kérelem átalakítása kívánt, más [átalakítási csoportházirendek](api-management-transformation-policies.md#TransformationPolicies) is használható.</span><span class="sxs-lookup"><span data-stu-id="aedb9-246">If further transformation of hello request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="aedb9-247">Például tooremove hello verzió lekérdezési paraméter most, hogy hello kérelem folyamatban van irányítva tooa verziót adott háttér hello [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) házirend lehet használt tooremove hello most redundáns version attribútum.</span><span class="sxs-lookup"><span data-stu-id="aedb9-247">For example, tooremove hello version query parameter now that hello request is being routed tooa version specific backend, hello  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used tooremove hello now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="aedb9-248">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-248">Example</span></span>  
  
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
<span data-ttu-id="aedb9-249">A a példa hello házirend útvonalak hello kérelem tooa service fabric háttérbeli hello userId lekérdezési karakterlánc hello partíciós kulcs használatával, és segítségével hello hello partíció elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="aedb9-249">In this example hello policy routes hello request tooa service fabric backend, using hello userId query string as hello partition key and using hello primary replica of hello partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="aedb9-250">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-250">Elements</span></span>  
  
|<span data-ttu-id="aedb9-251">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-251">Name</span></span>|<span data-ttu-id="aedb9-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-252">Description</span></span>|<span data-ttu-id="aedb9-253">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-254">háttér-szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-254">set-backend-service</span></span>|<span data-ttu-id="aedb9-255">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-255">Root element.</span></span>|<span data-ttu-id="aedb9-256">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="aedb9-257">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="aedb9-257">Attributes</span></span>  
  
|<span data-ttu-id="aedb9-258">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-258">Name</span></span>|<span data-ttu-id="aedb9-259">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-259">Description</span></span>|<span data-ttu-id="aedb9-260">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-260">Required</span></span>|<span data-ttu-id="aedb9-261">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-262">alap-URL-cím</span><span class="sxs-lookup"><span data-stu-id="aedb9-262">base-url</span></span>|<span data-ttu-id="aedb9-263">Új háttér alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="aedb9-263">New backend service base URL.</span></span>|<span data-ttu-id="aedb9-264">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-264">No</span></span>|<span data-ttu-id="aedb9-265">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-265">N/A</span></span>|  
|<span data-ttu-id="aedb9-266">háttér-azonosító</span><span class="sxs-lookup"><span data-stu-id="aedb9-266">backend-id</span></span>|<span data-ttu-id="aedb9-267">Hello háttér tooroute az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="aedb9-267">Identifier of hello backend tooroute to.</span></span>|<span data-ttu-id="aedb9-268">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-268">No</span></span>|<span data-ttu-id="aedb9-269">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-269">N/A</span></span>|  
|<span data-ttu-id="aedb9-270">ú partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="aedb9-270">sf-partition-key</span></span>|<span data-ttu-id="aedb9-271">Csak érvényes hello háttér Service Fabric-szolgáltatás, és a "háttér-id" van megadva.</span><span class="sxs-lookup"><span data-stu-id="aedb9-271">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="aedb9-272">Használt tooresolve egy adott partícióra hello name resolution szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="aedb9-272">Used tooresolve a specific partition from hello name resolution service.</span></span>|<span data-ttu-id="aedb9-273">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-273">No</span></span>|<span data-ttu-id="aedb9-274">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-274">N/A</span></span>|  
|<span data-ttu-id="aedb9-275">ú-replika-típusa</span><span class="sxs-lookup"><span data-stu-id="aedb9-275">sf-replica-type</span></span>|<span data-ttu-id="aedb9-276">Csak érvényes hello háttér Service Fabric-szolgáltatás, és a "háttér-id" van megadva.</span><span class="sxs-lookup"><span data-stu-id="aedb9-276">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="aedb9-277">Szabályozza, hogy hello kérelem toohello elsődleges vagy másodlagos replikáján partíció kell lépjen.</span><span class="sxs-lookup"><span data-stu-id="aedb9-277">Controls if hello request should go toohello primary or secondary replica of a partition.</span></span> |<span data-ttu-id="aedb9-278">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-278">No</span></span>|<span data-ttu-id="aedb9-279">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-279">N/A</span></span>|    
|<span data-ttu-id="aedb9-280">ú-resolve-feltétel</span><span class="sxs-lookup"><span data-stu-id="aedb9-280">sf-resolve-condition</span></span>|<span data-ttu-id="aedb9-281">Csak érvényes hello háttér esetén a Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aedb9-281">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="aedb9-282">A feltétel azonosítja Ha hello hívás tooService háló háttér rendelkezik új megoldás ismételni toobe.</span><span class="sxs-lookup"><span data-stu-id="aedb9-282">Condition identifying if hello call tooService Fabric backend has toobe repeated with new resolution.</span></span>|<span data-ttu-id="aedb9-283">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-283">No</span></span>|<span data-ttu-id="aedb9-284">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-284">N/A</span></span>|    
|<span data-ttu-id="aedb9-285">ú-példány-szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="aedb9-285">sf-service-instance-name</span></span>|<span data-ttu-id="aedb9-286">Csak érvényes hello háttér esetén a Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="aedb9-286">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="aedb9-287">Lehetővé teszi, hogy toochange szolgáltatáspéldány futásidőben.</span><span class="sxs-lookup"><span data-stu-id="aedb9-287">Allows toochange service instances at runtime.</span></span> |<span data-ttu-id="aedb9-288">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-288">No</span></span>|<span data-ttu-id="aedb9-289">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="aedb9-290">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-290">Usage</span></span>  
 <span data-ttu-id="aedb9-291">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-291">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-292">**Házirend szakaszok:** bejövő, háttér</span><span class="sxs-lookup"><span data-stu-id="aedb9-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="aedb9-293">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-294"><a name="SetBody"></a>Set törzse</span><span class="sxs-lookup"><span data-stu-id="aedb9-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="aedb9-295">Használjon hello `set-body` házirend tooset hello üzenettörzs a bejövő és kimenő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-295">Use hello `set-body` policy tooset hello message body for incoming and outgoing requests.</span></span> <span data-ttu-id="aedb9-296">tooaccess hello hello is használhatja az üzenet törzse `context.Request.Body` tulajdonság vagy hello `context.Response.Body`, attól függően, hogy hello házirend van hello bejövő vagy kimenő szakasz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-296">tooaccess hello message body you can use hello `context.Request.Body` property or hello `context.Response.Body`, depending on whether hello policy is in hello inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="aedb9-297">Vegye figyelembe, hogy amikor hozzáfér alapértelmezés szerint hello üzenet törzsének használatával `context.Request.Body` vagy `context.Response.Body`, hello eredeti üzenet törzsének elvész, és vissza hello kifejezésben hello törzs vissza kell állítani.</span><span class="sxs-lookup"><span data-stu-id="aedb9-297">Note that by default when you access hello message body using `context.Request.Body` or `context.Response.Body`, hello original message body is lost and must be set by returning hello body back in hello expression.</span></span> <span data-ttu-id="aedb9-298">tartalom, toopreserve hello törzs hello beállítása `preserveContent` paraméter túl`true` üdvözlőüzenetére való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="aedb9-298">toopreserve hello body content, set hello `preserveContent` parameter too`true` when accessing hello message.</span></span> <span data-ttu-id="aedb9-299">Ha `preserveContent` értéke túl`true` és egy másik szervezet által visszaadott hello kifejezés hello visszaadott törzs szolgál.</span><span class="sxs-lookup"><span data-stu-id="aedb9-299">If `preserveContent` is set too`true` and a different body is returned by hello expression, hello returned body is used.</span></span>  
>   
>  <span data-ttu-id="aedb9-300">Ne feledje hello használata esetén a következő szempontok hello `set-body` házirend.</span><span class="sxs-lookup"><span data-stu-id="aedb9-300">Please note hello following considerations when using hello `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="aedb9-301">Hello használata `set-body` házirend tooreturn tooset nem szükséges új vagy frissített törzs `preserveContent` túl`true` mert kifejezetten megadja hello új üzenettörzs tartalmát.</span><span class="sxs-lookup"><span data-stu-id="aedb9-301">If you are using hello `set-body` policy tooreturn a new or updated body you don't need tooset `preserveContent` too`true` because you are explicitly supplying hello new body contents.</span></span>  
> -   <span data-ttu-id="aedb9-302">A válasz hello tartalom megőrzése hello bejövő folyamat értelmetlen, mert még nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-302">Preserving hello content of a response in hello inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="aedb9-303">A kérelem tartalma hello megőrzése hello kimenő folyamat értelmetlen mert hello kérelem már elküldtek toohello háttér ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="aedb9-303">Preserving hello content of a request in hello outbound pipeline doesn't make sense because hello request has already been sent toohello backend at this point.</span></span>  
> -   <span data-ttu-id="aedb9-304">Ha ezt a házirendet használja, amikor nincs az üzenet törzse, például egy bejövő GET, a rendszer kivételt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aedb9-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="aedb9-305">További információkért lásd: hello `context.Request.Body`, `context.Response.Body`, és hello `IMessage` hello részeiben [környezeti változó](api-management-policy-expressions.md#ContextVariables) tábla.</span><span class="sxs-lookup"><span data-stu-id="aedb9-305">For more information, see hello `context.Request.Body`, `context.Response.Body`, and hello `IMessage` sections in hello [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-306">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="aedb9-307">Példák</span><span class="sxs-lookup"><span data-stu-id="aedb9-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="aedb9-308">Szöveg – példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a><span data-ttu-id="aedb9-309">Példa hello törzs karakterláncként elérése.</span><span class="sxs-lookup"><span data-stu-id="aedb9-309">Example accessing hello body as a string.</span></span> <span data-ttu-id="aedb9-310">Vegye figyelembe, hogy hello eredeti kérelemtörzset, hogy a Microsoft-e hozzáférési engedélye hello folyamat későbbi szakaszában vannak megőrzi.</span><span class="sxs-lookup"><span data-stu-id="aedb9-310">Note that we are preserving hello original request body so that we can access it later in hello pipeline.</span></span>
  
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
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a><span data-ttu-id="aedb9-311">A példa egy JObject hello szervezettől elérése.</span><span class="sxs-lookup"><span data-stu-id="aedb9-311">Example accessing hello body as a JObject.</span></span> <span data-ttu-id="aedb9-312">Vegye figyelembe, hogy a beállítást, mivel jelenleg nem lefoglalja hello eredeti kérelemtörzset, egypéldányú hello folyamat későbbi szakaszában okoz kivételt.</span><span class="sxs-lookup"><span data-stu-id="aedb9-312">Note that since we are not reserving hello original request body, accesing it later in hello pipeline will result in an exception.</span></span>  
  
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
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="aedb9-313">A termék szűrő választ</span><span class="sxs-lookup"><span data-stu-id="aedb9-313">Filter response based on product</span></span>  
 <span data-ttu-id="aedb9-314">Ez a példa bemutatja, hogyan tooperform tartalom alapján történő szűrés adatelemek eltávolítása hello választ kapott hello háttérszolgáltatást hello használatakor `Starter` termék.</span><span class="sxs-lookup"><span data-stu-id="aedb9-314">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="aedb9-315">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too34:30 előre.</span><span class="sxs-lookup"><span data-stu-id="aedb9-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="aedb9-316">Áttekintést 31:50 toosee kezdjék [sötét égbolt előrejelzési API hello](https://developer.forecast.io/) ebben a bemutatóban használt.</span><span class="sxs-lookup"><span data-stu-id="aedb9-316">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="aedb9-317">A készlet szövegtörzzsel folyékony sablonokkal</span><span class="sxs-lookup"><span data-stu-id="aedb9-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="aedb9-318">Hello `set-body` házirend lehet konfigurált toouse hello [folyékony](https://shopify.github.io/liquid/basics/introduction/) templating nyelvi tootransfom hello törzsének olyan kérésre vagy válaszra.</span><span class="sxs-lookup"><span data-stu-id="aedb9-318">hello `set-body` policy can be configured toouse hello [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language tootransfom hello body of a request or response.</span></span> <span data-ttu-id="aedb9-319">Ez nagyon hatékony, ha az üzenet toocompletely átalakítás hello formátumát szüksége lehet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-319">This can be very effective if you need toocompletely reshape hello format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aedb9-320">hello használt folyadék végrehajtásának hello `set-body` házirend úgy van konfigurálva, a "C# módban".</span><span class="sxs-lookup"><span data-stu-id="aedb9-320">hello implementation of Liquid used in hello `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="aedb9-321">Ez különösen fontos műveleteket, mint a szűrés során.</span><span class="sxs-lookup"><span data-stu-id="aedb9-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="aedb9-322">Tegyük fel, a dátum szűrő használatához Pascal hello használata és nagybetűhasználatnak, valamint C# dátum formázás, például:</span><span class="sxs-lookup"><span data-stu-id="aedb9-322">As an example, using a date filter requires hello use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="aedb9-323">{{body.foo.startDateTime| Dátum: "yyyyMMddTHH:mm:ddZ"}}</span><span class="sxs-lookup"><span data-stu-id="aedb9-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aedb9-324">A sorrend toocorrectly bind tooan XML-törzsére hello folyékony sablonnal, használja a `set-header` házirend tooset Content-Type tooeither application/xml, text/xml (vagy bármely típusú végződő + xml); a JSON-törzsére értéke lehet az application/json, text/json (vagy bármilyen típusú befejezése a + json).</span><span class="sxs-lookup"><span data-stu-id="aedb9-324">In order toocorrectly bind tooan XML body using hello Liquid template, use a `set-header` policy tooset Content-Type tooeither application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-toosoap-using-a-liquid-template"></a><span data-ttu-id="aedb9-325">Alakítsa át a JSON tooSOAP folyékony sablon használatával</span><span class="sxs-lookup"><span data-stu-id="aedb9-325">Convert JSON tooSOAP using a Liquid template</span></span>
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

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="aedb9-326">Tranform JSON folyékony sablon használatával</span><span class="sxs-lookup"><span data-stu-id="aedb9-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="aedb9-327">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-327">Elements</span></span>  
  
|<span data-ttu-id="aedb9-328">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-328">Name</span></span>|<span data-ttu-id="aedb9-329">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-329">Description</span></span>|<span data-ttu-id="aedb9-330">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-331">set-szervezet</span><span class="sxs-lookup"><span data-stu-id="aedb9-331">set-body</span></span>|<span data-ttu-id="aedb9-332">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-332">Root element.</span></span> <span data-ttu-id="aedb9-333">Hello szöveg vagy egy törzs visszaadó kifejezések tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-333">Contains hello body text or an expressions that returns a body.</span></span>|<span data-ttu-id="aedb9-334">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="aedb9-335">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="aedb9-335">Properties</span></span>  
  
|<span data-ttu-id="aedb9-336">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-336">Name</span></span>|<span data-ttu-id="aedb9-337">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-337">Description</span></span>|<span data-ttu-id="aedb9-338">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-338">Required</span></span>|<span data-ttu-id="aedb9-339">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-340">sablon</span><span class="sxs-lookup"><span data-stu-id="aedb9-340">template</span></span>|<span data-ttu-id="aedb9-341">Törzs szabály beállítása hello használt toochange hello templating módban fog futni.</span><span class="sxs-lookup"><span data-stu-id="aedb9-341">Used toochange hello templating mode that hello set body policy will run in.</span></span> <span data-ttu-id="aedb9-342">Jelenleg csak a támogatott hello érték a következő:</span><span class="sxs-lookup"><span data-stu-id="aedb9-342">Currently hello only supported value is:</span></span><br /><br /><span data-ttu-id="aedb9-343">-folyékony - hello beállítása törzs házirend hello folyékony templating motor fogja használni.</span><span class="sxs-lookup"><span data-stu-id="aedb9-343">- liquid - hello set body policy will use hello liquid templating engine</span></span> |<span data-ttu-id="aedb9-344">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-344">No</span></span>|<span data-ttu-id="aedb9-345">folyadék</span><span class="sxs-lookup"><span data-stu-id="aedb9-345">liquid</span></span>|  

<span data-ttu-id="aedb9-346">Hello kérelem-válasz információ eléréséhez, hello folyékony sablon az alábbi tulajdonságokkal hello köthető tooa környezeti objektumot:</span><span class="sxs-lookup"><span data-stu-id="aedb9-346">For accessing information about hello request and response, hello Liquid template can bind tooa context object with hello following properties:</span></span> <br />
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



### <a name="usage"></a><span data-ttu-id="aedb9-347">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-347">Usage</span></span>  
 <span data-ttu-id="aedb9-348">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-348">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-349">**Házirend szakaszok:** bejövő, kimenő háttér</span><span class="sxs-lookup"><span data-stu-id="aedb9-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="aedb9-350">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-351"><a name="SetHTTPheader"></a>HTTP-fejléc beállítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="aedb9-352">Hello `set-header` házirendet hozzárendeli egy érték tooan meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="aedb9-352">hello `set-header` policy assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="aedb9-353">A HTTP-fejlécek listáját szúr be a HTTP üzenet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="aedb9-354">Ha egy bejövő folyamat, ez a házirend hello HTTP-fejlécek átadott toohello célszolgáltatás hello kérelem állítja be.</span><span class="sxs-lookup"><span data-stu-id="aedb9-354">When placed in an inbound pipeline, this policy sets hello HTTP headers for hello request being passed toohello target service.</span></span> <span data-ttu-id="aedb9-355">Ha egy kimenő folyamat, ez a házirend hello HTTP-fejlécek toohello átjáró ügyfél küldött hello választ állítja be.</span><span class="sxs-lookup"><span data-stu-id="aedb9-355">When placed in an outbound pipeline, this policy sets hello HTTP headers for hello response being sent toohello gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-356">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="aedb9-357">Példák</span><span class="sxs-lookup"><span data-stu-id="aedb9-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="aedb9-358">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="aedb9-359">Környezeti információkat toohello háttérszolgáltatást továbbítsa</span><span class="sxs-lookup"><span data-stu-id="aedb9-359">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="aedb9-360">Ez a példa bemutatja, hogyan hello API tooapply házirendje szinten toosupply környezetben információk toohello háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-360">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="aedb9-361">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too10:30 előre.</span><span class="sxs-lookup"><span data-stu-id="aedb9-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="aedb9-362">12:10 nincs a művelet hívásának hello developer portálon, ahol láthatja a munkahelyi hello házirend bemutató.</span><span class="sxs-lookup"><span data-stu-id="aedb9-362">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="aedb9-363">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="aedb9-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="aedb9-364">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-364">Elements</span></span>  
  
|<span data-ttu-id="aedb9-365">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-365">Name</span></span>|<span data-ttu-id="aedb9-366">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-366">Description</span></span>|<span data-ttu-id="aedb9-367">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-368">set-fejléc</span><span class="sxs-lookup"><span data-stu-id="aedb9-368">set-header</span></span>|<span data-ttu-id="aedb9-369">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-369">Root element.</span></span>|<span data-ttu-id="aedb9-370">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-370">Yes</span></span>|  
|<span data-ttu-id="aedb9-371">érték</span><span class="sxs-lookup"><span data-stu-id="aedb9-371">value</span></span>|<span data-ttu-id="aedb9-372">Megadja a hello fejléc toobe set hello értékét.</span><span class="sxs-lookup"><span data-stu-id="aedb9-372">Specifies hello value of hello header toobe set.</span></span> <span data-ttu-id="aedb9-373">Hello fejléc több azonos nevű vegye fel az további `value` elemek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-373">For multiple headers with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="aedb9-374">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="aedb9-375">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="aedb9-375">Properties</span></span>  
  
|<span data-ttu-id="aedb9-376">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-376">Name</span></span>|<span data-ttu-id="aedb9-377">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-377">Description</span></span>|<span data-ttu-id="aedb9-378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-378">Required</span></span>|<span data-ttu-id="aedb9-379">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-380">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-380">exists-action</span></span>|<span data-ttu-id="aedb9-381">Milyen műveletet tootake határozza meg, amikor hello fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="aedb9-381">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="aedb9-382">Ez az attribútum hello a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aedb9-382">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-383">-felülbírálás - cserél hello értéke hello meglévő fejléc.</span><span class="sxs-lookup"><span data-stu-id="aedb9-383">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="aedb9-384">-skip – nem helyettesíti a hello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="aedb9-384">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="aedb9-385">-hozzáfűzése - hozzáfűzi hello érték toohello meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="aedb9-385">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="aedb9-386">-törlés – hello kérelemből eltávolítja hello fejlécet.</span><span class="sxs-lookup"><span data-stu-id="aedb9-386">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="aedb9-387">Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello fejlécben set függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="aedb9-387">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="aedb9-388">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-388">No</span></span>|<span data-ttu-id="aedb9-389">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="aedb9-389">override</span></span>|  
|<span data-ttu-id="aedb9-390">név</span><span class="sxs-lookup"><span data-stu-id="aedb9-390">name</span></span>|<span data-ttu-id="aedb9-391">Hello fejléc toobe készlet nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="aedb9-391">Specifies name of hello header toobe set.</span></span>|<span data-ttu-id="aedb9-392">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-392">Yes</span></span>|<span data-ttu-id="aedb9-393">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-394">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-394">Usage</span></span>  
 <span data-ttu-id="aedb9-395">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-395">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-396">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="aedb9-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="aedb9-397">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-398"><a name="SetQueryStringParameter"></a>Lekérdezési karakterlánc-paraméter beállítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="aedb9-399">Hello `set-query-parameter` házirendnek, cserél értékét, vagy a törlések kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="aedb9-399">hello `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="aedb9-400">Lehet használt toopass hello háttérszolgáltatást által várt lekérdezési paramétereket, amelyek választható vagy soha nem szerepelnek a hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-400">Can be used toopass query parameters expected by hello backend service which are optional or never present in hello request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-401">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="aedb9-402">Példák</span><span class="sxs-lookup"><span data-stu-id="aedb9-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="aedb9-403">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="aedb9-404">Környezeti információkat toohello háttérszolgáltatást továbbítsa</span><span class="sxs-lookup"><span data-stu-id="aedb9-404">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="aedb9-405">Ez a példa bemutatja, hogyan hello API tooapply házirendje szinten toosupply környezetben információk toohello háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="aedb9-405">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="aedb9-406">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too10:30 előre.</span><span class="sxs-lookup"><span data-stu-id="aedb9-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="aedb9-407">12:10 nincs a művelet hívásának hello developer portálon, ahol láthatja a munkahelyi hello házirend bemutató.</span><span class="sxs-lookup"><span data-stu-id="aedb9-407">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="aedb9-408">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="aedb9-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="aedb9-409">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-409">Elements</span></span>  
  
|<span data-ttu-id="aedb9-410">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-410">Name</span></span>|<span data-ttu-id="aedb9-411">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-411">Description</span></span>|<span data-ttu-id="aedb9-412">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-413">a Set-lekérdezés-paraméter</span><span class="sxs-lookup"><span data-stu-id="aedb9-413">set-query-parameter</span></span>|<span data-ttu-id="aedb9-414">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-414">Root element.</span></span>|<span data-ttu-id="aedb9-415">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-415">Yes</span></span>|  
|<span data-ttu-id="aedb9-416">érték</span><span class="sxs-lookup"><span data-stu-id="aedb9-416">value</span></span>|<span data-ttu-id="aedb9-417">Megadja a hello lekérdezés paraméterhalmaz toobe hello értékét.</span><span class="sxs-lookup"><span data-stu-id="aedb9-417">Specifies hello value of hello query parameter toobe set.</span></span> <span data-ttu-id="aedb9-418">Több lekérdezés paramétereket hello ugyanazt a nevet adja hozzá további `value` elemek.</span><span class="sxs-lookup"><span data-stu-id="aedb9-418">For multiple query parameters with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="aedb9-419">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="aedb9-420">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="aedb9-420">Properties</span></span>  
  
|<span data-ttu-id="aedb9-421">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-421">Name</span></span>|<span data-ttu-id="aedb9-422">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-422">Description</span></span>|<span data-ttu-id="aedb9-423">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-423">Required</span></span>|<span data-ttu-id="aedb9-424">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-425">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-425">exists-action</span></span>|<span data-ttu-id="aedb9-426">Milyen műveletet tootake határozza meg, amikor hello lekérdezési paraméter már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="aedb9-426">Specifies what action tootake when hello query parameter is already specified.</span></span> <span data-ttu-id="aedb9-427">Ez az attribútum hello a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aedb9-427">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="aedb9-428">-felülbírálás - cserél hello hello meglévő paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="aedb9-428">-   override - replaces hello value of hello existing parameter.</span></span><br /><span data-ttu-id="aedb9-429">-skip - függvény nem cseréli le hello meglévő lekérdezési paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="aedb9-429">-   skip - does not replace hello existing query parameter value.</span></span><br /><span data-ttu-id="aedb9-430">-hozzáfűzése - hozzáfűzi hello érték toohello meglévő lekérdezési paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="aedb9-430">-   append - appends hello value toohello existing query parameter value.</span></span><br /><span data-ttu-id="aedb9-431">-törlés - eltávolítása hello kérelem hello lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="aedb9-431">-   delete - removes hello query parameter from hello request.</span></span><br /><br /> <span data-ttu-id="aedb9-432">Ha értéke túl`override` csak a listában szereplő értékek hello eredmény be lesznek állítva; a hello azonos nevű eredmények hello lekérdezési paraméter beállítása függően tooall bejegyzéseket tartalmaz, (amelyek jelennek meg több alkalommal) alatt történő besorolásakor több bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="aedb9-432">When set too`override` enlisting multiple entries with hello same name results in hello query parameter being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="aedb9-433">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-433">No</span></span>|<span data-ttu-id="aedb9-434">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="aedb9-434">override</span></span>|  
|<span data-ttu-id="aedb9-435">név</span><span class="sxs-lookup"><span data-stu-id="aedb9-435">name</span></span>|<span data-ttu-id="aedb9-436">Hello lekérdezési paraméter toobe készlet nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="aedb9-436">Specifies name of hello query parameter toobe set.</span></span>|<span data-ttu-id="aedb9-437">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-437">Yes</span></span>|<span data-ttu-id="aedb9-438">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-439">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-439">Usage</span></span>  
 <span data-ttu-id="aedb9-440">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-440">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-441">**Házirend szakaszok:** bejövő, háttér</span><span class="sxs-lookup"><span data-stu-id="aedb9-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="aedb9-442">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-443"><a name="RewriteURL"></a>URL-cím újraírása</span><span class="sxs-lookup"><span data-stu-id="aedb9-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="aedb9-444">Hello `rewrite-uri` házirend a nyilvános űrlap toohello formájukban hello webszolgáltatás, amelyet várt alakítja a kérelem URL-CÍMÉT, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="aedb9-444">hello `rewrite-uri` policy converts a request URL from its public form toohello form expected by hello web service, as shown in hello following example.</span></span>  
  
-   <span data-ttu-id="aedb9-445">Nyilvános URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="aedb9-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="aedb9-446">Kérelem URL-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="aedb9-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="aedb9-447">Ez a házirend használhatja emberi és/vagy a böngésző-barát URL-címet kell alakul hello webszolgáltatás által várt hello URL-cím formátumba.</span><span class="sxs-lookup"><span data-stu-id="aedb9-447">This policy can be used when a human and/or browser-friendly URL should be transformed into hello URL format expected by hello web service.</span></span> <span data-ttu-id="aedb9-448">Ez a házirend csak kell alkalmazni, amikor egy másik URL-cím formátumban, például a tiszta URL-címeket, a RESTful URL-címeket, a felhasználóbarát URL-címek vagy a Keresőmotor-barát URL-címek, amelyek tisztán strukturális URL-címek, amely nem tartalmazhat lekérdezési karakterláncot, és helyette a hello csak hello elérési tartalmaz ilyen toobe az erőforrás (után hello séma és a hello szolgáltató).</span><span class="sxs-lookup"><span data-stu-id="aedb9-448">This policy only needs toobe applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only hello path of hello resource (after hello scheme and hello authority).</span></span> <span data-ttu-id="aedb9-449">Ez gyakran történik esztétikai, a használhatóság és a keresőmotor (keresőmotor-Optimalizáláshoz) optimalizálási céllal.</span><span class="sxs-lookup"><span data-stu-id="aedb9-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="aedb9-450">Csak a lekérdezési karakterlánc paraméterek hello házirend használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="aedb9-450">You can only add query string parameters using hello policy.</span></span> <span data-ttu-id="aedb9-451">További elérési Sablonparaméterek a hello újraírási URL-cím nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="aedb9-451">You cannot add extra template path parameters in hello rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="aedb9-452">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="aedb9-453">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-453">Example</span></span>  
  
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

### <a name="elements"></a><span data-ttu-id="aedb9-454">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-454">Elements</span></span>  
  
|<span data-ttu-id="aedb9-455">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-455">Name</span></span>|<span data-ttu-id="aedb9-456">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-456">Description</span></span>|<span data-ttu-id="aedb9-457">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-458">Módosítsa úgy-uri</span><span class="sxs-lookup"><span data-stu-id="aedb9-458">rewrite-uri</span></span>|<span data-ttu-id="aedb9-459">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-459">Root element.</span></span>|<span data-ttu-id="aedb9-460">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="aedb9-461">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="aedb9-461">Attributes</span></span>  
  
|<span data-ttu-id="aedb9-462">Attribútum</span><span class="sxs-lookup"><span data-stu-id="aedb9-462">Attribute</span></span>|<span data-ttu-id="aedb9-463">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-463">Description</span></span>|<span data-ttu-id="aedb9-464">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-464">Required</span></span>|<span data-ttu-id="aedb9-465">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="aedb9-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="aedb9-466">sablon</span><span class="sxs-lookup"><span data-stu-id="aedb9-466">template</span></span>|<span data-ttu-id="aedb9-467">hello tényleges webes szolgáltatás URL-cím elé a lekérdezési karakterlánc paramétereket.</span><span class="sxs-lookup"><span data-stu-id="aedb9-467">hello actual web service URL with any query string parameters.</span></span> <span data-ttu-id="aedb9-468">Kifejezések használata esetén a hello egész értéknek kell lennie egy kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aedb9-468">When using expressions, hello whole value must be an expression.</span></span>|<span data-ttu-id="aedb9-469">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-469">Yes</span></span>|<span data-ttu-id="aedb9-470">N/A</span><span class="sxs-lookup"><span data-stu-id="aedb9-470">N/A</span></span>|  
|<span data-ttu-id="aedb9-471">Másolás páratlan számú paraméterei</span><span class="sxs-lookup"><span data-stu-id="aedb9-471">copy-unmatched-params</span></span>|<span data-ttu-id="aedb9-472">Megadja, hogy a lekérdezés-paraméterek a hello bejövő kérelem nem található meg a hello eredeti URL-cím sablon kerülnek, toohello hello által megadott URL-cím újbóli írása sablon</span><span class="sxs-lookup"><span data-stu-id="aedb9-472">Specifies whether query parameters in hello incoming request not present in hello original URL template are added toohello URL defined by hello re-write template</span></span>|<span data-ttu-id="aedb9-473">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-473">No</span></span>|<span data-ttu-id="aedb9-474">Igaz</span><span class="sxs-lookup"><span data-stu-id="aedb9-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-475">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-475">Usage</span></span>  
 <span data-ttu-id="aedb9-476">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-476">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-477">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="aedb9-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="aedb9-478">**Házirend hatókörök:** termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="aedb9-479"><a name="XSLTransform"></a>Az XSLT-vel XML átalakítása</span><span class="sxs-lookup"><span data-stu-id="aedb9-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="aedb9-480">Hello `Transform XML using an XSLT` házirend vonatkozik egy XSL átalakítása tooXML hello kérés vagy válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="aedb9-480">hello `Transform XML using an XSLT` policy applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="aedb9-481">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-481">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="aedb9-482">Példa</span><span class="sxs-lookup"><span data-stu-id="aedb9-482">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="aedb9-483">Elemek</span><span class="sxs-lookup"><span data-stu-id="aedb9-483">Elements</span></span>  
  
|<span data-ttu-id="aedb9-484">Név</span><span class="sxs-lookup"><span data-stu-id="aedb9-484">Name</span></span>|<span data-ttu-id="aedb9-485">Leírás</span><span class="sxs-lookup"><span data-stu-id="aedb9-485">Description</span></span>|<span data-ttu-id="aedb9-486">Szükséges</span><span class="sxs-lookup"><span data-stu-id="aedb9-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="aedb9-487">XSL-átalakítás</span><span class="sxs-lookup"><span data-stu-id="aedb9-487">xsl-transform</span></span>|<span data-ttu-id="aedb9-488">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-488">Root element.</span></span>|<span data-ttu-id="aedb9-489">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-489">Yes</span></span>|  
|<span data-ttu-id="aedb9-490">A paraméter</span><span class="sxs-lookup"><span data-stu-id="aedb9-490">parameter</span></span>|<span data-ttu-id="aedb9-491">Hello átalakító használt használt toodefine változók</span><span class="sxs-lookup"><span data-stu-id="aedb9-491">Used toodefine variables used in hello transform</span></span>|<span data-ttu-id="aedb9-492">Nem</span><span class="sxs-lookup"><span data-stu-id="aedb9-492">No</span></span>|  
|<span data-ttu-id="aedb9-493">XSL: stylesheet</span><span class="sxs-lookup"><span data-stu-id="aedb9-493">xsl:stylesheet</span></span>|<span data-ttu-id="aedb9-494">Stíluslap gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="aedb9-494">Root stylesheet element.</span></span> <span data-ttu-id="aedb9-495">Minden elemek és attribútumok vannak meghatározva kövesse hello standard [XSLT-specifikációja](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="aedb9-495">All elements and attributes defined within follow hello standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="aedb9-496">Igen</span><span class="sxs-lookup"><span data-stu-id="aedb9-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="aedb9-497">Használat</span><span class="sxs-lookup"><span data-stu-id="aedb9-497">Usage</span></span>  
 <span data-ttu-id="aedb9-498">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="aedb9-498">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="aedb9-499">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="aedb9-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="aedb9-500">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="aedb9-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="aedb9-501">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aedb9-501">Next steps</span></span>
<span data-ttu-id="aedb9-502">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="aedb9-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
