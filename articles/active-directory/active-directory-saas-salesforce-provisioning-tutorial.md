---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a573a7ef79e28c50ae0923849a88f88af40f21be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a><span data-ttu-id="1aa8d-103">Oktatóanyag: Salesforce konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="1aa8d-103">Tutorial: Configuring Salesforce for Automatic User Provisioning</span></span>

<span data-ttu-id="1aa8d-104">Ez az oktatóanyag célja a Salesforce és az Azure AD automatikus kiépítéséhez elvégzéséhez szükséges lépéseket és deaktiválás rendelkezés felhasználói fiókok Azure ad-Salesforce megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-104">The objective of this tutorial is to show the steps required to perform in Salesforce and Azure AD to automatically provision and de-provision user accounts from Azure AD to Salesforce.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1aa8d-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1aa8d-105">Prerequisites</span></span>

<span data-ttu-id="1aa8d-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="1aa8d-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="1aa8d-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="1aa8d-108">Rendelkeznie kell egy érvényes bérlőt Salesforce a munkahelyén vagy a Salesforce oktatási célokra.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-108">You must have a valid tenant for Salesforce for Work or Salesforce for Education.</span></span> <span data-ttu-id="1aa8d-109">Egy ingyenes próbafiók vagy a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="1aa8d-110">Egy felhasználói fiókot a Salesforce-ban Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-110">A user account in Salesforce with Team Admin permissions.</span></span>

## <a name="assigning-users-to-salesforce"></a><span data-ttu-id="1aa8d-111">Felhasználók hozzárendelése Salesforce</span><span class="sxs-lookup"><span data-stu-id="1aa8d-111">Assigning users to Salesforce</span></span>

<span data-ttu-id="1aa8d-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="1aa8d-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="1aa8d-114">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a Salesforce alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Salesforce app.</span></span> <span data-ttu-id="1aa8d-115">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezek a felhasználók a Salesforce alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="1aa8d-115">Once decided, you can assign these users to your Salesforce app by following the instructions here:</span></span>

