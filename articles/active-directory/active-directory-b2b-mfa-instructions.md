---
title: "Azure Active Directory B2B együttműködés felhasználók hozzáférésének aaaConditional |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés többtényezős hitelesítés (MFA) támogatja a szelektív hozzáférést tooyour vállalati alkalmazások"
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
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="2e3f1-103">Feltételes hozzáférés a B2B együttműködés felhasználók</span><span class="sxs-lookup"><span data-stu-id="2e3f1-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="2e3f1-104">A B2B felhasználók a többtényezős hitelesítést</span><span class="sxs-lookup"><span data-stu-id="2e3f1-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="2e3f1-105">Az Azure AD B2B együttműködés a szervezetek kényszerítheti a többtényezős hitelesítés (MFA) házirendek a B2B felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="2e3f1-106">Ezek a házirendek kényszerítheti a hello bérlői, alkalmazás vagy egyéni felhasználói szintjén, hello azonos módon, hogy a teljes munkaidejű alkalmazottak és a szervezet hello engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-106">These policies can be enforced at hello tenant, app, or individual user level, hello same way that they are enabled for full-time employees and members of hello organization.</span></span> <span data-ttu-id="2e3f1-107">Többtényezős hitelesítési házirendek érvényben vannak hello erőforrás-szervezetben.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-107">MFA policies are enforced at hello resource organization.</span></span>

<span data-ttu-id="2e3f1-108">Példa:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-108">Example:</span></span>
1. <span data-ttu-id="2e3f1-109">A vállalat rendszergazdai vagy információk munkavégző felkéri vállalati B tooan alkalmazás felhasználói *PEL* vállalat azonosítójához.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-109">Admin or information worker in Company A invites user from company B tooan application *Foo* in company A.</span></span>
2. <span data-ttu-id="2e3f1-110">Alkalmazás *PEL* vállalat A konfigurált toorequire MFA hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-110">Application *Foo* in company A is configured toorequire MFA on access.</span></span>
3. <span data-ttu-id="2e3f1-111">Amikor a vállalat B hello felhasználó kísérli meg tooaccess app *PEL* hello vállalati A bérlőt, a rendszer kéri toocomplete az MFA-kérdést.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-111">When hello user from company B attempts tooaccess app *Foo* in hello company A tenant, they are asked toocomplete an MFA challenge.</span></span>
4. <span data-ttu-id="2e3f1-112">hello felhasználó állíthat be az MFA legyenek A vállalat, majd a többtényezős hitelesítés lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-112">hello user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="2e3f1-113">Ebben a forgatókönyvben minden identitás működik (az Azure AD vagy az msa-t, például, ha a felhasználók a vállalati B azonosítania társadalombiztosítási azonosító)</span><span class="sxs-lookup"><span data-stu-id="2e3f1-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="2e3f1-114">A vállalat elegendő az Azure AD Premium licenc, amely támogatja a többtényezős Hitelesítést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="2e3f1-115">hello felhasználói vállalat B igényel, a vállalat A. licenc</span><span class="sxs-lookup"><span data-stu-id="2e3f1-115">hello user from company B consumes this license from company A.</span></span>

<span data-ttu-id="2e3f1-116">hello hívja fel bérleti feladata mindig a multi-factor Authentication hello erőforráspartner szervezetnél, a felhasználók akkor is, ha a fiókpartner-szervezet hello MFA képességekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-116">hello inviting tenancy is always responsible for MFA for users from hello partner organization, even if hello partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="2e3f1-117">Többtényezős hitelesítés beállítása B2B együttműködés felhasználók</span><span class="sxs-lookup"><span data-stu-id="2e3f1-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="2e3f1-118">milyen egyszerűen B2B együttműködés a felhasználóknak többtényezős hitelesítés be tooset toodiscover lásd: hogyan a következő videó hello:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-118">toodiscover how easy it is tooset up MFA for B2B collaboration users, see how in hello following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="2e3f1-119">Többtényezős hitelesítés élményt a B2B felhasználók kínálnak érvényesítési</span><span class="sxs-lookup"><span data-stu-id="2e3f1-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="2e3f1-120">Tekintse meg a következő animáció toosee hello érvényesítési élmény hello:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-120">Check out hello following animation toosee hello redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="2e3f1-121">B2B együttműködés felhasználóknak az új MFA</span><span class="sxs-lookup"><span data-stu-id="2e3f1-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="2e3f1-122">Jelenleg Üdvözöljük a rendszergazdákat a jelszó használata kötelezővé B2B együttműködés felhasználók tooproof be újra csak hello a következő PowerShell-parancsmagok használatával:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-122">Currently, hello admin can require B2B collaboration users tooproof up again only by using hello following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="2e3f1-123">TooAzure AD Connect</span><span class="sxs-lookup"><span data-stu-id="2e3f1-123">Connect tooAzure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="2e3f1-124">Minden felhasználó lekérdezni módszereket igazolása</span><span class="sxs-lookup"><span data-stu-id="2e3f1-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="2e3f1-125">Például:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="2e3f1-126">Hello többtényezős hitelesítési módszer egy adott felhasználó toorequire hello B2B együttműködés felhasználói tooset igazolása felfelé módszerek újra átállítani.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-126">Reset hello MFA method for a specific user toorequire hello B2B collaboration user tooset proof-up methods again.</span></span> <span data-ttu-id="2e3f1-127">Példa:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a><span data-ttu-id="2e3f1-128">Miért most elvégezni az MFA a hello erőforrás bérleti:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-128">Why do we perform MFA at hello resource tenancy?</span></span>

