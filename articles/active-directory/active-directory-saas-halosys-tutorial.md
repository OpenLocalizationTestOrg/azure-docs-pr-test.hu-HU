---
title: "Oktatóanyag: Azure Active Directoryval integrált Halosys |} Microsoft Docs"
description: "Megtudhatja, hogyan Halosys használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18c5cd8eb4ca211f8ae2b8dd994c0e8c48625a2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="21d0d-103">Oktatóanyag: Azure Active Directoryval integrált Halosys</span><span class="sxs-lookup"><span data-stu-id="21d0d-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="21d0d-104">Ebben az oktatóanyagban elsajátíthatja Halosys integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="21d0d-104">In this tutorial, you learn how to integrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21d0d-105">Halosys integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="21d0d-105">Integrating Halosys with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="21d0d-106">Megadhatja a Halosys hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="21d0d-106">You can control in Azure AD who has access to Halosys</span></span>
- <span data-ttu-id="21d0d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Halosys (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="21d0d-107">You can enable your users to automatically get signed-on to Halosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="21d0d-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="21d0d-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="21d0d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="21d0d-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21d0d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21d0d-110">Prerequisites</span></span>

<span data-ttu-id="21d0d-111">Konfigurálása az Azure AD-integrációs Halosys, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="21d0d-111">To configure Azure AD integration with Halosys, you need the following items:</span></span>

- <span data-ttu-id="21d0d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="21d0d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="21d0d-113">Egy Halosys egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="21d0d-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="21d0d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="21d0d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="21d0d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="21d0d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="21d0d-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="21d0d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="21d0d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="21d0d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="21d0d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="21d0d-118">Scenario description</span></span>
<span data-ttu-id="21d0d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="21d0d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="21d0d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="21d0d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="21d0d-121">A gyűjteményből Halosys hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21d0d-121">Adding Halosys from the gallery</span></span>
2. <span data-ttu-id="21d0d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21d0d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-the-gallery"></a><span data-ttu-id="21d0d-123">A gyűjteményből Halosys hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21d0d-123">Adding Halosys from the gallery</span></span>
<span data-ttu-id="21d0d-124">Az Azure AD integrálása a Halosys konfigurálásához kell hozzáadnia Halosys a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="21d0d-124">To configure the integration of Halosys into Azure AD, you need to add Halosys from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="21d0d-125">**A gyűjteményből Halosys hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21d0d-125">**To add Halosys from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="21d0d-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="21d0d-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="21d0d-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="21d0d-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="21d0d-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="21d0d-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="21d0d-131">Click **Add** at the bottom of the page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="21d0d-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>

    ![Alkalmazások][4]

6. <span data-ttu-id="21d0d-135">Írja be a keresőmezőbe, **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-135">In the search box, type **Halosys**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="21d0d-137">Az eredmények ablaktáblájában válassza **Halosys**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="21d0d-137">In the results pane, select **Halosys**, and then click **Complete** to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="21d0d-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="21d0d-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="21d0d-140">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Halosys.</span><span class="sxs-lookup"><span data-stu-id="21d0d-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="21d0d-141">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Halosys a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="21d0d-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Halosys is to a user in Azure AD.</span></span> <span data-ttu-id="21d0d-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Halosys közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="21d0d-142">In other words, a link relationship between an Azure AD user and the related user in Halosys needs to be established.</span></span>

<span data-ttu-id="21d0d-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Halosys a.</span><span class="sxs-lookup"><span data-stu-id="21d0d-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Halosys.</span></span>

