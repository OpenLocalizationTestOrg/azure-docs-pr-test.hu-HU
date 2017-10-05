---
title: "Oktatóanyag: Azure Active Directoryval integrált @Task|} Microsoft Docs"
description: "Egyszeri bejelentkezés Azure Active Directory közötti konfigurálásával és @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: ebb19ca6cbaf04106fbce937d95651e709854cfd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="62668-103">Oktatóanyag: Azure Active Directory integrációja@Task</span><span class="sxs-lookup"><span data-stu-id="62668-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="62668-104">Ez az oktatóanyag célja mutatjuk be, hogyan integrálható @Task az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="62668-104">The objective of this tutorial is to show you how to integrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="62668-105">Integrálása @Task az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="62668-105">Integrating @Task with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="62668-106">Szabályozhatja, aki az Azure AD-ben@Task</span><span class="sxs-lookup"><span data-stu-id="62668-106">You can control in Azure AD who has access to @Task</span></span>
* <span data-ttu-id="62668-107">Engedélyezheti a felhasználók automatikusan el a bejelentkezett @Task (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="62668-107">You can enable your users to automatically get signed-on to @Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="62668-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="62668-108">You can manage your accounts in one central location - the Azure classic Portal</span></span>

<span data-ttu-id="62668-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="62668-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62668-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="62668-110">Prerequisites</span></span>
<span data-ttu-id="62668-111">Az Azure AD integrálása konfigurálása @Task, a következőkre van szüksége:</span><span class="sxs-lookup"><span data-stu-id="62668-111">To configure Azure AD integration with @Task, you need the following items:</span></span>

* <span data-ttu-id="62668-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="62668-112">An Azure AD subscription</span></span>
* <span data-ttu-id="62668-113">Egy @Task egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="62668-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="62668-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="62668-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="62668-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="62668-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="62668-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="62668-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="62668-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="62668-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="62668-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="62668-118">Scenario Description</span></span>
<span data-ttu-id="62668-119">Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="62668-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="62668-120">A forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="62668-120">The scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="62668-121">Hozzáadás @Task a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="62668-121">Adding @Task from the gallery</span></span> 
2. <span data-ttu-id="62668-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="62668-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-the-gallery"></a><span data-ttu-id="62668-123">Hozzáadás @Task a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="62668-123">Adding @Task from the gallery</span></span>
<span data-ttu-id="62668-124">Az integráció konfigurálásához @Task az Azure AD hozzá kell adnia @Task a felügyelt SaaS-alkalmazások listájára a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="62668-124">To configure the integration of @Task into Azure AD, you need to add @Task from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="62668-125">**Hozzáadandó @Task a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="62668-125">**To add @Task from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="62668-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="62668-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="62668-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="62668-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="62668-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="62668-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2] 
4. <span data-ttu-id="62668-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="62668-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3] 
5. <span data-ttu-id="62668-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="62668-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4] 
6. <span data-ttu-id="62668-135">Írja be a keresőmezőbe,  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="62668-135">In the search box, type **@Task**.</span></span>
   
    ![Alkalmazások][5] 
