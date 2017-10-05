---
title: "Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Útmutató: Azure Active Directory konfigurálása automatikusan rendelkezni felhasználóknak, hogy a Résztvevőlista Cerner közép-India"
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
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: 84613b7f8d7bd031d492a62da0bc53be96ac45a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="073a7-103">Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="073a7-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="073a7-104">Ez az oktatóanyag célja mutatjuk be, a lépéseket kell elvégeznie a Cerner központi és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-egy felhasználó Résztvevőlista Cerner közép-India.</span><span class="sxs-lookup"><span data-stu-id="073a7-104">The objective of this tutorial is to show you the steps you need to perform in Cerner Central and Azure AD to automatically provision and de-provision user accounts from Azure AD to a user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="073a7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="073a7-105">Prerequisites</span></span>

<span data-ttu-id="073a7-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="073a7-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="073a7-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="073a7-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="073a7-108">A Cerner központi bérlő</span><span class="sxs-lookup"><span data-stu-id="073a7-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="073a7-109">Az Azure Active Directory használatával Cerner központi integrálható a [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="073a7-109">Azure Active Directory integrates with Cerner Central using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-cerner-central"></a><span data-ttu-id="073a7-110">Felhasználók hozzárendelése Cerner központi</span><span class="sxs-lookup"><span data-stu-id="073a7-110">Assigning users to Cerner Central</span></span>

<span data-ttu-id="073a7-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="073a7-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="073a7-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok számára "rendelt" az Azure AD alkalmazás szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="073a7-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="073a7-113">A létesítési szolgáltatás engedélyezése és konfigurálása, előtt meg kell határoznia, mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik Cerner központi elérésére.</span><span class="sxs-lookup"><span data-stu-id="073a7-113">Before configuring and enabling the provisioning service, you should decide what users and/or groups in Azure AD represent the users who need access to Cerner Central.</span></span> <span data-ttu-id="073a7-114">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezek a felhasználók Cerner központi:</span><span class="sxs-lookup"><span data-stu-id="073a7-114">Once decided, you can assign these users to Cerner Central by following the instructions here:</span></span>

