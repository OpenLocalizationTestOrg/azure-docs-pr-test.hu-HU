---
title: "Az Azure API Management fejlesztői portál sablonok |} Microsoft Docs"
description: "Ismerje meg, hogyan szabhatja testre a sablonok használatával az Azure API Management portál lapjai fejlesztői tartalmát."
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
ms.openlocfilehash: dc3f32a3cff2e66a798bd8f6c19c6b56a47643ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-api-management-developer-portal-templates"></a><span data-ttu-id="288a0-103">Az Azure API Management fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="288a0-103">Azure API Management Developer Portal Templates</span></span>
<span data-ttu-id="288a0-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="288a0-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="288a0-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="288a0-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="288a0-106">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="288a0-106">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  



  
##  <span data-ttu-id="288a0-107"><a name="DeveloperPortalTemplates"></a>Fejlesztői portál sablonok</span><span class="sxs-lookup"><span data-stu-id="288a0-107"><a name="DeveloperPortalTemplates"></a> Developer portal templates</span></span>  
  
-   [<span data-ttu-id="288a0-108">API-k</span><span class="sxs-lookup"><span data-stu-id="288a0-108">APIs</span></span>](api-management-api-templates.md)  
    -   [<span data-ttu-id="288a0-109">API-lista</span><span class="sxs-lookup"><span data-stu-id="288a0-109">API list</span></span>](api-management-api-templates.md#APIList)  
    -   [<span data-ttu-id="288a0-110">Művelet</span><span class="sxs-lookup"><span data-stu-id="288a0-110">Operation</span></span>](api-management-api-templates.md#Product)  
    -   [<span data-ttu-id="288a0-111">Kódminták</span><span class="sxs-lookup"><span data-stu-id="288a0-111">Code samples</span></span>](api-management-api-templates.md#CodeSamples)  
        -   [<span data-ttu-id="288a0-112">Curl</span><span class="sxs-lookup"><span data-stu-id="288a0-112">Curl</span></span>](api-management-api-templates.md#Curl)  
        -   [<span data-ttu-id="288a0-113">C#</span><span class="sxs-lookup"><span data-stu-id="288a0-113">C#</span></span>](api-management-api-templates.md#CSharp)  
        -   [<span data-ttu-id="288a0-114">Java</span><span class="sxs-lookup"><span data-stu-id="288a0-114">Java</span></span>](api-management-api-templates.md#Stub)  
        -   [<span data-ttu-id="288a0-115">JavaScript</span><span class="sxs-lookup"><span data-stu-id="288a0-115">JavaScript</span></span>](api-management-api-templates.md#JavaScript)  
        -   [<span data-ttu-id="288a0-116">Az Objective C</span><span class="sxs-lookup"><span data-stu-id="288a0-116">Objective C</span></span>](api-management-api-templates.md#ObjectiveC)  
        -   [<span data-ttu-id="288a0-117">PHP</span><span class="sxs-lookup"><span data-stu-id="288a0-117">PHP</span></span>](api-management-api-templates.md#PHP)  
        -   [<span data-ttu-id="288a0-118">Python</span><span class="sxs-lookup"><span data-stu-id="288a0-118">Python</span></span>](api-management-api-templates.md#Python)  
        -   [<span data-ttu-id="288a0-119">Ruby</span><span class="sxs-lookup"><span data-stu-id="288a0-119">Ruby</span></span>](api-management-api-templates.md#Ruby)  
-   [<span data-ttu-id="288a0-120">Termékek</span><span class="sxs-lookup"><span data-stu-id="288a0-120">Products</span></span>](api-management-product-templates.md)  
    -   [<span data-ttu-id="288a0-121">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="288a0-121">Product list</span></span>](api-management-product-templates.md#ProductList)  
    -   [<span data-ttu-id="288a0-122">A termék</span><span class="sxs-lookup"><span data-stu-id="288a0-122">Product</span></span>](api-management-product-templates.md#Product)  
-   [<span data-ttu-id="288a0-123">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="288a0-123">Applications</span></span>](api-management-application-templates.md)  
    -   [<span data-ttu-id="288a0-124">– Alkalmazáslista</span><span class="sxs-lookup"><span data-stu-id="288a0-124">Application list</span></span>](api-management-application-templates.md#ProductList)  
    -   [<span data-ttu-id="288a0-125">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="288a0-125">Application</span></span>](api-management-application-templates.md#Application)  
-   [<span data-ttu-id="288a0-126">Hibák</span><span class="sxs-lookup"><span data-stu-id="288a0-126">Issues</span></span>](api-management-issue-templates.md)  
    -   [<span data-ttu-id="288a0-127">Problémák listája</span><span class="sxs-lookup"><span data-stu-id="288a0-127">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
-   [<span data-ttu-id="288a0-128">Felhasználói profil</span><span class="sxs-lookup"><span data-stu-id="288a0-128">User Profile</span></span>](api-management-user-profile-templates.md)  
    -   [<span data-ttu-id="288a0-129">Profil</span><span class="sxs-lookup"><span data-stu-id="288a0-129">Profile</span></span>](api-management-user-profile-templates.md#Profile)  
    -   [<span data-ttu-id="288a0-130">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="288a0-130">Subscriptions</span></span>](api-management-user-profile-templates.md#Subscriptions)  
    -   [<span data-ttu-id="288a0-131">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="288a0-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
    -   [<span data-ttu-id="288a0-132">Fiók adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="288a0-132">Update account info</span></span>](api-management-user-profile-templates.md#UpdateAccountInfo)  
-   [<span data-ttu-id="288a0-133">Oldalak</span><span class="sxs-lookup"><span data-stu-id="288a0-133">Pages</span></span>](api-management-page-templates.md)  
    -   [<span data-ttu-id="288a0-134">bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="288a0-134">Sign in</span></span>](api-management-page-templates.md#SignIn)  
    -   [<span data-ttu-id="288a0-135">feliratkozni</span><span class="sxs-lookup"><span data-stu-id="288a0-135">Sign up</span></span>](api-management-page-templates.md#SignUp)  
    -   [<span data-ttu-id="288a0-136">A lap nem található</span><span class="sxs-lookup"><span data-stu-id="288a0-136">Page not found</span></span>](api-management-page-templates.md#PageNotFound)


## <a name="next-steps"></a><span data-ttu-id="288a0-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="288a0-137">Next steps</span></span>  
-   [<span data-ttu-id="288a0-138">Hivatkozása</span><span class="sxs-lookup"><span data-stu-id="288a0-138">Template reference</span></span>](api-management-developer-portal-templates-reference.md)  
-   [<span data-ttu-id="288a0-139">Adatmodell-referencia</span><span class="sxs-lookup"><span data-stu-id="288a0-139">Data model reference</span></span>](api-management-template-data-model-reference.md)  
-   [<span data-ttu-id="288a0-140">Oldalvezérlők</span><span class="sxs-lookup"><span data-stu-id="288a0-140">Page controls</span></span>](api-management-page-controls.md)  
-   [<span data-ttu-id="288a0-141">Sablonerőforrások</span><span class="sxs-lookup"><span data-stu-id="288a0-141">Template resources</span></span>](api-management-template-resources.md)