---
title: "Az Azure API Management Lapvezérlők |} Microsoft Docs"
description: "További tudnivalók a Lapvezérlők fejlesztői portál sablonok az Azure API Management használható."
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
ms.openlocfilehash: 1ce0657aebe34d093ae94281de208c929067742a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-page-controls"></a><span data-ttu-id="aadfb-103">Az Azure API Management Lapvezérlők</span><span class="sxs-lookup"><span data-stu-id="aadfb-103">Azure API Management page controls</span></span>
<span data-ttu-id="aadfb-104">Az Azure API Management a következő vezérlőelemeket biztosítja a fejlesztői használható portál sablonok.</span><span class="sxs-lookup"><span data-stu-id="aadfb-104">Azure API Management provides the following controls for use in the developer portal templates.</span></span>  
  
 <span data-ttu-id="aadfb-105">A vezérlő használatához helyezze el a kívánt helyre a fejlesztői portálon sablonban.</span><span class="sxs-lookup"><span data-stu-id="aadfb-105">To use a control, place it in the desired location in the developer portal template.</span></span> <span data-ttu-id="aadfb-106">Néhány szabályozza, mint például a [alkalmazás-műveletek](#app-actions) kontrolljához paraméterekkel rendelkezik, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="aadfb-106">Some controls, such as the [app-actions](#app-actions) control, have parameters, as shown in the following example.</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 <span data-ttu-id="aadfb-107">A paraméterek értékeit az adatmodell a sablon részeként átadott a.</span><span class="sxs-lookup"><span data-stu-id="aadfb-107">The values for the parameters are passed in as part of the data model for the template.</span></span> <span data-ttu-id="aadfb-108">A legtöbb esetben egyszerűen illessze be a megadott példa minden vezérlőjének, ahhoz, hogy a megfelelő működéséhez.</span><span class="sxs-lookup"><span data-stu-id="aadfb-108">In most cases, you can simply paste in the provided example for each control for it to work correctly.</span></span> <span data-ttu-id="aadfb-109">A paraméterértékek további információkért lásd: az adatszakasz modell minden sablon egy vezérlő használja.</span><span class="sxs-lookup"><span data-stu-id="aadfb-109">For more information on the parameter values, you can see the data model section for each template in which a control may be used.</span></span>  
  
 <span data-ttu-id="aadfb-110">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="aadfb-110">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
## <a name="developer-portal-template-page-controls"></a><span data-ttu-id="aadfb-111">Fejlesztői portálsablon Lapvezérlők</span><span class="sxs-lookup"><span data-stu-id="aadfb-111">Developer portal template page controls</span></span>  
  
-   [<span data-ttu-id="aadfb-112">alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="aadfb-112">app-actions</span></span>](#app-actions)  
  
-   [<span data-ttu-id="aadfb-113">Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aadfb-113">basic-signin</span></span>](#basic-signin)  
  
-   [<span data-ttu-id="aadfb-114">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="aadfb-114">paging-control</span></span>](#paging-control)  
  
-   [<span data-ttu-id="aadfb-115">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="aadfb-115">providers</span></span>](#providers)  
  
-   [<span data-ttu-id="aadfb-116">keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="aadfb-116">search-control</span></span>](#search-control)  
  
-   [<span data-ttu-id="aadfb-117">-előfizetés</span><span class="sxs-lookup"><span data-stu-id="aadfb-117">sign-up</span></span>](#sign-up)  
  
-   [<span data-ttu-id="aadfb-118">előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="aadfb-118">subscribe-button</span></span>](#subscribe-button)  
  
-   [<span data-ttu-id="aadfb-119">előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="aadfb-119">subscription-cancel</span></span>](#subscription-cancel)  
  
##  <span data-ttu-id="aadfb-120"><a name="app-actions"></a>alkalmazás-műveletek</span><span class="sxs-lookup"><span data-stu-id="aadfb-120"><a name="app-actions"></a> app-actions</span></span>  
 <span data-ttu-id="aadfb-121">A `app-actions` vezérlő felhasználói felülete lehetőséget nyújt az alkalmazások a felhasználó oldalon a fejlesztői portálra való interakció.</span><span class="sxs-lookup"><span data-stu-id="aadfb-121">The `app-actions` control provides a user interface for interacting with applications on the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="aadfb-122">![alkalmazás &#45; műveletek vezérlő](./media/api-management-page-controls/APIM-app-actions-control.png "APIM alkalmazás-műveletek vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-122">![app&#45;actions control](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app-actions control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-123">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-123">Usage</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-124">Parameters</span></span>  
  
|<span data-ttu-id="aadfb-125">Paraméter</span><span class="sxs-lookup"><span data-stu-id="aadfb-125">Parameter</span></span>|<span data-ttu-id="aadfb-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="aadfb-126">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="aadfb-127">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="aadfb-127">appId</span></span>|<span data-ttu-id="aadfb-128">Az alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="aadfb-128">The id of the application.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-129">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-129">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-130">A `app-actions` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-130">The `app-actions` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-131">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="aadfb-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
  
##  <span data-ttu-id="aadfb-132"><a name="basic-signin"></a>Basic-bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aadfb-132"><a name="basic-signin"></a> basic-signin</span></span>  
 <span data-ttu-id="aadfb-133">A `basic-signin` vezérlő egy felügyeletét biztosítja, a felhasználói bejelentkezési a bejelentkezési oldal a fejlesztői portálra található információk gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="aadfb-133">The `basic-signin` control provides a control for collecting user sign in information in the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="aadfb-134">![Basic &#45; signin vezérlő](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-134">![basic&#45;signin control](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-135">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-135">Usage</span></span>  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-136">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-136">Parameters</span></span>  
 <span data-ttu-id="aadfb-137">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-137">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-138">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-138">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-139">A `basic-signin` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-139">The `basic-signin` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-140">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aadfb-140">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="aadfb-141"><a name="paging-control"></a>lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="aadfb-141"><a name="paging-control"></a> paging-control</span></span>  
 <span data-ttu-id="aadfb-142">A `paging-control` lapozás a fejlesztői portálon lapokat biztosít, amelyek elemek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="aadfb-142">The `paging-control` provides paging functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="aadfb-143">![vezérlő lapozási](./media/api-management-page-controls/APIM-paging-control.png "APIM lapozási vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-143">![paging control](./media/api-management-page-controls/APIM-paging-control.png "APIM paging control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-144">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-144">Usage</span></span>  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-145">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-145">Parameters</span></span>  
 <span data-ttu-id="aadfb-146">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-146">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-147">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-147">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-148">A `paging-control` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-148">The `paging-control` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-149">API-lista</span><span class="sxs-lookup"><span data-stu-id="aadfb-149">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="aadfb-150">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="aadfb-150">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
  
-   [<span data-ttu-id="aadfb-151">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="aadfb-151">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="aadfb-152"><a name="providers"></a>szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="aadfb-152"><a name="providers"></a> providers</span></span>  
 <span data-ttu-id="aadfb-153">A `providers` vezérlő vezérlőt biztosít a bejelentkezési oldalon a fejlesztői portálra hitelesítésszolgáltatókat kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="aadfb-153">The `providers` control provides a control for selection of authentication providers in the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="aadfb-154">![szolgáltatók vezérlő](./media/api-management-page-controls/APIM-providers-control.png "APIM szolgáltatók vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-154">![providers control](./media/api-management-page-controls/APIM-providers-control.png "APIM providers control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-155">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-155">Usage</span></span>  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-156">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-156">Parameters</span></span>  
 <span data-ttu-id="aadfb-157">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-157">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-158">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-158">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-159">A `providers` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-159">The `providers` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-160">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aadfb-160">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="aadfb-161"><a name="search-control"></a>keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="aadfb-161"><a name="search-control"></a> search-control</span></span>  
 <span data-ttu-id="aadfb-162">A `search-control` keresési funkciókat biztosítja a fejlesztői portál által felügyelt oldalak felhívásainak elemek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="aadfb-162">The `search-control` provides search functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="aadfb-163">![Keresés vezérlő](./media/api-management-page-controls/APIM-search-control.png "APIM keresési vezérlőelemet")</span><span class="sxs-lookup"><span data-stu-id="aadfb-163">![search control](./media/api-management-page-controls/APIM-search-control.png "APIM search control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-164">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-164">Usage</span></span>  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-165">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-165">Parameters</span></span>  
 <span data-ttu-id="aadfb-166">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-166">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-167">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-167">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-168">A `search-control` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-168">The `search-control` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-169">API-lista</span><span class="sxs-lookup"><span data-stu-id="aadfb-169">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="aadfb-170">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="aadfb-170">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="aadfb-171"><a name="sign-up"></a>-előfizetés</span><span class="sxs-lookup"><span data-stu-id="aadfb-171"><a name="sign-up"></a> sign-up</span></span>  
 <span data-ttu-id="aadfb-172">A `sign-up` vezérlő egy felügyeletét biztosítja, a bejelentkezési oldal a fejlesztői portálra a felhasználói profillal kapcsolatos információk gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="aadfb-172">The `sign-up` control provides a control for collecting user profile information in the sign up page in the developer portal.</span></span>  
  
 <span data-ttu-id="aadfb-173">![bejelentkezési &#45; vezérlő legfeljebb](./media/api-management-page-controls/APIM-sign-up-control.png "APIM előfizetési vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-173">![sign&#45;up control](./media/api-management-page-controls/APIM-sign-up-control.png "APIM sign-up control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-174">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-174">Usage</span></span>  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-175">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-175">Parameters</span></span>  
 <span data-ttu-id="aadfb-176">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-176">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-177">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-177">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-178">A `sign-up` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-178">The `sign-up` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-179">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="aadfb-179">Sign up</span></span>](api-management-page-templates.md#SignUp)  
  
##  <span data-ttu-id="aadfb-180"><a name="subscribe-button"></a>előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="aadfb-180"><a name="subscribe-button"></a> subscribe-button</span></span>  
 <span data-ttu-id="aadfb-181">A `subscribe-button` vezérlőt biztosít a felhasználót, hogy a termék előfizetés.</span><span class="sxs-lookup"><span data-stu-id="aadfb-181">The `subscribe-button` provides a control for subscribing a user to a product.</span></span>  
  
 <span data-ttu-id="aadfb-182">![előfizetés &#45; vezérlőelem](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM előfizetés gomb vezérlőelem")</span><span class="sxs-lookup"><span data-stu-id="aadfb-182">![subscribe&#45;button control](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM subscribe-button control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-183">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-183">Usage</span></span>  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-184">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-184">Parameters</span></span>  
 <span data-ttu-id="aadfb-185">nincs.</span><span class="sxs-lookup"><span data-stu-id="aadfb-185">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-186">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-186">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-187">A `subscribe-button` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-187">The `subscribe-button` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-188">A termék</span><span class="sxs-lookup"><span data-stu-id="aadfb-188">Product</span></span>](api-management-product-templates.md#Product)  
  
##  <span data-ttu-id="aadfb-189"><a name="subscription-cancel"></a>előfizetés-Mégse</span><span class="sxs-lookup"><span data-stu-id="aadfb-189"><a name="subscription-cancel"></a> subscription-cancel</span></span>  
 <span data-ttu-id="aadfb-190">A `subscription-cancel` vezérlő vezérlőt biztosít a felhasználó oldalon a fejlesztői portálra való előfizetés megszakítása.</span><span class="sxs-lookup"><span data-stu-id="aadfb-190">The `subscription-cancel` control provides a control for cancelling a subscription to a product in the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="aadfb-191">![előfizetés &#45; szakítsa meg a vezérlő](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM előfizetés-Mégse vezérlő")</span><span class="sxs-lookup"><span data-stu-id="aadfb-191">![subscription&#45;cancel control](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM subscription-cancel control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="aadfb-192">Használat</span><span class="sxs-lookup"><span data-stu-id="aadfb-192">Usage</span></span>  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a><span data-ttu-id="aadfb-193">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="aadfb-193">Parameters</span></span>  
  
|<span data-ttu-id="aadfb-194">Paraméter</span><span class="sxs-lookup"><span data-stu-id="aadfb-194">Parameter</span></span>|<span data-ttu-id="aadfb-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="aadfb-195">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="aadfb-196">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="aadfb-196">subscriptionId</span></span>|<span data-ttu-id="aadfb-197">Megszakítja az előfizetés azonosítója.</span><span class="sxs-lookup"><span data-stu-id="aadfb-197">The id of the subscription to cancel.</span></span>|  
|<span data-ttu-id="aadfb-198">CancelUrl</span><span class="sxs-lookup"><span data-stu-id="aadfb-198">cancelUrl</span></span>|<span data-ttu-id="aadfb-199">Az előfizetés szakítsa meg az URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="aadfb-199">The subscription cancel URL.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="aadfb-200">Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="aadfb-200">Developer portal templates</span></span>  
 <span data-ttu-id="aadfb-201">A `subscription-cancel` vezérlő a következő fejlesztői portál sablonok is használható.</span><span class="sxs-lookup"><span data-stu-id="aadfb-201">The `subscription-cancel` control may be used in the following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="aadfb-202">A termék</span><span class="sxs-lookup"><span data-stu-id="aadfb-202">Product</span></span>](api-management-product-templates.md#Product)

## <a name="next-steps"></a><span data-ttu-id="aadfb-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aadfb-203">Next steps</span></span>
<span data-ttu-id="aadfb-204">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="aadfb-204">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>