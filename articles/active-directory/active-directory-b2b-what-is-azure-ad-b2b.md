---
title: "Mi az az Azure Active Directory B2B együttműködés? | Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok üzleti partnerek szelektíven érhessék el a vállalati alkalmazásokhoz való engedélyezésével támogatja."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: fbc12a52555b190d43b5e953fd4d19923a25b0ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a><span data-ttu-id="4e5ce-104">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="4e5ce-104">What is Azure AD B2B collaboration?</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

<span data-ttu-id="4e5ce-105">Az Azure AD-vállalatok (B2B) együttműködési képességek engedélyezése bármely szervezet használata az Azure AD biztonságos felhasználók bármilyen más szervezettől származik, minimális és maximális mérete.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-105">Azure AD business-to-business (B2B) collaboration capabilities enable any organization using Azure AD to work safely and securely with users from any other organization, small or large.</span></span> <span data-ttu-id="4e5ce-106">Azon szervezetek lehet az Azure ad-vel vagy nélkül, vagy akár egy informatikai szervezet vagy anélkül.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-106">Those organizations can be with Azure AD or without, or even with an IT organization or without.</span></span> 

<span data-ttu-id="4e5ce-107">Az Azure AD használatával a szervezetek dokumentumokhoz, erőforrások és a partnerek számára, alkalmazások hozzáférést biztosíthat a teljes felügyeletet gyakorolhat a saját vállalati adatok megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-107">Organizations using Azure AD can provide access to documents, resources, and applications to their partners, while maintaining complete control over their own corporate data.</span></span> <span data-ttu-id="4e5ce-108">A fejlesztők használhatják az Azure AD-vállalatok API-alkalmazások írását, amelyek a két szervezet a kapcsolják össze több biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-108">Developers can use the Azure AD business-to-business APIs to write applications that bring two organizations together in more securely.</span></span> <span data-ttu-id="4e5ce-109">Azt is igen egyszerű, a végfelhasználók számára, keresse meg.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-109">Also, it's pretty easy for end users to navigate.</span></span>

<span data-ttu-id="4e5ce-110">ügyfeleink 97 % jelzik, Azure AD B2B együttműködés nagyon fontos, hogy azokat.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-110">97% of our customers have told us Azure AD B2B collaboration is very important to them.</span></span>

