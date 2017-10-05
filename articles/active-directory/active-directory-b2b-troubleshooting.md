---
title: "Hibaelhárítás az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Jogorvoslatok Azure Active Directory B2B együttműködés kapcsolatos általános problémák"
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
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 2009cfc956a2703e268c9364996aa2d0fbd8f279
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="f59d0-103">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="f59d0-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="f59d0-104">Az alábbiakban néhány jogorvoslatok kapcsolatos Azure Active Directory (Azure AD) B2B együttműködés gyakori problémákat.</span><span class="sxs-lookup"><span data-stu-id="f59d0-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a><span data-ttu-id="f59d0-105">Tudom felvételét a külső felhasználó, de ne legyenek láthatóak a globális címlista vagy a személyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f59d0-105">I’ve added an external user but do not see them in my Global Address Book or in the people picker</span></span>

<span data-ttu-id="f59d0-106">Azokban az esetekben, ahol a külső felhasználók nem kerül a listában az objektum percet is igénybe vehet néhány replikálásához.</span><span class="sxs-lookup"><span data-stu-id="f59d0-106">In cases where external users are not populated in the list, the object might take a few minutes to replicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="f59d0-107">A B2B Vendég felhasználó nem látható a SharePoint Online/OneDrive személyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f59d0-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="f59d0-108">Keresse meg a SharePoint Online (SPO) személyek kiválasztása a meglévő vendégfelhasználók teszi örökölt értéke alapértelmezés szerint értéke OFF.</span><span class="sxs-lookup"><span data-stu-id="f59d0-108">The ability to search for existing guest users in the SharePoint Online (SPO) people picker is OFF by default to match legacy behavior.</span></span>

