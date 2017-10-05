---
title: "Az Azure Active Directory koncepció alkalmazástervezési bemutatása |} Microsoft Docs"
description: "Vizsgálatát, és gyorsan végrehajthatja az identitás- és kezelési helyzetek"
services: active-directory
keywords: "az Azure active directory, a forgatókönyv, a koncepció igazolása, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 567f3373594bc53435e8c0bd0a3445dd318af1cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="a8ea6-104">Az Azure Active Directory igazolása koncepció forgatókönyv: bemutatása</span><span class="sxs-lookup"><span data-stu-id="a8ea6-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="a8ea6-105">Ez a cikk útmutatásokat felfedezése, mely másik Azure AD-lehetőségek a egy a koncepció igazolása (PoC).</span><span class="sxs-lookup"><span data-stu-id="a8ea6-105">This article provides guidelines to explore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="a8ea6-106">A jelen dokumentum a célközönség identitás mérnökök, informatikai szakemberek és rendszerintegrátorok</span><span class="sxs-lookup"><span data-stu-id="a8ea6-106">The intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-to-use-this-playbook"></a><span data-ttu-id="a8ea6-107">Ez a forgatókönyv használata</span><span class="sxs-lookup"><span data-stu-id="a8ea6-107">How to use this Playbook</span></span>

1. <span data-ttu-id="a8ea6-108">Használja a [téma](active-directory-playbook-ingredients.md#theme) szakaszt, és válassza ki az egyik fontos igényei szerint megfelelőnek.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-108">Use the [Theme](active-directory-playbook-ingredients.md#theme) section and pick the area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="a8ea6-109">A koncepció hatókör megfelel-e az üzleti céljaihoz forgatókönyvek kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-109">Scope the PoC by choosing the scenarios that align with your business goals.</span></span> <span data-ttu-id="a8ea6-110">Minél rövidebb minél pontosabban.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-110">The shorter the better.</span></span> <span data-ttu-id="a8ea6-111">Javasolt azt rövid és pontos sikerkritériumokkal biztosíthatja a lehető átadja értékét az érdekelt felek, ugyanakkor minimalizálja a erősségét róla.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-111">We recommend doing it as short and concise as possible to convey the value to the stakeholders while minimizing the complexity to realize it.</span></span>  
3. <span data-ttu-id="a8ea6-112">Használja a [megvalósítási](active-directory-playbook-implementation.md) szakaszt, a forgatókönyveket, és mi azok jelentené alkalmaz környezetében.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-112">Use the [Implementation](active-directory-playbook-implementation.md) section to understand the scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="a8ea6-113">Minden esetben azt mutatják be, hogyan állítsa be (úgynevezett [építőelemeket](active-directory-playbook-building-blocks.md)), és keresse meg a forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-113">In each scenario, we describe how to set it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how to navigate the scenarios.</span></span> 
4. <span data-ttu-id="a8ea6-114">Minden egyes építőelem ismerteti a szükséges előfeltételt, valamint egy hozzávetőleges időt igényel.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-114">Each building block explains the pre-requisites needed, as well as an approximate time to complete.</span></span> <span data-ttu-id="a8ea6-115">Ez segítséget nyújthat a tervezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-115">This can help you during the planning process.</span></span> 
5. <span data-ttu-id="a8ea6-116">Adja meg az 1-3 alapján, a [környezet](active-directory-playbook-ingredients.md#environment) használandó hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-116">Based on 1-3 Above, define the [Environment](active-directory-playbook-ingredients.md#environment) in which to execute.</span></span> <span data-ttu-id="a8ea6-117">Ezért törekedni éles környezetbe történő beszerzéséhez a felhasználói élmény, a helyes működését a felhasználók javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-117">We encourage to strive for a production environment to get a good feel of the experience for your users.</span></span> 
6. <span data-ttu-id="a8ea6-118">Ha ütköző követelményeket támasztó, használja a hasznos kompromisszumot mátrix</span><span class="sxs-lookup"><span data-stu-id="a8ea6-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="a8ea6-119">Az érték téma-központú megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a8ea6-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="a8ea6-120">Simaság előkészítése, beállításához és a forgatókönyv végrehajtásához</span><span class="sxs-lookup"><span data-stu-id="a8ea6-120">Smoothness to prepare, to set up, and to execute the scenarios</span></span> 
   * <span data-ttu-id="a8ea6-121">A cél forgatókönyvek végrehajtási minimális időtartama</span><span class="sxs-lookup"><span data-stu-id="a8ea6-121">Minimal time to execute the target scenarios</span></span> 
   * <span data-ttu-id="a8ea6-122">Mivel hamarosan üzemi környezetben, a korlátozások megvalósítható</span><span class="sxs-lookup"><span data-stu-id="a8ea6-122">As close to production as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="a8ea6-123">Ez a cikk teljes jelenik meg néhány adott harmadik féltől származó alkalmazások és a termékek kényelmi célokat szolgál példaként említett.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="a8ea6-124">Az Azure AD-alkalmazások ezer támogatja a [alkalmazáskatalógusában](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) használható az Ön igényeinek és a környezet alapján.</span><span class="sxs-lookup"><span data-stu-id="a8ea6-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]