---
title: "Active Directory koncepció forgatókönyv bevezető aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="352a1-104">Az Azure Active Directory igazolása koncepció forgatókönyv: bemutatása</span><span class="sxs-lookup"><span data-stu-id="352a1-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="352a1-105">Ebben a cikkben talál útmutatást a koncepció igazolása (PoC) a tooexplore másik Azure AD lehetővé tevő szolgáltatásaival.</span><span class="sxs-lookup"><span data-stu-id="352a1-105">This article provides guidelines tooexplore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="352a1-106">hello célja, hogy a dokumentum az identitás mérnökök, informatikai szakemberek és rendszerintegrátorok</span><span class="sxs-lookup"><span data-stu-id="352a1-106">hello intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-toouse-this-playbook"></a><span data-ttu-id="352a1-107">Hogyan toouse Ez a forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="352a1-107">How toouse this Playbook</span></span>

1. <span data-ttu-id="352a1-108">Használjon hello [téma](active-directory-playbook-ingredients.md#theme) szakaszt, és válassza ki az igényeinek megfelelően érdeklő hello megfelelőnek.</span><span class="sxs-lookup"><span data-stu-id="352a1-108">Use hello [Theme](active-directory-playbook-ingredients.md#theme) section and pick hello area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="352a1-109">Hatókör hello koncepció megfelel-e az üzleti céljaihoz hello forgatókönyvek kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="352a1-109">Scope hello PoC by choosing hello scenarios that align with your business goals.</span></span> <span data-ttu-id="352a1-110">hello rövidebb hello jobb.</span><span class="sxs-lookup"><span data-stu-id="352a1-110">hello shorter hello better.</span></span> <span data-ttu-id="352a1-111">Azt javasoljuk, mint a rövid és tömör lehetséges tooconvey hello értékként toohello érdekelt felek, ugyanakkor minimalizálja a hello összetettsége toorealize azt.</span><span class="sxs-lookup"><span data-stu-id="352a1-111">We recommend doing it as short and concise as possible tooconvey hello value toohello stakeholders while minimizing hello complexity toorealize it.</span></span>  
3. <span data-ttu-id="352a1-112">Használjon hello [megvalósítási](active-directory-playbook-implementation.md) szakasz toounderstand hello forgatókönyvek, és szeretné értelmezéséről alkalmaz környezetében.</span><span class="sxs-lookup"><span data-stu-id="352a1-112">Use hello [Implementation](active-directory-playbook-implementation.md) section toounderstand hello scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="352a1-113">Minden esetben azt írják le hogyan tooset azt fel (úgynevezett [építőelemeket](active-directory-playbook-building-blocks.md)), és hogyan toonavigate hello forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="352a1-113">In each scenario, we describe how tooset it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how toonavigate hello scenarios.</span></span> 
4. <span data-ttu-id="352a1-114">Minden egyes építőelem a szükséges előfeltételek hello szükséges, valamint az megközelítőleges időpont, amikor toocomplete ismerteti.</span><span class="sxs-lookup"><span data-stu-id="352a1-114">Each building block explains hello pre-requisites needed, as well as an approximate time toocomplete.</span></span> <span data-ttu-id="352a1-115">Ez segítséget nyújthat hello tervezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="352a1-115">This can help you during hello planning process.</span></span> 
5. <span data-ttu-id="352a1-116">Az 1-3 alapján, adja meg a hello [környezet](active-directory-playbook-ingredients.md#environment) mely tooexecute a.</span><span class="sxs-lookup"><span data-stu-id="352a1-116">Based on 1-3 Above, define hello [Environment](active-directory-playbook-ingredients.md#environment) in which tooexecute.</span></span> <span data-ttu-id="352a1-117">A termelési környezetben tooget hello felhasználói élmény, a helyes működését a toostrive javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="352a1-117">We encourage toostrive for a production environment tooget a good feel of hello experience for your users.</span></span> 
6. <span data-ttu-id="352a1-118">Ha ütköző követelményeket támasztó, használja a hasznos kompromisszumot mátrix</span><span class="sxs-lookup"><span data-stu-id="352a1-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="352a1-119">Az érték téma-központú megjelenítése</span><span class="sxs-lookup"><span data-stu-id="352a1-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="352a1-120">Simaság tooprepare tooset fel és tooexecute hello forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="352a1-120">Smoothness tooprepare, tooset up, and tooexecute hello scenarios</span></span> 
   * <span data-ttu-id="352a1-121">Minimális idő tooexecute hello cél forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="352a1-121">Minimal time tooexecute hello target scenarios</span></span> 
   * <span data-ttu-id="352a1-122">Bezárás tooproduction a megkötések megvalósítható, mint</span><span class="sxs-lookup"><span data-stu-id="352a1-122">As close tooproduction as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="352a1-123">Ez a cikk teljes jelenik meg néhány adott harmadik féltől származó alkalmazások és a termékek kényelmi célokat szolgál példaként említett.</span><span class="sxs-lookup"><span data-stu-id="352a1-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="352a1-124">Az Azure AD-alkalmazások ezer támogatja a [alkalmazáskatalógusában](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) használható az Ön igényeinek és a környezet alapján.</span><span class="sxs-lookup"><span data-stu-id="352a1-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]