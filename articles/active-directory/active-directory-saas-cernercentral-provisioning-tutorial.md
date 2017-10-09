---
title: "Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése felhasználók tooa Résztvevőlista Cerner közép-India"
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
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="1cae8-103">Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="1cae8-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="1cae8-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform Cerner központi és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooa felhasználói Résztvevőlista Cerner közép a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1cae8-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Cerner Central and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooa user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="1cae8-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cae8-105">Prerequisites</span></span>

<span data-ttu-id="1cae8-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1cae8-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="1cae8-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="1cae8-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="1cae8-108">A Cerner központi bérlő</span><span class="sxs-lookup"><span data-stu-id="1cae8-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="1cae8-109">Az Azure Active Directory használatával hello Cerner központi integrálható [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="1cae8-109">Azure Active Directory integrates with Cerner Central using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toocerner-central"></a><span data-ttu-id="1cae8-110">Központi felhasználók tooCerner hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1cae8-110">Assigning users tooCerner Central</span></span>

<span data-ttu-id="1cae8-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="1cae8-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="1cae8-112">Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van.</span><span class="sxs-lookup"><span data-stu-id="1cae8-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="1cae8-113">Hello szolgáltatás kiépítését engedélyezése és konfigurálása, előtt meg kell határoznia, melyik felhasználói és/vagy az Azure AD-csoportok képviselő hello felhasználók számára, akik kell tooCerner központi.</span><span class="sxs-lookup"><span data-stu-id="1cae8-113">Before configuring and enabling hello provisioning service, you should decide what users and/or groups in Azure AD represent hello users who need access tooCerner Central.</span></span> <span data-ttu-id="1cae8-114">Ha úgy döntött, hozzárendelheti a felhasználók tooCerner központi itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="1cae8-114">Once decided, you can assign these users tooCerner Central by following hello instructions here:</span></span>

[<span data-ttu-id="1cae8-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="1cae8-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a><span data-ttu-id="1cae8-116">Fontos tippek a felhasználók tooCerner központi hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1cae8-116">Important tips for assigning users tooCerner Central</span></span>

*   <span data-ttu-id="1cae8-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooCerner központi tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="1cae8-117">It is recommended that a single Azure AD user be assigned tooCerner Central tootest hello provisioning configuration.</span></span> <span data-ttu-id="1cae8-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="1cae8-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="1cae8-119">Kezdeti tesztelés befejezése után egy-egy felhasználóhoz, Cerner központi azt javasolja, hogy hello teljes listáját a felhasználóknak szánt tooaccess bármely Cerner (nem csak Cerner központi) megoldás kiépített toobe tooCerner felhasználói Résztvevőlista hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="1cae8-119">Once initial testing is complete for a single user, Cerner Central recommends assigning hello entire list of users intended tooaccess any Cerner solution (not just Cerner Central) toobe provisioned tooCerner’s user roster.</span></span>  <span data-ttu-id="1cae8-120">Egyéb Cerner megoldások ebben a listában lévő hello felhasználói Résztvevőlista felhasználók tudják kihasználni.</span><span class="sxs-lookup"><span data-stu-id="1cae8-120">Other Cerner solutions leverage this list of users in hello user roster.</span></span>

*   <span data-ttu-id="1cae8-121">Amikor egy felhasználó tooCerner központi rendel, ki kell választania hello **felhasználói** szerepkör hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="1cae8-121">When assigning a user tooCerner Central, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="1cae8-122">Kiépítés hello "Alapértelmezett hozzáférés" szerepkörrel rendelkező felhasználók tartoznak.</span><span class="sxs-lookup"><span data-stu-id="1cae8-122">Users with hello "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-toocerner-central"></a><span data-ttu-id="1cae8-123">Kiépítés tooCerner központi felhasználói konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1cae8-123">Configuring user provisioning tooCerner Central</span></span>

<span data-ttu-id="1cae8-124">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooCerner központi felhasználói Résztvevőlista Cerner tartozó SCIM felhasználói fiókkal API kiépítés és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a fiókok a Cerner központi alapú hozzárendelt felhasználó a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="1cae8-124">This section guides you through connecting your Azure AD tooCerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="1cae8-125">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Cerner központi, a következő hello utasításokat [Azure-portálon (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1cae8-125">You may also choose tooenabled SAML-based Single Sign-On for Cerner Central, following hello instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="1cae8-126">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="1cae8-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="1cae8-127">További információkért lásd: hello [Cerner központi egyszeri bejelentkezés az oktatóanyag](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1cae8-127">For more information, see hello [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a><span data-ttu-id="1cae8-128">tooconfigure automatikus felhasználói fiók tooCerner központi kiépítése az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="1cae8-128">tooconfigure automatic user account provisioning tooCerner Central in Azure AD:</span></span>


<span data-ttu-id="1cae8-129">Rendelés tooprovision felhasználói fiókok tooCerner központi bemutatjuk a Cerner Cerner központi rendszerfiók toorequest kell, létrehozzon egy OAuth tulajdonosi jogkivonatot, hogy az Azure AD tooconnect tooCerner SCIM végpont tudja használni.</span><span class="sxs-lookup"><span data-stu-id="1cae8-129">In order tooprovision user accounts tooCerner Central, you’ll need toorequest a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use tooconnect tooCerner's SCIM endpoint.</span></span> <span data-ttu-id="1cae8-130">Ajánlott továbbá, hogy hello integrációs végezhető el egy Cerner védőfal mögötti környezet tooproduction telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="1cae8-130">It is also recommended that hello integration be performed in a Cerner sandbox environment before deploying tooproduction.</span></span>

1.  <span data-ttu-id="1cae8-131">hello első lépéseként tooensure hello személyek hello Cerner kezelése és az Azure AD-integrációs CernerCare fiókkal, amely szükséges tooaccess hello dokumentáció szükséges toocomplete hello utasításai rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1cae8-131">hello first step is tooensure hello people managing hello Cerner and Azure AD integration have a CernerCare account, which is required tooaccess hello documentation necessary toocomplete hello instructions.</span></span> <span data-ttu-id="1cae8-132">Ha szükséges, használjon hello URL-címeket, toocreate CernerCare fiókok alább minden esetben környezetben.</span><span class="sxs-lookup"><span data-stu-id="1cae8-132">If necessary, use hello URLs below toocreate CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="1cae8-133">Védőfal: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="1cae8-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="1cae8-134">Éles: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="1cae8-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="1cae8-135">A következő rendszerfiók léteznie kell az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1cae8-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="1cae8-136">A védőfal és éles környezetben használjon hello utasításokat toorequest rendszerfiók alatt.</span><span class="sxs-lookup"><span data-stu-id="1cae8-136">Use hello instructions below toorequest a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="1cae8-137">Útmutató: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="1cae8-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="1cae8-138">Védőfal: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="1cae8-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="1cae8-139">Éles: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="1cae8-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="1cae8-140">Ezt követően készítse el az OAuth tulajdonosi jogkivonat minden rendszer fiók.</span><span class="sxs-lookup"><span data-stu-id="1cae8-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="1cae8-141">toodo a, kövesse hello az utasításokat az alábbi.</span><span class="sxs-lookup"><span data-stu-id="1cae8-141">toodo this, follow hello instructions below.</span></span>

   * <span data-ttu-id="1cae8-142">Útmutató: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="1cae8-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="1cae8-143">Védőfal: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="1cae8-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="1cae8-144">Éles: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="1cae8-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="1cae8-145">Végezetül tooacquire felhasználói Résztvevőlista tartomány azonosítók szüksége Cerner toocomplete hello konfigurációs mindkét hello védőfal és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1cae8-145">Finally, you need tooacquire User Roster Realm IDs for both hello sandbox and production environments in Cerner toocomplete hello configuration.</span></span> <span data-ttu-id="1cae8-146">Információ tooacquire a, lásd: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="1cae8-146">For information on how tooacquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="1cae8-147">Most már konfigurálhatja az Azure AD tooprovision felhasználói fiókok tooCerner.</span><span class="sxs-lookup"><span data-stu-id="1cae8-147">Now you can configure Azure AD tooprovision user accounts tooCerner.</span></span> <span data-ttu-id="1cae8-148">Toohello bejelentkezés [Azure-portálon](https://portal.azure.com), és keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1cae8-148">Sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="1cae8-149">Ha már konfigurált Cerner központi egyszeri bejelentkezést, keresse meg az hello keresési mező Cerner központi-példány.</span><span class="sxs-lookup"><span data-stu-id="1cae8-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using hello search field.</span></span> <span data-ttu-id="1cae8-150">Máskülönben válassza **Hozzáadás** keresse meg a **Cerner központi** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="1cae8-150">Otherwise, select **Add** and search for **Cerner Central** in hello application gallery.</span></span> <span data-ttu-id="1cae8-151">Válassza ki a Cerner központi hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="1cae8-151">Select Cerner Central from hello search results, and add it tooyour list of applications.</span></span>

7.  <span data-ttu-id="1cae8-152">Jelölje ki a Cerner központi példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="1cae8-152">Select your instance of Cerner Central, then select hello **Provisioning** tab.</span></span>

8.  <span data-ttu-id="1cae8-153">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="1cae8-153">Set hello **Provisioning Mode** too**Automatic**.</span></span>

   ![Kiépítés Cerner központi](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="1cae8-155">Töltse ki a mezőket a következő hello **rendszergazdai hitelesítő adataival**:</span><span class="sxs-lookup"><span data-stu-id="1cae8-155">Fill in hello following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="1cae8-156">A hello **bérlői URL-cím** mezőbe írja be egy URL-címet az alábbi hello formátumban cseréje a "Felhasználói Résztvevőlista-tartományi-azonosító" #4. lépésben beszerzett hello tartomány azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1cae8-156">In hello **Tenant URL** field, enter a URL in hello format below, replacing "User-Roster-Realm-ID" with hello realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="1cae8-157">Védőfal: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="1cae8-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="1cae8-158">Éles: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="1cae8-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="1cae8-159">A hello **titkos Token** mezőben adja meg a #3. lépésben létrehozott hello OAuth tulajdonosi jogkivonatot, és kattintson a **kapcsolat tesztelése**.</span><span class="sxs-lookup"><span data-stu-id="1cae8-159">In hello **Secret Token** field, enter hello OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="1cae8-160">A portál hello upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1cae8-160">You should see a success notification on hello upper­right side of your portal.</span></span>

10. <span data-ttu-id="1cae8-161">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="1cae8-161">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

11. <span data-ttu-id="1cae8-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1cae8-162">Click **Save**.</span></span> 

12. <span data-ttu-id="1cae8-163">A hello **attribútum-leképezésekhez** területen hello felhasználó és csoport attribútumok toobe szinkronizálni az Azure AD tooCerner központi áttekintése.</span><span class="sxs-lookup"><span data-stu-id="1cae8-163">In hello **Attribute Mappings** section, review hello user and group attributes toobe synchronized from Azure AD tooCerner Central.</span></span> <span data-ttu-id="1cae8-164">kiválasztott attribútumok hello **egyező** használt toomatch hello felhasználói fiókok és csoportok Cerner központi a frissítési műveleteket a rendszer tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="1cae8-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="1cae8-165">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1cae8-165">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="1cae8-166">tooenable hello Azure AD létesítési szolgáltatás a Cerner központi, a módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="1cae8-166">tooenable hello Azure AD provisioning service for Cerner Central, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

14. <span data-ttu-id="1cae8-167">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1cae8-167">Click **Save**.</span></span> 

<span data-ttu-id="1cae8-168">Ez elindítja a kezdeti szinkronizálása hello azokat a felhasználókat, és/vagy csoportok hozzárendelve tooCerner központi hello felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1cae8-168">This starts hello initial synchronization of any users and/or groups assigned tooCerner Central in hello Users and Groups section.</span></span> <span data-ttu-id="1cae8-169">hello kezdeti szinkronizálás hosszabb, mint az ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg hello Azure AD létesítési szolgáltatás fut-e tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1cae8-169">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello Azure AD provisioning service is running.</span></span> <span data-ttu-id="1cae8-170">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az Cerner központi alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentések.</span><span class="sxs-lookup"><span data-stu-id="1cae8-170">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="1cae8-171">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="1cae8-171">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1cae8-172">További források</span><span class="sxs-lookup"><span data-stu-id="1cae8-172">Additional resources</span></span>

* [<span data-ttu-id="1cae8-173">Cerner központi: Az Azure AD identity adatok közzététele</span><span class="sxs-lookup"><span data-stu-id="1cae8-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="1cae8-174">Oktatóanyag: Cerner központi konfigurálása egyszeri bejelentkezéshez az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="1cae8-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="1cae8-175">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="1cae8-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="1cae8-176">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1cae8-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="1cae8-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cae8-177">Next steps</span></span>
* <span data-ttu-id="1cae8-178">[Ismerje meg, hogy miként naplózza az tooreview és get-kiépítés tevékenység jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="1cae8-178">[Learn how tooreview logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
