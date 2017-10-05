---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6f7616e47322242f01a13d763f71c93d4ac06a92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="f9bfe-103">Oktatóanyag: Üzleti Dropbox konfigurálása automatikus felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="f9bfe-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="f9bfe-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Dropbox és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Dropbox vállalati mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-104">The objective of this tutorial is to show you the steps you need to perform in Dropbox for Business and Azure AD to automatically provision and de-provision user accounts from Azure AD to Dropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9bfe-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f9bfe-105">Prerequisites</span></span>

<span data-ttu-id="f9bfe-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="f9bfe-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="f9bfe-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="f9bfe-108">A Dropbox az üzleti egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="f9bfe-109">Egy felhasználói fiókot az üzleti Dropbox Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-to-dropbox-for-business"></a><span data-ttu-id="f9bfe-110">A dropbox alkalmazásba vállalati felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f9bfe-110">Assigning users to Dropbox for Business</span></span>

<span data-ttu-id="f9bfe-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="f9bfe-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="f9bfe-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a dropbox-ba való hozzáférés üzleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Dropbox for Business app.</span></span> <span data-ttu-id="f9bfe-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat a Dropbox alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="f9bfe-114">Once decided, you can assign these users to your Dropbox for Business app by following the instructions here:</span></span>

[<span data-ttu-id="f9bfe-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f9bfe-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a><span data-ttu-id="f9bfe-116">Fontos tippek a Dropbox vállalati felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f9bfe-116">Important tips for assigning users to Dropbox for Business</span></span>

*   <span data-ttu-id="f9bfe-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve a Dropbox vállalati teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-117">It is recommended that a single Azure AD user is assigned to Dropbox for Business to test the provisioning configuration.</span></span> <span data-ttu-id="f9bfe-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="f9bfe-119">Amikor egy felhasználó rendel a Dropbox vállalati, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-119">When assigning a user to Dropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="f9bfe-120">A "Default" szerepkör nem működik a kiépítés...</span><span class="sxs-lookup"><span data-stu-id="f9bfe-120">The "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="f9bfe-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f9bfe-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="f9bfe-122">Ez a szakasz végigvezeti az Azure AD az üzleti a felhasználói fiók kiépítése API dropbox-ba való csatlakozáshoz és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Dropbox vállalati felhasználók és csoportok alapján az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-122">This section guides you through connecting your Azure AD to Dropbox for Business's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="f9bfe-123">Dönthet úgy is, engedélyezze SAML-alapú egyszeri bejelentkezést a vállalati Dropbox, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f9bfe-123">You may also choose to enabled SAML-based Single Sign-On for Dropbox for Business, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f9bfe-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="f9bfe-125">Konfigurálása automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="f9bfe-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="f9bfe-126">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="f9bfe-127">Ha már konfigurálta a vállalati Dropbox az egyszeri bejelentkezést, keresse meg az üzleti használja a keresőmezőt Dropbox-példány.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using the search field.</span></span> <span data-ttu-id="f9bfe-128">Máskülönben válassza **Hozzáadás** keresse meg a **vállalati Dropbox** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-128">Otherwise, select **Add** and search for **Dropbox for Business** in the application gallery.</span></span> <span data-ttu-id="f9bfe-129">Válassza ki a Dropbox vállalati a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-129">Select Dropbox for Business from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="f9bfe-130">Jelölje ki a Dropbox vállalati példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-130">Select your instance of Dropbox for Business, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="f9bfe-131">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-131">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="f9bfe-133">Az a **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-133">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="f9bfe-134">A Dropbox bejelentkezési párbeszédpanel üzleti egy új böngészőablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="f9bfe-135">Az a **jelentkezzen be az Azure ad-val csatolásához Dropbox** párbeszédpanel, jelentkezzen be a Dropbox üzleti bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-135">On the **Sign-in to Dropbox to link with Azure AD** dialog, sign in to your Dropbox for Business tenant.</span></span>

     <span data-ttu-id="f9bfe-136">![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="f9bfe-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="f9bfe-137">Győződjön meg arról, hogy szeretné-e a Dropbox üzleti bérlői módosításokat Azure Active Directory engedélyt.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-137">Confirm that you would like to give Azure Active Directory permission to make changes to your Dropbox for Business tenant.</span></span> <span data-ttu-id="f9bfe-138">Kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="f9bfe-139">![A felhasználók átadása](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="f9bfe-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="f9bfe-140">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD alkalmazás a Dropbox kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-140">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Dropbox for Business app.</span></span> <span data-ttu-id="f9bfe-141">Ha a kapcsolódás sikertelen, a vállalati fiók Team rendszergazdai jogosultságokkal rendelkezik, győződjön meg arról, hogy a Dropbox, és próbálkozzon a **"Engedélyezés"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-141">If the connection fails, ensure your Dropbox for Business account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="f9bfe-142">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-142">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="f9bfe-143">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="f9bfe-143">Click **Save.**</span></span>

11. <span data-ttu-id="f9bfe-144">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók a vállalati dropbox alkalmazásba.**</span><span class="sxs-lookup"><span data-stu-id="f9bfe-144">Under the Mappings section, select **Synchronize Azure Active Directory Users to Dropbox for Business.**</span></span>

12. <span data-ttu-id="f9bfe-145">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumokat a dropbox alkalmazásba vállalati szinkronizált az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-145">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Dropbox for Business.</span></span> <span data-ttu-id="f9bfe-146">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Dropbox vállalati a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-146">The attributes selected as **Matching** properties are used to match the user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="f9bfe-147">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-147">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="f9bfe-148">Az Azure AD vállalati Dropbox szolgáltatáshoz kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="f9bfe-148">To enable the Azure AD provisioning service for Dropbox for Business, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="f9bfe-149">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="f9bfe-149">Click **Save.**</span></span>

<span data-ttu-id="f9bfe-150">A kezdeti szinkronizálás bármely felhasználói és/vagy a dropbox-ba, a felhasználók és csoportok szakaszban vállalati rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-150">It starts the initial synchronization of any users and/or groups assigned to Dropbox for Business in the Users and Groups section.</span></span> <span data-ttu-id="f9bfe-151">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-151">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="f9bfe-152">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amely alkalmazás a Dropbox a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-152">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="f9bfe-153">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-153">You can now create a test account.</span></span> <span data-ttu-id="f9bfe-154">Várjon 20 percig győződjön meg arról, hogy a szinkronizált fiókkal a vállalati dropbox alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-154">Wait for up to 20 minutes to verify that the account has been synchronized to Dropbox for Business.</span></span>

<span data-ttu-id="f9bfe-155">A sikeresen befejezett felhasználók átadásához ciklus kapcsolódó állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="f9bfe-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="f9bfe-156">![Felhasználók hozzárendelése](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="f9bfe-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f9bfe-157">További források</span><span class="sxs-lookup"><span data-stu-id="f9bfe-157">Additional resources</span></span>

* [<span data-ttu-id="f9bfe-158">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="f9bfe-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f9bfe-159">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f9bfe-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f9bfe-160">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f9bfe-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)