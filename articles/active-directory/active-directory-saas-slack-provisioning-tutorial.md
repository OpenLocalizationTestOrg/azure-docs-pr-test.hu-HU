---
title: "Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooSlack."
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
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a><span data-ttu-id="fa0de-103">Oktatóanyag: Slackhez konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="fa0de-103">Tutorial: Configuring Slack for Automatic User Provisioning</span></span>


<span data-ttu-id="fa0de-104">hello Ez az oktatóanyag célja tooshow meg hello lépéseket kell tooperform a Slackhez és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="fa0de-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Slack and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSlack.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fa0de-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fa0de-105">Prerequisites</span></span>

<span data-ttu-id="fa0de-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="fa0de-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="fa0de-107">Az Azure Active Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="fa0de-107">An Azure Active Active directory tenant</span></span>
*   <span data-ttu-id="fa0de-108">A Slackhez-bérlőben hello [plusz terv](https://aadsyncfabric.slack.com/pricing) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="fa0de-108">A Slack tenant with hello [Plus plan](https://aadsyncfabric.slack.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="fa0de-109">A Slackhez Team rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="fa0de-109">A user account in Slack with Team Admin permissions</span></span> 

<span data-ttu-id="fa0de-110">Megjegyzés: hello az Azure AD-integrációs kiépítés támaszkodik hello [Slackhez SCIM API](https://api.slack.com/scim) Ez az hello terv plusz vagy nagyobb a rendelkezésre álló tooSlack csoportok.</span><span class="sxs-lookup"><span data-stu-id="fa0de-110">Note: hello Azure AD provisioning integration relies on hello [Slack SCIM API](https://api.slack.com/scim) which is available tooSlack teams on hello Plus plan or better.</span></span>

## <a name="assigning-users-tooslack"></a><span data-ttu-id="fa0de-111">Felhasználók tooSlack hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fa0de-111">Assigning users tooSlack</span></span>

<span data-ttu-id="fa0de-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="fa0de-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="fa0de-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD-szinkronizálásra kerül.</span><span class="sxs-lookup"><span data-stu-id="fa0de-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="fa0de-114">Hello szolgáltatás kiépítését engedélyezése és konfigurálása, mielőtt kell toodecide milyen felhasználói és/vagy csoportok tooyour közzététele a Slack-alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="fa0de-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Slack app.</span></span> <span data-ttu-id="fa0de-115">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Slack app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="fa0de-115">Once decided, you can assign these users tooyour Slack app by following hello instructions here:</span></span>

[<span data-ttu-id="fa0de-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="fa0de-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a><span data-ttu-id="fa0de-117">Fontos tippek a felhasználók tooSlack hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fa0de-117">Important tips for assigning users tooSlack</span></span>

*   <span data-ttu-id="fa0de-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSlack tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="fa0de-118">It is recommended that a single Azure AD user be assigned tooSlack tootest hello provisioning configuration.</span></span> <span data-ttu-id="fa0de-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="fa0de-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="fa0de-120">Amikor egy felhasználó tooSlack rendel, ki kell választania hello **felhasználói** vagy a "Csoport" szerepkör hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="fa0de-120">When assigning a user tooSlack, you must select hello **User** or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="fa0de-121">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fa0de-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-tooslack"></a><span data-ttu-id="fa0de-122">Felhasználók átadásához tooSlack konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa0de-122">Configuring user provisioning tooSlack</span></span> 

<span data-ttu-id="fa0de-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSlack felhasználói fiók kiépítése API, és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése és tiltsa le a hozzárendelt felhasználói fiókok a Slackhez alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="fa0de-123">This section guides you through connecting your Azure AD tooSlack's user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in Slack based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="fa0de-124">**Tipp:** is választhatja, hogy SAML-alapú egyszeri bejelentkezés a Slackhez, (az Azure portálon) hello megjelenő utasításokat követve tooenabled [https://portal.azure.com].</span><span class="sxs-lookup"><span data-stu-id="fa0de-124">**Tip:** You may also choose tooenabled SAML-based Single Sign-On for Slack, following hello instructions provided in (Azure portal)[https://portal.azure.com].</span></span> <span data-ttu-id="fa0de-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="fa0de-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a><span data-ttu-id="fa0de-126">tooconfigure automatikus felhasználói fiók tooSlack kiépítése az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="fa0de-126">tooconfigure automatic user account provisioning tooSlack in Azure AD:</span></span>


1)  <span data-ttu-id="fa0de-127">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="fa0de-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2) <span data-ttu-id="fa0de-128">Ha már konfigurált Slackhez egyszeri bejelentkezést, keresse meg a Slackhez hello keresési mező példányát.</span><span class="sxs-lookup"><span data-stu-id="fa0de-128">If you have already configured Slack for single sign-on, search for your instance of Slack using hello search field.</span></span> <span data-ttu-id="fa0de-129">Máskülönben válassza **Hozzáadás** keresse meg a **Slackhez** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="fa0de-129">Otherwise, select **Add** and search for **Slack** in hello application gallery.</span></span> <span data-ttu-id="fa0de-130">Válassza ki a Slackhez hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="fa0de-130">Select Slack from hello search results, and add it tooyour list of applications.</span></span>

3)  <span data-ttu-id="fa0de-131">Jelölje ki a Slackhez példányát, majd válassza ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="fa0de-131">Select your instance of Slack, then select hello **Provisioning** tab.</span></span>

4)  <span data-ttu-id="fa0de-132">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="fa0de-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![Slack-kiépítés](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  <span data-ttu-id="fa0de-134">A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="fa0de-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="fa0de-135">A Slack engedélyezési párbeszédpanel megnyílik egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="fa0de-135">This opens a Slack authorization dialog in a new browser window.</span></span> 

