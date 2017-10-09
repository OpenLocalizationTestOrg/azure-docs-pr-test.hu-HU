---
title: "Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, a Facebook által az Azure AD tooWorkplace."
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
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="4f644-103">Oktatóanyag: A felhasználók átadása által Facebook munkahelyi konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f644-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="4f644-104">Az oktatóanyag azt mutatja be, a szükséges lépéseket tooautomatically hello kiépítése és leépíti a felhasználói fiókok Azure Active Directory (Azure AD) tooWorkplace által Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="4f644-104">This tutorial shows you hello steps necessary tooautomatically provision and de-provision user accounts from Azure Active Directory (Azure AD) tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f644-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f644-105">Prerequisites</span></span>

<span data-ttu-id="4f644-106">az Azure AD-integráció a munkahely által Facebook tooconfigure, következőkre lesz szüksége hello:</span><span class="sxs-lookup"><span data-stu-id="4f644-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following:</span></span>

- <span data-ttu-id="4f644-107">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4f644-107">An Azure AD subscription</span></span>
- <span data-ttu-id="4f644-108">A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="4f644-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="4f644-109">Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="4f644-109">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="4f644-110">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4f644-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4f644-111">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f644-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-tooworkplace-by-facebook"></a><span data-ttu-id="4f644-112">Rendelje hozzá a felhasználók tooWorkplace által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="4f644-112">Assign users tooWorkplace by Facebook</span></span>

