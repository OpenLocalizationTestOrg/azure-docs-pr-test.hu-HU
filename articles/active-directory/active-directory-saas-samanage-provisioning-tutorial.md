---
title: "Oktatóanyag: Samanage konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja az Azure Active Directory automatikusan ellátásához, majd leépíti Samanage felhasználói fiókokat."
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
ms.openlocfilehash: 278ebf464fbe815568fbe332f80d5ea6b29e1811
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a><span data-ttu-id="20c9f-103">Oktatóanyag: Samanage konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="20c9f-103">Tutorial: Configuring Samanage for Automatic User Provisioning</span></span>


<span data-ttu-id="20c9f-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a Samanage és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Samanage mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="20c9f-104">The objective of this tutorial is to show you the steps you need to perform in Samanage and Azure AD to automatically provision and de-provision user accounts from Azure AD to Samanage.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="20c9f-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="20c9f-105">Prerequisites</span></span>

<span data-ttu-id="20c9f-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="20c9f-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="20c9f-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="20c9f-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="20c9f-108">Egy Samanage bérlőt a [szakmai terv](https://www.samanage.com/pricing/) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="20c9f-108">A Samanage tenant with the [Professional plan](https://www.samanage.com/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="20c9f-109">A Samanage rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="20c9f-109">A user account in Samanage with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="20c9f-110">Az Azure AD-integrációs kiépítés támaszkodik a [Samanage REST API](https://www.samanage.com/api/), a Professional terven Samanage csoportokhoz érhető el vagy nagyobb.</span><span class="sxs-lookup"><span data-stu-id="20c9f-110">The Azure AD provisioning integration relies on the [Samanage REST API](https://www.samanage.com/api/), which is available to Samanage teams on the Professional plan or better.</span></span>

## <a name="assigning-users-to-samanage"></a><span data-ttu-id="20c9f-111">Felhasználók hozzárendelése Samanage</span><span class="sxs-lookup"><span data-stu-id="20c9f-111">Assigning users to Samanage</span></span>

<span data-ttu-id="20c9f-112">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="20c9f-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="20c9f-113">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="20c9f-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="20c9f-114">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az Samanage alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="20c9f-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Samanage app.</span></span> <span data-ttu-id="20c9f-115">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az Samanage alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="20c9f-115">Once decided, you can assign these users to your Samanage app by following the instructions here:</span></span>

[<span data-ttu-id="20c9f-116">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="20c9f-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a><span data-ttu-id="20c9f-117">Felhasználók hozzárendelése Samanage fontos tippek</span><span class="sxs-lookup"><span data-stu-id="20c9f-117">Important tips for assigning users to Samanage</span></span>

*   <span data-ttu-id="20c9f-118">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve Samanage teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="20c9f-118">It is recommended that a single Azure AD user is assigned to Samanage to test the provisioning configuration.</span></span> <span data-ttu-id="20c9f-119">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="20c9f-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="20c9f-120">Ha egy felhasználó hozzárendelése Samanage, kell-e ki lehet a **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) a hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="20c9f-120">When assigning a user to Samanage, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="20c9f-121">A **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="20c9f-121">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="20c9f-122">Hozzáadott szolgáltatás a létesítési szolgáltatás beolvassa a Samanage definiált egyéni szerepköröket, és importálja azokat az Azure AD, ha azok választható ki a szerepkör kiválasztása párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20c9f-122">As an added feature, the provisioning service reads any custom roles defined in Samanage, and imports them into Azure AD where they can be selected in the Select Role dialog.</span></span> <span data-ttu-id="20c9f-123">Ezek a szerepkörök lesz látható az Azure portálon, a létesítési szolgáltatás engedélyezve van, és egy szinkronizálási ciklust befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="20c9f-123">These roles will be visible in the Azure portal after the provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-to-samanage"></a><span data-ttu-id="20c9f-124">A felhasználók átadása a Samanage konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20c9f-124">Configuring user provisioning to Samanage</span></span> 

<span data-ttu-id="20c9f-125">Ez a szakasz végigvezeti az Azure AD kapcsolódás Samanage a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú Samanage hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="20c9f-125">This section guides you through connecting your Azure AD to Samanage's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Samanage based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="20c9f-126">Dönthet úgy is, SAML-alapú egyszeri bejelentkezést Samanage engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="20c9f-126">You may also choose to enabled SAML-based Single Sign-On for Samanage, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="20c9f-127">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="20c9f-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-samanage-in-azure-ad"></a><span data-ttu-id="20c9f-128">Konfigurálja az automatikus felhasználói fiók kiépítése Samanage az Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="20c9f-128">Configure automatic user account provisioning to Samanage in Azure AD:</span></span>


1. <span data-ttu-id="20c9f-129">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="20c9f-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="20c9f-130">Ha már konfigurált Samanage egyszeri bejelentkezést, keresse meg a keresési mező Samanage példányát.</span><span class="sxs-lookup"><span data-stu-id="20c9f-130">If you have already configured Samanage for single sign-on, search for your instance of Samanage using the search field.</span></span> <span data-ttu-id="20c9f-131">Máskülönben válassza **Hozzáadás** keresse meg a **Samanage** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="20c9f-131">Otherwise, select **Add** and search for **Samanage** in the application gallery.</span></span> <span data-ttu-id="20c9f-132">Válassza ki a Samanage a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="20c9f-132">Select Samanage from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="20c9f-133">Jelölje ki a Samanage példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="20c9f-133">Select your instance of Samanage, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="20c9f-134">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="20c9f-134">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![Samanage kiépítése](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. <span data-ttu-id="20c9f-136">Az a **rendszergazdai hitelesítő adataival** területen adjon meg a **rendszergazda felhasználónevét és a rendszergazdai jelszó** a Samanage fiók.</span><span class="sxs-lookup"><span data-stu-id="20c9f-136">Under the **Admin Credentials** section, input the **Admin Username&Admin Password** of your Samanage's account.</span></span> 

6. <span data-ttu-id="20c9f-137">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az Samanage alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="20c9f-137">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Samanage app.</span></span> <span data-ttu-id="20c9f-138">Ha nem sikerül, ellenőrizze a Samanage fiókja rendszergazdai jogosultságokkal rendelkezik, és ismételje meg 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="20c9f-138">If the connection fails, ensure your Samanage account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="20c9f-139">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet a "E-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="20c9f-139">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="20c9f-140">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="20c9f-140">Click **Save**.</span></span> 

9. <span data-ttu-id="20c9f-141">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat Samanage**.</span><span class="sxs-lookup"><span data-stu-id="20c9f-141">Under the Mappings section, select **Synchronize Azure Active Directory Users to Samanage**.</span></span>

10. <span data-ttu-id="20c9f-142">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD Samanage lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="20c9f-142">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Samanage.</span></span> <span data-ttu-id="20c9f-143">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Samanage a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="20c9f-143">The attributes selected as **Matching** properties are used to match the user accounts in Samanage for update operations.</span></span> <span data-ttu-id="20c9f-144">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="20c9f-144">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="20c9f-145">Az Azure AD szolgáltatás Samanage kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="20c9f-145">To enable the Azure AD provisioning service for Samanage, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="20c9f-146">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="20c9f-146">Click **Save**.</span></span> 

<span data-ttu-id="20c9f-147">Ez a művelet bármilyen felhasználói és/vagy a felhasználók és csoportok szakaszban Samanage rendelt csoportok a kezdeti szinkronizálás elindul.</span><span class="sxs-lookup"><span data-stu-id="20c9f-147">This operation starts the initial synchronization of any users and/or groups assigned to Samanage in the Users and Groups section.</span></span> <span data-ttu-id="20c9f-148">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="20c9f-148">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="20c9f-149">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amelyek a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="20c9f-149">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="20c9f-150">Olvassa el az Azure AD-naplók kiépítés módjáról további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="20c9f-150">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="20c9f-151">További források</span><span class="sxs-lookup"><span data-stu-id="20c9f-151">Additional resources</span></span>

* [<span data-ttu-id="20c9f-152">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="20c9f-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="20c9f-153">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="20c9f-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="20c9f-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20c9f-154">Next steps</span></span>

* [<span data-ttu-id="20c9f-155">Ismerje meg, tekintse át a naplók és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="20c9f-155">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
