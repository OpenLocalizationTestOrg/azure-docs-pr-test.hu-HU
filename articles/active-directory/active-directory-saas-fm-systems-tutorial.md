---
title: "Oktatóanyag: Azure Active Directoryval integrált FM:Systems |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és FM:Systems között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3a597d228f6c9234ec2fd2644ec3ac50b98f3b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="bd4a0-103">Oktatóanyag: Azure Active Directoryval integrált FM:Systems</span><span class="sxs-lookup"><span data-stu-id="bd4a0-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="bd4a0-104">Ebben az oktatóanyagban elsajátíthatja FM:Systems integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bd4a0-104">In this tutorial, you learn how to integrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd4a0-105">FM:Systems integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-105">Integrating FM:Systems with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bd4a0-106">Megadhatja a FM:Systems hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bd4a0-106">You can control in Azure AD who has access to FM:Systems</span></span>
- <span data-ttu-id="bd4a0-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a FM:Systems (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bd4a0-107">You can enable your users to automatically get signed-on to FM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd4a0-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bd4a0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bd4a0-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd4a0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd4a0-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bd4a0-110">Prerequisites</span></span>

<span data-ttu-id="bd4a0-111">Konfigurálása az Azure AD-integrációs FM:Systems, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-111">To configure Azure AD integration with FM:Systems, you need the following items:</span></span>

- <span data-ttu-id="bd4a0-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bd4a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd4a0-113">Egy FM:Systems egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bd4a0-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd4a0-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd4a0-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd4a0-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd4a0-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd4a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd4a0-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-118">Scenario description</span></span>
<span data-ttu-id="bd4a0-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd4a0-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd4a0-121">A gyűjteményből FM:Systems hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-121">Adding FM:Systems from the gallery</span></span>
2. <span data-ttu-id="bd4a0-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bd4a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-the-gallery"></a><span data-ttu-id="bd4a0-123">A gyűjteményből FM:Systems hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-123">Adding FM:Systems from the gallery</span></span>
<span data-ttu-id="bd4a0-124">Az Azure AD integrálása a FM:Systems konfigurálásához kell hozzáadnia FM:Systems a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-124">To configure the integration of FM:Systems into Azure AD, you need to add FM:Systems from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bd4a0-125">**A gyűjteményből FM:Systems hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bd4a0-125">**To add FM:Systems from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bd4a0-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd4a0-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bd4a0-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bd4a0-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bd4a0-133">Írja be a keresőmezőbe, **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-133">In the search box, type **FM:Systems**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="bd4a0-135">Az eredmények panelen válassza ki a **FM:Systems**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-135">In the results panel, select **FM:Systems**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd4a0-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bd4a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd4a0-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bd4a0-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó FM:Systems a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FM:Systems is to a user in Azure AD.</span></span> <span data-ttu-id="bd4a0-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a FM:Systems közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-140">In other words, a link relationship between an Azure AD user and the related user in FM:Systems needs to be established.</span></span>

