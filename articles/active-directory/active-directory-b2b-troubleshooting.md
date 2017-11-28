---
title: "Azure Active Directory B2B együttműködés aaaTroubleshooting |} Microsoft Docs"
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
ms.openlocfilehash: 6fcfd7e543cd7bb833225f8aa56e332e7a989faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="1854a-103">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="1854a-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="1854a-104">Az alábbiakban néhány jogorvoslatok kapcsolatos Azure Active Directory (Azure AD) B2B együttműködés gyakori problémákat.</span><span class="sxs-lookup"><span data-stu-id="1854a-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a><span data-ttu-id="1854a-105">Tudom felvételét a külső felhasználó, de ne legyenek láthatóak a globális címlista vagy hello személyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="1854a-105">I’ve added an external user but do not see them in my Global Address Book or in hello people picker</span></span>

<span data-ttu-id="1854a-106">Azokban az esetekben, ahol a külső felhasználók nincsenek feltöltve adattal hello listában hello objektum eltarthat néhány percig tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="1854a-106">In cases where external users are not populated in hello list, hello object might take a few minutes tooreplicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="1854a-107">A B2B Vendég felhasználó nem látható a SharePoint Online/OneDrive személyek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="1854a-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="1854a-108">hello képességét toosearch a hello SharePoint Online (SPO) személyek kiválasztása a meglévő vendégfelhasználók alapértelmezett toomatch örökölt működése értéke OFF.</span><span class="sxs-lookup"><span data-stu-id="1854a-108">hello ability toosearch for existing guest users in hello SharePoint Online (SPO) people picker is OFF by default toomatch legacy behavior.</span></span>

