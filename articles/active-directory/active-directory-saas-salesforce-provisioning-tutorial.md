---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a><span data-ttu-id="1d249-103">Oktatóanyag: Salesforce konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="1d249-103">Tutorial: Configuring Salesforce for Automatic User Provisioning</span></span>

<span data-ttu-id="1d249-104">hello Ez az oktatóanyag célja tooshow hello lépéseket szükséges tooperform a Salesforce és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="1d249-104">hello objective of this tutorial is tooshow hello steps required tooperform in Salesforce and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSalesforce.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d249-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d249-105">Prerequisites</span></span>

<span data-ttu-id="1d249-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="1d249-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="1d249-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="1d249-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="1d249-108">Rendelkeznie kell egy érvényes bérlőt Salesforce a munkahelyén vagy a Salesforce oktatási célokra.</span><span class="sxs-lookup"><span data-stu-id="1d249-108">You must have a valid tenant for Salesforce for Work or Salesforce for Education.</span></span> <span data-ttu-id="1d249-109">Egy ingyenes próbafiók vagy a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d249-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="1d249-110">Egy felhasználói fiókot a Salesforce-ban Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="1d249-110">A user account in Salesforce with Team Admin permissions.</span></span>

## <a name="assigning-users-toosalesforce"></a><span data-ttu-id="1d249-111">Felhasználók tooSalesforce hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1d249-111">Assigning users tooSalesforce</span></span>

<span data-ttu-id="1d249-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="1d249-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="1d249-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="1d249-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="1d249-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Salesforce alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="1d249-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Salesforce app.</span></span> <span data-ttu-id="1d249-115">Ha úgy döntött, itt hello utasításokat követve rendelhet a felhasználók tooyour Salesforce alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="1d249-115">Once decided, you can assign these users tooyour Salesforce app by following hello instructions here:</span></span>

