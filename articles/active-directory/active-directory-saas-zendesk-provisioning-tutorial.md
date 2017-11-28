---
title: "Oktatóanyag: ZenDesk konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooZenDesk."
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
ms.openlocfilehash: 200e8790ec1755f5cf927274ceb38527dd993f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a><span data-ttu-id="42ba2-103">Oktatóanyag: ZenDesk konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="42ba2-103">Tutorial: Configuring ZenDesk for Automatic User Provisioning</span></span>


<span data-ttu-id="42ba2-104">hello Ez az oktatóanyag célja tooshow meg hello tooperform ZenDesk és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooZenDesk a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="42ba2-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in ZenDesk and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooZenDesk.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="42ba2-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42ba2-105">Prerequisites</span></span>

<span data-ttu-id="42ba2-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="42ba2-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="42ba2-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="42ba2-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="42ba2-108">Egy ZenDesk bérlőt hello [vállalati terv](https://www.zendesk.com/product/pricing/) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="42ba2-108">A ZenDesk tenant with hello [Enterprise plan](https://www.zendesk.com/product/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="42ba2-109">A ZenDesk rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="42ba2-109">A user account in ZenDesk with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="42ba2-110">hello az Azure AD-integrációs kiépítés támaszkodik hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), ez az elérhető tooZenDesk csapatok hello alapvető terv vagy nagyobb.</span><span class="sxs-lookup"><span data-stu-id="42ba2-110">hello Azure AD provisioning integration relies on hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), which is available tooZenDesk teams on hello Essential plan or better.</span></span>

## <a name="assigning-users-toozendesk"></a><span data-ttu-id="42ba2-111">Felhasználók tooZenDesk hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="42ba2-111">Assigning users tooZenDesk</span></span>

<span data-ttu-id="42ba2-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="42ba2-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="42ba2-113">Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van.</span><span class="sxs-lookup"><span data-stu-id="42ba2-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="42ba2-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour ZenDesk alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="42ba2-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour ZenDesk app.</span></span> <span data-ttu-id="42ba2-115">Ha úgy döntött, rendelheti hozzá ezen felhasználók tooyour ZenDesk app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="42ba2-115">Once decided, you can assign these users tooyour ZenDesk app by following hello instructions here:</span></span>

[<span data-ttu-id="42ba2-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="42ba2-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toozendesk"></a><span data-ttu-id="42ba2-117">Fontos tippek a felhasználók tooZenDesk hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="42ba2-117">Important tips for assigning users tooZenDesk</span></span>

*   <span data-ttu-id="42ba2-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooZenDesk tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="42ba2-118">It is recommended that a single Azure AD user is assigned tooZenDesk tootest hello provisioning configuration.</span></span> <span data-ttu-id="42ba2-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="42ba2-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="42ba2-120">Amikor egy felhasználó tooZenDesk rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="42ba2-120">When assigning a user tooZenDesk, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="42ba2-121">Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="42ba2-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="42ba2-122">Hozzáadott szolgáltatás hello szolgáltatás kiépítését Zendesk definiált egyéni szerepköröket olvas, és importálja azokat az Azure AD, ha azok is kijelölhető hello szerepkör kiválasztása párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42ba2-122">As an added feature, hello provisioning service reads any custom roles defined in Zendesk, and imports them into Azure AD where they can be selected in hello Select Role dialog.</span></span> <span data-ttu-id="42ba2-123">Ezek a szerepkörök fog megjelenni a hello Azure-portálon hello szolgáltatás kiépítését engedélyezve van, és egy szinkronizálási ciklust befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="42ba2-123">These roles will be visible in hello Azure portal after hello provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-toozendesk"></a><span data-ttu-id="42ba2-124">Felhasználók átadásához tooZenDesk konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42ba2-124">Configuring user provisioning tooZenDesk</span></span> 

<span data-ttu-id="42ba2-125">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooZenDesk felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a ZenDesk alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="42ba2-125">This section guides you through connecting your Azure AD tooZenDesk's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in ZenDesk based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="42ba2-126">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a ZenDesk hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="42ba2-126">You may also choose tooenabled SAML-based Single Sign-On for ZenDesk, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="42ba2-127">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="42ba2-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toozendesk-in-azure-ad"></a><span data-ttu-id="42ba2-128">Az Azure AD-tooZenDesk kiépítés automatikus felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="42ba2-128">Configure automatic user account provisioning tooZenDesk in Azure AD</span></span>


1. <span data-ttu-id="42ba2-129">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="42ba2-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="42ba2-130">Ha már konfigurált ZenDesk egyszeri bejelentkezést, keresse meg az hello keresési mező ZenDesk-példány.</span><span class="sxs-lookup"><span data-stu-id="42ba2-130">If you have already configured ZenDesk for single sign-on, search for your instance of ZenDesk using hello search field.</span></span> <span data-ttu-id="42ba2-131">Máskülönben válassza **Hozzáadás** keresse meg a **ZenDesk** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="42ba2-131">Otherwise, select **Add** and search for **ZenDesk** in hello application gallery.</span></span> <span data-ttu-id="42ba2-132">Válassza ki a ZenDesk hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="42ba2-132">Select ZenDesk from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="42ba2-133">Válassza ki az ehhez a példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="42ba2-133">Select your instance of ZenDesk, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="42ba2-134">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="42ba2-134">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![ZenDesk kiépítése](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. <span data-ttu-id="42ba2-136">Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **rendszergazda felhasználóneve & tokenkey & tartomány** állítja elő a ZenDesk-fiókja (hello jogkivonat található a fiókjában: **rendszergazda**   >  **API** > **beállítások**).</span><span class="sxs-lookup"><span data-stu-id="42ba2-136">Under hello **Admin Credentials** section, input hello **Admin Username&tokenkey&Domain** generated by your ZenDesk's account (you can find hello token under your account: **Admin** > **API** > **Settings**).</span></span> 

    ![ZenDesk kiépítése](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. <span data-ttu-id="42ba2-138">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour ZenDesk alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="42ba2-138">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour ZenDesk app.</span></span> <span data-ttu-id="42ba2-139">Ha hello létesített kapcsolat megszakad, győződjön meg arról, ZenDesk-fiókja rendszergazdai jogosultságokkal rendelkezik, és ismételje meg 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="42ba2-139">If hello connection fails, ensure your ZenDesk account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="42ba2-140">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="42ba2-140">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="42ba2-141">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42ba2-141">Click **Save**.</span></span> 

9. <span data-ttu-id="42ba2-142">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooZenDesk**.</span><span class="sxs-lookup"><span data-stu-id="42ba2-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooZenDesk**.</span></span>

10. <span data-ttu-id="42ba2-143">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooZenDesk szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="42ba2-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooZenDesk.</span></span> <span data-ttu-id="42ba2-144">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat ehhez a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="42ba2-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in ZenDesk for update operations.</span></span> <span data-ttu-id="42ba2-145">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="42ba2-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="42ba2-146">tooenable hello Azure AD létesítési szolgáltatás a ZenDesk, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="42ba2-146">tooenable hello Azure AD provisioning service for ZenDesk, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="42ba2-147">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42ba2-147">Click **Save**.</span></span> 

<span data-ttu-id="42ba2-148">Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooZenDesk a hello felhasználók és csoportok szakasz elindul.</span><span class="sxs-lookup"><span data-stu-id="42ba2-148">This operation starts hello initial synchronization of any users and/or groups assigned tooZenDesk in hello Users and Groups section.</span></span> <span data-ttu-id="42ba2-149">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="42ba2-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="42ba2-150">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="42ba2-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="42ba2-151">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="42ba2-151">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="42ba2-152">További források</span><span class="sxs-lookup"><span data-stu-id="42ba2-152">Additional resources</span></span>

* [<span data-ttu-id="42ba2-153">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="42ba2-153">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="42ba2-154">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="42ba2-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="42ba2-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42ba2-155">Next steps</span></span>

* [<span data-ttu-id="42ba2-156">Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="42ba2-156">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
