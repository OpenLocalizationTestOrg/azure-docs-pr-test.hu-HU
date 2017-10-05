---
title: "Oktatóanyag: Azure Active Directoryval integrált Netsuite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Netsuite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 277c393536615fc8bfe8af0bc6d487115f04776c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a><span data-ttu-id="06596-103">Oktatóanyag: Netsuite konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="06596-103">Tutorial: Configuring Netsuite for Automatic User Provisioning</span></span>

<span data-ttu-id="06596-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Netsuite és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Netsuite mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="06596-104">The objective of this tutorial is to show you the steps you need to perform in Netsuite and Azure AD to automatically provision and de-provision user accounts from Azure AD to Netsuite.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06596-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06596-105">Prerequisites</span></span>

<span data-ttu-id="06596-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="06596-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="06596-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="06596-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="06596-108">Egy Netsuite egyszeri bejelentkezés engedélyezve van az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="06596-108">A Netsuite single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="06596-109">Egy felhasználói fiókot az Netsuite Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="06596-109">A user account in Netsuite with Team Admin permissions.</span></span>

## <a name="assigning-users-to-netsuite"></a><span data-ttu-id="06596-110">Felhasználók hozzárendelése Netsuite</span><span class="sxs-lookup"><span data-stu-id="06596-110">Assigning users to Netsuite</span></span>

<span data-ttu-id="06596-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="06596-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="06596-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok számára "rendelt" az Azure AD alkalmazás szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="06596-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="06596-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az Netsuite alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="06596-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Netsuite app.</span></span> <span data-ttu-id="06596-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az Netsuite alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="06596-114">Once decided, you can assign these users to your Netsuite app by following the instructions here:</span></span>

[<span data-ttu-id="06596-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="06596-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a><span data-ttu-id="06596-116">Felhasználók hozzárendelése Netsuite fontos tippek</span><span class="sxs-lookup"><span data-stu-id="06596-116">Important tips for assigning users to Netsuite</span></span>

*   <span data-ttu-id="06596-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve Netsuite teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="06596-117">It is recommended that a single Azure AD user is assigned to Netsuite to test the provisioning configuration.</span></span> <span data-ttu-id="06596-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="06596-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="06596-119">Amikor egy felhasználó hozzárendelése Netsuite, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="06596-119">When assigning a user to Netsuite, you must select a valid user role.</span></span> <span data-ttu-id="06596-120">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="06596-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="06596-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="06596-121">Enable User Provisioning</span></span>

<span data-ttu-id="06596-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás Netsuite a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú Netsuite hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="06596-122">This section guides you through connecting your Azure AD to Netsuite's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Netsuite based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="06596-123">Dönthet úgy is, SAML-alapú egyszeri bejelentkezést Netsuite engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06596-123">You may also choose to enabled SAML-based Single Sign-On for Netsuite, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="06596-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="06596-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="06596-125">Konfigurálhatja a felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="06596-125">To configure user account provisioning:</span></span>

<span data-ttu-id="06596-126">Ez a szakasz célja engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókoknak az Netsuite felvázoló.</span><span class="sxs-lookup"><span data-stu-id="06596-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Netsuite.</span></span>

1. <span data-ttu-id="06596-127">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="06596-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="06596-128">Ha már konfigurált Netsuite egyszeri bejelentkezést, keresse meg a keresési mező Netsuite példányát.</span><span class="sxs-lookup"><span data-stu-id="06596-128">If you have already configured Netsuite for single sign-on, search for your instance of Netsuite using the search field.</span></span> <span data-ttu-id="06596-129">Máskülönben válassza **Hozzáadás** keresse meg a **Netsuite** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="06596-129">Otherwise, select **Add** and search for **Netsuite** in the application gallery.</span></span> <span data-ttu-id="06596-130">Válassza ki a Netsuite a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="06596-130">Select Netsuite from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="06596-131">Jelölje ki a Netsuite példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="06596-131">Select your instance of Netsuite, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="06596-132">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="06596-132">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="06596-134">Az a **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="06596-134">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="06596-135">a.</span><span class="sxs-lookup"><span data-stu-id="06596-135">a.</span></span> <span data-ttu-id="06596-136">Az a **rendszergazda felhasználóneve** szövegmezőhöz egy Netsuite a fióknevet, amelynek típusa a **rendszergazda** Netsuite.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="06596-136">In the **Admin User Name** textbox, type a Netsuite account name that has the **System Administrator** profile in Netsuite.com assigned.</span></span>
   
    <span data-ttu-id="06596-137">b.</span><span class="sxs-lookup"><span data-stu-id="06596-137">b.</span></span> <span data-ttu-id="06596-138">Az a **rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="06596-138">In the **Admin Password** textbox, type the password for this account.</span></span>
      
6. <span data-ttu-id="06596-139">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az Netsuite alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="06596-139">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Netsuite app.</span></span>

7. <span data-ttu-id="06596-140">Az a **értesítő e-mailt** mezőbe írja be az e-mail cím vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="06596-140">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

8. <span data-ttu-id="06596-141">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="06596-141">Click **Save.**</span></span>

9. <span data-ttu-id="06596-142">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat Netsuite.**</span><span class="sxs-lookup"><span data-stu-id="06596-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to Netsuite.**</span></span>

10. <span data-ttu-id="06596-143">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD Netsuite lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="06596-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Netsuite.</span></span> <span data-ttu-id="06596-144">Vegye figyelembe, hogy az attribútumok választotta **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Netsuite a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="06596-144">Note that the attributes selected as **Matching** properties are used to match the user accounts in Netsuite for update operations.</span></span> <span data-ttu-id="06596-145">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="06596-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="06596-146">Az Azure AD szolgáltatás Netsuite kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="06596-146">To enable the Azure AD provisioning service for Netsuite, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="06596-147">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="06596-147">Click **Save.**</span></span>

<span data-ttu-id="06596-148">A kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban Netsuite rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="06596-148">It starts the initial synchronization of any users and/or groups assigned to Netsuite in the Users and Groups section.</span></span> <span data-ttu-id="06596-149">Figyelje meg, hogy a kezdeti szinkronizálás végrehajtásához bekövetkező körülbelül 20 percenként, mindaddig, amíg a szolgáltatás fut. ezt követő szinkronizálások hosszabb időbe telik.</span><span class="sxs-lookup"><span data-stu-id="06596-149">Note that the initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="06596-150">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az Netsuite alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="06596-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Netsuite app.</span></span>

<span data-ttu-id="06596-151">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="06596-151">You can now create a test account.</span></span> <span data-ttu-id="06596-152">Akár 20 percig várjon győződjön meg arról, hogy a fiók Netsuite lett-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="06596-152">Wait for up to 20 minutes to verify that the account has been synchronized to Netsuite.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="06596-153">További források</span><span class="sxs-lookup"><span data-stu-id="06596-153">Additional resources</span></span>

* [<span data-ttu-id="06596-154">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="06596-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="06596-155">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="06596-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="06596-156">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="06596-156">Configure Single Sign-on</span></span>](active-directory-saas-netsuite-tutorial.md)