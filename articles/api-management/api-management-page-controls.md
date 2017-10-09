---
title: "az API Management Lapvezérlők aaaAzure |} Microsoft Docs"
description: "További információk a hello Lapvezérlők fejlesztői portál sablonok az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a><span data-ttu-id="d9ae7-103">Az Azure API Management Lapvezérlők</span><span class="sxs-lookup"><span data-stu-id="d9ae7-103">Azure API Management page controls</span></span>
<span data-ttu-id="d9ae7-104">Az Azure API Management vezérlők hello fejlesztői portál sablonok használható a következő hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-104">Azure API Management provides hello following controls for use in hello developer portal templates.</span></span>  
  
 <span data-ttu-id="d9ae7-105">a vezérlő toouse helyezze el szükséges hello helyen hello fejlesztői portálsablon.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-105">toouse a control, place it in hello desired location in hello developer portal template.</span></span> <span data-ttu-id="d9ae7-106">Egyes vezérlők, például a hello [alkalmazás-műveletek](#app-actions) kontrolljához rendelkezik paraméterekkel, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-106">Some controls, such as hello [app-actions](#app-actions) control, have parameters, as shown in hello following example.</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 <span data-ttu-id="d9ae7-107">hello hello paraméter értékét a átadott hello adatmodell hello sablon részeként.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-107">hello values for hello parameters are passed in as part of hello data model for hello template.</span></span> <span data-ttu-id="d9ae7-108">A legtöbb esetben egyszerűen illessze be a hello megadott példa minden vezérlőjének, amíg toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-108">In most cases, you can simply paste in hello provided example for each control for it toowork correctly.</span></span> <span data-ttu-id="d9ae7-109">Hello paraméterértékeket a további információkért lásd: modell adatszakasz hello minden sablon egy vezérlő használja.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-109">For more information on hello parameter values, you can see hello data model section for each template in which a control may be used.</span></span>  
  
 <span data-ttu-id="d9ae7-110">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="d9ae7-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
## <a name="developer-portal-template-page-controls"></a><span data-ttu-id="d9ae7-111">Fejlesztői portálsablon Lapvezérlők</span><span class="sxs-lookup"><span data-stu-id="d9ae7-111">Developer portal template page controls</span></span>  
  
-   [<span data-ttu-id="d9ae7-112">alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-112">app-actions</span></span>](#app-actions)  
  
-   [<span data-ttu-id="d9ae7-113">Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-113">basic-signin</span></span>](#basic-signin)  
  
-   [<span data-ttu-id="d9ae7-114">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="d9ae7-114">paging-control</span></span>](#paging-control)  
  
-   [<span data-ttu-id="d9ae7-115">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="d9ae7-115">providers</span></span>](#providers)  
  
-   [<span data-ttu-id="d9ae7-116">keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="d9ae7-116">search-control</span></span>](#search-control)  
  
-   [<span data-ttu-id="d9ae7-117">-előfizetés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-117">sign-up</span></span>](#sign-up)  
  
-   [<span data-ttu-id="d9ae7-118">előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="d9ae7-118">subscribe-button</span></span>](#subscribe-button)  
  
-   [<span data-ttu-id="d9ae7-119">előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="d9ae7-119">subscription-cancel</span></span>](#subscription-cancel)  
  
##  <span data-ttu-id="d9ae7-120"><a name="app-actions"></a>alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-120"><a name="app-actions"></a> app-actions</span></span>  
 <span data-ttu-id="d9ae7-121">Hello `app-actions` vezérlő felhasználói felülete lehetőséget nyújt az alkalmazások hello felhasználói profil lapon hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-121">hello `app-actions` control provides a user interface for interacting with applications on hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="d9ae7-122">![alkalmazás &#45; műveletek vezérlő](./media/api-management-page-controls/APIM-app-actions-control.png "APIM alkalmazás-műveletek vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-122">![app&#45;actions control](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app-actions control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-123">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-123">Usage</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-124">Parameters</span></span>  
  
|<span data-ttu-id="d9ae7-125">Paraméter</span><span class="sxs-lookup"><span data-stu-id="d9ae7-125">Parameter</span></span>|<span data-ttu-id="d9ae7-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="d9ae7-126">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="d9ae7-127">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="d9ae7-127">appId</span></span>|<span data-ttu-id="d9ae7-128">hello hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-128">hello id of hello application.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-129">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-129">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-130">Hello `app-actions` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-130">hello `app-actions` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-131">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="d9ae7-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
  
##  <span data-ttu-id="d9ae7-132"><a name="basic-signin"></a>Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-132"><a name="basic-signin"></a> basic-signin</span></span>  
 <span data-ttu-id="d9ae7-133">Hello `basic-signin` vezérlő egy felügyeletét biztosítja, a hello adatok gyűjtését a felhasználói bejelentkezési oldal hello developer portálon bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-133">hello `basic-signin` control provides a control for collecting user sign in information in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="d9ae7-134">![Basic &#45; signin vezérlő](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-134">![basic&#45;signin control](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-135">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-135">Usage</span></span>  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-136">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-136">Parameters</span></span>  
 <span data-ttu-id="d9ae7-137">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-137">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-138">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-138">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-139">Hello `basic-signin` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-139">hello `basic-signin` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-140">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-140">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="d9ae7-141"><a name="paging-control"></a>lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="d9ae7-141"><a name="paging-control"></a> paging-control</span></span>  
 <span data-ttu-id="d9ae7-142">Hello `paging-control` lapozás a fejlesztői portálon lapokat biztosít, amelyek elemek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-142">hello `paging-control` provides paging functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="d9ae7-143">![vezérlő lapozási](./media/api-management-page-controls/APIM-paging-control.png "APIM lapozási vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-143">![paging control](./media/api-management-page-controls/APIM-paging-control.png "APIM paging control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-144">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-144">Usage</span></span>  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-145">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-145">Parameters</span></span>  
 <span data-ttu-id="d9ae7-146">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-146">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-147">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-147">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-148">Hello `paging-control` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-148">hello `paging-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-149">API-lista</span><span class="sxs-lookup"><span data-stu-id="d9ae7-149">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="d9ae7-150">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="d9ae7-150">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
  
-   [<span data-ttu-id="d9ae7-151">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="d9ae7-151">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="d9ae7-152"><a name="providers"></a>szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="d9ae7-152"><a name="providers"></a> providers</span></span>  
 <span data-ttu-id="d9ae7-153">Hello `providers` vezérlője egy vezérlő kiválasztásának hitelesítésszolgáltatók a hello bejelentkezési oldal hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-153">hello `providers` control provides a control for selection of authentication providers in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="d9ae7-154">![szolgáltatók vezérlő](./media/api-management-page-controls/APIM-providers-control.png "APIM szolgáltatók vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-154">![providers control](./media/api-management-page-controls/APIM-providers-control.png "APIM providers control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-155">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-155">Usage</span></span>  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-156">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-156">Parameters</span></span>  
 <span data-ttu-id="d9ae7-157">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-157">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-158">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-158">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-159">Hello `providers` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-159">hello `providers` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-160">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-160">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="d9ae7-161"><a name="search-control"></a>keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="d9ae7-161"><a name="search-control"></a> search-control</span></span>  
 <span data-ttu-id="d9ae7-162">Hello `search-control` keresési funkciókat biztosítja a fejlesztői portál által felügyelt oldalak felhívásainak elemek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-162">hello `search-control` provides search functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="d9ae7-163">![Keresés vezérlő](./media/api-management-page-controls/APIM-search-control.png "APIM keresési vezérlőelemet")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-163">![search control](./media/api-management-page-controls/APIM-search-control.png "APIM search control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-164">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-164">Usage</span></span>  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-165">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-165">Parameters</span></span>  
 <span data-ttu-id="d9ae7-166">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-166">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-167">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-167">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-168">Hello `search-control` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-168">hello `search-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-169">API-lista</span><span class="sxs-lookup"><span data-stu-id="d9ae7-169">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="d9ae7-170">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="d9ae7-170">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="d9ae7-171"><a name="sign-up"></a>-előfizetés</span><span class="sxs-lookup"><span data-stu-id="d9ae7-171"><a name="sign-up"></a> sign-up</span></span>  
 <span data-ttu-id="d9ae7-172">Hello `sign-up` vezérlő biztosít egy vezérlő hello bejelentkezési oldal hello developer portálon a felhasználói profillal kapcsolatos információk gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-172">hello `sign-up` control provides a control for collecting user profile information in hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="d9ae7-173">![bejelentkezési &#45; vezérlő legfeljebb](./media/api-management-page-controls/APIM-sign-up-control.png "APIM előfizetési vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-173">![sign&#45;up control](./media/api-management-page-controls/APIM-sign-up-control.png "APIM sign-up control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-174">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-174">Usage</span></span>  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-175">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-175">Parameters</span></span>  
 <span data-ttu-id="d9ae7-176">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-176">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-177">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-177">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-178">Hello `sign-up` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-178">hello `sign-up` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-179">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="d9ae7-179">Sign up</span></span>](api-management-page-templates.md#SignUp)  
  
##  <span data-ttu-id="d9ae7-180"><a name="subscribe-button"></a>előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="d9ae7-180"><a name="subscribe-button"></a> subscribe-button</span></span>  
 <span data-ttu-id="d9ae7-181">Hello `subscribe-button` vezérlőt biztosít a felhasználó tooa termék előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-181">hello `subscribe-button` provides a control for subscribing a user tooa product.</span></span>  
  
 <span data-ttu-id="d9ae7-182">![előfizetés &#45; vezérlőelem](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM előfizetés gomb vezérlőelem")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-182">![subscribe&#45;button control](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM subscribe-button control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-183">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-183">Usage</span></span>  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-184">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-184">Parameters</span></span>  
 <span data-ttu-id="d9ae7-185">nincs.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-185">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-186">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-186">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-187">Hello `subscribe-button` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-187">hello `subscribe-button` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-188">A termék</span><span class="sxs-lookup"><span data-stu-id="d9ae7-188">Product</span></span>](api-management-product-templates.md#Product)  
  
##  <span data-ttu-id="d9ae7-189"><a name="subscription-cancel"></a>előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="d9ae7-189"><a name="subscription-cancel"></a> subscription-cancel</span></span>  
 <span data-ttu-id="d9ae7-190">Hello `subscription-cancel` vezérlője egy vezérlő törlésének egy előfizetés tooa termék hello felhasználói profil lapon hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-190">hello `subscription-cancel` control provides a control for cancelling a subscription tooa product in hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="d9ae7-191">![előfizetés &#45; szakítsa meg a vezérlő](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM előfizetés-Mégse vezérlő")</span><span class="sxs-lookup"><span data-stu-id="d9ae7-191">![subscription&#45;cancel control](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM subscription-cancel control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="d9ae7-192">Használat</span><span class="sxs-lookup"><span data-stu-id="d9ae7-192">Usage</span></span>  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a><span data-ttu-id="d9ae7-193">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d9ae7-193">Parameters</span></span>  
  
|<span data-ttu-id="d9ae7-194">Paraméter</span><span class="sxs-lookup"><span data-stu-id="d9ae7-194">Parameter</span></span>|<span data-ttu-id="d9ae7-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="d9ae7-195">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="d9ae7-196">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="d9ae7-196">subscriptionId</span></span>|<span data-ttu-id="d9ae7-197">hello előfizetés toocancel hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-197">hello id of hello subscription toocancel.</span></span>|  
|<span data-ttu-id="d9ae7-198">CancelUrl</span><span class="sxs-lookup"><span data-stu-id="d9ae7-198">cancelUrl</span></span>|<span data-ttu-id="d9ae7-199">hello előfizetés szakítsa meg az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-199">hello subscription cancel URL.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="d9ae7-200">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="d9ae7-200">Developer portal templates</span></span>  
 <span data-ttu-id="d9ae7-201">Hello `subscription-cancel` hello fejlesztői portál sablonok a következő vezérlő is használható.</span><span class="sxs-lookup"><span data-stu-id="d9ae7-201">hello `subscription-cancel` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="d9ae7-202">A termék</span><span class="sxs-lookup"><span data-stu-id="d9ae7-202">Product</span></span>](api-management-product-templates.md#Product)

## <a name="next-steps"></a><span data-ttu-id="d9ae7-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9ae7-203">Next steps</span></span>
<span data-ttu-id="d9ae7-204">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d9ae7-204">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
