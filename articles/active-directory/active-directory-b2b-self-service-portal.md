---
title: "Önkiszolgáló regisztrációs portálon az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok támogatása érdekében lehetővé teszi, hogy az üzleti partnerek szelektíven érhessék el a vállalati alkalmazásokat"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a><span data-ttu-id="33b4e-103">Az Azure AD B2B együttműködés előfizetési önkiszolgáló portál</span><span class="sxs-lookup"><span data-stu-id="33b4e-103">Self-service portal for Azure AD B2B collaboration sign-up</span></span>

<span data-ttu-id="33b4e-104">Az ügyfelek sokkal teheti a beépített szolgáltatásai az informatikai rendszergazda keresztül közzétett [Azure-portálon](https://portal.azure.com) és a [alkalmazás hozzáférési Panel](https://myapps.microsoft.com) a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="33b4e-104">Customers can do a lot with the built-in features that are exposed through our IT admin [Azure portal](https://portal.azure.com) and our [Application Access Panel](https://myapps.microsoft.com) for end users.</span></span> <span data-ttu-id="33b4e-105">De azt is vegye figyelembe, hogy vállalatok testre kell szabnia a bevezetési munkafolyamat B2B felhasználók számára a szervezeti igényeknek.</span><span class="sxs-lookup"><span data-stu-id="33b4e-105">But we are also aware that businesses need to customize the onboarding workflow for B2B users to fit their organization’s needs.</span></span> <span data-ttu-id="33b4e-106">Akkor teheti meg, hogy a [az API felületen](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span><span class="sxs-lookup"><span data-stu-id="33b4e-106">They can do that with [our API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span></span>

<span data-ttu-id="33b4e-107">Az ügyfelekkel folytatott beszélgetéseket látható egy közös kell növekedésnek mindenekelőtt mások fel.</span><span class="sxs-lookup"><span data-stu-id="33b4e-107">In discussions with our customers, we see one common need rise up above all others.</span></span> <span data-ttu-id="33b4e-108">Hívja fel a szervezet előfordulhat, hogy nem tudja időben az egyes külső együttműködők akik számára az erőforrásokhoz való hozzáférésre van szükségük.</span><span class="sxs-lookup"><span data-stu-id="33b4e-108">The inviting organization may not know ahead of time who the individual external collaborators are who need access to their resources.</span></span> <span data-ttu-id="33b4e-109">A felhasználók partnervállalatokból regisztrálni magát, amely a hívja fel a szervezet házirendjei úgy végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="33b4e-109">They wanted a way for users from partner companies to  sign themselves up with a set of policies that the inviting organization controls.</span></span> <span data-ttu-id="33b4e-110">Ebben a forgatókönyvben az API-k segítségével lehetséges, így azt a projektet a Githubon, amely csak elvégző közzétevő: [Github mintaprojektet](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="33b4e-110">This scenario is possible through our APIs,  so we published a project on Github that did just that: [sample Github project](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

<span data-ttu-id="33b4e-111">A Github-projekt bemutatja, hogyan szervezetek az API-k, és adja meg egy csoportházirend-alapú, az önkiszolgáló bejelentkezési képesség a megbízható partnerek, szabályok, amelyek meghatározzák, az alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="33b4e-111">Our Github project demonstrates how organizations can use our APIs, and provide a policy-based, self-service sign-up capability for their trusted partners, with rules that determine the apps they can access.</span></span> <span data-ttu-id="33b4e-112">Partner felhasználók is kapnak erőforrásokhoz való hozzáférés, amikor szükségük van rá, biztonságosan, anélkül, hogy a manuális előkészítésére hívja fel szervezet őket.</span><span class="sxs-lookup"><span data-stu-id="33b4e-112">Partner users can get access to resources when they need them, securely, without requiring the inviting organization to manually onboard them.</span></span> <span data-ttu-id="33b4e-113">A projekt az Azure-előfizetés tetszés szerinti könnyen telepíthető.</span><span class="sxs-lookup"><span data-stu-id="33b4e-113">You can easily deploy the project into an Azure subscription of your choice.</span></span>

## <a name="as-is-code"></a><span data-ttu-id="33b4e-114">Mint-kód</span><span class="sxs-lookup"><span data-stu-id="33b4e-114">As-is code</span></span>

<span data-ttu-id="33b4e-115">Ne feledje, hogy ez a kód bemutatása az Azure Active Directory B2B meghívó API használatát egy minta elérhetővé tegyen.</span><span class="sxs-lookup"><span data-stu-id="33b4e-115">Remember that this code is made available as a sample to demonstrate usage of the Azure Active Directory B2B invitation API.</span></span> <span data-ttu-id="33b4e-116">A fejlesztői csapat vagy egy partner által kell szabható testre, és egy éles telepítési forgatókönyvhöz a telepített előtt meg kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="33b4e-116">It should be customized by your dev team or a partner, and should be reviewed before being deployed in a production scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33b4e-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33b4e-117">Next steps</span></span>

<span data-ttu-id="33b4e-118">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="33b4e-118">Browse our other articles on Azure AD B2B collaboration:</span></span>
* [<span data-ttu-id="33b4e-119">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="33b4e-119">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="33b4e-120">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="33b4e-120">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="33b4e-121">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="33b4e-121">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="33b4e-122">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="33b4e-122">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="33b4e-123">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="33b4e-123">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="33b4e-124">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="33b4e-124">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="33b4e-125">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="33b4e-125">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="33b4e-126">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="33b4e-126">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="33b4e-127">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="33b4e-127">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="33b4e-128">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="33b4e-128">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="33b4e-129">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="33b4e-129">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)