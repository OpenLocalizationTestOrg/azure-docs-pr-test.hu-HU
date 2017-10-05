---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="656ce-103">Oktatóanyag: Citrix GoToMeeting konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="656ce-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="656ce-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Citrix GoToMeeting és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Citrix GoToMeeting mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="656ce-104">The objective of this tutorial is to show you the steps you need to perform in Citrix GoToMeeting and Azure AD to automatically provision and de-provision user accounts from Azure AD to Citrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="656ce-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="656ce-105">Prerequisites</span></span>

<span data-ttu-id="656ce-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="656ce-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="656ce-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="656ce-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="656ce-108">A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="656ce-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="656ce-109">Egy felhasználói fiókot a Citrix GoToMeeting Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="656ce-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="656ce-110">Felhasználók hozzárendelése Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="656ce-110">Assigning users to Citrix GoToMeeting</span></span>

<span data-ttu-id="656ce-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="656ce-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="656ce-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="656ce-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="656ce-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a Citrix GoToMeeting alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="656ce-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="656ce-114">Ha úgy döntött, itt utasításokat követve hozzárendelheti ezeket a felhasználókat a Citrix GoToMeeting app:</span><span class="sxs-lookup"><span data-stu-id="656ce-114">Once decided, you can assign these users to your Citrix GoToMeeting app by following the instructions here:</span></span>

[<span data-ttu-id="656ce-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="656ce-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="656ce-116">Felhasználók hozzárendelése Citrix GoToMeeting fontos tippek</span><span class="sxs-lookup"><span data-stu-id="656ce-116">Important tips for assigning users to Citrix GoToMeeting</span></span>

*   <span data-ttu-id="656ce-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve a Citrix GoToMeeting teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="656ce-117">It is recommended that a single Azure AD user is assigned to Citrix GoToMeeting to test the provisioning configuration.</span></span> <span data-ttu-id="656ce-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="656ce-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="656ce-119">Amikor egy felhasználó rendel a Citrix GoToMeeting, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="656ce-119">When assigning a user to Citrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="656ce-120">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="656ce-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="656ce-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="656ce-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="656ce-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás Citrix GoToMeeting felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Citrix GoToMeeting alapján a felhasználók és csoportok az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="656ce-122">This section guides you through connecting your Azure AD to Citrix GoToMeeting's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="656ce-123">Dönthet úgy is, engedélyezze SAML-alapú egyszeri bejelentkezést a Citrix GoToMeeting, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="656ce-123">You may also choose to enabled SAML-based Single Sign-On for Citrix GoToMeeting, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="656ce-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="656ce-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="656ce-125">Konfigurálása automatikus felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="656ce-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="656ce-126">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="656ce-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="656ce-127">Ha már konfigurált Citrix GoToMeeting egyszeri bejelentkezést, keresse meg a Citrix GoToMeeting használja a keresőmezőt példányát.</span><span class="sxs-lookup"><span data-stu-id="656ce-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using the search field.</span></span> <span data-ttu-id="656ce-128">Máskülönben válassza **Hozzáadás** keresse meg a **Citrix GoToMeeting** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="656ce-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in the application gallery.</span></span> <span data-ttu-id="656ce-129">Válassza ki a Citrix GoToMeeting a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="656ce-129">Select Citrix GoToMeeting from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="656ce-130">Jelölje ki a Citrix GoToMeeting példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="656ce-130">Select your instance of Citrix GoToMeeting, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="656ce-131">Állítsa be a **kiépítés** módot **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="656ce-131">Set the **Provisioning** Mode to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="656ce-133">A rendszergazdai hitelesítő adataival szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="656ce-133">Under the Admin Credentials section, perform the following steps:</span></span>
   
    <span data-ttu-id="656ce-134">a.</span><span class="sxs-lookup"><span data-stu-id="656ce-134">a.</span></span> <span data-ttu-id="656ce-135">Az a **Citrix GoToMeeting rendszergazda felhasználóneve** szövegmező, írja be a felhasználónevet, a rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="656ce-135">In the **Citrix GoToMeeting Admin User Name** textbox, type the user name of an administrator.</span></span>

    <span data-ttu-id="656ce-136">b.</span><span class="sxs-lookup"><span data-stu-id="656ce-136">b.</span></span> <span data-ttu-id="656ce-137">Az a **Citrix GoToMeeting rendszergazdai jelszó** szövegmező, a rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="656ce-137">In the **Citrix GoToMeeting Admin Password** textbox, the administrator's password.</span></span>

6. <span data-ttu-id="656ce-138">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a Citrix GoToMeeting alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="656ce-138">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="656ce-139">Ha nem sikerül, győződjön meg arról, Citrix GoToMeeting fiókja Team rendszergazdai jogosultságokkal rendelkezik, majd próbálja meg a **"Rendszergazdai hitelesítő adatok"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="656ce-139">If the connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try the **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="656ce-140">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="656ce-140">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="656ce-141">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="656ce-141">Click **Save.**</span></span>

9. <span data-ttu-id="656ce-142">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók a Citrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="656ce-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to Citrix GoToMeeting.**</span></span>

10. <span data-ttu-id="656ce-143">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumokat a Citrix GoToMeeting szinkronizált az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="656ce-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Citrix GoToMeeting.</span></span> <span data-ttu-id="656ce-144">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Citrix GoToMeeting a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="656ce-144">The attributes selected as **Matching** properties are used to match the user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="656ce-145">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="656ce-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="656ce-146">Az Azure AD szolgáltatás a Citrix GoToMeeting kiépítését engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="656ce-146">To enable the Azure AD provisioning service for Citrix GoToMeeting, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="656ce-147">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="656ce-147">Click **Save.**</span></span>

<span data-ttu-id="656ce-148">A kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Citrix GoToMeeting rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="656ce-148">It starts the initial synchronization of any users and/or groups assigned to Citrix GoToMeeting in the Users and Groups section.</span></span> <span data-ttu-id="656ce-149">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="656ce-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="656ce-150">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amely a Citrix GoToMeeting app a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="656ce-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="656ce-151">További források</span><span class="sxs-lookup"><span data-stu-id="656ce-151">Additional resources</span></span>

* [<span data-ttu-id="656ce-152">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="656ce-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="656ce-153">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="656ce-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="656ce-154">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="656ce-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


