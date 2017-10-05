---
title: "Oktatóanyag: Azure Active Directory-integráció SCC életciklusával |} Microsoft Docs"
description: "Útmutató SCC életciklusa az Azure Active Directoryval az egyszeri bejelentkezés, automatikus kiépítésének, és több!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a><span data-ttu-id="7a847-103">Oktatóanyag: Azure Active Directoryval integrált SCC életciklusa</span><span class="sxs-lookup"><span data-stu-id="7a847-103">Tutorial: Azure Active Directory integration with SCC LifeCycle</span></span>
<span data-ttu-id="7a847-104">Ez az oktatóanyag célja az Azure és SCC életciklus integrálása megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7a847-104">The objective of this tutorial is to show the integration of Azure and SCC LifeCycle.</span></span>  

<span data-ttu-id="7a847-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="7a847-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="7a847-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="7a847-106">A valid Azure subscription</span></span>
* <span data-ttu-id="7a847-107">Egy SCC életciklusa az egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7a847-107">A SCC LifeCycle single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="7a847-108">Ez az oktatóanyag befejezése után az Azure AD-felhasználók SCC életciklus rendelt fog tudni egyetlen jelentkezzen be az alkalmazás a SCC életciklus vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési), vagy használja a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a847-108">After completing this tutorial, the Azure AD users you have assigned to SCC LifeCycle will be able to single sign into the application at your SCC LifeCycle company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="7a847-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7a847-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="7a847-110">Az alkalmazás SCC életciklus-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7a847-110">Enabling the application integration for SCC LifeCycle</span></span>
2. <span data-ttu-id="7a847-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a847-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="7a847-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="7a847-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="7a847-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7a847-113">Assigning users</span></span>

<span data-ttu-id="7a847-114">![A forgatókönyv](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="7a847-114">![Scenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")</span></span>

## <a name="enable-the-application-integration-for-scc-lifecycle"></a><span data-ttu-id="7a847-115">Az alkalmazás SCC életciklus-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7a847-115">Enable the application integration for SCC LifeCycle</span></span>
<span data-ttu-id="7a847-116">Ez a szakasz célja felvázoló SCC életciklusa az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7a847-116">The objective of this section is to outline how to enable the application integration for SCC LifeCycle.</span></span>

<span data-ttu-id="7a847-117">**Ahhoz, hogy az alkalmazás-integráció SCC életciklusát, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7a847-117">**To enable the application integration for SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="7a847-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7a847-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="7a847-119">![Az Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="7a847-119">![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="7a847-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="7a847-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="7a847-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="7a847-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="7a847-122">![Alkalmazások](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="7a847-122">![Applications](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="7a847-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="7a847-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="7a847-124">![Alkalmazás hozzáadása](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="7a847-124">![Add application](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="7a847-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="7a847-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="7a847-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="7a847-126">![Add an application from gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="7a847-127">Az a **keresőmezőbe**, típus **SCC életciklus**.</span><span class="sxs-lookup"><span data-stu-id="7a847-127">In the **search box**, type **SCC LifeCycle**.</span></span>
   
    <span data-ttu-id="7a847-128">![Alkalmazáskatalógusában](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="7a847-128">![Application Gallery](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")</span></span>
7. <span data-ttu-id="7a847-129">Az eredmények ablaktáblájában válassza **SCC életciklus**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7a847-129">In the results pane, select **SCC LifeCycle**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="7a847-130">![SCC életciklus](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC életciklusa")</span><span class="sxs-lookup"><span data-stu-id="7a847-130">![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="7a847-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a847-131">Configure single sign-on</span></span>

<span data-ttu-id="7a847-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez SCC életciklus fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="7a847-132">The objective of this section is to outline how to enable users to authenticate to SCC LifeCycle with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="7a847-133">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7a847-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="7a847-134">A klasszikus Azure portálon a a **SCC életciklus** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a847-134">In the Azure classic portal, on the **SCC LifeCycle** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="7a847-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7a847-135">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="7a847-136">A a **hová bejelentkezni SCC életciklus felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7a847-136">On the **How would you like users to sign on to SCC LifeCycle** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="7a847-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7a847-137">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="7a847-138">A a **alkalmazás URL-cím konfigurálása** lap a **URL-cím bejelentkezési** szövegmező, írja be az URL-cím segítségével a felhasználók jelentkezzen be a SCC életciklus-alkalmazás a következő minta használatával "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="7a847-138">On the **Configure App URL** page, in the **Sign On URL** textbox, type the URL used by your users to sign on to your SCC LifeCycle application using the following pattern "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*", and then click **Next**.</span></span>
   
    <span data-ttu-id="7a847-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7a847-139">![Configure App URL](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")</span></span>
4. <span data-ttu-id="7a847-140">A a **konfigurálhatja az egyszeri bejelentkezés SCC életciklus** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7a847-140">On the **Configure single sign-on at SCC LifeCycle** page, click **Download metadata**, and then save metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="7a847-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7a847-141">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="7a847-142">Továbbítsa a SCC életciklus támogatási csoportnak adott metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7a847-142">Forward that Metadata file to SCC LifeCycle Support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="7a847-143">Egyszeri bejelentkezés ki engedélyezni lehessen az SCC életciklus támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="7a847-143">Single sign-on has to be enabled by the SCC LifeCycle support team.</span></span>
   > 
   > 

6. <span data-ttu-id="7a847-144">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a847-144">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="7a847-145">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="7a847-145">![Configure Single Sign-On](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="7a847-146">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7a847-146">Configure user provisioning</span></span>

<span data-ttu-id="7a847-147">Ahhoz, hogy az Azure AD-felhasználók SCC életciklus bejelentkezni, akkor ki kell építenie SCC életciklus be.</span><span class="sxs-lookup"><span data-stu-id="7a847-147">In order to enable Azure AD users to log into SCC LifeCycle, they must be provisioned into SCC LifeCycle.</span></span> <span data-ttu-id="7a847-148">Nincs művelet elem felhasználói kiépítés SCC életciklus konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="7a847-148">There is no action item for you to configure user provisioning to SCC LifeCycle.</span></span>

<span data-ttu-id="7a847-149">Ha egy hozzárendelt felhasználó próbál bejelentkezni az SCC életciklusát, SCC életciklus fiók automatikusan létrejön, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="7a847-149">When an assigned user tries to log into SCC LifeCycle, an SCC LifeCycle account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="7a847-150">Bármely más SCC életciklus felhasználói fiók létrehozása eszközök, vagy API-k biztosítja SCC életciklus rendelkezés AAD felhasználói fiókokhoz.</span><span class="sxs-lookup"><span data-stu-id="7a847-150">You can use any other SCC LifeCycle user account creation tools or APIs provided by SCC LifeCycle to provision AAD user accounts.</span></span>
> 
> 

## <a name="assign-users"></a><span data-ttu-id="7a847-151">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7a847-151">Assign users</span></span>
<span data-ttu-id="7a847-152">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="7a847-152">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="7a847-153">**Felhasználók hozzárendelése SCC életciklusát, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7a847-153">**To assign users to SCC LifeCycle, perform the following steps:**</span></span>

1. <span data-ttu-id="7a847-154">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="7a847-154">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="7a847-155">Az a ** SCC életciklus ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="7a847-155">On the **SCC LifeCycle **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="7a847-156">![Felhasználók hozzárendelése](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="7a847-156">![Assign Users](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")</span></span>
3. <span data-ttu-id="7a847-157">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7a847-157">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="7a847-158">![Igen](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="7a847-158">![Yes](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="7a847-159">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="7a847-159">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="7a847-160">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a847-160">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

