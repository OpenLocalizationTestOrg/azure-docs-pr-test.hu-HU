---
title: "Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés automatikus a felhasználók átadása az Azure Active Directoryhoz |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja az Azure Active Directory automatikus kiépítése és leépíti a felhasználói fiókok LinkedIn jogosultságszintjének emeléséhez."
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
ms.openlocfilehash: 526666301aad1e5284c621024649d9cd52c92d18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a><span data-ttu-id="067d1-103">Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés automatikus a felhasználók átadása</span><span class="sxs-lookup"><span data-stu-id="067d1-103">Tutorial: Configuring LinkedIn Elevate for Automatic User Provisioning</span></span>


<span data-ttu-id="067d1-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a LinkedIn jogosultságszint-emelés és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure AD-ből való jogosultságszint-emelés LinkedIn mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="067d1-104">The objective of this tutorial is to show you the steps you need to perform in LinkedIn Elevate and Azure AD to automatically provision and de-provision user accounts from Azure AD to LinkedIn Elevate.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="067d1-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="067d1-105">Prerequisites</span></span>

<span data-ttu-id="067d1-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="067d1-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="067d1-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="067d1-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="067d1-108">A LinkedIn jogosultságszint-emelés bérlő</span><span class="sxs-lookup"><span data-stu-id="067d1-108">A LinkedIn Elevate tenant</span></span> 
*   <span data-ttu-id="067d1-109">A LinkedIn Account Center hozzáféréssel rendelkező LinkedIn jogosultságszint-emelés a rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="067d1-109">An administrator account in LinkedIn Elevate with access to the LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="067d1-110">Az Azure Active Directory integrálható LinkedIn jogosultságszint-emelés használatával a [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="067d1-110">Azure Active Directory integrates with LinkedIn Elevate using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-linkedin-elevate"></a><span data-ttu-id="067d1-111">Felhasználók hozzárendelése LinkedIn jogosultságszint-emelés</span><span class="sxs-lookup"><span data-stu-id="067d1-111">Assigning users to LinkedIn Elevate</span></span>

<span data-ttu-id="067d1-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="067d1-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="067d1-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="067d1-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="067d1-114">A létesítési szolgáltatás engedélyezése és konfigurálása, előtt kell döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik LinkedIn jogosultságszint-emelés elérésére.</span><span class="sxs-lookup"><span data-stu-id="067d1-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to LinkedIn Elevate.</span></span> <span data-ttu-id="067d1-115">Ha úgy döntött, itt cikk utasításait követve ezeket a felhasználókat rendelhet LinkedIn jogosultságszintjének emeléséhez:</span><span class="sxs-lookup"><span data-stu-id="067d1-115">Once decided, you can assign these users to LinkedIn Elevate by following the instructions here:</span></span>

[<span data-ttu-id="067d1-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="067d1-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-elevate"></a><span data-ttu-id="067d1-117">Felhasználók hozzárendelése LinkedIn jogosultságszint-emelés fontos tippek</span><span class="sxs-lookup"><span data-stu-id="067d1-117">Important tips for assigning users to LinkedIn Elevate</span></span>

*   <span data-ttu-id="067d1-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzárendelni LinkedIn jogosultságszint-emelés teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="067d1-118">It is recommended that a single Azure AD user be assigned to LinkedIn Elevate to test the provisioning configuration.</span></span> <span data-ttu-id="067d1-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="067d1-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="067d1-120">Amikor egy felhasználó hozzárendelése LinkedIn jogosultságszint-emelés, ki kell választania a **felhasználói** szerepkör-hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="067d1-120">When assigning a user to LinkedIn Elevate, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="067d1-121">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="067d1-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-linkedin-elevate"></a><span data-ttu-id="067d1-122">Felhasználók átadására LinkedIn jogosultságszintjének emeléséhez</span><span class="sxs-lookup"><span data-stu-id="067d1-122">Configuring user provisioning to LinkedIn Elevate</span></span>

<span data-ttu-id="067d1-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD LinkedIn jogosultságszintjének SCIM felhasználói fiókhoz kiépítés API, és a felhasználói fiókok a LinkedIn jogosultságszint-emelés alapján a felhasználó és csoport-hozzárendelés létrehozásához, frissítéséhez és tiltsa le a létesítési szolgáltatás konfigurálása hozzárendelve az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="067d1-123">This section guides you through connecting your Azure AD to LinkedIn Elevate's SCIM user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in LinkedIn Elevate based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="067d1-124">**Tipp:** is választhatja, hogy engedélyezze SAML-alapú egyszeri bejelentkezést a LinkedIn jogosultságszint-emelés, a megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="067d1-124">**Tip:** You may also choose to enabled SAML-based Single Sign-On for LinkedIn Elevate, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="067d1-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="067d1-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-elevate-in-azure-ad"></a><span data-ttu-id="067d1-126">Konfigurálása automatikus felhasználói fiók kiépítése LinkedIn jogosultságszintjének emeléséhez Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="067d1-126">To configure automatic user account provisioning to LinkedIn Elevate in Azure AD:</span></span>


<span data-ttu-id="067d1-127">Az első lépés a LinkedIn-jogkivonatot lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="067d1-127">The first step is to retrieve your LinkedIn access token.</span></span> <span data-ttu-id="067d1-128">Ha a vállalati rendszergazdák, önálló megadhat egy hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="067d1-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="067d1-129">A fiók központban navigáljon **beállítások &gt; globális beállítások** , és nyissa meg a **SCIM telepítő** panel.</span><span class="sxs-lookup"><span data-stu-id="067d1-129">In your account center, go to **Settings &gt; Global Settings** and open the **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="067d1-130">Ha az account center érik el, hanem közvetlenül mutató hivatkozás, csatlakozni tud hozzá az alábbi lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="067d1-130">If you are accessing the account center directly rather than through a link, you can reach it using the following steps.</span></span>

1)  <span data-ttu-id="067d1-131">Jelentkezzen be Center fiók.</span><span class="sxs-lookup"><span data-stu-id="067d1-131">Sign in to Account Center.</span></span>

2)  <span data-ttu-id="067d1-132">Válassza ki **Admin &gt; rendszergazdai beállítások** .</span><span class="sxs-lookup"><span data-stu-id="067d1-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="067d1-133">Kattintson a **speciális integrációja** a bal oldali oldalsávon.</span><span class="sxs-lookup"><span data-stu-id="067d1-133">Click **Advanced Integrations** on the left sidebar.</span></span> <span data-ttu-id="067d1-134">Az account center van irányítva.</span><span class="sxs-lookup"><span data-stu-id="067d1-134">You are directed to the account center.</span></span>

