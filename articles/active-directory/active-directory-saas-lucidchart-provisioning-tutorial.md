---
title: "Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooLucidChart."
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
ms.openlocfilehash: d3af45141731215f2edc8942ad21b016468c1e38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="76d8e-103">Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="76d8e-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="76d8e-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform LucidChart és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooLucidChart a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="76d8e-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LucidChart and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="76d8e-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="76d8e-105">Prerequisites</span></span>

<span data-ttu-id="76d8e-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="76d8e-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="76d8e-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="76d8e-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="76d8e-108">Egy LucidChart bérlőt hello [vállalati terv](https://www.lucidchart.com/user/117598685#/subscriptionLevel) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="76d8e-108">A LucidChart tenant with hello [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="76d8e-109">A LucidChart rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="76d8e-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-toolucidchart"></a><span data-ttu-id="76d8e-110">Felhasználók tooLucidChart hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="76d8e-110">Assigning users tooLucidChart</span></span>

<span data-ttu-id="76d8e-111">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="76d8e-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="76d8e-112">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="76d8e-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="76d8e-113">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour LucidChart alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="76d8e-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour LucidChart app.</span></span> <span data-ttu-id="76d8e-114">Ha úgy döntött, hozzárendelheti a felhasználók tooyour LucidChart app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="76d8e-114">Once decided, you can assign these users tooyour LucidChart app by following hello instructions here:</span></span>

[<span data-ttu-id="76d8e-115">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="76d8e-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolucidchart"></a><span data-ttu-id="76d8e-116">Fontos tippek a felhasználók tooLucidChart hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="76d8e-116">Important tips for assigning users tooLucidChart</span></span>

*   <span data-ttu-id="76d8e-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooLucidChart tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="76d8e-117">It is recommended that a single Azure AD user is assigned tooLucidChart tootest hello provisioning configuration.</span></span> <span data-ttu-id="76d8e-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="76d8e-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="76d8e-119">Amikor egy felhasználó tooLucidChart rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="76d8e-119">When assigning a user tooLucidChart, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="76d8e-120">Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="76d8e-120">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toolucidchart"></a><span data-ttu-id="76d8e-121">Felhasználók átadásához tooLucidChart konfigurálása</span><span class="sxs-lookup"><span data-stu-id="76d8e-121">Configuring user provisioning tooLucidChart</span></span> 

<span data-ttu-id="76d8e-122">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooLucidChart felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú LucidChart hozzárendelt felhasználói fiókok .</span><span class="sxs-lookup"><span data-stu-id="76d8e-122">This section guides you through connecting your Azure AD tooLucidChart's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="76d8e-123">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a LucidChart hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="76d8e-123">You may also choose tooenabled SAML-based Single Sign-On for LucidChart, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="76d8e-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="76d8e-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toolucidchart-in-azure-ad"></a><span data-ttu-id="76d8e-125">Az Azure AD-tooLucidChart kiépítés automatikus felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="76d8e-125">Configure automatic user account provisioning tooLucidChart in Azure AD</span></span>


1. <span data-ttu-id="76d8e-126">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="76d8e-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="76d8e-127">Ha már konfigurált LucidChart egyszeri bejelentkezést, keresse meg a hello keresési mező LucidChart példányát.</span><span class="sxs-lookup"><span data-stu-id="76d8e-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using hello search field.</span></span> <span data-ttu-id="76d8e-128">Máskülönben válassza **Hozzáadás** keresse meg a **LucidChart** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="76d8e-128">Otherwise, select **Add** and search for **LucidChart** in hello application gallery.</span></span> <span data-ttu-id="76d8e-129">Válassza ki a LucidChart hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="76d8e-129">Select LucidChart from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="76d8e-130">Jelölje ki a LucidChart példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="76d8e-130">Select your instance of LucidChart, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="76d8e-131">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="76d8e-131">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="76d8e-133">Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **titkos Token** állítja elő a LucidChart fiók (hello jogkivonat található a fiókjában: **Team**  >  **Alkalmazásintegráció** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="76d8e-133">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your LucidChart's account (you can find hello token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="76d8e-135">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour LucidChart alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="76d8e-135">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour LucidChart app.</span></span> <span data-ttu-id="76d8e-136">Hello létesített kapcsolat megszakad, ha győződjön meg arról, LucidChart fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="76d8e-136">If hello connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="76d8e-137">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="76d8e-137">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="76d8e-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="76d8e-138">Click **Save**.</span></span> 

9. <span data-ttu-id="76d8e-139">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooLucidChart**.</span><span class="sxs-lookup"><span data-stu-id="76d8e-139">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooLucidChart**.</span></span>

10. <span data-ttu-id="76d8e-140">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooLucidChart szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="76d8e-140">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooLucidChart.</span></span> <span data-ttu-id="76d8e-141">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok LucidChart a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="76d8e-141">hello attributes selected as **Matching** properties are used toomatch hello user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="76d8e-142">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="76d8e-142">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="76d8e-143">tooenable hello Azure AD LucidChart, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="76d8e-143">tooenable hello Azure AD provisioning service for LucidChart, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="76d8e-144">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="76d8e-144">Click **Save**.</span></span> 

<span data-ttu-id="76d8e-145">Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooLucidChart a hello felhasználók és csoportok szakasz elindul.</span><span class="sxs-lookup"><span data-stu-id="76d8e-145">This operation starts hello initial synchronization of any users and/or groups assigned tooLucidChart in hello Users and Groups section.</span></span> <span data-ttu-id="76d8e-146">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="76d8e-146">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="76d8e-147">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="76d8e-147">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="76d8e-148">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="76d8e-148">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="76d8e-149">További források</span><span class="sxs-lookup"><span data-stu-id="76d8e-149">Additional resources</span></span>

* [<span data-ttu-id="76d8e-150">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="76d8e-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="76d8e-151">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="76d8e-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="76d8e-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76d8e-152">Next steps</span></span>

* [<span data-ttu-id="76d8e-153">Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="76d8e-153">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
