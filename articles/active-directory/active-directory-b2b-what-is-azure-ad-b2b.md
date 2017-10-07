---
title: "aaaWhat az Azure Active Directory B2B együttműködés? | Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok üzleti partnerek tooselectively hozzáférés engedélyezése a vállalati alkalmazásokat támogatja."
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
ms.openlocfilehash: 359989b66f3d012c306e8748a607662fffacb919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a><span data-ttu-id="fa167-104">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="fa167-104">What is Azure AD B2B collaboration?</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

<span data-ttu-id="fa167-105">Az Azure AD-vállalatok (B2B) együttműködési képességek engedélyezése bármely szervezet használata az Azure AD toowork biztonságos felhasználók bármilyen más szervezettől származik, minimális és maximális mérete.</span><span class="sxs-lookup"><span data-stu-id="fa167-105">Azure AD business-to-business (B2B) collaboration capabilities enable any organization using Azure AD toowork safely and securely with users from any other organization, small or large.</span></span> <span data-ttu-id="fa167-106">Azon szervezetek lehet az Azure ad-vel vagy nélkül, vagy akár egy informatikai szervezet vagy anélkül.</span><span class="sxs-lookup"><span data-stu-id="fa167-106">Those organizations can be with Azure AD or without, or even with an IT organization or without.</span></span> 

<span data-ttu-id="fa167-107">A szervezetek az Azure AD használatával hozzáférést biztosíthat toodocuments, erőforrások és alkalmazások tootheir partnerek, teljes felügyeletet gyakorolhat a saját vállalati adatok megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="fa167-107">Organizations using Azure AD can provide access toodocuments, resources, and applications tootheir partners, while maintaining complete control over their own corporate data.</span></span> <span data-ttu-id="fa167-108">A fejlesztők a hello Azure AD vállalatok API-k toowrite alkalmazásokat, amelyek a két szervezet a kapcsolják össze több biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="fa167-108">Developers can use hello Azure AD business-to-business APIs toowrite applications that bring two organizations together in more securely.</span></span> <span data-ttu-id="fa167-109">Azt is igen egyszerű, a végfelhasználók toonavigate.</span><span class="sxs-lookup"><span data-stu-id="fa167-109">Also, it's pretty easy for end users toonavigate.</span></span>

<span data-ttu-id="fa167-110">ügyfeleink 97 % jelzik, Azure AD B2B együttműködés nagyon fontos toothem.</span><span class="sxs-lookup"><span data-stu-id="fa167-110">97% of our customers have told us Azure AD B2B collaboration is very important toothem.</span></span>

