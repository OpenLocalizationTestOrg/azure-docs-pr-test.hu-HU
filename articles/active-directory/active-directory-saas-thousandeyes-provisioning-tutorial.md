---
title: "Oktatóanyag: ThousandEyes konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooThousandEyes."
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
ms.openlocfilehash: f31883ab685d0ffcd9a830aa4a7d43c056f5f4cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a><span data-ttu-id="70795-103">Oktatóanyag: ThousandEyes konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="70795-103">Tutorial: Configuring ThousandEyes for Automatic User Provisioning</span></span>


<span data-ttu-id="70795-104">hello Ez az oktatóanyag célja meg hello lépéseket kell ThousandEyes és az Azure AD tooautomatically rendelkezés tooperform és leépíti a felhasználói fiókok Azure AD tooThousandEyes tooshow.</span><span class="sxs-lookup"><span data-stu-id="70795-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in ThousandEyes and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooThousandEyes.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="70795-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="70795-105">Prerequisites</span></span>

<span data-ttu-id="70795-106">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="70795-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="70795-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="70795-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="70795-108">Egy ThousandEyes bérlőt hello [Standard csomag](https://www.thousandeyes.com/pricing) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="70795-108">A ThousandEyes tenant with hello [Standard plan](https://www.thousandeyes.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="70795-109">A ThousandEyes rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="70795-109">A user account in ThousandEyes with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="70795-110">hello az Azure AD-integrációs kiépítés támaszkodik hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), ez az elérhető tooThousandEyes csapatok hello Standard csomag vagy nagyobb.</span><span class="sxs-lookup"><span data-stu-id="70795-110">hello Azure AD provisioning integration relies on hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), which is available tooThousandEyes teams on hello Standard plan or better.</span></span>

## <a name="assigning-users-toothousandeyes"></a><span data-ttu-id="70795-111">Felhasználók tooThousandEyes hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70795-111">Assigning users tooThousandEyes</span></span>

<span data-ttu-id="70795-112">Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja.</span><span class="sxs-lookup"><span data-stu-id="70795-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="70795-113">Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="70795-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="70795-114">Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour ThousandEyes alkalmazást kell használni az Azure AD jelentik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="70795-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour ThousandEyes app.</span></span> <span data-ttu-id="70795-115">Ha úgy döntött, hozzárendelheti a felhasználók tooyour ThousandEyes app itt hello utasításokat követve:</span><span class="sxs-lookup"><span data-stu-id="70795-115">Once decided, you can assign these users tooyour ThousandEyes app by following hello instructions here:</span></span>