[<span data-ttu-id="1d249-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="1d249-116">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a><span data-ttu-id="1d249-117">Fontos tippek a felhasználók tooSalesforce hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1d249-117">Important tips for assigning users tooSalesforce</span></span>

*   <span data-ttu-id="1d249-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSalesforce tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="1d249-118">It is recommended that a single Azure AD user is assigned tooSalesforce tootest hello provisioning configuration.</span></span> <span data-ttu-id="1d249-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="1d249-119">Additional users and/or groups may be assigned later.</span></span>

*  <span data-ttu-id="1d249-120">Amikor egy felhasználó tooSalesforce rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="1d249-120">When assigning a user tooSalesforce, you must select a valid user role.</span></span> <span data-ttu-id="1d249-121">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="1d249-121">hello "Default Access" role does not work for provisioning</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d249-122">Ez az alkalmazás egyéni szerepkörök importál a Salesforce létesítésének folyamatát kell használnia, mely hello ügyfél érdemes tooselect felhasználók hozzárendelésekor hello részeként</span><span class="sxs-lookup"><span data-stu-id="1d249-122">This app imports custom roles from Salesforce as part of hello provisioning process, which hello customer may want tooselect when assigning users</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="1d249-123">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1d249-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="1d249-124">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSalesforce felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Salesforce alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben .</span><span class="sxs-lookup"><span data-stu-id="1d249-124">This section guides you through connecting your Azure AD tooSalesforce's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Salesforce based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="1d249-125">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Salesforce hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1d249-125">You may also choose tooenabled SAML-based Single Sign-On for Salesforce, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1d249-126">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="1d249-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="1d249-127">tooconfigure automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="1d249-127">tooconfigure automatic user account provisioning:</span></span>

<span data-ttu-id="1d249-128">hello ebben a szakaszban célja toooutline hogyan tooSalesforce tooenable a felhasználók átadása az Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="1d249-128">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce.</span></span>

1. <span data-ttu-id="1d249-129">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1d249-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="1d249-130">Ha már beállította az egyszeri bejelentkezés Salesforce, keresse meg az hello keresési mező Salesforce-példány.</span><span class="sxs-lookup"><span data-stu-id="1d249-130">If you have already configured Salesforce for single sign-on, search for your instance of Salesforce using hello search field.</span></span> <span data-ttu-id="1d249-131">Máskülönben válassza **Hozzáadás** keresse meg a **Salesforce** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="1d249-131">Otherwise, select **Add** and search for **Salesforce** in hello application gallery.</span></span> <span data-ttu-id="1d249-132">Válassza ki a Salesforce hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="1d249-132">Select Salesforce from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="1d249-133">Válassza ki az Salesforce-példány, majd válassza ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="1d249-133">Select your instance of Salesforce, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="1d249-134">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="1d249-134">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
<span data-ttu-id="1d249-135">![kiépítés](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="1d249-135">![provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="1d249-136">A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="1d249-136">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="1d249-137">a.</span><span class="sxs-lookup"><span data-stu-id="1d249-137">a.</span></span> <span data-ttu-id="1d249-138">A hello **rendszergazda felhasználóneve** szövegmezőhöz a Salesforce-fióknév, amely rendelkezik hello típus **rendszergazda** Salesforce.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="1d249-138">In hello **Admin User Name** textbox, type a Salesforce account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="1d249-139">b.</span><span class="sxs-lookup"><span data-stu-id="1d249-139">b.</span></span> <span data-ttu-id="1d249-140">A hello **rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="1d249-140">In hello **Admin Password** textbox, type hello password for this account.</span></span>

6. <span data-ttu-id="1d249-141">tooget a Salesforce biztonsági jogkivonatot, nyisson meg egy új lapot, és jelentkezzen be a hello azonos Salesforce rendszergazdai fiókot.</span><span class="sxs-lookup"><span data-stu-id="1d249-141">tooget your Salesforce security token, open a new tab and sign into hello same Salesforce admin account.</span></span> <span data-ttu-id="1d249-142">A hello jobb felső sarkában hello lap, kattintson a nevére, és kattintson **saját beállítások**.</span><span class="sxs-lookup"><span data-stu-id="1d249-142">On hello top right corner of hello page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="1d249-143">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1d249-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="1d249-144">A hello bal oldali navigációs ablaktábláján kattintson **személyes** tooexpand hello kapcsolódó szakaszt, és kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat**.</span><span class="sxs-lookup"><span data-stu-id="1d249-144">On hello left navigation pane, click **Personal** tooexpand hello related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="1d249-145">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1d249-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="1d249-146">A hello **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d249-146">On hello **Reset My Security Token** page, click **Reset Security Token** button.</span></span>

    <span data-ttu-id="1d249-147">![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "automatikus felhasználó-kiépítés engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="1d249-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="1d249-148">Ellenőrizze a rendszergazdai fiókhoz társított hello e-mailben kapják.</span><span class="sxs-lookup"><span data-stu-id="1d249-148">Check hello email inbox associated with this admin account.</span></span> <span data-ttu-id="1d249-149">Keresse meg a Salesforce.com hello új biztonsági jogkivonatot tartalmazó e-mailt.</span><span class="sxs-lookup"><span data-stu-id="1d249-149">Look for an email from Salesforce.com that contains hello new security token.</span></span>
10. <span data-ttu-id="1d249-150">Hello token, nyissa meg a tooyour az Azure AD-ablakban másolja és illessze be hello **szoftvercsatorna Token** mező.</span><span class="sxs-lookup"><span data-stu-id="1d249-150">Copy hello token, go tooyour Azure AD window, and paste it into hello **Socket Token** field.</span></span>

11. <span data-ttu-id="1d249-151">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Salesforce alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="1d249-151">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Salesforce app.</span></span>

12. <span data-ttu-id="1d249-152">A hello **értesítő e-mailt** mezőbe írja be a hello e-mail címet vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be az alábbi hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="1d249-152">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox below.</span></span>

13. <span data-ttu-id="1d249-153">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="1d249-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="1d249-154">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSalesforce.**</span><span class="sxs-lookup"><span data-stu-id="1d249-154">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSalesforce.**</span></span>

15. <span data-ttu-id="1d249-155">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooSalesforce szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="1d249-155">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSalesforce.</span></span> <span data-ttu-id="1d249-156">Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat a Salesforce-ban a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="1d249-156">Note that hello attributes selected as **Matching** properties are used toomatch hello user accounts in Salesforce for update operations.</span></span> <span data-ttu-id="1d249-157">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1d249-157">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="1d249-158">tooenable hello Azure AD létesítési szolgáltatás a Salesforce, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="1d249-158">tooenable hello Azure AD provisioning service for Salesforce, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

17. <span data-ttu-id="1d249-159">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="1d249-159">Click **Save.**</span></span>

<span data-ttu-id="1d249-160">Ezzel elindítja a kezdeti szinkronizálás hello bármely felhasználói és/vagy csoportok tooSalesforce a hello felhasználók és csoportok szakasz.</span><span class="sxs-lookup"><span data-stu-id="1d249-160">This starts hello initial synchronization of any users and/or groups assigned tooSalesforce in hello Users and Groups section.</span></span> <span data-ttu-id="1d249-161">Vegye figyelembe, hogy a hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1d249-161">Note that hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="1d249-162">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Salesforce alkalmazást a kiépítés végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="1d249-162">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Salesforce app.</span></span>

<span data-ttu-id="1d249-163">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="1d249-163">You can now create a test account.</span></span> <span data-ttu-id="1d249-164">Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooSalesforce too20 perc.</span><span class="sxs-lookup"><span data-stu-id="1d249-164">Wait for up too20 minutes tooverify that hello account has been synchronized tooSalesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1d249-165">További források</span><span class="sxs-lookup"><span data-stu-id="1d249-165">Additional resources</span></span>

* [<span data-ttu-id="1d249-166">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="1d249-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1d249-167">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1d249-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="1d249-168">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d249-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforce-tutorial.md)