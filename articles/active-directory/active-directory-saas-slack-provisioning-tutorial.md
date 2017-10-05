---
title: "Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Útmutató: Azure Active Directory konfigurálása automatikusan kiépítés és deaktiválás rendelkezés felhasználói fiókokhoz slackhez."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: 3cb49a4abb26c34a938c963c4cf326b5ccd490de
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a><span data-ttu-id="d6619-103">Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="d6619-103">Tutorial: Configuring Slack for Automatic User Provisioning</span></span>


<span data-ttu-id="d6619-104">Ez az oktatóanyag célja mutatjuk be, a lépéseket kell elvégeznie a Slackhez és az Azure AD automatikus kiépítés és deaktiválás rendelkezés felhasználói fiókok Azure ad-slackhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-104">The objective of this tutorial is to show you the steps you need to perform in Slack and Azure AD to automatically provision and de-provision user accounts from Azure AD to Slack.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d6619-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6619-105">Prerequisites</span></span>

<span data-ttu-id="d6619-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="d6619-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="d6619-107">Az Azure Active Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="d6619-107">An Azure Active Active directory tenant</span></span>
*   <span data-ttu-id="d6619-108">A Slackhez-bérlőben a [plusz terv](https://aadsyncfabric.slack.com/pricing) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="d6619-108">A Slack tenant with the [Plus plan](https://aadsyncfabric.slack.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="d6619-109">A Slackhez Team rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="d6619-109">A user account in Slack with Team Admin permissions</span></span> 

<span data-ttu-id="d6619-110">Megjegyzés: Az Azure AD-integrációs kiépítés támaszkodik a [Slackhez SCIM API](https://api.slack.com/scim) Slack csoportokhoz elérhető a plusz a terv és jobb.</span><span class="sxs-lookup"><span data-stu-id="d6619-110">Note: The Azure AD provisioning integration relies on the [Slack SCIM API](https://api.slack.com/scim) which is available to Slack teams on the Plus plan or better.</span></span>

## <a name="assigning-users-to-slack"></a><span data-ttu-id="d6619-111">Felhasználók hozzárendelése a Slackhez</span><span class="sxs-lookup"><span data-stu-id="d6619-111">Assigning users to Slack</span></span>

<span data-ttu-id="d6619-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="d6619-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="d6619-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="d6619-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="d6619-114">A létesítési szolgáltatás engedélyezése és konfigurálása, előtt kell döntenie, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az Slack alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="d6619-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to your Slack app.</span></span> <span data-ttu-id="d6619-115">Ha úgy döntött, itt cikk utasításait követve ezek a felhasználók rendelhet a Slack-alkalmazásba:</span><span class="sxs-lookup"><span data-stu-id="d6619-115">Once decided, you can assign these users to your Slack app by following the instructions here:</span></span>

[<span data-ttu-id="d6619-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d6619-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-slack"></a><span data-ttu-id="d6619-117">Felhasználók hozzárendelése a Slackhez fontos tippek</span><span class="sxs-lookup"><span data-stu-id="d6619-117">Important tips for assigning users to Slack</span></span>

*   <span data-ttu-id="d6619-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzárendelni Slackhez teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="d6619-118">It is recommended that a single Azure AD user be assigned to Slack to test the provisioning configuration.</span></span> <span data-ttu-id="d6619-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="d6619-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="d6619-120">Amikor egy felhasználó hozzárendelése a Slackhez, ki kell választania a **felhasználói** vagy a "Csoport" szerepkör-hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="d6619-120">When assigning a user to Slack, you must select the **User** or "Group" role in the assignment dialog.</span></span> <span data-ttu-id="d6619-121">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-slack"></a><span data-ttu-id="d6619-122">A felhasználók átadása slackhez konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6619-122">Configuring user provisioning to Slack</span></span> 

<span data-ttu-id="d6619-123">Ez a szakasz végigvezeti az Azure AD csatlakozás a Slackhez a felhasználói fiók kiépítése API, és a Slackhez, a felhasználó és csoport-hozzárendelés az Azure AD-alapú felhasználói fiókok létrehozásához, frissítéséhez és tiltsa le a létesítési szolgáltatás konfigurálása hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="d6619-123">This section guides you through connecting your Azure AD to Slack's user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in Slack based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="d6619-124">**Tipp:** is választhatja, hogy engedélyezze a SAML-alapú egyszeri bejelentkezés a Slackhez, utasításai (az Azure portálon) [https://portal.azure.com].</span><span class="sxs-lookup"><span data-stu-id="d6619-124">**Tip:** You may also choose to enabled SAML-based Single Sign-On for Slack, following the instructions provided in (Azure portal)[https://portal.azure.com].</span></span> <span data-ttu-id="d6619-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="d6619-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a><span data-ttu-id="d6619-126">Konfigurálása automatikus felhasználói fiók kiépítése slackhez Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="d6619-126">To configure automatic user account provisioning to Slack in Azure AD:</span></span>


1)  <span data-ttu-id="d6619-127">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d6619-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2) <span data-ttu-id="d6619-128">Ha már konfigurált Slackhez egyszeri bejelentkezést, keresse meg a Slackhez, használja a keresőmezőt példányát.</span><span class="sxs-lookup"><span data-stu-id="d6619-128">If you have already configured Slack for single sign-on, search for your instance of Slack using the search field.</span></span> <span data-ttu-id="d6619-129">Máskülönben válassza **Hozzáadás** keresse meg a **Slackhez** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="d6619-129">Otherwise, select **Add** and search for **Slack** in the application gallery.</span></span> <span data-ttu-id="d6619-130">A keresési eredmények közül válassza ki a Slackhez, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="d6619-130">Select Slack from the search results, and add it to your list of applications.</span></span>

3)  <span data-ttu-id="d6619-131">Jelölje ki a Slackhez példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="d6619-131">Select your instance of Slack, then select the **Provisioning** tab.</span></span>

