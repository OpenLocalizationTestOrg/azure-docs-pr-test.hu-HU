---
title: "Oktatóanyag: Azure Active Directoryval integrált által Desire2Learn Brightspace |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Brightspace által Desire2Learn között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7076b476ba71c5d94ae4728e5f6032b0d7e047ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a><span data-ttu-id="e2f93-103">Oktatóanyag: Azure Active Directoryval integrált Brightspace Desire2Learn által</span><span class="sxs-lookup"><span data-stu-id="e2f93-103">Tutorial: Azure Active Directory integration with Brightspace by Desire2Learn</span></span>

<span data-ttu-id="e2f93-104">Ebben az oktatóanyagban elsajátíthatja által Desire2Learn Brightspace integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e2f93-104">In this tutorial, you learn how to integrate Brightspace by Desire2Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e2f93-105">Brightspace által Desire2Learn integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e2f93-105">Integrating Brightspace by Desire2Learn with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e2f93-106">Az Azure AD, aki hozzáfér Brightspace Desire2Learn által szabályozható</span><span class="sxs-lookup"><span data-stu-id="e2f93-106">You can control in Azure AD who has access to Brightspace by Desire2Learn</span></span>
- <span data-ttu-id="e2f93-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Brightspace által Desire2Learn (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e2f93-107">You can enable your users to automatically get signed-on to Brightspace by Desire2Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e2f93-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e2f93-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e2f93-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2f93-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2f93-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e2f93-110">Prerequisites</span></span>

<span data-ttu-id="e2f93-111">Konfigurálása az Azure AD-integrációs Brightspace Desire2Learn által, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="e2f93-111">To configure Azure AD integration with Brightspace by Desire2Learn, you need the following items:</span></span>

- <span data-ttu-id="e2f93-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e2f93-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e2f93-113">Egy Brightspace Desire2Learn egyszeri bejelentkezés által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="e2f93-113">A Brightspace by Desire2Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2f93-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e2f93-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e2f93-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e2f93-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e2f93-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e2f93-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e2f93-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2f93-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e2f93-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e2f93-118">Scenario description</span></span>
<span data-ttu-id="e2f93-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e2f93-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e2f93-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e2f93-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e2f93-121">A gyűjteményből által Desire2Learn Brightspace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2f93-121">Adding Brightspace by Desire2Learn from the gallery</span></span>
2. <span data-ttu-id="e2f93-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e2f93-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-brightspace-by-desire2learn-from-the-gallery"></a><span data-ttu-id="e2f93-123">A gyűjteményből által Desire2Learn Brightspace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e2f93-123">Adding Brightspace by Desire2Learn from the gallery</span></span>
<span data-ttu-id="e2f93-124">Az Azure AD integrálása a Brightspace által Desire2Learn konfigurálásához kell hozzáadnia Brightspace Desire2Learn által a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e2f93-124">To configure the integration of Brightspace by Desire2Learn into Azure AD, you need to add Brightspace by Desire2Learn from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e2f93-125">**Adja hozzá a Brightspace Desire2Learn által a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e2f93-125">**To add Brightspace by Desire2Learn from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e2f93-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e2f93-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e2f93-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e2f93-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e2f93-133">Írja be a keresőmezőbe, **által Desire2Learn Brightspace**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-133">In the search box, type **Brightspace by Desire2Learn**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. <span data-ttu-id="e2f93-135">Az eredmények panelen válassza ki a **által Desire2Learn Brightspace**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e2f93-135">In the results panel, select **Brightspace by Desire2Learn**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e2f93-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e2f93-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e2f93-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Brightspace "Britta Simon" nevű tesztfelhasználó alapján Desire2Learn által.</span><span class="sxs-lookup"><span data-stu-id="e2f93-138">In this section, you configure and test Azure AD single sign-on with Brightspace by Desire2Learn based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e2f93-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó által Desire2Learn Brightspace a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e2f93-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Brightspace by Desire2Learn is to a user in Azure AD.</span></span> <span data-ttu-id="e2f93-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó által Desire2Learn Brightspace közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e2f93-140">In other words, a link relationship between an Azure AD user and the related user in Brightspace by Desire2Learn needs to be established.</span></span>