![a tortadiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

<span data-ttu-id="4e5ce-112">Korai április 2017 frissítésétől volt körülbelül 3 millió felhasználó alapján már használja az Azure AD B2B együttműködés képességeit.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-112">As of early April 2017, we had about 3 million users already using Azure AD B2B collaboration capabilities.</span></span> <span data-ttu-id="4e5ce-113">És az Azure AD a szervezeteknek, amelyek több mint 10 olyan felhasználót 23 százaléka már élvező ezeket a képességeket.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-113">And more than 23% of Azure AD organizations that have more than 10 users are already benefiting from these capabilities.</span></span>

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a><span data-ttu-id="4e5ce-114">A szervezet Azure AD B2B együttműködés legfontosabb előnyei</span><span class="sxs-lookup"><span data-stu-id="4e5ce-114">The key benefits of Azure AD B2B collaboration to your organization</span></span>

### <a name="work-with-any-user-from-any-partner"></a><span data-ttu-id="4e5ce-115">Bármely felhasználó bármely partnertől használata</span><span class="sxs-lookup"><span data-stu-id="4e5ce-115">Work with any user from any partner</span></span>

* <span data-ttu-id="4e5ce-116">Partnerek a saját hitelesítő adatok használata</span><span class="sxs-lookup"><span data-stu-id="4e5ce-116">Partners use their own credentials</span></span>

* <span data-ttu-id="4e5ce-117">Nem kötelező használni az Azure AD-partnerek számára</span><span class="sxs-lookup"><span data-stu-id="4e5ce-117">No requirement for partners to use Azure AD</span></span>

* <span data-ttu-id="4e5ce-118">Nincsenek külső címtárak vagy bonyolult telepítési szükséges</span><span class="sxs-lookup"><span data-stu-id="4e5ce-118">No external directories or complex set-up required</span></span>

### <a name="simple-and-secure-collaboration"></a><span data-ttu-id="4e5ce-119">Egyszerű és biztonságos együttműködési</span><span class="sxs-lookup"><span data-stu-id="4e5ce-119">Simple and secure collaboration</span></span>

* <span data-ttu-id="4e5ce-120">Minden vállalati alkalmazás vagy adatokat, hozzáférést biztosítanak a kifinomult, az Azure AD által biztosított engedélyezési házirendek alkalmazása közben</span><span class="sxs-lookup"><span data-stu-id="4e5ce-120">Provide access to any corporate app or data, while applying sophisticated, Azure AD-powered authorization policies</span></span>

* <span data-ttu-id="4e5ce-121">A felhasználók számára könnyen</span><span class="sxs-lookup"><span data-stu-id="4e5ce-121">Easy for users</span></span>

* <span data-ttu-id="4e5ce-122">Vállalati szintű védelmet biztosít az alkalmazások és adatok</span><span class="sxs-lookup"><span data-stu-id="4e5ce-122">Enterprise-grade security for apps and data</span></span>

### <a name="no-management-overhead"></a><span data-ttu-id="4e5ce-123">Egyetlen kezelési terhelés mellett.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-123">No management overhead</span></span>

* <span data-ttu-id="4e5ce-124">Nincsenek külső fiókjának vagy jelszavának kezelése</span><span class="sxs-lookup"><span data-stu-id="4e5ce-124">No external account or password management</span></span>

* <span data-ttu-id="4e5ce-125">Egyetlen szinkronizálási vagy manuális fiók életciklusának kezelésére</span><span class="sxs-lookup"><span data-stu-id="4e5ce-125">No sync or manual account lifecycle management</span></span>

* <span data-ttu-id="4e5ce-126">Nincsenek külső adminisztrációs terhelés</span><span class="sxs-lookup"><span data-stu-id="4e5ce-126">No external administrative overhead</span></span>

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a><span data-ttu-id="4e5ce-127">Egyszerűen hozzáadhatja B2B együttműködés felhasználók a szervezet</span><span class="sxs-lookup"><span data-stu-id="4e5ce-127">You can easily add B2B collaboration users to your organization</span></span>

<span data-ttu-id="4e5ce-128">Rendszergazdák B2B együttműködés (vendég) felhasználója hozzáadhatja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e5ce-128">Admins can add B2B collaboration (guest) users in the [Azure portal](https://portal.azure.com).</span></span>

![vendég felhasználók hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a><span data-ttu-id="4e5ce-130">A közreműködők a saját identitás érdekében engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4e5ce-130">Enable your collaborators to bring their own identity</span></span>

<span data-ttu-id="4e5ce-131">B2B közreműködők is jelentkezzen be az általuk választott identitást.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-131">B2B collaborators can sign in with an identity of their choice.</span></span> <span data-ttu-id="4e5ce-132">Ha a felhasználó nem rendelkezik Microsoft-fiókkal vagy egy Azure AD-fiókot – egy hoz létre őket zökkenőmentesen ajánlat beváltásra időpontjában.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-132">If the user doesn’t have a Microsoft account or an Azure AD account – one is created for them seamlessly at the time for offer redemption.</span></span>

![bejelentkezési identitás választás](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a><span data-ttu-id="4e5ce-134">Alkalmazás és a csoportházirend-tulajdonosok delegálása</span><span class="sxs-lookup"><span data-stu-id="4e5ce-134">Delegate to application and group owners</span></span> 
<span data-ttu-id="4e5ce-135">Alkalmazás és a csoportházirend-tulajdonosok adhat hozzá B2B felhasználók közvetlenül bármely alkalmazás, amely a fontos adatokhoz, hogy a Microsoft-alkalmazások vagy a nem.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-135">Application and group owners can add B2B users directly to any application that you care about, whether it is a Microsoft application or not.</span></span> <span data-ttu-id="4e5ce-136">Rendszergazdák számára delegálhatják engedély B2B felhasználók hozzáadása a nem rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-136">Admins can delegate permission to add B2B users to non-admins.</span></span> <span data-ttu-id="4e5ce-137">A nem rendszergazda használhatja a [az Azure AD alkalmazás-hozzáférési Panel](https://myapps.microsoft.com) B2B együttműködés felhasználók hozzáadása az alkalmazások vagy csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-137">Non-admins can use the [Azure AD Application Access Panel](https://myapps.microsoft.com) to add B2B collaboration users to applications or groups.</span></span>

![hozzáférési panel](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a><span data-ttu-id="4e5ce-140">Engedélyezési házirendek a vállalati tartalom védelme</span><span class="sxs-lookup"><span data-stu-id="4e5ce-140">Authorization policies protect your corporate content</span></span>

<span data-ttu-id="4e5ce-141">Kényszerítheti a feltételes hozzáférési szabályzatok, például többtényezős hitelesítés:</span><span class="sxs-lookup"><span data-stu-id="4e5ce-141">Conditional access policies, such as multi-factor authentication, can be enforced:</span></span>
- <span data-ttu-id="4e5ce-142">A bérlői szintű</span><span class="sxs-lookup"><span data-stu-id="4e5ce-142">At the tenant level</span></span>
- <span data-ttu-id="4e5ce-143">Az alkalmazás szintjén</span><span class="sxs-lookup"><span data-stu-id="4e5ce-143">At the application level</span></span>
- <span data-ttu-id="4e5ce-144">Vállalati alkalmazások és adatok védelme érdekében bizonyos felhasználók részére</span><span class="sxs-lookup"><span data-stu-id="4e5ce-144">For specific users to protect corporate apps and data</span></span>

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a><span data-ttu-id="4e5ce-146">Az API-k és a mintakódot érheti alkalmazások</span><span class="sxs-lookup"><span data-stu-id="4e5ce-146">Use our APIs and sample code to easily build applications to onboard</span></span>
<span data-ttu-id="4e5ce-147">A külső partnerek a board a szervezet igényeinek megfelelően testre szabott módon kapcsolja.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-147">Bring your external partners on board in ways customized to your organization’s needs.</span></span>

<span data-ttu-id="4e5ce-148">Használja a [B2B együttműködés meghívó API-k](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), testre szabhatja a bevezetési lép, beleértve az előfizetési önkiszolgáló portálokat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-148">Using the [B2B collaboration invitation APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), you can customize your onboarding experiences, including creating self-service sign-up portals.</span></span> <span data-ttu-id="4e5ce-149">Az önkiszolgáló portál nyújtunk mintakód [a Githubon](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="4e5ce-149">We provide sample code for a self-service portal [on Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

![regisztrációs portál](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

<span data-ttu-id="4e5ce-151">Az Azure AD B2B együttműködés a partnerkapcsolatok oly módon, hogy a végfelhasználók számára egyszerű és intuitív található védelméhez az Azure AD a teljes power kérheti le.</span><span class="sxs-lookup"><span data-stu-id="4e5ce-151">With Azure AD B2B collaboration, you can get the full power of Azure AD to protect your partner relationships in a way that end users find easy and intuitive.</span></span> <span data-ttu-id="4e5ce-152">Ezért induljon el, a külső együttműködés az Azure AD B2B már használó szervezetek több ezer csatlakozáshoz!</span><span class="sxs-lookup"><span data-stu-id="4e5ce-152">So go ahead, join the thousands of organizations that are already using Azure AD B2B for their external collaboration!</span></span>

## <a name="next-steps"></a><span data-ttu-id="4e5ce-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e5ce-153">Next steps</span></span>

* <span data-ttu-id="4e5ce-154">Rendszergazdai élmény találhatók a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4e5ce-154">Administrator experiences are found in the [Azure portal](https://portal.azure.com).</span></span>

* <span data-ttu-id="4e5ce-155">Információk munkavégző lép érhetők el a [hozzáférési Panel](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4e5ce-155">Information worker experiences are available in the [Access Panel](https://myapps.microsoft.com).</span></span>

* <span data-ttu-id="4e5ce-156">És szerint mindig a termékért felelős csoport visszajelzést, beszélgetéseket, és javaslatokat keresztül csatlakozzon a [a Microsoft technikai közösségi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span><span class="sxs-lookup"><span data-stu-id="4e5ce-156">And, as always, connect with the product team for any feedback, discussions, and suggestions through our [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span></span>

<span data-ttu-id="4e5ce-157">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="4e5ce-157">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="4e5ce-158">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="4e5ce-158">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="4e5ce-159">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="4e5ce-159">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="4e5ce-160">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="4e5ce-160">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="4e5ce-161">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="4e5ce-161">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="4e5ce-162">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="4e5ce-162">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="4e5ce-163">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="4e5ce-163">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="4e5ce-164">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="4e5ce-164">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="4e5ce-165">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="4e5ce-165">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="4e5ce-166">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="4e5ce-166">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="4e5ce-167">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="4e5ce-167">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="4e5ce-168">Naplózási és jelentéskészítési B2B együttműködés felhasználó</span><span class="sxs-lookup"><span data-stu-id="4e5ce-168">B2B collaboration user auditing and reporting</span></span>](active-directory-b2b-auditing-and-reporting.md)
* [<span data-ttu-id="4e5ce-169">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="4e5ce-169">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
