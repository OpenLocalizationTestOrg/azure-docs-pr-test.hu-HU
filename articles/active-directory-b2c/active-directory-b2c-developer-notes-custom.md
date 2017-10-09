---
title: "Az Azure Active Directory B2C: Megjegyzések egyéni házirendek segítségével a fejlesztők számára |} Microsoft Docs"
description: "Megjegyzések fejlesztők konfigurálása és karbantartása az Azure AD B2C egyéni házirendekkel"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="6f2bd-103">Az Azure Active Directory B2C egyéni házirend nyilvános előzetes kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="6f2bd-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="6f2bd-104">hello egyéni házirend szolgáltatáskészlet már elérhető az összes Azure Active Directory B2C előzetes verzióját nyilvános értékelésre (az Azure AD B2C) ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-104">hello custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="6f2bd-105">E szolgáltatáskészlet hello legösszetettebb identitáskezelési megoldások kialakításának speciális identitás fejlesztők irányul.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-105">This feature set is targeted at advanced identity developers building hello most complex identity solutions.</span></span>  

<span data-ttu-id="6f2bd-106">E szolgáltatáskészlet jelenleg fejlesztők tooconfigure hello identitás élmény keretrendszer keresztül közvetlenül XML-fájl szerkesztésével van szükség.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-106">Today, this feature set requires developers tooconfigure hello Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="6f2bd-107">A konfigurációs módszer hatékony és összetett.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="6f2bd-108">Identitás fejlesztők hello használata speciális identitás élmény keretrendszer kell terveznie a tooinvest útmutatók befejezése és a hivatkozás dokumentumok olvasási időt.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-108">Advanced identity developers using hello Identity Experience Framework should plan tooinvest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="6f2bd-109">A nyilvános előzetes szereplő szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6f2bd-109">Features included in this public preview</span></span>
<span data-ttu-id="6f2bd-110">Hello új szolgáltatásainak hello nyilvános előzetes verziójában bevezetett a fejlesztők hello a következő feladatokat hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="6f2bd-110">With hello new features introduced in hello public preview, developers can perform hello following tasks:</span></span><br>

* <span data-ttu-id="6f2bd-111">Szerző és feltöltése az egyéni hitelesítési felhasználói utak egyéni házirendekkel.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="6f2bd-112">Felhasználói utak cseréjét, részletes Jogcímszolgáltatók közötti ismertetik.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="6f2bd-113">Határozza meg azt a felhasználói utak feltételes ugorhat.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="6f2bd-114">Az egyéni hitelesítési felhasználói utak szolgáltatások REST API-t integrálja.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="6f2bd-115">Adja hozzá az Identitásszolgáltatók, amelyek megfelelnek a szabványos OpenIDConnect hello összevonásának.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-115">Add federation with identity providers that are compliant with hello OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="6f2bd-116">Adja hozzá az összevonás az identitás-szolgáltatóktól toohello SAML 2.0 protokoll igazodik.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-116">Add federation with identity providers that adhere toohello SAML 2.0 protocol.</span></span> 

## <a name="terms-of-hello-public-preview"></a><span data-ttu-id="6f2bd-117">Hello nyilvános előzetes feltételeit.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-117">Terms of hello public preview</span></span>

