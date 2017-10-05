---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Salesforce védőfal között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 7d3c655a754f83284c386d2007c604a731367814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a><span data-ttu-id="b7930-103">Oktatóanyag: Salesforce védőfal konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="b7930-103">Tutorial: Configuring Salesforce Sandbox for Automatic User Provisioning</span></span>

<span data-ttu-id="b7930-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Salesforce védőfal és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Salesforce védőfal mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="b7930-104">The objective of this tutorial is to show you the steps you need to perform in Salesforce Sandbox and Azure AD to automatically provision and de-provision user accounts from Azure AD to Salesforce Sandbox.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7930-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7930-105">Prerequisites</span></span>

<span data-ttu-id="b7930-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="b7930-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="b7930-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="b7930-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="b7930-108">Rendelkeznie kell egy érvényes bérlő Salesforce védőfal munka vagy a Salesforce védőfal oktatási célokra.</span><span class="sxs-lookup"><span data-stu-id="b7930-108">You must have a valid tenant for Salesforce Sandbox for Work or Salesforce Sandbox for Education.</span></span> <span data-ttu-id="b7930-109">Egy ingyenes próbafiók vagy a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="b7930-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="b7930-110">Egy felhasználói fiókot a Salesforce védőfal Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="b7930-110">A user account in Salesforce Sandbox with Team Admin permissions.</span></span>

## <a name="assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="b7930-111">Felhasználók hozzárendelése Salesforce védőfal</span><span class="sxs-lookup"><span data-stu-id="b7930-111">Assigning users to Salesforce Sandbox</span></span>

<span data-ttu-id="b7930-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="b7930-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="b7930-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok számára "rendelt" az Azure AD alkalmazás szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="b7930-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="b7930-114">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az védőfal Salesforce alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="b7930-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Salesforce Sandbox app.</span></span> <span data-ttu-id="b7930-115">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezeket a felhasználókat a Salesforce védőfal app:</span><span class="sxs-lookup"><span data-stu-id="b7930-115">Once decided, you can assign these users to your Salesforce Sandbox app by following the instructions here:</span></span>