<span data-ttu-id="2e3f1-129">Hello jelenlegi kiadásban MFA mindig van hello erőforrás bérlőhöz, kiszámíthatóságot miatt.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-129">In hello current release, MFA is always in hello resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="2e3f1-130">Például tegyük fel, a Contoso a felhasználó (Sally) meghívott tooFabrikam és Fabrikam engedélyezte a többtényezős hitelesítés B2B felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-130">For example, let’s say a Contoso user (Sally) is invited tooFabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="2e3f1-131">Ha Contoso többtényezős hitelesítési szabályzat az App1 számítógépen, de nem App2 engedélyezve van, majd úgy tekintünk hello Contoso MFA jogcím hello tokent, ha azt tapasztalhatja hello probléma a következő:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at hello Contoso MFA claim in hello token, we might see hello following issue:</span></span>

* <span data-ttu-id="2e3f1-132">1. napja: A felhasználó rendelkezik-e többtényezős hitelesítés Contoso és fér hozzá az App1, akkor nincs további MFA kérdés megjelenik-e a Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="2e3f1-133">2. napon: hello felhasználó általi App 2 futnak, így most Fabrikam elérésekor, kell regisztrálnia az MFA van.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-133">Day 2: hello user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="2e3f1-134">Ez a folyamat zavaró lehet, és a bejelentkezési befejezésekhez toodrop vezethet.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-134">This process can be confusing and could lead toodrop in sign-in completions.</span></span>

<span data-ttu-id="2e3f1-135">Továbbá akkor is, ha a Contoso többtényezős hitelesítési funkció, nincs mindig hello eset hello Fabrikam volna megbízható hello Contoso többtényezős hitelesítési szabályzat.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-135">Moreover, even if Contoso has MFA capability, it is not always hello case hello Fabrikam would trust hello Contoso MFA policy.</span></span>

<span data-ttu-id="2e3f1-136">Végezetül erőforrás bérlőt többtényezős Hitelesítést is működik, az msa-k és társadalombiztosítási azonosítókat és az erőforráspartner-szervezethez, amelyek nem rendelkeznek a többtényezős hitelesítés beállítása.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="2e3f1-137">Hello ajánlott az MFA szolgáltatásra B2B felhasználók ezért tooalways megkövetelő a bérlő meghívása hello.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-137">Therefore, hello recommendation for MFA for B2B users is tooalways require MFA in hello inviting tenant.</span></span> <span data-ttu-id="2e3f1-138">Ez a követelmény toodouble MFA bizonyos esetekben előfordulhat, de hello hívja fel bérlői fér hozzá, amikor hello végfelhasználói élmény kiszámítható: Sally regisztrálnia kell a multi-factor Authentication hello hívja fel bérlő.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-138">This requirement could lead toodouble MFA in some cases, but whenever accessing hello inviting tenant, hello end-users experience is predictable: Sally must register for MFA with hello inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="2e3f1-139">Eszköz, helyét és kockázati-alapú feltételes hozzáférés a B2B felhasználók</span><span class="sxs-lookup"><span data-stu-id="2e3f1-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="2e3f1-140">Contoso lehetővé teszi, hogy a vállalati adatok eszközalapú feltételes hozzáférési házirendeket, amikor a rendszer megakadályozza hozzáférési és hello Contoso eszköz házirendeknek nem megfelelő, a Contoso nem kezelt eszközök.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with hello Contoso device policies.</span></span>

<span data-ttu-id="2e3f1-141">Ha hello B2B felhasználó-eszköz nem a Contoso felügyeli, a B2B felhasználókat az erőforráspartner-szervezetek hello le van tiltva a abban a környezetben, ezek a házirendek érvényben vannak.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-141">If hello B2B user’s device isn't managed by Contoso, access of B2B users from hello partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="2e3f1-142">Contoso azonban adott partner felhasználók tooexclude tartalmazó listák azokat hello eszközalapú feltételes hozzáférési házirend kizárási hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-142">However, Contoso can create exclusion lists containing specific partner users tooexclude them from hello device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="2e3f1-143">Feltételes hozzáférés helyalapú B2B</span><span class="sxs-lookup"><span data-stu-id="2e3f1-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="2e3f1-144">Helyalapú feltételes hozzáférési szabályzatokat a B2B felhasználók kényszeríthető, ha hello hívja fel szervezet képes toocreate egy megbízható IP-címtartományt, amely meghatározza a fiókpartner-szervezetek.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-144">Location-based conditional access policies can be enforced for B2B users if hello inviting organization is able toocreate a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="2e3f1-145">Kockázati-alapú feltételes hozzáférés a B2B</span><span class="sxs-lookup"><span data-stu-id="2e3f1-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="2e3f1-146">Jelenleg kockázati-alapú bejelentkezési házirendek nem lehet alkalmazott tooB2B felhasználók, mert hello B2B felhasználó otthoni szervezet hello kockázat kiértékelésekor történik.</span><span class="sxs-lookup"><span data-stu-id="2e3f1-146">Currently, risk-based sign-in policies cannot be applied tooB2B users because hello risk evaluation is performed at hello B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e3f1-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e3f1-147">Next steps</span></span>

<span data-ttu-id="2e3f1-148">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="2e3f1-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="2e3f1-149">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="2e3f1-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="2e3f1-150">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="2e3f1-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="2e3f1-151">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="2e3f1-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="2e3f1-152">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="2e3f1-152">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="2e3f1-153">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="2e3f1-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="2e3f1-154">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="2e3f1-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="2e3f1-155">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="2e3f1-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="2e3f1-156">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="2e3f1-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="2e3f1-157">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="2e3f1-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="2e3f1-158">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="2e3f1-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="2e3f1-159">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="2e3f1-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
