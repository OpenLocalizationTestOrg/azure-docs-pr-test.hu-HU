---
title: "beállítások a aaaHow toomanage aktiválási |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange hello-alapértelmezett beállításait a kiemelt jogosultságú identitások hello Azure Active Directory Privileged Identity Management bővítmény."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="7f837-103">Hogyan toomanage szerepkör aktiválási beállításokat az Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="7f837-103">How toomanage role activation settings in Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="7f837-104">A kiemelt szerepkörű rendszergazda testre szabhatja az Azure AD Privileged Identity Management (PIM) a szervezetek, beleértve a felhasználó számára a megfelelő szerepkör-hozzárendelés az aktiválás hello élmény módosítása.</span><span class="sxs-lookup"><span data-stu-id="7f837-104">A privileged role administrator can customize Azure AD Privileged Identity Management (PIM) in their organization, including changing hello experience for a user who is activating an eligible role assignment.</span></span>

## <a name="manage-hello-role-activation-settings"></a><span data-ttu-id="7f837-105">Hello szerepkör aktiválási beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="7f837-105">Manage hello role activation settings</span></span>
1. <span data-ttu-id="7f837-106">Nyissa meg toohello [Azure-portálon](https://portal.azure.com) és select hello **Azure AD Privileged Identity Management** hello irányítópult alkalmazásából.</span><span class="sxs-lookup"><span data-stu-id="7f837-106">Go toohello [Azure portal](https://portal.azure.com) and select hello **Azure AD Privileged Identity Management** app from hello dashboard.</span></span>
2. <span data-ttu-id="7f837-107">Válassza ki **kiemelt szerepköröket kezelése** > **beállítások** > **kiemelt szerepköröket**.</span><span class="sxs-lookup"><span data-stu-id="7f837-107">Select **Manage privileged roles** > **Settings** > **Privileged Roles**.</span></span>
3. <span data-ttu-id="7f837-108">Válassza ki a hello szerepkört amelynek beállításait szeretné toomanage.</span><span class="sxs-lookup"><span data-stu-id="7f837-108">Choose hello role whose settings you want toomanage.</span></span>

<span data-ttu-id="7f837-109">A hello-beállítások lapon az egyes szerepkörökhöz számos a konfigurálható beállítások.</span><span class="sxs-lookup"><span data-stu-id="7f837-109">On hello settings page for each role, there are a number of settings you can configure.</span></span> <span data-ttu-id="7f837-110">Ezek a beállítások csak a jogosult rendszergazdai, nem állandó rendszergazdák felhasználók számára is vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="7f837-110">These settings only affect users who are eligible admins, not permanent admins.</span></span>

<span data-ttu-id="7f837-111">**Aktiválás**: hello idő órában, amely a szerepkör érvényességének lejárata előtt.</span><span class="sxs-lookup"><span data-stu-id="7f837-111">**Activations**: hello time, in hours, that a role stays active before it expires.</span></span> <span data-ttu-id="7f837-112">Ez az 1 és 72 óra közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="7f837-112">This can be between 1 and 72 hours.</span></span>

<span data-ttu-id="7f837-113">**Értesítések**: dönthet úgy, hogy hello küldi e-mailek tooadmins erősítse meg, hogy a szerepkör aktiválta.</span><span class="sxs-lookup"><span data-stu-id="7f837-113">**Notifications**: You can choose whether or not hello system sends emails tooadmins confirming that they have activated a role.</span></span> <span data-ttu-id="7f837-114">Ez lehet hasznos, ha a jogosulatlan vagy illegitimate aktiválások észlelése.</span><span class="sxs-lookup"><span data-stu-id="7f837-114">This can be useful for detecting unauthorized or illegitimate activations.</span></span>

<span data-ttu-id="7f837-115">**Incidens/kérelmezési jegye**: választhatja, függetlenül attól, toorequire jogosult rendszergazdák tooinclude jegy number, amikor azok a szerepkör aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="7f837-115">**Incident/Request Ticket**: You can choose whether or not toorequire eligible admins tooinclude a ticket number when they activate their role.</span></span> <span data-ttu-id="7f837-116">Ez akkor lehet hasznos, szerepkör-hozzáférési eseményeket végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="7f837-116">This can be useful when you perform role access audits.</span></span>

<span data-ttu-id="7f837-117">**A multi-factor Authentication**: választhat-e toorequire felhasználók tooverify az identitásukat ahhoz, azok a szerepkörök aktiválásához többtényezős hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="7f837-117">**Multi-Factor Authentication**: You can choose whether or not toorequire users tooverify their identity with MFA before they can activate their roles.</span></span> <span data-ttu-id="7f837-118">Csak rendelkeznek tooverify a egyszer munkamenet, nem minden alkalommal, amikor azok a szerepkör aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="7f837-118">They only have tooverify this once per session, not every time they activate a role.</span></span> <span data-ttu-id="7f837-119">Nincsenek két tippek tookeep szem előtt tartva a többtényezős hitelesítés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="7f837-119">There are two tips tookeep in mind when you enable MFA:</span></span>

* <span data-ttu-id="7f837-120">E-mail címüket a Microsoft-fiókkal rendelkező felhasználók számára (általában @outlook.com, de nem minden esetben) az Azure MFA nem regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="7f837-120">Users who have Microsoft accounts for their email addresses (typically @outlook.com, but not always) cannot register for Azure MFA.</span></span> <span data-ttu-id="7f837-121">Ha azt szeretné, hogy a Microsoft-fiókkal rendelkező tooassign szerepkörök toousers, kell tenni őket állandó rendszergazdák vagy a többtényezős hitelesítés letiltása adott szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="7f837-121">If you want tooassign roles toousers with Microsoft accounts, you should either make them permanent admins or disable MFA for that role.</span></span>
* <span data-ttu-id="7f837-122">Nem tiltható le MFA kiemelt jogosultságokkal rendelkező szerepkörök az Azure AD és az Office365.</span><span class="sxs-lookup"><span data-stu-id="7f837-122">You cannot disable MFA for highly privileged roles for Azure AD and Office365.</span></span> <span data-ttu-id="7f837-123">Ez egy olyan biztonsági beállítás, mert ezeket a szerepköröket kell védeni:</span><span class="sxs-lookup"><span data-stu-id="7f837-123">This is a safety feature because these roles should be carefully protected:</span></span>  
  
  * <span data-ttu-id="7f837-124">Alkalmazás-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-124">Application administrator</span></span>
  * <span data-ttu-id="7f837-125">Alkalmazás-Proxy kiszolgáló-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-125">Application Proxy server administrator</span></span>
  * <span data-ttu-id="7f837-126">Számlázási rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-126">Billing administrator</span></span>  
  * <span data-ttu-id="7f837-127">Megfelelőségi rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-127">Compliance administrator</span></span>  
  * <span data-ttu-id="7f837-128">CRM szolgáltatás-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-128">CRM service administrator</span></span>
  * <span data-ttu-id="7f837-129">Ügyfél kulcstároló hozzáférési jóváhagyó</span><span class="sxs-lookup"><span data-stu-id="7f837-129">Customer LockBox access approver</span></span>
  * <span data-ttu-id="7f837-130">Directory írója</span><span class="sxs-lookup"><span data-stu-id="7f837-130">Directory writer</span></span>  
  * <span data-ttu-id="7f837-131">Exchange-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-131">Exchange administrator</span></span>  
  * <span data-ttu-id="7f837-132">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-132">Global administrator</span></span>
  * <span data-ttu-id="7f837-133">Intune szolgáltatás-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-133">Intune service administrator</span></span>
  * <span data-ttu-id="7f837-134">Postaláda-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-134">Mailbox administrator</span></span>  
  * <span data-ttu-id="7f837-135">Partner tier1 támogatása</span><span class="sxs-lookup"><span data-stu-id="7f837-135">Partner tier1 support</span></span>  
  * <span data-ttu-id="7f837-136">Partner tier2 támogatása</span><span class="sxs-lookup"><span data-stu-id="7f837-136">Partner tier2 support</span></span>  
  * <span data-ttu-id="7f837-137">Kiemelt szerepkörű rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-137">Privileged role administrator</span></span>   
  * <span data-ttu-id="7f837-138">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-138">Security administrator</span></span>  
  * <span data-ttu-id="7f837-139">SharePoint-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-139">SharePoint administrator</span></span>  
  * <span data-ttu-id="7f837-140">Skype Vállalati verzió-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="7f837-140">Skype for Business administrator</span></span>  
  * <span data-ttu-id="7f837-141">Felhasználói fiók rendszergazdája</span><span class="sxs-lookup"><span data-stu-id="7f837-141">User account administrator</span></span>  

<span data-ttu-id="7f837-142">További információ a többtényezős hitelesítés használata a PIM: [hogyan tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span><span class="sxs-lookup"><span data-stu-id="7f837-142">For more information about using MFA with PIM see [How tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).</span></span>

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="7f837-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f837-143">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

