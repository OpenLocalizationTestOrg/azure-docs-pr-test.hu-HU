---
title: "Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, az Azure AD tooGoogle alkalmazásokat."
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
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a><span data-ttu-id="00812-103">Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása</span><span class="sxs-lookup"><span data-stu-id="00812-103">Tutorial: Configuring Google Apps for automatic user provisioning</span></span>

<span data-ttu-id="00812-104">hello Ez az oktatóanyag célja, a Google Apps és az Azure AD tooautomatically rendelkezés tooperform kell, és a leépíti a felhasználói fiókok Azure AD tooGoogle alkalmazások hello tooshow.</span><span class="sxs-lookup"><span data-stu-id="00812-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Google Apps and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGoogle Apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00812-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00812-105">Prerequisites</span></span>

<span data-ttu-id="00812-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="00812-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="00812-107">Az Azure Active directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="00812-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="00812-108">Rendelkeznie kell egy érvényes bérlőt Google Apps a munkahelyén vagy a Google Apps oktatási célokra.</span><span class="sxs-lookup"><span data-stu-id="00812-108">You must have a valid tenant for Google Apps for Work or Google Apps for Education.</span></span> <span data-ttu-id="00812-109">Egy ingyenes próbafiók vagy a szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="00812-109">You may use a free trial account for either service.</span></span>
*   <span data-ttu-id="00812-110">Egy felhasználói fiókot a Google Apps Team rendszergazdai engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="00812-110">A user account in Google Apps with Team Admin permissions.</span></span>

## <a name="assigning-users-toogoogle-apps"></a><span data-ttu-id="00812-111">Felhasználók hozzárendelése tooGoogle alkalmazások</span><span class="sxs-lookup"><span data-stu-id="00812-111">Assigning users tooGoogle Apps</span></span>

<span data-ttu-id="00812-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="00812-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="00812-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="00812-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="00812-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Google Apps alkalmazást kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="00812-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Google Apps app.</span></span> <span data-ttu-id="00812-115">Ha úgy döntött, ezen felhasználók tooyour Google Apps alkalmazást itt hello utasításokat követve hozzárendelheti: [hozzárendelése egy felhasználóhoz vagy csoporthoz tooan vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span><span class="sxs-lookup"><span data-stu-id="00812-115">Once decided, you can assign these users tooyour Google Apps app by following hello instructions here: [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span></span>

> [!IMPORTANT]
>*   <span data-ttu-id="00812-116">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooGoogle alkalmazások tootest hello konfigurálása kiosztás rendelni.</span><span class="sxs-lookup"><span data-stu-id="00812-116">It is recommended that a single Azure AD user be assigned tooGoogle Apps tootest hello provisioning configuration.</span></span> <span data-ttu-id="00812-117">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="00812-117">Additional users and/or groups may be assigned later.</span></span>
>*   <span data-ttu-id="00812-118">Amikor egy felhasználó tooGoogle alkalmazások rendel, ki kell választania hello felhasználó vagy a "Csoport" szerepkör hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="00812-118">When assigning a user tooGoogle Apps, you must select hello User or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="00812-119">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="00812-119">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="00812-120">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="00812-120">Enable automated user provisioning</span></span>

<span data-ttu-id="00812-121">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooGoogle alkalmazások felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Google Apps alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben .</span><span class="sxs-lookup"><span data-stu-id="00812-121">This section guides you through connecting your Azure AD tooGoogle Apps's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Google Apps based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="00812-122">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezés Google Apps, hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="00812-122">You may also choose tooenabled SAML-based Single Sign-On for Google Apps, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="00812-123">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="00812-123">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="configure-automatic-user-account-provisioning"></a><span data-ttu-id="00812-124">Konfigurálja az automatikus felhasználói fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="00812-124">Configure automatic user account provisioning</span></span>

> [!NOTE]
> <span data-ttu-id="00812-125">A felhasználók átadásához tooGoogle alkalmazások automatizálásához egy másik kivitelezhető lehetőség toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) amely látja el a helyszíni Active Directory identitások tooGoogle alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="00812-125">Another viable option for automating user provisioning tooGoogle Apps is toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) which provisions your on-premises Active Directory identities tooGoogle Apps.</span></span> <span data-ttu-id="00812-126">Ezzel ellentétben ebben az oktatóanyagban hello megoldás látja el az Azure Active Directoryban (felhő) felhasználók és a levelezési csoportok tooGoogle alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="00812-126">In contrast, hello solution in this tutorial provisions your Azure Active Directory (cloud) users and mail-enabled groups tooGoogle Apps.</span></span> 

