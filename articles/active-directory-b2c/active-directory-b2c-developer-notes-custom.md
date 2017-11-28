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
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a><span data-ttu-id="e77ad-103">Az Azure Active Directory B2C egyéni házirend nyilvános előzetes kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="e77ad-103">Release notes for Azure Active Directory B2C custom policy public preview</span></span>
<span data-ttu-id="e77ad-104">Az egyéni házirend szolgáltatáskészlet már elérhető az összes Azure Active Directory B2C előzetes verzióját nyilvános értékelésre (az Azure AD B2C) ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="e77ad-104">The custom policy feature set is now available for evaluation under public preview for all Azure Active Directory B2C (Azure AD B2C) customers.</span></span> <span data-ttu-id="e77ad-105">E szolgáltatáskészlet speciális identitás fejlesztők a legösszetettebb identitáskezelési megoldások kialakításának irányul.</span><span class="sxs-lookup"><span data-stu-id="e77ad-105">This feature set is targeted at advanced identity developers building the most complex identity solutions.</span></span>  

<span data-ttu-id="e77ad-106">E szolgáltatáskészlet jelenleg a identitás élmény keretrendszer keresztül közvetlenül XML-fájl szerkesztésével konfigurálhatja a fejlesztők van szükség.</span><span class="sxs-lookup"><span data-stu-id="e77ad-106">Today, this feature set requires developers to configure the Identity Experience Framework directly via XML file editing.</span></span> <span data-ttu-id="e77ad-107">A konfigurációs módszer hatékony és összetett.</span><span class="sxs-lookup"><span data-stu-id="e77ad-107">This method of configuration is powerful and complex.</span></span> <span data-ttu-id="e77ad-108">Speciális identitás identitás élmény keretrendszerrel fejlesztők kell terveznie a fektetnek útmutatók befejezése és a hivatkozás dokumentumok olvasási időt.</span><span class="sxs-lookup"><span data-stu-id="e77ad-108">Advanced identity developers using the Identity Experience Framework should plan to invest some time completing walk-throughs and reading reference documents.</span></span> 

## <a name="features-included-in-this-public-preview"></a><span data-ttu-id="e77ad-109">A nyilvános előzetes szereplő szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e77ad-109">Features included in this public preview</span></span>
<span data-ttu-id="e77ad-110">A nyilvános előzetes verziójában bevezetett új funkciókkal fejlesztők számára a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="e77ad-110">With the new features introduced in the public preview, developers can perform the following tasks:</span></span><br>

* <span data-ttu-id="e77ad-111">Szerző és feltöltése az egyéni hitelesítési felhasználói utak egyéni házirendekkel.</span><span class="sxs-lookup"><span data-stu-id="e77ad-111">Author and upload custom authentication user journeys by using custom policies.</span></span> 
   * <span data-ttu-id="e77ad-112">Felhasználói utak cseréjét, részletes Jogcímszolgáltatók közötti ismertetik.</span><span class="sxs-lookup"><span data-stu-id="e77ad-112">Describe user journeys step-by-step as exchanges between claims providers.</span></span> 
   * <span data-ttu-id="e77ad-113">Határozza meg azt a felhasználói utak feltételes ugorhat.</span><span class="sxs-lookup"><span data-stu-id="e77ad-113">Define conditional branching in user journeys.</span></span> 
* <span data-ttu-id="e77ad-114">Az egyéni hitelesítési felhasználói utak szolgáltatások REST API-t integrálja.</span><span class="sxs-lookup"><span data-stu-id="e77ad-114">Integrate REST API-enabled services in your custom authentication user journeys.</span></span>  
* <span data-ttu-id="e77ad-115">Adja hozzá az Identitásszolgáltatók, amelyek megfelelnek a szabványos OpenIDConnect összevonásának.</span><span class="sxs-lookup"><span data-stu-id="e77ad-115">Add federation with identity providers that are compliant with the OpenIDConnect standard.</span></span> <br>
* <span data-ttu-id="e77ad-116">Adja hozzá az összevonás az identitás-szolgáltatóktól igazodnia kell a SAML 2.0 protokoll.</span><span class="sxs-lookup"><span data-stu-id="e77ad-116">Add federation with identity providers that adhere to the SAML 2.0 protocol.</span></span> 

## <a name="terms-of-the-public-preview"></a><span data-ttu-id="e77ad-117">A nyilvános előzetes feltételeit.</span><span class="sxs-lookup"><span data-stu-id="e77ad-117">Terms of the public preview</span></span>

