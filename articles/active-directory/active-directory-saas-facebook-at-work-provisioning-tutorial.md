---
title: "Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása |} Microsoft Docs"
description: "Megtudhatja, hogyan automatikusan ellátásához, majd leépíti a felhasználói fiókok Azure ad-munkahelyi Facebook által."
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
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: a347eedbf5511dc83e1bc7721667441cfb87cb59
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="c9cc9-103">Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9cc9-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="c9cc9-104">Ez az oktatóanyag a lépéseket az Azure Active Directory (Azure AD) a munkahelyhez Facebook által automatikusan rendelkezni és deaktiválás rendelkezés felhasználói fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-104">This tutorial shows you the steps necessary to automatically provision and de-provision user accounts from Azure Active Directory (Azure AD) to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9cc9-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c9cc9-105">Prerequisites</span></span>

<span data-ttu-id="c9cc9-106">Konfigurálása az Azure AD-integrációs munkahelyi által Facebook, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c9cc9-106">To configure Azure AD integration with Workplace by Facebook, you need the following:</span></span>

- <span data-ttu-id="c9cc9-107">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c9cc9-107">An Azure AD subscription</span></span>
- <span data-ttu-id="c9cc9-108">A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="c9cc9-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="c9cc9-109">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="c9cc9-109">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="c9cc9-110">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9cc9-111">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9cc9-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-to-workplace-by-facebook"></a><span data-ttu-id="c9cc9-112">Felhasználók hozzárendelése munkahelyi által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="c9cc9-112">Assign users to Workplace by Facebook</span></span>