1. <span data-ttu-id="00812-127">Jelentkezzen be a hello [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával, és kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="00812-127">Sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account, and click **Security**.</span></span> <span data-ttu-id="00812-128">Ha hello hivatkozás nem látható, akkor előfordulhat, hogy rejtve a hello **további vezérlők** menü üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="00812-128">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Kattintson a Security (Biztonság) elemre.][10]

2. <span data-ttu-id="00812-130">A hello **biztonsági** kattintson **API-referencia**.</span><span class="sxs-lookup"><span data-stu-id="00812-130">On hello **Security** page, click **API Reference**.</span></span>
   
    ![Kattintson az API-hivatkozás.][15]

3. <span data-ttu-id="00812-132">Válassza ki **engedélyezése API-hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="00812-132">Select **Enable API access**.</span></span>
   
    ![Kattintson az API-hivatkozás.][16]

    > [!IMPORTANT]
    > <span data-ttu-id="00812-134">Minden felhasználó esetén, hogy szeretné-e tooprovision tooGoogle alkalmazások, a felhasználónevét, az Azure Active Directoryban *kell* kapcsolt tooa egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="00812-134">For every user that you intend tooprovision tooGoogle Apps, their username in Azure Active Directory *must* be tied tooa custom domain.</span></span> <span data-ttu-id="00812-135">Például az alábbihoz hasonló felhasználónevek bob@contoso.onmicrosoft.com nem fogadja el a Google Apps, mivel bob@contoso.com el van fogadva.</span><span class="sxs-lookup"><span data-stu-id="00812-135">For example, usernames that look like bob@contoso.onmicrosoft.com is not accepted by Google Apps, whereas bob@contoso.com is accepted.</span></span> <span data-ttu-id="00812-136">A tulajdonságok módosítása az Azure AD egy meglévő felhasználó tartományi módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="00812-136">You can change an existing user's domain by editing their properties in Azure AD.</span></span> <span data-ttu-id="00812-137">Útmutatás a hogyan tooset egyéni tartományt az Azure Active Directory és a Google alkalmazások is szerepelnek lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="00812-137">Instructions for how tooset a custom domain for both Azure Active Directory and Google Apps are included in following steps.</span></span>
      
