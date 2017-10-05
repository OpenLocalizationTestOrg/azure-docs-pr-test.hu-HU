---
title: "Feltételes hozzáférés az Azure Active Directory B2B együttműködés felhasználók |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés többtényezős hitelesítés (MFA) támogatja a szelektív hozzáférést biztosít a vállalati alkalmazásokhoz"
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
ms.openlocfilehash: d85f711d6551a68d1248ae8ec61e2ecc1ddc8ecd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="ec4a0-103">Feltételes hozzáférés a B2B együttműködés felhasználók</span><span class="sxs-lookup"><span data-stu-id="ec4a0-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="ec4a0-104">A B2B felhasználók a többtényezős hitelesítést</span><span class="sxs-lookup"><span data-stu-id="ec4a0-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="ec4a0-105">Az Azure AD B2B együttműködés a szervezetek kényszerítheti a többtényezős hitelesítés (MFA) házirendek a B2B felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="ec4a0-106">Ezek a házirendek kényszerítheti a bérlői, alkalmazás vagy egyéni felhasználói szintjén, a megszokott módon, hogy a teljes munkaidejű alkalmazottak és a szervezet a engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-106">These policies can be enforced at the tenant, app, or individual user level, the same way that they are enabled for full-time employees and members of the organization.</span></span> <span data-ttu-id="ec4a0-107">Többtényezős hitelesítési házirendek érvényben vannak, az erőforrás-szervezetben.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-107">MFA policies are enforced at the resource organization.</span></span>

<span data-ttu-id="ec4a0-108">Példa:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-108">Example:</span></span>
1. <span data-ttu-id="ec4a0-109">A vállalat rendszergazdai vagy információk munkavégző meghívja a vállalat B egy alkalmazás felhasználói *PEL* vállalat azonosítójához.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-109">Admin or information worker in Company A invites user from company B to an application *Foo* in company A.</span></span>
2. <span data-ttu-id="ec4a0-110">Alkalmazás *PEL* vállalat A többtényezős Hitelesítést a hozzáférési úgy van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-110">Application *Foo* in company A is configured to require MFA on access.</span></span>
3. <span data-ttu-id="ec4a0-111">Ha a vállalat B felhasználó megpróbálja elérni az alkalmazás *PEL* A bérlők a vállalatnál is megkéri őket végezze el az MFA-kérdést.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-111">When the user from company B attempts to access app *Foo* in the company A tenant, they are asked to complete an MFA challenge.</span></span>
4. <span data-ttu-id="ec4a0-112">A felhasználó az MFA legyenek A vállalat állíthat be, és úgy dönt, a többtényezős hitelesítés lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-112">The user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="ec4a0-113">Ebben a forgatókönyvben minden identitás működik (az Azure AD vagy az msa-t, például, ha a felhasználók a vállalati B azonosítania társadalombiztosítási azonosító)</span><span class="sxs-lookup"><span data-stu-id="ec4a0-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="ec4a0-114">A vállalat elegendő az Azure AD Premium licenc, amely támogatja a többtényezős Hitelesítést kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="ec4a0-115">A felhasználó a vállalati B igényel, a vállalat A. licenc</span><span class="sxs-lookup"><span data-stu-id="ec4a0-115">The user from company B consumes this license from company A.</span></span>

<span data-ttu-id="ec4a0-116">Hívja fel a bérlet felelős mindig a multi-factor Authentication a felhasználókat az erőforráspartner szervezetnél, akkor is, ha a fiókpartner-szervezet MFA képességekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-116">The inviting tenancy is always responsible for MFA for users from the partner organization, even if the partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="ec4a0-117">Többtényezős hitelesítés beállítása B2B együttműködés felhasználók</span><span class="sxs-lookup"><span data-stu-id="ec4a0-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="ec4a0-118">Annak megállapításához, hogy milyen egyszerűen B2B együttműködés felhasználók többtényezős hitelesítés beállítása, tekintse meg az alábbi videóban módját:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-118">To discover how easy it is to set up MFA for B2B collaboration users, see how in the following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="ec4a0-119">Többtényezős hitelesítés élményt a B2B felhasználók kínálnak érvényesítési</span><span class="sxs-lookup"><span data-stu-id="ec4a0-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="ec4a0-120">Tekintse meg az érvényesítési megtekintéséhez a következő animáció élményt:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-120">Check out the following animation to see the redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="ec4a0-121">B2B együttműködés felhasználóknak az új MFA</span><span class="sxs-lookup"><span data-stu-id="ec4a0-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="ec4a0-122">Jelenleg a rendszergazda megkövetelheti B2B együttműködés felhasználók igazolása be újra csak a következő PowerShell-parancsmagok használatával:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-122">Currently, the admin can require B2B collaboration users to proof up again only by using the following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="ec4a0-123">Csatlakozás az Azure AD szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="ec4a0-123">Connect to Azure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="ec4a0-124">Minden felhasználó lekérdezni módszereket igazolása</span><span class="sxs-lookup"><span data-stu-id="ec4a0-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="ec4a0-125">Például:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="ec4a0-126">Állítsa vissza a multi-factor Authentication módszer egy adott felhasználó, hogy a B2B együttműködés felhasználó igazolása felfelé módszerek újból beállítani.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-126">Reset the MFA method for a specific user to require the B2B collaboration user to set proof-up methods again.</span></span> <span data-ttu-id="ec4a0-127">Példa:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a><span data-ttu-id="ec4a0-128">Miért most elvégezni a többtényezős Hitelesítést, az erőforrás-bérlőhöz?</span><span class="sxs-lookup"><span data-stu-id="ec4a0-128">Why do we perform MFA at the resource tenancy?</span></span>

