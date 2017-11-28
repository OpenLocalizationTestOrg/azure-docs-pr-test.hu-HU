---
title: "Oktatóanyag: Azure Active Directoryval integrált DocuSign |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és DocuSign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 3b509ffa934949200277ae431761d2accd4a02d6
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a><span data-ttu-id="f9276-103">Oktatóanyag: A felhasználók átadása DocuSign konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f9276-103">Tutorial: Configuring DocuSign for User Provisioning</span></span>

<span data-ttu-id="f9276-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a DocuSign és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-DocuSign mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="f9276-104">The objective of this tutorial is to show you the steps you need to perform in DocuSign and Azure AD to automatically provision and de-provision user accounts from Azure AD to DocuSign.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9276-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f9276-105">Prerequisites</span></span>

<span data-ttu-id="f9276-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="f9276-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="f9276-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="f9276-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="f9276-108">Egy DocuSign egyszeri bejelentkezés engedélyezve van az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f9276-108">A DocuSign single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="f9276-109">Egy felhasználói fiókot az DocuSign Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="f9276-109">A user account in DocuSign with Team Admin permissions.</span></span>

## <a name="assigning-users-to-docusign"></a><span data-ttu-id="f9276-110">Felhasználók hozzárendelése DocuSign</span><span class="sxs-lookup"><span data-stu-id="f9276-110">Assigning users to DocuSign</span></span>

<span data-ttu-id="f9276-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="f9276-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="f9276-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok számára "rendelt" az Azure AD alkalmazás szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="f9276-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="f9276-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az DocuSign alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="f9276-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your DocuSign app.</span></span> <span data-ttu-id="f9276-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az DocuSign alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="f9276-114">Once decided, you can assign these users to your DocuSign app by following the instructions here:</span></span>

[<span data-ttu-id="f9276-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f9276-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a><span data-ttu-id="f9276-116">Felhasználók hozzárendelése DocuSign fontos tippek</span><span class="sxs-lookup"><span data-stu-id="f9276-116">Important tips for assigning users to DocuSign</span></span>

*   <span data-ttu-id="f9276-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve DocuSign teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="f9276-117">It is recommended that a single Azure AD user is assigned to DocuSign to test the provisioning configuration.</span></span> <span data-ttu-id="f9276-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="f9276-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="f9276-119">Amikor egy felhasználó hozzárendelése DocuSign, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="f9276-119">When assigning a user to DocuSign, you must select a valid user role.</span></span> <span data-ttu-id="f9276-120">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9276-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="f9276-121">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f9276-121">Enable User Provisioning</span></span>

<span data-ttu-id="f9276-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás DocuSign a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú DocuSign hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="f9276-122">This section guides you through connecting your Azure AD to DocuSign's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in DocuSign based on user and group assignment in Azure AD.</span></span>

> [!Tip]
> <span data-ttu-id="f9276-123">Dönthet úgy is, SAML-alapú egyszeri bejelentkezést DocuSign engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f9276-123">You may also choose to enabled SAML-based Single Sign-On for DocuSign, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f9276-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="f9276-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="f9276-125">Konfigurálhatja a felhasználói fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="f9276-125">To configure user account provisioning:</span></span>

<span data-ttu-id="f9276-126">Ez a szakasz célja engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókoknak az DocuSign felvázoló.</span><span class="sxs-lookup"><span data-stu-id="f9276-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to DocuSign.</span></span>

1. <span data-ttu-id="f9276-127">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f9276-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="f9276-128">Ha már konfigurált DocuSign egyszeri bejelentkezést, keresse meg a keresési mező DocuSign példányát.</span><span class="sxs-lookup"><span data-stu-id="f9276-128">If you have already configured DocuSign for single sign-on, search for your instance of DocuSign using the search field.</span></span> <span data-ttu-id="f9276-129">Máskülönben válassza **Hozzáadás** keresse meg a **DocuSign** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="f9276-129">Otherwise, select **Add** and search for **DocuSign** in the application gallery.</span></span> <span data-ttu-id="f9276-130">Válassza ki a DocuSign a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="f9276-130">Select DocuSign from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="f9276-131">Jelölje ki a DocuSign példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="f9276-131">Select your instance of DocuSign, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="f9276-132">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="f9276-132">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="f9276-134">Az a **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f9276-134">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="f9276-135">a.</span><span class="sxs-lookup"><span data-stu-id="f9276-135">a.</span></span> <span data-ttu-id="f9276-136">Az a **rendszergazda felhasználóneve** szövegmezőhöz egy DocuSign a fióknevet, amelynek típusa a **rendszergazda** DocuSign.com rendelt profillal.</span><span class="sxs-lookup"><span data-stu-id="f9276-136">In the **Admin User Name** textbox, type a DocuSign account name that has the **System Administrator** profile in DocuSign.com assigned.</span></span>
   
    <span data-ttu-id="f9276-137">b.</span><span class="sxs-lookup"><span data-stu-id="f9276-137">b.</span></span> <span data-ttu-id="f9276-138">Az a **rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="f9276-138">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="f9276-139">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az DocuSign alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f9276-139">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your DocuSign app.</span></span>

7. <span data-ttu-id="f9276-140">Az a **értesítő e-mailt** mezőbe írja be az e-mail cím vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="f9276-140">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

8. <span data-ttu-id="f9276-141">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="f9276-141">Click **Save.**</span></span>

9. <span data-ttu-id="f9276-142">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat DocuSign.**</span><span class="sxs-lookup"><span data-stu-id="f9276-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to DocuSign.**</span></span>

10. <span data-ttu-id="f9276-143">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD DocuSign lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="f9276-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to DocuSign.</span></span> <span data-ttu-id="f9276-144">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a DocuSign a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f9276-144">The attributes selected as **Matching** properties are used to match the user accounts in DocuSign for update operations.</span></span> <span data-ttu-id="f9276-145">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f9276-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="f9276-146">Az Azure AD szolgáltatás DocuSign kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="f9276-146">To enable the Azure AD provisioning service for DocuSign, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="f9276-147">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="f9276-147">Click **Save.**</span></span>

<span data-ttu-id="f9276-148">A kezdeti szinkronizálás bármely felhasználói és/vagy a felhasználók és csoportok szakaszban DocuSign rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f9276-148">It starts the initial synchronization of any users and/or groups assigned to DocuSign in the Users and Groups section.</span></span> <span data-ttu-id="f9276-149">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="f9276-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="f9276-150">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához tevékenység jelentéseit, amelyek a létesítési szolgáltatás az DocuSign alkalmazás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="f9276-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your DocuSign app.</span></span>

<span data-ttu-id="f9276-151">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="f9276-151">You can now create a test account.</span></span> <span data-ttu-id="f9276-152">Akár 20 percig várjon győződjön meg arról, hogy a fiók DocuSign lett-e szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="f9276-152">Wait for up to 20 minutes to verify that the account has been synchronized to DocuSign.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9276-153">További források</span><span class="sxs-lookup"><span data-stu-id="f9276-153">Additional resources</span></span>

* [<span data-ttu-id="f9276-154">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="f9276-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f9276-155">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f9276-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f9276-156">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f9276-156">Configure Single Sign-on</span></span>](active-directory-saas-docusign-tutorial.md)