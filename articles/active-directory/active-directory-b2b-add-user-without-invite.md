---
title: "Azure Active Directory B2B együttműködés felhasználók hozzáadása nélkül |} Microsoft Docs"
description: "Beállítható, hogy a Vendég felhasználó más vendég felhasználók hozzáadása az Azure ad az Azure Active Directory B2B együttműködés meghívót gyűjtésével nélkül."
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 91b9477cdb679851e7d8d2942c06999a05f64e46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="42a66-103">Adja hozzá a B2B együttműködés vendégfelhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="42a66-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="42a66-104">Egy felhasználó, például a partner képviselője adhat hozzá felhasználókat a partnerről a szervezet felhívás váltható anélkül engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="42a66-104">You can allow a user, such as a partner representative, to add users from the partner to your organization without needing invitations to be redeemed.</span></span> <span data-ttu-id="42a66-105">El kell végeznie jogosultságokat, hogy a felhasználó számbavételi használata a partnerrel. szervezeti a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="42a66-105">All you must do is grant that user enumeration privileges in the directory you're using for the partner org.</span></span> 

<span data-ttu-id="42a66-106">Megadja, hogy ezek a jogosultságok mikor:</span><span class="sxs-lookup"><span data-stu-id="42a66-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="42a66-107">A gazdagép szervezetében (például WoodGrove) a felhasználó meghívja a fiókpartner-szervezet egy felhasználót (például Sam@litware.com) vendégként.</span><span class="sxs-lookup"><span data-stu-id="42a66-107">A user in the host organization (for example, WoodGrove) invites one user from the partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="42a66-108">A rendszergazda a gazdagép szervezet Sam azonosíthatja és más felhasználók felvétele az erőforráspartner-szervezet (Litware) házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="42a66-108">The admin in the host organization sets up policies that allow Sam to identify and add other users from the partner organization (Litware).</span></span>
3. <span data-ttu-id="42a66-109">Most Sam származó is hozzáadhat más felhasználók Litware a WoodGrove könyvtár, csoportok vagy alkalmazások felhívás váltható anélkül.</span><span class="sxs-lookup"><span data-stu-id="42a66-109">Now Sam can add other users from Litware to the WoodGrove directory, groups, or applications without needing invitations to be redeemed.</span></span> <span data-ttu-id="42a66-110">Ha a Sam Litware a számbavételi megfelelő jogosultságokkal rendelkezik, akkor automatikusan megtörténik.</span><span class="sxs-lookup"><span data-stu-id="42a66-110">If Sam has the appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="42a66-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42a66-111">Next steps</span></span>

<span data-ttu-id="42a66-112">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="42a66-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="42a66-113">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="42a66-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="42a66-114">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="42a66-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="42a66-115">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="42a66-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="42a66-116">A B2B együttműködés meghívó e-mail elemei</span><span class="sxs-lookup"><span data-stu-id="42a66-116">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="42a66-117">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="42a66-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="42a66-118">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="42a66-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="42a66-119">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="42a66-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="42a66-120">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="42a66-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="42a66-121">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="42a66-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="42a66-122">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="42a66-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="42a66-123">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="42a66-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)