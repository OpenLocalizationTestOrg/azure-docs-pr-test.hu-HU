---
title: "aaaHow toogive hozzáférés tooPrivileged Identity Management - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd szerepkörök toousers a hello-e Azure Active Directory Privileged Identity Management bővítmény, PIM kezelésére."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5d99589af4af766e430d7cecd743ace752f63768
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="giving-access-toomanage-azure-ad-privileged-identity-management"></a><span data-ttu-id="485cf-103">Jogosultságot ad az Azure AD Privileged Identity Management hozzáférés toomanage</span><span class="sxs-lookup"><span data-stu-id="485cf-103">Giving access toomanage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="485cf-104">hello globális rendszergazda Azure AD Privileged Identity Management (PIM) egy olyan szervezet automatikusan lehetővé szerepkör-hozzárendelések hozzáférni és tooPIM.</span><span class="sxs-lookup"><span data-stu-id="485cf-104">hello global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access tooPIM.</span></span> <span data-ttu-id="485cf-105">Más írási hozzáféréssel alapértelmezés szerint kap, azonban más globális rendszergazdákat is.</span><span class="sxs-lookup"><span data-stu-id="485cf-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="485cf-106">Más globális rendszergazdák, biztonsági rendszergazdák és biztonsági olvasók rendelkezik olvasási hozzáféréssel tooAzure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="485cf-106">Other global adminstrators, security administrators, and security readers have read-only access tooAzure AD PIM.</span></span> <span data-ttu-id="485cf-107">toogive hozzáférés tooPIM, hello első felhasználó rendelhet mások toohello **kiemelt szerepkörű rendszergazda** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="485cf-107">toogive access tooPIM, hello first user can assign others toohello **Privileged role administrator** role.</span></span> <span data-ttu-id="485cf-108">Ehhez a hozzárendeléshez maga PIM belül kell végrehajtani, és a PowerShell vagy más portálokon keresztül nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="485cf-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="485cf-109">Az Azure MFA kezelése az Azure AD PIM van szükség.</span><span class="sxs-lookup"><span data-stu-id="485cf-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="485cf-110">Microsoft-fiókok az Azure MFA nem regisztrálható, mert a Microsoft-fiókkal jelentkezik be egy felhasználó nem fér hozzá a Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="485cf-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="485cf-111">Győződjön meg arról, hogy mindig legalább két felhasználók egy kiemelt szerepkörű rendszergazda, abban az esetben, ha egy felhasználó ki van zárva, vagy a fiókot törölték.</span><span class="sxs-lookup"><span data-stu-id="485cf-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-toomanage-pim"></a><span data-ttu-id="485cf-112">Adjon meg egy másik felhasználó hozzáférési toomanage PIM</span><span class="sxs-lookup"><span data-stu-id="485cf-112">Give another user access toomanage PIM</span></span>
1. <span data-ttu-id="485cf-113">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és select hello **Azure AD Privileged Identity Management** app hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="485cf-113">Sign in toohello [Azure portal](https://portal.azure.com/) and select hello **Azure AD Privileged Identity Management** app on hello dashboard.</span></span>
2. <span data-ttu-id="485cf-114">Válassza ki **kiemelt szerepköröket kezelése** > **kiemelt szerepkörű rendszergazda** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="485cf-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Adja hozzá a kiemelt szerepkörű rendszergazda – képernyőkép][1]
3. <span data-ttu-id="485cf-116">Hello felügyelt felhasználók hozzáadása paneljét, az 1. lépés már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="485cf-116">On hello Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="485cf-117">Válassza ki a 2 **válasszon ki egy felhasználót** és a keresési hello felhasználói tooadd keresi.</span><span class="sxs-lookup"><span data-stu-id="485cf-117">Select step 2, **Select users** and search for hello user you want tooadd.</span></span>
   
    ![Válassza ki a felhasználók – képernyőkép][2]
4. <span data-ttu-id="485cf-119">Jelöljön ki hello felhasználói hello keresési eredményeket, majd **végzett**.</span><span class="sxs-lookup"><span data-stu-id="485cf-119">Select hello user from hello search results, and click **Done**.</span></span>
5. <span data-ttu-id="485cf-120">Kattintson a **OK** toosave a kijelölés.</span><span class="sxs-lookup"><span data-stu-id="485cf-120">Click **OK** toosave your selection.</span></span> <span data-ttu-id="485cf-121">kijelölt hello felhasználói kiemelt szerepkörű rendszergazda hello listája megjelenik.</span><span class="sxs-lookup"><span data-stu-id="485cf-121">hello user you have selected will appear in hello list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="485cf-122">Amikor egy új szerepkör toosomeone rendelje hozzá, azok automatikusan be vannak állítva jogosult tooactivate hello szerepkörként.</span><span class="sxs-lookup"><span data-stu-id="485cf-122">Whenever you assign a new role toosomeone, they are automatically set up as eligible tooactivate hello role.</span></span> <span data-ttu-id="485cf-123">Ha azt szeretné, hogy toomake hello felhasználói hello listában kattintson azokat állandó hello szerepkörben.</span><span class="sxs-lookup"><span data-stu-id="485cf-123">If you want toomake them permanent in hello role, click hello user in hello list.</span></span> <span data-ttu-id="485cf-124">Válassza ki **perm ellenőrizze** hello felhasználói információ menüben.</span><span class="sxs-lookup"><span data-stu-id="485cf-124">Select **make perm** in hello user information menu.</span></span>
6. <span data-ttu-id="485cf-125">Hello felhasználói hivatkozás küldése túl[Ismerkedés az Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="485cf-125">Send hello user a link too[Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="485cf-126">Egy másik felhasználói hozzáférési jogosultságokat PIM kezelésére szolgáló eltávolítása</span><span class="sxs-lookup"><span data-stu-id="485cf-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="485cf-127">Mielőtt törli valakit hello kiemelt szerepkörű rendszergazda, ügyeljen arra, hogy még mindig lesz két felhasználóira tooit.</span><span class="sxs-lookup"><span data-stu-id="485cf-127">Before you remove someone from hello privileged role administrator role, always make sure there will still be two users assigned tooit.</span></span>

1. <span data-ttu-id="485cf-128">Hello PIM irányítópultot, kattintson a hello szerepkör **kiemelt szerepkörű rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="485cf-128">In hello PIM dashboard, click on hello role **Privileged role administrator**.</span></span>  <span data-ttu-id="485cf-129">jelenleg a szerepet betöltő felhasználók hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="485cf-129">hello list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="485cf-130">Kattintson a Felhasználólista hello hello felhasználói.</span><span class="sxs-lookup"><span data-stu-id="485cf-130">Click on hello user in hello user list.</span></span>
3. <span data-ttu-id="485cf-131">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="485cf-131">Click on **Remove**.</span></span>  <span data-ttu-id="485cf-132">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="485cf-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="485cf-133">Kattintson a **Igen** tooremove hello felhasználói hello szerepkörből.</span><span class="sxs-lookup"><span data-stu-id="485cf-133">Click **Yes** tooremove hello user from hello role.</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="485cf-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="485cf-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
