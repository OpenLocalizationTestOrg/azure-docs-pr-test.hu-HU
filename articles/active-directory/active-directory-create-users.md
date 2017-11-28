---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók, vagy módosítsa a felhasználói adatokat az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a><span data-ttu-id="8f87c-103">Új felhasználók vagy a Microsoft-fiókok tooAzure Active Directory felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f87c-103">Add new users or users with Microsoft accounts tooAzure Active Directory</span></span>
<span data-ttu-id="8f87c-104">Adja hozzá a felhasználók toopopulate a címtárban.</span><span class="sxs-lookup"><span data-stu-id="8f87c-104">Add users toopopulate your directory.</span></span> <span data-ttu-id="8f87c-105">Ez a cikk azt ismerteti, hogyan tooadd új felhasználókat a szervezetben, és hogyan tooadd Microsoft-fiókkal rendelkező felhasználók.</span><span class="sxs-lookup"><span data-stu-id="8f87c-105">This article explains how tooadd new users in your organization, and how tooadd users who have Microsoft accounts.</span></span> <span data-ttu-id="8f87c-106">Az Azure Active Directoryban a más címtárakban lévő felhasználók felvételéről vagy a partnervállalatokban lévő felhasználók felvételéről további információért lásd: [Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="8f87c-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="8f87c-107">A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzárendelheti a szerepkörökhöz toothem.</span><span class="sxs-lookup"><span data-stu-id="8f87c-107">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f87c-108">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8f87c-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="8f87c-109">Hogyan tooadd hello Azure AD felügyeleti központban, felhasználó: a [adja hozzá az új felhasználók tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8f87c-109">For how tooadd a user in hello Azure AD admin center, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="8f87c-110">Felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f87c-110">Add a user</span></span>
1. <span data-ttu-id="8f87c-111">Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="8f87c-111">Sign in toohello [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="8f87c-112">Válassza ki **Active Directory**, majd válassza ki a szervezete címtárának nevét hello.</span><span class="sxs-lookup"><span data-stu-id="8f87c-112">Select **Active Directory**, and then select hello name of your organization directory.</span></span>
3. <span data-ttu-id="8f87c-113">Jelölje be hello **felhasználók** lapot, és ezt követően hello parancssávon válassza **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8f87c-113">Select hello **Users** tab, and then, in hello command bar, select **Add User**.</span></span>
4. <span data-ttu-id="8f87c-114">A hello **adja meg azt a felhasználó** lap **felhasználó típusa**, a következők közül választhat:</span><span class="sxs-lookup"><span data-stu-id="8f87c-114">On hello **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="8f87c-115">**Új felhasználó a szervezetben** – egy új felhasználói fiókot ad a címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="8f87c-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="8f87c-116">**Egy meglévő Microsoft-fiókkal rendelkező felhasználó** – ad hozzá egy meglévő Microsoft fogyasztói fiók tooyour könyvtár (például egy Outlook-fiókot)</span><span class="sxs-lookup"><span data-stu-id="8f87c-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account tooyour directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="8f87c-117">A **Felhasználó típusától** függően írjon be egy felhasználónevet (új felhasználóhoz) vagy e-mail-címet (Microsoft-fiókkal rendelkező felhasználóhoz).</span><span class="sxs-lookup"><span data-stu-id="8f87c-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="8f87c-118">Hello felhasználó **profil** lapján adja meg az első és utolsó nevét, egy felhasználóbarát nevet és egy felhasználói szerepkört hello **szerepkörök** listája.</span><span class="sxs-lookup"><span data-stu-id="8f87c-118">On hello user **Profile** page, provide a first and last name, a user-friendly name, and a user role from hello **Roles** list.</span></span> <span data-ttu-id="8f87c-119">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="8f87c-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="8f87c-120">Adja meg, hogy túl**többtényezős hitelesítés engedélyezése** hello felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="8f87c-120">Specify whether too**Enable Multi-Factor Authentication** for hello user.</span></span>
7. <span data-ttu-id="8f87c-121">A hello **ideiglenes jelszó beszerzése** lapon jelölje be **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f87c-121">On hello **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8f87c-122">Ha szervezete több tartományt használ, a felhasználói fiók hozzáadásakor a következő problémák hello kell tudnia:</span><span class="sxs-lookup"><span data-stu-id="8f87c-122">If your organization uses more than one domain, you should know about hello following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="8f87c-123">felhasználói fiókok tooadd hello azonos egyszerű felhasználónév (UPN) tartományokban, **első** hozzáadni, például geoffgrisso@contoso.onmicrosoft.com, **követ** geoffgrisso@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="8f87c-123">tooadd user accounts with hello same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="8f87c-124">**Ne** adja hozzá a geoffgrisso@contoso.com címet a geoffgrisso@contoso.onmicrosoft.com hozzáadása előtt. Ez a sorrend fontos, és nehézkes tooundo lehet.</span><span class="sxs-lookup"><span data-stu-id="8f87c-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com. This order is important, and can be cumbersome tooundo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="8f87c-125">Felhasználói adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="8f87c-125">Change user information</span></span>
<span data-ttu-id="8f87c-126">Módosíthatja bármely felhasználói attribútumot, kivéve a hello objektum azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="8f87c-126">You can change any user attribute except for hello object ID.</span></span>

1. <span data-ttu-id="8f87c-127">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="8f87c-127">Open your directory.</span></span>
2. <span data-ttu-id="8f87c-128">Jelölje be hello **felhasználók** fülre, majd jelölje ki hello megjelenített neve a hello toochange kívánt felhasználó.</span><span class="sxs-lookup"><span data-stu-id="8f87c-128">Select hello **Users** tab, and then select hello display name of hello user you want toochange.</span></span>
3. <span data-ttu-id="8f87c-129">Végezze el a módosításokat, majd kattintson a **Mentés** parancsra.</span><span class="sxs-lookup"><span data-stu-id="8f87c-129">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="8f87c-130">Ha hello épp módosított felhasználó szinkronizálva van a helyszíni Active Directory szolgáltatással, nem módosíthatja hello felhasználó adatait ezzel az eljárással.</span><span class="sxs-lookup"><span data-stu-id="8f87c-130">If hello user that you're changing is synchronized with your on-premises Active Directory service, you can't change hello user information using this procedure.</span></span> <span data-ttu-id="8f87c-131">toochange hello felhasználói, használja a helyszíni Active Directory kezelőeszközöket.</span><span class="sxs-lookup"><span data-stu-id="8f87c-131">toochange hello user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="8f87c-132">Vendégfelhasználók kezelése és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="8f87c-132">Guest user management and limitations</span></span>
<span data-ttu-id="8f87c-133">A Vendég felhasználókat más címtárakból, akik a meghívott tooyour directory tooaccess SharePoint-dokumentumok, alkalmazások vagy más Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="8f87c-133">Guest accounts are users from other directories who were invited tooyour directory tooaccess SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="8f87c-134">A Vendég fiók a könyvtárban van, a mögöttes UserType attribútuma túl "Guest."</span><span class="sxs-lookup"><span data-stu-id="8f87c-134">A guest account in your directory has its underlying UserType attribute set too"Guest."</span></span> <span data-ttu-id="8f87c-135">Normál felhasználók (különösen a címtár tagjai) rendelkezik hello UserType attribútuma "Tag".</span><span class="sxs-lookup"><span data-stu-id="8f87c-135">Regular users (specifically, members of your directory) have hello UserType attribute "Member."</span></span>

<span data-ttu-id="8f87c-136">A vendégek rendelkeznek korlátozott jogok hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8f87c-136">Guests have a limited set of rights in hello directory.</span></span> <span data-ttu-id="8f87c-137">Ezek a jogosultságok korlátozzák a vendégek toodiscover információt hello címtár más felhasználóinak hello képességét.</span><span class="sxs-lookup"><span data-stu-id="8f87c-137">These rights limit hello ability for Guests toodiscover information about other users in hello directory.</span></span> <span data-ttu-id="8f87c-138">Azonban vendégfelhasználók továbbra is kommunikálhatnak hello felhasználókhoz és csoportokhoz társított hello erőforrásokat, amelyeken dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="8f87c-138">However, guest users can still interact with hello users and groups associated with hello resources they're working on.</span></span> <span data-ttu-id="8f87c-139">A vendégfelhasználók a következőkre képesek:</span><span class="sxs-lookup"><span data-stu-id="8f87c-139">Guest users can:</span></span>

* <span data-ttu-id="8f87c-140">Felhasználók és csoportok társított egy Azure-előfizetés toowhich hozzá vannak rendelve</span><span class="sxs-lookup"><span data-stu-id="8f87c-140">See other users and groups associated with an Azure subscription toowhich they're assigned</span></span>
* <span data-ttu-id="8f87c-141">Tekintse meg a csoportok toowhich tartoznak hello tagjai</span><span class="sxs-lookup"><span data-stu-id="8f87c-141">See hello members of groups toowhich they belong</span></span>
* <span data-ttu-id="8f87c-142">Más felhasználók hello könyvtárban kereshet, ha ismerik hello hello felhasználó teljes e-mail címe</span><span class="sxs-lookup"><span data-stu-id="8f87c-142">Look up other users in hello directory, if they know hello full email address of hello user</span></span>
* <span data-ttu-id="8f87c-143">Lásd: hello felhasználók megtekintése – korlátozott toodisplay nevét, e-mail címét, egyszerű felhasználónév (UPN) és a miniatűr fényképre attribútumok csak korlátozott számú</span><span class="sxs-lookup"><span data-stu-id="8f87c-143">See only a limited set of attributes of hello users they look up--limited toodisplay name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="8f87c-144">Hello könyvtárban lévő ellenőrzött tartományok listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="8f87c-144">Get a list of verified domains in hello directory</span></span>
* <span data-ttu-id="8f87c-145">Hozzájárulás tooapplications számukra hello ugyanolyan szintű hozzáférése, amely a tagok rendelkeznek a címtárban</span><span class="sxs-lookup"><span data-stu-id="8f87c-145">Consent tooapplications, granting them hello same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="8f87c-146">A vendégfelhasználók hozzáférési házirendjeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="8f87c-146">Set guest user access policies</span></span>
<span data-ttu-id="8f87c-147">Hello **konfigurálása** címtár lapján található beállítások toocontrol a vendégfelhasználók hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="8f87c-147">hello **Configure** tab of a directory includes options toocontrol access for guest users.</span></span> <span data-ttu-id="8f87c-148">Ezek a lehetőségek csak a klasszikus Azure portálon módosíthatók a címtár globális rendszergazdája által.</span><span class="sxs-lookup"><span data-stu-id="8f87c-148">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="8f87c-149">Jelenleg erre a célra nem létezik PowerShell- vagy API-módszer.</span><span class="sxs-lookup"><span data-stu-id="8f87c-149">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="8f87c-150">tooopen hello **konfigurálása** lapján hello Azure klasszikus portál, válassza ki **Active Directory**, majd válassza ki a hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="8f87c-150">tooopen hello **Configure** tab in hello Azure classic portal, select **Active Directory**, and then select hello name of hello directory.</span></span>

![Konfigurálás lap az Azure Active Directoryban][1]

<span data-ttu-id="8f87c-152">Ezután szerkesztheti a vendégfelhasználók hello beállítások toocontrol hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="8f87c-152">Then you can edit hello options toocontrol access for guest users.</span></span>

![hozzáférés-vezérlési lehetőségek a vendégfelhasználók számára][2]

## <a name="whats-next"></a><span data-ttu-id="8f87c-154">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f87c-154">What's next</span></span>
* [<span data-ttu-id="8f87c-155">Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba</span><span class="sxs-lookup"><span data-stu-id="8f87c-155">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="8f87c-156">Az Azure AD felügyelete</span><span class="sxs-lookup"><span data-stu-id="8f87c-156">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="8f87c-157">Jelszavak kezelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8f87c-157">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="8f87c-158">Csoportok kezelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8f87c-158">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
