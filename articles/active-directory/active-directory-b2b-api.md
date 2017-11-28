---
title: "aaaAzure Active Directory B2B együttműködés API és a Testreszabás |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés a vállalatokon átívelő kapcsolatok üzleti partnerek tooselectively hozzáférés engedélyezése a vállalati alkalmazásokat támogatja."
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
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a><span data-ttu-id="f092f-103">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="f092f-103">Azure Active Directory B2B collaboration API and customization</span></span>

<span data-ttu-id="f092f-104">Adja meg, hogy meg kívánják-toocustomize hello meghívó folyamat úgy, hogy működik a legjobban, a szervezeteknek számos ügyfél volt.</span><span class="sxs-lookup"><span data-stu-id="f092f-104">We've had many customers tell us that they want toocustomize hello invitation process in a way that works best for their organizations.</span></span> <span data-ttu-id="f092f-105">Az API-val most, hogy elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="f092f-105">With our API, you can do just that.</span></span> [<span data-ttu-id="f092f-106">https://Developer.microsoft.com/Graph/docs/API-Reference/V1.0/Resources/invitation</span><span class="sxs-lookup"><span data-stu-id="f092f-106">https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation</span></span>](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a><span data-ttu-id="f092f-107">Hello meghívó API képességei</span><span class="sxs-lookup"><span data-stu-id="f092f-107">Capabilities of hello invitation API</span></span>
<span data-ttu-id="f092f-108">hello API hello a következő lehetőségeket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="f092f-108">hello API offers hello following capabilities:</span></span>

1. <span data-ttu-id="f092f-109">A külső felhasználó meghívása *bármely* e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="f092f-109">Invite an external user with *any* email address.</span></span>

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. <span data-ttu-id="f092f-110">Testre szabhatja, ahová a felhasználók tooland azok elfogadja a meghívót.</span><span class="sxs-lookup"><span data-stu-id="f092f-110">Customize where you want your users tooland after they accept their invitation.</span></span>

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. <span data-ttu-id="f092f-111">Válassza ki a toosend hello szabványos meghívó e-mail velünk keresztül</span><span class="sxs-lookup"><span data-stu-id="f092f-111">Choose toosend hello standard invitation mail through us</span></span>

    ```
    "sendInvitationMessage": true
    ```

  <span data-ttu-id="f092f-112">egy üzenet toohello címzettel testre szabható</span><span class="sxs-lookup"><span data-stu-id="f092f-112">with a message toohello recipient that you can customize</span></span>

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. <span data-ttu-id="f092f-113">Válassza a toocc: személyek tookeep hello a kapcsolatos a fióknevet a közreműködő hurok.</span><span class="sxs-lookup"><span data-stu-id="f092f-113">And choose toocc: people you want tookeep in hello loop about your inviting this collaborator.</span></span>

5. <span data-ttu-id="f092f-114">Vagy teljesen testre szabhatja a meghívó és a bevezetési munkafolyamat nem toosend értesítések az Azure AD keresztül kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="f092f-114">Or completely customize your invitation and onboarding workflow by choosing not toosend notifications through Azure AD.</span></span>

    ```
    "sendInvitationMessage": false
    ```

  <span data-ttu-id="f092f-115">Ebben az esetben vissza érvényesítési URL-címet a hello API, amely beágyazása e-mail sablont, IM vagy más elosztási módszer az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="f092f-115">In this case, you get back a redemption URL from hello API that you can embed in an email template, IM, or other distribution method of your choice.</span></span>

6. <span data-ttu-id="f092f-116">Végül ha Ön rendszergazda, kiválaszthatja tooinvite hello felhasználó tagja.</span><span class="sxs-lookup"><span data-stu-id="f092f-116">Finally, if you are an admin, you can choose tooinvite hello user as member.</span></span>

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a><span data-ttu-id="f092f-117">Engedélyezési modellt</span><span class="sxs-lookup"><span data-stu-id="f092f-117">Authorization model</span></span>
<span data-ttu-id="f092f-118">a következő hitelesítési módok hello hello API is futtatható:</span><span class="sxs-lookup"><span data-stu-id="f092f-118">hello API can be run in hello following authorization modes:</span></span>

