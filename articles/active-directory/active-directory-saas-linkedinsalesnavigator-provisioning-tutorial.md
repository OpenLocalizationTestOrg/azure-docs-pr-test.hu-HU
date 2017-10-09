---
title: "Oktatóanyag: LinkedIn értékesítési Navigator konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooLinkedIn értékesítési Navigator."
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
ms.openlocfilehash: 322c5271535994c13a9fafadbf74f356cdfe865d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a><span data-ttu-id="9e209-103">Oktatóanyag: LinkedIn értékesítési Navigator automatikus felhasználói kialakítási konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9e209-103">Tutorial: Configuring LinkedIn Sales Navigator for Automatic User Provisioning</span></span>


<span data-ttu-id="9e209-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform LinkedIn értékesítési Navigator és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooLinkedIn értékesítési Navigator a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9e209-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LinkedIn Sales Navigator and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLinkedIn Sales Navigator.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="9e209-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9e209-105">Prerequisites</span></span>

<span data-ttu-id="9e209-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9e209-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="9e209-107">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="9e209-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="9e209-108">A LinkedIn értékesítési Navigator bérlő</span><span class="sxs-lookup"><span data-stu-id="9e209-108">A LinkedIn Sales Navigator tenant</span></span> 
*   <span data-ttu-id="9e209-109">A hozzáférés toohello LinkedIn Account Center LinkedIn értékesítési Navigator rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="9e209-109">An administrator account in LinkedIn Sales Navigator with access toohello LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="9e209-110">Az Azure Active Directory LinkedIn értékesítési Navigator hello segítségével integrálható [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="9e209-110">Azure Active Directory integrates with LinkedIn Sales Navigator using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toolinkedin-sales-navigator"></a><span data-ttu-id="9e209-111">Felhasználók tooLinkedIn értékesítési Navigator hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9e209-111">Assigning users tooLinkedIn Sales Navigator</span></span>

<span data-ttu-id="9e209-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="9e209-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="9e209-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD-szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="9e209-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="9e209-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, szüksége lesz toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooLinkedIn értékesítési Navigator kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="9e209-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooLinkedIn Sales Navigator.</span></span> <span data-ttu-id="9e209-115">Ha úgy döntött, rendelheti hozzá ezen felhasználók tooLinkedIn értékesítési Navigator itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="9e209-115">Once decided, you can assign these users tooLinkedIn Sales Navigator by following hello instructions here:</span></span>

[<span data-ttu-id="9e209-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="9e209-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-sales-navigator"></a><span data-ttu-id="9e209-117">Fontos tippek az értékesítési Navigator felhasználók tooLinkedIn</span><span class="sxs-lookup"><span data-stu-id="9e209-117">Important tips for assigning users tooLinkedIn Sales Navigator</span></span>

*   <span data-ttu-id="9e209-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooLinkedIn értékesítési Navigator tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="9e209-118">It is recommended that a single Azure AD user be assigned tooLinkedIn Sales Navigator tootest hello provisioning configuration.</span></span> <span data-ttu-id="9e209-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="9e209-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="9e209-120">Amikor egy felhasználó tooLinkedIn értékesítési Navigator rendel, ki kell választania hello **felhasználói** szerepkör hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="9e209-120">When assigning a user tooLinkedIn Sales Navigator, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="9e209-121">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9e209-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-toolinkedin-sales-navigator"></a><span data-ttu-id="9e209-122">Felhasználók átadásához tooLinkedIn értékesítési Navigator konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9e209-122">Configuring user provisioning tooLinkedIn Sales Navigator</span></span>

<span data-ttu-id="9e209-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooLinkedIn értékesítési Navigator SCIM felhasználói fiók kiépítése API, és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése és tiltsa le a hozzárendelt felhasználói fiókok a LinkedIn értékesítési Navigator felhasználó alapján és az Azure AD-csoport-hozzárendelést.</span><span class="sxs-lookup"><span data-stu-id="9e209-123">This section guides you through connecting your Azure AD tooLinkedIn Sales Navigator's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in LinkedIn Sales Navigator based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="9e209-124">Dönthet úgy is SAML-alapú egyszeri bejelentkezést tooenabled LinkedIn értékesítési Navigator hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9e209-124">You may also choose tooenabled SAML-based Single Sign-On for LinkedIn Sales Navigator, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9e209-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="9e209-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-sales-navigator-in-azure-ad"></a><span data-ttu-id="9e209-126">tooconfigure automatikus felhasználói fiók tooLinkedIn értékesítési Navigator kiépítése az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="9e209-126">tooconfigure automatic user account provisioning tooLinkedIn Sales Navigator in Azure AD:</span></span>


<span data-ttu-id="9e209-127">első lépés hello van tooretrieve a LinkedIn hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="9e209-127">hello first step is tooretrieve your LinkedIn access token.</span></span> <span data-ttu-id="9e209-128">Ha a vállalati rendszergazdák, önálló megadhat egy hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="9e209-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="9e209-129">A fiók center lépjen túl**beállítások &gt; globális beállítások** és a nyitott hello **SCIM telepítő** panel.</span><span class="sxs-lookup"><span data-stu-id="9e209-129">In your account center, go too**Settings &gt; Global Settings** and open hello **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="9e209-130">Ha igénybe veszi hello fiók center közvetlenül, hanem a kapcsolaton keresztül érhető el a lépéseket követve hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="9e209-130">If you are accessing hello account center directly rather than through a link, you can reach it using hello following steps.</span></span>

1)  <span data-ttu-id="9e209-131">Jelentkezzen be tooAccount Center.</span><span class="sxs-lookup"><span data-stu-id="9e209-131">Sign in tooAccount Center.</span></span>

2)  <span data-ttu-id="9e209-132">Válassza ki **Admin &gt; rendszergazdai beállítások** .</span><span class="sxs-lookup"><span data-stu-id="9e209-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="9e209-133">Kattintson a **speciális integrációja** hello bal oldalsávon.</span><span class="sxs-lookup"><span data-stu-id="9e209-133">Click **Advanced Integrations** on hello left sidebar.</span></span> <span data-ttu-id="9e209-134">Irányított toohello fiók center áll.</span><span class="sxs-lookup"><span data-stu-id="9e209-134">You are directed toohello account center.</span></span>

