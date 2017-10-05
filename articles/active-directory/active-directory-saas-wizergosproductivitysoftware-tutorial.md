---
title: "Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a termelékenység szoftver Wizergos között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="ad70b-103">Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel</span><span class="sxs-lookup"><span data-stu-id="ad70b-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="ad70b-104">Ez az oktatóanyag célja Wizergos termelékenység szoftver integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="ad70b-104">The objective of this tutorial is to show you how to integrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ad70b-105">Wizergos termelékenység szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ad70b-105">Integrating Wizergos Productivity Software with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="ad70b-106">Szabályozhatja az Azure AD, aki hozzáfér Wizergos termelékenység szoftver</span><span class="sxs-lookup"><span data-stu-id="ad70b-106">You can control in Azure AD who has access to Wizergos Productivity Software</span></span>
* <span data-ttu-id="ad70b-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Wizergos termelékenység szoftver egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ad70b-107">You can enable your users to automatically get signed-on to Wizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="ad70b-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="ad70b-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="ad70b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ad70b-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad70b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad70b-110">Prerequisites</span></span>
<span data-ttu-id="ad70b-111">Az Azure AD-integráció konfigurálása Wizergos termelékenység szoftverrel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ad70b-111">To configure Azure AD integration with Wizergos Productivity Software, you need the following items:</span></span>

