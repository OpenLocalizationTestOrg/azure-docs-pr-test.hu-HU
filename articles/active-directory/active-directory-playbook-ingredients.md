---
title: "Active Directory koncepció alkalmazástervezési összetevők aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a><span data-ttu-id="fa279-104">Az Azure Active Directory igazolása koncepció alkalmazástervezési összetevők</span><span class="sxs-lookup"><span data-stu-id="fa279-104">Azure Active Directory Proof of Concept Playbook Ingredients</span></span> 

## <a name="theme"></a><span data-ttu-id="fa279-105">Téma</span><span class="sxs-lookup"><span data-stu-id="fa279-105">Theme</span></span>
<span data-ttu-id="fa279-106">Az Azure AD lehetővé teszi az identitás- és hozzáférés megoldások hello vállalat több területet.</span><span class="sxs-lookup"><span data-stu-id="fa279-106">Azure AD provides identity and access solutions across multiple areas in hello enterprise.</span></span> <span data-ttu-id="fa279-107">A következő területeken hello hello forgatókönyvei azt besorolása:</span><span class="sxs-lookup"><span data-stu-id="fa279-107">We classify hello scenarios in hello following areas:</span></span> 

* [<span data-ttu-id="fa279-108">Nagy mennyiségű alkalmazásokat, egy identitás</span><span class="sxs-lookup"><span data-stu-id="fa279-108">Lots of apps, one identity</span></span>](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [<span data-ttu-id="fa279-109">A biztonság növelése</span><span class="sxs-lookup"><span data-stu-id="fa279-109">Increase your security</span></span>](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [<span data-ttu-id="fa279-110">Méretezéssel önkiszolgáló,</span><span class="sxs-lookup"><span data-stu-id="fa279-110">Scale with Self Service</span></span>](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

<span data-ttu-id="fa279-111">Határozza meg a téma tooframe hello koncepció toofocus hello erőfeszítéseket segít az üzleti céljaihoz mellőzése, amelyek gyakran hello eseményindítók a koncepció igazolása hello érdeklő hello első hely.</span><span class="sxs-lookup"><span data-stu-id="fa279-111">Defining a theme tooframe hello PoC helps toofocus hello efforts that resonates with business goals, which oftentimes are hello triggers of hello interest in a proof of concept in hello first place.</span></span> 

## <a name="environment"></a><span data-ttu-id="fa279-112">Környezet</span><span class="sxs-lookup"><span data-stu-id="fa279-112">Environment</span></span>

<span data-ttu-id="fa279-113">Fontos fontos toodetermine hello részletek hello környezet, amelyben hello koncepció fog továbbítani.</span><span class="sxs-lookup"><span data-stu-id="fa279-113">It is important toodetermine hello details of hello environment where you will deliver hello PoC.</span></span> <span data-ttu-id="fa279-114">Ideális esetben hozhat létre léphessen hello koncepció befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="fa279-114">Ideally you can build upon it after hello PoC is completed.</span></span> <span data-ttu-id="fa279-115">hello célkörnyezet elengedhetetlen, és keresse meg azt a valódi lehető és megkötések vagy további szempontok hello terhek közötti hello egyensúlyt.</span><span class="sxs-lookup"><span data-stu-id="fa279-115">hello target environment is crucial and you should find hello right balance between making it as real as possible and hello overhead of constraints or extra considerations.</span></span> <span data-ttu-id="fa279-116">tipikus környezeteket hello POC-khez használható a következők:</span><span class="sxs-lookup"><span data-stu-id="fa279-116">hello typical environments for PoCs are:</span></span>
* <span data-ttu-id="fa279-117">**Éles:** hello forgatókönyvek hajtják végre élő környezetében, és már központilag telepítve a Microsoft Cloud services (éles AD, Office 365, az Azure AD bérlő/SSO megoldás).</span><span class="sxs-lookup"><span data-stu-id="fa279-117">**Production:** hello scenarios will be implemented in your live environment and already deployed Microsoft Cloud services (production AD, Office 365, Azure AD tenant/SSO solution).</span></span> 
* <span data-ttu-id="fa279-118">**Felhasználói elfogadás tesztelése ellenőrzését / fejlesztői környezetben:** teszt infrastruktúrával rendelkezik (párhuzamos AD és az esetlegesen Azure AD bérlő/SSO megoldás) a Tesztadatok levő üzemi.</span><span class="sxs-lookup"><span data-stu-id="fa279-118">**User Acceptance Test (UAT)/Dev environment:** You have test infrastructure (parallel AD and potentially Azure AD tenant/SSO solution) with test data that resembles production.</span></span> <span data-ttu-id="fa279-119">Általában hello tesztkörnyezetben által megosztott több projektet hello vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="fa279-119">Typically, hello test environment is shared across multiple projects in hello enterprise.</span></span>

<span data-ttu-id="fa279-120">Az útmutatóban a legtöbb forgatókönyv additívak jellegűek.</span><span class="sxs-lookup"><span data-stu-id="fa279-120">Most scenarios in this guide are additive in nature.</span></span> <span data-ttu-id="fa279-121">Ennek eredményeképpen ezek telepíthető hello éles bérlői hello koncepció kívüli felhasználók befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="fa279-121">As a result, they can be deployed in hello production tenant without affecting users outside hello PoC.</span></span> <span data-ttu-id="fa279-122">Ez a dokumentum azt fogja kell létesítő változatok hatása lenne bérlői szintű.</span><span class="sxs-lookup"><span data-stu-id="fa279-122">Throughout this document, we will be calling out which scenarios would have tenant-wide effect.</span></span> <span data-ttu-id="fa279-123">Ezekben az esetekben érdemes lehet tooconsider nem éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="fa279-123">In those cases, you might want tooconsider a non-production environment.</span></span> 


## <a name="target-users"></a><span data-ttu-id="fa279-124">Azon felhasználók megcélzása</span><span class="sxs-lookup"><span data-stu-id="fa279-124">Target Users</span></span>

<span data-ttu-id="fa279-125">Fontos toodetermine hello célkészlet, amely intéz a hello forgatókönyvek, különösen ha hello környezet üzemi és tesztkörnyezetben is.</span><span class="sxs-lookup"><span data-stu-id="fa279-125">It is important toodetermine hello target set of users that will exercise hello scenarios, especially when hello environment is production or test.</span></span> <span data-ttu-id="fa279-126">a koncepció azon felhasználók megcélzása hello kategóriái a következők:</span><span class="sxs-lookup"><span data-stu-id="fa279-126">hello categories of target users for PoC are:</span></span>
* <span data-ttu-id="fa279-127">**Próbafelhasználók:** ellátására használnak, a nap tooday hello fiókkal hello megoldással hello környezetben valós felhasználói feladat-funkciók</span><span class="sxs-lookup"><span data-stu-id="fa279-127">**Pilot Users:** Real users in hello environment that will be using hello solution with hello account they use for their day tooday job functions</span></span>
* <span data-ttu-id="fa279-128">**Tesztfelhasználók:** hello környezetben létrehozott fiókot tesztelése</span><span class="sxs-lookup"><span data-stu-id="fa279-128">**Test Users:** Test accounts created in hello environment</span></span> 

<span data-ttu-id="fa279-129">Ez az útmutató a legtöbb esetben kísérleti felhasználók lehet érvényesíteni.</span><span class="sxs-lookup"><span data-stu-id="fa279-129">Most scenarios in this guide can be exercised by pilot users.</span></span> <span data-ttu-id="fa279-130">Ez a dokumentum azt fogja kell létesítő cél felhasználói kapcsolatos megfontolások szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="fa279-130">Throughout this document, we will be calling out target user considerations if needed.</span></span>


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]