<span data-ttu-id="1854a-109">Ez a szolgáltatás "ShowPeoplePickerSuggestionsForGuestUsers" szintű hello bérlő és a webhelycsoport beállítását hello használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="1854a-109">You can enable this feature by using hello setting 'ShowPeoplePickerSuggestionsForGuestUsers' at hello tenant and site collection level.</span></span> <span data-ttu-id="1854a-110">Beállíthatja a hello Set-SPOTenant és a Set-SPOSite-parancsmagok használatával, amelyek lehetővé teszik a tagok hello szolgáltatás toosearch hello címtárban szereplő összes meglévő Vendég felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1854a-110">You can set hello feature using hello Set-SPOTenant and Set-SPOSite cmdlets, which allow members toosearch all existing guest users in hello directory.</span></span> <span data-ttu-id="1854a-111">Hello bérlői hatókörhöz változásai nem befolyásolják a már kiépített SPO helyek.</span><span class="sxs-lookup"><span data-stu-id="1854a-111">Changes in hello tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="1854a-112">Könyvtár meghívókat le van tiltva</span><span class="sxs-lookup"><span data-stu-id="1854a-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="1854a-113">Ha értesítést kap, hogy nem rendelkezik engedélyekkel tooinvite felhasználók, győződjön meg arról, hogy a felhasználói fiók jogosult tooinvite külső felhasználók a felhasználói beállítások:</span><span class="sxs-lookup"><span data-stu-id="1854a-113">If you are notified that you do not have permissions tooinvite users, verify that your user account is authorized tooinvite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="1854a-114">Ha nemrég módosította ezeket a beállításokat, vagy hozzárendelt hello Vendég meghívó szerepkör tooa felhasználó, előfordulhat, a 15-60 perc késleltetéssel a hello módosítások érvénybe lépéséhez.</span><span class="sxs-lookup"><span data-stu-id="1854a-114">If you have recently modified these settings or assigned hello Guest Inviter role tooa user, there might be a 15-60 minute delay before hello changes take effect.</span></span>

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="1854a-115">I meghívott hello felhasználó hibaüzenetet kap érvényesítési során</span><span class="sxs-lookup"><span data-stu-id="1854a-115">hello user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="1854a-116">Gyakori hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="1854a-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="1854a-117">Meghívott rendszergazda nem engedélyezte a bérlőben jöjjön létre EmailVerified felhasználók</span><span class="sxs-lookup"><span data-stu-id="1854a-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="1854a-118">Amikor felhasználók, akiknek a szervezet által használt Azure Active Directory, de ha hello az adott felhasználói fiók nem létezik (például hello felhasználó nem létezik az Azure AD contoso.com).</span><span class="sxs-lookup"><span data-stu-id="1854a-118">When inviting users whose organization is using Azure Active Directory, but where hello specific user’s account does not exist (for example, hello user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="1854a-119">a contoso.com hello rendszergazda is tartalmaznak egy házirend meggátolja, hogy a felhasználók létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="1854a-119">hello administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="1854a-120">hello felhasználói kell egyeztetni a rendszergazda toodetermine, ha a külső felhasználók számára engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1854a-120">hello user must check with their admin toodetermine if external users are allowed.</span></span> <span data-ttu-id="1854a-121">hello külső felhasználó rendszergazda esetleg tooallow, e-mailek ellenőrzése a tartományukba tartozó felhasználók (Ez [cikk](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) az e-mailek ellenőrzése felhasználók).</span><span class="sxs-lookup"><span data-stu-id="1854a-121">hello external user’s admin may need tooallow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="1854a-122">Külső felhasználó nem létezik már egy összevont tartományban</span><span class="sxs-lookup"><span data-stu-id="1854a-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="1854a-123">Ha hello felhasználó már nem létezik az Azure Active Directory összevonási hitelesítést használ, hello felhasználót nem hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="1854a-123">If you are using federation authentication and hello user does not already exist in Azure Active Directory, hello user cannot be invited.</span></span>

<span data-ttu-id="1854a-124">a probléma hello tooresolve külső felhasználó rendszergazda hello felhasználói fiók tooAzure Active Directory kell szinkronizálnia.</span><span class="sxs-lookup"><span data-stu-id="1854a-124">tooresolve this issue, hello external user’s admin must synchronize hello user’s account tooAzure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="1854a-125">Hogyan biztosítja a\#", amely nincs általában érvénytelen karakter, és az Azure AD sync?</span><span class="sxs-lookup"><span data-stu-id="1854a-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="1854a-126">"\#" nem egy foglalt karakter UPN-EK az Azure AD B2B együttműködés vagy külső felhasználók számára, mert hello meghívott fiók user@contoso.com user_contoso.com# válikEXT@fabrikam.onmicrosoft.com. Ezért \# az UPN-EK a helyszíni érkező nem engedélyezett toosign toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1854a-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because hello invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com. Therefore, \# in UPNs coming from on-premises aren't allowed toosign in toohello Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a><span data-ttu-id="1854a-127">A megjelenő hibaüzenet szinkronizáláskor a külső felhasználók tooa hozzáadása csoporthoz</span><span class="sxs-lookup"><span data-stu-id="1854a-127">I receive an error when adding external users tooa synchronized group</span></span>

<span data-ttu-id="1854a-128">Külső felhasználók felveheti túl csak "hozzárendelt" vagy "Biztonság" csoportok és a nem a rendszer toogroups értékűre a helyszínen.</span><span class="sxs-lookup"><span data-stu-id="1854a-128">External users can be added only too“assigned” or “Security” groups and not toogroups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a><span data-ttu-id="1854a-129">A külső felhasználó nem kapta meg az e-mailek tooredeem</span><span class="sxs-lookup"><span data-stu-id="1854a-129">My external user did not receive an email tooredeem</span></span>

<span data-ttu-id="1854a-130">hello meghívott érdemes egyeztetni az internetszolgáltató által biztosított, vagy a levélszemét szűrő tooensure, amely a következő cím hello engedélyezve:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="1854a-130">hello invitee should check with their ISP or spam filter tooensure that hello following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="1854a-131">I figyelje meg, hogy egyéni üdvözlőüzenetére nem kérdezhető le a meghívó üzeneteket mellékelt néha</span><span class="sxs-lookup"><span data-stu-id="1854a-131">I notice that hello custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="1854a-132">az adatvédelmi törvényekkel toocomply, az API-k nem tartalmaznak egyéni üzenetek hello e-mail ajánlati során:</span><span class="sxs-lookup"><span data-stu-id="1854a-132">toocomply with privacy laws, our APIs do not include custom messages in hello email invitation when:</span></span>

- <span data-ttu-id="1854a-133">hello meghívó hello meghívása a bérlő nem rendelkezik egy e-mail címet</span><span class="sxs-lookup"><span data-stu-id="1854a-133">hello inviter doesn’t have an email address in hello inviting tenant</span></span>
- <span data-ttu-id="1854a-134">Amikor egy App Service principal küldi hello meghívó</span><span class="sxs-lookup"><span data-stu-id="1854a-134">When an appservice principal sends hello invitation</span></span>

<span data-ttu-id="1854a-135">Ha ebben a forgatókönyvben fontos tooyou, ne jelenjen meg többé az API-t meghívó e-mail, és küldje el az Ön által választott hello e-mail mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="1854a-135">If this scenario is important tooyou, you can suppress our API invitation email, and send it through hello email mechanism of your choice.</span></span> <span data-ttu-id="1854a-136">Tekintse meg a szervezet védőt toomake meg arról, hogy minden e-mailek küldésekor, így is adatvédelmi törvényekkel összhangban.</span><span class="sxs-lookup"><span data-stu-id="1854a-136">Consult your organization’s legal counsel toomake sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1854a-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1854a-137">Next steps</span></span>

<span data-ttu-id="1854a-138">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="1854a-138">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="1854a-139">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="1854a-139">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="1854a-140">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="1854a-140">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="1854a-141">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="1854a-141">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="1854a-142">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="1854a-142">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="1854a-143">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="1854a-143">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="1854a-144">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="1854a-144">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="1854a-145">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="1854a-145">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="1854a-146">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="1854a-146">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="1854a-147">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="1854a-147">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="1854a-148">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="1854a-148">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="1854a-149">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="1854a-149">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
