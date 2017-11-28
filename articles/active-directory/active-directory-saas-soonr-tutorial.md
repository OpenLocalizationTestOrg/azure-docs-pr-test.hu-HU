---
title: "Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Soonr munkahelyi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="59180-103">Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi</span><span class="sxs-lookup"><span data-stu-id="59180-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="59180-104">Ez az oktatóanyag célja Soonr munkahelyi integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="59180-104">The objective of this tutorial is to show you how to integrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="59180-105">Soonr munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="59180-105">Integrating Soonr Workplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="59180-106">Szabályozhatja, aki hozzáféréssel rendelkezik Soonr munkahelyi Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="59180-106">You can control in Azure AD who has access to Soonr Workplace</span></span>
- <span data-ttu-id="59180-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Soonr munkahelyi (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="59180-107">You can enable your users to automatically get signed-on to Soonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="59180-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="59180-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="59180-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="59180-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59180-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="59180-110">Prerequisites</span></span>

<span data-ttu-id="59180-111">Az Azure AD-integrációs Soonr munkahelyi konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="59180-111">To configure Azure AD integration with Soonr Workplace, you need the following items:</span></span>

- <span data-ttu-id="59180-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="59180-112">An Azure AD subscription</span></span>
- <span data-ttu-id="59180-113">Egy Soonr munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="59180-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="59180-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="59180-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="59180-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="59180-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="59180-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="59180-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="59180-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59180-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="59180-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="59180-118">Scenario description</span></span>
<span data-ttu-id="59180-119">Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="59180-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="59180-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="59180-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="59180-121">A gyűjteményből Soonr munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="59180-121">Adding Soonr Workplace from the gallery</span></span>
2. <span data-ttu-id="59180-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="59180-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-the-gallery"></a><span data-ttu-id="59180-123">A gyűjteményből Soonr munkahelyi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="59180-123">Adding Soonr Workplace from the gallery</span></span>
<span data-ttu-id="59180-124">Az Azure AD integrálása a Soonr munkahelyi konfigurálásához kell hozzáadnia Soonr munkahelyi a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="59180-124">To configure the integration of Soonr Workplace into Azure AD, you need to add Soonr Workplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="59180-125">**A gyűjteményből Soonr munkahelyi hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="59180-125">**To add Soonr Workplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="59180-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="59180-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="59180-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="59180-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="59180-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="59180-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="59180-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="59180-131">Click **Add** at the bottom of the page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="59180-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="59180-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
 
    ![Alkalmazások][4]

6. <span data-ttu-id="59180-135">Írja be a keresőmezőbe, **Soonr munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="59180-135">In the search box, type **Soonr Workplace**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="59180-137">Az eredmények ablaktáblájában válassza **Soonr munkahelyi**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="59180-137">In the results pane, select **Soonr Workplace**, and then click **Complete** to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="59180-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="59180-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="59180-140">Ez a szakasz célja bemutatják az Azure AD egyszeri bejelentkezést a Soonr munkahelyi tesztelése és konfigurálása a tesztfelhasználó "Britta Simon" nevű alapján.</span><span class="sxs-lookup"><span data-stu-id="59180-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="59180-141">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Soonr munkahelyi egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="59180-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Soonr Workplace to an user in Azure AD is.</span></span> <span data-ttu-id="59180-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Soonr munkahelyi közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="59180-142">In other words, a link relationship between an Azure AD user and the related user in Soonr Workplace needs to be established.</span></span>  

