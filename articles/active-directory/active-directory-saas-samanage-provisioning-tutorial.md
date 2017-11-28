---
title: "Oktatóanyag: Samanage konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooSamanage."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 6cb36d2cc6ce33da4f8ebba65d138bfd4f2aca9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a><span data-ttu-id="6d503-103">Oktatóanyag: Samanage konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="6d503-103">Tutorial: Configuring Samanage for Automatic User Provisioning</span></span>


<span data-ttu-id="6d503-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform Samanage és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSamanage a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6d503-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Samanage and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSamanage.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6d503-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6d503-105">Prerequisites</span></span>

<span data-ttu-id="6d503-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6d503-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="6d503-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="6d503-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="6d503-108">Egy Samanage bérlőt hello [szakmai terv](https://www.samanage.com/pricing/) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="6d503-108">A Samanage tenant with hello [Professional plan](https://www.samanage.com/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="6d503-109">A Samanage rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="6d503-109">A user account in Samanage with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="6d503-110">hello az Azure AD-integrációs kiépítés támaszkodik hello [Samanage REST API](https://www.samanage.com/api/), amely a hello Professional elérhető tooSamanage csoportok tervezése vagy jobb.</span><span class="sxs-lookup"><span data-stu-id="6d503-110">hello Azure AD provisioning integration relies on hello [Samanage REST API](https://www.samanage.com/api/), which is available tooSamanage teams on hello Professional plan or better.</span></span>

## <a name="assigning-users-toosamanage"></a><span data-ttu-id="6d503-111">Felhasználók tooSamanage hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6d503-111">Assigning users tooSamanage</span></span>

<span data-ttu-id="6d503-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="6d503-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="6d503-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="6d503-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="6d503-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Samanage alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="6d503-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Samanage app.</span></span> <span data-ttu-id="6d503-115">Ha úgy döntött, hozzárendelheti a felhasználók tooyour Samanage app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="6d503-115">Once decided, you can assign these users tooyour Samanage app by following hello instructions here:</span></span>

[<span data-ttu-id="6d503-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="6d503-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toosamanage"></a><span data-ttu-id="6d503-117">Fontos tippek a felhasználók tooSamanage hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6d503-117">Important tips for assigning users tooSamanage</span></span>

*   <span data-ttu-id="6d503-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSamanage tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="6d503-118">It is recommended that a single Azure AD user is assigned tooSamanage tootest hello provisioning configuration.</span></span> <span data-ttu-id="6d503-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="6d503-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="6d503-120">Amikor egy felhasználó tooSamanage rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="6d503-120">When assigning a user tooSamanage, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="6d503-121">Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="6d503-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="6d503-122">Hozzáadott szolgáltatás hello szolgáltatás kiépítését Samanage definiált egyéni szerepköröket olvas, és importálja azokat az Azure AD, ha azok is kijelölhető hello szerepkör kiválasztása párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6d503-122">As an added feature, hello provisioning service reads any custom roles defined in Samanage, and imports them into Azure AD where they can be selected in hello Select Role dialog.</span></span> <span data-ttu-id="6d503-123">Ezek a szerepkörök fog megjelenni a hello Azure-portálon hello szolgáltatás kiépítését engedélyezve van, és egy szinkronizálási ciklust befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="6d503-123">These roles will be visible in hello Azure portal after hello provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-toosamanage"></a><span data-ttu-id="6d503-124">Felhasználók átadásához tooSamanage konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d503-124">Configuring user provisioning tooSamanage</span></span> 

<span data-ttu-id="6d503-125">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSamanage felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Samanage alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6d503-125">This section guides you through connecting your Azure AD tooSamanage's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Samanage based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="6d503-126">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Samanage hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6d503-126">You may also choose tooenabled SAML-based Single Sign-On for Samanage, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6d503-127">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="6d503-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toosamanage-in-azure-ad"></a><span data-ttu-id="6d503-128">Az Azure AD-tooSamanage kiépítés automatikus felhasználói fiók beállítása:</span><span class="sxs-lookup"><span data-stu-id="6d503-128">Configure automatic user account provisioning tooSamanage in Azure AD:</span></span>


1. <span data-ttu-id="6d503-129">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6d503-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="6d503-130">Ha már konfigurált Samanage egyszeri bejelentkezést, keresse meg a hello keresési mező Samanage példányát.</span><span class="sxs-lookup"><span data-stu-id="6d503-130">If you have already configured Samanage for single sign-on, search for your instance of Samanage using hello search field.</span></span> <span data-ttu-id="6d503-131">Máskülönben válassza **Hozzáadás** keresse meg a **Samanage** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="6d503-131">Otherwise, select **Add** and search for **Samanage** in hello application gallery.</span></span> <span data-ttu-id="6d503-132">Válassza ki a Samanage hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="6d503-132">Select Samanage from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="6d503-133">Jelölje ki a Samanage példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="6d503-133">Select your instance of Samanage, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="6d503-134">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="6d503-134">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Samanage kiépítése](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. <span data-ttu-id="6d503-136">A hello **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **rendszergazda felhasználónevét és a rendszergazdai jelszó** a Samanage fiók.</span><span class="sxs-lookup"><span data-stu-id="6d503-136">Under hello **Admin Credentials** section, input hello **Admin Username&Admin Password** of your Samanage's account.</span></span> 

6. <span data-ttu-id="6d503-137">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Samanage alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="6d503-137">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Samanage app.</span></span> <span data-ttu-id="6d503-138">Hello létesített kapcsolat megszakad, ha győződjön meg arról, Samanage fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="6d503-138">If hello connection fails, ensure your Samanage account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="6d503-139">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="6d503-139">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="6d503-140">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6d503-140">Click **Save**.</span></span> 

9. <span data-ttu-id="6d503-141">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSamanage**.</span><span class="sxs-lookup"><span data-stu-id="6d503-141">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSamanage**.</span></span>

10. <span data-ttu-id="6d503-142">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooSamanage szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="6d503-142">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSamanage.</span></span> <span data-ttu-id="6d503-143">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Samanage a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="6d503-143">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Samanage for update operations.</span></span> <span data-ttu-id="6d503-144">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6d503-144">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="6d503-145">tooenable hello Azure AD Samanage, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="6d503-145">tooenable hello Azure AD provisioning service for Samanage, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="6d503-146">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6d503-146">Click **Save**.</span></span> 

<span data-ttu-id="6d503-147">Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooSamanage a hello felhasználók és csoportok szakasz elindul.</span><span class="sxs-lookup"><span data-stu-id="6d503-147">This operation starts hello initial synchronization of any users and/or groups assigned tooSamanage in hello Users and Groups section.</span></span> <span data-ttu-id="6d503-148">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6d503-148">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="6d503-149">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="6d503-149">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="6d503-150">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="6d503-150">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="6d503-151">További források</span><span class="sxs-lookup"><span data-stu-id="6d503-151">Additional resources</span></span>

* [<span data-ttu-id="6d503-152">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="6d503-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="6d503-153">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6d503-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="6d503-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d503-154">Next steps</span></span>

* [<span data-ttu-id="6d503-155">Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="6d503-155">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
