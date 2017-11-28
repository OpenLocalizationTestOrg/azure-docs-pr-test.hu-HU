---
title: "Oktatóanyag: GitHub konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooGitHub."
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
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a><span data-ttu-id="8cf4f-103">Oktatóanyag: GitHub konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="8cf4f-103">Tutorial: Configuring GitHub for Automatic User Provisioning</span></span>


<span data-ttu-id="8cf4f-104">hello Ez az oktatóanyag célja tooshow meg hello a Githubon és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooGitHub tooperform szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in GitHub and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGitHub.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8cf4f-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8cf4f-105">Prerequisites</span></span>

<span data-ttu-id="8cf4f-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8cf4f-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="8cf4f-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="8cf4f-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="8cf4f-108">A Github-bérlő a hello [üzleti terv](https://help.github.com/articles/organization-billing-plans/#business-plan) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="8cf4f-108">A Github tenant with hello [Business plan](https://help.github.com/articles/organization-billing-plans/#business-plan) or better enabled</span></span> 
*   <span data-ttu-id="8cf4f-109">A Githubon rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="8cf4f-109">A user account in GitHub with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="8cf4f-110">hello az Azure AD-integrációs kiépítés támaszkodik hello [GitHub SCIM API](https://developer.github.com/v3/scim/), ez az elérhető tooGithub csapatok hello üzleti terv vagy nagyobb.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-110">hello Azure AD provisioning integration relies on hello [GitHub SCIM API](https://developer.github.com/v3/scim/), which is available tooGithub teams on hello Business plan or better.</span></span>

## <a name="assigning-users-toogithub"></a><span data-ttu-id="8cf4f-111">Felhasználók tooGitHub hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8cf4f-111">Assigning users tooGitHub</span></span>

<span data-ttu-id="8cf4f-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="8cf4f-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="8cf4f-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour GitHub alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour GitHub app.</span></span> <span data-ttu-id="8cf4f-115">Ha úgy döntött, rendelheti hozzá ezen felhasználók tooyour GitHub app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="8cf4f-115">Once decided, you can assign these users tooyour GitHub app by following hello instructions here:</span></span>

[<span data-ttu-id="8cf4f-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8cf4f-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a><span data-ttu-id="8cf4f-117">Fontos tippek a felhasználók tooGitHub hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8cf4f-117">Important tips for assigning users tooGitHub</span></span>

*   <span data-ttu-id="8cf4f-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooGitHub tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-118">It is recommended that a single Azure AD user is assigned tooGitHub tootest hello provisioning configuration.</span></span> <span data-ttu-id="8cf4f-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="8cf4f-120">Amikor egy felhasználó tooGitHub rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-120">When assigning a user tooGitHub, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="8cf4f-121">Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toogithub"></a><span data-ttu-id="8cf4f-122">Felhasználók átadásához tooGitHub konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8cf4f-122">Configuring user provisioning tooGitHub</span></span> 

<span data-ttu-id="8cf4f-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooGitHub felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Githubon alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-123">This section guides you through connecting your Azure AD tooGitHub's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in GitHub based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="8cf4f-124">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a github webhelyen, hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8cf4f-124">You may also choose tooenabled SAML-based Single Sign-On for GitHub, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8cf4f-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a><span data-ttu-id="8cf4f-126">Az Azure AD-tooGitHub kiépítés automatikus felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="8cf4f-126">Configure automatic user account provisioning tooGitHub in Azure AD</span></span>


1. <span data-ttu-id="8cf4f-127">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="8cf4f-128">Ha már konfigurált GitHub egyszeri bejelentkezést, keresse meg az hello keresési mező GitHub-példány.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-128">If you have already configured GitHub for single sign-on, search for your instance of GitHub using hello search field.</span></span> <span data-ttu-id="8cf4f-129">Máskülönben válassza **Hozzáadás** keresse meg a **GitHub** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-129">Otherwise, select **Add** and search for **GitHub** in hello application gallery.</span></span> <span data-ttu-id="8cf4f-130">Válassza ki a Githubon hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-130">Select GitHub from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="8cf4f-131">Jelölje ki a Githubon példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-131">Select your instance of GitHub, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="8cf4f-132">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![GitHub-kiépítés](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. <span data-ttu-id="8cf4f-134">A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="8cf4f-135">Ez a művelet egy GitHub-engedélyezési párbeszédpanelt egy új böngészőablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-135">This operation opens a GitHub authorization dialog in a new browser window.</span></span> 

6. <span data-ttu-id="8cf4f-136">Hello új ablakban jelentkezzen be arra a Githubon a rendszergazda fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-136">In hello new window, sign into GitHub using your Admin account.</span></span> <span data-ttu-id="8cf4f-137">Hello eredményül kapott engedélyezési párbeszédpanelen válassza ki a hello GitHub csapatot szeretné, hogy tooenable kiépítést, és válassza ki **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-137">In hello resulting authorization dialog, select hello GitHub team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="8cf4f-138">Egyszer befejezett, visszatérési toohello az Azure portál toocomplete hello konfigurálása kiosztás.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

    ![Engedélyezési párbeszédpanel](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. <span data-ttu-id="8cf4f-140">Hello Azure-portálon, a bemeneti **bérlői URL-cím** kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour GitHub alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-140">In hello Azure portal, input **Tenant URL** and click **Test Connection** tooensure Azure AD can connect tooyour GitHub app.</span></span> <span data-ttu-id="8cf4f-141">Ha hello létesített kapcsolat megszakad, győződjön meg arról, a GitHub-fiók rendszergazdai jogosultságokkal rendelkezik, és **bérlői URL-cím** megfelelően van-e későbbinek, majd próbálja meg újból. "Engedélyezés" lépés hello (is jelentenek **bérlői URL-cím** szabály: "https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ", a szervezetek a GitHub-fiókjában található: **beállítások** > **szervezetek**).</span><span class="sxs-lookup"><span data-stu-id="8cf4f-141">If hello connection fails, ensure your GitHub account has Admin permissions and **Tenant URl** is inputted correctly, then try hello "Authorize" step again (you can constitute **Tenant URL** by rule: "https://api.github.com/scim/v2/organizations/ + <Organizations_name>", you can find your organizations under your GitHub account: **Settings** > **Organizations**).</span></span>

    ![Engedélyezési párbeszédpanel](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. <span data-ttu-id="8cf4f-143">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="8cf4f-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

9. <span data-ttu-id="8cf4f-144">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-144">Click **Save**.</span></span> 

10. <span data-ttu-id="8cf4f-145">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooGitHub**.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGitHub**.</span></span>

11. <span data-ttu-id="8cf4f-146">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooGitHub szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGitHub.</span></span> <span data-ttu-id="8cf4f-147">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat a Githubon a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in GitHub for update operations.</span></span> <span data-ttu-id="8cf4f-148">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-148">Select hello Save button toocommit any changes.</span></span>

12. <span data-ttu-id="8cf4f-149">tooenable hello Azure AD létesítési szolgáltatás a github webhelyen, a módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="8cf4f-149">tooenable hello Azure AD provisioning service for GitHub, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13. <span data-ttu-id="8cf4f-150">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-150">Click **Save**.</span></span> 

<span data-ttu-id="8cf4f-151">Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooGitHub a hello felhasználók és csoportok szakasz elindul.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-151">This operation starts hello initial synchronization of any users and/or groups assigned tooGitHub in hello Users and Groups section.</span></span> <span data-ttu-id="8cf4f-152">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-152">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="8cf4f-153">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="8cf4f-153">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="8cf4f-154">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="8cf4f-154">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8cf4f-155">További források</span><span class="sxs-lookup"><span data-stu-id="8cf4f-155">Additional resources</span></span>

* [<span data-ttu-id="8cf4f-156">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="8cf4f-156">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="8cf4f-157">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8cf4f-157">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="8cf4f-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cf4f-158">Next steps</span></span>

* [<span data-ttu-id="8cf4f-159">Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="8cf4f-159">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