<span data-ttu-id="59180-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Soonr munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="59180-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="59180-144">Az Azure AD egyszeri bejelentkezést a Soonr munkahelyi tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="59180-144">To configure and test Azure AD single sign-on with Soonr Workplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="59180-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="59180-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="59180-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="59180-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="59180-147">**[Soonr munkahelyi tesztfelhasználó létrehozása](#creating-a-soonr-workplace-test-user)**  – egy megfelelője a Britta Simon rendelkezik, amely csatolva van rá, hogy az Azure AD ábrázolása Soonr munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="59180-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - to have a counterpart of Britta Simon in Soonr Workplace that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="59180-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="59180-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="59180-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="59180-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="59180-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="59180-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="59180-151">Ebben a szakaszban engedélyezze az Azure AD az egyszeri bejelentkezés a klasszikus portálon, és konfigurálása egyszeri bejelentkezéshez az Soonr munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59180-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="59180-152">**Konfigurálása az Azure AD az egyszeri bejelentkezés Soonr munkahelyi, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="59180-152">**To configure Azure AD single sign-on with Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="59180-153">A klasszikus Azure portálon a a **Soonr munkahelyi** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="59180-153">In the Azure classic portal, on the **Soonr Workplace** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="59180-155">Az a **hová felhasználók bejelentkezhetnek a Soonr munkahelyi** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="59180-155">On the **How would you like users to sign on to Soonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="59180-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:.</span><span class="sxs-lookup"><span data-stu-id="59180-157">On the **Configure App Settings** dialog page, perform the following steps:.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="59180-159">a.</span><span class="sxs-lookup"><span data-stu-id="59180-159">a.</span></span> <span data-ttu-id="59180-160">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-cím: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="59180-160">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="59180-161">b.</span><span class="sxs-lookup"><span data-stu-id="59180-161">b.</span></span> <span data-ttu-id="59180-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="59180-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="59180-163">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="59180-163">Please note that this is not the real value.</span></span> <span data-ttu-id="59180-164">Ezt az értéket a tényleges bejelentkezési URL-cím frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="59180-164">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="59180-165">Lépjen kapcsolatba a Soonr munkahelyi terméktámogatással lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="59180-165">Contact Soonr Workplace support team to get this value.</span></span>

4. <span data-ttu-id="59180-166">Az a **Soonr munkahelyi egyszeri bejelentkezés konfigurálása** kattintson **metaadatok letöltése** és mentse a fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="59180-166">On the **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save the file on your computer:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="59180-168">Ahhoz, hogy az egyszeri bejelentkezés az alkalmazáshoz konfigurált, a Soonr munkahelyi támogatási csoportjához és adja meg a következő:</span><span class="sxs-lookup"><span data-stu-id="59180-168">To get SSO configured for your application, contact your Soonr Workplace support team and provide them with the following:</span></span> 

    <span data-ttu-id="59180-169">• A letöltött **metaadatok** fájl</span><span class="sxs-lookup"><span data-stu-id="59180-169">•  The downloaded **Metadata** file</span></span>

    <span data-ttu-id="59180-170">• A **kiállítójának URL-címe**</span><span class="sxs-lookup"><span data-stu-id="59180-170">•  The **Issuer URL**</span></span>

    <span data-ttu-id="59180-171">• A **SAML SSO URL-címe**</span><span class="sxs-lookup"><span data-stu-id="59180-171">•  The **SAML SSO URL**</span></span>

    <span data-ttu-id="59180-172">• A **egyszeri kijelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="59180-172">•  The **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="59180-173">Ezt az alkalmazást felváltotta <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask munkahelyi</a> és olvassa el a <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">ez</a> útmutató a az alkalmazás konfigurálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="59180-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring the application with Azure AD.</span></span>
   
6. <span data-ttu-id="59180-174">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="59180-174">In the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="59180-176">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="59180-176">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Az Azure AD-egyszeri bejelentkezés][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="59180-178">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="59180-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="59180-179">Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="59180-179">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>  

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="59180-181">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="59180-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="59180-182">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="59180-182">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="59180-184">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="59180-184">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="59180-185">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="59180-185">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="59180-187">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="59180-187">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="59180-189">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="59180-189">On the **Tell us about this user** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="59180-191">a.</span><span class="sxs-lookup"><span data-stu-id="59180-191">a.</span></span> <span data-ttu-id="59180-192">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="59180-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="59180-193">b.</span><span class="sxs-lookup"><span data-stu-id="59180-193">b.</span></span> <span data-ttu-id="59180-194">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="59180-194">In the User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="59180-195">c.</span><span class="sxs-lookup"><span data-stu-id="59180-195">c.</span></span> <span data-ttu-id="59180-196">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="59180-196">Click **Next**.</span></span>

6.  <span data-ttu-id="59180-197">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="59180-197">On the **User Profile** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="59180-199">a.</span><span class="sxs-lookup"><span data-stu-id="59180-199">a.</span></span> <span data-ttu-id="59180-200">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="59180-200">In the **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="59180-201">b.</span><span class="sxs-lookup"><span data-stu-id="59180-201">b.</span></span> <span data-ttu-id="59180-202">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="59180-202">In the **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="59180-203">c.</span><span class="sxs-lookup"><span data-stu-id="59180-203">c.</span></span> <span data-ttu-id="59180-204">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="59180-204">In the **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="59180-205">d.</span><span class="sxs-lookup"><span data-stu-id="59180-205">d.</span></span> <span data-ttu-id="59180-206">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="59180-206">In the **Role** list, select **User**.</span></span>

    <span data-ttu-id="59180-207">e.</span><span class="sxs-lookup"><span data-stu-id="59180-207">e.</span></span> <span data-ttu-id="59180-208">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="59180-208">Click **Next**.</span></span>

7. <span data-ttu-id="59180-209">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="59180-209">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="59180-211">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="59180-211">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="59180-213">a.</span><span class="sxs-lookup"><span data-stu-id="59180-213">a.</span></span> <span data-ttu-id="59180-214">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="59180-214">Write down the value of the **New Password**.</span></span>

    <span data-ttu-id="59180-215">b.</span><span class="sxs-lookup"><span data-stu-id="59180-215">b.</span></span> <span data-ttu-id="59180-216">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="59180-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="59180-217">Soonr munkahelyi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="59180-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="59180-218">Ez a szakasz célja Soonr munkahelyi Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="59180-218">The objective of this section is to create a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="59180-219">Felhasználó létrehozása a platform Soonr munkahelyi támogatási csapattal működik.</span><span class="sxs-lookup"><span data-stu-id="59180-219">Please work with Soonr Workplace support team to create a user in the platform.</span></span> <span data-ttu-id="59180-220">A támogatási jegy a Soonr rendelkező is növelheti <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">Itt</a>.</span><span class="sxs-lookup"><span data-stu-id="59180-220">You can raise the support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="59180-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="59180-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="59180-222">Ez a szakasz célja Britta Simon nyújtó saját Soonr munkahelyi Azure egyszeri bejelentkezéshez használandó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="59180-222">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Soonr Workplace.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="59180-224">**Britta Simon hozzárendelése Soonr munkahelyi, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="59180-224">**To assign Britta Simon to Soonr Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="59180-225">A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="59180-225">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="59180-227">Az alkalmazások listában válassza ki a **Soonr munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="59180-227">In the applications list, select **Soonr Workplace**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="59180-229">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="59180-229">In the menu on the top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203] 

1. <span data-ttu-id="59180-231">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="59180-231">In the Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="59180-232">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="59180-232">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="59180-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="59180-234">Testing single sign-on</span></span>

<span data-ttu-id="59180-235">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="59180-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="59180-236">Ha a hozzáférési panelen Soonr munkahelyi csempére kattint, akkor kell beolvasni automatikusan bejelentkezett az Soonr munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="59180-236">When you click the Soonr Workplace tile in the Access Panel, you should get automatically signed-on to your Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="59180-237">További források</span><span class="sxs-lookup"><span data-stu-id="59180-237">Additional resources</span></span>

* [<span data-ttu-id="59180-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="59180-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="59180-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="59180-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
