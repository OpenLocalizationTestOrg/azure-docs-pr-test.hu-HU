---
title: "Oktatóanyag: LinkedIn értékesítési Navigator konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja az Azure Active Directory automatikus kiépítése és leépíti LinkedIn értékesítési Navigator felhasználói fiókok."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 86357949c8e6927f78ca5bb8b7e20a6b88c37ef3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a><span data-ttu-id="28d31-103">Oktatóanyag: LinkedIn értékesítési Navigator automatikus felhasználói kialakítási konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28d31-103">Tutorial: Configuring LinkedIn Sales Navigator for Automatic User Provisioning</span></span>


<span data-ttu-id="28d31-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a LinkedIn értékesítési Navigator és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-LinkedIn értékesítési Navigator mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="28d31-104">The objective of this tutorial is to show you the steps you need to perform in LinkedIn Sales Navigator and Azure AD to automatically provision and de-provision user accounts from Azure AD to LinkedIn Sales Navigator.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="28d31-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28d31-105">Prerequisites</span></span>

<span data-ttu-id="28d31-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="28d31-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="28d31-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="28d31-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="28d31-108">A LinkedIn értékesítési Navigator bérlő</span><span class="sxs-lookup"><span data-stu-id="28d31-108">A LinkedIn Sales Navigator tenant</span></span> 
*   <span data-ttu-id="28d31-109">A LinkedIn Account Center hozzáféréssel rendelkező LinkedIn értékesítési Navigator rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="28d31-109">An administrator account in LinkedIn Sales Navigator with access to the LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="28d31-110">Az Azure Active Directory integrálható LinkedIn értékesítési Navigator használatával a [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="28d31-110">Azure Active Directory integrates with LinkedIn Sales Navigator using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-linkedin-sales-navigator"></a><span data-ttu-id="28d31-111">Felhasználók hozzárendelése LinkedIn értékesítési Navigator</span><span class="sxs-lookup"><span data-stu-id="28d31-111">Assigning users to LinkedIn Sales Navigator</span></span>

<span data-ttu-id="28d31-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="28d31-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="28d31-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="28d31-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="28d31-114">A létesítési szolgáltatás engedélyezése és konfigurálása, előtt kell döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik LinkedIn értékesítési Navigator elérésére.</span><span class="sxs-lookup"><span data-stu-id="28d31-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to LinkedIn Sales Navigator.</span></span> <span data-ttu-id="28d31-115">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezek a felhasználók LinkedIn értékesítési Navigator:</span><span class="sxs-lookup"><span data-stu-id="28d31-115">Once decided, you can assign these users to LinkedIn Sales Navigator by following the instructions here:</span></span>

[<span data-ttu-id="28d31-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="28d31-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a><span data-ttu-id="28d31-117">Felhasználók hozzárendelése LinkedIn értékesítési Navigator fontos tippek</span><span class="sxs-lookup"><span data-stu-id="28d31-117">Important tips for assigning users to LinkedIn Sales Navigator</span></span>

*   <span data-ttu-id="28d31-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzárendelni LinkedIn értékesítési Navigator teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="28d31-118">It is recommended that a single Azure AD user be assigned to LinkedIn Sales Navigator to test the provisioning configuration.</span></span> <span data-ttu-id="28d31-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="28d31-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="28d31-120">Amikor egy felhasználó hozzárendelése LinkedIn értékesítési Navigator, ki kell választania a **felhasználói** szerepkör-hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="28d31-120">When assigning a user to LinkedIn Sales Navigator, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="28d31-121">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="28d31-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a><span data-ttu-id="28d31-122">A felhasználók átadása a LinkedIn értékesítési Navigator konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28d31-122">Configuring user provisioning to LinkedIn Sales Navigator</span></span>

<span data-ttu-id="28d31-123">Ez a szakasz végigvezeti az összekötő API-kiépítés LinkedIn értékesítési Navigator SCIM felhasználói fiókhoz az Azure AD, és a LinkedIn értékesítési Navigator alapján a felhasználó a felhasználói fiókok létrehozásához, frissítéséhez és tiltsa le a létesítési szolgáltatás konfigurálása hozzárendelve és az Azure AD-csoport-hozzárendelést.</span><span class="sxs-lookup"><span data-stu-id="28d31-123">This section guides you through connecting your Azure AD to LinkedIn Sales Navigator's SCIM user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in LinkedIn Sales Navigator based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="28d31-124">Dönthet úgy is, LinkedIn értékesítési Navigator SAML-alapú egyszeri bejelentkezés engedélyezése, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="28d31-124">You may also choose to enabled SAML-based Single Sign-On for LinkedIn Sales Navigator, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="28d31-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="28d31-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a><span data-ttu-id="28d31-126">Konfigurálása automatikus felhasználói fiók kiépítés LinkedIn értékesítési Navigator Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="28d31-126">To configure automatic user account provisioning to LinkedIn Sales Navigator in Azure AD:</span></span>


<span data-ttu-id="28d31-127">Az első lépés a LinkedIn-jogkivonatot lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="28d31-127">The first step is to retrieve your LinkedIn access token.</span></span> <span data-ttu-id="28d31-128">Ha a vállalati rendszergazdák, önálló megadhat egy hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="28d31-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="28d31-129">A fiók központban navigáljon **beállítások &gt; globális beállítások** , és nyissa meg a **SCIM telepítő** panel.</span><span class="sxs-lookup"><span data-stu-id="28d31-129">In your account center, go to **Settings &gt; Global Settings** and open the **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="28d31-130">Ha az account center érik el, hanem közvetlenül mutató hivatkozás, csatlakozni tud hozzá az alábbi lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="28d31-130">If you are accessing the account center directly rather than through a link, you can reach it using the following steps.</span></span>

1)  <span data-ttu-id="28d31-131">Jelentkezzen be Center fiók.</span><span class="sxs-lookup"><span data-stu-id="28d31-131">Sign in to Account Center.</span></span>

2)  <span data-ttu-id="28d31-132">Válassza ki **Admin &gt; rendszergazdai beállítások** .</span><span class="sxs-lookup"><span data-stu-id="28d31-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="28d31-133">Kattintson a **speciális integrációja** a bal oldali oldalsávon.</span><span class="sxs-lookup"><span data-stu-id="28d31-133">Click **Advanced Integrations** on the left sidebar.</span></span> <span data-ttu-id="28d31-134">Az account center van irányítva.</span><span class="sxs-lookup"><span data-stu-id="28d31-134">You are directed to the account center.</span></span>