<span data-ttu-id="e2f93-141">Brightspace Desire2Learn által, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e2f93-141">In Brightspace by Desire2Learn, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e2f93-142">Az Azure AD egyszeri bejelentkezést a Brightspace által Desire2Learn tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e2f93-142">To configure and test Azure AD single sign-on with Brightspace by Desire2Learn, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e2f93-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e2f93-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e2f93-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e2f93-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2f93-145">**[Egy Brightspace Desire2Learn teszt felhasználó létrehozása](#creating-a-brightspace-by-desire2learn-test-user)**  - való egy megfelelője a Britta Simon Brightspace által Desire2Learn, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e2f93-145">**[Creating a Brightspace by Desire2Learn test user](#creating-a-brightspace-by-desire2learn-test-user)** - to have a counterpart of Britta Simon in Brightspace by Desire2Learn that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e2f93-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e2f93-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2f93-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e2f93-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e2f93-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e2f93-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e2f93-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez a Brightspace Desire2Learn alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2f93-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Brightspace by Desire2Learn application.</span></span>

<span data-ttu-id="e2f93-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés által Desire2Learn Brightspace, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e2f93-150">**To configure Azure AD single sign-on with Brightspace by Desire2Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="e2f93-151">Az Azure portálon a a **által Desire2Learn Brightspace** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-151">In the Azure portal, on the **Brightspace by Desire2Learn** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e2f93-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e2f93-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. <span data-ttu-id="e2f93-155">Az a **Brightspace Desire2Learn tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e2f93-155">On the **Brightspace by Desire2Learn Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    <span data-ttu-id="e2f93-157">a.</span><span class="sxs-lookup"><span data-stu-id="e2f93-157">a.</span></span> <span data-ttu-id="e2f93-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="e2f93-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    <span data-ttu-id="e2f93-159">b.</span><span class="sxs-lookup"><span data-stu-id="e2f93-159">b.</span></span> <span data-ttu-id="e2f93-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span><span class="sxs-lookup"><span data-stu-id="e2f93-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e2f93-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e2f93-161">These values are not real.</span></span> <span data-ttu-id="e2f93-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="e2f93-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="e2f93-163">Ügyfél [Brightspace Desire2Learn támogatási csapat](https://www.d2l.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e2f93-163">Contact [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/) to get these values.</span></span>
 


4. <span data-ttu-id="e2f93-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e2f93-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. <span data-ttu-id="e2f93-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e2f93-168">Egyszeri bejelentkezés konfigurálása **által Desire2Learn Brightspace** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Brightspace Desire2Learn támogatási csapat](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="e2f93-168">To configure single sign-on on **Brightspace by Desire2Learn** side, you need to send the downloaded **Metadata XML** to [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="e2f93-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e2f93-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e2f93-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e2f93-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e2f93-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e2f93-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e2f93-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2f93-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="e2f93-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e2f93-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e2f93-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e2f93-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e2f93-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e2f93-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e2f93-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e2f93-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e2f93-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e2f93-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e2f93-184">a.</span><span class="sxs-lookup"><span data-stu-id="e2f93-184">a.</span></span> <span data-ttu-id="e2f93-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e2f93-186">b.</span><span class="sxs-lookup"><span data-stu-id="e2f93-186">b.</span></span> <span data-ttu-id="e2f93-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e2f93-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e2f93-188">c.</span><span class="sxs-lookup"><span data-stu-id="e2f93-188">c.</span></span> <span data-ttu-id="e2f93-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e2f93-190">d.</span><span class="sxs-lookup"><span data-stu-id="e2f93-190">d.</span></span> <span data-ttu-id="e2f93-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-191">Click **Create**.</span></span>
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a><span data-ttu-id="e2f93-192">Egy Brightspace által Desire2Learn tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2f93-192">Creating a Brightspace by Desire2Learn test user</span></span>

<span data-ttu-id="e2f93-193">Ahhoz, hogy az Azure AD-felhasználók által Desire2Learn Brightspace bejelentkezni, akkor ki kell építenie Brightspace történő Desire2Learn által.</span><span class="sxs-lookup"><span data-stu-id="e2f93-193">In order to enable Azure AD users to log into Brightspace by Desire2Learn, they must be provisioned into Brightspace by Desire2Learn.</span></span>  

<span data-ttu-id="e2f93-194">Esetén Brightspace Desire2Learn által, a felhasználói fiókokat kell létrehozni a [Desire2Learn támogatási csapat Brightspace](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="e2f93-194">In the case of Brightspace by Desire2Learn, the user accounts need to be created by your [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

>[!NOTE]
><span data-ttu-id="e2f93-195">Bármely más Brightspace Desire2Learn felhasználói fiók létrehozása eszközök is használhatja, vagy API-k által biztosított Brightspace által Desire2Learn kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e2f93-195">You can use any other Brightspace by Desire2Learn user account creation tools or APIs provided by Brightspace by Desire2Learn to provision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e2f93-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e2f93-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e2f93-197">Ebben a szakaszban Britta Simon Brightspace Desire2Learn által biztosított hozzáférés szerint használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e2f93-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Brightspace by Desire2Learn.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e2f93-199">**Britta Simon hozzárendelése által Desire2Learn Brightspace, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e2f93-199">**To assign Britta Simon to Brightspace by Desire2Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="e2f93-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e2f93-202">Az alkalmazások listában válassza ki a **által Desire2Learn Brightspace**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-202">In the applications list, select **Brightspace by Desire2Learn**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. <span data-ttu-id="e2f93-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e2f93-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e2f93-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e2f93-206">Click **Add** button.</span></span> <span data-ttu-id="e2f93-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2f93-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e2f93-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e2f93-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e2f93-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2f93-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e2f93-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e2f93-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e2f93-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e2f93-212">Testing single sign-on</span></span>

<span data-ttu-id="e2f93-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e2f93-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e2f93-214">A Brightspace kattintva által Desire2Learn csempe a hozzáférési panelen, meg kell beolvasása automatikusan aláírt a a Brightspace Desire2Learn alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e2f93-214">When you click the Brightspace by Desire2Learn tile in the Access Panel, you should get automatically signed-on to your Brightspace by Desire2Learn application.</span></span>
<span data-ttu-id="e2f93-215">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e2f93-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2f93-216">További források</span><span class="sxs-lookup"><span data-stu-id="e2f93-216">Additional resources</span></span>

* [<span data-ttu-id="e2f93-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e2f93-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2f93-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e2f93-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_203.png

