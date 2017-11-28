---
title: "Új felhasználók hozzáadása az Azure Active Directoryhoz | Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat, vagy hogyan módosíthatja a felhasználói adatokat az Azure Active Directoryban."
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
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a><span data-ttu-id="a96f5-103">Új felhasználók hozzáadása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="a96f5-103">Add new users to Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a96f5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a96f5-104">Azure portal</span></span>](active-directory-users-create-azure-portal.md)
> * [<span data-ttu-id="a96f5-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="a96f5-105">Azure classic portal</span></span>](active-directory-create-users.md)
>
>

<span data-ttu-id="a96f5-106">Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat a szervezet az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a96f5-106">This article explains how to add new users in your organization in the Azure Active Directory (Azure AD).</span></span> 

1. <span data-ttu-id="a96f5-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="a96f5-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="a96f5-108">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="a96f5-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók és csoportok](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="a96f5-110">Az a **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a96f5-110">On the **Users and groups** blade, select **All users**, and then select **Add**.</span></span>

   ![Az Add parancs kiválasztása](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. <span data-ttu-id="a96f5-112">Adja meg a felhasználó, például a **neve** és **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="a96f5-112">Enter details for the user, such as **Name** and **User name**.</span></span> <span data-ttu-id="a96f5-113">A tartomány a felhasználónév részét kell lennie, a kezdeti alapértelmezett tartomány neve "foo.onmicrosoft.com" tartomány nevét, vagy egy nem összevont ellenőrzött tartomány nevét, például "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="a96f5-113">The domain name portion of the user name must either be the initial default domain name "foo.onmicrosoft.com" domain name, or a verified, non-federated domain name such as "contoso.com."</span></span>
5. <span data-ttu-id="a96f5-114">Másolja, vagy ellenkező esetben vegye figyelembe a létrehozott felhasználói jelszót, így megadhatja azt a felhasználó számára a folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="a96f5-114">Copy or otherwise note the generated user password so that you can provide it to the user after this process is complete.</span></span>
6. <span data-ttu-id="a96f5-115">Másik lehetőségként megnyithatja és töltse ki a **profil** panelen a **csoportok** panelen, vagy a **címtár szerepkörrel** panel a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a96f5-115">Optionally, you can open and fill out the information in the **Profile** blade, the **Groups** blade, or the **Directory role** blade for the user.</span></span> <span data-ttu-id="a96f5-116">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a96f5-116">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="a96f5-117">Az a **felhasználói** panelen válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a96f5-117">On the **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="a96f5-118">Az új felhasználó számára létrehozott jelszót biztonságos terjesztése, hogy a felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="a96f5-118">Securely distribute the generated password to the new user so that the user can sign in.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a96f5-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a96f5-119">Next steps</span></span>
* [<span data-ttu-id="a96f5-120">Külső felhasználó hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="a96f5-120">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)
* [<span data-ttu-id="a96f5-121">A felhasználó jelszavát az új Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a96f5-121">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="a96f5-122">A felhasználó munkahelyi adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="a96f5-122">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="a96f5-123">Felhasználói profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="a96f5-123">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="a96f5-124">Felhasználó törlése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a96f5-124">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)
* [<span data-ttu-id="a96f5-125">Felhasználó hozzárendelése egy szerepkörhöz az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a96f5-125">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
