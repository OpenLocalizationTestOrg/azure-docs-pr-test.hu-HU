---
title: "Oktatóanyag: Azure Active Directoryval integrált mezőben |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a mező között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9f061f3f5a0a4825854b893150ceccc8951487de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a><span data-ttu-id="86053-103">Oktatóanyag: Mezőben konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="86053-103">Tutorial: Configuring Box for Automatic User Provisioning</span></span>

<span data-ttu-id="86053-104">Ez az oktatóanyag célja szemlélteti a lépéseket kell elvégeznie a mezőbe, és az Azure AD automatikus kiépítés és deaktiválás rendelkezés felhasználói fiókok Azure ad-mezőben.</span><span class="sxs-lookup"><span data-stu-id="86053-104">The objective of this tutorial is to show the steps you need to perform in Box and Azure AD to automatically provision and de-provision user accounts from Azure AD to Box.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86053-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="86053-105">Prerequisites</span></span>

<span data-ttu-id="86053-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="86053-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="86053-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="86053-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="86053-108">A mezőben egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="86053-108">A Box single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="86053-109">Egy felhasználói fiókot mezőben Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="86053-109">A user account in Box with Team Admin permissions.</span></span>

## <a name="assigning-users-to-box"></a><span data-ttu-id="86053-110">Felhasználók hozzárendelése mezőbe</span><span class="sxs-lookup"><span data-stu-id="86053-110">Assigning users to Box</span></span> 

<span data-ttu-id="86053-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="86053-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="86053-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="86053-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="86053-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a Box alkalmazásához a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="86053-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Box app.</span></span> <span data-ttu-id="86053-114">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezeket a felhasználókat a Box alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="86053-114">Once decided, you can assign these users to your Box app by following the instructions here:</span></span>