[<span data-ttu-id="1aa8d-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="1aa8d-116">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a><span data-ttu-id="1aa8d-117">Felhasználók hozzárendelése Salesforce fontos tippek</span><span class="sxs-lookup"><span data-stu-id="1aa8d-117">Important tips for assigning users to Salesforce</span></span>

*   <span data-ttu-id="1aa8d-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve Salesforce teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-118">It is recommended that a single Azure AD user is assigned to Salesforce to test the provisioning configuration.</span></span> <span data-ttu-id="1aa8d-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-119">Additional users and/or groups may be assigned later.</span></span>

*  <span data-ttu-id="1aa8d-120">Amikor egy felhasználó rendel a Salesforce, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-120">When assigning a user to Salesforce, you must select a valid user role.</span></span> <span data-ttu-id="1aa8d-121">A "Default" szerepkör nem működik történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="1aa8d-121">The "Default Access" role does not work for provisioning</span></span>

    > [!NOTE]
    > <span data-ttu-id="1aa8d-122">Ez az alkalmazás egyéni szerepkörök importál a telepítési folyamatot, amely az ügyfél esetleg szeretné kiválasztani a felhasználók hozzárendelésekor részeként Salesforce</span><span class="sxs-lookup"><span data-stu-id="1aa8d-122">This app imports custom roles from Salesforce as part of the provisioning process, which the customer may want to select when assigning users</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="1aa8d-123">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1aa8d-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="1aa8d-124">Ez a szakasz végigvezeti az Azure AD kapcsolódás Salesforce a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Salesforce alapján a felhasználók és csoportok hozzárendelése az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-124">This section guides you through connecting your Azure AD to Salesforce's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Salesforce based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="1aa8d-125">Dönthet úgy is, SAML-alapú egyszeri bejelentkezés Salesforce engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1aa8d-125">You may also choose to enabled SAML-based Single Sign-On for Salesforce, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1aa8d-126">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="1aa8d-127">Konfigurálása automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="1aa8d-127">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="1aa8d-128">Ez a szakasz célja felvázoló engedélyezése a felhasználók átadása Salesforce Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-128">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce.</span></span>

1. <span data-ttu-id="1aa8d-129">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="1aa8d-130">Ha már beállította az egyszeri bejelentkezés Salesforce, keresse meg az használja a keresőmezőt Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-130">If you have already configured Salesforce for single sign-on, search for your instance of Salesforce using the search field.</span></span> <span data-ttu-id="1aa8d-131">Máskülönben válassza **Hozzáadás** keresse meg a **Salesforce** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-131">Otherwise, select **Add** and search for **Salesforce** in the application gallery.</span></span> <span data-ttu-id="1aa8d-132">Válassza ki a Salesforce a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-132">Select Salesforce from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="1aa8d-133">Jelölje ki a Salesforce példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-133">Select your instance of Salesforce, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="1aa8d-134">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-134">Set the **Provisioning Mode** to **Automatic**.</span></span> 
<span data-ttu-id="1aa8d-135">![kiépítés](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="1aa8d-135">![provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="1aa8d-136">Az a **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1aa8d-136">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="1aa8d-137">a.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-137">a.</span></span> <span data-ttu-id="1aa8d-138">Az a **rendszergazda felhasználóneve** szövegmezőhöz a Salesforce-fióknév, amelynek típusa a **rendszergazda** Salesforce.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-138">In the **Admin User Name** textbox, type a Salesforce account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="1aa8d-139">b.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-139">b.</span></span> <span data-ttu-id="1aa8d-140">Az a **rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-140">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="1aa8d-141">A Salesforce biztonsági jogkivonatának beszerzéséhez, nyisson meg egy új lapon és a bejelentkezés Salesforce egy rendszergazdai fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-141">To get your Salesforce security token, open a new tab and sign into the same Salesforce admin account.</span></span> <span data-ttu-id="1aa8d-142">Az oldal jobb felső sarkában kattintson a nevére, és kattintson a **saját beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-142">On the top right corner of the page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="1aa8d-143">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1aa8d-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="1aa8d-144">A bal oldali navigációs ablaktábláján kattintson **személyes** bontsa ki a kapcsolódó csomópontot, majd **alaphelyzetbe állítani a biztonsági jogkivonat**.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-144">On the left navigation pane, click **Personal** to expand the related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="1aa8d-145">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1aa8d-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="1aa8d-146">A a **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** gombra.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-146">On the **Reset My Security Token** page, click **Reset Security Token** button.</span></span>

    <span data-ttu-id="1aa8d-147">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1aa8d-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="1aa8d-148">Ellenőrizze a rendszergazdai fiókhoz tartozó e-mailben kapják.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-148">Check the email inbox associated with this admin account.</span></span> <span data-ttu-id="1aa8d-149">Keresse meg a Salesforce.com az új biztonsági jogkivonatot tartalmazó e-mailt.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-149">Look for an email from Salesforce.com that contains the new security token.</span></span>
10. <span data-ttu-id="1aa8d-150">Másolja a token nyissa meg az Azure AD ablakba, és illessze be azt a **szoftvercsatorna Token** mező.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-150">Copy the token, go to your Azure AD window, and paste it into the **Socket Token** field.</span></span>

11. <span data-ttu-id="1aa8d-151">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a Salesforce alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-151">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Salesforce app.</span></span>

12. <span data-ttu-id="1aa8d-152">Az a **értesítő e-mailt** mezőbe írja be az e-mail cím vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-152">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox below.</span></span>

13. <span data-ttu-id="1aa8d-153">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="1aa8d-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="1aa8d-154">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók a Salesforce.**</span><span class="sxs-lookup"><span data-stu-id="1aa8d-154">Under the Mappings section, select **Synchronize Azure Active Directory Users to Salesforce.**</span></span>

15. <span data-ttu-id="1aa8d-155">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumokat a Salesforce szinkronizált Azure AD-ből.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-155">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Salesforce.</span></span> <span data-ttu-id="1aa8d-156">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Salesforce-ban a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-156">Note that the attributes selected as **Matching** properties are used to match the user accounts in Salesforce for update operations.</span></span> <span data-ttu-id="1aa8d-157">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-157">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="1aa8d-158">Az Azure AD szolgáltatás a Salesforce-kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="1aa8d-158">To enable the Azure AD provisioning service for Salesforce, change the **Provisioning Status** to **On** in the Settings section</span></span>

17. <span data-ttu-id="1aa8d-159">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="1aa8d-159">Click **Save.**</span></span>

<span data-ttu-id="1aa8d-160">Ezzel elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Salesforce rendelt csoportok.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-160">This starts the initial synchronization of any users and/or groups assigned to Salesforce in the Users and Groups section.</span></span> <span data-ttu-id="1aa8d-161">Figyelje meg, hogy a kezdeti szinkronizálás végrehajtásához bekövetkező körülbelül 20 percenként, mindaddig, amíg a szolgáltatás fut. ezt követő szinkronizálások hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-161">Note that the initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="1aa8d-162">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a Salesforce alkalmazást a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-162">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Salesforce app.</span></span>

<span data-ttu-id="1aa8d-163">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-163">You can now create a test account.</span></span> <span data-ttu-id="1aa8d-164">Akár 20 percig várjon győződjön meg arról, hogy a fiók a Salesforce lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="1aa8d-164">Wait for up to 20 minutes to verify that the account has been synchronized to Salesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1aa8d-165">További források</span><span class="sxs-lookup"><span data-stu-id="1aa8d-165">Additional resources</span></span>

* [<span data-ttu-id="1aa8d-166">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="1aa8d-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1aa8d-167">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1aa8d-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="1aa8d-168">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1aa8d-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforce-tutorial.md)