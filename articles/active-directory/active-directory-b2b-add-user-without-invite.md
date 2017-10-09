---
title: "aaaAdd B2B együttműködés felhasználók tooAzure Active Directory nélkül |} Microsoft Docs"
description: "Beállítható, hogy a Vendég felhasználó más vendég felhasználók tooyour az Azure AD hozzáadni anélkül, hogy az Azure Active Directory B2B együttműködés meghívót váltja be."
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
ms.openlocfilehash: 459d99b9f856a35973d1b2cbfabdc23fe40c8f44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="6ed65-103">Adja hozzá a B2B együttműködés vendégfelhasználók nélkül</span><span class="sxs-lookup"><span data-stu-id="6ed65-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="6ed65-104">Engedélyezheti, hogy egy felhasználó, például egy partner reprezentatív, tooadd felhasználók tooyour partnerszervezetnek hello anélkül, hogy meghívókat toobe sikerült beváltani.</span><span class="sxs-lookup"><span data-stu-id="6ed65-104">You can allow a user, such as a partner representative, tooadd users from hello partner tooyour organization without needing invitations toobe redeemed.</span></span> <span data-ttu-id="6ed65-105">El kell végeznie jogosultságokat, hogy a felhasználó számbavételi hello partner. szervezeti tárolására használni kívánt hello könyvtárban</span><span class="sxs-lookup"><span data-stu-id="6ed65-105">All you must do is grant that user enumeration privileges in hello directory you're using for hello partner org.</span></span> 

<span data-ttu-id="6ed65-106">Megadja, hogy ezek a jogosultságok mikor:</span><span class="sxs-lookup"><span data-stu-id="6ed65-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="6ed65-107">Hello állomás szervezetben (például WoodGrove) a felhasználó meghívja a hello fiókpartner-szervezet egy felhasználói (például Sam@litware.com) vendégként.</span><span class="sxs-lookup"><span data-stu-id="6ed65-107">A user in hello host organization (for example, WoodGrove) invites one user from hello partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="6ed65-108">Üdvözöljük a rendszergazdákat hello állomás szervezet Sam tooidentify engedélyezése és más felhasználók felvétele hello fiókpartner-szervezet (Litware) házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="6ed65-108">hello admin in hello host organization sets up policies that allow Sam tooidentify and add other users from hello partner organization (Litware).</span></span>
3. <span data-ttu-id="6ed65-109">Most Sam adhat hozzá más felhasználók Litware toohello WoodGrove könyvtár, csoportok vagy alkalmazások anélkül, hogy meghívókat toobe sikerült beváltani.</span><span class="sxs-lookup"><span data-stu-id="6ed65-109">Now Sam can add other users from Litware toohello WoodGrove directory, groups, or applications without needing invitations toobe redeemed.</span></span> <span data-ttu-id="6ed65-110">Ha a Sam Litware hello számbavételi megfelelő jogosultságokkal rendelkezik, akkor automatikusan megtörténik.</span><span class="sxs-lookup"><span data-stu-id="6ed65-110">If Sam has hello appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6ed65-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ed65-111">Next steps</span></span>

<span data-ttu-id="6ed65-112">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="6ed65-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="6ed65-113">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="6ed65-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="6ed65-114">Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?</span><span class="sxs-lookup"><span data-stu-id="6ed65-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="6ed65-115">Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?</span><span class="sxs-lookup"><span data-stu-id="6ed65-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="6ed65-116">hello B2B együttműködés meghívó e-mail hello elemei</span><span class="sxs-lookup"><span data-stu-id="6ed65-116">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="6ed65-117">B2B együttműködés meghívó érvényesítési</span><span class="sxs-lookup"><span data-stu-id="6ed65-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="6ed65-118">Az Azure AD B2B együttműködés licencelés</span><span class="sxs-lookup"><span data-stu-id="6ed65-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="6ed65-119">Hibaelhárítás az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="6ed65-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="6ed65-120">Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="6ed65-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="6ed65-121">Az Azure Active Directory B2B együttműködés API és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="6ed65-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="6ed65-122">Többtényezős hitelesítés a B2B-együttműködés felhasználói számára</span><span class="sxs-lookup"><span data-stu-id="6ed65-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="6ed65-123">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="6ed65-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)