* <span data-ttu-id="6f2bd-118">Javasoljuk, toouse hello új funkciók csak tesztelési célokra.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-118">We encourage you toouse hello new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="6f2bd-119">Az új szolgáltatások nem feltétlenül egy éles környezetben való használathoz.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="6f2bd-120">Szolgáltatásszint-szerződések (SLA) csak akkor érvényesíthetők toohello új funkciókat.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-120">Service level agreements (SLAs) do not apply toohello new features.</span></span> <br>
* <span data-ttu-id="6f2bd-121">Támogatási kérelmek nyilvántartásához rendszeres támogatási csatornákon keresztül.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="6f2bd-122">Nincs megadva általánosan rendelkezésre álló Ígért dátum.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="6f2bd-123">Saját belátása szerint, és bármilyen okból Microsoft is jelzőt és elutasítása vagy forgatókönyvek és a felhasználó útvonal be, amelyek mérete meghaladja a hello Azure AD B2C termék okleveles tooserve platformként felhasználói identitások és hozzáférések kezelése (CIAM) hello hatókörének korlátozása.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed hello scope of hello Azure AD B2C product charter tooserve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="6f2bd-124">Egyéni házirend szolgáltatáskészlet fejlesztők feladatai</span><span class="sxs-lookup"><span data-stu-id="6f2bd-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="6f2bd-125">Manuális konfigurálása az alapul szolgáló platform az Azure AD B2C alacsonyabb szintű hozzáférés toohello biztosít, és egyedi, teljes mértékben testreszabható megbízhatósági keretrendszer hello létrehozását eredményezi.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-125">Manual policy configuration grants lower-level access toohello underlying platform of Azure AD B2C and results in hello creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="6f2bd-126">Ezek lehetséges kombinációinak egyéni Identitásszolgáltatók, megbízhatósági kapcsolatokat, külső szolgáltatásokkal, és lépésről lépésre munkafolyamatok Integrációk fel őket a fejlesztők speciális hello nagyobb iránti igények kielégítése érdekében helyezze el.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on hello advanced developers consuming them.</span></span>

<span data-ttu-id="6f2bd-127">toofully juttatás hello nyilvános Preview, javasoljuk, hogy a fejlesztők hello egyéni házirend szolgáltatáskészlet fel igazodik a következő irányelveket toohello:</span><span class="sxs-lookup"><span data-stu-id="6f2bd-127">toofully benefit from hello public preview, we suggest that developers consuming hello custom policy feature set adhere toohello following guidelines:</span></span>
* <span data-ttu-id="6f2bd-128">Ismerkedjen meg hello konfigurációs nyelvét hello identitás élmény motor és a kulcs vagy titkos kulcsok kezelése.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-128">Become familiar with hello configuration language of hello Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="6f2bd-129">Saját tulajdonba forgatókönyveket és a használatot.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="6f2bd-130">A forgatókönyv módszeres tesztek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="6f2bd-131">Hajtsa végre a szoftverek fejlesztési és átmeneti legalább egy fejlesztési és tesztelési környezetben az ajánlott eljárásokról és egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="6f2bd-132">Maradjon naprakész az identitás-szolgáltatóktól hello és integrálja szolgáltatások új fejlesztéseiről.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-132">Stay informed about new developments from hello identity providers and services you integrate with.</span></span> <span data-ttu-id="6f2bd-133">Például nyomon követheti, változások a titkos kulcsok és ütemezett és nem tervezett módosítások toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes toohello service.</span></span>
* <span data-ttu-id="6f2bd-134">Aktív figyelés beállítása, és figyelje a hello válaszképességének éles környezetekben.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-134">Set up active monitoring, and monitor hello responsiveness of production environments.</span></span>
* <span data-ttu-id="6f2bd-135">Naprakészen tartható kapcsolattartási e-mail címet, és rugalmas toohello Microsoft live hely team e-mailek maradnak.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-135">Keep contact email addresses current, and stay responsive toohello Microsoft live-site team emails.</span></span>
* <span data-ttu-id="6f2bd-136">Intézkedéseket időben elkeverni toodo módon hello Microsoft live hely csoportja.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-136">Take timely action when advised toodo so by hello Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="6f2bd-137">Ezek a funkciók végül az Azure AD beépített házirendek minősítené akadálymentesített tooall fejlesztők tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="6f2bd-137">These features might eventually be included in Azure AD built-in policies, making them more accessible tooall developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f2bd-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f2bd-138">Next steps</span></span>
<span data-ttu-id="6f2bd-139">[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="6f2bd-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>