<span data-ttu-id="bd4a0-141">FM:Systems, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-141">In FM:Systems, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bd4a0-142">Az Azure AD egyszeri bejelentkezést a FM:Systems tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-142">To configure and test Azure AD single sign-on with FM:Systems, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bd4a0-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bd4a0-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd4a0-145">**[Egy FM:Systems tesztfelhasználó létrehozása](#creating-an-fmsystems-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó FM:Systems rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - to have a counterpart of Britta Simon in FM:Systems that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd4a0-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd4a0-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd4a0-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd4a0-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az FM:Systems alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="bd4a0-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés FM:Systems, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bd4a0-150">**To configure Azure AD single sign-on with FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="bd4a0-151">Az Azure portálon a a **FM:Systems** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-151">In the Azure portal, on the **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bd4a0-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="bd4a0-155">Az a **FM:Systems tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-155">On the **FM:Systems Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="bd4a0-157">Az a **válasz URL-CÍMEN** szövegmező, írja be a FM:Systems **válasz URL-CÍMEN**, írja be az URL-CÍMÉT a következő mintát:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="bd4a0-157">In the **Reply URL** textbox, type your FM:Systems **Reply URL**, type the URL using the following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bd4a0-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-158">This value is not real.</span></span> <span data-ttu-id="bd4a0-159">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="bd4a0-160">Ügyfél [FM:Systems támogatási csoport](https://fmsystems.com/ask-us/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) to get this value.</span></span>
 
4. <span data-ttu-id="bd4a0-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="bd4a0-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bd4a0-165">Egyszeri bejelentkezés konfigurálása **FM:Systems** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [FM:Systems támogatási csoport](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="bd4a0-165">To configure single sign-on on **FM:Systems** side, you need to send the downloaded **Metadata XML** to [FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="bd4a0-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="bd4a0-167">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="bd4a0-168">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="bd4a0-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bd4a0-169">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bd4a0-170">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd4a0-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd4a0-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd4a0-172">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bd4a0-174">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bd4a0-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bd4a0-175">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd4a0-177">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd4a0-179">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd4a0-181">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd4a0-183">a.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-183">a.</span></span> <span data-ttu-id="bd4a0-184">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd4a0-185">b.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-185">b.</span></span> <span data-ttu-id="bd4a0-186">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd4a0-187">c.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-187">c.</span></span> <span data-ttu-id="bd4a0-188">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bd4a0-189">d.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-189">d.</span></span> <span data-ttu-id="bd4a0-190">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="bd4a0-191">Egy FM:Systems tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="bd4a0-192">A webböngésző ablakának jelentkezzen be a FM:Systems vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="bd4a0-193">Ugrás a **rendszerfelügyelet \> kezelése biztonsági \> felhasználók \> Felhasználólista**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-193">Go to **System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="bd4a0-194">![Rendszer-felügyeleti](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "rendszerfelügyelet")</span><span class="sxs-lookup"><span data-stu-id="bd4a0-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="bd4a0-195">Kattintson a **új felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="bd4a0-196">![Új felhasználó létrehozása](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "új felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="bd4a0-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="bd4a0-197">Az a **felhasználó létrehozása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bd4a0-197">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="bd4a0-198">![Hozzon létre felhasználói](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="bd4a0-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="bd4a0-199">a.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-199">a.</span></span> <span data-ttu-id="bd4a0-200">Típus a **felhasználónév**, a **jelszó**, **jelszó megerősítése**, **E-mail** és a **Alkalmazottazonosító** egy érvényes Azure Active Directory-fiók szeretné azokat a kapcsolódó szövegmezők kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-200">Type the **UserName**, the **Password**, **Confirm Password**, **E-mail** and the **Employee ID** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="bd4a0-201">b.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-201">b.</span></span> <span data-ttu-id="bd4a0-202">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-202">Click **Next**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bd4a0-203">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bd4a0-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bd4a0-204">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó FM:Systems való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FM:Systems.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bd4a0-206">**Britta Simon hozzárendelése FM:Systems, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bd4a0-206">**To assign Britta Simon to FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="bd4a0-207">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bd4a0-209">Az alkalmazások listában válassza ki a **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-209">In the applications list, select **FM:Systems**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="bd4a0-211">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bd4a0-213">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-213">Click **Add** button.</span></span> <span data-ttu-id="bd4a0-214">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bd4a0-216">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bd4a0-217">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd4a0-218">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd4a0-219">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bd4a0-219">Testing single sign-on</span></span>

<span data-ttu-id="bd4a0-220">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bd4a0-221">Ha a hozzáférési panelen FM:Systems csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az FM:Systems alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="bd4a0-221">When you click the FM:Systems tile in the Access Panel, you should get automatically signed-on to your FM:Systems application.</span></span>
<span data-ttu-id="bd4a0-222">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bd4a0-222">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd4a0-223">További források</span><span class="sxs-lookup"><span data-stu-id="bd4a0-223">Additional resources</span></span>

* [<span data-ttu-id="bd4a0-224">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="bd4a0-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd4a0-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bd4a0-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

