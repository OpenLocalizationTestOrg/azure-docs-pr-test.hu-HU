---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
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
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="78926-103">Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78926-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="78926-104">hello Ez az oktatóanyag célja tooshow meg hello által Facebook és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooWorkplace által Facebook munkahelyi tooperform lépéseit.</span><span class="sxs-lookup"><span data-stu-id="78926-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Workplace by Facebook and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78926-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="78926-105">Prerequisites</span></span>

<span data-ttu-id="78926-106">tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="78926-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="78926-107">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="78926-107">An Azure AD subscription</span></span>
- <span data-ttu-id="78926-108">A munkahelyi által Facebook egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="78926-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="78926-109">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="78926-109">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="78926-110">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="78926-110">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="78926-111">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="78926-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="78926-112">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="78926-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="78926-113">Felhasználók tooWorkplace által Facebook hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="78926-113">Assigning users tooWorkplace by Facebook</span></span>

<span data-ttu-id="78926-114">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="78926-114">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="78926-115">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="78926-115">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="78926-116">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour munkahelyi Facebook-alkalmazást úgy kell meghatároznia.</span><span class="sxs-lookup"><span data-stu-id="78926-116">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="78926-117">Ha úgy döntött, hozzárendelheti a felhasználók tooyour munkahelyi Facebook-alkalmazás által itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="78926-117">Once decided, you can assign these users tooyour Workplace by Facebook app by following hello instructions here:</span></span>

[<span data-ttu-id="78926-118">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="78926-118">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="78926-119">Fontos tippek a felhasználók tooWorkplace által Facebook hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="78926-119">Important tips for assigning users tooWorkplace by Facebook</span></span>

*   <span data-ttu-id="78926-120">Javasoljuk, hogy egyetlen Azure AD-felhasználó Facebook tootest hello konfigurálása kiosztás által hozzárendelt tooWorkplace.</span><span class="sxs-lookup"><span data-stu-id="78926-120">It is recommended that a single Azure AD user is assigned tooWorkplace by Facebook tootest hello provisioning configuration.</span></span> <span data-ttu-id="78926-121">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="78926-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="78926-122">Amikor egy felhasználó tooWorkplace által Facebook rendel, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="78926-122">When assigning a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="78926-123">hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="78926-123">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="78926-124">Felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="78926-124">Enable User Provisioning</span></span>

<span data-ttu-id="78926-125">Ez a szakasz végigvezeti az Azure AD tooWorkplace csatlakozó Facebook a felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a munkahelyi Facebook alapján a felhasználók és csoportok által hozzárendelt felhasználói fiókok az Azure AD-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="78926-125">This section guides you through connecting your Azure AD tooWorkplace by Facebook's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="78926-126">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Facebook, a következő utasításokat hello által munkahelyi [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="78926-126">You may also choose tooenabled SAML-based Single Sign-On for Workplace by Facebook, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="78926-127">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="78926-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="78926-128">tooconfigure felhasználói fiók által Facebook tooWorkplace kiépítése az Azure ad-ben:</span><span class="sxs-lookup"><span data-stu-id="78926-128">tooconfigure user account provisioning tooWorkplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="78926-129">hello ebben a szakaszban célja toooutline hogyan tooenable kiépítése Active Directory felhasználói fiókok által Facebook tooWorkplace.</span><span class="sxs-lookup"><span data-stu-id="78926-129">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooWorkplace by Facebook.</span></span>

<span data-ttu-id="78926-130">Az Azure AD által támogatott hello képességét tooautomatically szinkronizálása hello fiók részleteinek felhasználók tooWorkplace Facebook által hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="78926-130">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="78926-131">Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi Facebook tooget hello adatok tooauthorize felhasználók a hozzáférés érdekében előtt kell őket toosign a hello az első alkalommal próbált.</span><span class="sxs-lookup"><span data-stu-id="78926-131">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="78926-132">Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.</span><span class="sxs-lookup"><span data-stu-id="78926-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="78926-133">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="78926-133">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="78926-134">Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által hello keresési mező Facebook példányát.</span><span class="sxs-lookup"><span data-stu-id="78926-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using hello search field.</span></span> <span data-ttu-id="78926-135">Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="78926-135">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="78926-136">Válassza ki a munkahely által Facebook hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="78926-136">Select Workplace by Facebook from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="78926-137">Válassza ki a munkahely által Facebook példányt, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="78926-137">Select your instance of Workplace by Facebook, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="78926-138">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="78926-138">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Kiépítés](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="78926-140">A hello **rendszergazdai hitelesítő adataival** szakaszt, adja meg a titkos kulcs Token hello és bérlői URL-cím, a munkahelyének Facebook rendszergazda hello.</span><span class="sxs-lookup"><span data-stu-id="78926-140">Under hello **Admin Credentials** section, enter hello Secret Token and hello Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="78926-141">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak munkahelyi tooyour által Facebook-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="78926-141">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="78926-142">Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Facebook-fiókkal munkahelyi Team rendszergazdai jogosultságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="78926-142">If hello connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="78926-143">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="78926-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="78926-144">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="78926-144">Click **Save.**</span></span>

9. <span data-ttu-id="78926-145">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooWorkplace által Facebook-on.**</span><span class="sxs-lookup"><span data-stu-id="78926-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook.**</span></span>

10. <span data-ttu-id="78926-146">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooWorkplace Facebook által szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="78926-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="78926-147">kiválasztott attribútumok hello **egyező** tulajdonságok: használt toomatch hello tartozó felhasználói fiókok által Facebook munkahelyi frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="78926-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="78926-148">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="78926-148">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="78926-149">tooenable hello Azure AD létesítési szolgáltatás által a Facebook, a módosítás hello munkahelyi fiókhoz **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="78926-149">tooenable hello Azure AD provisioning service for Workplace by Facebook, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="78926-150">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="78926-150">Click **Save.**</span></span>

<span data-ttu-id="78926-151">További információt a tooconfigure automatikus kiépítés, lásd: [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="78926-151">For more information on how tooconfigure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="78926-152">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="78926-152">You can now create a test account.</span></span> <span data-ttu-id="78926-153">Várjon, amíg fel hello fiókot töltött tooverify tooWorkplace Facebook által szinkronizált too20 perc.</span><span class="sxs-lookup"><span data-stu-id="78926-153">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78926-154">További források</span><span class="sxs-lookup"><span data-stu-id="78926-154">Additional resources</span></span>

* [<span data-ttu-id="78926-155">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="78926-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="78926-156">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="78926-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="78926-157">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78926-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

