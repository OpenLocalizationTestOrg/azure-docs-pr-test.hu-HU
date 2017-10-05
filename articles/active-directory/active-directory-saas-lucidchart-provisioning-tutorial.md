---
title: "Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja az Azure Active Directory automatikusan ellátásához, majd leépíti LucidChart felhasználói fiókokat."
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
ms.openlocfilehash: 1f9344a5e750360e21ed7dc8e3ed013c2c2e1a45
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="55117-103">Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó létesítése</span><span class="sxs-lookup"><span data-stu-id="55117-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="55117-104">Ez az oktatóanyag célja a lépéseket kell elvégeznie a LucidChart és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-LucidChart mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="55117-104">The objective of this tutorial is to show you the steps you need to perform in LucidChart and Azure AD to automatically provision and de-provision user accounts from Azure AD to LucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="55117-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55117-105">Prerequisites</span></span>

<span data-ttu-id="55117-106">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="55117-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="55117-107">Az Azure Active directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="55117-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="55117-108">Egy LucidChart bérlőt a [vállalati terv](https://www.lucidchart.com/user/117598685#/subscriptionLevel) vagy jobban engedélyezve</span><span class="sxs-lookup"><span data-stu-id="55117-108">A LucidChart tenant with the [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="55117-109">A LucidChart rendszergazdai jogosultságokkal rendelkező felhasználói fiókot</span><span class="sxs-lookup"><span data-stu-id="55117-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-to-lucidchart"></a><span data-ttu-id="55117-110">Felhasználók hozzárendelése LucidChart</span><span class="sxs-lookup"><span data-stu-id="55117-110">Assigning users to LucidChart</span></span>

<span data-ttu-id="55117-111">Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="55117-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="55117-112">Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="55117-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="55117-113">A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik az LucidChart alkalmazásához való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="55117-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your LucidChart app.</span></span> <span data-ttu-id="55117-114">Ha úgy döntött, itt cikk utasításait követve hozzárendelheti ezeket a felhasználókat az LucidChart alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="55117-114">Once decided, you can assign these users to your LucidChart app by following the instructions here:</span></span>

[<span data-ttu-id="55117-115">Egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás</span><span class="sxs-lookup"><span data-stu-id="55117-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a><span data-ttu-id="55117-116">Felhasználók hozzárendelése LucidChart fontos tippek</span><span class="sxs-lookup"><span data-stu-id="55117-116">Important tips for assigning users to LucidChart</span></span>

*   <span data-ttu-id="55117-117">Javasoljuk, hogy egyetlen Azure AD-felhasználó van rendelve LucidChart teszteli a telepítési konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="55117-117">It is recommended that a single Azure AD user is assigned to LucidChart to test the provisioning configuration.</span></span> <span data-ttu-id="55117-118">További felhasználók és/vagy csoportok később is rendelhető.</span><span class="sxs-lookup"><span data-stu-id="55117-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="55117-119">Ha egy felhasználó hozzárendelése LucidChart, kell-e ki lehet a **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) a hozzárendelés párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="55117-119">When assigning a user to LucidChart, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="55117-120">A **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.</span><span class="sxs-lookup"><span data-stu-id="55117-120">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-to-lucidchart"></a><span data-ttu-id="55117-121">A felhasználók átadása a LucidChart konfigurálása</span><span class="sxs-lookup"><span data-stu-id="55117-121">Configuring user provisioning to LucidChart</span></span> 

<span data-ttu-id="55117-122">Ez a szakasz végigvezeti az Azure AD kapcsolódás LucidChart a felhasználói fiók kiépítése API és a létesítési szolgáltatás létrehozása, konfigurálása frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú LucidChart hozzárendelt felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="55117-122">This section guides you through connecting your Azure AD to LucidChart's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="55117-123">Dönthet úgy is, SAML-alapú egyszeri bejelentkezést LucidChart engedélyezni, utasítások megadott [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="55117-123">You may also choose to enabled SAML-based Single Sign-On for LucidChart, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="55117-124">Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.</span><span class="sxs-lookup"><span data-stu-id="55117-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a><span data-ttu-id="55117-125">Automatikus felhasználói fiók kiépítés LucidChart az Azure AD konfigurálása</span><span class="sxs-lookup"><span data-stu-id="55117-125">Configure automatic user account provisioning to LucidChart in Azure AD</span></span>


1. <span data-ttu-id="55117-126">Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="55117-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="55117-127">Ha már konfigurált LucidChart egyszeri bejelentkezést, keresse meg a keresési mező LucidChart példányát.</span><span class="sxs-lookup"><span data-stu-id="55117-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using the search field.</span></span> <span data-ttu-id="55117-128">Máskülönben válassza **Hozzáadás** keresse meg a **LucidChart** az alkalmazás katalógusában.</span><span class="sxs-lookup"><span data-stu-id="55117-128">Otherwise, select **Add** and search for **LucidChart** in the application gallery.</span></span> <span data-ttu-id="55117-129">Válassza ki a LucidChart a keresési eredmények közül, és adja hozzá az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="55117-129">Select LucidChart from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="55117-130">Jelölje ki a LucidChart példányát, majd válassza ki a **kiépítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="55117-130">Select your instance of LucidChart, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="55117-131">Állítsa be a **kiépítési üzemmódját** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="55117-131">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="55117-133">Alatt a **rendszergazdai hitelesítő adatokat** területen adjon meg a **jogkivonat titkos kulcs** állítja elő a LucidChart fiók (a token található a fiókjában: **Team**  >  **Alkalmazásintegráció** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="55117-133">Under the **Admin Credentials** section, input the **Secret Token** generated by your LucidChart's account (you can find the token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="55117-135">Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD csatlakozhat az LucidChart alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="55117-135">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your LucidChart app.</span></span> <span data-ttu-id="55117-136">Ha nem sikerül, ellenőrizze a LucidChart fiókja rendszergazdai jogosultságokkal rendelkezik, és ismételje meg 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="55117-136">If the connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="55117-137">Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet a "E-mail értesítés küldése hiba esetén."</span><span class="sxs-lookup"><span data-stu-id="55117-137">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="55117-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="55117-138">Click **Save**.</span></span> 

9. <span data-ttu-id="55117-139">A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználókat LucidChart**.</span><span class="sxs-lookup"><span data-stu-id="55117-139">Under the Mappings section, select **Synchronize Azure Active Directory Users to LucidChart**.</span></span>

10. <span data-ttu-id="55117-140">Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumok, az Azure AD LucidChart lettek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="55117-140">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to LucidChart.</span></span> <span data-ttu-id="55117-141">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a LucidChart a frissítési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="55117-141">The attributes selected as **Matching** properties are used to match the user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="55117-142">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="55117-142">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="55117-143">Az Azure AD szolgáltatás LucidChart kiépítés engedélyezéséhez módosítsa a **kiépítési állapot** való **a** a a **beállítások** szakasz</span><span class="sxs-lookup"><span data-stu-id="55117-143">To enable the Azure AD provisioning service for LucidChart, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="55117-144">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="55117-144">Click **Save**.</span></span> 

<span data-ttu-id="55117-145">Ez a művelet bármilyen felhasználói és/vagy a felhasználók és csoportok szakaszban LucidChart rendelt csoportok a kezdeti szinkronizálás elindul.</span><span class="sxs-lookup"><span data-stu-id="55117-145">This operation starts the initial synchronization of any users and/or groups assigned to LucidChart in the Users and Groups section.</span></span> <span data-ttu-id="55117-146">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="55117-146">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="55117-147">Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amelyek a létesítési szolgáltatás által végzett összes műveletet írják le.</span><span class="sxs-lookup"><span data-stu-id="55117-147">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="55117-148">Olvassa el az Azure AD-naplók kiépítés módjáról további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="55117-148">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="55117-149">További források</span><span class="sxs-lookup"><span data-stu-id="55117-149">Additional resources</span></span>

* [<span data-ttu-id="55117-150">Felhasználói fiók kiépítése vállalati alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="55117-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="55117-151">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="55117-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="55117-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55117-152">Next steps</span></span>

* [<span data-ttu-id="55117-153">Ismerje meg, tekintse át a naplók és jelentések készítése a kiépítés tevékenység</span><span class="sxs-lookup"><span data-stu-id="55117-153">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