4)  <span data-ttu-id="d6619-132">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="d6619-132">Set the **Provisioning Mode** to **Automatic**.</span></span>

![Slack-kiépítés](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  <span data-ttu-id="d6619-134">Az a **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="d6619-134">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="d6619-135">A Slack engedélyezési párbeszédpanel megnyílik egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="d6619-135">This opens a Slack authorization dialog in a new browser window.</span></span> 

6) <span data-ttu-id="d6619-136">Az új ablakban jelentkezzen be arra a Slackhez a csapat rendszergazda fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="d6619-136">In the new window, sign into Slack using your Team Admin account.</span></span> <span data-ttu-id="d6619-137">az eredményül kapott engedélyezési párbeszédpanelen válassza ki a Slack, amely engedélyezi a kiépítést, majd válassza ki **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="d6619-137">in the resulting authorization dialog, select the Slack team that you want to enable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="d6619-138">Ezt követően térjen vissza az Azure-portálon a létesítési konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-138">Once completed, return to the Azure portal to complete the provisioning configuration.</span></span>

![Engedélyezési párbeszédpanel](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) <span data-ttu-id="d6619-140">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a Slack alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d6619-140">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Slack app.</span></span> <span data-ttu-id="d6619-141">Ha nem sikerül, győződjön meg arról, a Slack fiók Team rendszergazdai jogosultságokkal rendelkezik, és ismételje meg az "Engedélyezés" lépést.</span><span class="sxs-lookup"><span data-stu-id="d6619-141">If the connection fails, ensure your Slack account has Team Admin permissions and try the "Authorize" step again.</span></span>

8) <span data-ttu-id="d6619-142">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d6619-142">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

9) <span data-ttu-id="d6619-143">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6619-143">Click **Save**.</span></span> 

10) <span data-ttu-id="d6619-144">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók slackhez**.</span><span class="sxs-lookup"><span data-stu-id="d6619-144">Under the Mappings section, select **Synchronize Azure Active Directory Users to Slack**.</span></span>

11) <span data-ttu-id="d6619-145">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok szinkronizálva lesznek az Azure AD az Slackhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-145">In the **Attribute Mappings** section, review the user attributes that will be synchronized from Azure AD to Slack.</span></span> <span data-ttu-id="d6619-146">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok felel meg a felhasználói fiókokat a Slackhez a frissítési műveletek használatával.</span><span class="sxs-lookup"><span data-stu-id="d6619-146">Note that the attributes selected as **Matching** properties will be used to match the user accounts in Slack for update operations.</span></span> <span data-ttu-id="d6619-147">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-147">Select the Save button to commit any changes.</span></span>

12) <span data-ttu-id="d6619-148">Az Azure AD szolgáltatás a Slackhez kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="d6619-148">To enable the Azure AD provisioning service for Slack, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

