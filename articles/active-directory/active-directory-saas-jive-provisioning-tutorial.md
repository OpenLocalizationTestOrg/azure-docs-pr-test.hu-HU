---
title: "Oktatóanyag: Azure Active Directoryval integrált Jive |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Jive között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="28272-103">Oktatóanyag: A felhasználók átadása Jive konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28272-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="28272-104">Ez az oktatóanyag célja mutatjuk be, végezze el a Jive és az Azure AD automatikus kiépítés és deaktiválás rendelkezés felhasználói fiókok Azure AD-Jive lépéseket.</span><span class="sxs-lookup"><span data-stu-id="28272-104">The objective of this tutorial is to show you the steps you need to perform in Jive and Azure AD to automatically provision and de-provision user accounts from Azure AD to Jive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28272-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28272-105">Prerequisites</span></span>

<span data-ttu-id="28272-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="28272-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="28272-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="28272-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="28272-108">Egy Jive egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="28272-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="28272-109">Egy felhasználói fiókot az Jive Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="28272-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-to-jive"></a><span data-ttu-id="28272-110">Felhasználók hozzárendelése Jive</span><span class="sxs-lookup"><span data-stu-id="28272-110">Assigning users to Jive</span></span>

<span data-ttu-id="28272-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="28272-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="28272-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="28272-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="28272-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az Jive alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="28272-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Jive app.</span></span> <span data-ttu-id="28272-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az Jive alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="28272-114">Once decided, you can assign these users to your Jive app by following the instructions here:</span></span>

[<span data-ttu-id="28272-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="28272-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a><span data-ttu-id="28272-116">Felhasználók hozzárendelése Jive fontos tippek</span><span class="sxs-lookup"><span data-stu-id="28272-116">Important tips for assigning users to Jive</span></span>

*   <span data-ttu-id="28272-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó a kiépítési konfigurációjának tesztelése rendelendő Jive.</span><span class="sxs-lookup"><span data-stu-id="28272-117">It is recommended that a single Azure AD user be assigned to Jive to test the provisioning configuration.</span></span> <span data-ttu-id="28272-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="28272-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="28272-119">Amikor egy felhasználó hozzárendelése Jive, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="28272-119">When assigning a user to Jive, you must select a valid user role.</span></span> <span data-ttu-id="28272-120">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="28272-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="28272-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="28272-121">Enable User Provisioning</span></span>

<span data-ttu-id="28272-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás Jive a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú Jive hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="28272-122">This section guides you through connecting your Azure AD to Jive's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="28272-123">Dönthet úgy is, SAML-alapú egyszeri bejelentkezés Jive számára engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="28272-123">You may also choose to enabled SAML-based Single Sign-On for Jive, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="28272-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="28272-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="28272-125">Konfigurálhatja a felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="28272-125">To configure user account provisioning:</span></span>

<span data-ttu-id="28272-126">Ez a szakasz célja engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókoknak az Jive felvázoló.</span><span class="sxs-lookup"><span data-stu-id="28272-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Jive.</span></span>
<span data-ttu-id="28272-127">Ez az eljárás részeként meg kell beszereznie Jive.com felhasználó biztonsági jogkivonat szükség.</span><span class="sxs-lookup"><span data-stu-id="28272-127">As part of this procedure, you are required to provide a user security token you need to request from Jive.com.</span></span>

1. <span data-ttu-id="28272-128">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="28272-128">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="28272-129">Ha már konfigurált Jive egyszeri bejelentkezést, keresse meg a keresési mező Jive példányát.</span><span class="sxs-lookup"><span data-stu-id="28272-129">If you have already configured Jive for single sign-on, search for your instance of Jive using the search field.</span></span> <span data-ttu-id="28272-130">Máskülönben válassza **Hozzáadás** keresse meg a **Jive** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="28272-130">Otherwise, select **Add** and search for **Jive** in the application gallery.</span></span> <span data-ttu-id="28272-131">Válassza ki a Jive a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="28272-131">Select Jive from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="28272-132">Jelölje ki a Jive példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="28272-132">Select your instance of Jive, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="28272-133">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="28272-133">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="28272-135">Az a **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="28272-135">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="28272-136">a.</span><span class="sxs-lookup"><span data-stu-id="28272-136">a.</span></span> <span data-ttu-id="28272-137">Az a **Jive rendszergazda felhasználóneve** szövegmezőhöz egy Jive a fióknevet, amelynek típusa a **rendszergazda** Jive.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="28272-137">In the **Jive Admin User Name** textbox, type a Jive account name that has the **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="28272-138">b.</span><span class="sxs-lookup"><span data-stu-id="28272-138">b.</span></span> <span data-ttu-id="28272-139">Az a **Jive rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="28272-139">In the **Jive Admin Password** textbox, type the password for this account.</span></span>
   
    <span data-ttu-id="28272-140">c.</span><span class="sxs-lookup"><span data-stu-id="28272-140">c.</span></span> <span data-ttu-id="28272-141">Az a **Jive bérlői URL-cím** szövegmezőhöz Jive bérlői URL-címét.</span><span class="sxs-lookup"><span data-stu-id="28272-141">In the **Jive Tenant URL** textbox, type the Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="28272-142">A Jive bérlői URL-cím bejelentkezni Jive a szervezet által használt URL-cím.</span><span class="sxs-lookup"><span data-stu-id="28272-142">The Jive tenant URL is URL that is used by your organization to log in to Jive.</span></span>  
      > <span data-ttu-id="28272-143">Általában az URL-cím formátuma a következő: **www.\< szervezet\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="28272-143">Typically, the URL has the following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="28272-144">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az Jive alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="28272-144">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Jive app.</span></span>

7. <span data-ttu-id="28272-145">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="28272-145">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

8. <span data-ttu-id="28272-146">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="28272-146">Click **Save.**</span></span>

9. <span data-ttu-id="28272-147">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat Jive.**</span><span class="sxs-lookup"><span data-stu-id="28272-147">Under the Mappings section, select **Synchronize Azure Active Directory Users to Jive.**</span></span>

10. <span data-ttu-id="28272-148">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD Jive lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="28272-148">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Jive.</span></span> <span data-ttu-id="28272-149">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Jive a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="28272-149">The attributes selected as **Matching** properties are used to match the user accounts in Jive for update operations.</span></span> <span data-ttu-id="28272-150">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="28272-150">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="28272-151">Az Azure AD szolgáltatás Jive kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="28272-151">To enable the Azure AD provisioning service for Jive, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="28272-152">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="28272-152">Click **Save.**</span></span>

<span data-ttu-id="28272-153">A kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Jive rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="28272-153">It starts the initial synchronization of any users and/or groups assigned to Jive in the Users and Groups section.</span></span> <span data-ttu-id="28272-154">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="28272-154">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="28272-155">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az Jive alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="28272-155">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Jive app.</span></span>

<span data-ttu-id="28272-156">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="28272-156">You can now create a test account.</span></span> <span data-ttu-id="28272-157">Akár 20 percig várjon győződjön meg arról, hogy a fiók Jive lett-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="28272-157">Wait for up to 20 minutes to verify that the account has been synchronized to Jive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28272-158">További források</span><span class="sxs-lookup"><span data-stu-id="28272-158">Additional resources</span></span>

* [<span data-ttu-id="28272-159">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="28272-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="28272-160">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="28272-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="28272-161">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28272-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)