4. <span data-ttu-id="00812-138">Ha még nem adott meg egy egyéni tartomány nevét tooyour Azure Active Directoryban, majd kövesse a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="00812-138">If you haven't added a custom domain name tooyour Azure Active Directory yet, then follow hello following steps:</span></span>
  
    <span data-ttu-id="00812-139">a.</span><span class="sxs-lookup"><span data-stu-id="00812-139">a.</span></span> <span data-ttu-id="00812-140">A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="00812-140">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span> <span data-ttu-id="00812-141">Hello directory listában válassza ki a címtárát.</span><span class="sxs-lookup"><span data-stu-id="00812-141">In hello directory list, select your directory.</span></span> 

    <span data-ttu-id="00812-142">b.</span><span class="sxs-lookup"><span data-stu-id="00812-142">b.</span></span> <span data-ttu-id="00812-143">Kattintson a **tartományok neve** hello bal oldali navigációs panelen, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="00812-143">Click **Domains name** on hello left navigation pane, and then click **Add**.</span></span>
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![tartomány hozzáadása](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    <span data-ttu-id="00812-146">c.</span><span class="sxs-lookup"><span data-stu-id="00812-146">c.</span></span> <span data-ttu-id="00812-147">Adja meg a tartomány nevét az hello **tartománynév** mező.</span><span class="sxs-lookup"><span data-stu-id="00812-147">Type your domain name into hello **Domain name** field.</span></span> <span data-ttu-id="00812-148">Ezt a tartománynevet kell hello toouse szeretné Google Apps ugyanazon tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="00812-148">This domain name should be hello same domain name that you intend toouse for Google Apps.</span></span> <span data-ttu-id="00812-149">Ha elkészült, kattintson a hello **tartomány hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="00812-149">When ready, click hello **Add Domain** button.</span></span>
     
     ![Tartománynév](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    <span data-ttu-id="00812-151">d.</span><span class="sxs-lookup"><span data-stu-id="00812-151">d.</span></span> <span data-ttu-id="00812-152">Kattintson a **következő** toogo toohello ellenőrzési lapot.</span><span class="sxs-lookup"><span data-stu-id="00812-152">Click **Next** toogo toohello verification page.</span></span> <span data-ttu-id="00812-153">tooverify, hogy Ön a tulajdonosa ezt a tartományt, szerkesztenie kell a hello tartomány DNS-rekordok ezen a lapon megadott toohello értékek alapján történik.</span><span class="sxs-lookup"><span data-stu-id="00812-153">tooverify that you own this domain, you must edit hello domain's DNS records according toohello values provided on this page.</span></span> <span data-ttu-id="00812-154">Úgy is dönthet, segítségével tooverify **MX-rekordok** vagy **TXT rekord**, attól függően, válassza a hello **rekordtípus** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="00812-154">You may choose tooverify using either **MX records** or **TXT records**, depending on what you select for hello **Record Type** option.</span></span> <span data-ttu-id="00812-155">Hogyan tooverify tartománynév az Azure ad-val átfogóbb utasításokért tekintse meg a [adja hozzá a saját tartomány neve tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="00812-155">For more comprehensive instructions on how tooverify domain name with Azure AD, refer [Add your own domain name tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span></span>
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    <span data-ttu-id="00812-157">e.</span><span class="sxs-lookup"><span data-stu-id="00812-157">e.</span></span> <span data-ttu-id="00812-158">Ismételje meg az előző lépésekben, hogy szeretné-e tooadd tooyour directory tartomány hello hello.</span><span class="sxs-lookup"><span data-stu-id="00812-158">Repeat hello preceding steps for all hello domains that you intend tooadd tooyour directory.</span></span>

5. <span data-ttu-id="00812-159">Most, hogy az Azure ad-vel a tartományok ellenőrzését, most ellenőriznie kell őket újra a Google Apps.</span><span class="sxs-lookup"><span data-stu-id="00812-159">Now that you have verified all your domains with Azure AD, you must now verify them again with Google Apps.</span></span> <span data-ttu-id="00812-160">Minden tartományhoz, amely már nincs regisztrálva a Google Apps hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="00812-160">For each domain that isn't already registered with Google Apps, perform hello following steps:</span></span>
   
    <span data-ttu-id="00812-161">a.</span><span class="sxs-lookup"><span data-stu-id="00812-161">a.</span></span> <span data-ttu-id="00812-162">A hello [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="00812-162">In hello [Google Apps Admin Console](http://admin.google.com/), click **Domains**.</span></span>
     
     ![Kattintson a tartományok][20]

    <span data-ttu-id="00812-164">b.</span><span class="sxs-lookup"><span data-stu-id="00812-164">b.</span></span> <span data-ttu-id="00812-165">Kattintson a **hozzáadása egy tartományhoz vagy egy tartományi alias**.</span><span class="sxs-lookup"><span data-stu-id="00812-165">Click **Add a domain or a domain alias**.</span></span>
     
     ![Új tartomány hozzáadása][21]

    <span data-ttu-id="00812-167">c.</span><span class="sxs-lookup"><span data-stu-id="00812-167">c.</span></span> <span data-ttu-id="00812-168">Válassza ki **egy másik tartomány hozzáadása**, és, hogy szeretné-e tooadd hello tartomány hello nevében típusát.</span><span class="sxs-lookup"><span data-stu-id="00812-168">Select **Add another domain**, and type in hello name of hello domain that you would like tooadd.</span></span>
     
     ![Adja meg a tartomány neve][22]

    <span data-ttu-id="00812-170">d.</span><span class="sxs-lookup"><span data-stu-id="00812-170">d.</span></span> <span data-ttu-id="00812-171">Kattintson a **folytatja, és ellenőrizze a tartomány tulajdonosa**.</span><span class="sxs-lookup"><span data-stu-id="00812-171">Click **Continue and verify domain ownership**.</span></span> <span data-ttu-id="00812-172">Hajtsa végre a hello lépéseket tooverify, hogy Ön a tulajdonosa hello tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="00812-172">Then follow hello steps tooverify that you own hello domain name.</span></span> <span data-ttu-id="00812-173">Hogyan tooverify a Google Apps, tartomány: átfogó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="00812-173">For comprehensive instructions on how tooverify your domain with Google Apps, see.</span></span> <span data-ttu-id="00812-174">[Ellenőrizze a hely tulajdonjoga, a Google Apps](https://support.google.com/webmasters/answer/35179).</span><span class="sxs-lookup"><span data-stu-id="00812-174">[Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179).</span></span>

    <span data-ttu-id="00812-175">e.</span><span class="sxs-lookup"><span data-stu-id="00812-175">e.</span></span> <span data-ttu-id="00812-176">Ismételje meg a fenti lépéseket minden olyan további tartományt, hogy szeretné-e tooadd tooGoogle alkalmazások hello.</span><span class="sxs-lookup"><span data-stu-id="00812-176">Repeat hello preceding steps for any additional domains that you intend tooadd tooGoogle Apps.</span></span>
     
     > [!WARNING]
     > <span data-ttu-id="00812-177">Ha módosítja a Google Apps-bérlő hello elsődleges tartományt, és ha már konfigurált az egyszeri bejelentkezés az Azure AD, akkor el kell toorepeat lépés #3 alatt [lépés két: engedélyezése egyszeri bejelentkezéshez](#step-two-enable-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="00812-177">If you change hello primary domain for your Google Apps tenant, and if you have already configured single sign-on with Azure AD, then you have toorepeat step #3 under [Step Two: Enable Single Sign-On](#step-two-enable-single-sign-on).</span></span>
       
6. <span data-ttu-id="00812-178">A hello [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **rendszergazdai szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="00812-178">In hello [Google Apps Admin Console](http://admin.google.com/), click **Admin Roles**.</span></span>
   
     ![Kattintson a Google Apps][26]

7. <span data-ttu-id="00812-180">Határozza meg, mely rendszergazdai fiókhoz toouse toomanage a felhasználók átadása.</span><span class="sxs-lookup"><span data-stu-id="00812-180">Determine which admin account you would like toouse toomanage user provisioning.</span></span> <span data-ttu-id="00812-181">A hello **rendszergazdai szerepkör** -fiók, szerkesztheti a hello **jogosultságokkal** adott szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="00812-181">For hello **admin role** of that account, edit hello **Privileges** for that role.</span></span> <span data-ttu-id="00812-182">Ellenőrizze, hogy minden hello **rendszergazda API jogosultságokkal** engedélyezve van, hogy ez a fiók kiépítése is használható.</span><span class="sxs-lookup"><span data-stu-id="00812-182">Make sure it has all hello **Admin API Privileges** enabled so that this account can be used for provisioning.</span></span>
   
     ![Kattintson a Google Apps][27]
   
    > [!NOTE]
    > <span data-ttu-id="00812-184">Ha éles környezetben, hello ajánlott toocreate kifejezetten ebben a lépésben a Google Apps a rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="00812-184">If you are configuring a production environment, hello best practice is toocreate an admin account in Google Apps specifically for this step.</span></span> <span data-ttu-id="00812-185">Ezek a fiókok rendszergazda szerepkörrel társított hello szükséges API-jogosultsággal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="00812-185">These accounts must have an admin role associated with it that has hello necessary API privileges.</span></span>
     
8. <span data-ttu-id="00812-186">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="00812-186">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

9. <span data-ttu-id="00812-187">Ha már beállította az egyszeri bejelentkezés Google Apps, keresse meg a Google Apps hello keresési mező példányát.</span><span class="sxs-lookup"><span data-stu-id="00812-187">If you have already configured Google Apps for single sign-on, search for your instance of Google Apps using hello search field.</span></span> <span data-ttu-id="00812-188">Máskülönben válassza **Hozzáadás** keresse meg a **Google Apps** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="00812-188">Otherwise, select **Add** and search for **Google Apps** in hello application gallery.</span></span> <span data-ttu-id="00812-189">Válassza ki a Google Apps hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="00812-189">Select Google Apps from hello search results, and add it tooyour list of applications.</span></span>

10. <span data-ttu-id="00812-190">Jelölje ki a Google Apps példányát, majd válassza ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="00812-190">Select your instance of Google Apps, then select hello **Provisioning** tab.</span></span>

11. <span data-ttu-id="00812-191">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="00812-191">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

     ![Kiépítés](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. <span data-ttu-id="00812-193">A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="00812-193">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="00812-194">A Google Apps engedélyezési párbeszédpanel egy új böngészőablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="00812-194">It opens a Google Apps authorization dialog in a new browser window.</span></span>

13. <span data-ttu-id="00812-195">Győződjön meg arról, hogy szeretné-e toogive Azure Active Directory engedély toomake módosításai tooyour Google Apps-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="00812-195">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Google Apps tenant.</span></span> <span data-ttu-id="00812-196">Kattintson a **elfogadása**.</span><span class="sxs-lookup"><span data-stu-id="00812-196">Click **Accept**.</span></span>
    
     ![Ellenőrizze az engedélyeket.][28]

14. <span data-ttu-id="00812-198">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Google Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="00812-198">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Google Apps app.</span></span> <span data-ttu-id="00812-199">Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Google Apps-fiók Team rendszergazdai jogosultságokkal rendelkezik, majd próbálja hello **"Engedélyezés"** léptessen ismét.</span><span class="sxs-lookup"><span data-stu-id="00812-199">If hello connection fails, ensure your Google Apps account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

15. <span data-ttu-id="00812-200">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="00812-200">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

16. <span data-ttu-id="00812-201">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="00812-201">Click **Save.**</span></span>

17. <span data-ttu-id="00812-202">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooGoogle alkalmazásokat.**</span><span class="sxs-lookup"><span data-stu-id="00812-202">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGoogle Apps.**</span></span>

18. <span data-ttu-id="00812-203">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooGoogle alkalmazások szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="00812-203">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGoogle Apps.</span></span> <span data-ttu-id="00812-204">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok a Google Apps a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="00812-204">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Google Apps for update operations.</span></span> <span data-ttu-id="00812-205">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="00812-205">Select hello Save button toocommit any changes.</span></span>

19. <span data-ttu-id="00812-206">tooenable hello Azure AD létesítési szolgáltatás Google Apps, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a</span><span class="sxs-lookup"><span data-stu-id="00812-206">tooenable hello Azure AD provisioning service for Google Apps, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

20. <span data-ttu-id="00812-207">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="00812-207">Click **Save.**</span></span>

<span data-ttu-id="00812-208">Hello azokat a felhasználókat a kezdeti szinkronizálását kezdődik, és/vagy csoportok hozzárendelve tooGoogle alkalmazások hello felhasználók és csoportok szakaszban.</span><span class="sxs-lookup"><span data-stu-id="00812-208">It starts hello initial synchronization of any users and/or groups assigned tooGoogle Apps in hello Users and Groups section.</span></span> <span data-ttu-id="00812-209">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="00812-209">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="00812-210">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Google Apps-alkalmazások létesítési végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="00812-210">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Google Apps app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00812-211">További források</span><span class="sxs-lookup"><span data-stu-id="00812-211">Additional resources</span></span>

* [<span data-ttu-id="00812-212">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="00812-212">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00812-213">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="00812-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="00812-214">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="00812-214">Configure Single Sign-on</span></span>](active-directory-saas-google-apps-tutorial.md)



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