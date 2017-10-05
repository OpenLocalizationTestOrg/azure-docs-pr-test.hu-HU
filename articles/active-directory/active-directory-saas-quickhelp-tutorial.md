---
title: "Oktatóanyag: Azure Active Directoryval integrált QuickHelp |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és QuickHelp között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 1c72b0ddee636090129dab7a5c7ec6ffd452434a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a><span data-ttu-id="2913f-103">Oktatóanyag: Azure Active Directoryval integrált QuickHelp</span><span class="sxs-lookup"><span data-stu-id="2913f-103">Tutorial: Azure Active Directory integration with QuickHelp</span></span>

<span data-ttu-id="2913f-104">Ebben az oktatóanyagban elsajátíthatja QuickHelp integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2913f-104">In this tutorial, you learn how to integrate QuickHelp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2913f-105">QuickHelp integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2913f-105">Integrating QuickHelp with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2913f-106">Megadhatja a QuickHelp hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2913f-106">You can control in Azure AD who has access to QuickHelp</span></span>
- <span data-ttu-id="2913f-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett QuickHelp (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2913f-107">You can enable your users to automatically get signed-on to QuickHelp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2913f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2913f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2913f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2913f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2913f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2913f-110">Prerequisites</span></span>

<span data-ttu-id="2913f-111">Konfigurálása az Azure AD-integrációs QuickHelp, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="2913f-111">To configure Azure AD integration with QuickHelp, you need the following items:</span></span>

- <span data-ttu-id="2913f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2913f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2913f-113">Egy QuickHelp egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2913f-113">A QuickHelp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2913f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="2913f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2913f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="2913f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2913f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2913f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2913f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2913f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2913f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2913f-118">Scenario description</span></span>
<span data-ttu-id="2913f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2913f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2913f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2913f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2913f-121">A gyűjteményből QuickHelp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2913f-121">Adding QuickHelp from the gallery</span></span>
2. <span data-ttu-id="2913f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2913f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-quickhelp-from-the-gallery"></a><span data-ttu-id="2913f-123">A gyűjteményből QuickHelp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2913f-123">Adding QuickHelp from the gallery</span></span>
<span data-ttu-id="2913f-124">Az Azure AD integrálása a QuickHelp konfigurálásához kell hozzáadnia QuickHelp a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="2913f-124">To configure the integration of QuickHelp into Azure AD, you need to add QuickHelp from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2913f-125">**A gyűjteményből QuickHelp hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2913f-125">**To add QuickHelp from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2913f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2913f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2913f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2913f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2913f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2913f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2913f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="2913f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2913f-133">Írja be a keresőmezőbe, **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="2913f-133">In the search box, type **QuickHelp**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. <span data-ttu-id="2913f-135">Az eredmények panelen válassza ki a **QuickHelp**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2913f-135">In the results panel, select **QuickHelp**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2913f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2913f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2913f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="2913f-138">In this section, you configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2913f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó QuickHelp a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="2913f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in QuickHelp is to a user in Azure AD.</span></span> <span data-ttu-id="2913f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a QuickHelp közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2913f-140">In other words, a link relationship between an Azure AD user and the related user in QuickHelp needs to be established.</span></span>