* <span data-ttu-id="ad70b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ad70b-112">An Azure AD subscription</span></span>
* <span data-ttu-id="ad70b-113">Egy Wizergos termelékenység szoftver SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ad70b-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="ad70b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ad70b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="ad70b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ad70b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="ad70b-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ad70b-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="ad70b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad70b-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ad70b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ad70b-118">Scenario description</span></span>
<span data-ttu-id="ad70b-119">Ez az oktatóanyag célja ahhoz, hogy az Azure AD SSO teszteléséhez tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ad70b-119">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="ad70b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ad70b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ad70b-121">A gyűjteményből Wizergos termelékenység szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ad70b-121">Adding Wizergos Productivity Software from the gallery</span></span>
2. <span data-ttu-id="ad70b-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ad70b-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a><span data-ttu-id="ad70b-123">A gyűjteményből Wizergos termelékenység szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ad70b-123">Adding Wizergos Productivity Software from the gallery</span></span>
<span data-ttu-id="ad70b-124">Az Azure AD integrálása a Wizergos termelékenység szoftver konfigurálásához kell hozzáadnia Wizergos termelékenység szoftver a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ad70b-124">To configure the integration of Wizergos Productivity Software into Azure AD, you need to add Wizergos Productivity Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ad70b-125">**Wizergos termelékenység szoftver hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ad70b-125">**To add Wizergos Productivity Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ad70b-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="ad70b-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="ad70b-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="ad70b-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="ad70b-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="ad70b-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="ad70b-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="ad70b-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="ad70b-135">Írja be a keresőmezőbe, **Wizergos termelékenység szoftver**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-135">In the search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="ad70b-137">Az eredmények panelen válassza ki a **Wizergos termelékenység szoftver**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ad70b-137">In the results panel, select **Wizergos Productivity Software**, and then click **Complete** to add the application.</span></span>
   
    ![Az alkalmazás kiválasztása a katalógusban](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="ad70b-139">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ad70b-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="ad70b-140">Ez a szakasz célja mutatjuk be, hogyan lehet konfigurálni és tesztelése az Azure AD SSO "Britta Simon" nevű tesztfelhasználó alapján Wizergos termelékenység szoftverrel.</span><span class="sxs-lookup"><span data-stu-id="ad70b-140">The objective of this section is to show you how to configure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ad70b-141">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a partner felhasználó Wizergos termelékenység szoftver egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="ad70b-141">For SSO to work, Azure AD needs to know what the counterpart user in Wizergos Productivity Software to an user in Azure AD is.</span></span> <span data-ttu-id="ad70b-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Wizergos termelékenység szoftver közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ad70b-142">In other words, a link relationship between an Azure AD user and the related user in Wizergos Productivity Software needs to be established.</span></span>

<span data-ttu-id="ad70b-143">A hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Wizergos termelékenység szoftverben.</span><span class="sxs-lookup"><span data-stu-id="ad70b-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="ad70b-144">Az Azure AD egyszeri bejelentkezést a BynWizergos termelékenység Softwareder tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ad70b-144">To configure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ad70b-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ad70b-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ad70b-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ad70b-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ad70b-147">**[Wizergos termelékenység szoftver tesztfelhasználó létrehozása](#creating-a-wizergos-productivity-software-test-user)**  - való egy megfelelője a Britta Simon Wizergos termelékenység szoftver, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ad70b-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - to have a counterpart of Britta Simon in Wizergos Productivity Software that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ad70b-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ad70b-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ad70b-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ad70b-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="ad70b-150">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ad70b-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="ad70b-151">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a klasszikus portálon, és a Wizergos termelékenység alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ad70b-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="ad70b-152">**Konfigurálja az Azure AD az egyszeri bejelentkezés Wizergos termelékenység szoftverrel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ad70b-152">**To configure Azure AD single sign-on with Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="ad70b-153">A klasszikus portálon a a **Wizergos termelékenység szoftver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ad70b-153">In the classic portal, on the **Wizergos Productivity Software** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="ad70b-155">A a **hová felhasználók bejelentkezhetnek a Wizergos termelékenység szoftver** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="ad70b-155">On the **How would you like users to sign on to Wizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="ad70b-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lap, kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="ad70b-157">On the **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="ad70b-159">A a **konfigurálhatja az egyszeri bejelentkezés Wizergos termelékenység szoftver** kattintson **tanúsítvánnyal letöltés**, majd mentse a fájlt a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="ad70b-159">On the **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save the file on your computer:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="ad70b-161">Egy másik webes böngészőablakban bejelentkezés a Wizergos termelékenység szoftver bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ad70b-161">In a different web browser window, sign-on to your Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="ad70b-162">A Hamburg menüben válassza ki a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-162">From the hamburger menu, select **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="ad70b-164">A rendszergazda lap bal oldali menüben válassza ki **hitelesítési** , majd kattintson a **az Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="ad70b-166">Hajtsa végre a következő lépéseket **hitelesítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ad70b-166">Perform the following steps on **AUTHENTICATION** section.</span></span>
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="ad70b-168">Kattintson a **FELTÖLTÉSE** gomb az Azure AD a letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="ad70b-168">Click **UPLOAD** button to upload the downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="ad70b-169">Az a **kiállítójának URL-címe** szövegmezőbe írja be az értéket a **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="ad70b-169">In the **Issuer URL** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="ad70b-170">Az a **egyszeri bejelentkezési URL-cím** szövegmezőbe írja be az értéket a **egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="ad70b-170">In the **Single Sign-On URL** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="ad70b-171">Az a **egyetlen Sign-Out URL-címet** szövegmezőbe írja be az értéket a **egyetlen Sign-out URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="ad70b-171">In the **Single Sign-Out URL** textbox put the value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="ad70b-172">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad70b-172">Click **Save** button.</span></span>
9. <span data-ttu-id="ad70b-173">A klasszikus portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
10. <span data-ttu-id="ad70b-175">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ad70b-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="ad70b-177">Create an Azure AD test user</span></span>
<span data-ttu-id="ad70b-178">Ez a szakasz célja a tesztfelhasználó létrehozása Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="ad70b-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="ad70b-180">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ad70b-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ad70b-181">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="ad70b-183">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="ad70b-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="ad70b-184">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="ad70b-186">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="ad70b-188">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ad70b-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="ad70b-190">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="ad70b-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="ad70b-191">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="ad70b-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad70b-192">Click **Next**.</span></span>
6. <span data-ttu-id="ad70b-193">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ad70b-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="ad70b-195">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="ad70b-196">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-196">In the **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="ad70b-197">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="ad70b-198">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="ad70b-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad70b-199">Click **Next**.</span></span>
7. <span data-ttu-id="ad70b-200">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="ad70b-202">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ad70b-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="ad70b-204">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-204">Write down the value of the **New Password**.</span></span>
  2. <span data-ttu-id="ad70b-205">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ad70b-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="ad70b-206">Wizergos termelékenység szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad70b-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="ad70b-207">Ebben a szakaszban egy felhasználó Britta Simon meghívta Wizergos termelékenység szoftver hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ad70b-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="ad70b-208">Adjon Wizergos termelékenység szoftver támogatási csoport keresztül együttműködésre [ support@wizergos.com ](emailTo:support@wizergos.com) a felhasználók hozzáadása a Wizergos termelékenység szoftver platform.</span><span class="sxs-lookup"><span data-stu-id="ad70b-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) to add the users in the Wizergos Productivity Software platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ad70b-209">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="ad70b-209">Assign the Azure AD test user</span></span>
<span data-ttu-id="ad70b-210">Ez a szakasz célja Britta Simon nyújtó saját Wizergos termelékenység szoftver Azure SSO használandó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ad70b-210">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Wizergos Productivity Software.</span></span>

  ![Felhasználó hozzárendelése][200]

<span data-ttu-id="ad70b-212">**Britta Simon hozzárendelése Wizergos termelékenység szoftver, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ad70b-212">**To assign Britta Simon to Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="ad70b-213">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="ad70b-213">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201]
2. <span data-ttu-id="ad70b-215">Az alkalmazások listában válassza ki a **Wizergos termelékenység szoftver**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-215">In the applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="ad70b-217">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-217">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203]
4. <span data-ttu-id="ad70b-219">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-219">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="ad70b-220">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="ad70b-220">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="ad70b-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ad70b-222">Test single sign-on</span></span>
<span data-ttu-id="ad70b-223">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ad70b-223">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ad70b-224">Ha a hozzáférési panelen Wizergos termelékenység szoftver csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Wizergos termelékenység szoftverfrissítések alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="ad70b-224">When you click the Wizergos Productivity Software tile in the Access Panel, you should get automatically signed-on to your Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ad70b-225">További források</span><span class="sxs-lookup"><span data-stu-id="ad70b-225">Additional resources</span></span>
* [<span data-ttu-id="ad70b-226">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ad70b-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ad70b-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ad70b-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