![a tortadiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

<span data-ttu-id="fa167-112">Korai április 2017 frissítésétől volt körülbelül 3 millió felhasználó alapján már használja az Azure AD B2B együttműködés képességeit.</span><span class="sxs-lookup"><span data-stu-id="fa167-112">As of early April 2017, we had about 3 million users already using Azure AD B2B collaboration capabilities.</span></span> <span data-ttu-id="fa167-113">És az Azure AD a szervezeteknek, amelyek több mint 10 olyan felhasználót 23 százaléka már élvező ezeket a képességeket.</span><span class="sxs-lookup"><span data-stu-id="fa167-113">And more than 23% of Azure AD organizations that have more than 10 users are already benefiting from these capabilities.</span></span>

## <a name="hello-key-benefits-of-azure-ad-b2b-collaboration-tooyour-organization"></a><span data-ttu-id="fa167-114">Azure AD B2B együttműködés tooyour szervezet hello legfontosabb előnyei</span><span class="sxs-lookup"><span data-stu-id="fa167-114">hello key benefits of Azure AD B2B collaboration tooyour organization</span></span>

### <a name="work-with-any-user-from-any-partner"></a><span data-ttu-id="fa167-115">Bármely felhasználó bármely partnertől használata</span><span class="sxs-lookup"><span data-stu-id="fa167-115">Work with any user from any partner</span></span>

* <span data-ttu-id="fa167-116">Partnerek a saját hitelesítő adatok használata</span><span class="sxs-lookup"><span data-stu-id="fa167-116">Partners use their own credentials</span></span>

* <span data-ttu-id="fa167-117">Az Azure AD partnerek toouse nincsenek követelményei</span><span class="sxs-lookup"><span data-stu-id="fa167-117">No requirement for partners toouse Azure AD</span></span>

* <span data-ttu-id="fa167-118">Nincsenek külső címtárak vagy bonyolult telepítési szükséges</span><span class="sxs-lookup"><span data-stu-id="fa167-118">No external directories or complex set-up required</span></span>

### <a name="simple-and-secure-collaboration"></a><span data-ttu-id="fa167-119">Egyszerű és biztonságos együttműködési</span><span class="sxs-lookup"><span data-stu-id="fa167-119">Simple and secure collaboration</span></span>

* <span data-ttu-id="fa167-120">Adja meg a hozzáférés tooany vállalati alkalmazás vagy adatokat, kifinomult, az Azure AD által biztosított engedélyezési házirendek alkalmazása közben</span><span class="sxs-lookup"><span data-stu-id="fa167-120">Provide access tooany corporate app or data, while applying sophisticated, Azure AD-powered authorization policies</span></span>

* <span data-ttu-id="fa167-121">A felhasználók számára könnyen</span><span class="sxs-lookup"><span data-stu-id="fa167-121">Easy for users</span></span>

* <span data-ttu-id="fa167-122">Vállalati szintű védelmet biztosít az alkalmazások és adatok</span><span class="sxs-lookup"><span data-stu-id="fa167-122">Enterprise-grade security for apps and data</span></span>

### <a name="no-management-overhead"></a><span data-ttu-id="fa167-123">Egyetlen kezelési terhelés mellett.</span><span class="sxs-lookup"><span data-stu-id="fa167-123">No management overhead</span></span>

* <span data-ttu-id="fa167-124">Nincsenek külső fiókjának vagy jelszavának kezelése</span><span class="sxs-lookup"><span data-stu-id="fa167-124">No external account or password management</span></span>

* <span data-ttu-id="fa167-125">Egyetlen szinkronizálási vagy manuális fiók életciklusának kezelésére</span><span class="sxs-lookup"><span data-stu-id="fa167-125">No sync or manual account lifecycle management</span></span>

* <span data-ttu-id="fa167-126">Nincsenek külső adminisztrációs terhelés</span><span class="sxs-lookup"><span data-stu-id="fa167-126">No external administrative overhead</span></span>

## <a name="you-can-easily-add-b2b-collaboration-users-tooyour-organization"></a><span data-ttu-id="fa167-127">Egyszerűen hozzáadhatja a B2B együttműködés felhasználók tooyour szervezet</span><span class="sxs-lookup"><span data-stu-id="fa167-127">You can easily add B2B collaboration users tooyour organization</span></span>

<span data-ttu-id="fa167-128">Rendszergazdák B2B együttműködés (vendég) felhasználókat adhat hozzá hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa167-128">Admins can add B2B collaboration (guest) users in hello [Azure portal](https://portal.azure.com).</span></span>

![vendég felhasználók hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-toobring-their-own-identity"></a><span data-ttu-id="fa167-130">A közreműködők toobring saját identitás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fa167-130">Enable your collaborators toobring their own identity</span></span>

<span data-ttu-id="fa167-131">B2B közreműködők is jelentkezzen be az általuk választott identitást.</span><span class="sxs-lookup"><span data-stu-id="fa167-131">B2B collaborators can sign in with an identity of their choice.</span></span> <span data-ttu-id="fa167-132">Ha hello felhasználó nem rendelkezik Microsoft-fiókkal vagy egy Azure AD-fiókot – egy során jön létre a számukra zökkenőmentesen hello ajánlat beváltásra.</span><span class="sxs-lookup"><span data-stu-id="fa167-132">If hello user doesn’t have a Microsoft account or an Azure AD account – one is created for them seamlessly at hello time for offer redemption.</span></span>

![bejelentkezési identitás választás](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-tooapplication-and-group-owners"></a><span data-ttu-id="fa167-134">Delegált tooapplication és csoport tulajdonosainak</span><span class="sxs-lookup"><span data-stu-id="fa167-134">Delegate tooapplication and group owners</span></span> 
<span data-ttu-id="fa167-135">Alkalmazás és a csoportházirend-tulajdonosok B2B felhasználókat adhat hozzá közvetlenül az Ön számára legfontosabb, hogy a Microsoft-alkalmazások vagy a nem tooany alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fa167-135">Application and group owners can add B2B users directly tooany application that you care about, whether it is a Microsoft application or not.</span></span> <span data-ttu-id="fa167-136">Rendszergazdák engedélyt tooadd B2B felhasználók toonon-rendszergazdák számára delegálhatják.</span><span class="sxs-lookup"><span data-stu-id="fa167-136">Admins can delegate permission tooadd B2B users toonon-admins.</span></span> <span data-ttu-id="fa167-137">A nem rendszergazda használható hello [az Azure AD alkalmazás-hozzáférési Panel](https://myapps.microsoft.com) tooadd B2B együttműködés felhasználók tooapplications vagy csoportokat.</span><span class="sxs-lookup"><span data-stu-id="fa167-137">Non-admins can use hello [Azure AD Application Access Panel](https://myapps.microsoft.com) tooadd B2B collaboration users tooapplications or groups.</span></span>

![hozzáférési panel](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a><span data-ttu-id="fa167-140">Engedélyezési házirendek a vállalati tartalom védelme</span><span class="sxs-lookup"><span data-stu-id="fa167-140">Authorization policies protect your corporate content</span></span>

<span data-ttu-id="fa167-141">Kényszerítheti a feltételes hozzáférési szabályzatok, például többtényezős hitelesítés:</span><span class="sxs-lookup"><span data-stu-id="fa167-141">Conditional access policies, such as multi-factor authentication, can be enforced:</span></span>
- <span data-ttu-id="fa167-142">Hello bérlői szinten</span><span class="sxs-lookup"><span data-stu-id="fa167-142">At hello tenant level</span></span>
- <span data-ttu-id="fa167-143">Hello alkalmazás szinten</span><span class="sxs-lookup"><span data-stu-id="fa167-143">At hello application level</span></span>
- <span data-ttu-id="fa167-144">Az adott felhasználóknak tooprotect vállalati alkalmazások és adatok</span><span class="sxs-lookup"><span data-stu-id="fa167-144">For specific users tooprotect corporate apps and data</span></span>

![tag hozzáadása](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-tooeasily-build-applications-tooonboard"></a><span data-ttu-id="fa167-146">Az API-k és minta kód tooeasily alkalmazások tooonboard összeállítása</span><span class="sxs-lookup"><span data-stu-id="fa167-146">Use our APIs and sample code tooeasily build applications tooonboard</span></span>
<span data-ttu-id="fa167-147">Kapcsolja a külső partnerek a board módon testreszabott tooyour szervezete igényeinek.</span><span class="sxs-lookup"><span data-stu-id="fa167-147">Bring your external partners on board in ways customized tooyour organization’s needs.</span></span>

<span data-ttu-id="fa167-148">Hello segítségével [B2B együttműködés meghívó API-k](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), testre szabhatja a bevezetési lép, beleértve az előfizetési önkiszolgáló portálokat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fa167-148">Using hello [B2B collaboration invitation APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), you can customize your onboarding experiences, including creating self-service sign-up portals.</span></span> <span data-ttu-id="fa167-149">Az önkiszolgáló portál nyújtunk mintakód [a Githubon](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="fa167-149">We provide sample code for a self-service portal [on Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

![regisztrációs portál](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

<span data-ttu-id="fa167-151">Azure AD B2B együttműködés érhet hello teljes hatványra emelésének Azure AD tooprotect a partnerkapcsolatok oly módon, hogy a végfelhasználók számára egyszerű és intuitív található.</span><span class="sxs-lookup"><span data-stu-id="fa167-151">With Azure AD B2B collaboration, you can get hello full power of Azure AD tooprotect your partner relationships in a way that end users find easy and intuitive.</span></span> <span data-ttu-id="fa167-152">Ezért habozzon, illesztési hello több ezer a külső együttműködés az Azure AD B2B már használó szervezetek!</span><span class="sxs-lookup"><span data-stu-id="fa167-152">So go ahead, join hello thousands of organizations that are already using Azure AD B2B for their external collaboration!</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa167-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa167-153">Next steps</span></span>

* <span data-ttu-id="fa167-154">Rendszergazdai élmény találhatók hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa167-154">Administrator experiences are found in hello [Azure portal](https://portal.azure.com).</span></span>

* <span data-ttu-id="fa167-155">Információk munkavégző lép érhetők el hello [hozzáférési Panel](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="fa167-155">Information worker experiences are available in hello [Access Panel](https://myapps.microsoft.com).</span></span>

* <span data-ttu-id="fa167-156">És visszajelzés, beszélgetéseket, és javaslatokat keresztül hello termékért felelős csoport szerint mindig csatlakozzon a [a Microsoft technikai közösségi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span><span class="sxs-lookup"><span data-stu-id="fa167-156">And, as always, connect with hello product team for any feedback, discussions, and suggestions through our [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span></span>

<span data-ttu-id="fa167-157">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="fa167-157">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="fa167-158">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="fa167-158">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="fa167-159">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="fa167-159">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="fa167-160">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="fa167-160">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="fa167-161">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="fa167-161">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="fa167-162">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="fa167-162">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="fa167-163">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="fa167-163">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="fa167-164">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="fa167-164">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="fa167-165">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="fa167-165">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="fa167-166">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="fa167-166">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="fa167-167">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="fa167-167">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="fa167-168">Naplózási és jelentéskészítési B2B együttműködés felhasználó</span><span class="sxs-lookup"><span data-stu-id="fa167-168">B2B collaboration user auditing and reporting</span></span>](active-directory-b2b-auditing-and-reporting.md)
* [<span data-ttu-id="fa167-169">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="fa167-169">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