7. <span data-ttu-id="62668-137">Az eredmények ablaktáblájában válassza  **@Task** , és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="62668-137">In the results pane, select **@Task**, and then click **Complete** to add the application.</span></span>
   
    ![Alkalmazások][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="62668-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="62668-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="62668-140">Ez a szakasz az a célja, hogy bemutatják az Azure AD egyszeri bejelentkezést, tesztelése és konfigurálása @Task "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="62668-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="62668-141">Az egyszeri bejelentkezés működéséhez, az Azure AD tudnia kell, mely a partner felhasználót a @Task van egy olyan felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="62668-141">For single sign-on to work, Azure AD needs to know what the counterpart user in @Task to an user in Azure AD is.</span></span> <span data-ttu-id="62668-142">Ez azt jelenti, az Azure AD-felhasználó és a kapcsolódó felhasználó a közötti kapcsolat kapcsolatot @Task kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="62668-142">In other words, a link relationship between an Azure AD user and the related user in @Task needs to be established.</span></span>   
<span data-ttu-id="62668-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a @Task.</span><span class="sxs-lookup"><span data-stu-id="62668-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in @Task.</span></span>

<span data-ttu-id="62668-144">Az Azure AD egyszeri bejelentkezést, tesztelése és konfigurálása a @Task, végre kell hajtania a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="62668-144">To configure and test Azure AD single sign-on with @Task, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="62668-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="62668-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="62668-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="62668-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="62668-147">**[Létrehozása egy @Tasktest felhasználói](#creating-a-halogen-software-test-user)**  - kell rendelkeznie a Britta Simon valami @Taskthat csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="62668-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in @Taskthat is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="62668-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="62668-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="62668-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="62668-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="62668-150">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="62668-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="62668-151">Ez a szakasz célja az Azure AD az egyszeri bejelentkezés a klasszikus Azure portálon engedélyezése és konfigurálása egyszeri bejelentkezéshez a a @Task alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="62668-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your @Task application.</span></span>

<span data-ttu-id="62668-152">**Konfigurálhatja az Azure AD egyszeri bejelentkezést a @Task, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="62668-152">**To configure Azure AD single sign-on with @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="62668-153">A klasszikus Azure portálon a a  **@Task**  alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="62668-153">In the Azure classic portal, on the **@Task** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="62668-155">Az a **hová felhasználók jelentkezhetnek be @Task**  lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="62668-155">On the **How would you like users to sign on to @Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 
3. <span data-ttu-id="62668-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="62668-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Alkalmazásbeállítások konfigurálása][8] 
   
     <span data-ttu-id="62668-159">a.</span><span class="sxs-lookup"><span data-stu-id="62668-159">a.</span></span> <span data-ttu-id="62668-160">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés a @Task alkalmazás (pl.:*https://<Tenant name>.attask-ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="62668-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="62668-161">b.</span><span class="sxs-lookup"><span data-stu-id="62668-161">b.</span></span> <span data-ttu-id="62668-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="62668-162">Click **Next**.</span></span>
4. <span data-ttu-id="62668-163">A a **konfigurálása egyszeri bejelentkezéshez, @Task**  kattintson **metaadatok letöltése**, mentse helyileg a számítógépen a metaadatok fájlt, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="62668-163">On the **Configure single sign-on at @Task** page, click **Download metadata**, save the metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Mi az az Azure AD Connect?][9] 
5. <span data-ttu-id="62668-165">Bejelentkezés a @Task vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="62668-165">Sign-on to your @Task company site as administrator.</span></span>
6. <span data-ttu-id="62668-166">Ugrás a **az egyszeri bejelentkezés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="62668-166">Go to **Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="62668-167">Az a **egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépéseket</span><span class="sxs-lookup"><span data-stu-id="62668-167">On the **Single Sign-On** dialog, perform the following steps</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="62668-169">a.</span><span class="sxs-lookup"><span data-stu-id="62668-169">a.</span></span> <span data-ttu-id="62668-170">Mint **típus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="62668-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="62668-171">b.</span><span class="sxs-lookup"><span data-stu-id="62668-171">b.</span></span> <span data-ttu-id="62668-172">Válassza ki **Szolgáltatóazonosító szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="62668-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="62668-173">c.</span><span class="sxs-lookup"><span data-stu-id="62668-173">c.</span></span> <span data-ttu-id="62668-174">A klasszikus Azure portálra, másolja a **távoli bejelentkezési URL-cím**, és illessze be azt a **bejelentkezési portál URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="62668-174">On the Azure classic portal, copy the **Remote Login URL**, and then paste it into the **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="62668-175">d.</span><span class="sxs-lookup"><span data-stu-id="62668-175">d.</span></span> <span data-ttu-id="62668-176">A klasszikus Azure portálra, másolja a **egyetlen Sign-Out URL-címe**, majd illessze be azt a **Sign-Out URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="62668-176">On the Azure classic portal, copy the **Single Sign-Out Service URL**, and then paste it into the **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="62668-177">e.</span><span class="sxs-lookup"><span data-stu-id="62668-177">e.</span></span> <span data-ttu-id="62668-178">A klasszikus Azure portálra, másolja a **jelszó URL-cím módosítása**, majd illessze be azt a **jelszó URL-cím módosítása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="62668-178">On the Azure classic portal, copy the **Change Password URL**, and then paste it into the **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="62668-179">f.</span><span class="sxs-lookup"><span data-stu-id="62668-179">f.</span></span> <span data-ttu-id="62668-180">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="62668-180">Click **Save**.</span></span>
8. <span data-ttu-id="62668-181">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="62668-181">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Mi az az Azure AD Connect?][10]
9. <span data-ttu-id="62668-183">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="62668-183">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Mi az az Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="62668-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="62668-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="62668-186">Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="62668-186">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="62668-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="62668-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="62668-189">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="62668-189">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="62668-191">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="62668-191">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="62668-192">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="62668-192">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="62668-194">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="62668-194">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="62668-196">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="62668-196">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="62668-198">a.</span><span class="sxs-lookup"><span data-stu-id="62668-198">a.</span></span> <span data-ttu-id="62668-199">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="62668-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="62668-200">b.</span><span class="sxs-lookup"><span data-stu-id="62668-200">b.</span></span> <span data-ttu-id="62668-201">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="62668-201">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="62668-202">c.</span><span class="sxs-lookup"><span data-stu-id="62668-202">c.</span></span> <span data-ttu-id="62668-203">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="62668-203">Click **Next**.</span></span>
6. <span data-ttu-id="62668-204">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="62668-204">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="62668-206">a.</span><span class="sxs-lookup"><span data-stu-id="62668-206">a.</span></span> <span data-ttu-id="62668-207">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="62668-207">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="62668-208">b.</span><span class="sxs-lookup"><span data-stu-id="62668-208">b.</span></span> <span data-ttu-id="62668-209">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="62668-209">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="62668-210">c.</span><span class="sxs-lookup"><span data-stu-id="62668-210">c.</span></span> <span data-ttu-id="62668-211">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="62668-211">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="62668-212">d.</span><span class="sxs-lookup"><span data-stu-id="62668-212">d.</span></span> <span data-ttu-id="62668-213">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="62668-213">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="62668-214">e.</span><span class="sxs-lookup"><span data-stu-id="62668-214">e.</span></span> <span data-ttu-id="62668-215">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="62668-215">Click **Next**.</span></span>

