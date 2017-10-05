---
title: "Oktatóanyag: Azure Active Directoryval integrált Aha! | Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7723864b2e1ab2d5b69d86f0fa18416b9d3f9aa3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="28da6-104">Oktatóanyag: Azure Active Directoryval integrált Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="28da6-105">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálhatja Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-105">In this tutorial, you learn how to integrate Aha!</span></span> <span data-ttu-id="28da6-106">az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="28da6-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="28da6-107">Integrálása Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-107">Integrating Aha!</span></span> <span data-ttu-id="28da6-108">az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="28da6-108">with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="28da6-109">Az Azure AD, aki hozzáfér Aha szabályozhatja!</span><span class="sxs-lookup"><span data-stu-id="28da6-109">You can control in Azure AD who has access to Aha!</span></span>
- <span data-ttu-id="28da6-110">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-110">You can enable your users to automatically get signed-on to Aha!</span></span> <span data-ttu-id="28da6-111">(Egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="28da6-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="28da6-112">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="28da6-112">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="28da6-113">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="28da6-113">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28da6-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28da6-114">Prerequisites</span></span>

<span data-ttu-id="28da6-115">Az Azure AD-integrációs konfigurálása Aha!, a következőkre van szüksége:</span><span class="sxs-lookup"><span data-stu-id="28da6-115">To configure Azure AD integration with Aha!, you need the following items:</span></span>

- <span data-ttu-id="28da6-116">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="28da6-116">An Azure AD subscription</span></span>
- <span data-ttu-id="28da6-117">Egy Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-117">An Aha!</span></span> <span data-ttu-id="28da6-118">egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="28da6-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="28da6-119">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="28da6-119">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="28da6-120">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="28da6-120">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="28da6-121">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="28da6-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="28da6-122">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="28da6-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="28da6-123">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="28da6-123">Scenario description</span></span>
<span data-ttu-id="28da6-124">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="28da6-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="28da6-125">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="28da6-125">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="28da6-126">Hozzáadás Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-126">Adding Aha!</span></span> <span data-ttu-id="28da6-127">a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="28da6-127">from the gallery</span></span>
2. <span data-ttu-id="28da6-128">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="28da6-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-the-gallery"></a><span data-ttu-id="28da6-129">Hozzáadás Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-129">Adding Aha!</span></span> <span data-ttu-id="28da6-130">a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="28da6-130">from the gallery</span></span>
<span data-ttu-id="28da6-131">Aha integrációjának konfigurálása!</span><span class="sxs-lookup"><span data-stu-id="28da6-131">To configure the integration of Aha!</span></span> <span data-ttu-id="28da6-132">az Azure AD hozzá kell adnia a Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-132">into Azure AD, you need to add Aha!</span></span> <span data-ttu-id="28da6-133">a felügyelt SaaS-alkalmazások listájára gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="28da6-133">from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="28da6-134">**Hozzáadandó Aha! a gyűjteményből hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28da6-134">**To add Aha! from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="28da6-135">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="28da6-135">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="28da6-137">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="28da6-137">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="28da6-138">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28da6-138">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="28da6-140">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="28da6-140">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="28da6-142">Írja be a keresőmezőbe, **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="28da6-142">In the search box, type **Aha!**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="28da6-144">Az eredmények panelen válassza ki a **Aha!**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="28da6-144">In the results panel, select **Aha!**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="28da6-146">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="28da6-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="28da6-147">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="28da6-148">a "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="28da6-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="28da6-149">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, milyen a párjukhoz felhasználó Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-149">For single sign-on to work, Azure AD needs to know what the counterpart user in Aha!</span></span> <span data-ttu-id="28da6-150">az Azure AD-hoz a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="28da6-150">is to a user in Azure AD.</span></span> <span data-ttu-id="28da6-151">Más szóval egy hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználó a Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-151">In other words, a link relationship between an Azure AD user and the related user in Aha!</span></span> <span data-ttu-id="28da6-152">meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="28da6-152">needs to be established.</span></span>