6) <span data-ttu-id="fa0de-136">Hello új ablakban jelentkezzen be arra a Slackhez a csapat rendszergazda fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="fa0de-136">In hello new window, sign into Slack using your Team Admin account.</span></span> <span data-ttu-id="fa0de-137">hello eredményül kapott engedélyezési párbeszédpanelen válassza ki a Slack csoport, amely szeretné, hogy tooenable kiépítést, és válassza hello **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="fa0de-137">in hello resulting authorization dialog, select hello Slack team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="fa0de-138">Egyszer befejezett, visszatérési toohello az Azure portál toocomplete hello konfigurálása kiosztás.</span><span class="sxs-lookup"><span data-stu-id="fa0de-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

![Engedélyezési párbeszédpanel](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) <span data-ttu-id="fa0de-140">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour közzététele a Slack-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa0de-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Slack app.</span></span> <span data-ttu-id="fa0de-141">Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Slack fiók Team rendszergazdai jogosultságokkal rendelkezik, és próbálja meg újból. "Engedélyezés" lépés hello.</span><span class="sxs-lookup"><span data-stu-id="fa0de-141">If hello connection fails, ensure your Slack account has Team Admin permissions and try hello "Authorize" step again.</span></span>

8) <span data-ttu-id="fa0de-142">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="fa0de-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

9) <span data-ttu-id="fa0de-143">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fa0de-143">Click **Save**.</span></span> 

10) <span data-ttu-id="fa0de-144">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="fa0de-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSlack**.</span></span>

11) <span data-ttu-id="fa0de-145">A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello felhasználói attribútumok szinkronizálva lesznek az Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="fa0de-145">In hello **Attribute Mappings** section, review hello user attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="fa0de-146">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok a Slackhez a frissítési műveletek lesz.</span><span class="sxs-lookup"><span data-stu-id="fa0de-146">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts in Slack for update operations.</span></span> <span data-ttu-id="fa0de-147">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="fa0de-147">Select hello Save button toocommit any changes.</span></span>

12) <span data-ttu-id="fa0de-148">tooenable hello Azure AD létesítési szolgáltatás a Slackhez, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="fa0de-148">tooenable hello Azure AD provisioning service for Slack, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13) <span data-ttu-id="fa0de-149">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fa0de-149">Click **Save**.</span></span> 