<span data-ttu-id="c9cc9-113">Az Azure AD egy fogalom, más néven "hozzárendeléseket" alapján határozza meg, mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-113">Azure AD uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="c9cc9-114">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és a csoportokat, amelyek számára az Azure AD alkalmazás szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-114">In the context of automatic user account provisioning, only the users and groups that have been assigned to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="c9cc9-115">A létesítési szolgáltatás engedélyezése és konfigurálása, előtt döntse el, mely felhasználók és csoportok az Azure AD határoz meg a felhasználókat, akik a Facebook-alkalmazást a munkahelyi hozzáférés szükséges.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-115">Before configuring and enabling the provisioning service, decide what users and groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="c9cc9-116">Majd rendelhet ezeket a felhasználókat a munkahelyi Facebook-alkalmazást a utasításait követve [egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c9cc9-116">You can then assign these users to your Workplace by Facebook app by following the instructions in [Assign a user or group to an enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="c9cc9-117">A létesítési konfiguráció tesztelése egyetlen hozzárendelésével által Facebook munkahelyi Azure AD-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-117">Test the provisioning configuration by assigning a single Azure AD user to Workplace by Facebook.</span></span> <span data-ttu-id="c9cc9-118">További felhasználók és csoportok hozzárendelése a később.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="c9cc9-119">A felhasználó által Facebook munkahelyi rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-119">When you assign a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="c9cc9-120">Az alapértelmezett szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-120">The Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="c9cc9-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c9cc9-121">Enable automated user provisioning</span></span>

<span data-ttu-id="c9cc9-122">Ez a szakasz végigvezeti az Azure AD csatlakozik a felhasználói fiók kiépítése munkahelyi API által a Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-122">This section guides you through connecting your Azure AD to the user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="c9cc9-123">Azt is megtudhatja, hogyan a létesítési szolgáltatás létrehozása, frissítése és tiltsa le a munkahelyi Facebook által hozzárendelt felhasználói fiókjainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-123">You also learn how to configure the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="c9cc9-124">Ez a felhasználó és az Azure AD-csoport-hozzárendelés alapul.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="c9cc9-125">Másik lehetőségként engedélyezze SAML-alapú egyszeri bejelentkezés a munkahelyi által Facebook, az a megjelenő utasításokat követve a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c9cc9-125">You can also choose to enabled SAML-based SSO for Workplace by Facebook, by following the instructions provided in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c9cc9-126">Egyszeri bejelentkezés automatikus kiépítés, függetlenül konfigurálhatók, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="c9cc9-127">Az Azure AD által Facebook munkahelyi kiépítés felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="c9cc9-127">Configure user account provisioning to Workplace by Facebook in Azure AD</span></span>

<span data-ttu-id="c9cc9-128">Az Azure AD képes való automatikus szinkronizálása a munkahelyi Facebook által hozzárendelt felhasználói fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-128">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="c9cc9-129">Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi hozzáférés előtt megpróbálja az első alkalommal jelentkeznek be a felhasználók hitelesítéséhez szükséges adatok megszerzéséhez Facebook által.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-129">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="c9cc9-130">Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="c9cc9-131">Az a [Azure-portálon](https://portal.azure.com), jelölje be **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-131">In the [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="c9cc9-132">Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által Facebook példányát keresési mező használatával.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using the search field.</span></span> <span data-ttu-id="c9cc9-133">Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-133">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="c9cc9-134">Válassza ki **által Facebook munkahelyi** a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-134">Select **Workplace by Facebook** from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="c9cc9-135">A munkahely által Facebook példányát, majd válassza ki és a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-135">Select your instance of Workplace by Facebook, and then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="c9cc9-136">Állítsa be **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-136">Set **Provisioning Mode** to **Automatic**.</span></span> 

    ![Képernyőfelvétel a munkahelyi által Facebook üzembe helyezési lehetőségek](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="c9cc9-138">A a **rendszergazdai hitelesítő adataival** területen adja meg a **titkos Token** és a **bérlői URL-cím** a munkahelyének Facebook-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-138">Under the **Admin Credentials** section, enter the **Secret Token** and the **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="c9cc9-139">Válassza ki az Azure-portálon **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat a munkahelyi Facebook-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-139">In the Azure portal, select **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="c9cc9-140">Ha nem sikerül, győződjön meg arról, hogy a munkahelyi Facebook-fiókkal Team rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-140">If the connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="c9cc9-141">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-141">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the check box.</span></span>

8. <span data-ttu-id="c9cc9-142">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-142">Select **Save**.</span></span>

9. <span data-ttu-id="c9cc9-143">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-143">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook**.</span></span>

10. <span data-ttu-id="c9cc9-144">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok az Azure AD munkahelyi által szinkronizált Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-144">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="c9cc9-145">A kiválasztott attribútumok **egyező** tulajdonságok kell egyeznie a felhasználói fiókok, a munkahely által használt Facebook a frissítési műveletek.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-145">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="c9cc9-146">Véglegesítse a módosításokat, jelölje be **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-146">To commit any changes, select **Save**.</span></span>

11. <span data-ttu-id="c9cc9-147">Ahhoz, hogy a munkahely által Facebook, a szolgáltatás kiépítését az Azure AD a **beállítások** területen módosítsa a **kiépítési állapot** való **a**.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-147">To enable the Azure AD provisioning service for Workplace by Facebook, in the **Settings** section, change the **Provisioning Status** to **On**.</span></span>

12. <span data-ttu-id="c9cc9-148">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-148">Select **Save**.</span></span>

<span data-ttu-id="c9cc9-149">Automatikus kiépítés konfigurálásával kapcsolatos további információkért lásd: [a Facebook-dokumentáció](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="c9cc9-149">For more information on how to configure automatic provisioning, see [the Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="c9cc9-150">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-150">You can now create a test account.</span></span> <span data-ttu-id="c9cc9-151">Várjon 20 percig győződjön meg arról, hogy a munkahelyi fiók lett szinkronizálva a Facebook által.</span><span class="sxs-lookup"><span data-stu-id="c9cc9-151">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9cc9-152">További források</span><span class="sxs-lookup"><span data-stu-id="c9cc9-152">Additional resources</span></span>

* [<span data-ttu-id="c9cc9-153">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="c9cc9-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9cc9-154">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c9cc9-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c9cc9-155">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9cc9-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

