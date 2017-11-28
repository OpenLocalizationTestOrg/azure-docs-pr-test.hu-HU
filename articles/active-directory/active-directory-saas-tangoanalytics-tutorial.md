---
title: "Oktatóanyag: Azure Active Directoryval integrált Tango Analytics |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Tango Analytics között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2f7555d3-e9ba-40b2-9b3a-2f0ab38a4c08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 46f6facc3c86630a9252340b2e89634368c0263d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tango-analytics"></a><span data-ttu-id="3639b-103">Oktatóanyag: Azure Active Directoryval integrált Tango elemzés</span><span class="sxs-lookup"><span data-stu-id="3639b-103">Tutorial: Azure Active Directory integration with Tango Analytics</span></span>

<span data-ttu-id="3639b-104">Ebben az oktatóanyagban elsajátíthatja Tango Analytics integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3639b-104">In this tutorial, you learn how to integrate Tango Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3639b-105">Tango Analytics integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3639b-105">Integrating Tango Analytics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3639b-106">Szabályozhatja az Azure AD, aki hozzáfér Tango elemzés</span><span class="sxs-lookup"><span data-stu-id="3639b-106">You can control in Azure AD who has access to Tango Analytics</span></span>
- <span data-ttu-id="3639b-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Tango Analytics (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="3639b-107">You can enable your users to automatically get signed-on to Tango Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3639b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3639b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3639b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3639b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3639b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3639b-110">Prerequisites</span></span>

<span data-ttu-id="3639b-111">Konfigurálása az Azure AD-integrációs Tango Analytics, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3639b-111">To configure Azure AD integration with Tango Analytics, you need the following items:</span></span>

- <span data-ttu-id="3639b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3639b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3639b-113">Egy Tango Analytics egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3639b-113">A Tango Analytics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3639b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3639b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3639b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3639b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3639b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3639b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3639b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3639b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3639b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3639b-118">Scenario description</span></span>
<span data-ttu-id="3639b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3639b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3639b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3639b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3639b-121">A gyűjteményből Tango Analytics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3639b-121">Adding Tango Analytics from the gallery</span></span>
2. <span data-ttu-id="3639b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3639b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tango-analytics-from-the-gallery"></a><span data-ttu-id="3639b-123">A gyűjteményből Tango Analytics hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3639b-123">Adding Tango Analytics from the gallery</span></span>
<span data-ttu-id="3639b-124">Az Azure AD integrálása a Tango Analytics konfigurálásához kell hozzáadnia Tango Analytics a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3639b-124">To configure the integration of Tango Analytics into Azure AD, you need to add Tango Analytics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3639b-125">**A gyűjteményből Tango Analytics hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3639b-125">**To add Tango Analytics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3639b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3639b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3639b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3639b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3639b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3639b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3639b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3639b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3639b-133">Írja be a keresőmezőbe, **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="3639b-133">In the search box, type **Tango Analytics**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_search.png)

5. <span data-ttu-id="3639b-135">Az eredmények panelen válassza ki a **Tango Analytics**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3639b-135">In the results panel, select **Tango Analytics**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3639b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3639b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3639b-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Tango Analytics "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="3639b-138">In this section, you configure and test Azure AD single sign-on with Tango Analytics based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3639b-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Tango Analytics egy felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3639b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tango Analytics is to a user in Azure AD.</span></span> <span data-ttu-id="3639b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Tango Analytics közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3639b-140">In other words, a link relationship between an Azure AD user and the related user in Tango Analytics needs to be established.</span></span>

