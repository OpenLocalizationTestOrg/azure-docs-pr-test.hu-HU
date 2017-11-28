---
title: "Oktatóanyag: Azure Active Directoryval integrált Concur |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Concur között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="8f8c1-103">Oktatóanyag: A felhasználók átadása konfigurálása egyetért</span><span class="sxs-lookup"><span data-stu-id="8f8c1-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="8f8c1-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform Concur és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooConcur a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Concur and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooConcur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f8c1-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f8c1-105">Prerequisites</span></span>

<span data-ttu-id="8f8c1-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8f8c1-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="8f8c1-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="8f8c1-108">Egy Concur egyszeri bejelentkezés engedélyezve van az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="8f8c1-109">Egy felhasználói fiókot az Concur Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-tooconcur"></a><span data-ttu-id="8f8c1-110">Felhasználók tooConcur hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8f8c1-110">Assigning users tooConcur</span></span>

<span data-ttu-id="8f8c1-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="8f8c1-112">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="8f8c1-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Concur alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Concur app.</span></span> <span data-ttu-id="8f8c1-114">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Concur app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="8f8c1-114">Once decided, you can assign these users tooyour Concur app by following hello instructions here:</span></span>

[<span data-ttu-id="8f8c1-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8f8c1-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a><span data-ttu-id="8f8c1-116">Fontos tippek a felhasználók tooConcur hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8f8c1-116">Important tips for assigning users tooConcur</span></span>

*   <span data-ttu-id="8f8c1-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooConcur tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-117">It is recommended that a single Azure AD user be assigned tooConcur tootest hello provisioning configuration.</span></span> <span data-ttu-id="8f8c1-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="8f8c1-119">Amikor egy felhasználó tooConcur rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-119">When assigning a user tooConcur, you must select a valid user role.</span></span> <span data-ttu-id="8f8c1-120">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="8f8c1-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8f8c1-121">Enable user provisioning</span></span>

<span data-ttu-id="8f8c1-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooConcur felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Concur alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-122">This section guides you through connecting your Azure AD tooConcur's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="8f8c1-123">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Concur hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8f8c1-123">You may also choose tooenabled SAML-based Single Sign-On for Concur, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8f8c1-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="8f8c1-125">tooconfigure felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="8f8c1-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="8f8c1-126">hello ebben a szakaszban célja toooutline hogyan tooConcur tooenable kiépítése Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-126">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooConcur.</span></span>

<span data-ttu-id="8f8c1-127">tooenable alkalmazások hello költség szolgáltatás, nem rendelkezik a toobe megfelelő telepítő és a webes szolgáltatás-rendszergazdák profil használatát.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-127">tooenable apps in hello Expense Service, there has toobe proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="8f8c1-128">Ne adjon hello WS rendszergazdai szerepkör tooyour meglévő rendszergazda profillal, amelyekkel a T & E felügyeleti funkcióihoz.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-128">Don't add hello WS Admin role tooyour existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="8f8c1-129">Tanácsadók egyetért vagy hello ügyfél rendszergazdának létre kell hoznia egy különálló webes szolgáltatás-rendszergazda profil és hello ügyfél rendszergazda ezt a profilt kell használnia hello Web Services rendszergazdai feladatokat (például, amely lehetővé teszi alkalmazások).</span><span class="sxs-lookup"><span data-stu-id="8f8c1-129">Concur Consultants or hello client administrator must create a distinct Web Service Administrator profile and hello Client administrator must use this profile for hello Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="8f8c1-130">Ezeket a profilokat el kell különíteni a hello ügyfél rendszergazda napi T & E felügyeleti profil (hello T & E felügyeleti profil nem lehetnek hello WSAdmin szerepkörrel).</span><span class="sxs-lookup"><span data-stu-id="8f8c1-130">These profiles must be kept separate from hello client administrator's daily T&E admin profile (hello T&E admin profile should not have hello WSAdmin role assigned).</span></span>

<span data-ttu-id="8f8c1-131">Hello-profil toobe hello alkalmazás engedélyezésének használt létrehozásakor adja meg a hello ügyfél rendszergazda nevét hello felhasználói profil mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-131">When you create hello profile toobe used for enabling hello app, enter hello client administrator's name into hello user profile fields.</span></span> <span data-ttu-id="8f8c1-132">Ez a tulajdonosi toohello profil rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-132">This assigns ownership toohello profile.</span></span> <span data-ttu-id="8f8c1-133">Egy vagy több profilok létrehozása után hello ügyfél jelentkezzen be a profil tooclick hello "*engedélyezése*" gombra egy Partner alkalmazás hello webszolgáltatások menü belül.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-133">Once one or more profiles is created, hello client must log in with this profile tooclick hello "*Enable*" button for a Partner App within hello Web Services menu.</span></span>

<span data-ttu-id="8f8c1-134">A következő okok miatt hello Ez a művelet nem használ a normál T & E felügyeleti hello-profillal kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-134">For hello following reasons, this action should not be done with hello profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="8f8c1-135">hello ügyfél rendelkezik egy, a kattintásokról hello toobe "*Igen*" hello párbeszéd ablak akkor jelenik meg, egy alkalmazás engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-135">hello client has toobe hello one that clicks "*Yes*" on hello dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="8f8c1-136">Kattintson a hello ügyfél van hajlandó hello Partner alkalmazás tooaccess az adataikat, akkor vagy hello Partner nem kattintson, amely az Igen gombra, elfogadja.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-136">This click acknowledges hello client is willing for hello Partner application tooaccess their data, so you or hello Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="8f8c1-137">Ha egy ügyfél hello T & E felügyeleti profilt használó alkalmazások engedélyező kilép hello vállalati (ami alatt inaktivált hello-profil), a profilt használó engedélyezett alkalmazások nem működik hello alkalmazás egy másik aktív WS Admin engedélyezéséig profil.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-137">If a client administrator that has enabled an app using hello T&E admin profile leaves hello company (resulting in hello profile being inactivated), any apps enabled using that profile does not function until hello app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="8f8c1-138">Ezért toocreate különböző WS-felügyeleti profilok kellett volna lennie.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-138">This is why you are supposed toocreate distinct WS Admin profiles.</span></span>

* <span data-ttu-id="8f8c1-139">Ha egy rendszergazda hello vállalati kilép, hello neve társított WS-felügyeleti profil lehet módosított toohello helyettesítő rendszergazda, ha engedélyezett hello befolyásolása nélkül kívánt alkalmazást, mert a profilt nem kell inaktivált toohello.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-139">If an administrator leaves hello company, hello name associated toohello WS Admin profile can be changed toohello replacement administrator if desired without impacting hello enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="8f8c1-140">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8f8c1-140">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f8c1-141">Jelentkezzen be tooyour **Concur** bérlő.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-141">Log on tooyour **Concur** tenant.</span></span>

2. <span data-ttu-id="8f8c1-142">A hello **felügyeleti** menü **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-142">From hello **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="8f8c1-143">![Concur bérlői](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur bérlői")</span><span class="sxs-lookup"><span data-stu-id="8f8c1-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="8f8c1-144">A bal oldalon, a hello hello **webszolgáltatások** ablaktáblán válassza előbb **Partner alkalmazás engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-144">On hello left side, from hello **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="8f8c1-145">![Partner alkalmazás engedélyezése](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Partner alkalmazás engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="8f8c1-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="8f8c1-146">A hello **alkalmazás engedélyezése** listáról válassza ki **Azure Active Directory**, és kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-146">From hello **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="8f8c1-147">![A Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directoryban")</span><span class="sxs-lookup"><span data-stu-id="8f8c1-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="8f8c1-148">Kattintson a **Igen** tooclose hello **a művelet megerősítéséhez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-148">Click **Yes** tooclose hello **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="8f8c1-149">![Erősítse meg a műveletet](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "erősítse meg a műveletet")</span><span class="sxs-lookup"><span data-stu-id="8f8c1-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="8f8c1-150">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-150">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="8f8c1-151">Ha már konfigurált Concur egyszeri bejelentkezést, keresse meg a hello keresési mező Concur példányát.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-151">If you have already configured Concur for single sign-on, search for your instance of Concur using hello search field.</span></span> <span data-ttu-id="8f8c1-152">Máskülönben válassza **Hozzáadás** keresse meg a **Concur** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-152">Otherwise, select **Add** and search for **Concur** in hello application gallery.</span></span> <span data-ttu-id="8f8c1-153">Válassza ki a Concur hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-153">Select Concur from hello search results, and add it tooyour list of applications.</span></span>

8. <span data-ttu-id="8f8c1-154">Jelölje ki a Concur példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-154">Select your instance of Concur, then select hello **Provisioning** tab.</span></span>

9. <span data-ttu-id="8f8c1-155">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-155">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
 
    ![Kiépítés](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="8f8c1-157">A hello **rendszergazdai hitelesítő adataival** területen adja meg a hello **felhasználónév** és hello **jelszó** Concur rendszergazdától.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-157">Under hello **Admin Credentials** section, enter hello **user name** and hello **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="8f8c1-158">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Concur alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-158">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Concur app.</span></span> <span data-ttu-id="8f8c1-159">Ha hello létesített kapcsolat megszakad, győződjön meg arról, Concur fiókja Team rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-159">If hello connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="8f8c1-160">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-160">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

13. <span data-ttu-id="8f8c1-161">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="8f8c1-161">Click **Save.**</span></span>

14. <span data-ttu-id="8f8c1-162">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooConcur.**</span><span class="sxs-lookup"><span data-stu-id="8f8c1-162">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooConcur.**</span></span>

15. <span data-ttu-id="8f8c1-163">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooConcur szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-163">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooConcur.</span></span> <span data-ttu-id="8f8c1-164">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Concur a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Concur for update operations.</span></span> <span data-ttu-id="8f8c1-165">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-165">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="8f8c1-166">tooenable hello Azure AD Concur, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="8f8c1-166">tooenable hello Azure AD provisioning service for Concur, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

17. <span data-ttu-id="8f8c1-167">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="8f8c1-167">Click **Save.**</span></span>

<span data-ttu-id="8f8c1-168">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-168">You can now create a test account.</span></span> <span data-ttu-id="8f8c1-169">Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooConcur too20 perc.</span><span class="sxs-lookup"><span data-stu-id="8f8c1-169">Wait for up too20 minutes tooverify that hello account has been synchronized tooConcur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f8c1-170">További források</span><span class="sxs-lookup"><span data-stu-id="8f8c1-170">Additional resources</span></span>

* [<span data-ttu-id="8f8c1-171">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="8f8c1-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f8c1-172">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8f8c1-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="8f8c1-173">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f8c1-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

