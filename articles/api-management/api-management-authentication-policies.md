---
title: "aaaAzure API Management hitelesítési házirendek |} Microsoft Docs"
description: "További tudnivalók hello hitelesítési házirendek az Azure API Management használható."
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
ms.openlocfilehash: ce93cced66cb67520e97c7c15f3685bffb08e1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="38d4a-103">Az API Management hitelesítési házirendek</span><span class="sxs-lookup"><span data-stu-id="38d4a-103">API Management authentication policies</span></span>
<span data-ttu-id="38d4a-104">Ez a témakör egy hivatkozást a következő API-felügyeleti házirendek hello.</span><span class="sxs-lookup"><span data-stu-id="38d4a-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="38d4a-105">Hozzáadása és házirendek konfigurálásával kapcsolatos tudnivalókat lásd: [házirendek az API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="38d4a-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="38d4a-106"><a name="AuthenticationPolicies"></a>Hitelesítési házirendek</span><span class="sxs-lookup"><span data-stu-id="38d4a-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="38d4a-107">[Basic hitelesítés](api-management-authentication-policies.md#Basic) -alapszintű hitelesítést használó háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="38d4a-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="38d4a-108">[Az ügyféltanúsítvány hitelesítéséhez](api-management-authentication-policies.md#ClientCertificate) -az ügyféltanúsítványok háttérszolgáltatás a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="38d4a-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="38d4a-109"><a name="Basic"></a>Alapszintű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="38d4a-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="38d4a-110">Használjon hello `authentication-basic` házirend tooauthenticate alapszintű hitelesítést használ, egy háttér szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="38d4a-110">Use hello `authentication-basic` policy tooauthenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="38d4a-111">Ez a házirend hatékonyan hello HTTP engedélyezési fejléc toohello érték megfelelő toohello hitelesítő adatok beállítása a hello házirendekben.</span><span class="sxs-lookup"><span data-stu-id="38d4a-111">This policy effectively sets hello HTTP Authorization header toohello value corresponding toohello credentials provided in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="38d4a-112">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="38d4a-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="38d4a-113">Példa</span><span class="sxs-lookup"><span data-stu-id="38d4a-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="38d4a-114">Elemek</span><span class="sxs-lookup"><span data-stu-id="38d4a-114">Elements</span></span>  
  
|<span data-ttu-id="38d4a-115">Név</span><span class="sxs-lookup"><span data-stu-id="38d4a-115">Name</span></span>|<span data-ttu-id="38d4a-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="38d4a-116">Description</span></span>|<span data-ttu-id="38d4a-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="38d4a-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="38d4a-118">egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="38d4a-118">authentication-basic</span></span>|<span data-ttu-id="38d4a-119">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="38d4a-119">Root element.</span></span>|<span data-ttu-id="38d4a-120">Igen</span><span class="sxs-lookup"><span data-stu-id="38d4a-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="38d4a-121">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="38d4a-121">Attributes</span></span>  
  
|<span data-ttu-id="38d4a-122">Név</span><span class="sxs-lookup"><span data-stu-id="38d4a-122">Name</span></span>|<span data-ttu-id="38d4a-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="38d4a-123">Description</span></span>|<span data-ttu-id="38d4a-124">Szükséges</span><span class="sxs-lookup"><span data-stu-id="38d4a-124">Required</span></span>|<span data-ttu-id="38d4a-125">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="38d4a-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="38d4a-126">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="38d4a-126">username</span></span>|<span data-ttu-id="38d4a-127">Hello felhasználónév a hello alapvető hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="38d4a-127">Specifies hello username of hello Basic credential.</span></span>|<span data-ttu-id="38d4a-128">Igen</span><span class="sxs-lookup"><span data-stu-id="38d4a-128">Yes</span></span>|<span data-ttu-id="38d4a-129">N/A</span><span class="sxs-lookup"><span data-stu-id="38d4a-129">N/A</span></span>|  
|<span data-ttu-id="38d4a-130">jelszó</span><span class="sxs-lookup"><span data-stu-id="38d4a-130">password</span></span>|<span data-ttu-id="38d4a-131">A hello alapvető hitelesítő adatok hello jelszót ad meg.</span><span class="sxs-lookup"><span data-stu-id="38d4a-131">Specifies hello password of hello Basic credential.</span></span>|<span data-ttu-id="38d4a-132">Igen</span><span class="sxs-lookup"><span data-stu-id="38d4a-132">Yes</span></span>|<span data-ttu-id="38d4a-133">N/A</span><span class="sxs-lookup"><span data-stu-id="38d4a-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="38d4a-134">Használat</span><span class="sxs-lookup"><span data-stu-id="38d4a-134">Usage</span></span>  
 <span data-ttu-id="38d4a-135">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="38d4a-135">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="38d4a-136">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="38d4a-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="38d4a-137">**Házirend hatókörök:** API</span><span class="sxs-lookup"><span data-stu-id="38d4a-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="38d4a-138"><a name="ClientCertificate"></a>Az ügyféltanúsítvány hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="38d4a-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="38d4a-139">Használjon hello `authentication-certificate` házirend tooauthenticate egy háttér szolgáltatással ügyféltanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="38d4a-139">Use hello `authentication-certificate` policy tooauthenticate with a backend service using client certificate.</span></span> <span data-ttu-id="38d4a-140">hello tanúsítványt kell toobe [telepítve az API Management](http://go.microsoft.com/fwlink/?LinkID=511599) első és az ujjlenyomat azonosít.</span><span class="sxs-lookup"><span data-stu-id="38d4a-140">hello certificate needs toobe [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="38d4a-141">Házirendutasítás</span><span class="sxs-lookup"><span data-stu-id="38d4a-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="38d4a-142">Példa</span><span class="sxs-lookup"><span data-stu-id="38d4a-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="38d4a-143">Elemek</span><span class="sxs-lookup"><span data-stu-id="38d4a-143">Elements</span></span>  
  
|<span data-ttu-id="38d4a-144">Név</span><span class="sxs-lookup"><span data-stu-id="38d4a-144">Name</span></span>|<span data-ttu-id="38d4a-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="38d4a-145">Description</span></span>|<span data-ttu-id="38d4a-146">Szükséges</span><span class="sxs-lookup"><span data-stu-id="38d4a-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="38d4a-147">hitelesítési tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="38d4a-147">authentication-certificate</span></span>|<span data-ttu-id="38d4a-148">A gyökérelem.</span><span class="sxs-lookup"><span data-stu-id="38d4a-148">Root element.</span></span>|<span data-ttu-id="38d4a-149">Igen</span><span class="sxs-lookup"><span data-stu-id="38d4a-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="38d4a-150">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="38d4a-150">Attributes</span></span>  
  
|<span data-ttu-id="38d4a-151">Név</span><span class="sxs-lookup"><span data-stu-id="38d4a-151">Name</span></span>|<span data-ttu-id="38d4a-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="38d4a-152">Description</span></span>|<span data-ttu-id="38d4a-153">Szükséges</span><span class="sxs-lookup"><span data-stu-id="38d4a-153">Required</span></span>|<span data-ttu-id="38d4a-154">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="38d4a-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="38d4a-155">ujjlenyomat</span><span class="sxs-lookup"><span data-stu-id="38d4a-155">thumbprint</span></span>|<span data-ttu-id="38d4a-156">hello hello-ügyfél tanúsítványának ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="38d4a-156">hello thumbprint for hello client certificate.</span></span>|<span data-ttu-id="38d4a-157">Igen</span><span class="sxs-lookup"><span data-stu-id="38d4a-157">Yes</span></span>|<span data-ttu-id="38d4a-158">N/A</span><span class="sxs-lookup"><span data-stu-id="38d4a-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="38d4a-159">Használat</span><span class="sxs-lookup"><span data-stu-id="38d4a-159">Usage</span></span>  
 <span data-ttu-id="38d4a-160">Ez a házirend használható a következő házirend hello [szakaszok](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) és [hatókörök](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="38d4a-160">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="38d4a-161">**Házirend szakaszok:** bejövő</span><span class="sxs-lookup"><span data-stu-id="38d4a-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="38d4a-162">**Házirend hatókörök:** API</span><span class="sxs-lookup"><span data-stu-id="38d4a-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="38d4a-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="38d4a-163">Next steps</span></span>
<span data-ttu-id="38d4a-164">Házirendek használata további információkért lásd: [házirendek az API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="38d4a-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  