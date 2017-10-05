---
title: "Oktatóanyag: Azure Active Directoryval integrált Concur |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Concur között."
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
ms.openlocfilehash: cd35b6e2dc3171e9cffdb820bbc5b0d45ff58e07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="075d2-103">Oktatóanyag: A felhasználók átadása konfigurálása egyetért</span><span class="sxs-lookup"><span data-stu-id="075d2-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="075d2-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Concur és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Concur mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="075d2-104">The objective of this tutorial is to show you the steps you need to perform in Concur and Azure AD to automatically provision and de-provision user accounts from Azure AD to Concur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="075d2-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="075d2-105">Prerequisites</span></span>

<span data-ttu-id="075d2-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="075d2-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="075d2-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="075d2-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="075d2-108">Egy Concur egyszeri bejelentkezés engedélyezve van az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="075d2-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="075d2-109">Egy felhasználói fiókot az Concur Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="075d2-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-to-concur"></a><span data-ttu-id="075d2-110">Felhasználók hozzárendelése Concur</span><span class="sxs-lookup"><span data-stu-id="075d2-110">Assigning users to Concur</span></span>

<span data-ttu-id="075d2-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="075d2-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="075d2-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="075d2-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="075d2-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az Concur alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="075d2-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Concur app.</span></span> <span data-ttu-id="075d2-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az Concur alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="075d2-114">Once decided, you can assign these users to your Concur app by following the instructions here:</span></span>