[<span data-ttu-id="86053-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="86053-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a><span data-ttu-id="86053-116">Felhasználók és csoportok hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="86053-116">Assign users and groups</span></span>
<span data-ttu-id="86053-117">A **mezőben > felhasználók és csoportok** az Azure-portálon állíthatja be, hogy mely felhasználók és csoportok kell hozzáférést adjon meg.</span><span class="sxs-lookup"><span data-stu-id="86053-117">The **Box > Users and Groups** tab in the Azure portal allows you to specify which users and groups should be granted access to Box.</span></span> <span data-ttu-id="86053-118">Egy felhasználó vagy csoport hozzárendelése hatására megtörténik a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="86053-118">Assignment of a user or group causes the following things to occur:</span></span>

* <span data-ttu-id="86053-119">Az Azure AD való hitelesítésre szolgáló mezőben lehetővé teszi a kijelölt felhasználó (akár közvetlen címhozzárendelési vagy csoporttagság).</span><span class="sxs-lookup"><span data-stu-id="86053-119">Azure AD permits the assigned user (either by direct assignment or group membership) to authenticate to Box.</span></span> <span data-ttu-id="86053-120">Ha a felhasználó nem tartozik, az Azure AD engedi jelentkezzen be a mezőbe, és az Azure AD bejelentkezési lapon hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="86053-120">If a user is not assigned, then Azure AD does not permit them to sign in to Box and returns an error on the Azure AD sign-in page.</span></span>
* <span data-ttu-id="86053-121">Az alkalmazás csempéjére a hozzá tartozó mezőben kerül a felhasználó [alkalmazásindító](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="86053-121">An app tile for Box is added to the user's [application launcher](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>
* <span data-ttu-id="86053-122">Ha automatikus kiépítés engedélyezve van, majd a kijelölt felhasználók és/vagy csoportok kerülnek a kiépítési várólista automatikusan építeni.</span><span class="sxs-lookup"><span data-stu-id="86053-122">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
  
  * <span data-ttu-id="86053-123">Ha csak a felhasználói objektumok be lett állítva, úgy kell létrehozni, majd kerülnek, a telepítési várólistán lévő összes közvetlenül hozzárendelt felhasználó, és minden olyan felhasználó, amelyek bármely hozzárendelt csoportok tagjai kerülnek be a létesítési várólistába.</span><span class="sxs-lookup"><span data-stu-id="86053-123">If only user objects were configured to be provisioned, then all directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
  * <span data-ttu-id="86053-124">Objektumok be lett állítva, úgy kell létrehozni, ha majd hozzárendelt csoportobjektumok törlődnek, és minden olyan felhasználó, hogy ezek a csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="86053-124">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="86053-125">A csoport és a felhasználó tagságát után mezőbe írt megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="86053-125">The group and user memberships are preserved upon being written to Box.</span></span>

<span data-ttu-id="86053-126">Használhatja a **attribútumok > egyszeri bejelentkezés** lapján adja meg, melyik felhasználói attribútumok (vagy jogcímeket) mutatnak be mezőben SAML-alapú hitelesítés során és a **attribútumok > kiépítési** lap segítségével konfigurálhatja, hogyan felhasználói és csoportattribútum flow az Azure AD mezőben műveletek kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="86053-126">You can use the **Attributes > Single Sign-On** tab to configure which user attributes (or claims) are presented to Box during SAML-based authentication, and the **Attributes > Provisioning** tab to configure how user and group attributes flow from Azure AD to Box during provisioning operations.</span></span>

### <a name="important-tips-for-assigning-users-to-box"></a><span data-ttu-id="86053-127">Felhasználók hozzárendelése mezőben fontos tippek</span><span class="sxs-lookup"><span data-stu-id="86053-127">Important tips for assigning users to Box</span></span> 

*   <span data-ttu-id="86053-128">Javasoljuk, hogy egyetlen Azure AD-felhasználó mezőhöz rendelt tesztelje a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="86053-128">It is recommended that a single Azure AD user assigned to Box to test the provisioning configuration.</span></span> <span data-ttu-id="86053-129">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="86053-129">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="86053-130">Amikor egy felhasználó rendel a mezőbe, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="86053-130">When assigning a user to box, you must select a valid user role.</span></span> <span data-ttu-id="86053-131">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="86053-131">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="86053-132">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="86053-132">Enable Automated User Provisioning</span></span>

<span data-ttu-id="86053-133">Ez a szakasz végigvezeti a csatlakozás az Azure AD lista felhasználói fiókhoz kiépítés API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok alapján, az Azure AD-felhasználók és csoportok hozzárendelése a.</span><span class="sxs-lookup"><span data-stu-id="86053-133">This section guides through connecting your Azure AD to Box's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Box based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="86053-134">Ha automatikus kiépítés engedélyezve van, majd a kijelölt felhasználók és/vagy csoportok kerülnek a kiépítési várólista automatikusan építeni.</span><span class="sxs-lookup"><span data-stu-id="86053-134">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
    
 * <span data-ttu-id="86053-135">Ha csak a felhasználói objektumok úgy kell létrehozni, akkor közvetlenül hozzárendelt felhasználók kerülnek be a létesítési várólistába, és minden olyan felhasználó, amelyek bármely hozzárendelt csoportok tagjai kerülnek be a létesítési várólistába vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="86053-135">If only user objects are configured to be provisioned, then directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
    
 * <span data-ttu-id="86053-136">Objektumok be lett állítva, úgy kell létrehozni, ha majd hozzárendelt csoportobjektumok törlődnek, és minden olyan felhasználó, hogy ezek a csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="86053-136">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="86053-137">A csoport és a felhasználó tagságát után mezőbe írt megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="86053-137">The group and user memberships are preserved upon being written to Box.</span></span>

> [!TIP] 
> <span data-ttu-id="86053-138">Dönthet úgy is, SAML-alapú egyszeri bejelentkezés a Box engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="86053-138">You may also choose to enabled SAML-based Single Sign-On for Box, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="86053-139">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="86053-139">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="86053-140">Konfigurálása automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="86053-140">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="86053-141">Ez a szakasz célja felvázoló engedélyezése az Active Directory felhasználói létesítése mezőben.</span><span class="sxs-lookup"><span data-stu-id="86053-141">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Box.</span></span>

1. <span data-ttu-id="86053-142">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="86053-142">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="86053-143">Ha már konfigurált be egyszeri bejelentkezést, keresse meg a példányát használja a keresőmezőt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="86053-143">If you have already configured Box for single sign-on, search for your instance of Box using the search field.</span></span> <span data-ttu-id="86053-144">Máskülönben válassza **Hozzáadás** keresse meg a **mezőben** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="86053-144">Otherwise, select **Add** and search for **Box** in the application gallery.</span></span> <span data-ttu-id="86053-145">Jelölje be a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="86053-145">Select Box from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="86053-146">Válassza ki azt a példányt mezőben, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="86053-146">Select your instance of Box, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="86053-147">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="86053-147">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. <span data-ttu-id="86053-149">Az a **rendszergazdai hitelesítő adataival** területén kattintson **engedélyezés** egy új böngészőablakban mezőben bejelentkezési párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="86053-149">Under the **Admin Credentials** section, click **Authorize** to open a Box login dialog in a new browser window.</span></span>

6. <span data-ttu-id="86053-150">Az a **hozzáférési jogot mezőben bejelentkezési** lapon adja meg a szükséges hitelesítő adatokat, és kattintson a **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="86053-150">On the **Login to grant access to Box** page, provide the required credentials, and then click **Authorize**.</span></span> 
   
    <span data-ttu-id="86053-151">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="86053-151">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "Enable automatic user provisioning")</span></span>

7. <span data-ttu-id="86053-152">Kattintson a **be a hozzáférési jogot** engedélyezi ezt a műveletet, és térjen vissza az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="86053-152">Click **Grant access to Box** to authorize this operation and to return to the Azure portal.</span></span> 
   
    <span data-ttu-id="86053-153">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="86053-153">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "Enable automatic user provisioning")</span></span>