<span data-ttu-id="4f644-113">Az Azure AD egy fogalom, mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű használja.</span><span class="sxs-lookup"><span data-stu-id="4f644-113">Azure AD uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="4f644-114">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok tooan alkalmazás az Azure ad-ben rendelt szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="4f644-114">In hello context of automatic user account provisioning, only hello users and groups that have been assigned tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="4f644-115">Hello szolgáltatás, kiépítés engedélyezése és konfigurálása eldönti mely felhasználók és csoportok az Azure AD képviselő hello felhasználók számára, akik kell tooyour munkahelyi által Facebook-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f644-115">Before configuring and enabling hello provisioning service, decide what users and groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="4f644-116">Facebook-alkalmazás által a felhasználók tooyour munkahelyi rendelheti a következő témakör utasításait: hello [hozzárendelése egy felhasználóhoz vagy csoporthoz tooan vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4f644-116">You can then assign these users tooyour Workplace by Facebook app by following hello instructions in [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="4f644-117">Teszt hello egyetlen hozzárendelésével konfigurációs kiépítése az Azure AD felhasználói tooWorkplace által Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="4f644-117">Test hello provisioning configuration by assigning a single Azure AD user tooWorkplace by Facebook.</span></span> <span data-ttu-id="4f644-118">További felhasználók és csoportok hozzárendelése a később.</span><span class="sxs-lookup"><span data-stu-id="4f644-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="4f644-119">Ha egy felhasználó tooWorkplace által Facebook, ki kell választania egy érvényes felhasználói szerepkörnek.</span><span class="sxs-lookup"><span data-stu-id="4f644-119">When you assign a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="4f644-120">hello alapértelmezett szerepkör nem működik történő üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f644-120">hello Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="4f644-121">Az automatikus felhasználó-kiépítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4f644-121">Enable automated user provisioning</span></span>

<span data-ttu-id="4f644-122">Ez a szakasz végigvezeti az Azure AD toohello felhasználói fiókja munkahelyi API által a Facebook-kiépítés kapcsolódás.</span><span class="sxs-lookup"><span data-stu-id="4f644-122">This section guides you through connecting your Azure AD toohello user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="4f644-123">Azt is megtudhatja, hogyan tooconfigure hello létesítési szolgáltatás toocreate frissítése, és tiltsa le a munkahelyi Facebook által hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="4f644-123">You also learn how tooconfigure hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="4f644-124">Ez a felhasználó és az Azure AD-csoport-hozzárendelés alapul.</span><span class="sxs-lookup"><span data-stu-id="4f644-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="4f644-125">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezés által Facebook, a munkahelyi fiókhoz az hello hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4f644-125">You can also choose tooenabled SAML-based SSO for Workplace by Facebook, by following hello instructions provided in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4f644-126">Egyszeri bejelentkezés automatikus kiépítés, függetlenül konfigurálhatók, abban az esetben, ha ez a két funkció egészítik ki egymást.</span><span class="sxs-lookup"><span data-stu-id="4f644-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="4f644-127">Az Azure AD által Facebook tooWorkplace kiépítés felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="4f644-127">Configure user account provisioning tooWorkplace by Facebook in Azure AD</span></span>

<span data-ttu-id="4f644-128">Az Azure AD által támogatott hello képességét tooautomatically szinkronizálása hello fiók részleteinek felhasználók tooWorkplace Facebook által hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="4f644-128">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="4f644-129">Az automatikus szinkronizálás lehetővé teszi, hogy a munkahelyi Facebook tooget hello adatok tooauthorize felhasználók a hozzáférés érdekében előtt kell őket toosign a hello az első alkalommal próbált.</span><span class="sxs-lookup"><span data-stu-id="4f644-129">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="4f644-130">Azt is deszerializálni látja el felhasználók a munkahely által Facebook amikor hozzáférés az Azure AD vissza lett vonva.</span><span class="sxs-lookup"><span data-stu-id="4f644-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="4f644-131">A hello [Azure-portálon](https://portal.azure.com), jelölje be **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4f644-131">In hello [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="4f644-132">Ha már beállította az egyszeri bejelentkezés munkahelyi által Facebook, keresse meg a munkahely által Facebook példányát hello keresési mező használatával.</span><span class="sxs-lookup"><span data-stu-id="4f644-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using hello search field.</span></span> <span data-ttu-id="4f644-133">Máskülönben válassza **Hozzáadás** keresse meg a **által Facebook munkahelyi** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="4f644-133">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="4f644-134">Válassza ki **által Facebook munkahelyi** hello a keresési eredményekben, és adja hozzá tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="4f644-134">Select **Workplace by Facebook** from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="4f644-135">A munkahely által Facebook példányát, majd válassza ki és hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="4f644-135">Select your instance of Workplace by Facebook, and then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="4f644-136">Állítsa be **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="4f644-136">Set **Provisioning Mode** too**Automatic**.</span></span> 

    ![Képernyőfelvétel a munkahelyi által Facebook üzembe helyezési lehetőségek](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="4f644-138">A hello **rendszergazdai hitelesítő adataival** területen adja meg a hello **jogkivonat titkos kulcs** és hello **bérlői URL-cím** a munkahelyének Facebook-rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="4f644-138">Under hello **Admin Credentials** section, enter hello **Secret Token** and hello **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="4f644-139">Hello Azure-portálon, válassza ki **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak munkahelyi tooyour által Facebook-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f644-139">In hello Azure portal, select **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="4f644-140">Ha hello létesített kapcsolat megszakad, győződjön meg arról, hogy a munkahelyi Facebook-fiókkal Team rendszergazdai jogosultságokkal rendelkezik egy.</span><span class="sxs-lookup"><span data-stu-id="4f644-140">If hello connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="4f644-141">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be a hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="4f644-141">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello check box.</span></span>

8. <span data-ttu-id="4f644-142">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f644-142">Select **Save**.</span></span>

9. <span data-ttu-id="4f644-143">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooWorkplace által Facebook**.</span><span class="sxs-lookup"><span data-stu-id="4f644-143">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook**.</span></span>

10. <span data-ttu-id="4f644-144">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooWorkplace Facebook által szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="4f644-144">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="4f644-145">kiválasztott attribútumok hello **egyező** tulajdonságok: használt toomatch hello tartozó felhasználói fiókok által Facebook munkahelyi frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="4f644-145">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="4f644-146">toocommit a módosításokat, válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4f644-146">toocommit any changes, select **Save**.</span></span>

11. <span data-ttu-id="4f644-147">tooenable hello Azure AD létesítési szolgáltatás által a Facebook, a munkahelyi fiókhoz hello **beállítások** területen módosítsa a hello **kiépítési állapot** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="4f644-147">tooenable hello Azure AD provisioning service for Workplace by Facebook, in hello **Settings** section, change hello **Provisioning Status** too**On**.</span></span>

12. <span data-ttu-id="4f644-148">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f644-148">Select **Save**.</span></span>

<span data-ttu-id="4f644-149">További információt a tooconfigure automatikus kiépítés, lásd: [Facebook-dokumentáció hello](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="4f644-149">For more information on how tooconfigure automatic provisioning, see [hello Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="4f644-150">Mostantól létrehozhat egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="4f644-150">You can now create a test account.</span></span> <span data-ttu-id="4f644-151">Várjon, amíg fel hello fiókot töltött tooverify tooWorkplace Facebook által szinkronizált too20 perc.</span><span class="sxs-lookup"><span data-stu-id="4f644-151">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f644-152">További források</span><span class="sxs-lookup"><span data-stu-id="4f644-152">Additional resources</span></span>

* [<span data-ttu-id="4f644-153">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="4f644-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4f644-154">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4f644-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="4f644-155">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f644-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