[<span data-ttu-id="075d2-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="075d2-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a><span data-ttu-id="075d2-116">Felhasználók hozzárendelése Concur fontos tippek</span><span class="sxs-lookup"><span data-stu-id="075d2-116">Important tips for assigning users to Concur</span></span>

*   <span data-ttu-id="075d2-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó a kiépítési konfigurációjának tesztelése rendelendő Concur.</span><span class="sxs-lookup"><span data-stu-id="075d2-117">It is recommended that a single Azure AD user be assigned to Concur to test the provisioning configuration.</span></span> <span data-ttu-id="075d2-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="075d2-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="075d2-119">Amikor egy felhasználó hozzárendelése Concur, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="075d2-119">When assigning a user to Concur, you must select a valid user role.</span></span> <span data-ttu-id="075d2-120">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="075d2-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="075d2-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="075d2-121">Enable user provisioning</span></span>

<span data-ttu-id="075d2-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás Concur a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú Concur hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="075d2-122">This section guides you through connecting your Azure AD to Concur's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="075d2-123">Dönthet úgy is, SAML-alapú egyszeri bejelentkezést Concur engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="075d2-123">You may also choose to enabled SAML-based Single Sign-On for Concur, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="075d2-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="075d2-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="075d2-125">Konfigurálhatja a felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="075d2-125">To configure user account provisioning:</span></span>

<span data-ttu-id="075d2-126">Ez a szakasz célja helyzeteit vázolják fel, az Active Directory felhasználói fiókoknak az Concur kiépítés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="075d2-126">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Concur.</span></span>

<span data-ttu-id="075d2-127">Lehetővé teszi az alkalmazások kiadás szolgáltatási, nem rendelkezik megfelelő telepítő és a webes szolgáltatás-rendszergazdák profil használatát.</span><span class="sxs-lookup"><span data-stu-id="075d2-127">To enable apps in the Expense Service, there has to be proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="075d2-128">A WS-rendszergazdai szerepkör nem tölti fel a rendszergazda profil, amelyekkel a T & E felügyeleti funkcióihoz.</span><span class="sxs-lookup"><span data-stu-id="075d2-128">Don't add the WS Admin role to your existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="075d2-129">Tanácsadók egyetért vagy az ügyfél rendszergazdának létre kell hoznia egy különálló webes szolgáltatás-rendszergazda profil, és az ügyfél rendszergazda ezt a profilt kell használnia a webes szolgáltatások felügyeleti funkciók (például, amely lehetővé teszi alkalmazások).</span><span class="sxs-lookup"><span data-stu-id="075d2-129">Concur Consultants or the client administrator must create a distinct Web Service Administrator profile and the Client administrator must use this profile for the Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="075d2-130">Ezeket a profilokat kell különíteni az ügyfél rendszergazda napi T & E felügyeleti profil (a T & E felügyeleti profil nem szerepkörrel kell rendelkeznie a WSAdmin rendelt).</span><span class="sxs-lookup"><span data-stu-id="075d2-130">These profiles must be kept separate from the client administrator's daily T&E admin profile (the T&E admin profile should not have the WSAdmin role assigned).</span></span>

<span data-ttu-id="075d2-131">Az alkalmazás engedélyezésének használandó a profil létrehozásakor adja meg az ügyfél-rendszergazdai nevet a felhasználói profil mezőkben.</span><span class="sxs-lookup"><span data-stu-id="075d2-131">When you create the profile to be used for enabling the app, enter the client administrator's name into the user profile fields.</span></span> <span data-ttu-id="075d2-132">Ez a profil tulajdonjoga rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="075d2-132">This assigns ownership to the profile.</span></span> <span data-ttu-id="075d2-133">Egy vagy több profilok létrehozása után az ügyfél jelentkezzen be ezt a profilt kattintson a "*engedélyezése*" gombra a Web Services menü belül Partner alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="075d2-133">Once one or more profiles is created, the client must log in with this profile to click the "*Enable*" button for a Partner App within the Web Services menu.</span></span>

<span data-ttu-id="075d2-134">A következő okok miatt ez a művelet nem a normál T & E felügyeleti használ a profillal kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="075d2-134">For the following reasons, this action should not be done with the profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="075d2-135">Az ügyfél rendelkezik, amely kattint kell "*Igen*" a párbeszéd ablak akkor jelenik meg, egy alkalmazás engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="075d2-135">The client has to be the one that clicks "*Yes*" on the dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="075d2-136">Kattintson elfogadja, hogy az ügyfél nem kész a Partner alkalmazás számára az adatok elérését, így Ön vagy a Partner nem gombra, hogy Igen.</span><span class="sxs-lookup"><span data-stu-id="075d2-136">This click acknowledges the client is willing for the Partner application to access their data, so you or the Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="075d2-137">Ha egy ügyfél rendszergazda alkalmazás engedélyezve van a T & E rendszergazdai profil elhagyja a céget (ami azt eredményezi, a profil alatt inaktivált), engedélyezve van, hogy a profil nem működik addig, amíg az alkalmazás engedélyezve van egy másik aktív WS-felügyeleti profilt használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="075d2-137">If a client administrator that has enabled an app using the T&E admin profile leaves the company (resulting in the profile being inactivated), any apps enabled using that profile does not function until the app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="075d2-138">Ezért profilok létrehozása a különböző WS-felügyeleti kellene.</span><span class="sxs-lookup"><span data-stu-id="075d2-138">This is why you are supposed to create distinct WS Admin profiles.</span></span>

* <span data-ttu-id="075d2-139">Ha egy rendszergazda elhagyja a vállalatot, WS-felügyeleti profilt tartozó név módosíthatja a csere rendszergazdájának igény befolyásolása nélkül az engedélyezett alkalmazás, mert a profilt nem kell inaktivált.</span><span class="sxs-lookup"><span data-stu-id="075d2-139">If an administrator leaves the company, the name associated to the WS Admin profile can be changed to the replacement administrator if desired without impacting the enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="075d2-140">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="075d2-140">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="075d2-141">Jelentkezzen be a **Concur** bérlő.</span><span class="sxs-lookup"><span data-stu-id="075d2-141">Log on to your **Concur** tenant.</span></span>

2. <span data-ttu-id="075d2-142">Az a **felügyeleti** menü **webszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="075d2-142">From the **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="075d2-143">![Concur bérlői](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur bérlői")</span><span class="sxs-lookup"><span data-stu-id="075d2-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="075d2-144">A bal oldalon a a **webszolgáltatások** ablaktáblán válassza előbb **Partner alkalmazás engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="075d2-144">On the left side, from the **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="075d2-145">![Partner alkalmazás engedélyezése](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Partner alkalmazás engedélyezése")</span><span class="sxs-lookup"><span data-stu-id="075d2-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="075d2-146">Az a **alkalmazás engedélyezése** listáról válassza ki **Azure Active Directory**, és kattintson a **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="075d2-146">From the **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="075d2-147">![A Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directoryban")</span><span class="sxs-lookup"><span data-stu-id="075d2-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="075d2-148">Kattintson a **Igen** bezárásához a **a művelet megerősítéséhez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="075d2-148">Click **Yes** to close the **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="075d2-149">![Erősítse meg a műveletet](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "erősítse meg a műveletet")</span><span class="sxs-lookup"><span data-stu-id="075d2-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="075d2-150">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="075d2-150">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="075d2-151">Ha már konfigurált Concur egyszeri bejelentkezést, keresse meg a keresési mező Concur példányát.</span><span class="sxs-lookup"><span data-stu-id="075d2-151">If you have already configured Concur for single sign-on, search for your instance of Concur using the search field.</span></span> <span data-ttu-id="075d2-152">Máskülönben válassza **Hozzáadás** keresse meg a **Concur** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="075d2-152">Otherwise, select **Add** and search for **Concur** in the application gallery.</span></span> <span data-ttu-id="075d2-153">Válassza ki a Concur a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="075d2-153">Select Concur from the search results, and add it to your list of applications.</span></span>

8. <span data-ttu-id="075d2-154">Jelölje ki a Concur példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="075d2-154">Select your instance of Concur, then select the **Provisioning** tab.</span></span>

9. <span data-ttu-id="075d2-155">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="075d2-155">Set the **Provisioning Mode** to **Automatic**.</span></span> 
 
    ![Kiépítés](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="075d2-157">Az a **rendszergazdai hitelesítő adataival** területen adja meg a **felhasználónév** és a **jelszó** Concur rendszergazdától.</span><span class="sxs-lookup"><span data-stu-id="075d2-157">Under the **Admin Credentials** section, enter the **user name** and the **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="075d2-158">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az Concur alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="075d2-158">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Concur app.</span></span> <span data-ttu-id="075d2-159">Ha nem sikerül, győződjön meg arról, Concur fiókja Team rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="075d2-159">If the connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="075d2-160">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="075d2-160">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

13. <span data-ttu-id="075d2-161">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="075d2-161">Click **Save.**</span></span>

14. <span data-ttu-id="075d2-162">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat Concur.**</span><span class="sxs-lookup"><span data-stu-id="075d2-162">Under the Mappings section, select **Synchronize Azure Active Directory Users to Concur.**</span></span>

15. <span data-ttu-id="075d2-163">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD Concur lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="075d2-163">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Concur.</span></span> <span data-ttu-id="075d2-164">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Concur a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="075d2-164">The attributes selected as **Matching** properties are used to match the user accounts in Concur for update operations.</span></span> <span data-ttu-id="075d2-165">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="075d2-165">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="075d2-166">Az Azure AD szolgáltatás Concur kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="075d2-166">To enable the Azure AD provisioning service for Concur, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

17. <span data-ttu-id="075d2-167">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="075d2-167">Click **Save.**</span></span>

<span data-ttu-id="075d2-168">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="075d2-168">You can now create a test account.</span></span> <span data-ttu-id="075d2-169">Akár 20 percig várjon győződjön meg arról, hogy a fiók Concur lett-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="075d2-169">Wait for up to 20 minutes to verify that the account has been synchronized to Concur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="075d2-170">További források</span><span class="sxs-lookup"><span data-stu-id="075d2-170">Additional resources</span></span>

* [<span data-ttu-id="075d2-171">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="075d2-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="075d2-172">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="075d2-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="075d2-173">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="075d2-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

