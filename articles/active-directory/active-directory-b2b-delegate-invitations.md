---
title: "az Azure Active Directory B2B együttműködés aaaDelegate meghívókat |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés felhasználó tulajdonságainak konfigurálható"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="6e830-103">Az Azure Active Directory B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="6e830-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="6e830-104">Az Azure Active Directory (Azure AD) üzleti vállalatközi (B2B) együttműködés nincs toobe egy globális rendszergazdai toosend meghívókat.</span><span class="sxs-lookup"><span data-stu-id="6e830-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have toobe a global admin toosend invitations.</span></span> <span data-ttu-id="6e830-105">Ehelyett házirendekkel, és amelynek szerepkörök lehetővé teszik toosend meghívókat meghívókat toousers delegálása.</span><span class="sxs-lookup"><span data-stu-id="6e830-105">Instead, you can use policies and delegate invitations toousers whose roles allow them toosend invitations.</span></span> <span data-ttu-id="6e830-106">Egy fontos új módon toodelegate Vendég felhasználó meghívókat hello Vendég meghívó szerepkör keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="6e830-106">An important new way toodelegate guest user invitations is through hello Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="6e830-107">Vendég meghívó szerepkör</span><span class="sxs-lookup"><span data-stu-id="6e830-107">Guest Inviter role</span></span>
<span data-ttu-id="6e830-108">Azt is hozzárendelheti hello felhasználói tooGuest meghívó szerepkör toosend meghívókat.</span><span class="sxs-lookup"><span data-stu-id="6e830-108">We can assign hello user tooGuest Inviter role toosend invitations.</span></span> <span data-ttu-id="6e830-109">Nincs hello globális rendszergazdai szerepkör toosend meghívókat toobe tagja.</span><span class="sxs-lookup"><span data-stu-id="6e830-109">You don't have toobe member of hello global admin role toosend invitations.</span></span> <span data-ttu-id="6e830-110">Alapértelmezés szerint rendszeres felhasználók is hívhat meg a meghívás API hello egy globális rendszergazdai tiltja le a rendszeres felhasználók meghívókat.</span><span class="sxs-lookup"><span data-stu-id="6e830-110">By default, regular users can also invoke hello invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="6e830-111">A felhasználó is hívhat meg hello API hello Azure-portálon vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="6e830-111">A user can also invoke hello API using hello Azure portal or PowerShell.</span></span>

<span data-ttu-id="6e830-112">Íme egy példa bemutatja, hogyan toouse PowerShell tooadd toohello Vendég meghívó felhasználói szerepkört:</span><span class="sxs-lookup"><span data-stu-id="6e830-112">Here's an example that shows how toouse PowerShell tooadd a user toohello Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="6e830-113">Személyek kérhetnek</span><span class="sxs-lookup"><span data-stu-id="6e830-113">Control who can invite</span></span>

![Vezérlő hogyan tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="6e830-115">Az Azure AD B2B együttműködés egy Bérlői rendszergazda következő meghívó házirendek hello állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="6e830-115">With Azure AD B2B collaboration, a tenant admin can set hello following invitation policies:</span></span>

- <span data-ttu-id="6e830-116">Kapcsolja ki a meghívókat</span><span class="sxs-lookup"><span data-stu-id="6e830-116">Turn off invitations</span></span>
- <span data-ttu-id="6e830-117">Csak a rendszergazdák és a hello Vendég meghívó szerepet betöltő felhasználók kérhetnek</span><span class="sxs-lookup"><span data-stu-id="6e830-117">Only admins and users in hello Guest Inviter role can invite</span></span>
- <span data-ttu-id="6e830-118">Rendszergazdák, a hello Vendég meghívó szerepkör és a tagjainak meghívása</span><span class="sxs-lookup"><span data-stu-id="6e830-118">Admins, hello Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="6e830-119">Minden felhasználó, beleértve a Vendégek, hívhat</span><span class="sxs-lookup"><span data-stu-id="6e830-119">All users, including guests, can invite</span></span>

<span data-ttu-id="6e830-120">Alapértelmezés szerint bérlők túl van beállítva 4.</span><span class="sxs-lookup"><span data-stu-id="6e830-120">By default, tenants are set too#4.</span></span> <span data-ttu-id="6e830-121">(Az összes olyan felhasználót, beleértve a Vendégek, B2B felhasználók kérhetnek.)</span><span class="sxs-lookup"><span data-stu-id="6e830-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e830-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e830-122">Next steps</span></span>

<span data-ttu-id="6e830-123">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="6e830-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="6e830-124">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="6e830-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="6e830-125">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6e830-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="6e830-126">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6e830-126">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="6e830-127">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="6e830-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="6e830-128">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="6e830-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="6e830-129">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e830-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="6e830-130">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="6e830-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="6e830-131">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="6e830-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="6e830-132">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="6e830-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="6e830-133">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="6e830-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