7. <span data-ttu-id="62668-216">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="62668-216">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="62668-218">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="62668-218">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="62668-220">a.</span><span class="sxs-lookup"><span data-stu-id="62668-220">a.</span></span> <span data-ttu-id="62668-221">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="62668-221">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="62668-222">b.</span><span class="sxs-lookup"><span data-stu-id="62668-222">b.</span></span> <span data-ttu-id="62668-223">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="62668-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="62668-224">Létrehozása egy @Task tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="62668-224">Creating an @Task test user</span></span>
<span data-ttu-id="62668-225">Ez a szakasz célja Britta Simon nevű felhasználó létrehozásához @Task.</span><span class="sxs-lookup"><span data-stu-id="62668-225">The objective of this section is to create a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="62668-226">**Britta Simon nevű felhasználó létrehozásához @Task, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="62668-226">**To create a user called Britta Simon in @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="62668-227">Jelentkezzen be a @Task vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="62668-227">Sign on to your @Task company site as administrator.</span></span>
2. <span data-ttu-id="62668-228">Kattintson a felső menüben **személyek**.</span><span class="sxs-lookup"><span data-stu-id="62668-228">In the menu on the top, click **People**.</span></span>
3. <span data-ttu-id="62668-229">Kattintson a **új személy**.</span><span class="sxs-lookup"><span data-stu-id="62668-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="62668-230">Új személy párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="62668-230">On the New Person dialog, perform the following steps:</span></span>
   
    ![Hozzon létre egy @Task tesztfelhasználó számára][21] 
   
    <span data-ttu-id="62668-232">a.</span><span class="sxs-lookup"><span data-stu-id="62668-232">a.</span></span> <span data-ttu-id="62668-233">Az a **Keresztnév** szövegmező, írja be a "Britta".</span><span class="sxs-lookup"><span data-stu-id="62668-233">In the **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="62668-234">b.</span><span class="sxs-lookup"><span data-stu-id="62668-234">b.</span></span> <span data-ttu-id="62668-235">Az a **Vezetéknév** szövegmező, írja be a "Simon".</span><span class="sxs-lookup"><span data-stu-id="62668-235">In the **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="62668-236">c.</span><span class="sxs-lookup"><span data-stu-id="62668-236">c.</span></span> <span data-ttu-id="62668-237">Az a **E-mail cím** szövegmező, írja be a Britta Simon e-mail címét az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="62668-237">In the **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="62668-238">d.</span><span class="sxs-lookup"><span data-stu-id="62668-238">d.</span></span> <span data-ttu-id="62668-239">Kattintson a **személy hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="62668-239">Click **Add Person**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="62668-240">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="62668-240">Assigning the Azure AD test user</span></span>
<span data-ttu-id="62668-241">Ez a szakasz az a célja, hogy Britta Simon szerint saját való hozzáférés biztosítása használandó Azure egyszeri bejelentkezés engedélyezése @Task.</span><span class="sxs-lookup"><span data-stu-id="62668-241">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to @Task.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="62668-243">**A Britta Simon hozzárendelni @Task, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="62668-243">**To assign Britta Simon to @Task, perform the following steps:**</span></span>

1. <span data-ttu-id="62668-244">A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="62668-244">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 
2. <span data-ttu-id="62668-246">Az alkalmazások listában válassza ki a  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="62668-246">In the applications list, select **@Task**.</span></span>
   
    ![Felhasználó hozzárendelése][202] 
3. <span data-ttu-id="62668-248">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="62668-248">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 
4. <span data-ttu-id="62668-250">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="62668-250">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="62668-251">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="62668-251">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="62668-253">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="62668-253">Testing Single Sign-On</span></span>
<span data-ttu-id="62668-254">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="62668-254">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="62668-255">Ha a @Task csempére a hozzáférési panelen kapja meg automatikusan bejelentkezett a a @Task alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="62668-255">When you click the @Task tile in the Access Panel, you should get automatically signed-on to your @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62668-256">További források</span><span class="sxs-lookup"><span data-stu-id="62668-256">Additional Resources</span></span>
* [<span data-ttu-id="62668-257">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="62668-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62668-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="62668-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






