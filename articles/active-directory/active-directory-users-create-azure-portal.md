---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók, vagy módosítsa a felhasználói adatokat az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: c4a156ea31b81202bb0d0ac224afbfc3f1785532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-tooazure-active-directory"></a><span data-ttu-id="fd282-103">Adja hozzá az új felhasználók tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fd282-103">Add new users tooAzure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd282-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fd282-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="fd282-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="fd282-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="fd282-106">Ez a cikk azt ismerteti, hogyan tooadd új felhasználók az Ön szervezetében hello Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fd282-106">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="fd282-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="fd282-107">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="fd282-108">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="fd282-108">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók és csoportok](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="fd282-110">A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fd282-110">On hello **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Hello hozzáadása paranccsal](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="fd282-112">Adja meg például a részleteit hello felhasználói **neve** és **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="fd282-112">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="fd282-113">hello tartomány neve hello felhasználónév részét kell hello kezdeti alapértelmezett tartomány neve "foo.onmicrosoft.com" tartomány nevét, vagy egy nem összevont ellenőrzött tartomány nevét, például "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="fd282-113">hello domain name portion of hello user name must either be hello initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="fd282-114">Másolat vagy más módon Megjegyzés hello által létrehozott felhasználói jelszó, így megadhatja azt toohello felhasználói a folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="fd282-114">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="fd282-115">Igény szerint megnyitásához, és a hello hello adatok **profil** panelen, hello **csoportok** panel vagy hello **Directory szerepkör** hello felhasználói paneljét.</span><span class="sxs-lookup"><span data-stu-id="fd282-115">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="fd282-116">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="fd282-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="fd282-117">A hello **felhasználói** panelen válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fd282-117">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="fd282-118">Biztonságosan elosztani a generált hello jelszó toohello új felhasználó, így hello felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="fd282-118">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="fd282-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd282-119">Next steps</span></span>
* [<span data-ttu-id="fd282-120">Külső felhasználó hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="fd282-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="fd282-121">A felhasználó jelszavát hello új Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fd282-121">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="fd282-122">A felhasználó munkahelyi adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="fd282-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="fd282-123">Felhasználói profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="fd282-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="fd282-124">Felhasználó törlése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fd282-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="fd282-125">Egy felhasználó tooa szerepkör hozzárendelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fd282-125">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
