---
title: "Oktatóanyag: Azure Active Directoryval integrált Qualtrics |} Microsoft Docs"
description: "Megtudhatja, hogyan Qualtrics használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a><span data-ttu-id="82372-103">Oktatóanyag: Azure Active Directoryval integrált Qualtrics</span><span class="sxs-lookup"><span data-stu-id="82372-103">Tutorial: Azure Active Directory integration with Qualtrics</span></span>
<span data-ttu-id="82372-104">Ez az oktatóanyag célja az Azure és Qualtrics integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="82372-104">The objective of this tutorial is to show the integration of Azure and Qualtrics.</span></span>  

<span data-ttu-id="82372-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="82372-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="82372-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="82372-106">A valid Azure subscription</span></span>
* <span data-ttu-id="82372-107">Egy Qualtrics egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="82372-107">A Qualtrics single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="82372-108">Ez az oktatóanyag befejezése után az Azure AD-felhasználók Qualtrics rendelt fog tudni egyetlen jelentkezzen be az alkalmazás a Qualtrics vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési), vagy használja a [a hozzáférési Panelbemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82372-108">After completing this tutorial, the Azure AD users you have assigned to Qualtrics will be able to single sign into the application at your Qualtrics company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="82372-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="82372-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="82372-110">Az alkalmazás Qualtrics-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="82372-110">Enabling the application integration for Qualtrics</span></span>
2. <span data-ttu-id="82372-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="82372-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="82372-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="82372-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="82372-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="82372-113">Assigning users</span></span>

<span data-ttu-id="82372-114">![A forgatókönyv](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="82372-114">![Scenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-qualtrics"></a><span data-ttu-id="82372-115">Az alkalmazás Qualtrics-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="82372-115">Enabling the application integration for Qualtrics</span></span>
<span data-ttu-id="82372-116">Ez a szakasz célja felvázoló Qualtrics az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="82372-116">The objective of this section is to outline how to enable the application integration for Qualtrics.</span></span>

<span data-ttu-id="82372-117">**Ahhoz, hogy az alkalmazás-integráció Qualtrics, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82372-117">**To enable the application integration for Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="82372-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="82372-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="82372-119">![Az Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="82372-119">![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="82372-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="82372-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="82372-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="82372-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="82372-122">![Alkalmazások](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="82372-122">![Applications](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="82372-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="82372-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="82372-124">![Alkalmazás hozzáadása](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="82372-124">![Add application](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="82372-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="82372-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="82372-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="82372-126">![Add an application from gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="82372-127">Az a **keresőmezőbe**, típus **Qualtrics**.</span><span class="sxs-lookup"><span data-stu-id="82372-127">In the **search box**, type **Qualtrics**.</span></span>
   
   <span data-ttu-id="82372-128">![Alkalmazáskatalógusában](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="82372-128">![Application Gallery](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")</span></span>
7. <span data-ttu-id="82372-129">Az eredmények ablaktáblájában válassza **Qualtrics**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="82372-129">In the results pane, select **Qualtrics**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="82372-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span><span class="sxs-lookup"><span data-stu-id="82372-130">![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="82372-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="82372-131">Configure single sign-on</span></span>

<span data-ttu-id="82372-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Qualtrics fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="82372-132">The objective of this section is to outline how to enable users to authenticate to Qualtrics with their account in Azure AD using federation based on the SAML protocol.</span></span>

<span data-ttu-id="82372-133">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82372-133">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="82372-134">A klasszikus Azure portálon a a **Qualtrics** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82372-134">In the Azure classic portal, on the **Qualtrics** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="82372-135">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="82372-135">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="82372-136">Az a **hová bejelentkezni Qualtrics felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="82372-136">On the **How would you like users to sign on to Qualtrics** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="82372-137">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="82372-137">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="82372-138">A a **alkalmazás URL-cím konfigurálása** lap a **Qualtrics bejelentkezési URL-cím** szövegmező, írja be az URL-cím (pl.: "*https://ssotest2ut1.qualtrics.com*"), és kattintson a **Következő**.</span><span class="sxs-lookup"><span data-stu-id="82372-138">On the **Configure App URL** page, in the **Qualtrics Sign On URL** textbox, type your URL (e.g.: “*https://ssotest2ut1.qualtrics.com*"), and then click **Next**.</span></span>
   
   <span data-ttu-id="82372-139">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="82372-139">![Configure App URL](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")</span></span>
4. <span data-ttu-id="82372-140">A a **konfigurálhatja az egyszeri bejelentkezés Qualtrics** kattintson **metaadatok letöltése**, és mentse a fájlt a számítógépen a metaadat.</span><span class="sxs-lookup"><span data-stu-id="82372-140">On the **Configure single sign-on at Qualtrics** page, click **Download metadata**, and then save the metadata file on your computer.</span></span>
   
   <span data-ttu-id="82372-141">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="82372-141">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="82372-142">A metaadatfájl küldeni a Qualtrics támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="82372-142">Send the metadata file to the Qualtrics support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="82372-143">Az egyszeri bejelentkezés konfigurációs ki a Qualtrics támogatási csoport végzi el.</span><span class="sxs-lookup"><span data-stu-id="82372-143">The SSO configuration has to be performed by the Qualtrics support team.</span></span> <span data-ttu-id="82372-144">Értesítést kap, amint a konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="82372-144">You will get a notification as soon as the configuration has been completed.</span></span>
   > 
   > 
6. <span data-ttu-id="82372-145">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82372-145">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="82372-146">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="82372-146">![Configure Single Sign-On](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")</span></span>
   
## <a name="configure-user-provisioning"></a><span data-ttu-id="82372-147">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="82372-147">Configure user provisioning</span></span>

<span data-ttu-id="82372-148">Nincs művelet elem ahhoz, hogy a felhasználó Qualtrics történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="82372-148">There is no action item for you to configure user provisioning to Qualtrics.</span></span> <span data-ttu-id="82372-149">Ha egy hozzárendelt felhasználó megpróbál bejelentkezni, a hozzáférési panelen Qualtrics, Qualtrics ellenőrzi, hogy a felhasználó létezik-e.</span><span class="sxs-lookup"><span data-stu-id="82372-149">When an assigned user tries to log into Qualtrics using the access panel, Qualtrics checks whether the user exists.</span></span>  

<span data-ttu-id="82372-150">Nincs még nincs felhasználói fiók érhető el, ha a Qualtrics automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="82372-150">If there is no user account available yet, it is automatically created by Qualtrics.</span></span>

## <a name="assign-users"></a><span data-ttu-id="82372-151">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="82372-151">Assign users</span></span>
<span data-ttu-id="82372-152">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="82372-152">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

<span data-ttu-id="82372-153">**Felhasználók hozzárendelése Qualtrics, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82372-153">**To assign users to Qualtrics, perform the following steps:**</span></span>

1. <span data-ttu-id="82372-154">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="82372-154">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="82372-155">Az a **Qualtrics** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="82372-155">On the **Qualtrics** application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="82372-156">![Felhasználók hozzárendelése](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="82372-156">![Assign Users](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")</span></span>
3. <span data-ttu-id="82372-157">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="82372-157">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="82372-158">![Igen](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="82372-158">![Yes](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="82372-159">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="82372-159">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="82372-160">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82372-160">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