[<span data-ttu-id="073a7-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="073a7-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a><span data-ttu-id="073a7-116">Felhasználók hozzárendelése Cerner központi fontos tippek</span><span class="sxs-lookup"><span data-stu-id="073a7-116">Important tips for assigning users to Cerner Central</span></span>

*   <span data-ttu-id="073a7-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó a kiépítési konfigurációjának tesztelése rendelendő Cerner központi.</span><span class="sxs-lookup"><span data-stu-id="073a7-117">It is recommended that a single Azure AD user be assigned to Cerner Central to test the provisioning configuration.</span></span> <span data-ttu-id="073a7-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="073a7-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="073a7-119">Kezdeti tesztelés befejezése után egy-egy felhasználóhoz, Cerner központi azt javasolja, hogy hozzárendelése a teljes listát a felhasználók bármely Cerner megoldás (nem csak Cerner központi) eléréséhez szánt Cerner tartozó felhasználói Résztvevőlista kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="073a7-119">Once initial testing is complete for a single user, Cerner Central recommends assigning the entire list of users intended to access any Cerner solution (not just Cerner Central) to be provisioned to Cerner’s user roster.</span></span>  <span data-ttu-id="073a7-120">Cerner megoldásait használja ki a felhasználók a felhasználói Résztvevőlista listájában.</span><span class="sxs-lookup"><span data-stu-id="073a7-120">Other Cerner solutions leverage this list of users in the user roster.</span></span>

*   <span data-ttu-id="073a7-121">Amikor egy felhasználó hozzárendelése Cerner központi, ki kell választania a **felhasználói** szerepkör-hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="073a7-121">When assigning a user to Cerner Central, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="073a7-122">A "Default hozzáférés" szerepkörrel rendelkező felhasználók kiépítés tartoznak.</span><span class="sxs-lookup"><span data-stu-id="073a7-122">Users with the "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-to-cerner-central"></a><span data-ttu-id="073a7-123">A felhasználók átadása a Cerner központi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="073a7-123">Configuring user provisioning to Cerner Central</span></span>

<span data-ttu-id="073a7-124">Ez a szakasz végigvezeti az Azure AD Cerner központi felhasználói Résztvevőlista Cerner tartozó SCIM felhasználói fiók kiépítése API használatával való kapcsolódás és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Cerner központi alapján felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="073a7-124">This section guides you through connecting your Azure AD to Cerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="073a7-125">Dönthet úgy is engedélyezni SAML-alapú egyszeri bejelentkezést a Cerner központi utasításai [Azure-portálon (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="073a7-125">You may also choose to enabled SAML-based Single Sign-On for Cerner Central, following the instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="073a7-126">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="073a7-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="073a7-127">További információkért lásd: a [Cerner központi egyszeri bejelentkezés az oktatóanyag](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="073a7-127">For more information, see the [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a><span data-ttu-id="073a7-128">Konfigurálása automatikus felhasználói fiók kiépítés Cerner központi Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="073a7-128">To configure automatic user account provisioning to Cerner Central in Azure AD:</span></span>


<span data-ttu-id="073a7-129">Ahhoz, hogy a felhasználói fiókok Cerner központi telepítéséhez, szüksége lesz egy Cerner központi rendszer fiókot kérhetnek Cerner, és létrehozzon egy OAuth tulajdonosi jogkivonatot, amely az Azure AD használatával Cerner tartozó SCIM végponthoz kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="073a7-129">In order to provision user accounts to Cerner Central, you’ll need to request a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use to connect to Cerner's SCIM endpoint.</span></span> <span data-ttu-id="073a7-130">Ajánlott továbbá, hogy az integrációs végezhető el egy Cerner védőfal mögötti környezet üzemi környezetben üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="073a7-130">It is also recommended that the integration be performed in a Cerner sandbox environment before deploying to production.</span></span>

1.  <span data-ttu-id="073a7-131">Az első lépés annak a személynek a Cerner kezelése érdekében, és az Azure AD-integrációs kell CernerCare-fiókkal, amely utasítások végrehajtásához szükséges dokumentáció eléréséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="073a7-131">The first step is to ensure the people managing the Cerner and Azure AD integration have a CernerCare account, which is required to access the documentation necessary to complete the instructions.</span></span> <span data-ttu-id="073a7-132">Ha szükséges, használja az alábbi URL-címek CernerCare fiókokat létrehozni minden esetben környezetben.</span><span class="sxs-lookup"><span data-stu-id="073a7-132">If necessary, use the URLs below to create CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="073a7-133">Védőfal: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="073a7-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="073a7-134">Éles: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="073a7-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="073a7-135">A következő rendszerfiók léteznie kell az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="073a7-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="073a7-136">Az alábbi utasításokat követve a védőfal és éles környezetekhez rendszerfiók kérelem.</span><span class="sxs-lookup"><span data-stu-id="073a7-136">Use the instructions below to request a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="073a7-137">Útmutató: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="073a7-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="073a7-138">Védőfal: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="073a7-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="073a7-139">Éles: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="073a7-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="073a7-140">Ezt követően készítse el az OAuth tulajdonosi jogkivonat minden rendszer fiók.</span><span class="sxs-lookup"><span data-stu-id="073a7-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="073a7-141">Ehhez kövesse az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="073a7-141">To do this, follow the instructions below.</span></span>

   * <span data-ttu-id="073a7-142">Útmutató: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="073a7-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="073a7-143">Védőfal: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="073a7-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="073a7-144">Éles: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="073a7-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="073a7-145">Végezetül beszerzése szükséges felhasználói Résztvevőlista tartomány azonosítók Cerner a konfigurálás befejezéséhez a védőfal mind az üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="073a7-145">Finally, you need to acquire User Roster Realm IDs for both the sandbox and production environments in Cerner to complete the configuration.</span></span> <span data-ttu-id="073a7-146">Ez beszerzésére vonatkozó további információkért lásd:: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="073a7-146">For information on how to acquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="073a7-147">Azure AD-felhasználói fiókok létesítése Cerner most konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="073a7-147">Now you can configure Azure AD to provision user accounts to Cerner.</span></span> <span data-ttu-id="073a7-148">Jelentkezzen be a [Azure-portálon](https://portal.azure.com), és keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="073a7-148">Sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="073a7-149">Ha már konfigurált Cerner központi egyszeri bejelentkezést, keresse meg a példányát használja a keresőmezőt Cerner központi.</span><span class="sxs-lookup"><span data-stu-id="073a7-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using the search field.</span></span> <span data-ttu-id="073a7-150">Máskülönben válassza **Hozzáadás** keresse meg a **Cerner központi** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="073a7-150">Otherwise, select **Add** and search for **Cerner Central** in the application gallery.</span></span> <span data-ttu-id="073a7-151">Válassza ki a Cerner központi a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="073a7-151">Select Cerner Central from the search results, and add it to your list of applications.</span></span>

7.  <span data-ttu-id="073a7-152">Jelölje ki a Cerner központi példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="073a7-152">Select your instance of Cerner Central, then select the **Provisioning** tab.</span></span>

8.  <span data-ttu-id="073a7-153">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="073a7-153">Set the **Provisioning Mode** to **Automatic**.</span></span>

   ![Kiépítés Cerner központi](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="073a7-155">Töltse ki a következő mezőket a **rendszergazdai hitelesítő adataival**:</span><span class="sxs-lookup"><span data-stu-id="073a7-155">Fill in the following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="073a7-156">Az a **bérlői URL-cím** mezőbe írja be az alábbi formátumban "Felhasználói Résztvevőlista-tartományi-azonosító" cseréje a tartomány azonosítója #4. lépésben beszerzett URL.</span><span class="sxs-lookup"><span data-stu-id="073a7-156">In the **Tenant URL** field, enter a URL in the format below, replacing "User-Roster-Realm-ID" with the realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="073a7-157">Védőfal: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="073a7-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="073a7-158">Éles: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="073a7-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="073a7-159">Az a **titkos Token** mezőben adja meg a #3. lépésében létrehozott OAuth tulajdonosi jogkivonatot, és kattintson a **kapcsolat tesztelése**.</span><span class="sxs-lookup"><span data-stu-id="073a7-159">In the **Secret Token** field, enter the OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="073a7-160">A portál upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="073a7-160">You should see a success notification on the upper­right side of your portal.</span></span>

10. <span data-ttu-id="073a7-161">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be az alábbi jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="073a7-161">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

11. <span data-ttu-id="073a7-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="073a7-162">Click **Save**.</span></span> 

12. <span data-ttu-id="073a7-163">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói és csoportattribútum Cerner központi az Azure ad-szinkronizálandó.</span><span class="sxs-lookup"><span data-stu-id="073a7-163">In the **Attribute Mappings** section, review the user and group attributes to be synchronized from Azure AD to Cerner Central.</span></span> <span data-ttu-id="073a7-164">A kiválasztott attribútumok **egyező** tulajdonságok használt a felhasználói fiókok és a frissítési műveletek Cerner központi csoportok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="073a7-164">The attributes selected as **Matching** properties are used to match the user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="073a7-165">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="073a7-165">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="073a7-166">Az Azure AD szolgáltatás a Cerner központi kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="073a7-166">To enable the Azure AD provisioning service for Cerner Central, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

14. <span data-ttu-id="073a7-167">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="073a7-167">Click **Save**.</span></span> 

<span data-ttu-id="073a7-168">Ezzel elindítja a kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Cerner központi rendelt csoportok.</span><span class="sxs-lookup"><span data-stu-id="073a7-168">This starts the initial synchronization of any users and/or groups assigned to Cerner Central in the Users and Groups section.</span></span> <span data-ttu-id="073a7-169">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, körülbelül 20 percenként történik, amíg fut a szolgáltatás kiépítését az Azure AD-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="073a7-169">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the Azure AD provisioning service is running.</span></span> <span data-ttu-id="073a7-170">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az Cerner központi alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="073a7-170">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="073a7-171">Olvassa el az Azure AD-naplók kiépítés módjáról további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="073a7-171">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="073a7-172">További források</span><span class="sxs-lookup"><span data-stu-id="073a7-172">Additional resources</span></span>

* [<span data-ttu-id="073a7-173">Cerner központi: Az Azure AD identity adatok közzététele</span><span class="sxs-lookup"><span data-stu-id="073a7-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="073a7-174">Oktatóanyag: Cerner központi konfigurálása egyszeri bejelentkezéshez az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="073a7-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="073a7-175">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="073a7-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="073a7-176">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="073a7-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="073a7-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="073a7-177">Next steps</span></span>
* <span data-ttu-id="073a7-178">[Ismerje meg, tekintse át a naplók és jelentések készítése a kiépítés tevékenység](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="073a7-178">[Learn how to review logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