4)  <span data-ttu-id="28d31-135">Kattintson a **+ Hozzáadás új SCIM konfigurációs** és hajtsa végre az eljárást minden mező kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="28d31-135">Click **+ Add new SCIM configuration** and follow the procedure by filling in each field.</span></span>

> <span data-ttu-id="28d31-136">Autoassign licencek nincs engedélyezve, az azt jelenti, hogy csak a felhasználói adatok van-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="28d31-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> <span data-ttu-id="28d31-138">Ha autolicense hozzárendelés engedélyezve van, jegyezze fel az alkalmazáspéldány és licenc típusa kell.</span><span class="sxs-lookup"><span data-stu-id="28d31-138">When auto­license assignment is enabled, you need to note the application instance and license type.</span></span> <span data-ttu-id="28d31-139">Elsőként érkezett a hozzárendelt licenceket, először kiszolgálására alapján, addig, amíg a licencek kerül.</span><span class="sxs-lookup"><span data-stu-id="28d31-139">Licenses are assigned on a first come, first serve basis until all the licenses are taken.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  <span data-ttu-id="28d31-141">Kattintson a **Generate token**.</span><span class="sxs-lookup"><span data-stu-id="28d31-141">Click **Generate token**.</span></span> <span data-ttu-id="28d31-142">A hozzáférési token megjelenítési alatt kell megjelennie a **hozzáférési jogkivonat** mező.</span><span class="sxs-lookup"><span data-stu-id="28d31-142">You should see your access token display under the **Access token** field.</span></span>

6)  <span data-ttu-id="28d31-143">Az oldal elhagyása előtt mentse a vágólapra vagy a számítógép a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="28d31-143">Save your access token to your clipboard or computer before leaving the page.</span></span>

