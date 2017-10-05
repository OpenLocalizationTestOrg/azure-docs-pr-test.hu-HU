---
title: "Az Azure Active Directory koncepció alkalmazástervezési összetevők |} Microsoft Docs"
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
ms.openlocfilehash: d2a0fe280f143d390f5e4ba40e0ebe92d8a4a837
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="faefb-104">Az Azure Active Directory igazolása koncepció alkalmazástervezési összetevők</span><span class="sxs-lookup"><span data-stu-id="faefb-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="faefb-105">Téma</span><span class="sxs-lookup"><span data-stu-id="faefb-105">Theme</span></span>
<span data-ttu-id="faefb-106">Az Azure AD lehetővé teszi az identitás- és hozzáférés megoldások a vállalat több területet.</span><span class="sxs-lookup"><span data-stu-id="faefb-106">Azure AD provides identity and access solutions across multiple areas in the enterprise.</span></span> <span data-ttu-id="faefb-107">A Microsoft besorolni a forgatókönyvek a következő területeken:</span><span class="sxs-lookup"><span data-stu-id="faefb-107">We classify the scenarios in the following areas:</span></span> 

* [<span data-ttu-id="faefb-108">Nagy mennyiségű alkalmazásokat, egy identitás</span><span class="sxs-lookup"><span data-stu-id="faefb-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="faefb-109">A biztonság növelése</span><span class="sxs-lookup"><span data-stu-id="faefb-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="faefb-110">Méretezéssel önkiszolgáló,</span><span class="sxs-lookup"><span data-stu-id="faefb-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="faefb-111">A téma a koncepció segítségével a figyelmet a próbálkozások keret mellőzése a vállalati célok meghatározása, gyakran ezek az eseményindítók a koncepció igazolása érdeklő először.</span><span class="sxs-lookup"><span data-stu-id="faefb-111">Defining a theme to frame the PoC helps to focus the efforts that resonates with business goals, which oftentimes are the triggers of the interest in a proof of concept in the first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="faefb-112">Környezet</span><span class="sxs-lookup"><span data-stu-id="faefb-112">Environment</span></span>

<span data-ttu-id="faefb-113">Fontos meghatározni a környezetben, ahol a koncepció fog továbbítani részleteit.</span><span class="sxs-lookup"><span data-stu-id="faefb-113">It is important to determine the details of the environment where you will deliver the PoC.</span></span> <span data-ttu-id="faefb-114">Ideális esetben hozhat létre rá a koncepció befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="faefb-114">Ideally you can build upon it after the PoC is completed.</span></span> <span data-ttu-id="faefb-115">A célkörnyezet elengedhetetlen, és meg kell találniuk az egyensúlyt azt a valódi lehető és növeli a megkötések vagy további szempontok közé.</span><span class="sxs-lookup"><span data-stu-id="faefb-115">The target environment is crucial and you should find the right balance between making it as real as possible and the overhead of constraints or extra considerations.</span></span> <span data-ttu-id="faefb-116">A tipikus környezeteket POC-khez használható a következők:</span><span class="sxs-lookup"><span data-stu-id="faefb-116">The typical environments for PoCs are:</span></span>
* <span data-ttu-id="faefb-117">**Éles:** a forgatókönyvek hajtják végre élő környezetében, és már központilag telepítve a Microsoft Cloud services (éles AD, Office 365, az Azure AD bérlő/SSO megoldás).</span><span class="sxs-lookup"><span data-stu-id="faefb-117">**Production:** The scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="faefb-118">**Felhasználói elfogadás tesztelése ellenőrzését / fejlesztői környezetben:** teszt infrastruktúrával rendelkezik (párhuzamos AD és az esetlegesen Azure AD bérlő/SSO megoldás) a Tesztadatok levő üzemi.</span><span class="sxs-lookup"><span data-stu-id="faefb-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="faefb-119">Általában a tesztkörnyezet által megosztott több projektet a vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="faefb-119">Typically, the test environment is shared across multiple projects in the enterprise.</span></span>

<span data-ttu-id="faefb-120">Az útmutatóban a legtöbb forgatókönyv additívak jellegűek.</span><span class="sxs-lookup"><span data-stu-id="faefb-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="faefb-121">Ennek eredményeképpen ezek is telepíthető a termelési bérlő anélkül, hogy befolyásolná a koncepció kívüli felhasználók.</span><span class="sxs-lookup"><span data-stu-id="faefb-121">As a result, they can be deployed in the production tenant without affecting users outside the PoC.</span></span> <span data-ttu-id="faefb-122">Ez a dokumentum azt fogja kell létesítő változatok hatása lenne bérlői szintű.</span><span class="sxs-lookup"><span data-stu-id="faefb-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="faefb-123">Ebben az esetben érdemes figyelembe kell venni egy nem éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="faefb-123">In those cases, you might want to consider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="faefb-124">Azon felhasználók megcélzása</span><span class="sxs-lookup"><span data-stu-id="faefb-124">Target Users</span></span>

<span data-ttu-id="faefb-125">Fontos a cél meg, amely a forgatókönyvek intéz, különösen akkor, ha a környezet nem üzemi és tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="faefb-125">It is important to determine the target set of users that will exercise the scenarios, especially when the environment is production or test.</span></span> <span data-ttu-id="faefb-126">A koncepció azon felhasználók megcélzása kategóriái a következők:</span><span class="sxs-lookup"><span data-stu-id="faefb-126">The categories of target users for PoC are:</span></span>
* <span data-ttu-id="faefb-127">**Próbafelhasználók:** valódi felhasználók által használt a megoldás a napi munkakörök használ a fiókhoz a környezetben</span><span class="sxs-lookup"><span data-stu-id="faefb-127">**Pilot Users:** Real users in the environment that will be using the solution with the account they use for their day to day job functions</span></span>
* <span data-ttu-id="faefb-128">**Tesztfelhasználók:** a környezetben létrehozott fiókot tesztelése</span><span class="sxs-lookup"><span data-stu-id="faefb-128">**Test Users:** Test accounts created in the environment</span></span> 

<span data-ttu-id="faefb-129">Ez az útmutató a legtöbb esetben kísérleti felhasználók lehet érvényesíteni.</span><span class="sxs-lookup"><span data-stu-id="faefb-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="faefb-130">Ez a dokumentum azt fogja kell létesítő cél felhasználói kapcsolatos megfontolások szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="faefb-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]