<span data-ttu-id="f59d0-109">Ez a funkció a bérlői és a hely gyűjtemény szintjén "ShowPeoplePickerSuggestionsForGuestUsers" beállítás használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="f59d0-109">You can enable this feature by using the setting 'ShowPeoplePickerSuggestionsForGuestUsers' at the tenant and site collection level.</span></span> <span data-ttu-id="f59d0-110">Beállíthatja, hogy a funkciót, a Set-SPOTenant és a Set-SPOSite parancsmagok, amelyek lehetővé teszik az összes meglévő vendég felhasználók kereséséhez a könyvtárban tagok használatával.</span><span class="sxs-lookup"><span data-stu-id="f59d0-110">You can set the feature using the Set-SPOTenant and Set-SPOSite cmdlets, which allow members to search all existing guest users in the directory.</span></span> <span data-ttu-id="f59d0-111">A bérlői hatókört változásai nem befolyásolják a már kiépített SPO helyek.</span><span class="sxs-lookup"><span data-stu-id="f59d0-111">Changes in the tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="f59d0-112">Könyvtár meghívókat le van tiltva</span><span class="sxs-lookup"><span data-stu-id="f59d0-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="f59d0-113">Ha Ön nem jogosult a meghívott felhasználóknak akkor kapnak értesítést, győződjön meg arról, hogy a felhasználói fiók engedélyezve van-e a külső felhasználók a felhasználói beállítások szakasz meghívott:</span><span class="sxs-lookup"><span data-stu-id="f59d0-113">If you are notified that you do not have permissions to invite users, verify that your user account is authorized to invite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="f59d0-114">Ha nemrég módosította ezeket a beállításokat, vagy a Vendég meghívó szerepkört a felhasználóhoz rendelt, előfordulhat, a 15-60 perc késleltetéssel a módosítások érvénybe lépéséhez.</span><span class="sxs-lookup"><span data-stu-id="f59d0-114">If you have recently modified these settings or assigned the Guest Inviter role to a user, there might be a 15-60 minute delay before the changes take effect.</span></span>

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="f59d0-115">A felhasználót, hogy I meghívott hiba fogad érvényesítési során</span><span class="sxs-lookup"><span data-stu-id="f59d0-115">The user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="f59d0-116">Gyakori hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="f59d0-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="f59d0-117">Meghívott rendszergazda nem engedélyezte a bérlőben jöjjön létre EmailVerified felhasználók</span><span class="sxs-lookup"><span data-stu-id="f59d0-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="f59d0-118">Amikor felhasználókat fióknevet, amelynek a szervezet által használt Azure Active Directory, de ha az adott felhasználói fiók nem létezik (például a felhasználó nem létezik az Azure AD contoso.com).</span><span class="sxs-lookup"><span data-stu-id="f59d0-118">When inviting users whose organization is using Azure Active Directory, but where the specific user’s account does not exist (for example, the user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="f59d0-119">A rendszergazda a contoso.com is tartalmaznak egy házirend meggátolja, hogy a felhasználók létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="f59d0-119">The administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="f59d0-120">A felhasználó ellenőriznie kell meghatározni, ha a külső felhasználók számára engedélyezett-e a rendszergazdától.</span><span class="sxs-lookup"><span data-stu-id="f59d0-120">The user must check with their admin to determine if external users are allowed.</span></span> <span data-ttu-id="f59d0-121">A külső felhasználó rendszergazda is engedélyeznie kell a tartomány felhasználói e-mailek ellenőrzése (Ez [cikk](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) az e-mailek ellenőrzése felhasználók).</span><span class="sxs-lookup"><span data-stu-id="f59d0-121">The external user’s admin may need to allow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="f59d0-122">Külső felhasználó nem létezik már egy összevont tartományban</span><span class="sxs-lookup"><span data-stu-id="f59d0-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="f59d0-123">Ha a felhasználó már nem létezik az Azure Active Directory összevonási hitelesítést használ, a felhasználót nem hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="f59d0-123">If you are using federation authentication and the user does not already exist in Azure Active Directory, the user cannot be invited.</span></span>

<span data-ttu-id="f59d0-124">A probléma megoldásához, a külső felhasználó rendszergazda szinkronizálnia kell a felhasználói fiók az Azure Active Directoryhoz.</span><span class="sxs-lookup"><span data-stu-id="f59d0-124">To resolve this issue, the external user’s admin must synchronize the user’s account to Azure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="f59d0-125">Hogyan biztosítja a\#", amely nincs általában érvénytelen karakter, és az Azure AD sync?</span><span class="sxs-lookup"><span data-stu-id="f59d0-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="f59d0-126">"\#" nem egy foglalt karakter UPN-EK az Azure AD B2B együttműködés vagy külső felhasználók számára, mert a meghívott fiók user@contoso.com user_contoso.com# válikEXT@fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="f59d0-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because the invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com.</span></span> <span data-ttu-id="f59d0-127">Ezért \# az UPN-EK a helyszíni érkező nem engedélyezett az Azure-portálon bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="f59d0-127">Therefore, \# in UPNs coming from on-premises aren't allowed to sign in to the Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a><span data-ttu-id="f59d0-128">Ha a külső felhasználók felvétele egy szinkronizált csoportot hibaüzenetet kap</span><span class="sxs-lookup"><span data-stu-id="f59d0-128">I receive an error when adding external users to a synchronized group</span></span>

<span data-ttu-id="f59d0-129">Külső felhasználók csak "hozzárendelt" vagy "Biztonság" csoportok és nem a csoportokat, amelyek a helyszíni lemezkép formátumú felveheti.</span><span class="sxs-lookup"><span data-stu-id="f59d0-129">External users can be added only to “assigned” or “Security” groups and not to groups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a><span data-ttu-id="f59d0-130">A külső felhasználó nem kapta meg a beváltani e-mailt</span><span class="sxs-lookup"><span data-stu-id="f59d0-130">My external user did not receive an email to redeem</span></span>

<span data-ttu-id="f59d0-131">A meghívott érdemes egyeztetni az internetszolgáltató által biztosított és a levélszemét szűrő annak ellenőrzéséhez, hogy engedélyezett-e a következő címre:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="f59d0-131">The invitee should check with their ISP or spam filter to ensure that the following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="f59d0-132">I figyelje meg, hogy az egyéni üzenet nem kérdezhető le a meghívó üzeneteket mellékelt néha</span><span class="sxs-lookup"><span data-stu-id="f59d0-132">I notice that the custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="f59d0-133">Adatvédelmi törvényekkel ahhoz, hogy az API-k nem tartalmaznak egyéni üzenetek az e-mailek ajánlati során:</span><span class="sxs-lookup"><span data-stu-id="f59d0-133">To comply with privacy laws, our APIs do not include custom messages in the email invitation when:</span></span>

- <span data-ttu-id="f59d0-134">A meghívó hívja fel a bérlő nem rendelkezik egy e-mail címet</span><span class="sxs-lookup"><span data-stu-id="f59d0-134">The inviter doesn’t have an email address in the inviting tenant</span></span>
- <span data-ttu-id="f59d0-135">Amikor egy App Service principal elküldi a meghívó</span><span class="sxs-lookup"><span data-stu-id="f59d0-135">When an appservice principal sends the invitation</span></span>

<span data-ttu-id="f59d0-136">Ebben a forgatókönyvben az Ön számára fontosak, ha az API-t meghívó e-mail mellőzése, és küldje el az e-mailek mechanizmus az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="f59d0-136">If this scenario is important to you, you can suppress our API invitation email, and send it through the email mechanism of your choice.</span></span> <span data-ttu-id="f59d0-137">Tekintse meg a szervezet védőt való győződjön meg arról, hogy minden e-mailek küldésekor, így is adatvédelmi törvényekkel összhangban.</span><span class="sxs-lookup"><span data-stu-id="f59d0-137">Consult your organization’s legal counsel to make sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f59d0-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f59d0-138">Next steps</span></span>

<span data-ttu-id="f59d0-139">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="f59d0-139">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="f59d0-140">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="f59d0-140">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="f59d0-141">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="f59d0-141">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="f59d0-142">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="f59d0-142">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="f59d0-143">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="f59d0-143">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="f59d0-144">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="f59d0-144">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="f59d0-145">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="f59d0-145">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="f59d0-146">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="f59d0-146">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="f59d0-147">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="f59d0-147">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="f59d0-148">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="f59d0-148">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="f59d0-149">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="f59d0-149">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="f59d0-150">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="f59d0-150">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