[<span data-ttu-id="b7930-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b7930-116">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="b7930-117">Felhasználók hozzárendelése Salesforce védőfal fontos tippek</span><span class="sxs-lookup"><span data-stu-id="b7930-117">Important tips for assigning users to Salesforce Sandbox</span></span>

* <span data-ttu-id="b7930-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve Salesforce védőfal teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="b7930-118">It is recommended that a single Azure AD user is assigned to Salesforce Sandbox to test the provisioning configuration.</span></span> <span data-ttu-id="b7930-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="b7930-119">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="b7930-120">Amikor egy felhasználó rendel a Salesforce védőfal, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="b7930-120">When assigning a user to Salesforce Sandbox, you must select a valid user role.</span></span> <span data-ttu-id="b7930-121">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7930-121">The "Default Access" role does not work for provisioning.</span></span>

> [!NOTE]
> <span data-ttu-id="b7930-122">Ez az alkalmazás egyéni szerepkörök importálja a Salesforce védőfal a telepítési folyamatot, amely az ügyfél esetleg szeretné kiválasztani a felhasználók hozzárendelésekor részeként.</span><span class="sxs-lookup"><span data-stu-id="b7930-122">This app imports custom roles from Salesforce Sandbox as part of the provisioning process, which the customer may want to select when assigning users.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="b7930-123">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b7930-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="b7930-124">Ez a szakasz végigvezeti az Azure AD kapcsolódás Salesforce védőfal felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Salesforce védőfal alapján a felhasználók és csoportok az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="b7930-124">This section guides you through connecting your Azure AD to Salesforce Sandbox's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Salesforce Sandbox based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="b7930-125">Választhatja azt is, SAML-alapú egyszeri bejelentkezés Salesforce védőfal engedélyezve van, az utasításokat követve megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b7930-125">You may also choose to enabled SAML-based Single Sign-On for Salesforce Sandbox, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b7930-126">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="b7930-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="b7930-127">Konfigurálása automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="b7930-127">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="b7930-128">Ez a szakasz célja felvázoló engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókoknak Salesforce védőfal felé.</span><span class="sxs-lookup"><span data-stu-id="b7930-128">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce Sandbox.</span></span>

1. <span data-ttu-id="b7930-129">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b7930-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="b7930-130">Ha az egyszeri bejelentkezés Salesforce védőfal már beállította, keresse meg a Salesforce védőfal használja a keresőmezőt példányát.</span><span class="sxs-lookup"><span data-stu-id="b7930-130">If you have already configured Salesforce Sandbox for single sign-on, search for your instance of Salesforce Sandbox using the search field.</span></span> <span data-ttu-id="b7930-131">Máskülönben válassza **Hozzáadás** keresse meg a **Salesforce védőfal** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="b7930-131">Otherwise, select **Add** and search for **Salesforce Sandbox** in the application gallery.</span></span> <span data-ttu-id="b7930-132">Válassza ki a Salesforce védőfal a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b7930-132">Select Salesforce Sandbox from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="b7930-133">Jelölje ki a Salesforce védőfal példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7930-133">Select your instance of Salesforce Sandbox, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="b7930-134">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="b7930-134">Set the **Provisioning Mode** to **Automatic**.</span></span> 
    <span data-ttu-id="b7930-135">![kiépítés](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="b7930-135">![provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="b7930-136">Az a **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b7930-136">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="b7930-137">a.</span><span class="sxs-lookup"><span data-stu-id="b7930-137">a.</span></span> <span data-ttu-id="b7930-138">Az a **rendszergazda felhasználóneve** szövegmezőhöz Salesforce védőfalat a fióknevet, amelynek típusa a **rendszergazda** Salesforce.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="b7930-138">In the **Admin User Name** textbox, type a Salesforce Sandbox account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="b7930-139">b.</span><span class="sxs-lookup"><span data-stu-id="b7930-139">b.</span></span> <span data-ttu-id="b7930-140">Az a **rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b7930-140">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="b7930-141">A Salesforce védőfal biztonsági jogkivonatának beszerzéséhez, nyisson meg egy új lapon és a bejelentkezés Salesforce védőfal egy rendszergazdai fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b7930-141">To get your Salesforce Sandbox security token, open a new tab and sign into the same Salesforce Sandbox admin account.</span></span> <span data-ttu-id="b7930-142">Az oldal jobb felső sarkában kattintson a nevére, és kattintson a **saját beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b7930-142">On the top right corner of the page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="b7930-143">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="b7930-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="b7930-144">A bal oldali navigációs ablaktábláján kattintson **személyes** bontsa ki a kapcsolódó csomópontot, majd **alaphelyzetbe állítani a biztonsági jogkivonat**.</span><span class="sxs-lookup"><span data-stu-id="b7930-144">On the left navigation pane, click **Personal** to expand the related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="b7930-145">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="b7930-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="b7930-146">Az a **alaphelyzetbe állítani a biztonsági jogkivonat** lapján kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat** gombra.</span><span class="sxs-lookup"><span data-stu-id="b7930-146">On the **Reset My Security Token** page, click the **Reset Security Token** button.</span></span>

    <span data-ttu-id="b7930-147">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="b7930-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="b7930-148">Ellenőrizze a rendszergazdai fiókhoz tartozó e-mailben kapják.</span><span class="sxs-lookup"><span data-stu-id="b7930-148">Check the email inbox associated with this admin account.</span></span> <span data-ttu-id="b7930-149">Keresse meg a Salesforce Sandbox.com az új biztonsági jogkivonatot tartalmazó e-mailt.</span><span class="sxs-lookup"><span data-stu-id="b7930-149">Look for an email from Salesforce Sandbox.com that contains the new security token.</span></span>
10. <span data-ttu-id="b7930-150">Másolja a token nyissa meg az Azure AD ablakba, és illessze be azt a **szoftvercsatorna Token** mező.</span><span class="sxs-lookup"><span data-stu-id="b7930-150">Copy the token, go to your Azure AD window, and paste it into the **Socket Token** field.</span></span>

11. <span data-ttu-id="b7930-151">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a Salesforce védőfal alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7930-151">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Salesforce Sandbox app.</span></span>

12. <span data-ttu-id="b7930-152">Az a **értesítő e-mailt** mezőbe írja be az e-mail cím vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="b7930-152">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

13. <span data-ttu-id="b7930-153">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b7930-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="b7930-154">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók Salesforce védőfal felé.**</span><span class="sxs-lookup"><span data-stu-id="b7930-154">Under the Mappings section, select **Synchronize Azure Active Directory Users to Salesforce Sandbox.**</span></span>

15. <span data-ttu-id="b7930-155">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok szinkronizált Azure ad-Salesforce védőfal felé.</span><span class="sxs-lookup"><span data-stu-id="b7930-155">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Salesforce Sandbox.</span></span> <span data-ttu-id="b7930-156">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Salesforce védőfal a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="b7930-156">The attributes selected as **Matching** properties are used to match the user accounts in Salesforce Sandbox for update operations.</span></span> <span data-ttu-id="b7930-157">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7930-157">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="b7930-158">Az Azure AD szolgáltatás Salesforce védőfal kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="b7930-158">To enable the Azure AD provisioning service for Salesforce Sandbox, change the **Provisioning Status** to **On** in the Settings section</span></span>

17. <span data-ttu-id="b7930-159">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b7930-159">Click **Save.**</span></span>


<span data-ttu-id="b7930-160">A kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Salesforce védőfal rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="b7930-160">It starts the initial synchronization of any users and/or groups assigned to Salesforce Sandbox in the Users and Groups section.</span></span> <span data-ttu-id="b7930-161">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b7930-161">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="b7930-162">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amelynek Salesforce védőfal app a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="b7930-162">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on Salesforce Sandbox app.</span></span>

<span data-ttu-id="b7930-163">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="b7930-163">You can now create a test account.</span></span> <span data-ttu-id="b7930-164">Akár 20 percig várjon győződjön meg arról, hogy a fiók a salesforce lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="b7930-164">Wait for up to 20 minutes to verify that the account has been synchronized to salesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7930-165">További források</span><span class="sxs-lookup"><span data-stu-id="b7930-165">Additional resources</span></span>

* [<span data-ttu-id="b7930-166">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="b7930-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7930-167">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b7930-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b7930-168">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7930-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforcesandbox-tutorial.md)