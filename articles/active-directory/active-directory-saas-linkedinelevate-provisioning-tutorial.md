---
title: "Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés automatikus a felhasználók átadása az Azure Active Directoryhoz |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, jogosultságszint-emelés tooLinkedIn."
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
ms.openlocfilehash: 08201c078ece0054e75ec0c004840e5186e0e704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-elevate-for-automatic-user-provisioning"></a><span data-ttu-id="194be-103">Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés automatikus a felhasználók átadása</span><span class="sxs-lookup"><span data-stu-id="194be-103">Tutorial: Configuring LinkedIn Elevate for Automatic User Provisioning</span></span>


<span data-ttu-id="194be-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform LinkedIn jogosultságszint-emelés és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooLinkedIn jogosultságszint-emelés a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="194be-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LinkedIn Elevate and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLinkedIn Elevate.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="194be-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="194be-105">Prerequisites</span></span>

<span data-ttu-id="194be-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="194be-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="194be-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="194be-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="194be-108">A LinkedIn jogosultságszint-emelés bérlő</span><span class="sxs-lookup"><span data-stu-id="194be-108">A LinkedIn Elevate tenant</span></span> 
*   <span data-ttu-id="194be-109">A hozzáférés toohello LinkedIn Account Center LinkedIn jogosultságszint-emelés a rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="194be-109">An administrator account in LinkedIn Elevate with access toohello LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="194be-110">Az Azure Active Directory integrálható LinkedIn jogosultságszint-emelés hello segítségével [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="194be-110">Azure Active Directory integrates with LinkedIn Elevate using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toolinkedin-elevate"></a><span data-ttu-id="194be-111">Felhasználók tooLinkedIn jogosultságszint-emelés hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="194be-111">Assigning users tooLinkedIn Elevate</span></span>

<span data-ttu-id="194be-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="194be-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="194be-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD-szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="194be-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="194be-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, szüksége lesz toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooLinkedIn jogosultságszint-emelés kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="194be-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooLinkedIn Elevate.</span></span> <span data-ttu-id="194be-115">Ha úgy döntött, hozzárendelheti a felhasználók tooLinkedIn jogosultságszint-emelés itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="194be-115">Once decided, you can assign these users tooLinkedIn Elevate by following hello instructions here:</span></span>

[<span data-ttu-id="194be-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="194be-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-elevate"></a><span data-ttu-id="194be-117">Fontos tippek a felhasználók tooLinkedIn jogosultságszint-emelés hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="194be-117">Important tips for assigning users tooLinkedIn Elevate</span></span>

*   <span data-ttu-id="194be-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooLinkedIn jogosultságszint-emelés tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="194be-118">It is recommended that a single Azure AD user be assigned tooLinkedIn Elevate tootest hello provisioning configuration.</span></span> <span data-ttu-id="194be-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="194be-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="194be-120">Amikor egy felhasználó tooLinkedIn jogosultságszint-emelés rendel, ki kell választania hello **felhasználói** szerepkör hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="194be-120">When assigning a user tooLinkedIn Elevate, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="194be-121">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="194be-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-toolinkedin-elevate"></a><span data-ttu-id="194be-122">Felhasználók átadásához tooLinkedIn jogosultságszint-emelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="194be-122">Configuring user provisioning tooLinkedIn Elevate</span></span>

<span data-ttu-id="194be-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooLinkedIn jogosultságszint-emelés tartozó SCIM felhasználói fiók kiépítése API, és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése és tiltsa le a hozzárendelt felhasználói fiókok a LinkedIn jogosultságszint-emelés felhasználók és csoportok alapján az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="194be-123">This section guides you through connecting your Azure AD tooLinkedIn Elevate's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in LinkedIn Elevate based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="194be-124">**Tipp:** is választhatja, hogy tooenabled SAML-alapú egyszeri bejelentkezést a LinkedIn jogosultságszint-emelés, hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="194be-124">**Tip:** You may also choose tooenabled SAML-based Single Sign-On for LinkedIn Elevate, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="194be-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="194be-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-elevate-in-azure-ad"></a><span data-ttu-id="194be-126">tooconfigure automatikus felhasználói fiók tooLinkedIn jogosultságszint-emelés kiépítése az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="194be-126">tooconfigure automatic user account provisioning tooLinkedIn Elevate in Azure AD:</span></span>


<span data-ttu-id="194be-127">első lépés hello van tooretrieve a LinkedIn hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="194be-127">hello first step is tooretrieve your LinkedIn access token.</span></span> <span data-ttu-id="194be-128">Ha a vállalati rendszergazdák, önálló megadhat egy hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="194be-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="194be-129">A fiók center lépjen túl**beállítások &gt; globális beállítások** és a nyitott hello **SCIM telepítő** panel.</span><span class="sxs-lookup"><span data-stu-id="194be-129">In your account center, go too**Settings &gt; Global Settings** and open hello **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="194be-130">Ha igénybe veszi hello fiók center közvetlenül, hanem a kapcsolaton keresztül érhető el a lépéseket követve hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="194be-130">If you are accessing hello account center directly rather than through a link, you can reach it using hello following steps.</span></span>

1)  <span data-ttu-id="194be-131">Jelentkezzen be tooAccount Center.</span><span class="sxs-lookup"><span data-stu-id="194be-131">Sign in tooAccount Center.</span></span>

2)  <span data-ttu-id="194be-132">Válassza ki **Admin &gt; rendszergazdai beállítások** .</span><span class="sxs-lookup"><span data-stu-id="194be-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="194be-133">Kattintson a **speciális integrációja** hello bal oldalsávon.</span><span class="sxs-lookup"><span data-stu-id="194be-133">Click **Advanced Integrations** on hello left sidebar.</span></span> <span data-ttu-id="194be-134">Irányított toohello fiók center áll.</span><span class="sxs-lookup"><span data-stu-id="194be-134">You are directed toohello account center.</span></span>