4)  <span data-ttu-id="9e209-135">Kattintson a **+ Hozzáadás új SCIM konfigurációs** és hello eljárással minden mező kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="9e209-135">Click **+ Add new SCIM configuration** and follow hello procedure by filling in each field.</span></span>

> <span data-ttu-id="9e209-136">Autoassign licencek nincs engedélyezve, az azt jelenti, hogy csak a felhasználói adatok van-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="9e209-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> <span data-ttu-id="9e209-138">Autolicense hozzárendelés engedélyezve van, úgy kell toonote az alkalmazáspéldány és licenc típusa.</span><span class="sxs-lookup"><span data-stu-id="9e209-138">When auto­license assignment is enabled, you need toonote the application instance and license type.</span></span> <span data-ttu-id="9e209-139">Licencek hozzárendelésének az első származnak, először kiszolgálására alapján, addig, amíg az összes hello licenc kerül.</span><span class="sxs-lookup"><span data-stu-id="9e209-139">Licenses are assigned on a first come, first serve basis until all hello licenses are taken.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  <span data-ttu-id="9e209-141">Kattintson a **Generate token**.</span><span class="sxs-lookup"><span data-stu-id="9e209-141">Click **Generate token**.</span></span> <span data-ttu-id="9e209-142">A hozzáférési token megjelenítési hello alatt kell megjelennie **hozzáférési jogkivonat** mező.</span><span class="sxs-lookup"><span data-stu-id="9e209-142">You should see your access token display under hello **Access token** field.</span></span>

6)  <span data-ttu-id="9e209-143">Mentse a hozzáférési token tooyour vágólapra vagy a számítógép hello oldal elhagyása előtt.</span><span class="sxs-lookup"><span data-stu-id="9e209-143">Save your access token tooyour clipboard or computer before leaving hello page.</span></span>