4)  <span data-ttu-id="067d1-135">Kattintson a **+ Hozzáadás új SCIM konfigurációs** és hajtsa végre az eljárást minden mező kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="067d1-135">Click **+ Add new SCIM configuration** and follow the procedure by filling in each field.</span></span>

> <span data-ttu-id="067d1-136">Autoassign licencek nincs engedélyezve, az azt jelenti, hogy csak a felhasználói adatok van-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="067d1-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> <span data-ttu-id="067d1-138">Ha autolicense hozzárendelés engedélyezve van, jegyezze fel az alkalmazáspéldány és licenc típusa kell.</span><span class="sxs-lookup"><span data-stu-id="067d1-138">When auto­license assignment is enabled, you need to note the application instance and license type.</span></span> <span data-ttu-id="067d1-139">Elsőként érkezett a hozzárendelt licenceket, először kiszolgálására alapján, addig, amíg a licencek kerül.</span><span class="sxs-lookup"><span data-stu-id="067d1-139">Licenses are assigned on a first come, first serve basis until all the licenses are taken.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  <span data-ttu-id="067d1-141">Kattintson a **Generate token**.</span><span class="sxs-lookup"><span data-stu-id="067d1-141">Click **Generate token**.</span></span> <span data-ttu-id="067d1-142">A hozzáférési token megjelenítési alatt kell megjelennie a **hozzáférési jogkivonat** mező.</span><span class="sxs-lookup"><span data-stu-id="067d1-142">You should see your access token display under the **Access token** field.</span></span>

6)  <span data-ttu-id="067d1-143">Az oldal elhagyása előtt mentse a vágólapra vagy a számítógép a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="067d1-143">Save your access token to your clipboard or computer before leaving the page.</span></span>

