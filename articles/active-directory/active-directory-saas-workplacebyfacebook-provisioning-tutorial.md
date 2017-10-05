---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9b22679c304248ed7ba7a6bd9eaf82b64f7143cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="73b0f-103">Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="73b0f-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="73b0f-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Facebook-on és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad által Facebook munkahelyi munkaterületen mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="73b0f-104">The objective of this tutorial is to show you the steps you need to perform in Workplace by Facebook and Azure AD to automatically provision and de-provision user accounts from Azure AD to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73b0f-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73b0f-105">Prerequisites</span></span>

<span data-ttu-id="73b0f-106">Konfigurálása az Azure AD-integrációs munkahelyi által Facebook, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="73b0f-106">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="73b0f-107">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="73b0f-107">An Azure AD subscription</span></span>
- <span data-ttu-id="73b0f-108">A munkahelyi által Facebook egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="73b0f-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="73b0f-109">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="73b0f-109">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="73b0f-110">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="73b0f-110">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="73b0f-111">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="73b0f-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="73b0f-112">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73b0f-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="73b0f-113">Felhasználók hozzárendelése a munkahely által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="73b0f-113">Assigning users to Workplace by Facebook</span></span>

<span data-ttu-id="73b0f-114">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="73b0f-114">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="73b0f-115">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="73b0f-115">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="73b0f-116">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználókat, akik a Facebook-alkalmazást a munkahelyi hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="73b0f-116">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="73b0f-117">Ha úgy döntött, rendelhet ezeket a felhasználókat a munkahelyi Facebook alkalmazást itt cikk utasításait követve:</span><span class="sxs-lookup"><span data-stu-id="73b0f-117">Once decided, you can assign these users to your Workplace by Facebook app by following the instructions here:</span></span>

[<span data-ttu-id="73b0f-118">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="73b0f-118">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="73b0f-119">Felhasználók hozzárendelése a munkahely által Facebook fontos tippek</span><span class="sxs-lookup"><span data-stu-id="73b0f-119">Important tips for assigning users to Workplace by Facebook</span></span>

*   <span data-ttu-id="73b0f-120">Javasoljuk, hogy egyetlen Azure AD-felhasználó a munkahelyhez által hozzárendelt Facebook teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="73b0f-120">It is recommended that a single Azure AD user is assigned to Workplace by Facebook to test the provisioning configuration.</span></span> <span data-ttu-id="73b0f-121">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="73b0f-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="73b0f-122">A felhasználó által Facebook munkahelyi hozzárendelésekor ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="73b0f-122">When assigning a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="73b0f-123">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="73b0f-123">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="73b0f-124">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="73b0f-124">Enable User Provisioning</span></span>

<span data-ttu-id="73b0f-125">Ez a szakasz végigvezeti az Azure AD munkahelyi csatlakozás Facebook a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a munkahelyi Facebook alapján a felhasználók és csoportok hozzárendelése az Azure AD által hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="73b0f-125">This section guides you through connecting your Azure AD to Workplace by Facebook's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="73b0f-126">Is választhatja, hogy engedélyezze SAML-alapú egyszeri bejelentkezést a munkahely által Facebook, a következő utasításokat megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="73b0f-126">You may also choose to enabled SAML-based Single Sign-On for Workplace by Facebook, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="73b0f-127">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="73b0f-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="73b0f-128">Konfigurálhatja a felhasználói fiók kiépítése munkahelyi Facebook által az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="73b0f-128">To configure user account provisioning to Workplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="73b0f-129">Ez a szakasz célja felvázoló Active Directory felhasználói fiókok által Facebook munkahelyi kiépítés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="73b0f-129">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Workplace by Facebook.</span></span>

<span data-ttu-id="73b0f-130">Az Azure AD képes való automatikus szinkronizálása a munkahelyi Facebook által hozzárendelt felhasználói fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="73b0f-130">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="73b0f-131">Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi hozzáférés előtt megpróbálja az első alkalommal jelentkeznek be a felhasználók hitelesítéséhez szükséges adatok megszerzéséhez Facebook által.</span><span class="sxs-lookup"><span data-stu-id="73b0f-131">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="73b0f-132">Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.</span><span class="sxs-lookup"><span data-stu-id="73b0f-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="73b0f-133">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="73b0f-133">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="73b0f-134">Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által használja a keresőmezőt Facebook példányát.</span><span class="sxs-lookup"><span data-stu-id="73b0f-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using the search field.</span></span> <span data-ttu-id="73b0f-135">Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="73b0f-135">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="73b0f-136">Válassza ki a munkahelyi Facebook által a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="73b0f-136">Select Workplace by Facebook from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="73b0f-137">Válassza ki a munkahely által Facebook példányt, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="73b0f-137">Select your instance of Workplace by Facebook, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="73b0f-138">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="73b0f-138">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="73b0f-140">Az a **rendszergazdai hitelesítő adataival** területen adja meg a titkos kulcs Token és a bérlői URL-cím a munkahelyének Facebook-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="73b0f-140">Under the **Admin Credentials** section, enter the Secret Token and the Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="73b0f-141">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a munkahelyi Facebook-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="73b0f-141">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="73b0f-142">Ha nem sikerül, győződjön meg arról, a Facebook-fiókkal munkahelyi Team rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="73b0f-142">If the connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="73b0f-143">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="73b0f-143">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="73b0f-144">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="73b0f-144">Click **Save.**</span></span>

9. <span data-ttu-id="73b0f-145">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók által Facebook munkahelyhez.**</span><span class="sxs-lookup"><span data-stu-id="73b0f-145">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook.**</span></span>

10. <span data-ttu-id="73b0f-146">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok az Azure AD munkahelyi által szinkronizált Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="73b0f-146">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="73b0f-147">A kiválasztott attribútumok **egyező** tulajdonságok kell egyeznie a felhasználói fiókok, a munkahely által használt Facebook a frissítési műveletek.</span><span class="sxs-lookup"><span data-stu-id="73b0f-147">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="73b0f-148">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="73b0f-148">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="73b0f-149">Az Azure AD szolgáltatás által Facebook munkahelyi fiókhoz kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="73b0f-149">To enable the Azure AD provisioning service for Workplace by Facebook, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="73b0f-150">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="73b0f-150">Click **Save.**</span></span>

<span data-ttu-id="73b0f-151">Automatikus kiépítés konfigurálásával kapcsolatos további információkért lásd: [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="73b0f-151">For more information on how to configure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="73b0f-152">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="73b0f-152">You can now create a test account.</span></span> <span data-ttu-id="73b0f-153">Várjon 20 percig győződjön meg arról, hogy a munkahelyi fiók lett szinkronizálva a Facebook által.</span><span class="sxs-lookup"><span data-stu-id="73b0f-153">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73b0f-154">További források</span><span class="sxs-lookup"><span data-stu-id="73b0f-154">Additional resources</span></span>

* [<span data-ttu-id="73b0f-155">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="73b0f-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="73b0f-156">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="73b0f-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="73b0f-157">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="73b0f-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

