---
title: "Új felhasználók hozzáadása az Azure Active Directoryhoz | Microsoft Docs"
description: "Ismerteti, hogyan vehet fel új felhasználókat az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 13a7d2d3b991206c45e66872b590bc27a224eead
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a><span data-ttu-id="a2d25-103">Gyors üzembe helyezés: Új felhasználók hozzáadása az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="a2d25-103">Quickstart: Add new users to Azure Active Directory</span></span>
<span data-ttu-id="a2d25-104">Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat a szervezet az Azure Active Directory (Azure AD) egyik az Azure portál használatával egyszerre, vagy a helyi Windows Server AD felhasználói fiók adatok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="a2d25-104">This article explains how to add new users in your organization in the Azure Active Directory (Azure AD) one at a time using the Azure portal or by synchronizing your on-premises Windows Server AD user account data.</span></span> 

## <a name="add-cloud-based-users"></a><span data-ttu-id="a2d25-105">Adja hozzá a felhőalapú felhasználók</span><span class="sxs-lookup"><span data-stu-id="a2d25-105">Add cloud-based users</span></span>
1. <span data-ttu-id="a2d25-106">Jelentkezzen be a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="a2d25-106">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="a2d25-107">Válassza ki **Azure Active Directory** , majd **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-107">Select **Azure Active Directory** and then **Users and groups**.</span></span>
3. <span data-ttu-id="a2d25-108">Az a **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-108">On the **Users and groups** blade, select **All users**, and then select **New user**.</span></span>
   <span data-ttu-id="a2d25-109">![Az Add parancs kiválasztása](./media/add-users-azure-active-directory/add-user.png)</span><span class="sxs-lookup"><span data-stu-id="a2d25-109">![Selecting the Add command](./media/add-users-azure-active-directory/add-user.png)</span></span>
4. <span data-ttu-id="a2d25-110">Adja meg a felhasználó, például a **neve** és **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-110">Enter details for the user, such as **Name** and **User name**.</span></span> <span data-ttu-id="a2d25-111">A tartomány a felhasználónév részét kell lennie, a kezdeti alapértelmezett tartomány neve "[neve].onmicrosoft.com" vagy egy ellenőrzött és érvényes, nem összevont [egyéni tartománynév](add-custom-domain.md) például "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="a2d25-111">The domain name portion of the user name must either be the initial default domain name "[domain name].onmicrosoft.com" or a verified, non-federated [custom domain name](add-custom-domain.md) such as "contoso.com."</span></span>
5. <span data-ttu-id="a2d25-112">Másolja, vagy ellenkező esetben vegye figyelembe a létrehozott felhasználói jelszót, így megadhatja azt a felhasználó számára a folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="a2d25-112">Copy or otherwise note the generated user password so that you can provide it to the user after this process is complete.</span></span>
6. <span data-ttu-id="a2d25-113">Másik lehetőségként megnyithatja és töltse ki a **profil** panelen a **csoportok** panelen, vagy a **címtár szerepkörrel** panel a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a2d25-113">Optionally, you can open and fill out the information in the **Profile** blade, the **Groups** blade, or the **Directory role** blade for the user.</span></span> <span data-ttu-id="a2d25-114">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a2d25-114">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="a2d25-115">Az a **felhasználói** panelen válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-115">On the **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="a2d25-116">Az új felhasználó számára létrehozott jelszót biztonságos terjesztése, hogy a felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="a2d25-116">Securely distribute the generated password to the new user so that the user can sign in.</span></span>

> [!TIP]
> <span data-ttu-id="a2d25-117">Is szinkronizálhatja a helyszíni Windows Server AD a felhasználói fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="a2d25-117">You can also synchronize user account data from on-premises Windows Server AD.</span></span> <span data-ttu-id="a2d25-118">A Microsoft identity megoldások span a helyszíni és felhőalapú képességek, a hitelesítés és engedélyezés az összes erőforráshoz, függetlenül a hely egyetlen felhasználói azonosítót létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a2d25-118">Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization to all resources, regardless of location.</span></span> <span data-ttu-id="a2d25-119">A hibrid identitás nevezzük.</span><span class="sxs-lookup"><span data-stu-id="a2d25-119">We call this Hybrid Identity.</span></span> <span data-ttu-id="a2d25-120">[Az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) a helyszíni címtárak integrálása az Azure Active Directory hibrid identitáskezelési forgatókönyvek esetén is használható.</span><span class="sxs-lookup"><span data-stu-id="a2d25-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) can be used to integrate your on-premises directories with Azure Active Directory for hybrid identity scenarios.</span></span> <span data-ttu-id="a2d25-121">Így közös identitást biztosíthat a felhasználóinak az Azure AD-vel integrált Office 365-, Azure- és SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a2d25-121">This allows you to provide a common identity for your users for Office 365, Azure, and SaaS applications integrated with Azure AD.</span></span> 

## <a name="delete-users-from-azure-ad"></a><span data-ttu-id="a2d25-122">Azure ad-felhasználók törlése</span><span class="sxs-lookup"><span data-stu-id="a2d25-122">Delete users from Azure AD</span></span>
1. <span data-ttu-id="a2d25-123">Jelentkezzen be a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="a2d25-123">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="a2d25-124">Válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-124">Select **Users and groups**.</span></span>
3. <span data-ttu-id="a2d25-125">Az a **felhasználók és csoportok** panelen válassza ki a felhasználót, hogy törli a listáról.</span><span class="sxs-lookup"><span data-stu-id="a2d25-125">On the **Users and groups** blade, select the user to delete from the list.</span></span> 
4. <span data-ttu-id="a2d25-126">A kiválasztott felhasználó paneljén válassza **áttekintése**, majd a parancssávon válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a2d25-126">On the blade for the selected user, select **Overview**, and then in the command bar, select **Delete**.</span></span>
   <span data-ttu-id="a2d25-127">![Az Add parancs kiválasztása](./media/add-users-azure-active-directory/delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="a2d25-127">![Selecting the Add command](./media/add-users-azure-active-directory/delete-user.png)</span></span>


### <a name="learn-more"></a><span data-ttu-id="a2d25-128">Részletek</span><span class="sxs-lookup"><span data-stu-id="a2d25-128">Learn more</span></span> 
* [<span data-ttu-id="a2d25-129">Külső felhasználó hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="a2d25-129">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)

* [<span data-ttu-id="a2d25-130">Felhasználó hozzárendelése egy szerepkörhöz az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a2d25-130">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a><span data-ttu-id="a2d25-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2d25-131">Next steps</span></span>
<span data-ttu-id="a2d25-132">A gyors üzembe helyezés az új felhasználók felvétele az Azure AD Premium hogy megismerte.</span><span class="sxs-lookup"><span data-stu-id="a2d25-132">In this quickstart, you’ve learned how to add new users to Azure AD Premium.</span></span> 

<span data-ttu-id="a2d25-133">A következő hivatkozás használatával hozzon létre egy új felhasználót az Azure-portálon az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a2d25-133">You can use the following link to create a new user in Azure AD from the Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a2d25-134">Felhasználók hozzáadása az Azure AD-hez</span><span class="sxs-lookup"><span data-stu-id="a2d25-134">Add users to Azure AD</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 