4)  <span data-ttu-id="194be-135">Kattintson a **+ Hozzáadás új SCIM konfigurációs** és hello eljárással minden mező kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="194be-135">Click **+ Add new SCIM configuration** and follow hello procedure by filling in each field.</span></span>

> <span data-ttu-id="194be-136">Autoassign licencek nincs engedélyezve, az azt jelenti, hogy csak a felhasználói adatok van-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="194be-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate1.PNG)

> <span data-ttu-id="194be-138">Autolicense hozzárendelés engedélyezve van, úgy kell toonote az alkalmazáspéldány és licenc típusa.</span><span class="sxs-lookup"><span data-stu-id="194be-138">When auto­license assignment is enabled, you need toonote the application instance and license type.</span></span> <span data-ttu-id="194be-139">Licencek hozzárendelésének az első származnak, először kiszolgálására alapján, addig, amíg az összes hello licenc kerül.</span><span class="sxs-lookup"><span data-stu-id="194be-139">Licenses are assigned on a first come, first serve basis until all hello licenses are taken.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate2.PNG)

5)  <span data-ttu-id="194be-141">Kattintson a **Generate token**.</span><span class="sxs-lookup"><span data-stu-id="194be-141">Click **Generate token**.</span></span> <span data-ttu-id="194be-142">A hozzáférési token megjelenítési hello alatt kell megjelennie **hozzáférési jogkivonat** mező.</span><span class="sxs-lookup"><span data-stu-id="194be-142">You should see your access token display under hello **Access token** field.</span></span>

6)  <span data-ttu-id="194be-143">Mentse a hozzáférési token tooyour vágólapra vagy a számítógép hello oldal elhagyása előtt.</span><span class="sxs-lookup"><span data-stu-id="194be-143">Save your access token tooyour clipboard or computer before leaving hello page.</span></span>

