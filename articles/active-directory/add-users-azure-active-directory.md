---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók az Azure Active Directoryban."
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
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a><span data-ttu-id="7515c-103">Gyors üzembe helyezés: Új felhasználók tooAzure Active Directory hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7515c-103">Quickstart: Add new users tooAzure Active Directory</span></span>
<span data-ttu-id="7515c-104">Ez a cikk azt ismerteti, hogyan tooadd a szervezet hello Azure Active Directory (Azure AD) az új felhasználói hello Azure-portál használatával egyszerre, vagy a helyszíni Windows Server AD-felhasználó szinkronizálásával fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="7515c-104">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD) one at a time using hello Azure portal or by synchronizing your on-premises Windows Server AD user account data.</span></span> 

## <a name="add-cloud-based-users"></a><span data-ttu-id="7515c-105">Adja hozzá a felhőalapú felhasználók</span><span class="sxs-lookup"><span data-stu-id="7515c-105">Add cloud-based users</span></span>
1. <span data-ttu-id="7515c-106">Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="7515c-106">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="7515c-107">Válassza ki **Azure Active Directory** , majd **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7515c-107">Select **Azure Active Directory** and then **Users and groups**.</span></span>
3. <span data-ttu-id="7515c-108">A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7515c-108">On hello **Users and groups** blade, select **All users**, and then select **New user**.</span></span>
   <span data-ttu-id="7515c-109">![Hello hozzáadása paranccsal](./media/add-users-azure-active-directory/add-user.png)</span><span class="sxs-lookup"><span data-stu-id="7515c-109">![Selecting hello Add command](./media/add-users-azure-active-directory/add-user.png)</span></span>
4. <span data-ttu-id="7515c-110">Adja meg például a részleteit hello felhasználói **neve** és **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="7515c-110">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="7515c-111">hello tartomány hello felhasználónév részét kell hello kezdeti alapértelmezett tartomány neve "[neve].onmicrosoft.com", vagy egy ellenőrzött és érvényes, nem összevont [egyéni tartománynév](add-custom-domain.md) például "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="7515c-111">hello domain name portion of hello user name must either be hello initial default domain name "[domain name].onmicrosoft.com" or a verified, non-federated [custom domain name](add-custom-domain.md) such as "contoso.com."</span></span>
5. <span data-ttu-id="7515c-112">Másolat vagy más módon Megjegyzés hello által létrehozott felhasználói jelszó, így megadhatja azt toohello felhasználói a folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="7515c-112">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="7515c-113">Igény szerint megnyitásához, és a hello hello adatok **profil** panelen, hello **csoportok** panel vagy hello **Directory szerepkör** hello felhasználói paneljét.</span><span class="sxs-lookup"><span data-stu-id="7515c-113">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="7515c-114">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="7515c-114">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="7515c-115">A hello **felhasználói** panelen válassza **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7515c-115">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="7515c-116">Biztonságosan elosztani a generált hello jelszó toohello új felhasználó, így hello felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="7515c-116">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

> [!TIP]
> <span data-ttu-id="7515c-117">Is szinkronizálhatja a helyszíni Windows Server AD a felhasználói fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="7515c-117">You can also synchronize user account data from on-premises Windows Server AD.</span></span> <span data-ttu-id="7515c-118">A Microsoft identity megoldások span a helyszíni és felhőalapú képességek, létrehozása egy felhasználói azonosítót a hitelesítéshez és engedélyezéshez tooall erőforrások helyétől függetlenül.</span><span class="sxs-lookup"><span data-stu-id="7515c-118">Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization tooall resources, regardless of location.</span></span> <span data-ttu-id="7515c-119">A hibrid identitás nevezzük.</span><span class="sxs-lookup"><span data-stu-id="7515c-119">We call this Hybrid Identity.</span></span> <span data-ttu-id="7515c-120">[Az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) lehet használt toointegrate a helyszíni címtárak és az Azure Active Directory hibrid identitáskezelési környezetekben.</span><span class="sxs-lookup"><span data-stu-id="7515c-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) can be used toointegrate your on-premises directories with Azure Active Directory for hybrid identity scenarios.</span></span> <span data-ttu-id="7515c-121">Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazások a felhasználók közös identitás tooprovide.</span><span class="sxs-lookup"><span data-stu-id="7515c-121">This allows you tooprovide a common identity for your users for Office 365, Azure, and SaaS applications integrated with Azure AD.</span></span> 

## <a name="delete-users-from-azure-ad"></a><span data-ttu-id="7515c-122">Azure ad-felhasználók törlése</span><span class="sxs-lookup"><span data-stu-id="7515c-122">Delete users from Azure AD</span></span>
1. <span data-ttu-id="7515c-123">Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="7515c-123">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="7515c-124">Válassza ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7515c-124">Select **Users and groups**.</span></span>
3. <span data-ttu-id="7515c-125">A hello **felhasználók és csoportok** panelen, jelölje be hello felhasználói toodelete hello listából.</span><span class="sxs-lookup"><span data-stu-id="7515c-125">On hello **Users and groups** blade, select hello user toodelete from hello list.</span></span> 
4. <span data-ttu-id="7515c-126">A kiválasztott felhasználó hello hello panelen válassza ki a **áttekintése**, majd a hello parancssávon válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="7515c-126">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>
   <span data-ttu-id="7515c-127">![Hello hozzáadása paranccsal](./media/add-users-azure-active-directory/delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="7515c-127">![Selecting hello Add command](./media/add-users-azure-active-directory/delete-user.png)</span></span>


### <a name="learn-more"></a><span data-ttu-id="7515c-128">Részletek</span><span class="sxs-lookup"><span data-stu-id="7515c-128">Learn more</span></span> 
* [<span data-ttu-id="7515c-129">Külső felhasználó hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="7515c-129">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)

* [<span data-ttu-id="7515c-130">Egy felhasználó tooa szerepkör hozzárendelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7515c-130">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a><span data-ttu-id="7515c-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7515c-131">Next steps</span></span>
<span data-ttu-id="7515c-132">A gyors üzembe helyezés, hogy megtanulta, hogyan tooadd új felhasználók tooAzure prémium szintű AD.</span><span class="sxs-lookup"><span data-stu-id="7515c-132">In this quickstart, you’ve learned how tooadd new users tooAzure AD Premium.</span></span> 

<span data-ttu-id="7515c-133">Hello hivatkozás toocreate új felhasználó követően az Azure AD hello Azure-portálon is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7515c-133">You can use hello following link toocreate a new user in Azure AD from hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7515c-134">Felhasználók tooAzure AD hozzá</span><span class="sxs-lookup"><span data-stu-id="7515c-134">Add users tooAzure AD</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