7) <span data-ttu-id="9e209-144">Ezután jelentkezzen be a toohello [Azure-portálon](https://portal.azure.com), és keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9e209-144">Next, sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="9e209-145">Ha már konfigurált LinkedIn értékesítési Navigator egyszeri bejelentkezést, keresse meg a példány LinkedIn értékesítési Navigátor hello keresési mező.</span><span class="sxs-lookup"><span data-stu-id="9e209-145">If you have already configured LinkedIn Sales Navigator for single sign-on, search for your instance of LinkedIn Sales Navigator using hello search field.</span></span> <span data-ttu-id="9e209-146">Máskülönben válassza **Hozzáadás** keresse meg a **LinkedIn értékesítési Navigator** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="9e209-146">Otherwise, select **Add** and search for **LinkedIn Sales Navigator** in hello application gallery.</span></span> <span data-ttu-id="9e209-147">Válassza ki a LinkedIn értékesítési Navigator hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="9e209-147">Select LinkedIn Sales Navigator from hello search results, and add it tooyour list of applications.</span></span>

9)  <span data-ttu-id="9e209-148">Jelölje ki a LinkedIn értékesítési Navigator példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="9e209-148">Select your instance of LinkedIn Sales Navigator, then select hello **Provisioning** tab.</span></span>

10) <span data-ttu-id="9e209-149">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="9e209-149">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  <span data-ttu-id="9e209-151">Töltse ki a mezőket a következő hello **rendszergazdai hitelesítő adataival** :</span><span class="sxs-lookup"><span data-stu-id="9e209-151">Fill in hello following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="9e209-152">A hello **bérlői URL-cím** mezőbe írja be a https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="9e209-152">In hello **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="9e209-153">A hello **titkos Token** mezőben adja meg az 1. lépésben létrehozott hello hozzáférési jogkivonatot, és kattintson a **kapcsolat tesztelése** .</span><span class="sxs-lookup"><span data-stu-id="9e209-153">In hello **Secret Token** field, enter hello access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="9e209-154">A portál hello upperright oldalán egy sikeres következő értesítésnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="9e209-154">You should see a success notification on hello upper­right side of   your portal.</span></span>

12) <span data-ttu-id="9e209-155">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="9e209-155">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

13) <span data-ttu-id="9e209-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9e209-156">Click **Save**.</span></span> 

14) <span data-ttu-id="9e209-157">A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello felhasználói és csoportattribútum szinkronizált az Azure AD tooLinkedIn értékesítési Navigator.</span><span class="sxs-lookup"><span data-stu-id="9e209-157">In hello **Attribute Mappings** section, review hello user and group attributes that will be synchronized from Azure AD tooLinkedIn Sales Navigator.</span></span> <span data-ttu-id="9e209-158">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok és csoportok LinkedIn értékesítési Navigator a frissítési műveletek lesz.</span><span class="sxs-lookup"><span data-stu-id="9e209-158">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts and groups in LinkedIn Sales Navigator for update operations.</span></span> <span data-ttu-id="9e209-159">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9e209-159">Select hello Save button toocommit any changes.</span></span>

![A Navigátor értékesítési LinkedIn-kiépítés](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) <span data-ttu-id="9e209-161">tooenable hello Azure AD létesítési szolgáltatás LinkedIn értékesítési Navigator módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="9e209-161">tooenable hello Azure AD provisioning service for LinkedIn Sales Navigator, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

16) <span data-ttu-id="9e209-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9e209-162">Click **Save**.</span></span> 

<span data-ttu-id="9e209-163">Hello kezdeti szinkronizálás bármely felhasználói és/vagy a hozzárendelt tooLinkedIn értékesítési Navigator hello felhasználók és csoportok szakaszban csoportok elindítja.</span><span class="sxs-lookup"><span data-stu-id="9e209-163">This will start hello initial synchronization of any users and/or groups assigned tooLinkedIn Sales Navigator in hello Users and Groups section.</span></span> <span data-ttu-id="9e209-164">Megjegyzés: hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform kezeléséért.</span><span class="sxs-lookup"><span data-stu-id="9e209-164">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="9e209-165">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást az értékesítési Navigator LinkedIn-alkalmazás a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="9e209-165">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your LinkedIn Sales Navigator app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9e209-166">További források</span><span class="sxs-lookup"><span data-stu-id="9e209-166">Additional Resources</span></span>

* [<span data-ttu-id="9e209-167">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="9e209-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="9e209-168">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9e209-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