13) <span data-ttu-id="d6619-149">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6619-149">Click **Save**.</span></span> 

<span data-ttu-id="d6619-150">Elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban slackhez hozzárendelt csoportok.</span><span class="sxs-lookup"><span data-stu-id="d6619-150">This will start the initial synchronization of any users and/or groups assigned to Slack in the Users and Groups section.</span></span> <span data-ttu-id="d6619-151">Figyelje meg, hogy a kezdeti szinkronizálás hosszabb ideig tart hajtsa végre ezt követő szinkronizálások, amely körülbelül 10 percenként történik, amíg a szolgáltatás fut-nál.</span><span class="sxs-lookup"><span data-stu-id="d6619-151">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 10 minutes as long as the service is running.</span></span> <span data-ttu-id="d6619-152">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az közzététele a Slack-alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="d6619-152">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Slack app.</span></span>

## <a name="optional-configuring-group-object-provisioning-to-slack"></a><span data-ttu-id="d6619-153">[Választható] Csoport objektum kiépítés slackhez konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6619-153">[Optional] Configuring group object provisioning to Slack</span></span> 

<span data-ttu-id="d6619-154">Másik lehetőségként engedélyezheti a slackhez Azure ad-csoport objektumok kiépítését.</span><span class="sxs-lookup"><span data-stu-id="d6619-154">Optionally, you can enable the provisioning of group objects from Azure AD to Slack.</span></span> <span data-ttu-id="d6619-155">Ez eltér a "felhasználói csoportok hozzárendelése", az, hogy a tényleges csoport tagjai mellett objektumot a rendszer replikálja az Azure AD Slackhez.</span><span class="sxs-lookup"><span data-stu-id="d6619-155">This is different from "assigning groups of users", in that the actual group object in addition to its members will be replicated from Azure AD to Slack.</span></span> <span data-ttu-id="d6619-156">Például ha a "Saját csoport" nevű az Azure AD-csoport, egy "Saját csoport" nevű identitical csoportot az Slackhez belül létrejön.</span><span class="sxs-lookup"><span data-stu-id="d6619-156">For example, if you have a group named "My Group" in Azure AD, an identitical group named "My Group" will be created inside Slack.</span></span>

### <a name="to-enable-provisioning-of-group-objects"></a><span data-ttu-id="d6619-157">Az objektumok kiépítés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="d6619-157">To enable provisioning of group objects:</span></span>

1) <span data-ttu-id="d6619-158">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-csoportok slackhez**.</span><span class="sxs-lookup"><span data-stu-id="d6619-158">Under the Mappings section, select **Synchronize Azure Active Directory Groups to Slack**.</span></span>

2) <span data-ttu-id="d6619-159">Az attribútum hozzárendelése panelen beállítása engedélyezve igen.</span><span class="sxs-lookup"><span data-stu-id="d6619-159">In the Attribute Mapping blade, set Enabled to Yes.</span></span>

3) <span data-ttu-id="d6619-160">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a slackhez az Azure Active Directoryból szinkronizált csoport attribútumait.</span><span class="sxs-lookup"><span data-stu-id="d6619-160">In the **Attribute Mappings** section, review the group attributes that will be synchronized from Azure AD to Slack.</span></span> <span data-ttu-id="d6619-161">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok felel meg a frissítési műveleteket a Slackhez a csoportok használatával.</span><span class="sxs-lookup"><span data-stu-id="d6619-161">Note that the attributes selected as **Matching** properties will be used to match the groups in Slack for update operations.</span></span> 

4) <span data-ttu-id="d6619-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6619-162">Click **Save**.</span></span>

<span data-ttu-id="d6619-163">Ez bármilyen a Slackhez rendelt objektumok eredményez a **felhasználók és csoportok** Slackhez teljesen szinkronizált Azure ad-szakasz.</span><span class="sxs-lookup"><span data-stu-id="d6619-163">This result in any group objects assigned to Slack in the **Users and Groups** section being fully synchronized from Azure AD to Slack.</span></span> <span data-ttu-id="d6619-164">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az közzététele a Slack-alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="d6619-164">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Slack app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d6619-165">További források</span><span class="sxs-lookup"><span data-stu-id="d6619-165">Additional Resources</span></span>

* [<span data-ttu-id="d6619-166">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="d6619-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="d6619-167">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d6619-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