### <a name="app--user-mode"></a><span data-ttu-id="f092f-119">Alkalmazás + felhasználói mód</span><span class="sxs-lookup"><span data-stu-id="f092f-119">App + User mode</span></span>
<span data-ttu-id="f092f-120">Ebben a módban személy, aki által használt hello API igények toohave hello engedélyek toobe létrehozása B2B meghívókat.</span><span class="sxs-lookup"><span data-stu-id="f092f-120">In this mode, whoever is using hello API needs toohave hello permissions toobe create B2B invitations.</span></span>

### <a name="app-only-mode"></a><span data-ttu-id="f092f-121">Egyetlen módja</span><span class="sxs-lookup"><span data-stu-id="f092f-121">App only mode</span></span>
<span data-ttu-id="f092f-122">Alkalmazás csak a környezetben a hello meghívó toosucceed hello User.ReadWrite.All vagy Directory.ReadWrite.All hatókört kell hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f092f-122">In app only context, hello app needs hello User.ReadWrite.All or Directory.ReadWrite.All scopes for hello invitation toosucceed.</span></span>

<span data-ttu-id="f092f-123">További információkért lásd: https://graph.microsoft.io/docs/authorization/permission_scopes</span><span class="sxs-lookup"><span data-stu-id="f092f-123">For more information, refer to: https://graph.microsoft.io/docs/authorization/permission_scopes</span></span>


## <a name="powershell"></a><span data-ttu-id="f092f-124">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f092f-124">PowerShell</span></span>
<span data-ttu-id="f092f-125">Most már lehetséges toouse PowerShell tooadd és a meghívás külső felhasználók tooan szervezet könnyen is.</span><span class="sxs-lookup"><span data-stu-id="f092f-125">It is now possible toouse PowerShell tooadd and invite external users tooan organization easily.</span></span> <span data-ttu-id="f092f-126">Hello parancsmaggal meghívót létrehozni:</span><span class="sxs-lookup"><span data-stu-id="f092f-126">Create an invitation using hello cmdlet:</span></span>

```
New-AzureADMSInvitation
```

<span data-ttu-id="f092f-127">Használhatja az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="f092f-127">You can use hello following options:</span></span>

* <span data-ttu-id="f092f-128">-InvitedUserDisplayName</span><span class="sxs-lookup"><span data-stu-id="f092f-128">-InvitedUserDisplayName</span></span>
* <span data-ttu-id="f092f-129">-InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="f092f-129">-InvitedUserEmailAddress</span></span>
* <span data-ttu-id="f092f-130">-SendInvitationMessage</span><span class="sxs-lookup"><span data-stu-id="f092f-130">-SendInvitationMessage</span></span>
* <span data-ttu-id="f092f-131">-InvitedUserMessageInfo</span><span class="sxs-lookup"><span data-stu-id="f092f-131">-InvitedUserMessageInfo</span></span>

<span data-ttu-id="f092f-132">Is megtekintheti hello meghívó API-hivatkozás [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span><span class="sxs-lookup"><span data-stu-id="f092f-132">You can also check out hello invitation API reference in [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f092f-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f092f-133">Next steps</span></span>

<span data-ttu-id="f092f-134">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="f092f-134">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="f092f-135">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="f092f-135">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="f092f-136">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="f092f-136">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="f092f-137">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="f092f-137">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="f092f-138">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="f092f-138">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="f092f-139">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="f092f-139">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="f092f-140">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="f092f-140">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="f092f-141">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="f092f-141">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="f092f-142">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="f092f-142">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="f092f-143">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="f092f-143">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="f092f-144">Adja hozzá a B2B együttműködés felhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="f092f-144">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="f092f-145">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="f092f-145">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
