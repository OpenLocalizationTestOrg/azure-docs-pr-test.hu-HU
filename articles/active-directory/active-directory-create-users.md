---
title: "Új felhasználók hozzáadása az Azure Active Directoryhoz | Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat, vagy hogyan módosíthatja a felhasználói adatokat az Azure Active Directoryban."
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
ms.openlocfilehash: ff4b742e772a6062885313e9bb49e55907fe125a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-to-azure-active-directory"></a><span data-ttu-id="abb59-103">Új felhasználók vagy a Microsoft-fiókkal rendelkező felhasználók hozzáadása az Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="abb59-103">Add new users or users with Microsoft accounts to Azure Active Directory</span></span>
<span data-ttu-id="abb59-104">Felhasználók hozzáadásával feltöltheti a címtárat adatokkal.</span><span class="sxs-lookup"><span data-stu-id="abb59-104">Add users to populate your directory.</span></span> <span data-ttu-id="abb59-105">Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat a szervezetben, és hogyan vehet fel Microsoft-fiókkal rendelkező felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="abb59-105">This article explains how to add new users in your organization, and how to add users who have Microsoft accounts.</span></span> <span data-ttu-id="abb59-106">Az Azure Active Directoryban a más címtárakban lévő felhasználók felvételéről vagy a partnervállalatokban lévő felhasználók felvételéről további információért lásd: [Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba](active-directory-create-users-external.md).</span><span class="sxs-lookup"><span data-stu-id="abb59-106">For more information about adding users from other directories in Azure Active Directory or adding users from partner companies, see [Add users from other directories or partner companies in Azure Active Directory](active-directory-create-users-external.md).</span></span> <span data-ttu-id="abb59-107">A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzájuk rendelhet szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="abb59-107">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abb59-108">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="abb59-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> <span data-ttu-id="abb59-109">Szeretne hozzáadni egy felhasználót az Azure AD felügyeleti központban, olvassa el a [új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="abb59-109">For how to add a user in the Azure AD admin center, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="add-a-user"></a><span data-ttu-id="abb59-110">Felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="abb59-110">Add a user</span></span>
1. <span data-ttu-id="abb59-111">Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="abb59-111">Sign in to the [Azure classic portal](https://manage.windowsazure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="abb59-112">Válassza az **Active Directory** lehetőséget, majd válassza ki a szervezete címtárának nevét.</span><span class="sxs-lookup"><span data-stu-id="abb59-112">Select **Active Directory**, and then select the name of your organization directory.</span></span>
3. <span data-ttu-id="abb59-113">Válassza a **Felhasználók** lapot, majd a parancssávon válassza a **Felhasználó hozzáadása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="abb59-113">Select the **Users** tab, and then, in the command bar, select **Add User**.</span></span>
4. <span data-ttu-id="abb59-114">A **Felhasználó bemutatása** lap **Felhasználó típusa** területén válassza a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="abb59-114">On the **Tell us about this user** page, under **Type of user**, select either:</span></span>

   * <span data-ttu-id="abb59-115">**Új felhasználó a szervezetben** – egy új felhasználói fiókot ad a címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="abb59-115">**New user in your organization** – adds a new user account in your directory.</span></span>
   * <span data-ttu-id="abb59-116">**Felhasználó meglévő Microsoft-fiókkal** – egy meglévő végfelhasználói Microsoft-fiókot ad a címtárhoz (például egy Outlook-fiókot)</span><span class="sxs-lookup"><span data-stu-id="abb59-116">**User with an existing Microsoft account** – adds an existing Microsoft consumer account to your directory (for example, an Outlook account)</span></span>
5. <span data-ttu-id="abb59-117">A **Felhasználó típusától** függően írjon be egy felhasználónevet (új felhasználóhoz) vagy e-mail-címet (Microsoft-fiókkal rendelkező felhasználóhoz).</span><span class="sxs-lookup"><span data-stu-id="abb59-117">Depending on **Type of user**, enter a user name (for new user) or an email address (for a user with a Microsoft account).</span></span>
6. <span data-ttu-id="abb59-118">A felhasználói **Profil** oldalon adjon meg egy kereszt- és egy vezetéknevet, egy rövid nevet és egy felhasználói szerepkört a **Szerepkörök** listából.</span><span class="sxs-lookup"><span data-stu-id="abb59-118">On the user **Profile** page, provide a first and last name, a user-friendly name, and a user role from the **Roles** list.</span></span> <span data-ttu-id="abb59-119">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="abb59-119">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span> <span data-ttu-id="abb59-120">Adja meg, hogy **engedélyezi-e a többtényezős hitelesítést** a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="abb59-120">Specify whether to **Enable Multi-Factor Authentication** for the user.</span></span>
7. <span data-ttu-id="abb59-121">Az **Ideiglenes jelszó beszerzése** oldalon válassza a **Létrehozás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="abb59-121">On the **Get temporary password** page, select **Create**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abb59-122">Ha a szervezete több tartományt használ, a következő problémákról kell tudnia a felhasználói fiók hozzáadásakor:</span><span class="sxs-lookup"><span data-stu-id="abb59-122">If your organization uses more than one domain, you should know about the following issues when you add a user account:</span></span>
>
> * <span data-ttu-id="abb59-123">Ha azonos egyszerű felhasználónévvel (UPN) szeretne felhasználói fiókokat felvenni több tartományba, **először** adja hozzá például a geoffgrisso@contoso.onmicrosoft.com,  **címet, majd a**  geoffgrisso@contoso.com címet.</span><span class="sxs-lookup"><span data-stu-id="abb59-123">TO add user accounts with the same user principal name (UPN) across domains, **first** add, for example, geoffgrisso@contoso.onmicrosoft.com, **followed by** geoffgrisso@contoso.com.</span></span>
> * <span data-ttu-id="abb59-124">**Ne** adja hozzá a geoffgrisso@contoso.com címet a geoffgrisso@contoso.onmicrosoft.com hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="abb59-124">**Don't** add geoffgrisso@contoso.com before you add geoffgrisso@contoso.onmicrosoft.com.</span></span> <span data-ttu-id="abb59-125">A sorrend betartása fontos, és nehézkes lehet visszavonni a változásokat.</span><span class="sxs-lookup"><span data-stu-id="abb59-125">This order is important, and can be cumbersome to undo.</span></span>
>
>

## <a name="change-user-information"></a><span data-ttu-id="abb59-126">Felhasználói adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="abb59-126">Change user information</span></span>
<span data-ttu-id="abb59-127">Bármely felhasználói attribútumot módosíthat, kivéve az objektumazonosítót.</span><span class="sxs-lookup"><span data-stu-id="abb59-127">You can change any user attribute except for the object ID.</span></span>

1. <span data-ttu-id="abb59-128">Nyissa meg a címtárat.</span><span class="sxs-lookup"><span data-stu-id="abb59-128">Open your directory.</span></span>
2. <span data-ttu-id="abb59-129">Válassza a **Felhasználók** lapot, majd válassza ki a módosítani kívánt felhasználó megjelenítendő nevét.</span><span class="sxs-lookup"><span data-stu-id="abb59-129">Select the **Users** tab, and then select the display name of the user you want to change.</span></span>
3. <span data-ttu-id="abb59-130">Végezze el a módosításokat, majd kattintson a **Mentés** parancsra.</span><span class="sxs-lookup"><span data-stu-id="abb59-130">Complete your changes, and then click **Save**.</span></span>

<span data-ttu-id="abb59-131">Ha az épp módosított felhasználó szinkronizálva van a helyszíni Active Directory szolgáltatással, nem módosíthatja a felhasználó adatait ezzel az eljárással.</span><span class="sxs-lookup"><span data-stu-id="abb59-131">If the user that you're changing is synchronized with your on-premises Active Directory service, you can't change the user information using this procedure.</span></span> <span data-ttu-id="abb59-132">A felhasználó módosításához használja a helyszíni Active Directory-felügyeleti eszközöket.</span><span class="sxs-lookup"><span data-stu-id="abb59-132">To change the user, use your on-premises Active Directory management tools.</span></span>

## <a name="guest-user-management-and-limitations"></a><span data-ttu-id="abb59-133">Vendégfelhasználók kezelése és korlátozásai</span><span class="sxs-lookup"><span data-stu-id="abb59-133">Guest user management and limitations</span></span>
<span data-ttu-id="abb59-134">A vendégfiókok másik címtárakban lévő olyan felhasználók, akik meg lettek hívva a címtárába a SharePoint-dokumentumok, alkalmazások vagy más Azure-erőforrások elérése céljából.</span><span class="sxs-lookup"><span data-stu-id="abb59-134">Guest accounts are users from other directories who were invited to your directory to access SharePoint documents, applications, or other Azure resources.</span></span> <span data-ttu-id="abb59-135">A címtárban lévő vendégfiókok mögöttes UserType attribútuma „Vendég” beállítású.</span><span class="sxs-lookup"><span data-stu-id="abb59-135">A guest account in your directory has its underlying UserType attribute set to "Guest."</span></span> <span data-ttu-id="abb59-136">A normál felhasználók (különösen a címtár tagjai) UserType attribútuma „Tag”.</span><span class="sxs-lookup"><span data-stu-id="abb59-136">Regular users (specifically, members of your directory) have the UserType attribute "Member."</span></span>

<span data-ttu-id="abb59-137">A vendégek korlátozott jogosultságokkal rendelkeznek a címtárban.</span><span class="sxs-lookup"><span data-stu-id="abb59-137">Guests have a limited set of rights in the directory.</span></span> <span data-ttu-id="abb59-138">Ezek a jogosultságok korlátozzák a vendégek azon képességét, hogy a címtár más felhasználóinak adatait felderítsék.</span><span class="sxs-lookup"><span data-stu-id="abb59-138">These rights limit the ability for Guests to discover information about other users in the directory.</span></span> <span data-ttu-id="abb59-139">De a vendégfelhasználók továbbra is kommunikálhatnak azon erőforrásokkal társított felhasználókkal és csoportokkal, amelyeken dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="abb59-139">However, guest users can still interact with the users and groups associated with the resources they're working on.</span></span> <span data-ttu-id="abb59-140">A vendégfelhasználók a következőkre képesek:</span><span class="sxs-lookup"><span data-stu-id="abb59-140">Guest users can:</span></span>

* <span data-ttu-id="abb59-141">Azon Azure-előfizetéssel társított felhasználók és csoportok megtekintése, amelyhez hozzá vannak rendelve.</span><span class="sxs-lookup"><span data-stu-id="abb59-141">See other users and groups associated with an Azure subscription to which they're assigned</span></span>
* <span data-ttu-id="abb59-142">Azon csoportok tagjainak megtekintése, amelyekhez tartoznak.</span><span class="sxs-lookup"><span data-stu-id="abb59-142">See the members of groups to which they belong</span></span>
* <span data-ttu-id="abb59-143">A címtárban lévő többi felhasználó megkeresése, ha ismerik a felhasználó teljes e-mail-címét.</span><span class="sxs-lookup"><span data-stu-id="abb59-143">Look up other users in the directory, if they know the full email address of the user</span></span>
* <span data-ttu-id="abb59-144">A megkeresett felhasználók attribútumainak korlátozott megtekintése – ez a megjelenítendő névre, az e-mail-címre, az egyszerű felhasználónévre (UPN-re) és a miniatűr fényképre van korlátozva.</span><span class="sxs-lookup"><span data-stu-id="abb59-144">See only a limited set of attributes of the users they look up--limited to display name, email address, user principal name (UPN), and thumbnail photo</span></span>
* <span data-ttu-id="abb59-145">A címtárban lévő ellenőrzött tartományok listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="abb59-145">Get a list of verified domains in the directory</span></span>
* <span data-ttu-id="abb59-146">Hozzájárulás megadása az alkalmazásoknak, ugyanazon hozzáférést adva nekik, mint a címtár tagjainak.</span><span class="sxs-lookup"><span data-stu-id="abb59-146">Consent to applications, granting them the same access that Members have in your directory</span></span>

## <a name="set-guest-user-access-policies"></a><span data-ttu-id="abb59-147">A vendégfelhasználók hozzáférési házirendjeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="abb59-147">Set guest user access policies</span></span>
<span data-ttu-id="abb59-148">A címtár **Konfigurálás** lapja tartalmazza a vendégfelhasználók hozzáférés-vezérlésének lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="abb59-148">The **Configure** tab of a directory includes options to control access for guest users.</span></span> <span data-ttu-id="abb59-149">Ezek a lehetőségek csak a klasszikus Azure portálon módosíthatók a címtár globális rendszergazdája által.</span><span class="sxs-lookup"><span data-stu-id="abb59-149">These options can be changed only in Azure classic portal by a directory global administrator.</span></span> <span data-ttu-id="abb59-150">Jelenleg erre a célra nem létezik PowerShell- vagy API-módszer.</span><span class="sxs-lookup"><span data-stu-id="abb59-150">Currently, there's no PowerShell or API method.</span></span>

<span data-ttu-id="abb59-151">A klasszikus Azure portálon a **Konfigurálás** lap megnyitásához válassza az **Active Directory** lehetőséget, majd jelölje ki a címtár nevét.</span><span class="sxs-lookup"><span data-stu-id="abb59-151">To open the **Configure** tab in the Azure classic portal, select **Active Directory**, and then select the name of the directory.</span></span>

![Konfigurálás lap az Azure Active Directoryban][1]

<span data-ttu-id="abb59-153">Ezután szerkesztheti a vendégfelhasználók hozzáférés-vezérlésének lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="abb59-153">Then you can edit the options to control access for guest users.</span></span>

![hozzáférés-vezérlési lehetőségek a vendégfelhasználók számára][2]

## <a name="whats-next"></a><span data-ttu-id="abb59-155">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="abb59-155">What's next</span></span>
* [<span data-ttu-id="abb59-156">Más címtárakban vagy partnervállalatokban lévő felhasználók felvétele az Azure Active Directoryba</span><span class="sxs-lookup"><span data-stu-id="abb59-156">Add users from other directories or partner companies in Azure Active Directory</span></span>](active-directory-create-users-external.md)
* [<span data-ttu-id="abb59-157">Az Azure AD felügyelete</span><span class="sxs-lookup"><span data-stu-id="abb59-157">Administering Azure AD</span></span>](active-directory-administer.md)
* [<span data-ttu-id="abb59-158">Jelszavak kezelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="abb59-158">Manage passwords in Azure AD</span></span>](active-directory-manage-passwords.md)
* [<span data-ttu-id="abb59-159">Csoportok kezelése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="abb59-159">Manage groups in Azure AD</span></span>](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
