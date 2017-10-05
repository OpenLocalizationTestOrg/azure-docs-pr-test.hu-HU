---
title: "Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan automatikusan ellátásához, majd leépíti a felhasználói fiókok Azure ad-Google Apps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: b061f0ddad70be4a5ca48d48d1a737d6af8afa8d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a><span data-ttu-id="ad4f9-103">Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása</span><span class="sxs-lookup"><span data-stu-id="ad4f9-103">Tutorial: Configuring Google Apps for automatic user provisioning</span></span>

<span data-ttu-id="ad4f9-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Google Apps és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Google Apps mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-104">The objective of this tutorial is to show you the steps you need to perform in Google Apps and Azure AD to automatically provision and de-provision user accounts from Azure AD to Google Apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad4f9-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad4f9-105">Prerequisites</span></span>

<span data-ttu-id="ad4f9-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="ad4f9-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="ad4f9-108">Rendelkeznie kell egy érvényes bérlőt Google Apps a munkahelyén vagy a Google Apps oktatási célokra.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-108">You must have a valid tenant for Google Apps for Work or Google Apps for Education.</span></span> <span data-ttu-id="ad4f9-109">Egy ingyenes próbafiók vagy a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-109">You may use a free trial account for either service.</span></span>
*   <span data-ttu-id="ad4f9-110">Egy felhasználói fiókot a Google Apps Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-110">A user account in Google Apps with Team Admin permissions.</span></span>

## <a name="assigning-users-to-google-apps"></a><span data-ttu-id="ad4f9-111">Google Apps felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ad4f9-111">Assigning users to Google Apps</span></span>