<span data-ttu-id="ec4a0-129">A jelenlegi kiadásban MFA mindig van kapcsolva az erőforrás-bérlőhöz, kiszámíthatóságot miatt.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-129">In the current release, MFA is always in the resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="ec4a0-130">Például tegyük fel, a Contoso felhasználó (Sally) felkérik, hogy a Fabrikam és Fabrikam engedélyezte a többtényezős hitelesítés a B2B felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-130">For example, let’s say a Contoso user (Sally) is invited to Fabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="ec4a0-131">Ha Contoso többtényezős hitelesítési szabályzat az App1 számítógépen, de nem App2 engedélyezve van, majd ha úgy tekintünk, a Contoso MFA jogcím a jogkivonatot, előfordulhat, hogy látható a következő hibát:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at the Contoso MFA claim in the token, we might see the following issue:</span></span>

* <span data-ttu-id="ec4a0-132">1. napja: A felhasználó rendelkezik-e többtényezős hitelesítés Contoso és fér hozzá az App1, akkor nincs további MFA kérdés megjelenik-e a Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="ec4a0-133">2. napon: A felhasználó általi App 2 futnak, így most Fabrikam elérésekor, kell regisztrálnia az MFA van.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-133">Day 2: The user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="ec4a0-134">Ez a folyamat zavaró lehet, és előfordulhat, hogy a bejelentkezési befejezésekhez dobja el.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-134">This process can be confusing and could lead to drop in sign-in completions.</span></span>

<span data-ttu-id="ec4a0-135">Továbbá akkor is, ha a Contoso többtényezős hitelesítési funkció, nincs mindig az esetben a Fabrikam volna megbízik a Contoso többtényezős hitelesítési szabályzat.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-135">Moreover, even if Contoso has MFA capability, it is not always the case the Fabrikam would trust the Contoso MFA policy.</span></span>

<span data-ttu-id="ec4a0-136">Végezetül erőforrás bérlőt többtényezős Hitelesítést is működik, az msa-k és társadalombiztosítási azonosítókat és az erőforráspartner-szervezethez, amelyek nem rendelkeznek a többtényezős hitelesítés beállítása.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="ec4a0-137">A javaslat a multi-factor Authentication B2B felhasználók ezért mindig megkövetelő hívja fel a bérlőben.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-137">Therefore, the recommendation for MFA for B2B users is to always require MFA in the inviting tenant.</span></span> <span data-ttu-id="ec4a0-138">Ez a követelmény előfordulhat, hogy bizonyos esetekben dupla MFA, de hívja fel a bérlő fér hozzá, amikor a végfelhasználói élmény kiszámítható: Sally regisztrálnia kell a multi-factor Authentication hívja fel a bérlő.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-138">This requirement could lead to double MFA in some cases, but whenever accessing the inviting tenant, the end-users experience is predictable: Sally must register for MFA with the inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="ec4a0-139">Eszköz, helyét és kockázati-alapú feltételes hozzáférés a B2B felhasználók</span><span class="sxs-lookup"><span data-stu-id="ec4a0-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="ec4a0-140">Contoso lehetővé teszi, hogy a vállalati adatok eszközalapú feltételes hozzáférési házirendeket, ha hozzáférést nem lehetséges a, amelyek nem a Contoso által felügyelt és nem a Contoso eszköz házirendeknek megfelelő eszközökről.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with the Contoso device policies.</span></span>

<span data-ttu-id="ec4a0-141">Ha a B2B felhasználó-eszköz nem a Contoso felügyeli, a B2B felhasználókat az erőforráspartner-szervezetek az le van tiltva a abban a környezetben, ezek a házirendek érvényben vannak.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-141">If the B2B user’s device isn't managed by Contoso, access of B2B users from the partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="ec4a0-142">Contoso azonban, hogy kizárja őket az eszközalapú feltételes hozzáférési házirend adott partner felhasználók tartalmazó kizárási listák hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-142">However, Contoso can create exclusion lists containing specific partner users to exclude them from the device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="ec4a0-143">Feltételes hozzáférés helyalapú B2B</span><span class="sxs-lookup"><span data-stu-id="ec4a0-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="ec4a0-144">Helyalapú feltételes hozzáférési házirendek kényszerítheti a B2B felhasználók hívja fel a szervezet létre tudja hozni egy megbízható IP-címtartományt, amely meghatározza a fiókpartner-szervezetek esetén is.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-144">Location-based conditional access policies can be enforced for B2B users if the inviting organization is able to create a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="ec4a0-145">Kockázati-alapú feltételes hozzáférés a B2B</span><span class="sxs-lookup"><span data-stu-id="ec4a0-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="ec4a0-146">Jelenleg kockázati-alapú bejelentkezési házirendek nem alkalmazható B2B felhasználók mivel a kockázat kiértékelésekor a B2B felhasználó otthoni szervezet hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="ec4a0-146">Currently, risk-based sign-in policies cannot be applied to B2B users because the risk evaluation is performed at the B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec4a0-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec4a0-147">Next steps</span></span>

<span data-ttu-id="ec4a0-148">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="ec4a0-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ec4a0-149">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="ec4a0-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ec4a0-150">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="ec4a0-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="ec4a0-151">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="ec4a0-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="ec4a0-152">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="ec4a0-152">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="ec4a0-153">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="ec4a0-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="ec4a0-154">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="ec4a0-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="ec4a0-155">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="ec4a0-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="ec4a0-156">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="ec4a0-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="ec4a0-157">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="ec4a0-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="ec4a0-158">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="ec4a0-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="ec4a0-159">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="ec4a0-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