<span data-ttu-id="21d0d-144">Az Azure AD egyszeri bejelentkezést a Halosys tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="21d0d-144">To configure and test Azure AD single sign-on with Halosys, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="21d0d-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="21d0d-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="21d0d-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="21d0d-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="21d0d-147">**[Halosys tesztfelhasználó létrehozása](#creating-a-halosys-test-user)**  - való Britta Simon valami Halosys, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="21d0d-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - to have a counterpart of Britta Simon in Halosys that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="21d0d-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="21d0d-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="21d0d-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="21d0d-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="21d0d-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21d0d-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="21d0d-151">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a klasszikus portálon, és konfigurálása egyszeri bejelentkezéshez az Halosys alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="21d0d-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="21d0d-152">**Konfigurálása az Azure AD az egyszeri bejelentkezés Halosys, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21d0d-152">**To configure Azure AD single sign-on with Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="21d0d-153">A klasszikus portálon a a **Halosys** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="21d0d-153">In the classic portal, on the **Halosys** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="21d0d-155">Az a **hová bejelentkezni Halosys felhasználók** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-155">On the **How would you like users to sign on to Halosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="21d0d-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="21d0d-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="21d0d-159">a.</span><span class="sxs-lookup"><span data-stu-id="21d0d-159">a.</span></span> <span data-ttu-id="21d0d-160">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés a következő minta használatával Halosys Alkalmazásmódosítások: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="21d0d-160">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Halosys application using the following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="21d0d-161">b.In a **azonosító URL-t** szövegmező, írja be a következő mintát: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="21d0d-161">b.In the **Identifier URL** textbox, type the URL in the following pattern: `https://<company-name>.Halosys.com`.</span></span>   
         
4. <span data-ttu-id="21d0d-162">A a **konfigurálhatja az egyszeri bejelentkezés Halosys** lapján kattintson **metaadatok letöltése**, majd mentse a fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="21d0d-162">On the **Configure single sign-on at Halosys** page, click **Download metadata**, and then save the file on your computer:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="21d0d-164">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba a terméktámogatással Halosys és adja meg a következő:</span><span class="sxs-lookup"><span data-stu-id="21d0d-164">To get SSO configured for your application, contact Halosys support team and provide them with the following:</span></span>

    <span data-ttu-id="21d0d-165">• A letöltött **metaadatait tartalmazó fájl**</span><span class="sxs-lookup"><span data-stu-id="21d0d-165">• The downloaded **metadata file**</span></span>
    
    <span data-ttu-id="21d0d-166">• A **SAML SSO URL-címe**</span><span class="sxs-lookup"><span data-stu-id="21d0d-166">• The **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="21d0d-167">A klasszikus portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-167">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="21d0d-169">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-169">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="21d0d-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="21d0d-172">Ebben a szakaszban Britta Simon neve a klasszikus portálon tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="21d0d-172">In this section, you create a test user in the classic portal called Britta Simon.</span></span>


![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="21d0d-174">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21d0d-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="21d0d-175">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-175">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="21d0d-177">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="21d0d-177">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="21d0d-178">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-178">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="21d0d-180">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-180">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="21d0d-182">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="21d0d-182">On the **Tell us about this user** dialog page, perform the following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="21d0d-183">a.</span><span class="sxs-lookup"><span data-stu-id="21d0d-183">a.</span></span> <span data-ttu-id="21d0d-184">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="21d0d-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="21d0d-185">b.</span><span class="sxs-lookup"><span data-stu-id="21d0d-185">b.</span></span> <span data-ttu-id="21d0d-186">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="21d0d-187">c.</span><span class="sxs-lookup"><span data-stu-id="21d0d-187">c.</span></span> <span data-ttu-id="21d0d-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="21d0d-188">Click **Next**.</span></span>

6.  <span data-ttu-id="21d0d-189">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="21d0d-189">On the **User Profile** dialog page, perform the following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="21d0d-190">a.</span><span class="sxs-lookup"><span data-stu-id="21d0d-190">a.</span></span> <span data-ttu-id="21d0d-191">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-191">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="21d0d-192">b.</span><span class="sxs-lookup"><span data-stu-id="21d0d-192">b.</span></span> <span data-ttu-id="21d0d-193">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-193">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="21d0d-194">c.</span><span class="sxs-lookup"><span data-stu-id="21d0d-194">c.</span></span> <span data-ttu-id="21d0d-195">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-195">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="21d0d-196">d.</span><span class="sxs-lookup"><span data-stu-id="21d0d-196">d.</span></span> <span data-ttu-id="21d0d-197">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-197">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="21d0d-198">e.</span><span class="sxs-lookup"><span data-stu-id="21d0d-198">e.</span></span> <span data-ttu-id="21d0d-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="21d0d-199">Click **Next**.</span></span>

7. <span data-ttu-id="21d0d-200">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-200">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="21d0d-202">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="21d0d-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="21d0d-204">a.</span><span class="sxs-lookup"><span data-stu-id="21d0d-204">a.</span></span> <span data-ttu-id="21d0d-205">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-205">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="21d0d-206">b.</span><span class="sxs-lookup"><span data-stu-id="21d0d-206">b.</span></span> <span data-ttu-id="21d0d-207">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="21d0d-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="21d0d-208">Halosys tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-208">Creating a Halosys test user</span></span>

<span data-ttu-id="21d0d-209">Ebben a szakaszban egy Halosys Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="21d0d-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="21d0d-210">A felhasználók hozzáadása a Halosys platform Halosys támogatási csapattal működik.</span><span class="sxs-lookup"><span data-stu-id="21d0d-210">Please work with Halosys support team to add the users in the Halosys platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="21d0d-211">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="21d0d-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="21d0d-212">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Halosys Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="21d0d-212">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Halosys.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="21d0d-214">**Britta Simon hozzárendelése Halosys, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="21d0d-214">**To assign Britta Simon to Halosys, perform the following steps:**</span></span>

1. <span data-ttu-id="21d0d-215">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="21d0d-215">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="21d0d-217">Az alkalmazások listában válassza ki a **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-217">In the applications list, select **Halosys**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="21d0d-219">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-219">In the menu on the top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203]

4. <span data-ttu-id="21d0d-221">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-221">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="21d0d-222">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-222">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="21d0d-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="21d0d-224">Testing single sign-on</span></span>

<span data-ttu-id="21d0d-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="21d0d-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="21d0d-226">Ha a hozzáférési panelen Halosys csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Halosys alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="21d0d-226">When you click the Halosys tile in the Access Panel, you should get automatically signed-on to your Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="21d0d-227">További források</span><span class="sxs-lookup"><span data-stu-id="21d0d-227">Additional resources</span></span>

* [<span data-ttu-id="21d0d-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="21d0d-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="21d0d-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="21d0d-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