<span data-ttu-id="2913f-141">QuickHelp, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="2913f-141">In QuickHelp, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2913f-142">Az Azure AD egyszeri bejelentkezést a QuickHelp tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="2913f-142">To configure and test Azure AD single sign-on with QuickHelp, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2913f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="2913f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2913f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="2913f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2913f-145">**[QuickHelp tesztfelhasználó létrehozása](#creating-a-quickhelp-test-user)**  - való Britta Simon valami QuickHelp, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="2913f-145">**[Creating a QuickHelp test user](#creating-a-quickhelp-test-user)** - to have a counterpart of Britta Simon in QuickHelp that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2913f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2913f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2913f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2913f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2913f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2913f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2913f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az QuickHelp alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2913f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your QuickHelp application.</span></span>

<span data-ttu-id="2913f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés QuickHelp, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2913f-150">**To configure Azure AD single sign-on with QuickHelp, perform the following steps:**</span></span>

1. <span data-ttu-id="2913f-151">Az Azure portálon a a **QuickHelp** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2913f-151">In the Azure portal, on the **QuickHelp** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2913f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2913f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. <span data-ttu-id="2913f-155">Az a **QuickHelp tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2913f-155">On the **QuickHelp Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    <span data-ttu-id="2913f-157">a.</span><span class="sxs-lookup"><span data-stu-id="2913f-157">a.</span></span> <span data-ttu-id="2913f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://quickhelp.com/<instancename>/#/Login`</span><span class="sxs-lookup"><span data-stu-id="2913f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://quickhelp.com/<instancename>/#/Login`</span></span>

    <span data-ttu-id="2913f-159">b.</span><span class="sxs-lookup"><span data-stu-id="2913f-159">b.</span></span> <span data-ttu-id="2913f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.quickhelp.com`</span><span class="sxs-lookup"><span data-stu-id="2913f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.quickhelp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2913f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2913f-161">These values are not real.</span></span> <span data-ttu-id="2913f-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2913f-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2913f-163">Ügyfél [QuickHelp ügyfél-támogatási csoport](https://support.quickhelp.com/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2913f-163">Contact [QuickHelp Client support team](https://support.quickhelp.com/) to get these values.</span></span> 
 
4. <span data-ttu-id="2913f-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2913f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. <span data-ttu-id="2913f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2913f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="2913f-168">Bejelentkezés a QuickHelp vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="2913f-168">Sign-on to your QuickHelp company site as administrator.</span></span>

7. <span data-ttu-id="2913f-169">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="2913f-169">In the menu on the top, click **Admin**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][21]

8. <span data-ttu-id="2913f-171">Az a **QuickHelp Admin** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2913f-171">In the **QuickHelp Admin** menu, click **Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][22]

9. <span data-ttu-id="2913f-173">Kattintson a **hitelesítési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2913f-173">Click **Authentication Settings**.</span></span>

10. <span data-ttu-id="2913f-174">Az a **hitelesítési beállítások** lapon, a következő lépések</span><span class="sxs-lookup"><span data-stu-id="2913f-174">On the **Authentication Settings** page, perform the following steps</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    <span data-ttu-id="2913f-176">a.</span><span class="sxs-lookup"><span data-stu-id="2913f-176">a.</span></span> <span data-ttu-id="2913f-177">Mint **egyszeri bejelentkezés típusa**, jelölje be **WSFederation**.</span><span class="sxs-lookup"><span data-stu-id="2913f-177">As **SSO Type**, select **WSFederation**.</span></span>
   
    <span data-ttu-id="2913f-178">b.</span><span class="sxs-lookup"><span data-stu-id="2913f-178">b.</span></span> <span data-ttu-id="2913f-179">A letöltött Azure metaadatok fájl feltöltéséhez, kattintson **Tallózás**, keresse meg a fájlt, kattintson end **metaadatok feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="2913f-179">To upload your downloaded Azure metadata file, click **Browse**, navigate to the file, end then click **Upload Metadata**.</span></span>
   
    <span data-ttu-id="2913f-180">c.</span><span class="sxs-lookup"><span data-stu-id="2913f-180">c.</span></span> <span data-ttu-id="2913f-181">Az a **E-mail** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="2913f-181">In the **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
   
    <span data-ttu-id="2913f-182">d.</span><span class="sxs-lookup"><span data-stu-id="2913f-182">d.</span></span> <span data-ttu-id="2913f-183">Az a **Utónév** szövegmezőhöz `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="2913f-183">In the **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
   
    <span data-ttu-id="2913f-184">e.</span><span class="sxs-lookup"><span data-stu-id="2913f-184">e.</span></span> <span data-ttu-id="2913f-185">Az a **Vezetéknév** szövegmezőhöz `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="2913f-185">In the **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
   
    <span data-ttu-id="2913f-186">f.</span><span class="sxs-lookup"><span data-stu-id="2913f-186">f.</span></span> <span data-ttu-id="2913f-187">Az a **műveletsávon**, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2913f-187">In the **Action Bar**, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2913f-188">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="2913f-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2913f-189">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="2913f-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2913f-190">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2913f-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2913f-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2913f-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="2913f-192">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="2913f-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2913f-194">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2913f-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2913f-195">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2913f-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2913f-197">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2913f-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2913f-199">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="2913f-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2913f-201">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="2913f-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2913f-203">a.</span><span class="sxs-lookup"><span data-stu-id="2913f-203">a.</span></span> <span data-ttu-id="2913f-204">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2913f-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2913f-205">b.</span><span class="sxs-lookup"><span data-stu-id="2913f-205">b.</span></span> <span data-ttu-id="2913f-206">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2913f-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2913f-207">c.</span><span class="sxs-lookup"><span data-stu-id="2913f-207">c.</span></span> <span data-ttu-id="2913f-208">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2913f-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2913f-209">d.</span><span class="sxs-lookup"><span data-stu-id="2913f-209">d.</span></span> <span data-ttu-id="2913f-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2913f-210">Click **Create**.</span></span>
 
### <a name="creating-a-quickhelp-test-user"></a><span data-ttu-id="2913f-211">QuickHelp tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2913f-211">Creating a QuickHelp test user</span></span>

<span data-ttu-id="2913f-212">Ez a szakasz célja QuickHelp Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2913f-212">The objective of this section is to create a user called Britta Simon in QuickHelp.</span></span>
<span data-ttu-id="2913f-213">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó QuickHelp az Azure AD-felhasználóhoz van.</span><span class="sxs-lookup"><span data-stu-id="2913f-213">For single sign-on to work, Azure AD needs to know what the counterpart user in QuickHelp to a user in Azure AD is.</span></span> <span data-ttu-id="2913f-214">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a QuickHelp közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2913f-214">In other words, a link relationship between an Azure AD user and the related user in QuickHelp needs to be established.</span></span>

<span data-ttu-id="2913f-215">QuickHelp támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="2913f-215">QuickHelp supports just-in-time provisioning.</span></span> <span data-ttu-id="2913f-216">Ez azt jelenti, ha szükséges, a felhasználói fiók automatikusan létrejön a QuickHelp és a fiók kapcsolódik az Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="2913f-216">This means, if necessary, a user account is automatically created in QuickHelp and the account is linked to the Azure AD account.</span></span>

<span data-ttu-id="2913f-217">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="2913f-217">There is no action item for you in this section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2913f-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2913f-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2913f-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés QuickHelp Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="2913f-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to QuickHelp.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2913f-221">**Britta Simon hozzárendelése QuickHelp, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2913f-221">**To assign Britta Simon to QuickHelp, perform the following steps:**</span></span>

1. <span data-ttu-id="2913f-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2913f-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2913f-224">Az alkalmazások listában válassza ki a **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="2913f-224">In the applications list, select **QuickHelp**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. <span data-ttu-id="2913f-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2913f-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2913f-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2913f-228">Click **Add** button.</span></span> <span data-ttu-id="2913f-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2913f-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2913f-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2913f-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2913f-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2913f-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2913f-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2913f-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2913f-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2913f-234">Testing single sign-on</span></span>

<span data-ttu-id="2913f-235">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="2913f-235">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="2913f-236">Ha a hozzáférési panelen QuickHelp csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az QuickHelp alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="2913f-236">When you click the QuickHelp tile in the Access Panel, you should get automatically signed-on to your QuickHelp application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2913f-237">További források</span><span class="sxs-lookup"><span data-stu-id="2913f-237">Additional resources</span></span>

* [<span data-ttu-id="2913f-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="2913f-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2913f-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2913f-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