7) <span data-ttu-id="194be-144">Ezután jelentkezzen be a toohello [Azure-portálon](https://portal.azure.com), és keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="194be-144">Next, sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="194be-145">Ha már konfigurált LinkedIn jogosultságszint-emelés egyszeri bejelentkezést, keresse meg a LinkedIn jogosultságszint-emelés hello keresési mező példányát.</span><span class="sxs-lookup"><span data-stu-id="194be-145">If you have already configured LinkedIn Elevate for single sign-on, search for your instance of LinkedIn Elevate using hello search field.</span></span> <span data-ttu-id="194be-146">Máskülönben válassza **Hozzáadás** keresse meg a **LinkedIn jogosultságszint-emelés** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="194be-146">Otherwise, select **Add** and search for **LinkedIn Elevate** in hello application gallery.</span></span> <span data-ttu-id="194be-147">Válassza ki a LinkedIn jogosultságszint-emelés hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="194be-147">Select LinkedIn Elevate from hello search results, and add it tooyour list of applications.</span></span>

9)  <span data-ttu-id="194be-148">Jelölje ki a LinkedIn jogosultságszint-emelés példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="194be-148">Select your instance of LinkedIn Elevate, then select hello **Provisioning** tab.</span></span>

10) <span data-ttu-id="194be-149">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="194be-149">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate3.PNG)

11)  <span data-ttu-id="194be-151">Töltse ki a mezőket a következő hello **rendszergazdai hitelesítő adataival** :</span><span class="sxs-lookup"><span data-stu-id="194be-151">Fill in hello following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="194be-152">A hello **bérlői URL-cím** mezőbe írja be a https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="194be-152">In hello **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="194be-153">A hello **titkos Token** mezőben adja meg az 1. lépésben létrehozott hello hozzáférési jogkivonatot, és kattintson a **kapcsolat tesztelése** .</span><span class="sxs-lookup"><span data-stu-id="194be-153">In hello **Secret Token** field, enter hello access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="194be-154">A portál hello upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="194be-154">You should see a success notification on hello upper­right side of   your portal.</span></span>

12) <span data-ttu-id="194be-155">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="194be-155">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

13) <span data-ttu-id="194be-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="194be-156">Click **Save**.</span></span> 

14) <span data-ttu-id="194be-157">A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello felhasználói és csoportattribútum, amely az Azure AD tooLinkedIn jogosultságszint-emelés szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="194be-157">In hello **Attribute Mappings** section, review hello user and group attributes that will be synchronized from Azure AD tooLinkedIn Elevate.</span></span> <span data-ttu-id="194be-158">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok és csoportok LinkedIn jogosultságszint-emelés a frissítési műveletek lesz.</span><span class="sxs-lookup"><span data-stu-id="194be-158">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts and groups in LinkedIn Elevate for update operations.</span></span> <span data-ttu-id="194be-159">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="194be-159">Select hello Save button toocommit any changes.</span></span>

![LinkedIn jogosultságszint-emelés kiépítése](./media/active-directory-saas-linkedin-elevate-provisioning-tutorial/linkedin_elevate4.PNG)

15) <span data-ttu-id="194be-161">tooenable hello Azure AD létesítési szolgáltatás a LinkedIn jogosultságszint-emelés, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="194be-161">tooenable hello Azure AD provisioning service for LinkedIn Elevate, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

16) <span data-ttu-id="194be-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="194be-162">Click **Save**.</span></span> 

<span data-ttu-id="194be-163">Azokat a felhasználókat hello kezdeti szinkronizálása elindítja és/vagy csoportok hozzárendelve tooLinkedIn jogosultságszint-emelés hello felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="194be-163">This will start hello initial synchronization of any users and/or groups assigned tooLinkedIn Elevate in hello Users and Groups section.</span></span> <span data-ttu-id="194be-164">Megjegyzés: hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform kezeléséért.</span><span class="sxs-lookup"><span data-stu-id="194be-164">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="194be-165">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a LinkedIn jogosultságszint-emelés app a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="194be-165">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your LinkedIn Elevate app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="194be-166">További források</span><span class="sxs-lookup"><span data-stu-id="194be-166">Additional Resources</span></span>

* [<span data-ttu-id="194be-167">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="194be-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="194be-168">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="194be-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