* <span data-ttu-id="e77ad-118">Javasoljuk, hogy az új szolgáltatások használatához csak tesztelési célokra.</span><span class="sxs-lookup"><span data-stu-id="e77ad-118">We encourage you to use the new features for evaluation purposes only.</span></span><br>
* <span data-ttu-id="e77ad-119">Az új szolgáltatások nem feltétlenül egy éles környezetben való használathoz.</span><span class="sxs-lookup"><span data-stu-id="e77ad-119">The new features are not intended for use in a production environment.</span></span><br>
* <span data-ttu-id="e77ad-120">Szolgáltatásszint-szerződések (SLA) nem vonatkoznak az új szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="e77ad-120">Service level agreements (SLAs) do not apply to the new features.</span></span> <br>
* <span data-ttu-id="e77ad-121">Támogatási kérelmek nyilvántartásához rendszeres támogatási csatornákon keresztül.</span><span class="sxs-lookup"><span data-stu-id="e77ad-121">Support requests can be filed through regular support channels.</span></span> <br>
* <span data-ttu-id="e77ad-122">Nincs megadva általánosan rendelkezésre álló Ígért dátum.</span><span class="sxs-lookup"><span data-stu-id="e77ad-122">There is no promised date for general availability.</span></span><br>
* <span data-ttu-id="e77ad-123">Saját belátása szerint, és bármilyen okból Microsoft is jelzőt és elutasítása vagy forgatókönyvek és felhasználói útvonal be, amelyek mérete meghaladja az Azure AD B2C termék okleveles felhasználói identitások és hozzáférések (CIAM) felügyeleti platform, mely a körét korlátozza.</span><span class="sxs-lookup"><span data-stu-id="e77ad-123">At our discretion, and for any reason, Microsoft can flag and reject or restrict scenarios and user journeys that exceed the scope of the Azure AD B2C product charter to serve as a customer identity and access management (CIAM) platform.</span></span>

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a><span data-ttu-id="e77ad-124">Egyéni házirend szolgáltatáskészlet fejlesztők feladatai</span><span class="sxs-lookup"><span data-stu-id="e77ad-124">Responsibilities of custom policy feature-set developers</span></span>
<span data-ttu-id="e77ad-125">Kézi házirend konfigurálása az Azure AD B2C az alapul szolgáló platform alacsonyabb szintű hozzáférést biztosít, és egyedi, teljes mértékben testreszabható megbízhatósági keretrendszer létrehozását eredményezi.</span><span class="sxs-lookup"><span data-stu-id="e77ad-125">Manual policy configuration grants lower-level access to the underlying platform of Azure AD B2C and results in the creation of a unique, fully customizable trust framework.</span></span> <span data-ttu-id="e77ad-126">Ezek lehetséges kombinációinak egyéni Identitásszolgáltatók, megbízhatósági kapcsolatokat, külső szolgáltatásokkal, és lépésről lépésre munkafolyamatok Integrációk fel őket a speciális fejlesztők nagyobb iránti igények kielégítése érdekében helyezze el.</span><span class="sxs-lookup"><span data-stu-id="e77ad-126">The possible permutations of custom identity providers, trust relationships, integrations with external services, and step-by-step workflows place greater demands on the advanced developers consuming them.</span></span>

<span data-ttu-id="e77ad-127">Teljes mértékben kihasználják a nyilvános előzetes verzióhoz, javasoljuk, hogy a fejlesztők az egyéni házirend szolgáltatáskészlet fel felelnie a következő irányelveket:</span><span class="sxs-lookup"><span data-stu-id="e77ad-127">To fully benefit from the public preview, we suggest that developers consuming the custom policy feature set adhere to the following guidelines:</span></span>
* <span data-ttu-id="e77ad-128">Ismerkedjen meg az identitás élmény motor és a kulcs vagy titkos kulcsok kezelése konfigurációs nyelve.</span><span class="sxs-lookup"><span data-stu-id="e77ad-128">Become familiar with the configuration language of the Identity Experience Engine and key/secrets management.</span></span>
* <span data-ttu-id="e77ad-129">Saját tulajdonba forgatókönyveket és a használatot.</span><span class="sxs-lookup"><span data-stu-id="e77ad-129">Take ownership of scenarios and custom integrations.</span></span>
* <span data-ttu-id="e77ad-130">A forgatókönyv módszeres tesztek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e77ad-130">Perform methodical scenario testing.</span></span>
* <span data-ttu-id="e77ad-131">Hajtsa végre a szoftverek fejlesztési és átmeneti legalább egy fejlesztési és tesztelési környezetben az ajánlott eljárásokról és egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e77ad-131">Follow software development and staging best practices with a minimum of one development and testing environment and one production environment.</span></span>
* <span data-ttu-id="e77ad-132">Maradjon naprakész az identitás-szolgáltatóktól és integrálja szolgáltatások új fejlesztéseiről.</span><span class="sxs-lookup"><span data-stu-id="e77ad-132">Stay informed about new developments from the identity providers and services you integrate with.</span></span> <span data-ttu-id="e77ad-133">Például nyomon követheti, változások a titkos kulcsok és a szolgáltatás ütemezett és nem ütemezett módosításait.</span><span class="sxs-lookup"><span data-stu-id="e77ad-133">For example, keep track of changes in secrets and of scheduled and unscheduled changes to the service.</span></span>
* <span data-ttu-id="e77ad-134">Aktív figyelés beállítása, és figyelje a figyelt termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e77ad-134">Set up active monitoring, and monitor the responsiveness of production environments.</span></span>
* <span data-ttu-id="e77ad-135">Kapcsolattartási e-mail címek naprakészen tartása, és a Microsoft live hely team e-mailek válaszol marad.</span><span class="sxs-lookup"><span data-stu-id="e77ad-135">Keep contact email addresses current, and stay responsive to the Microsoft live-site team emails.</span></span>
* <span data-ttu-id="e77ad-136">Ha ehhez a Microsoft live hely csapat által ajánlott időben művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e77ad-136">Take timely action when advised to do so by the Microsoft live-site team.</span></span> 


>[!NOTE]
><span data-ttu-id="e77ad-137">Ezek a funkciók végül az Azure AD beépített házirendek, így több elérhető a fejlesztők tartalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="e77ad-137">These features might eventually be included in Azure AD built-in policies, making them more accessible to all developers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e77ad-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e77ad-138">Next steps</span></span>
<span data-ttu-id="e77ad-139">[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="e77ad-139">[Get started with custom policies](active-directory-b2c-get-started-custom.md).</span></span>