<span data-ttu-id="28da6-153">A Aha!, az értéket a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="28da6-153">In Aha!, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="28da6-154">Az Azure AD egyszeri bejelentkezést a Aha tesztelése és konfigurálása!, végre kell hajtania a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="28da6-154">To configure and test Azure AD single sign-on with Aha!, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="28da6-155">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="28da6-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="28da6-156">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="28da6-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="28da6-157">**[Létrehozása egy Aha! Teszt felhasználó](#creating-an-aha-test-user)**  - való egy megfelelője a Britta Simon Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - to have a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="28da6-158">a felhasználó az Azure AD-ábrázolását kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="28da6-158">that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="28da6-159">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="28da6-159">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="28da6-160">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="28da6-160">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="28da6-161">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28da6-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="28da6-162">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Aha az egyszeri bejelentkezés konfigurálása!</span><span class="sxs-lookup"><span data-stu-id="28da6-162">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="28da6-163">az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="28da6-163">application.</span></span>

<span data-ttu-id="28da6-164">**Az Azure AD az egyszeri bejelentkezés konfigurálása Aha!, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28da6-164">**To configure Azure AD single sign-on with Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="28da6-165">Az Azure portálon a a **Aha!**</span><span class="sxs-lookup"><span data-stu-id="28da6-165">In the Azure portal, on the **Aha!**</span></span> <span data-ttu-id="28da6-166">alkalmazás-integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28da6-166">application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="28da6-168">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="28da6-168">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="28da6-170">Az a **Aha! Tartomány- és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="28da6-170">On the **Aha! Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="28da6-172">a.</span><span class="sxs-lookup"><span data-stu-id="28da6-172">a.</span></span> <span data-ttu-id="28da6-173">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="28da6-173">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="28da6-174">b.</span><span class="sxs-lookup"><span data-stu-id="28da6-174">b.</span></span> <span data-ttu-id="28da6-175">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="28da6-175">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="28da6-176">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="28da6-176">These values are not real.</span></span> <span data-ttu-id="28da6-177">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="28da6-177">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="28da6-178">Ügyfél [Aha! Ügyfél-támogatási csoport](https://www.aha.io/company/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="28da6-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="28da6-179">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="28da6-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="28da6-181">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="28da6-181">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="28da6-183">Egy másik webes böngészőablakban jelentkezzen be a Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-183">In a different web browser window, log in to your Aha!</span></span> <span data-ttu-id="28da6-184">vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="28da6-184">company site as an administrator.</span></span>

7. <span data-ttu-id="28da6-185">Kattintson a felső menüben **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="28da6-185">In the menu on the top, click **Settings**.</span></span>

    <span data-ttu-id="28da6-186">![Beállítások](./media/active-directory-saas-aha-tutorial/IC798950.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="28da6-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="28da6-187">Kattintson a **fiók**.</span><span class="sxs-lookup"><span data-stu-id="28da6-187">Click **Account**.</span></span>
   
    <span data-ttu-id="28da6-188">![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "profil")</span><span class="sxs-lookup"><span data-stu-id="28da6-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="28da6-189">Kattintson a **biztonsági és egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28da6-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="28da6-190">![Biztonság és az egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798952.png "biztonsági és egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="28da6-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="28da6-191">A **egyszeri bejelentkezés** szakaszban, mint **identitásszolgáltató**, jelölje be **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="28da6-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="28da6-192">![Biztonság és az egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798953.png "biztonsági és egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="28da6-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="28da6-193">Az a **egyszeri bejelentkezés** konfigurációs lapján tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="28da6-193">On the **Single Sign-On** configuration page, perform the following steps:</span></span>
    
    <span data-ttu-id="28da6-194">![Egyszeri bejelentkezés](./media/active-directory-saas-aha-tutorial/IC798954.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="28da6-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="28da6-195">a.</span><span class="sxs-lookup"><span data-stu-id="28da6-195">a.</span></span> <span data-ttu-id="28da6-196">Az a **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="28da6-196">In the **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="28da6-197">b.</span><span class="sxs-lookup"><span data-stu-id="28da6-197">b.</span></span> <span data-ttu-id="28da6-198">A **beállításához használatával**, jelölje be **metaadatfájl**.</span><span class="sxs-lookup"><span data-stu-id="28da6-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="28da6-199">c.</span><span class="sxs-lookup"><span data-stu-id="28da6-199">c.</span></span> <span data-ttu-id="28da6-200">A letöltött metaadat-fájl feltöltése, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="28da6-200">To upload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="28da6-201">d.</span><span class="sxs-lookup"><span data-stu-id="28da6-201">d.</span></span> <span data-ttu-id="28da6-202">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="28da6-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="28da6-203">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="28da6-203">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="28da6-204">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="28da6-204">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="28da6-205">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="28da6-205">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="28da6-206">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="28da6-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="28da6-207">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="28da6-207">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="28da6-209">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28da6-209">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="28da6-210">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="28da6-210">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="28da6-212">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="28da6-212">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="28da6-214">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="28da6-214">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="28da6-216">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="28da6-216">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="28da6-218">a.</span><span class="sxs-lookup"><span data-stu-id="28da6-218">a.</span></span> <span data-ttu-id="28da6-219">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="28da6-219">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="28da6-220">b.</span><span class="sxs-lookup"><span data-stu-id="28da6-220">b.</span></span> <span data-ttu-id="28da6-221">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="28da6-221">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="28da6-222">c.</span><span class="sxs-lookup"><span data-stu-id="28da6-222">c.</span></span> <span data-ttu-id="28da6-223">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="28da6-223">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="28da6-224">d.</span><span class="sxs-lookup"><span data-stu-id="28da6-224">d.</span></span> <span data-ttu-id="28da6-225">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28da6-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="28da6-226">Létrehozása egy Aha!</span><span class="sxs-lookup"><span data-stu-id="28da6-226">Creating an Aha!</span></span> <span data-ttu-id="28da6-227">Teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="28da6-227">test user</span></span>

<span data-ttu-id="28da6-228">Ahhoz, hogy az Azure AD-felhasználók Aha bejelentkezni!, azok ki kell építenie a Aha!.</span><span class="sxs-lookup"><span data-stu-id="28da6-228">To enable Azure AD users to log in to Aha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="28da6-229">Aha esetén!, az automatizált feladat.</span><span class="sxs-lookup"><span data-stu-id="28da6-229">In the case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="28da6-230">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="28da6-230">There is no action item for you.</span></span>

<span data-ttu-id="28da6-231">Felhasználók automatikusan létrejönnek szükség esetén az első egy bejelentkezési kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="28da6-231">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="28da6-232">Bármely más Aha is használhatja!</span><span class="sxs-lookup"><span data-stu-id="28da6-232">You can use any other Aha!</span></span> <span data-ttu-id="28da6-233">felhasználói fiók létrehozása eszközök vagy Aha által nyújtott API-kat!</span><span class="sxs-lookup"><span data-stu-id="28da6-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="28da6-234">AAD-felhasználói fiókok kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="28da6-234">to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="28da6-235">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="28da6-235">Assigning the Azure AD test user</span></span>

<span data-ttu-id="28da6-236">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Aha Azure egyszeri bejelentkezéshez használandó!.</span><span class="sxs-lookup"><span data-stu-id="28da6-236">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Aha!.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="28da6-238">**Britta Simon hozzárendelése Aha!, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="28da6-238">**To assign Britta Simon to Aha!, perform the following steps:**</span></span>

1. <span data-ttu-id="28da6-239">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="28da6-239">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="28da6-241">Az alkalmazások listában válassza ki a **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="28da6-241">In the applications list, select **Aha!**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="28da6-243">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="28da6-243">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="28da6-245">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="28da6-245">Click **Add** button.</span></span> <span data-ttu-id="28da6-246">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28da6-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="28da6-248">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="28da6-248">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="28da6-249">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28da6-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="28da6-250">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28da6-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="28da6-251">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="28da6-251">Testing single sign-on</span></span>

<span data-ttu-id="28da6-252">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="28da6-252">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="28da6-253">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="28da6-253">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28da6-254">További források</span><span class="sxs-lookup"><span data-stu-id="28da6-254">Additional resources</span></span>

* [<span data-ttu-id="28da6-255">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="28da6-255">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="28da6-256">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="28da6-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