8. <span data-ttu-id="86053-154">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a Box alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="86053-154">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Box app.</span></span> <span data-ttu-id="86053-155">Ha nem sikerül, gondoskodjon be fiókjába Team rendszergazdai jogosultságokkal, majd próbálkozzon a **"Engedélyezés"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="86053-155">If the connection fails, ensure your Box account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="86053-156">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="86053-156">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="86053-157">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="86053-157">Click **Save.**</span></span>

11. <span data-ttu-id="86053-158">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók mezőben.**</span><span class="sxs-lookup"><span data-stu-id="86053-158">Under the Mappings section, select **Synchronize Azure Active Directory Users to Box.**</span></span>

12. <span data-ttu-id="86053-159">Az a **attribútum-leképezésekhez** szakaszban, tekintse át az Azure ad-be szinkronizált felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="86053-159">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Box.</span></span> <span data-ttu-id="86053-160">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a mezőbe a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="86053-160">The attributes selected as **Matching** properties are used to match the user accounts in Box for update operations.</span></span> <span data-ttu-id="86053-161">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="86053-161">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="86053-162">Az Azure AD a Box szolgáltatáshoz kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="86053-162">To enable the Azure AD provisioning service for Box, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="86053-163">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="86053-163">Click **Save.**</span></span>

<span data-ttu-id="86053-164">Amely elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy csoportok rendelt felhasználók és csoportok részén mezőbe.</span><span class="sxs-lookup"><span data-stu-id="86053-164">That starts the initial synchronization of any users and/or groups assigned to Box in the Users and Groups section.</span></span> <span data-ttu-id="86053-165">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="86053-165">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="86053-166">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a Box alkalmazásához a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="86053-166">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Box app.</span></span>

<span data-ttu-id="86053-167">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="86053-167">You can now create a test account.</span></span> <span data-ttu-id="86053-168">Győződjön meg arról, hogy a szinkronizált fiókkal akár 20 percig várjon mezőben.</span><span class="sxs-lookup"><span data-stu-id="86053-168">Wait for up to 20 minutes to verify that the account has been synchronized to box.</span></span>

<span data-ttu-id="86053-169">Az mezőbe-bérlőben szinkronizált felhasználók találhatók **felügyelt felhasználók** a a **felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="86053-169">In your Box tenant, synchronized users are listed under **Managed Users** in the **Admin Console**.</span></span>

<span data-ttu-id="86053-170">![Integráció állapotát](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integrációs állapota")</span><span class="sxs-lookup"><span data-stu-id="86053-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "Integration status")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="86053-171">További források</span><span class="sxs-lookup"><span data-stu-id="86053-171">Additional resources</span></span>

* [<span data-ttu-id="86053-172">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="86053-172">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86053-173">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="86053-173">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="86053-174">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="86053-174">Configure Single Sign-on</span></span>](active-directory-saas-box-tutorial.md)