7) <span data-ttu-id="28d31-144">Ezután jelentkezzen be a [Azure-portálon](https://portal.azure.com), és keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="28d31-144">Next, sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="28d31-145">Ha már konfigurált LinkedIn értékesítési Navigator egyszeri bejelentkezést, keresse meg a példányát használja a keresőmezőt LinkedIn értékesítési Navigátor.</span><span class="sxs-lookup"><span data-stu-id="28d31-145">If you have already configured LinkedIn Sales Navigator for single sign-on, search for your instance of LinkedIn Sales Navigator using the search field.</span></span> <span data-ttu-id="28d31-146">Máskülönben válassza **Hozzáadás** keresse meg a **LinkedIn értékesítési Navigator** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="28d31-146">Otherwise, select **Add** and search for **LinkedIn Sales Navigator** in the application gallery.</span></span> <span data-ttu-id="28d31-147">Válassza ki a LinkedIn értékesítési Navigator a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="28d31-147">Select LinkedIn Sales Navigator from the search results, and add it to your list of applications.</span></span>

9)  <span data-ttu-id="28d31-148">Jelölje ki a LinkedIn értékesítési Navigator példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="28d31-148">Select your instance of LinkedIn Sales Navigator, then select the **Provisioning** tab.</span></span>

10) <span data-ttu-id="28d31-149">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="28d31-149">Set the **Provisioning Mode** to **Automatic**.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  <span data-ttu-id="28d31-151">Töltse ki a következő mezőket a **rendszergazdai hitelesítő adataival** :</span><span class="sxs-lookup"><span data-stu-id="28d31-151">Fill in the following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="28d31-152">Az a **bérlői URL-cím** mezőbe írja be a https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="28d31-152">In the **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="28d31-153">Az a **titkos Token** mezőben adja meg az 1. lépésben létrehozott jogkivonat, és kattintson a **kapcsolat tesztelése** .</span><span class="sxs-lookup"><span data-stu-id="28d31-153">In the **Secret Token** field, enter the access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="28d31-154">A portál upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="28d31-154">You should see a success notification on the upper­right side of   your portal.</span></span>

12) <span data-ttu-id="28d31-155">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="28d31-155">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

13) <span data-ttu-id="28d31-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="28d31-156">Click **Save**.</span></span> 

14) <span data-ttu-id="28d31-157">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói és csoportattribútum, amely szinkronizálva lesznek az Azure AD az LinkedIn értékesítési Navigator.</span><span class="sxs-lookup"><span data-stu-id="28d31-157">In the **Attribute Mappings** section, review the user and group attributes that will be synchronized from Azure AD to LinkedIn Sales Navigator.</span></span> <span data-ttu-id="28d31-158">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok felel meg a felhasználói fiókokat és csoportokat LinkedIn értékesítési Navigator frissítés műveletekhez használható.</span><span class="sxs-lookup"><span data-stu-id="28d31-158">Note that the attributes selected as **Matching** properties will be used to match the user accounts and groups in LinkedIn Sales Navigator for update operations.</span></span> <span data-ttu-id="28d31-159">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="28d31-159">Select the Save button to commit any changes.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) <span data-ttu-id="28d31-161">Az Azure AD szolgáltatás LinkedIn értékesítési Navigator kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="28d31-161">To enable the Azure AD provisioning service for LinkedIn Sales Navigator, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

16) <span data-ttu-id="28d31-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="28d31-162">Click **Save**.</span></span> 

<span data-ttu-id="28d31-163">Elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy csoportok rendelt LinkedIn értékesítési Navigator a felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="28d31-163">This will start the initial synchronization of any users and/or groups assigned to LinkedIn Sales Navigator in the Users and Groups section.</span></span> <span data-ttu-id="28d31-164">Figyelje meg, hogy a kezdeti szinkronizálás hosszabb ideig tart hajtsa végre ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál.</span><span class="sxs-lookup"><span data-stu-id="28d31-164">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="28d31-165">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amely az értékesítési Navigator LinkedIn-alkalmazás a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="28d31-165">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your LinkedIn Sales Navigator app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="28d31-166">További források</span><span class="sxs-lookup"><span data-stu-id="28d31-166">Additional Resources</span></span>

* [<span data-ttu-id="28d31-167">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="28d31-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="28d31-168">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="28d31-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