7) <span data-ttu-id="067d1-144">Ezután jelentkezzen be a [Azure-portálon](https://portal.azure.com), és keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="067d1-144">Next, sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="067d1-145">Ha már konfigurált LinkedIn jogosultságszint-emelés egyszeri bejelentkezést, keresse meg a LinkedIn jogosultságszint-emelés használja a keresőmezőt példányát.</span><span class="sxs-lookup"><span data-stu-id="067d1-145">If you have already configured LinkedIn Elevate for single sign-on, search for your instance of LinkedIn Elevate using the search field.</span></span> <span data-ttu-id="067d1-146">Máskülönben válassza **Hozzáadás** keresse meg a **LinkedIn jogosultságszint-emelés** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="067d1-146">Otherwise, select **Add** and search for **LinkedIn Elevate** in the application gallery.</span></span> <span data-ttu-id="067d1-147">Válassza ki a LinkedIn jogosultságszint-emelés a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="067d1-147">Select LinkedIn Elevate from the search results, and add it to your list of applications.</span></span>

9)  <span data-ttu-id="067d1-148">Jelölje ki a LinkedIn jogosultságszint-emelés példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="067d1-148">Select your instance of LinkedIn Elevate, then select the **Provisioning** tab.</span></span>

10) <span data-ttu-id="067d1-149">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="067d1-149">Set the **Provisioning Mode** to **Automatic**.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  <span data-ttu-id="067d1-151">Töltse ki a következő mezőket a **rendszergazdai hitelesítő adataival** :</span><span class="sxs-lookup"><span data-stu-id="067d1-151">Fill in the following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="067d1-152">Az a **bérlői URL-cím** mezőbe írja be a https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="067d1-152">In the **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="067d1-153">Az a **titkos Token** mezőben adja meg az 1. lépésben létrehozott jogkivonat, és kattintson a **kapcsolat tesztelése** .</span><span class="sxs-lookup"><span data-stu-id="067d1-153">In the **Secret Token** field, enter the access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="067d1-154">A portál upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="067d1-154">You should see a success notification on the upper­right side of   your portal.</span></span>

12) <span data-ttu-id="067d1-155">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="067d1-155">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

13) <span data-ttu-id="067d1-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="067d1-156">Click **Save**.</span></span> 

14) <span data-ttu-id="067d1-157">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói és csoportattribútum LinkedIn jogosultságszintjének emeléséhez az Azure Active Directoryból szinkronizált.</span><span class="sxs-lookup"><span data-stu-id="067d1-157">In the **Attribute Mappings** section, review the user and group attributes that will be synchronized from Azure AD to LinkedIn Elevate.</span></span> <span data-ttu-id="067d1-158">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok felel meg a felhasználói fiókok és csoportok LinkedIn jogosultságszint-emelés frissítés műveletekhez használható.</span><span class="sxs-lookup"><span data-stu-id="067d1-158">Note that the attributes selected as **Matching** properties will be used to match the user accounts and groups in LinkedIn Elevate for update operations.</span></span> <span data-ttu-id="067d1-159">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="067d1-159">Select the Save button to commit any changes.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) <span data-ttu-id="067d1-161">Az Azure AD szolgáltatás LinkedIn jogosultságszint-emelés kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="067d1-161">To enable the Azure AD provisioning service for LinkedIn Elevate, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

16) <span data-ttu-id="067d1-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="067d1-162">Click **Save**.</span></span> 

<span data-ttu-id="067d1-163">Elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy csoportok rendelt LinkedIn jogosultságszint-emelés a felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="067d1-163">This will start the initial synchronization of any users and/or groups assigned to LinkedIn Elevate in the Users and Groups section.</span></span> <span data-ttu-id="067d1-164">Figyelje meg, hogy a kezdeti szinkronizálás hosszabb ideig tart hajtsa végre ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál.</span><span class="sxs-lookup"><span data-stu-id="067d1-164">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="067d1-165">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás a LinkedIn jogosultságszint-emelés alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="067d1-165">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your LinkedIn Elevate app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="067d1-166">További források</span><span class="sxs-lookup"><span data-stu-id="067d1-166">Additional Resources</span></span>

* [<span data-ttu-id="067d1-167">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="067d1-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="067d1-168">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="067d1-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