<span data-ttu-id="ad4f9-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="ad4f9-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="ad4f9-114">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a Google Apps alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Google Apps app.</span></span> <span data-ttu-id="ad4f9-115">Ha úgy döntött, rendelhet ezek a felhasználók a Google Apps alkalmazásba utasítások itt: [egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span><span class="sxs-lookup"><span data-stu-id="ad4f9-115">Once decided, you can assign these users to your Google Apps app by following the instructions here: [Assign a user or group to an enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span></span>

> [!IMPORTANT]
>*   <span data-ttu-id="ad4f9-116">Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzárendelni Google Apps teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-116">It is recommended that a single Azure AD user be assigned to Google Apps to test the provisioning configuration.</span></span> <span data-ttu-id="ad4f9-117">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-117">Additional users and/or groups may be assigned later.</span></span>
>*   <span data-ttu-id="ad4f9-118">Amikor egy felhasználó rendel a Google Apps, a hozzárendelés párbeszédpanelen válassza ki a felhasználó vagy a "Csoport" szerepkör.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-118">When assigning a user to Google Apps, you must select the User or "Group" role in the assignment dialog.</span></span> <span data-ttu-id="ad4f9-119">A "Default" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-119">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="ad4f9-120">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ad4f9-120">Enable automated user provisioning</span></span>

<span data-ttu-id="ad4f9-121">Ez a szakasz útmutatók csatlakozás az Azure AD Google Apps felhasználói fiók létesítési API, és a létesítési szolgáltatás létrehozása, konfigurálása frissíti, és tiltsa le a hozzárendelt felhasználói fiókok a Google Apps a felhasználói és az Azure AD-csoport-hozzárendelés alapján.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-121">This section guides you through connecting your Azure AD to Google Apps's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Google Apps based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="ad4f9-122">Dönthet úgy is, engedélyezze SAML-alapú egyszeri bejelentkezést a Google Apps, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ad4f9-122">You may also choose to enabled SAML-based Single Sign-On for Google Apps, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ad4f9-123">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-123">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="configure-automatic-user-account-provisioning"></a><span data-ttu-id="ad4f9-124">Konfigurálja az automatikus felhasználói fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="ad4f9-124">Configure automatic user account provisioning</span></span>

> [!NOTE]
> <span data-ttu-id="ad4f9-125">Egy másik kivitelezhető lehetőség, a felhasználók a Google Apps átadása automatizálásához [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) amely látja el a helyszíni Active Directory identitások Google Apps.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-125">Another viable option for automating user provisioning to Google Apps is to use [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) which provisions your on-premises Active Directory identities to Google Apps.</span></span> <span data-ttu-id="ad4f9-126">Ezzel ellentétben ebben az oktatóanyagban a megoldás látja el az Azure Active Directoryban (felhő) felhasználók és a Google Apps levelezési csoportokat.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-126">In contrast, the solution in this tutorial provisions your Azure Active Directory (cloud) users and mail-enabled groups to Google Apps.</span></span> 

1. <span data-ttu-id="ad4f9-127">Jelentkezzen be a [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával, és kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-127">Sign into the [Google Apps Admin Console](http://admin.google.com/) using your administrator account, and click **Security**.</span></span> <span data-ttu-id="ad4f9-128">Ha nem látja a hivatkozásra, akkor előfordulhat, hogy rejtve alatt a **további vezérlők** menü a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-128">If you don't see the link, it may be hidden under the **More Controls** menu at the bottom of the screen.</span></span>
   
    ![Kattintson a Security (Biztonság) elemre.][10]

2. <span data-ttu-id="ad4f9-130">Az a **biztonsági** kattintson **API-referencia**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-130">On the **Security** page, click **API Reference**.</span></span>
   
    ![Kattintson az API-hivatkozás.][15]

3. <span data-ttu-id="ad4f9-132">Válassza ki **engedélyezése API-hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-132">Select **Enable API access**.</span></span>
   
    ![Kattintson az API-hivatkozás.][16]

    > [!IMPORTANT]
    > <span data-ttu-id="ad4f9-134">Minden felhasználó kiépítését a Google Apps, az Azure Active Directoryban a felhasználónevét, melyet *kell* egyéni tartományt összekapcsolását.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-134">For every user that you intend to provision to Google Apps, their username in Azure Active Directory *must* be tied to a custom domain.</span></span> <span data-ttu-id="ad4f9-135">Például az alábbihoz hasonló felhasználónevek bob@contoso.onmicrosoft.com nem fogadja el a Google Apps, mivel bob@contoso.com el van fogadva.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-135">For example, usernames that look like bob@contoso.onmicrosoft.com is not accepted by Google Apps, whereas bob@contoso.com is accepted.</span></span> <span data-ttu-id="ad4f9-136">A tulajdonságok módosítása az Azure AD egy meglévő felhasználó tartományi módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-136">You can change an existing user's domain by editing their properties in Azure AD.</span></span> <span data-ttu-id="ad4f9-137">Az egyéni tartománynév beállítása az Azure Active Directory és a Google Apps utasításokat lépéseket követve szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-137">Instructions for how to set a custom domain for both Azure Active Directory and Google Apps are included in following steps.</span></span>
      
4. <span data-ttu-id="ad4f9-138">Ha egy egyéni tartománynevet még az Azure Active Directory még nem vett, majd kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-138">If you haven't added a custom domain name to your Azure Active Directory yet, then follow the following steps:</span></span>
  
    <span data-ttu-id="ad4f9-139">a.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-139">a.</span></span> <span data-ttu-id="ad4f9-140">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-140">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span> <span data-ttu-id="ad4f9-141">A könyvtár listában válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-141">In the directory list, select your directory.</span></span> 

    <span data-ttu-id="ad4f9-142">b.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-142">b.</span></span> <span data-ttu-id="ad4f9-143">Kattintson a **tartományok neve** a bal oldali navigációs panelen, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-143">Click **Domains name** on the left navigation pane, and then click **Add**.</span></span>
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![tartomány hozzáadása](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    <span data-ttu-id="ad4f9-146">c.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-146">c.</span></span> <span data-ttu-id="ad4f9-147">Írja be a tartomány nevét a **tartománynév** mező.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-147">Type your domain name into the **Domain name** field.</span></span> <span data-ttu-id="ad4f9-148">Ezt a tartománynevet szeretne használni a Google Apps tartományi megegyező nevet kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-148">This domain name should be the same domain name that you intend to use for Google Apps.</span></span> <span data-ttu-id="ad4f9-149">Ha elkészült, kattintson a **tartomány hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-149">When ready, click the **Add Domain** button.</span></span>
     
     ![Tartománynév](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    <span data-ttu-id="ad4f9-151">d.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-151">d.</span></span> <span data-ttu-id="ad4f9-152">Kattintson a **következő** az ellenőrzési lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-152">Click **Next** to go to the verification page.</span></span> <span data-ttu-id="ad4f9-153">Győződjön meg arról, hogy Ön a tulajdonosa ezt a tartományt, szerkesztenie kell a tartomány DNS-rekordok ezen a lapon megadott értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-153">To verify that you own this domain, you must edit the domain's DNS records according to the values provided on this page.</span></span> <span data-ttu-id="ad4f9-154">Annak ellenőrzése vagy választhatja, hogy **MX-rekordok** vagy **TXT rekord**, attól függően, válassza a a **rekordtípus** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-154">You may choose to verify using either **MX records** or **TXT records**, depending on what you select for the **Record Type** option.</span></span> <span data-ttu-id="ad4f9-155">Az Azure AD tartománynév ellenőrzése átfogóbb útmutatásra van szüksége, tekintse meg a [saját tartománynév hozzáadása az Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ad4f9-155">For more comprehensive instructions on how to verify domain name with Azure AD, refer [Add your own domain name to Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span></span>
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    <span data-ttu-id="ad4f9-157">e.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-157">e.</span></span> <span data-ttu-id="ad4f9-158">Ismételje meg az előző lépéseket a könyvtárhoz hozzáadni kívánt összes tartományát.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-158">Repeat the preceding steps for all the domains that you intend to add to your directory.</span></span>

5. <span data-ttu-id="ad4f9-159">Most, hogy az Azure ad-vel a tartományok ellenőrzését, most ellenőriznie kell őket újra a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-159">Now that you have verified all your domains with Azure AD, you must now verify them again with Google Apps.</span></span> <span data-ttu-id="ad4f9-160">Minden tartományhoz, amely már nincs regisztrálva a Google Apps hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ad4f9-160">For each domain that isn't already registered with Google Apps, perform the following steps:</span></span>
   
    <span data-ttu-id="ad4f9-161">a.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-161">a.</span></span> <span data-ttu-id="ad4f9-162">Az a [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-162">In the [Google Apps Admin Console](http://admin.google.com/), click **Domains**.</span></span>
     
     ![Kattintson a tartományok][20]

    <span data-ttu-id="ad4f9-164">b.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-164">b.</span></span> <span data-ttu-id="ad4f9-165">Kattintson a **hozzáadása egy tartományhoz vagy egy tartományi alias**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-165">Click **Add a domain or a domain alias**.</span></span>
     
     ![Új tartomány hozzáadása][21]

    <span data-ttu-id="ad4f9-167">c.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-167">c.</span></span> <span data-ttu-id="ad4f9-168">Válassza ki **egy másik tartomány hozzáadása**, és írja be a tartományt, amelyikhez hozzá szeretné nevében.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-168">Select **Add another domain**, and type in the name of the domain that you would like to add.</span></span>
     
     ![Adja meg a tartomány neve][22]

    <span data-ttu-id="ad4f9-170">d.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-170">d.</span></span> <span data-ttu-id="ad4f9-171">Kattintson a **folytatja, és ellenőrizze a tartomány tulajdonosa**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-171">Click **Continue and verify domain ownership**.</span></span> <span data-ttu-id="ad4f9-172">Kövesse a lépéseket, hogy Ön a tulajdonosa a tartománynév ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-172">Then follow the steps to verify that you own the domain name.</span></span> <span data-ttu-id="ad4f9-173">Ellenőrizze a tartományt a Google Apps átfogó útmutatást talál.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-173">For comprehensive instructions on how to verify your domain with Google Apps, see.</span></span> <span data-ttu-id="ad4f9-174">[Ellenőrizze a hely tulajdonjoga, a Google Apps](https://support.google.com/webmasters/answer/35179).</span><span class="sxs-lookup"><span data-stu-id="ad4f9-174">[Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179).</span></span>

    <span data-ttu-id="ad4f9-175">e.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-175">e.</span></span> <span data-ttu-id="ad4f9-176">Ismételje meg a fenti lépéseket minden olyan további tartományt, amelyet lemezképfájlforrásként kíván hozzáadni a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-176">Repeat the preceding steps for any additional domains that you intend to add to Google Apps.</span></span>
     
     > [!WARNING]
     > <span data-ttu-id="ad4f9-177">Ha az elsődleges tartomány módosítja a Google Apps bérlő számára, és ha már konfigurált az egyszeri bejelentkezés az Azure AD, majd ismételje meg a #3 alapján, hogy [lépés két: engedélyezése egyszeri bejelentkezéshez](#step-two-enable-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="ad4f9-177">If you change the primary domain for your Google Apps tenant, and if you have already configured single sign-on with Azure AD, then you have to repeat step #3 under [Step Two: Enable Single Sign-On](#step-two-enable-single-sign-on).</span></span>
       
6. <span data-ttu-id="ad4f9-178">Az a [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **rendszergazdai szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-178">In the [Google Apps Admin Console](http://admin.google.com/), click **Admin Roles**.</span></span>
   
     ![Kattintson a Google Apps][26]

7. <span data-ttu-id="ad4f9-180">Határozza meg, mely rendszergazdai fiók segítségével kezelheti a felhasználók átadása szeretné.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-180">Determine which admin account you would like to use to manage user provisioning.</span></span> <span data-ttu-id="ad4f9-181">Az a **rendszergazdai szerepkörrel** -fiók, szerkesztheti a **jogosultságokkal** adott szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-181">For the **admin role** of that account, edit the **Privileges** for that role.</span></span> <span data-ttu-id="ad4f9-182">Győződjön meg arról, hogy rendelkezik az összes a **rendszergazda API jogosultságokkal** engedélyezve van, hogy ez a fiók kiépítése is használható.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-182">Make sure it has all the **Admin API Privileges** enabled so that this account can be used for provisioning.</span></span>
   
     ![Kattintson a Google Apps][27]
   
    > [!NOTE]
    > <span data-ttu-id="ad4f9-184">Ha konfigurál egy éles környezetben, az ajánlott eljárás, ha egy rendszergazdai fiók a Google Apps kifejezetten a ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-184">If you are configuring a production environment, the best practice is to create an admin account in Google Apps specifically for this step.</span></span> <span data-ttu-id="ad4f9-185">Ezek a fiókok rendszergazda szerepkörrel társítva, amely rendelkezik a szükséges API-jogosultságokkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-185">These accounts must have an admin role associated with it that has the necessary API privileges.</span></span>
     
8. <span data-ttu-id="ad4f9-186">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-186">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

9. <span data-ttu-id="ad4f9-187">Ha már beállította az egyszeri bejelentkezés Google Apps, keresse meg a Google Apps, használja a keresőmezőt példányát.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-187">If you have already configured Google Apps for single sign-on, search for your instance of Google Apps using the search field.</span></span> <span data-ttu-id="ad4f9-188">Máskülönben válassza **Hozzáadás** keresse meg a **Google Apps** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-188">Otherwise, select **Add** and search for **Google Apps** in the application gallery.</span></span> <span data-ttu-id="ad4f9-189">A keresési eredmények közül válassza ki a Google Apps, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-189">Select Google Apps from the search results, and add it to your list of applications.</span></span>

10. <span data-ttu-id="ad4f9-190">Jelölje ki a Google Apps példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-190">Select your instance of Google Apps, then select the **Provisioning** tab.</span></span>

11. <span data-ttu-id="ad4f9-191">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-191">Set the **Provisioning Mode** to **Automatic**.</span></span> 

     ![Kiépítés](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. <span data-ttu-id="ad4f9-193">Az a **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-193">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="ad4f9-194">A Google Apps engedélyezési párbeszédpanel egy új böngészőablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-194">It opens a Google Apps authorization dialog in a new browser window.</span></span>

13. <span data-ttu-id="ad4f9-195">Győződjön meg arról, hogy szeretné-e hajtani a Google Apps-bérlő Azure Active Directory engedélyt.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-195">Confirm that you would like to give Azure Active Directory permission to make changes to your Google Apps tenant.</span></span> <span data-ttu-id="ad4f9-196">Kattintson a **elfogadása**.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-196">Click **Accept**.</span></span>
    
     ![Ellenőrizze az engedélyeket.][28]

14. <span data-ttu-id="ad4f9-198">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD kapcsolódhatnak a Google Apps-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-198">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Google Apps app.</span></span> <span data-ttu-id="ad4f9-199">Ha nem sikerül, győződjön meg arról, a Google Apps-fiók Team rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon a **"Engedélyezés"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-199">If the connection fails, ensure your Google Apps account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

15. <span data-ttu-id="ad4f9-200">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-200">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

16. <span data-ttu-id="ad4f9-201">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="ad4f9-201">Click **Save.**</span></span>

17. <span data-ttu-id="ad4f9-202">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók a Google Apps.**</span><span class="sxs-lookup"><span data-stu-id="ad4f9-202">Under the Mappings section, select **Synchronize Azure Active Directory Users to Google Apps.**</span></span>

18. <span data-ttu-id="ad4f9-203">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumokat a Google Apps szinkronizált Azure AD-ből.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-203">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Google Apps.</span></span> <span data-ttu-id="ad4f9-204">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Google Apps a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-204">The attributes selected as **Matching** properties are used to match the user accounts in Google Apps for update operations.</span></span> <span data-ttu-id="ad4f9-205">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-205">Select the Save button to commit any changes.</span></span>

19. <span data-ttu-id="ad4f9-206">Google Apps szolgáltatás kiépítését az Azure AD engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában</span><span class="sxs-lookup"><span data-stu-id="ad4f9-206">To enable the Azure AD provisioning service for Google Apps, change the **Provisioning Status** to **On** in the Settings section</span></span>

20. <span data-ttu-id="ad4f9-207">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="ad4f9-207">Click **Save.**</span></span>

<span data-ttu-id="ad4f9-208">A kezdeti szinkronizálás bármely felhasználói és/vagy a Google Apps, a felhasználók és csoportok szakaszban rendelt csoportok kezdődik.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-208">It starts the initial synchronization of any users and/or groups assigned to Google Apps in the Users and Groups section.</span></span> <span data-ttu-id="ad4f9-209">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-209">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="ad4f9-210">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amely minden, a Google Apps-alkalmazások létesítési szolgáltatás által végrehajtott műveleteket írják le.</span><span class="sxs-lookup"><span data-stu-id="ad4f9-210">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Google Apps app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ad4f9-211">További források</span><span class="sxs-lookup"><span data-stu-id="ad4f9-211">Additional resources</span></span>

* [<span data-ttu-id="ad4f9-212">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="ad4f9-212">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ad4f9-213">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ad4f9-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ad4f9-214">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ad4f9-214">Configure Single Sign-on</span></span>](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png