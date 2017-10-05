---
title: "Az Azure API Management hitelesítési házirendek |} Microsoft Docs"
description: "Ismerje meg a hitelesítési házirendek az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 63ef20a56ab7721f9ecc7025d05963cc4b0c27a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="1e3b1-103">Az API Management hitelesítési házirendek</span><span class="sxs-lookup"><span data-stu-id="1e3b1-103">API Management authentication policies</span></span>
<span data-ttu-id="1e3b1-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="1e3b1-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="1e3b1-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="1e3b1-106"><a name="AuthenticationPolicies"></a>Hitelesítési házirendek</span><span class="sxs-lookup"><span data-stu-id="1e3b1-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="1e3b1-107">[Basic hitelesítés](api-management-authentication-policies.md#Basic) -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="1e3b1-108">[Az ügyféltanúsítvány hitelesítéséhez](api-management-authentication-policies.md#ClientCertificate) -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="1e3b1-109"><a name="Basic"></a>Alapszintű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1e3b1-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="1e3b1-110">Használja a `authentication-basic` házirend az egyszerű hitelesítést használ a háttérszolgáltatáshoz való hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-110">Use the `authentication-basic` policy to authenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="1e3b1-111">Ez a házirend hatékonyan állítja be a HTTP Authorization fejlécet a házirendben megadott hitelesítő adatokhoz megfelelő értéket.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-111">This policy effectively sets the HTTP Authorization header to the value corresponding to the credentials provided in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1e3b1-112">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="1e3b1-113">Példa</span><span class="sxs-lookup"><span data-stu-id="1e3b1-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="1e3b1-114">Elemek</span><span class="sxs-lookup"><span data-stu-id="1e3b1-114">Elements</span></span>  
  
|<span data-ttu-id="1e3b1-115">Név</span><span class="sxs-lookup"><span data-stu-id="1e3b1-115">Name</span></span>|<span data-ttu-id="1e3b1-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-116">Description</span></span>|<span data-ttu-id="1e3b1-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1e3b1-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="1e3b1-118">egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1e3b1-118">authentication-basic</span></span>|<span data-ttu-id="1e3b1-119">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-119">Root element.</span></span>|<span data-ttu-id="1e3b1-120">Igen</span><span class="sxs-lookup"><span data-stu-id="1e3b1-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1e3b1-121">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1e3b1-121">Attributes</span></span>  
  
|<span data-ttu-id="1e3b1-122">Név</span><span class="sxs-lookup"><span data-stu-id="1e3b1-122">Name</span></span>|<span data-ttu-id="1e3b1-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-123">Description</span></span>|<span data-ttu-id="1e3b1-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1e3b1-124">Required</span></span>|<span data-ttu-id="1e3b1-125">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1e3b1-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="1e3b1-126">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="1e3b1-126">username</span></span>|<span data-ttu-id="1e3b1-127">Az alapszintű hitelesítő adat felhasználónevet adja meg.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-127">Specifies the username of the Basic credential.</span></span>|<span data-ttu-id="1e3b1-128">Igen</span><span class="sxs-lookup"><span data-stu-id="1e3b1-128">Yes</span></span>|<span data-ttu-id="1e3b1-129">N/A</span><span class="sxs-lookup"><span data-stu-id="1e3b1-129">N/A</span></span>|  
|<span data-ttu-id="1e3b1-130">jelszó</span><span class="sxs-lookup"><span data-stu-id="1e3b1-130">password</span></span>|<span data-ttu-id="1e3b1-131">Megadja az alapvető hitelesítő adat.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-131">Specifies the password of the Basic credential.</span></span>|<span data-ttu-id="1e3b1-132">Igen</span><span class="sxs-lookup"><span data-stu-id="1e3b1-132">Yes</span></span>|<span data-ttu-id="1e3b1-133">N/A</span><span class="sxs-lookup"><span data-stu-id="1e3b1-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1e3b1-134">Használat</span><span class="sxs-lookup"><span data-stu-id="1e3b1-134">Usage</span></span>  
 <span data-ttu-id="1e3b1-135">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1e3b1-135">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1e3b1-136">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="1e3b1-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="1e3b1-137">**Házirend hatókörök:** API</span><span class="sxs-lookup"><span data-stu-id="1e3b1-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="1e3b1-138"><a name="ClientCertificate"></a>Az ügyféltanúsítvány hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="1e3b1-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="1e3b1-139">Használja a `authentication-certificate` házirend az ügyféltanúsítvány használatával háttérszolgáltatás való hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-139">Use the `authentication-certificate` policy to authenticate with a backend service using client certificate.</span></span> <span data-ttu-id="1e3b1-140">A tanúsítvány kell lennie [telepítve az API Management](http://go.microsoft.com/fwlink/?LinkID=511599) első és az ujjlenyomat azonosíthatók.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-140">The certificate needs to be [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="1e3b1-141">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="1e3b1-142">Példa</span><span class="sxs-lookup"><span data-stu-id="1e3b1-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="1e3b1-143">Elemek</span><span class="sxs-lookup"><span data-stu-id="1e3b1-143">Elements</span></span>  
  
|<span data-ttu-id="1e3b1-144">Név</span><span class="sxs-lookup"><span data-stu-id="1e3b1-144">Name</span></span>|<span data-ttu-id="1e3b1-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-145">Description</span></span>|<span data-ttu-id="1e3b1-146">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1e3b1-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="1e3b1-147">hitelesítési tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="1e3b1-147">authentication-certificate</span></span>|<span data-ttu-id="1e3b1-148">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-148">Root element.</span></span>|<span data-ttu-id="1e3b1-149">Igen</span><span class="sxs-lookup"><span data-stu-id="1e3b1-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="1e3b1-150">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="1e3b1-150">Attributes</span></span>  
  
|<span data-ttu-id="1e3b1-151">Név</span><span class="sxs-lookup"><span data-stu-id="1e3b1-151">Name</span></span>|<span data-ttu-id="1e3b1-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="1e3b1-152">Description</span></span>|<span data-ttu-id="1e3b1-153">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1e3b1-153">Required</span></span>|<span data-ttu-id="1e3b1-154">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="1e3b1-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="1e3b1-155">ujjlenyomat</span><span class="sxs-lookup"><span data-stu-id="1e3b1-155">thumbprint</span></span>|<span data-ttu-id="1e3b1-156">Az ügyféltanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="1e3b1-156">The thumbprint for the client certificate.</span></span>|<span data-ttu-id="1e3b1-157">Igen</span><span class="sxs-lookup"><span data-stu-id="1e3b1-157">Yes</span></span>|<span data-ttu-id="1e3b1-158">N/A</span><span class="sxs-lookup"><span data-stu-id="1e3b1-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="1e3b1-159">Használat</span><span class="sxs-lookup"><span data-stu-id="1e3b1-159">Usage</span></span>  
 <span data-ttu-id="1e3b1-160">Ez a házirend használható a következő házirend [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="1e3b1-160">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="1e3b1-161">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="1e3b1-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="1e3b1-162">**Házirend hatókörök:** API</span><span class="sxs-lookup"><span data-stu-id="1e3b1-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="1e3b1-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e3b1-163">Next steps</span></span>
<span data-ttu-id="1e3b1-164">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1e3b1-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  