<span data-ttu-id="3639b-141">Tango Analytics, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3639b-141">In Tango Analytics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3639b-142">Az Azure AD egyszeri bejelentkezést az Tango Analytics tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3639b-142">To configure and test Azure AD single sign-on with Tango Analytics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3639b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3639b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3639b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3639b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3639b-145">**[Tango Analytics tesztfelhasználó létrehozása](#creating-a-tango-analytics-test-user)**  - való egy megfelelője a Britta Simon Tango elemzés, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3639b-145">**[Creating a Tango Analytics test user](#creating-a-tango-analytics-test-user)** - to have a counterpart of Britta Simon in Tango Analytics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3639b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3639b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3639b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3639b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3639b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3639b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3639b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Tango Analytics alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3639b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tango Analytics application.</span></span>

<span data-ttu-id="3639b-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Tango elemzés, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3639b-150">**To configure Azure AD single sign-on with Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="3639b-151">Az Azure portálon a a **Tango Analytics** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3639b-151">In the Azure portal, on the **Tango Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3639b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3639b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_samlbase.png)

3. <span data-ttu-id="3639b-155">Az a **Tango Analytics tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3639b-155">On the **Tango Analytics Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_url.png)

    <span data-ttu-id="3639b-157">a.</span><span class="sxs-lookup"><span data-stu-id="3639b-157">a.</span></span> <span data-ttu-id="3639b-158">Az a **azonosító** szövegmező, írja be az értéket`TACORE_SSO`</span><span class="sxs-lookup"><span data-stu-id="3639b-158">In the **Identifier** textbox, type the value `TACORE_SSO`</span></span>

    <span data-ttu-id="3639b-159">b.</span><span class="sxs-lookup"><span data-stu-id="3639b-159">b.</span></span> <span data-ttu-id="3639b-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://mts.tangoanalytics.com/saml2/sp/acs/post`</span><span class="sxs-lookup"><span data-stu-id="3639b-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://mts.tangoanalytics.com/saml2/sp/acs/post`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3639b-161">A válasz URL-cím értéke nincs valós.</span><span class="sxs-lookup"><span data-stu-id="3639b-161">The Reply URL value is not real.</span></span> <span data-ttu-id="3639b-162">Ez frissíti a tényleges válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="3639b-162">Update this with the actual Reply URL.</span></span> <span data-ttu-id="3639b-163">Ügyfél [Tango Analytics támogatási csoport](mailto:support@tangoanalytics.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="3639b-163">Contact [Tango Analytics support team](mailto:support@tangoanalytics.com) to get this value.</span></span>

4. <span data-ttu-id="3639b-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3639b-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_certificate.png) 

5. <span data-ttu-id="3639b-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3639b-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3639b-168">Egyszeri bejelentkezés konfigurálása **Tango Analytics** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Tango Analytics támogatási csoport](mailto:support@tangoanalytics.com).</span><span class="sxs-lookup"><span data-stu-id="3639b-168">To configure single sign-on on **Tango Analytics** side, you need to send the downloaded **Metadata XML** to [Tango Analytics support team](mailto:support@tangoanalytics.com).</span></span> <span data-ttu-id="3639b-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="3639b-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3639b-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3639b-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3639b-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3639b-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3639b-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3639b-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3639b-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3639b-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="3639b-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3639b-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3639b-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3639b-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3639b-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3639b-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3639b-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3639b-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3639b-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3639b-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3639b-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3639b-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3639b-185">a.</span><span class="sxs-lookup"><span data-stu-id="3639b-185">a.</span></span> <span data-ttu-id="3639b-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3639b-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3639b-187">b.</span><span class="sxs-lookup"><span data-stu-id="3639b-187">b.</span></span> <span data-ttu-id="3639b-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3639b-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3639b-189">c.</span><span class="sxs-lookup"><span data-stu-id="3639b-189">c.</span></span> <span data-ttu-id="3639b-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3639b-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3639b-191">d.</span><span class="sxs-lookup"><span data-stu-id="3639b-191">d.</span></span> <span data-ttu-id="3639b-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3639b-192">Click **Create**.</span></span>
 
### <a name="creating-a-tango-analytics-test-user"></a><span data-ttu-id="3639b-193">Tango Analytics tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3639b-193">Creating a Tango Analytics test user</span></span>

<span data-ttu-id="3639b-194">Ebben a szakaszban egy Britta Simon Tango Analytics nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3639b-194">In this section, you create a user called Britta Simon in Tango Analytics.</span></span> <span data-ttu-id="3639b-195">Együttműködve [Tango Analytics támogatási csoport](mailto:support@tangoanalytics.com) a felhasználók hozzáadása a Tango Analytics platform.</span><span class="sxs-lookup"><span data-stu-id="3639b-195">Work with [Tango Analytics support team](mailto:support@tangoanalytics.com) to add the users in the Tango Analytics platform.</span></span> <span data-ttu-id="3639b-196">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="3639b-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3639b-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3639b-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3639b-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Tango Analytics Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3639b-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tango Analytics.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3639b-200">**Britta Simon hozzárendelése Tango elemzés, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3639b-200">**To assign Britta Simon to Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="3639b-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3639b-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3639b-203">Az alkalmazások listában válassza ki a **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="3639b-203">In the applications list, select **Tango Analytics**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_app.png) 

3. <span data-ttu-id="3639b-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3639b-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3639b-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3639b-207">Click **Add** button.</span></span> <span data-ttu-id="3639b-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3639b-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3639b-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3639b-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3639b-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3639b-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3639b-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3639b-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3639b-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3639b-213">Testing single sign-on</span></span>

<span data-ttu-id="3639b-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3639b-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3639b-215">Ha a hozzáférési panelen Tango Analytics csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Tango Analytics alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3639b-215">When you click the Tango Analytics tile in the Access Panel, you should get automatically signed-on to your Tango Analytics application.</span></span>
<span data-ttu-id="3639b-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3639b-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3639b-217">További források</span><span class="sxs-lookup"><span data-stu-id="3639b-217">Additional resources</span></span>

* [<span data-ttu-id="3639b-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3639b-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3639b-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3639b-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_203.png

