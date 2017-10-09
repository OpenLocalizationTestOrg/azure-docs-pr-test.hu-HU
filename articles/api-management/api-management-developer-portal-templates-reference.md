---
title: "aaaAzure API Management Developer portálon sablonok |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize hello fejlesztői sablonok használatával az Azure API Management portál lapjai tartalmát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 5189f3d8-2a4c-4dc8-ab19-11c7df0114d4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 32908712a690dcf530038476ab323978c3e9a1d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-developer-portal-templates"></a><span data-ttu-id="382b9-103">Az Azure API Management fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="382b9-103">Azure API Management Developer Portal Templates</span></span>
<span data-ttu-id="382b9-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="382b9-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="382b9-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="382b9-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="382b9-106">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="382b9-106">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  



  
##  <span data-ttu-id="382b9-107"><a name="DeveloperPortalTemplates"></a>Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="382b9-107"><a name="DeveloperPortalTemplates"></a> Developer portal templates</span></span>  
  
-   [<span data-ttu-id="382b9-108">API-k</span><span class="sxs-lookup"><span data-stu-id="382b9-108">APIs</span></span>](api-management-api-templates.md)  
    -   [<span data-ttu-id="382b9-109">API-lista</span><span class="sxs-lookup"><span data-stu-id="382b9-109">API list</span></span>](api-management-api-templates.md#APIList)  
    -   [<span data-ttu-id="382b9-110">Művelet</span><span class="sxs-lookup"><span data-stu-id="382b9-110">Operation</span></span>](api-management-api-templates.md#Product)  
    -   [<span data-ttu-id="382b9-111">Kódminták</span><span class="sxs-lookup"><span data-stu-id="382b9-111">Code samples</span></span>](api-management-api-templates.md#CodeSamples)  
        -   [<span data-ttu-id="382b9-112">Curl</span><span class="sxs-lookup"><span data-stu-id="382b9-112">Curl</span></span>](api-management-api-templates.md#Curl)  
        -   [<span data-ttu-id="382b9-113">C#</span><span class="sxs-lookup"><span data-stu-id="382b9-113">C#</span></span>](api-management-api-templates.md#CSharp)  
        -   [<span data-ttu-id="382b9-114">Java</span><span class="sxs-lookup"><span data-stu-id="382b9-114">Java</span></span>](api-management-api-templates.md#Stub)  
        -   [<span data-ttu-id="382b9-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="382b9-115">JavaScript</span></span>](api-management-api-templates.md#JavaScript)  
        -   [<span data-ttu-id="382b9-116">Az Objective C</span><span class="sxs-lookup"><span data-stu-id="382b9-116">Objective C</span></span>](api-management-api-templates.md#ObjectiveC)  
        -   [<span data-ttu-id="382b9-117">PHP</span><span class="sxs-lookup"><span data-stu-id="382b9-117">PHP</span></span>](api-management-api-templates.md#PHP)  
        -   [<span data-ttu-id="382b9-118">Python</span><span class="sxs-lookup"><span data-stu-id="382b9-118">Python</span></span>](api-management-api-templates.md#Python)  
        -   [<span data-ttu-id="382b9-119">Ruby</span><span class="sxs-lookup"><span data-stu-id="382b9-119">Ruby</span></span>](api-management-api-templates.md#Ruby)  
-   [<span data-ttu-id="382b9-120">Termékek</span><span class="sxs-lookup"><span data-stu-id="382b9-120">Products</span></span>](api-management-product-templates.md)  
    -   [<span data-ttu-id="382b9-121">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="382b9-121">Product list</span></span>](api-management-product-templates.md#ProductList)  
    -   [<span data-ttu-id="382b9-122">A termék</span><span class="sxs-lookup"><span data-stu-id="382b9-122">Product</span></span>](api-management-product-templates.md#Product)  
-   [<span data-ttu-id="382b9-123">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="382b9-123">Applications</span></span>](api-management-application-templates.md)  
    -   [<span data-ttu-id="382b9-124">– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="382b9-124">Application list</span></span>](api-management-application-templates.md#ProductList)  
    -   [<span data-ttu-id="382b9-125">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="382b9-125">Application</span></span>](api-management-application-templates.md#Application)  
-   [<span data-ttu-id="382b9-126">Hibák</span><span class="sxs-lookup"><span data-stu-id="382b9-126">Issues</span></span>](api-management-issue-templates.md)  
    -   [<span data-ttu-id="382b9-127">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="382b9-127">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
-   [<span data-ttu-id="382b9-128">Felhasználói profil</span><span class="sxs-lookup"><span data-stu-id="382b9-128">User Profile</span></span>](api-management-user-profile-templates.md)  
    -   [<span data-ttu-id="382b9-129">Profil</span><span class="sxs-lookup"><span data-stu-id="382b9-129">Profile</span></span>](api-management-user-profile-templates.md#Profile)  
    -   [<span data-ttu-id="382b9-130">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="382b9-130">Subscriptions</span></span>](api-management-user-profile-templates.md#Subscriptions)  
    -   [<span data-ttu-id="382b9-131">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="382b9-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
    -   [<span data-ttu-id="382b9-132">Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="382b9-132">Update account info</span></span>](api-management-user-profile-templates.md#UpdateAccountInfo)  
-   [<span data-ttu-id="382b9-133">Oldalak</span><span class="sxs-lookup"><span data-stu-id="382b9-133">Pages</span></span>](api-management-page-templates.md)  
    -   [<span data-ttu-id="382b9-134">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="382b9-134">Sign in</span></span>](api-management-page-templates.md#SignIn)  
    -   [<span data-ttu-id="382b9-135">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="382b9-135">Sign up</span></span>](api-management-page-templates.md#SignUp)  
    -   [<span data-ttu-id="382b9-136">A lap nem található</span><span class="sxs-lookup"><span data-stu-id="382b9-136">Page not found</span></span>](api-management-page-templates.md#PageNotFound)


## <a name="next-steps"></a><span data-ttu-id="382b9-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="382b9-137">Next steps</span></span>  
-   [<span data-ttu-id="382b9-138">Hivatkozása</span><span class="sxs-lookup"><span data-stu-id="382b9-138">Template reference</span></span>](api-management-developer-portal-templates-reference.md)  
-   [<span data-ttu-id="382b9-139">Adatmodell-referencia</span><span class="sxs-lookup"><span data-stu-id="382b9-139">Data model reference</span></span>](api-management-template-data-model-reference.md)  
-   [<span data-ttu-id="382b9-140">Oldalvezérlők</span><span class="sxs-lookup"><span data-stu-id="382b9-140">Page controls</span></span>](api-management-page-controls.md)  
-   [<span data-ttu-id="382b9-141">Sablonerőforrások</span><span class="sxs-lookup"><span data-stu-id="382b9-141">Template resources</span></span>](api-management-template-resources.md)