[<span data-ttu-id="70795-116">Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="70795-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toothousandeyes"></a><span data-ttu-id="70795-117">Fontos tippek a felhasználók tooThousandEyes hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70795-117">Important tips for assigning users tooThousandEyes</span></span>

*   <span data-ttu-id="70795-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó tooThousandEyes tootest hello konfigurálása kiosztás van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="70795-118">It is recommended that a single Azure AD user is assigned tooThousandEyes tootest hello provisioning configuration.</span></span> <span data-ttu-id="70795-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="70795-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="70795-120">Amikor egy felhasználó tooThousandEyes rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="70795-120">When assigning a user tooThousandEyes, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="70795-121">Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="70795-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toothousandeyes"></a><span data-ttu-id="70795-122">Felhasználók átadásához tooThousandEyes konfigurálása</span><span class="sxs-lookup"><span data-stu-id="70795-122">Configuring user provisioning tooThousandEyes</span></span> 

<span data-ttu-id="70795-123">Ez a szakasz végigvezeti a csatlakozás az Azure AD tooThousandEyes felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a felhasználó és csoport-hozzárendelést alapján ThousandEyes hozzárendelt felhasználói fiókok Az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70795-123">This section guides you through connecting your Azure AD tooThousandEyes's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in ThousandEyes based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="70795-124">Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a ThousandEyes hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="70795-124">You may also choose tooenabled SAML-based Single Sign-On for ThousandEyes, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="70795-125">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="70795-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toothousandeyes-in-azure-ad"></a><span data-ttu-id="70795-126">Az Azure AD-tooThousandEyes kiépítés automatikus felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="70795-126">Configure automatic user account provisioning tooThousandEyes in Azure AD</span></span>


1. <span data-ttu-id="70795-127">A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="70795-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="70795-128">Ha már konfigurált ThousandEyes egyszeri bejelentkezést, keresse meg a hello keresési mező ThousandEyes példányát.</span><span class="sxs-lookup"><span data-stu-id="70795-128">If you have already configured ThousandEyes for single sign-on, search for your instance of ThousandEyes using hello search field.</span></span> <span data-ttu-id="70795-129">Máskülönben válassza **Hozzáadás** keresse meg a **ThousandEyes** hello alkalmazás gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="70795-129">Otherwise, select **Add** and search for **ThousandEyes** in hello application gallery.</span></span> <span data-ttu-id="70795-130">Válassza ki a ThousandEyes hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="70795-130">Select ThousandEyes from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="70795-131">Jelölje ki a ThousandEyes példányát, majd jelölje ki a hello **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="70795-131">Select your instance of ThousandEyes, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="70795-132">Set hello **kiépítési üzemmódban** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="70795-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Kiépítés ThousandEyes](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. <span data-ttu-id="70795-134">Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **titkos Token** a ThousandEyes fiók által generált (hello jogkivonat a ThousandEyes fiókjában található: **biztonsági & Hitelesítési**).</span><span class="sxs-lookup"><span data-stu-id="70795-134">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your ThousandEyes's account (you can find hello token under your ThousandEyes account: **Security & Authentication**).</span></span> 

    ![Kiépítés ThousandEyes](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. <span data-ttu-id="70795-136">Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour ThousandEyes alkalmazás képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="70795-136">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour ThousandEyes app.</span></span> <span data-ttu-id="70795-137">Hello létesített kapcsolat megszakad, ha győződjön meg arról, ThousandEyes fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="70795-137">If hello connection fails, ensure your ThousandEyes account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="70795-138">Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="70795-138">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="70795-139">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="70795-139">Click **Save**.</span></span> 

9. <span data-ttu-id="70795-140">A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="70795-140">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooThousandEyes**.</span></span>

10. <span data-ttu-id="70795-141">A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooThousandEyes szinkronizált hello felhasználói attribútumok.</span><span class="sxs-lookup"><span data-stu-id="70795-141">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooThousandEyes.</span></span> <span data-ttu-id="70795-142">kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok ThousandEyes a frissítési műveletekben.</span><span class="sxs-lookup"><span data-stu-id="70795-142">hello attributes selected as **Matching** properties are used toomatch hello user accounts in ThousandEyes for update operations.</span></span> <span data-ttu-id="70795-143">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="70795-143">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="70795-144">tooenable hello Azure AD ThousandEyes, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="70795-144">tooenable hello Azure AD provisioning service for ThousandEyes, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="70795-145">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="70795-145">Click **Save**.</span></span> 

<span data-ttu-id="70795-146">Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooThousandEyes a hello felhasználók és csoportok szakasz elindul.</span><span class="sxs-lookup"><span data-stu-id="70795-146">This operation starts hello initial synchronization of any users and/or groups assigned tooThousandEyes in hello Users and Groups section.</span></span> <span data-ttu-id="70795-147">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="70795-147">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="70795-148">Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.</span><span class="sxs-lookup"><span data-stu-id="70795-148">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="70795-149">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="70795-149">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="70795-150">További források</span><span class="sxs-lookup"><span data-stu-id="70795-150">Additional resources</span></span>

* [<span data-ttu-id="70795-151">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="70795-151">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="70795-152">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="70795-152">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="70795-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70795-153">Next steps</span></span>

* [<span data-ttu-id="70795-154">Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="70795-154">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
