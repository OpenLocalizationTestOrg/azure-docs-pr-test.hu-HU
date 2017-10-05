---
title: "Az Azure Active Directory B2B együttműködés meghívókat delegálása |} Microsoft Docs"
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
ms.openlocfilehash: 78613cc978b585a98d235245194c02371f7f3849
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="f8b1b-103">Az Azure Active Directory B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="f8b1b-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="f8b1b-104">Az Azure Active Directory (Azure AD) üzleti vállalatközi (B2B) együttműködés nincs-e a globális rendszergazda az meghívókat.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have to be a global admin to send invitations.</span></span> <span data-ttu-id="f8b1b-105">Ehelyett házirendekkel, és delegálja a felhasználók számára, akiknek szerepkörök engedélyezése meghívókat meghívókat.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-105">Instead, you can use policies and delegate invitations to users whose roles allow them to send invitations.</span></span> <span data-ttu-id="f8b1b-106">Új módja a Vendég felhasználói meghívókat delegálása van a Vendég meghívó szerepkörével.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-106">An important new way to delegate guest user invitations is through the Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="f8b1b-107">Vendég meghívó szerepkör</span><span class="sxs-lookup"><span data-stu-id="f8b1b-107">Guest Inviter role</span></span>
<span data-ttu-id="f8b1b-108">A felhasználó hozzárendelése azt a Vendég meghívó szerepkör meghívókat.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-108">We can assign the user to Guest Inviter role to send invitations.</span></span> <span data-ttu-id="f8b1b-109">Ne kelljen meghívókat küldhet a globális rendszergazdai szerepkör tagja lehet.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-109">You don't have to be member of the global admin role to send invitations.</span></span> <span data-ttu-id="f8b1b-110">Alapértelmezés szerint rendszeres felhasználók is hívhat meg a meghívás API egy globális rendszergazdai tiltja le a rendszeres felhasználók meghívókat.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-110">By default, regular users can also invoke the invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="f8b1b-111">A felhasználó is hívhat meg az API-t az Azure-portálon vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-111">A user can also invoke the API using the Azure portal or PowerShell.</span></span>

<span data-ttu-id="f8b1b-112">Íme egy példa, amely bemutatja, hogyan lehet hozzáadni egy felhasználót a Vendég meghívó szerepkör a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="f8b1b-112">Here's an example that shows how to use PowerShell to add a user to the Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="f8b1b-113">Személyek kérhetnek</span><span class="sxs-lookup"><span data-stu-id="f8b1b-113">Control who can invite</span></span>

![Szabályozza, hogyan hívhat meg](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="f8b1b-115">Az Azure AD B2B együttműködés egy Bérlői rendszergazda állíthatja be a következő meghívó házirendek:</span><span class="sxs-lookup"><span data-stu-id="f8b1b-115">With Azure AD B2B collaboration, a tenant admin can set the following invitation policies:</span></span>

- <span data-ttu-id="f8b1b-116">Kapcsolja ki a meghívókat</span><span class="sxs-lookup"><span data-stu-id="f8b1b-116">Turn off invitations</span></span>
- <span data-ttu-id="f8b1b-117">Csak rendszergazdák és a Vendég meghívó a szerepkörben levő felhasználók kérhetnek</span><span class="sxs-lookup"><span data-stu-id="f8b1b-117">Only admins and users in the Guest Inviter role can invite</span></span>
- <span data-ttu-id="f8b1b-118">Rendszergazdák, a Vendég meghívó szerepkör és a tagjainak meghívása</span><span class="sxs-lookup"><span data-stu-id="f8b1b-118">Admins, the Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="f8b1b-119">Minden felhasználó, beleértve a Vendégek, hívhat</span><span class="sxs-lookup"><span data-stu-id="f8b1b-119">All users, including guests, can invite</span></span>

<span data-ttu-id="f8b1b-120">Alapértelmezés szerint a bérlők #4 vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="f8b1b-120">By default, tenants are set to #4.</span></span> <span data-ttu-id="f8b1b-121">(Az összes olyan felhasználót, beleértve a Vendégek, B2B felhasználók kérhetnek.)</span><span class="sxs-lookup"><span data-stu-id="f8b1b-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8b1b-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8b1b-122">Next steps</span></span>

<span data-ttu-id="f8b1b-123">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="f8b1b-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="f8b1b-124">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="f8b1b-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="f8b1b-125">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f8b1b-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="f8b1b-126">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f8b1b-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="f8b1b-127">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="f8b1b-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="f8b1b-128">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="f8b1b-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="f8b1b-129">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8b1b-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="f8b1b-130">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="f8b1b-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="f8b1b-131">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="f8b1b-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="f8b1b-132">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="f8b1b-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="f8b1b-133">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="f8b1b-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