<span data-ttu-id="fa0de-150">Felhasználók és/vagy csoportok tooSlack a hello felhasználók és csoportok szakasz hello kezdeti szinkronizálása elindítja.</span><span class="sxs-lookup"><span data-stu-id="fa0de-150">This will start hello initial synchronization of any users and/or groups assigned tooSlack in hello Users and Groups section.</span></span> <span data-ttu-id="fa0de-151">Megjegyzés: hello kezdeti szinkronizálás hosszabb, mint körülbelül 10 percenként bekövetkező mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform kezeléséért.</span><span class="sxs-lookup"><span data-stu-id="fa0de-151">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 10 minutes as long as hello service is running.</span></span> <span data-ttu-id="fa0de-152">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Slack app a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="fa0de-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>

## <a name="optional-configuring-group-object-provisioning-tooslack"></a><span data-ttu-id="fa0de-153">[Választható] Kiépítés tooSlack csoportházirend-objektum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa0de-153">[Optional] Configuring group object provisioning tooSlack</span></span> 

<span data-ttu-id="fa0de-154">Másik lehetőségként engedélyezheti a hello kiépítése az Azure AD tooSlack objektumok.</span><span class="sxs-lookup"><span data-stu-id="fa0de-154">Optionally, you can enable hello provisioning of group objects from Azure AD tooSlack.</span></span> <span data-ttu-id="fa0de-155">Ez nem azonos a "felhasználói csoportok hozzárendelése", a hello tényleges csoportobjektumnak továbbá tooits tagok replikálásra kerülnek a Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="fa0de-155">This is different from "assigning groups of users", in that hello actual group object in addition tooits members will be replicated from Azure AD tooSlack.</span></span> <span data-ttu-id="fa0de-156">Például ha a "Saját csoport" nevű az Azure AD-csoport, egy "Saját csoport" nevű identitical csoportot az Slackhez belül létrejön.</span><span class="sxs-lookup"><span data-stu-id="fa0de-156">For example, if you have a group named "My Group" in Azure AD, an identitical group named "My Group" will be created inside Slack.</span></span>

### <a name="tooenable-provisioning-of-group-objects"></a><span data-ttu-id="fa0de-157">tooenable kiépítés objektumok:</span><span class="sxs-lookup"><span data-stu-id="fa0de-157">tooenable provisioning of group objects:</span></span>

1) <span data-ttu-id="fa0de-158">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-csoportok tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="fa0de-158">Under hello Mappings section, select **Synchronize Azure Active Directory Groups tooSlack**.</span></span>

2) <span data-ttu-id="fa0de-159">Állítson be engedélyezve tooYes hello attribútum leképezési panelen.</span><span class="sxs-lookup"><span data-stu-id="fa0de-159">In hello Attribute Mapping blade, set Enabled tooYes.</span></span>

3) <span data-ttu-id="fa0de-160">A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello attribútumokat, amelyek az Azure AD tooSlack szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="fa0de-160">In hello **Attribute Mappings** section, review hello group attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="fa0de-161">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok lesz a frissítési műveletek Slackhez használt toomatch hello csoportok.</span><span class="sxs-lookup"><span data-stu-id="fa0de-161">Note that hello attributes selected as **Matching** properties will be used toomatch hello groups in Slack for update operations.</span></span> 

4) <span data-ttu-id="fa0de-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fa0de-162">Click **Save**.</span></span>

<span data-ttu-id="fa0de-163">Ez az eredmény bármely csoporthoz rendelt objektumok tooSlack hello a **felhasználók és csoportok** szakaszban az Azure AD tooSlack a teljes szinkronizálás folyik.</span><span class="sxs-lookup"><span data-stu-id="fa0de-163">This result in any group objects assigned tooSlack in hello **Users and Groups** section being fully synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="fa0de-164">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Slack app a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="fa0de-164">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="fa0de-165">További források</span><span class="sxs-lookup"><span data-stu-id="fa0de-165">Additional Resources</span></span>

* [<span data-ttu-id="fa0de-166">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="fa0de-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="fa0de-167">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fa0de-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
