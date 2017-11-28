---
title: "Az Azure API Management-átalakítási csoportházirendek |} Microsoft Docs"
description: "További tudnivalók az Azure API Management használható átalakítási csoportházirendek."
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
ms.openlocfilehash: c2bed904b82c569b28a6e00d0cc9b49107c148dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-transformation-policies"></a><span data-ttu-id="2b829-103">Az API Management-átalakítási csoportházirendek</span><span class="sxs-lookup"><span data-stu-id="2b829-103">API Management transformation policies</span></span>
<span data-ttu-id="2b829-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="2b829-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="2b829-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="2b829-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="2b829-106"><a name="TransformationPolicies"></a>Átalakítási csoportházirendek</span><span class="sxs-lookup"><span data-stu-id="2b829-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="2b829-107">[JSON átalakítása XML](api-management-transformation-policies.md#ConvertJSONtoXML) - konvertálja kérés vagy válasz body JSON formátumból az XML.</span><span class="sxs-lookup"><span data-stu-id="2b829-107">[Convert JSON to XML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON to XML.</span></span>  
  
-   <span data-ttu-id="2b829-108">[XML konvertálása JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - konvertálja kérés vagy válasz body az XML-JSON.</span><span class="sxs-lookup"><span data-stu-id="2b829-108">[Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML to JSON.</span></span>  
  
-   <span data-ttu-id="2b829-109">[Keresés és csere törzsében karakterlánc](api-management-transformation-policies.md#Findandreplacestringinbody) - kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="2b829-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="2b829-110">[URL-címek maszkolja a tartalom](api-management-transformation-policies.md#MaskURLSContent) -hivatkozások átírja (maszkok) a válaszban szereplő body, hogy azok az átjárón keresztül a megfelelő hivatkozásra mutat.</span><span class="sxs-lookup"><span data-stu-id="2b829-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>  
  
-   <span data-ttu-id="2b829-111">[Állítsa be háttérszolgáltatás](api-management-transformation-policies.md#SetBackendService) -módosítja a háttérszolgáltatáshoz egy bejövő kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="2b829-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes the backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="2b829-112">[Állítsa be a szervezet](api-management-transformation-policies.md#SetBody) -beállítja az üzenettörzs, a bejövő és kimenő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="2b829-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets the message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="2b829-113">[Set HTTP-fejléc](api-management-transformation-policies.md#SetHTTPheader) - hozzárendel egy értéket egy meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="2b829-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="2b829-114">[Állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) - ad hozzá, értéke váltja fel, vagy törli a kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2b829-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="2b829-115">[URL-cím újraírása](api-management-transformation-policies.md#RewriteURL) – a kérelem URL-címe nyilvános formájukban a várt érték a webszolgáltatás által az űrlap alakítja.</span><span class="sxs-lookup"><span data-stu-id="2b829-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form to the form expected by the web service.</span></span>  
  
-   <span data-ttu-id="2b829-116">[Átalakítás XSLT használatával XML](api-management-transformation-policies.md#XSLTransform) -XSL-átalakítás alkalmazása XML-kérés vagy válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="2b829-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation to XML in the request or response body.</span></span>  
  
##  <span data-ttu-id="2b829-117"><a name="ConvertJSONtoXML"></a>JSON átalakítása XML</span><span class="sxs-lookup"><span data-stu-id="2b829-117"><a name="ConvertJSONtoXML"></a> Convert JSON to XML</span></span>  
 <span data-ttu-id="2b829-118">A `json-to-xml` házirend alakít egy kérés vagy válasz törzsében JSON formátumból XML.</span><span class="sxs-lookup"><span data-stu-id="2b829-118">The `json-to-xml` policy converts a request or response body from JSON to XML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-119">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-120">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-120">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="2b829-121">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-121">Elements</span></span>  
  
|<span data-ttu-id="2b829-122">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-122">Name</span></span>|<span data-ttu-id="2b829-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-123">Description</span></span>|<span data-ttu-id="2b829-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-125">JSON-xml</span><span class="sxs-lookup"><span data-stu-id="2b829-125">json-to-xml</span></span>|<span data-ttu-id="2b829-126">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-126">Root element.</span></span>|<span data-ttu-id="2b829-127">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2b829-128">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="2b829-128">Attributes</span></span>  
  
|<span data-ttu-id="2b829-129">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-129">Name</span></span>|<span data-ttu-id="2b829-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-130">Description</span></span>|<span data-ttu-id="2b829-131">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-131">Required</span></span>|<span data-ttu-id="2b829-132">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-133">alkalmazása</span><span class="sxs-lookup"><span data-stu-id="2b829-133">apply</span></span>|<span data-ttu-id="2b829-134">Az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-134">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-135">átalakítás - mindig - mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="2b829-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="2b829-136">-tartalom típus-json - konvertálás csak akkor, ha a response Content-Type fejléc JSON jelenlétét jelzi.</span><span class="sxs-lookup"><span data-stu-id="2b829-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="2b829-137">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-137">Yes</span></span>|<span data-ttu-id="2b829-138">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-138">N/A</span></span>|  
|<span data-ttu-id="2b829-139">Vegye figyelembe-elfogadja-fejléc</span><span class="sxs-lookup"><span data-stu-id="2b829-139">consider-accept-header</span></span>|<span data-ttu-id="2b829-140">Az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-140">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-141">átalakítás - igaz - alkalmazni, amennyiben JSON kérelem Accept fejlécet.</span><span class="sxs-lookup"><span data-stu-id="2b829-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="2b829-142">-hamis - átalakítás mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="2b829-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="2b829-143">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-143">No</span></span>|<span data-ttu-id="2b829-144">Igaz</span><span class="sxs-lookup"><span data-stu-id="2b829-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-145">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-145">Usage</span></span>  
 <span data-ttu-id="2b829-146">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-146">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-147">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="2b829-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="2b829-148">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-149"><a name="ConvertXMLtoJSON"></a>XML konvertálása JSON</span><span class="sxs-lookup"><span data-stu-id="2b829-149"><a name="ConvertXMLtoJSON"></a> Convert XML to JSON</span></span>  
 <span data-ttu-id="2b829-150">A `xml-to-json` házirend alakít egy kérés vagy válasz törzsének XML-fájljából JSON.</span><span class="sxs-lookup"><span data-stu-id="2b829-150">The `xml-to-json` policy converts a request or response body from XML to JSON.</span></span> <span data-ttu-id="2b829-151">Ez a házirend segítségével korszerűsítésére API-kat csak XML-háttérrendszer webszolgáltatások alapján.</span><span class="sxs-lookup"><span data-stu-id="2b829-151">This policy can be used to modernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-152">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-153">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-153">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="2b829-154">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-154">Elements</span></span>  
  
|<span data-ttu-id="2b829-155">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-155">Name</span></span>|<span data-ttu-id="2b829-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-156">Description</span></span>|<span data-ttu-id="2b829-157">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-158">XML-json</span><span class="sxs-lookup"><span data-stu-id="2b829-158">xml-to-json</span></span>|<span data-ttu-id="2b829-159">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-159">Root element.</span></span>|<span data-ttu-id="2b829-160">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2b829-161">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="2b829-161">Attributes</span></span>  
  
|<span data-ttu-id="2b829-162">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-162">Name</span></span>|<span data-ttu-id="2b829-163">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-163">Description</span></span>|<span data-ttu-id="2b829-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-164">Required</span></span>|<span data-ttu-id="2b829-165">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-166">típusa</span><span class="sxs-lookup"><span data-stu-id="2b829-166">kind</span></span>|<span data-ttu-id="2b829-167">Az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-167">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-168">-javascript-barát – az átalakított JSON a JavaScript-fejlesztők számára egyszerű űrlap rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b829-168">-   javascript-friendly - the converted JSON has a form friendly to JavaScript developers.</span></span><br /><span data-ttu-id="2b829-169">a konvertált JSON - közvetlen – az eredeti XML-dokumentum struktúra tükrözi.</span><span class="sxs-lookup"><span data-stu-id="2b829-169">-   direct - the converted JSON reflects the original XML document's structure.</span></span>|<span data-ttu-id="2b829-170">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-170">Yes</span></span>|<span data-ttu-id="2b829-171">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-171">N/A</span></span>|  
|<span data-ttu-id="2b829-172">alkalmazása</span><span class="sxs-lookup"><span data-stu-id="2b829-172">apply</span></span>|<span data-ttu-id="2b829-173">Az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-173">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-174">-mindig - átalakítás mindig.</span><span class="sxs-lookup"><span data-stu-id="2b829-174">-   always - convert always.</span></span><br /><span data-ttu-id="2b829-175">-tartalom típus-xml - konvertálás csak akkor, ha a response Content-Type fejléc XML jelenlétét jelzi.</span><span class="sxs-lookup"><span data-stu-id="2b829-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="2b829-176">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-176">Yes</span></span>|<span data-ttu-id="2b829-177">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-177">N/A</span></span>|  
|<span data-ttu-id="2b829-178">Vegye figyelembe-elfogadja-fejléc</span><span class="sxs-lookup"><span data-stu-id="2b829-178">consider-accept-header</span></span>|<span data-ttu-id="2b829-179">Az attribútum a következő értékek egyikére kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-179">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-180">átalakítás - igaz - alkalmazni, amennyiben XML Accept fejlécet kérés van szükség.</span><span class="sxs-lookup"><span data-stu-id="2b829-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="2b829-181">-hamis - átalakítás mindig érvényesek.</span><span class="sxs-lookup"><span data-stu-id="2b829-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="2b829-182">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-182">No</span></span>|<span data-ttu-id="2b829-183">Igaz</span><span class="sxs-lookup"><span data-stu-id="2b829-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-184">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-184">Usage</span></span>  
 <span data-ttu-id="2b829-185">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-185">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-186">**Házirend szakaszok:** bejövő, kimenő, hiba</span><span class="sxs-lookup"><span data-stu-id="2b829-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="2b829-187">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-188"><a name="Findandreplacestringinbody"></a>Keresés és csere törzsében karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2b829-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="2b829-189">A `find-and-replace` házirend kérés vagy válasz karakterláncrész keresése, és lecseréli egy másik karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="2b829-189">The `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-190">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-191">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="2b829-192">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-192">Elements</span></span>  
  
|<span data-ttu-id="2b829-193">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-193">Name</span></span>|<span data-ttu-id="2b829-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-194">Description</span></span>|<span data-ttu-id="2b829-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-196">keresése és cseréje</span><span class="sxs-lookup"><span data-stu-id="2b829-196">find-and-replace</span></span>|<span data-ttu-id="2b829-197">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-197">Root element.</span></span>|<span data-ttu-id="2b829-198">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2b829-199">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="2b829-199">Attributes</span></span>  
  
|<span data-ttu-id="2b829-200">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-200">Name</span></span>|<span data-ttu-id="2b829-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-201">Description</span></span>|<span data-ttu-id="2b829-202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-202">Required</span></span>|<span data-ttu-id="2b829-203">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-204">A</span><span class="sxs-lookup"><span data-stu-id="2b829-204">from</span></span>|<span data-ttu-id="2b829-205">A keresendő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2b829-205">The string to search for.</span></span>|<span data-ttu-id="2b829-206">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-206">Yes</span></span>|<span data-ttu-id="2b829-207">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-207">N/A</span></span>|  
|<span data-ttu-id="2b829-208">erre:</span><span class="sxs-lookup"><span data-stu-id="2b829-208">to</span></span>|<span data-ttu-id="2b829-209">A behelyettesítendő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2b829-209">The replacement string.</span></span> <span data-ttu-id="2b829-210">Adja meg a nulla hosszúságú helyettesítő karakterláncok eltávolítása a keresési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2b829-210">Specify a zero length replacement string to remove the search string.</span></span>|<span data-ttu-id="2b829-211">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-211">Yes</span></span>|<span data-ttu-id="2b829-212">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-213">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-213">Usage</span></span>  
 <span data-ttu-id="2b829-214">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-214">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-215">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="2b829-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="2b829-216">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-217"><a name="MaskURLSContent"></a>A tartalom maszk URL-címek</span><span class="sxs-lookup"><span data-stu-id="2b829-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="2b829-218">A `redirect-content-urls` házirend átírja a válasz törzsében (maszkok) hivatkozásokat úgy, hogy azok az átjárón keresztül a megfelelő hivatkozásra mutat.</span><span class="sxs-lookup"><span data-stu-id="2b829-218">The `redirect-content-urls` policy re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span> <span data-ttu-id="2b829-219">A kimenő szakaszban használatával újra írási válasz törzsében hivatkozások be az átjáró mutasson.</span><span class="sxs-lookup"><span data-stu-id="2b829-219">Use in the outbound section to re-write response body links to make them point to the gateway.</span></span> <span data-ttu-id="2b829-220">A bejövő szakaszban lévő használhatók az ellenkezője.</span><span class="sxs-lookup"><span data-stu-id="2b829-220">Use in the inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="2b829-221">Ez a házirend nem változik, mint bármely térközkaraktert `Location` fejléceket.</span><span class="sxs-lookup"><span data-stu-id="2b829-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="2b829-222">A fejléc értékei módosításához használja a [set-fejléc](api-management-transformation-policies.md#SetHTTPheader) házirend.</span><span class="sxs-lookup"><span data-stu-id="2b829-222">To change header values, use the [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-223">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-224">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="2b829-225">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-225">Elements</span></span>  
  
|<span data-ttu-id="2b829-226">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-226">Name</span></span>|<span data-ttu-id="2b829-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-227">Description</span></span>|<span data-ttu-id="2b829-228">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-229">az átirányítási-tartalom-URL-címek</span><span class="sxs-lookup"><span data-stu-id="2b829-229">redirect-content-urls</span></span>|<span data-ttu-id="2b829-230">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-230">Root element.</span></span>|<span data-ttu-id="2b829-231">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-232">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-232">Usage</span></span>  
 <span data-ttu-id="2b829-233">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-233">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-234">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="2b829-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="2b829-235">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-236"><a name="SetBackendService"></a>Háttér-szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="2b829-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="2b829-237">Használja a `set-backend-service` olyan bejövő kérelemre átirányítása egy másik háttér, mint a megadott API-beállítások az adott műveletre vonatkozó házirendet.</span><span class="sxs-lookup"><span data-stu-id="2b829-237">Use the `set-backend-service` policy to redirect an incoming request to a different backend than the one specified in the API settings for that operation.</span></span> <span data-ttu-id="2b829-238">Ez a házirend a háttérrendszer alap URL-címe a bejövő kérelem a házirendben megadott módosításait.</span><span class="sxs-lookup"><span data-stu-id="2b829-238">This policy changes the backend service base URL of the incoming request to the one specified in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-239">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-240">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-240">Example</span></span>  
  
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
<span data-ttu-id="2b829-241">Ebben a példában a készlet háttér szolgáltatás házirend továbbítja a kérelmet a verzióértéket, mint a megadott API-ban egy másik háttérszolgáltatás a lekérdezési karakterláncban átadott alapján.</span><span class="sxs-lookup"><span data-stu-id="2b829-241">In this example the set backend service policy routes requests based on the version value passed in the query string to a different backend service than the one specified in the API.</span></span>
  
<span data-ttu-id="2b829-242">Kezdetben a háttérrendszer szolgáltatás alap URL-címet a API-beállítások származik.</span><span class="sxs-lookup"><span data-stu-id="2b829-242">Initially the backend service base URL is derived from the API settings.</span></span> <span data-ttu-id="2b829-243">Ezért a kérelem URL-címe `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` válik `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` ahol `http://contoso.com/api/10.4/` a háttér szolgáltatás URL-cím megadott API-beállítások.</span><span class="sxs-lookup"><span data-stu-id="2b829-243">So the request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is the backend service URL specified in the API settings.</span></span>  
  
<span data-ttu-id="2b829-244">Ha a [< válasszon\> ](api-management-advanced-policies.md#choose) alkalmazott házirend-utasítás a háttérrendszer szolgáltatás alap URL-címet módosíthatja újra közül csak az egyiket `http://contoso.com/api/8.2` vagy `http://contoso.com/api/9.1`, attól függően, a verzió kérelem lekérdezési paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="2b829-244">When the [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied the backend service base URL may change again either to `http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on the value of the version request query parameter.</span></span> <span data-ttu-id="2b829-245">Ha az érték például `"2013-15"` a végső kérelem URL-cím lesz `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="2b829-245">For example, if the value is `"2013-15"` the final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="2b829-246">Ha a kérelem átalakítása-e további kívánt, más [átalakítási csoportházirendek](api-management-transformation-policies.md#TransformationPolicies) is használható.</span><span class="sxs-lookup"><span data-stu-id="2b829-246">If further transformation of the request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="2b829-247">Ahhoz például, hogy távolítsa el a version lekérdezési paramétert, most, hogy a kérelem verziót adott backend routedevent irányítása a [állítsa be a lekérdezési karakterlánc paraméter](api-management-transformation-policies.md#SetQueryStringParameter) házirend segítségével távolítsa el a most redundáns version attribútum.</span><span class="sxs-lookup"><span data-stu-id="2b829-247">For example, to remove the version query parameter now that the request is being routed to a version specific backend, the  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used to remove the now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="2b829-248">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-248">Example</span></span>  
  
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
<span data-ttu-id="2b829-249">Ebben a példában a házirend továbbítja a kérelmet a service fabric háttér, használja a userId lekérdezési karakterláncot, mint a partíciós kulcs, és a partíció az elsődleges replika használatával.</span><span class="sxs-lookup"><span data-stu-id="2b829-249">In this example the policy routes the request to a service fabric backend, using the userId query string as the partition key and using the primary replica of the partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="2b829-250">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-250">Elements</span></span>  
  
|<span data-ttu-id="2b829-251">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-251">Name</span></span>|<span data-ttu-id="2b829-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-252">Description</span></span>|<span data-ttu-id="2b829-253">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-254">háttér-szolgáltatás beállítása</span><span class="sxs-lookup"><span data-stu-id="2b829-254">set-backend-service</span></span>|<span data-ttu-id="2b829-255">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-255">Root element.</span></span>|<span data-ttu-id="2b829-256">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2b829-257">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="2b829-257">Attributes</span></span>  
  
|<span data-ttu-id="2b829-258">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-258">Name</span></span>|<span data-ttu-id="2b829-259">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-259">Description</span></span>|<span data-ttu-id="2b829-260">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-260">Required</span></span>|<span data-ttu-id="2b829-261">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-262">alap-URL-cím</span><span class="sxs-lookup"><span data-stu-id="2b829-262">base-url</span></span>|<span data-ttu-id="2b829-263">Új háttér alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="2b829-263">New backend service base URL.</span></span>|<span data-ttu-id="2b829-264">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-264">No</span></span>|<span data-ttu-id="2b829-265">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-265">N/A</span></span>|  
|<span data-ttu-id="2b829-266">háttér-azonosító</span><span class="sxs-lookup"><span data-stu-id="2b829-266">backend-id</span></span>|<span data-ttu-id="2b829-267">A háttér-hez irányítandó azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2b829-267">Identifier of the backend to route to.</span></span>|<span data-ttu-id="2b829-268">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-268">No</span></span>|<span data-ttu-id="2b829-269">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-269">N/A</span></span>|  
|<span data-ttu-id="2b829-270">ú partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="2b829-270">sf-partition-key</span></span>|<span data-ttu-id="2b829-271">Csak érvényes a háttérkiszolgálón a Service Fabric-szolgáltatás, és a "háttér-id" van megadva.</span><span class="sxs-lookup"><span data-stu-id="2b829-271">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="2b829-272">Egy adott partícióra a névfeloldási szolgáltatást használ.</span><span class="sxs-lookup"><span data-stu-id="2b829-272">Used to resolve a specific partition from the name resolution service.</span></span>|<span data-ttu-id="2b829-273">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-273">No</span></span>|<span data-ttu-id="2b829-274">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-274">N/A</span></span>|  
|<span data-ttu-id="2b829-275">ú-replika-típusa</span><span class="sxs-lookup"><span data-stu-id="2b829-275">sf-replica-type</span></span>|<span data-ttu-id="2b829-276">Csak érvényes a háttérkiszolgálón a Service Fabric-szolgáltatás, és a "háttér-id" van megadva.</span><span class="sxs-lookup"><span data-stu-id="2b829-276">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="2b829-277">Szabályozza, hogy a kérelem el kell küldeni egy partíció elsődleges vagy másodlagos replikája.</span><span class="sxs-lookup"><span data-stu-id="2b829-277">Controls if the request should go to the primary or secondary replica of a partition.</span></span> |<span data-ttu-id="2b829-278">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-278">No</span></span>|<span data-ttu-id="2b829-279">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-279">N/A</span></span>|    
|<span data-ttu-id="2b829-280">ú-resolve-feltétel</span><span class="sxs-lookup"><span data-stu-id="2b829-280">sf-resolve-condition</span></span>|<span data-ttu-id="2b829-281">Csak érvényes a Service Fabric-szolgáltatás esetén a háttér.</span><span class="sxs-lookup"><span data-stu-id="2b829-281">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="2b829-282">A feltétel azonosítja a Service Fabric háttér hívása új megoldás meg kell ismételni van-e.</span><span class="sxs-lookup"><span data-stu-id="2b829-282">Condition identifying if the call to Service Fabric backend has to be repeated with new resolution.</span></span>|<span data-ttu-id="2b829-283">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-283">No</span></span>|<span data-ttu-id="2b829-284">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-284">N/A</span></span>|    
|<span data-ttu-id="2b829-285">ú-példány-szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="2b829-285">sf-service-instance-name</span></span>|<span data-ttu-id="2b829-286">Csak érvényes a Service Fabric-szolgáltatás esetén a háttér.</span><span class="sxs-lookup"><span data-stu-id="2b829-286">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="2b829-287">Lehetővé teszi, hogy futásidőben szolgáltatáspéldány módosítása.</span><span class="sxs-lookup"><span data-stu-id="2b829-287">Allows to change service instances at runtime.</span></span> |<span data-ttu-id="2b829-288">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-288">No</span></span>|<span data-ttu-id="2b829-289">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="2b829-290">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-290">Usage</span></span>  
 <span data-ttu-id="2b829-291">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-291">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-292">**Házirend szakaszok:** bejövő, háttér</span><span class="sxs-lookup"><span data-stu-id="2b829-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="2b829-293">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-294"><a name="SetBody"></a>Set törzse</span><span class="sxs-lookup"><span data-stu-id="2b829-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="2b829-295">Használja a `set-body` házirend beállítása az üzenettörzs, a bejövő és kimenő kérelmek.</span><span class="sxs-lookup"><span data-stu-id="2b829-295">Use the `set-body` policy to set the message body for incoming and outgoing requests.</span></span> <span data-ttu-id="2b829-296">Az üzenettörzs használható eléréséhez a `context.Request.Body` tulajdonság vagy a `context.Response.Body`, attól függően, hogy a házirend van-e a bejövő vagy kimenő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2b829-296">To access the message body you can use the `context.Request.Body` property or the `context.Response.Body`, depending on whether the policy is in the inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="2b829-297">Vegye figyelembe, hogy az üzenet elérésekor alapértelmezés szerint body használatával `context.Request.Body` vagy `context.Response.Body`, az eredeti üzenettörzs elvész, és a szervezet vissza a kifejezésben vissza kell állítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-297">Note that by default when you access the message body using `context.Request.Body` or `context.Response.Body`, the original message body is lost and must be set by returning the body back in the expression.</span></span> <span data-ttu-id="2b829-298">A szervezet tartalom megőrzése érdekében állítsa a `preserveContent` paramétert `true` az üzenet való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="2b829-298">To preserve the body content, set the `preserveContent` parameter to `true` when accessing the message.</span></span> <span data-ttu-id="2b829-299">Ha `preserveContent` értéke `true` és egy másik szervezet által visszaadott a kifejezést, a visszaadott törzs szolgál.</span><span class="sxs-lookup"><span data-stu-id="2b829-299">If `preserveContent` is set to `true` and a different body is returned by the expression, the returned body is used.</span></span>  
>   
>  <span data-ttu-id="2b829-300">Használata esetén vegye figyelembe a következőket kell figyelembe venni a `set-body` házirend.</span><span class="sxs-lookup"><span data-stu-id="2b829-300">Please note the following considerations when using the `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="2b829-301">Használatakor a `set-body` házirend számára, nem kell telepítenie egy új vagy frissített törzs visszatérési `preserveContent` való `true` mert kifejezetten megadja az új üzenettörzs tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2b829-301">If you are using the `set-body` policy to return a new or updated body you don't need to set `preserveContent` to `true` because you are explicitly supplying the new body contents.</span></span>  
> -   <span data-ttu-id="2b829-302">A bejövő feldolgozási válasz tartalmának megőrzi értelmetlen, mert még nincs válasz.</span><span class="sxs-lookup"><span data-stu-id="2b829-302">Preserving the content of a response in the inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="2b829-303">Megőrzi a kérést a kimenő folyamat tartalmának értelmetlen mert elküldte a kérelmet már van a háttérrendszerhez ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="2b829-303">Preserving the content of a request in the outbound pipeline doesn't make sense because the request has already been sent to the backend at this point.</span></span>  
> -   <span data-ttu-id="2b829-304">Ha ezt a házirendet használja, amikor nincs az üzenet törzse, például egy bejövő GET, a rendszer kivételt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2b829-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="2b829-305">További információkért lásd: a `context.Request.Body`, `context.Response.Body`, és a `IMessage` szakaszában a [környezeti változó](api-management-policy-expressions.md#ContextVariables) tábla.</span><span class="sxs-lookup"><span data-stu-id="2b829-305">For more information, see the `context.Request.Body`, `context.Response.Body`, and the `IMessage` sections in the [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-306">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="2b829-307">Példák</span><span class="sxs-lookup"><span data-stu-id="2b829-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="2b829-308">Szöveg – példa</span><span class="sxs-lookup"><span data-stu-id="2b829-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a><span data-ttu-id="2b829-309">Példa a szervezet karakterláncként elérése.</span><span class="sxs-lookup"><span data-stu-id="2b829-309">Example accessing the body as a string.</span></span> <span data-ttu-id="2b829-310">Vegye figyelembe, hogy az eredeti kérés törzsében, hogy a Microsoft-e hozzáférési engedélye a folyamat későbbi szakaszában vannak megőrzi.</span><span class="sxs-lookup"><span data-stu-id="2b829-310">Note that we are preserving the original request body so that we can access it later in the pipeline.</span></span>
  
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
  
#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a><span data-ttu-id="2b829-311">A példa egy JObject a szervezettől elérése.</span><span class="sxs-lookup"><span data-stu-id="2b829-311">Example accessing the body as a JObject.</span></span> <span data-ttu-id="2b829-312">Vegye figyelembe, hogy azt nem az eredeti kérés törzsében, a folyamat későbbi szakaszában okoz kivétel egypéldányú lefoglalja mivel.</span><span class="sxs-lookup"><span data-stu-id="2b829-312">Note that since we are not reserving the original request body, accesing it later in the pipeline will result in an exception.</span></span>  
  
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
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="2b829-313">A termék szűrő választ</span><span class="sxs-lookup"><span data-stu-id="2b829-313">Filter response based on product</span></span>  
 <span data-ttu-id="2b829-314">A példa bemutatja, hogyan hajthat végre a tartalom alapján történő szűrés adatelemek eltávolítása a válasz érkezett a háttérszolgáltatás használata esetén a `Starter` termék.</span><span class="sxs-lookup"><span data-stu-id="2b829-314">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="2b829-315">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és a 34:30 előretekerés.</span><span class="sxs-lookup"><span data-stu-id="2b829-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="2b829-316">Megtekinthet egy áttekintést 31:50 kezdjék [a sötét égbolt előrejelzési API](https://developer.forecast.io/) ebben a bemutatóban használt.</span><span class="sxs-lookup"><span data-stu-id="2b829-316">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="2b829-317">A készlet szövegtörzzsel folyékony sablonokkal</span><span class="sxs-lookup"><span data-stu-id="2b829-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="2b829-318">A `set-body` házirend beállítható úgy, hogy használja a [folyékony](https://shopify.github.io/liquid/basics/introduction/) templating nyelv transfom olyan kérésre vagy válaszra törzsét.</span><span class="sxs-lookup"><span data-stu-id="2b829-318">The `set-body` policy can be configured to use the [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language to transfom the body of a request or response.</span></span> <span data-ttu-id="2b829-319">Ez nagyon hatékony, ha az üzenet formátuma teljesen átalakításához szüksége lehet.</span><span class="sxs-lookup"><span data-stu-id="2b829-319">This can be very effective if you need to completely reshape the format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b829-320">Folyadék megvalósítása szerepel a `set-body` házirend úgy van konfigurálva, a "C# módban".</span><span class="sxs-lookup"><span data-stu-id="2b829-320">The implementation of Liquid used in the `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="2b829-321">Ez különösen fontos műveleteket, mint a szűrés során.</span><span class="sxs-lookup"><span data-stu-id="2b829-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="2b829-322">Tegyük fel, dátum szűrő használata szükséges Pascal és nagybetűhasználatnak, valamint C# dátum formázás, például:</span><span class="sxs-lookup"><span data-stu-id="2b829-322">As an example, using a date filter requires the use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="2b829-323">{{body.foo.startDateTime| Dátum: "yyyyMMddTHH:mm:ddZ"}}</span><span class="sxs-lookup"><span data-stu-id="2b829-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b829-324">Ahhoz, hogy megfelelően egy XML-törzsére folyékony sablonnal kötést létrehozni, használja a `set-header` házirend beállítása a Content-Type vagy application/xml, text/xml (vagy bármely típusú végződő + xml), egy JSON-törzsére, kell lennie az application/json, text/json (vagy bármilyen végződő + JSON-ban).</span><span class="sxs-lookup"><span data-stu-id="2b829-324">In order to correctly bind to an XML body using the Liquid template, use a `set-header` policy to set Content-Type to either application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-to-soap-using-a-liquid-template"></a><span data-ttu-id="2b829-325">JSON átalakítása SOAP folyékony sablon használatával</span><span class="sxs-lookup"><span data-stu-id="2b829-325">Convert JSON to SOAP using a Liquid template</span></span>
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

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="2b829-326">Tranform JSON folyékony sablon használatával</span><span class="sxs-lookup"><span data-stu-id="2b829-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="2b829-327">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-327">Elements</span></span>  
  
|<span data-ttu-id="2b829-328">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-328">Name</span></span>|<span data-ttu-id="2b829-329">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-329">Description</span></span>|<span data-ttu-id="2b829-330">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-331">set-szervezet</span><span class="sxs-lookup"><span data-stu-id="2b829-331">set-body</span></span>|<span data-ttu-id="2b829-332">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-332">Root element.</span></span> <span data-ttu-id="2b829-333">A szöveg vagy egy törzs visszaadó kifejezések tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b829-333">Contains the body text or an expressions that returns a body.</span></span>|<span data-ttu-id="2b829-334">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="2b829-335">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2b829-335">Properties</span></span>  
  
|<span data-ttu-id="2b829-336">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-336">Name</span></span>|<span data-ttu-id="2b829-337">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-337">Description</span></span>|<span data-ttu-id="2b829-338">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-338">Required</span></span>|<span data-ttu-id="2b829-339">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-340">sablon</span><span class="sxs-lookup"><span data-stu-id="2b829-340">template</span></span>|<span data-ttu-id="2b829-341">Segítségével módosíthatja, amely a szervezet szabály beállítása templating módját.</span><span class="sxs-lookup"><span data-stu-id="2b829-341">Used to change the templating mode that the set body policy will run in.</span></span> <span data-ttu-id="2b829-342">Jelenleg az egyetlen támogatott érték:</span><span class="sxs-lookup"><span data-stu-id="2b829-342">Currently the only supported value is:</span></span><br /><br /><span data-ttu-id="2b829-343">-folyékony - törzs-szabály beállítása fogja használni a folyékony templating motor</span><span class="sxs-lookup"><span data-stu-id="2b829-343">- liquid - the set body policy will use the liquid templating engine</span></span> |<span data-ttu-id="2b829-344">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-344">No</span></span>|<span data-ttu-id="2b829-345">folyadék</span><span class="sxs-lookup"><span data-stu-id="2b829-345">liquid</span></span>|  

<span data-ttu-id="2b829-346">A kérelem és válasz adataihoz való hozzáférést, a folyékony sablon köthető egy környezeti objektum a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="2b829-346">For accessing information about the request and response, the Liquid template can bind to a context object with the following properties:</span></span> <br />
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



### <a name="usage"></a><span data-ttu-id="2b829-347">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-347">Usage</span></span>  
 <span data-ttu-id="2b829-348">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-348">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-349">**Házirend szakaszok:** bejövő, kimenő háttér</span><span class="sxs-lookup"><span data-stu-id="2b829-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="2b829-350">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-351"><a name="SetHTTPheader"></a>HTTP-fejléc beállítása</span><span class="sxs-lookup"><span data-stu-id="2b829-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="2b829-352">A `set-header` házirend hozzárendel egy értéket egy meglévő válasz és/vagy a kérelem fejlécében vagy ad hozzá egy új válasz és/vagy a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="2b829-352">The `set-header` policy assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="2b829-353">A HTTP-fejlécek listáját szúr be a HTTP üzenet.</span><span class="sxs-lookup"><span data-stu-id="2b829-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="2b829-354">Ha egy bejövő folyamat, ez a házirend a HTTP-fejléceket, a kérés átadódik a célként megadott szolgáltatás az állítja be.</span><span class="sxs-lookup"><span data-stu-id="2b829-354">When placed in an inbound pipeline, this policy sets the HTTP headers for the request being passed to the target service.</span></span> <span data-ttu-id="2b829-355">Ha egy kimenő folyamat, ez a házirend a HTTP-fejléceket, a válasz küldi el az átjáró ügyfél állítja be.</span><span class="sxs-lookup"><span data-stu-id="2b829-355">When placed in an outbound pipeline, this policy sets the HTTP headers for the response being sent to the gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-356">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="2b829-357">Példák</span><span class="sxs-lookup"><span data-stu-id="2b829-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="2b829-358">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="2b829-359">Környezet adatainak által a háttérszolgáltatáshoz továbbítsa</span><span class="sxs-lookup"><span data-stu-id="2b829-359">Forward context information to the backend service</span></span>  
 <span data-ttu-id="2b829-360">Ez a példa bemutatja, hogyan alkalmazni a házirendet, adja meg a környezet adatainak által a háttérszolgáltatáshoz API szinten.</span><span class="sxs-lookup"><span data-stu-id="2b829-360">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="2b829-361">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és előretekerés 10:30.</span><span class="sxs-lookup"><span data-stu-id="2b829-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="2b829-362">12:10 nincs egy bemutatója művelet hívása a fejlesztői portálra, ahol láthatja a házirendet, a munkahelyi hálózatban.</span><span class="sxs-lookup"><span data-stu-id="2b829-362">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="2b829-363">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="2b829-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="2b829-364">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-364">Elements</span></span>  
  
|<span data-ttu-id="2b829-365">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-365">Name</span></span>|<span data-ttu-id="2b829-366">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-366">Description</span></span>|<span data-ttu-id="2b829-367">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-368">set-fejléc</span><span class="sxs-lookup"><span data-stu-id="2b829-368">set-header</span></span>|<span data-ttu-id="2b829-369">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-369">Root element.</span></span>|<span data-ttu-id="2b829-370">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-370">Yes</span></span>|  
|<span data-ttu-id="2b829-371">érték</span><span class="sxs-lookup"><span data-stu-id="2b829-371">value</span></span>|<span data-ttu-id="2b829-372">Meghatározza azt az értéket, a fejléc kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-372">Specifies the value of the header to be set.</span></span> <span data-ttu-id="2b829-373">Ezzel a névvel több fejléc fel további `value` elemek.</span><span class="sxs-lookup"><span data-stu-id="2b829-373">For multiple headers with the same name add additional `value` elements.</span></span>|<span data-ttu-id="2b829-374">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="2b829-375">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2b829-375">Properties</span></span>  
  
|<span data-ttu-id="2b829-376">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-376">Name</span></span>|<span data-ttu-id="2b829-377">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-377">Description</span></span>|<span data-ttu-id="2b829-378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-378">Required</span></span>|<span data-ttu-id="2b829-379">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-380">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-380">exists-action</span></span>|<span data-ttu-id="2b829-381">Megadja, milyen műveletet hajtson végre a fejléc már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="2b829-381">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="2b829-382">Ez az attribútum a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2b829-382">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-383">-felülbírálás - lecseréli a meglévő fejléc értékének.</span><span class="sxs-lookup"><span data-stu-id="2b829-383">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="2b829-384">-skip – nem helyettesíti a meglévő-fejléc értékét.</span><span class="sxs-lookup"><span data-stu-id="2b829-384">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="2b829-385">-hozzáfűzése - az érték hozzáfűzi a meglévő állomásfejléc-érték.</span><span class="sxs-lookup"><span data-stu-id="2b829-385">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="2b829-386">-törlés - eltávolítja a fejlécet a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="2b829-386">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="2b829-387">Ha beállítása `override` történő besorolásakor ugyanazzal a névvel több bejegyzést eredményez az összes bejegyzés (amely jelennek meg több alkalommal) megfelelően történő beállítása fejléc; csak a listában szereplő értékek be lesznek állítva az eredményt.</span><span class="sxs-lookup"><span data-stu-id="2b829-387">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="2b829-388">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-388">No</span></span>|<span data-ttu-id="2b829-389">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="2b829-389">override</span></span>|  
|<span data-ttu-id="2b829-390">név</span><span class="sxs-lookup"><span data-stu-id="2b829-390">name</span></span>|<span data-ttu-id="2b829-391">A fejlécben állítható nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="2b829-391">Specifies name of the header to be set.</span></span>|<span data-ttu-id="2b829-392">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-392">Yes</span></span>|<span data-ttu-id="2b829-393">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-394">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-394">Usage</span></span>  
 <span data-ttu-id="2b829-395">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-395">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-396">**Házirend szakaszok:** bejövő, kimenő háttér,-hiba</span><span class="sxs-lookup"><span data-stu-id="2b829-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="2b829-397">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-398"><a name="SetQueryStringParameter"></a>Lekérdezési karakterlánc-paraméter beállítása</span><span class="sxs-lookup"><span data-stu-id="2b829-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="2b829-399">A `set-query-parameter` házirendnek, cserél értékét, vagy a törlések kérelem lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2b829-399">The `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="2b829-400">Felelt meg a lekérdezés által a háttérszolgáltatáshoz nem kötelező várt paraméter használható, vagy soha nem szerepelnek a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="2b829-400">Can be used to pass query parameters expected by the backend service which are optional or never present in the request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-401">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="2b829-402">Példák</span><span class="sxs-lookup"><span data-stu-id="2b829-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="2b829-403">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with the same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="2b829-404">Környezet adatainak által a háttérszolgáltatáshoz továbbítsa</span><span class="sxs-lookup"><span data-stu-id="2b829-404">Forward context information to the backend service</span></span>  
 <span data-ttu-id="2b829-405">Ez a példa bemutatja, hogyan alkalmazni a házirendet, adja meg a környezet adatainak által a háttérszolgáltatáshoz API szinten.</span><span class="sxs-lookup"><span data-stu-id="2b829-405">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="2b829-406">A házirenddel és konfigurálása a bemutatója, lásd: [felhő fedik le a epizód 177: több API a felügyeleti funkcióinak Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és előretekerés 10:30.</span><span class="sxs-lookup"><span data-stu-id="2b829-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="2b829-407">12:10 nincs egy bemutatója művelet hívása a fejlesztői portálra, ahol láthatja a házirendet, a munkahelyi hálózatban.</span><span class="sxs-lookup"><span data-stu-id="2b829-407">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="2b829-408">További információkért lásd: [házirend-kifejezések](api-management-policy-expressions.md) és [környezeti változó](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="2b829-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="2b829-409">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-409">Elements</span></span>  
  
|<span data-ttu-id="2b829-410">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-410">Name</span></span>|<span data-ttu-id="2b829-411">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-411">Description</span></span>|<span data-ttu-id="2b829-412">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-413">a Set-lekérdezés-paraméter</span><span class="sxs-lookup"><span data-stu-id="2b829-413">set-query-parameter</span></span>|<span data-ttu-id="2b829-414">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-414">Root element.</span></span>|<span data-ttu-id="2b829-415">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-415">Yes</span></span>|  
|<span data-ttu-id="2b829-416">érték</span><span class="sxs-lookup"><span data-stu-id="2b829-416">value</span></span>|<span data-ttu-id="2b829-417">Meghatározza azt az értéket, a lekérdezési paraméter kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-417">Specifies the value of the query parameter to be set.</span></span> <span data-ttu-id="2b829-418">Ezzel a névvel több lekérdezésparamétereket fel további `value` elemek.</span><span class="sxs-lookup"><span data-stu-id="2b829-418">For multiple query parameters with the same name add additional `value` elements.</span></span>|<span data-ttu-id="2b829-419">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="2b829-420">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2b829-420">Properties</span></span>  
  
|<span data-ttu-id="2b829-421">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-421">Name</span></span>|<span data-ttu-id="2b829-422">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-422">Description</span></span>|<span data-ttu-id="2b829-423">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-423">Required</span></span>|<span data-ttu-id="2b829-424">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-425">létezik-e művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-425">exists-action</span></span>|<span data-ttu-id="2b829-426">Megadja, milyen műveletet hajtson végre a következő lekérdezésparaméter már meg van adva.</span><span class="sxs-lookup"><span data-stu-id="2b829-426">Specifies what action to take when the query parameter is already specified.</span></span> <span data-ttu-id="2b829-427">Ez az attribútum a következő értékek egyikének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2b829-427">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="2b829-428">-felülbírálás - lecseréli a meglévő paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="2b829-428">-   override - replaces the value of the existing parameter.</span></span><br /><span data-ttu-id="2b829-429">-skip - függvény nem cseréli le a meglévő lekérdezési paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="2b829-429">-   skip - does not replace the existing query parameter value.</span></span><br /><span data-ttu-id="2b829-430">-hozzáfűzése - az érték hozzáfűzi a meglévő lekérdezési paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="2b829-430">-   append - appends the value to the existing query parameter value.</span></span><br /><span data-ttu-id="2b829-431">-törlés - eltávolítja a következő lekérdezésparaméter a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="2b829-431">-   delete - removes the query parameter from the request.</span></span><br /><br /> <span data-ttu-id="2b829-432">Ha beállítása `override` történő besorolásakor ugyanazzal a névvel több bejegyzés a következő lekérdezésparaméter összes bejegyzés (amely jelennek meg több alkalommal) megfelelően történő beállítása eredményez; csak a listában szereplő értékek be lesznek állítva az eredményt.</span><span class="sxs-lookup"><span data-stu-id="2b829-432">When set to `override` enlisting multiple entries with the same name results in the query parameter being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="2b829-433">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-433">No</span></span>|<span data-ttu-id="2b829-434">felülbírálás</span><span class="sxs-lookup"><span data-stu-id="2b829-434">override</span></span>|  
|<span data-ttu-id="2b829-435">név</span><span class="sxs-lookup"><span data-stu-id="2b829-435">name</span></span>|<span data-ttu-id="2b829-436">Megadja annak a nevét, a lekérdezési paramétert kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2b829-436">Specifies name of the query parameter to be set.</span></span>|<span data-ttu-id="2b829-437">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-437">Yes</span></span>|<span data-ttu-id="2b829-438">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-439">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-439">Usage</span></span>  
 <span data-ttu-id="2b829-440">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-440">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-441">**Házirend szakaszok:** bejövő, háttér</span><span class="sxs-lookup"><span data-stu-id="2b829-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="2b829-442">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-443"><a name="RewriteURL"></a>URL-cím újraírása</span><span class="sxs-lookup"><span data-stu-id="2b829-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="2b829-444">A `rewrite-uri` házirend konvertálja a kérelem URL-CÍMÉT a nyilvános űrlapból az űrlapot, a webszolgáltatás által várt a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="2b829-444">The `rewrite-uri` policy converts a request URL from its public form to the form expected by the web service, as shown in the following example.</span></span>  
  
-   <span data-ttu-id="2b829-445">Nyilvános URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="2b829-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="2b829-446">Kérelem URL-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="2b829-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="2b829-447">Ez a házirend használhatja emberi és/vagy a böngésző-barát URL-címet kell lennie alakítja át a webszolgáltatás által várt URL-cím formátumú.</span><span class="sxs-lookup"><span data-stu-id="2b829-447">This policy can be used when a human and/or browser-friendly URL should be transformed into the URL format expected by the web service.</span></span> <span data-ttu-id="2b829-448">Ez a házirend csak kell alkalmazni, amikor az ilyen tiszta URL-címeket, a RESTful URL-címek, a felhasználóbarát URL-címek vagy a Keresőmotor-barát URL-címeket tisztán strukturális URL-címeket, nem tartalmazhat lekérdezési karakterláncot, és ehelyett tartalmaznak az csak az elérési útját az erőforrás (például egy másik URL-formátum Miután a rendszer és a szolgáltató).</span><span class="sxs-lookup"><span data-stu-id="2b829-448">This policy only needs to be applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only the path of the resource (after the scheme and the authority).</span></span> <span data-ttu-id="2b829-449">Ez gyakran történik esztétikai, a használhatóság és a keresőmotor (keresőmotor-Optimalizáláshoz) optimalizálási céllal.</span><span class="sxs-lookup"><span data-stu-id="2b829-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="2b829-450">Csak a házirenddel lekérdezési karakterlánc paramétereket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="2b829-450">You can only add query string parameters using the policy.</span></span> <span data-ttu-id="2b829-451">Extra Sablonparaméterek elérési útja nem adható hozzá az átírást URL-címben.</span><span class="sxs-lookup"><span data-stu-id="2b829-451">You cannot add extra template path parameters in the rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="2b829-452">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="2b829-453">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-453">Example</span></span>  
  
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
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

### <a name="elements"></a><span data-ttu-id="2b829-454">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-454">Elements</span></span>  
  
|<span data-ttu-id="2b829-455">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-455">Name</span></span>|<span data-ttu-id="2b829-456">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-456">Description</span></span>|<span data-ttu-id="2b829-457">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-458">Módosítsa úgy-uri</span><span class="sxs-lookup"><span data-stu-id="2b829-458">rewrite-uri</span></span>|<span data-ttu-id="2b829-459">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-459">Root element.</span></span>|<span data-ttu-id="2b829-460">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2b829-461">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="2b829-461">Attributes</span></span>  
  
|<span data-ttu-id="2b829-462">Attribútum</span><span class="sxs-lookup"><span data-stu-id="2b829-462">Attribute</span></span>|<span data-ttu-id="2b829-463">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-463">Description</span></span>|<span data-ttu-id="2b829-464">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-464">Required</span></span>|<span data-ttu-id="2b829-465">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2b829-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="2b829-466">sablon</span><span class="sxs-lookup"><span data-stu-id="2b829-466">template</span></span>|<span data-ttu-id="2b829-467">A tényleges webes szolgáltatás URL-cím elé a lekérdezési karakterlánc paramétereket.</span><span class="sxs-lookup"><span data-stu-id="2b829-467">The actual web service URL with any query string parameters.</span></span> <span data-ttu-id="2b829-468">Kifejezések használata esetén a teljes érték kifejezésnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2b829-468">When using expressions, the whole value must be an expression.</span></span>|<span data-ttu-id="2b829-469">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-469">Yes</span></span>|<span data-ttu-id="2b829-470">N/A</span><span class="sxs-lookup"><span data-stu-id="2b829-470">N/A</span></span>|  
|<span data-ttu-id="2b829-471">Másolás páratlan számú paraméterei</span><span class="sxs-lookup"><span data-stu-id="2b829-471">copy-unmatched-params</span></span>|<span data-ttu-id="2b829-472">Megadja, hogy nincs jelen az eredeti URL-sablonban a bejövő kérelemben szereplő lekérdezési paraméterek kerülnek újbóli írása sablonban megadott URL-</span><span class="sxs-lookup"><span data-stu-id="2b829-472">Specifies whether query parameters in the incoming request not present in the original URL template are added to the URL defined by the re-write template</span></span>|<span data-ttu-id="2b829-473">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-473">No</span></span>|<span data-ttu-id="2b829-474">Igaz</span><span class="sxs-lookup"><span data-stu-id="2b829-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-475">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-475">Usage</span></span>  
 <span data-ttu-id="2b829-476">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-476">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-477">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="2b829-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="2b829-478">**Házirend hatókörök:** termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="2b829-479"><a name="XSLTransform"></a>Az XSLT-vel XML átalakítása</span><span class="sxs-lookup"><span data-stu-id="2b829-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="2b829-480">A `Transform XML using an XSLT` házirend XSL-átalakítás alkalmazása XML-kérés vagy válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="2b829-480">The `Transform XML using an XSLT` policy applies an XSL transformation to XML in the request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2b829-481">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="2b829-481">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="2b829-482">Példa</span><span class="sxs-lookup"><span data-stu-id="2b829-482">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="2b829-483">Elemek</span><span class="sxs-lookup"><span data-stu-id="2b829-483">Elements</span></span>  
  
|<span data-ttu-id="2b829-484">Név</span><span class="sxs-lookup"><span data-stu-id="2b829-484">Name</span></span>|<span data-ttu-id="2b829-485">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b829-485">Description</span></span>|<span data-ttu-id="2b829-486">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2b829-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2b829-487">XSL-átalakítás</span><span class="sxs-lookup"><span data-stu-id="2b829-487">xsl-transform</span></span>|<span data-ttu-id="2b829-488">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-488">Root element.</span></span>|<span data-ttu-id="2b829-489">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-489">Yes</span></span>|  
|<span data-ttu-id="2b829-490">A paraméter</span><span class="sxs-lookup"><span data-stu-id="2b829-490">parameter</span></span>|<span data-ttu-id="2b829-491">Adja meg a transzformáció bemeneti használt változókat használatával</span><span class="sxs-lookup"><span data-stu-id="2b829-491">Used to define variables used in the transform</span></span>|<span data-ttu-id="2b829-492">Nem</span><span class="sxs-lookup"><span data-stu-id="2b829-492">No</span></span>|  
|<span data-ttu-id="2b829-493">XSL: stylesheet</span><span class="sxs-lookup"><span data-stu-id="2b829-493">xsl:stylesheet</span></span>|<span data-ttu-id="2b829-494">Stíluslap gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="2b829-494">Root stylesheet element.</span></span> <span data-ttu-id="2b829-495">Minden elemek és attribútumok vannak meghatározva hajtsa végre a szabványos [XSLT-specifikációja](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="2b829-495">All elements and attributes defined within follow the standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="2b829-496">Igen</span><span class="sxs-lookup"><span data-stu-id="2b829-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2b829-497">Használat</span><span class="sxs-lookup"><span data-stu-id="2b829-497">Usage</span></span>  
 <span data-ttu-id="2b829-498">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2b829-498">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2b829-499">**Házirend szakaszok:** bejövő, kimenő</span><span class="sxs-lookup"><span data-stu-id="2b829-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="2b829-500">**Házirend hatókörök:** globális, termék, API-művelet</span><span class="sxs-lookup"><span data-stu-id="2b829-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="2b829-501">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b829-501">Next steps</span></span>
<span data-ttu-id="2b829-502">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="2b829-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
