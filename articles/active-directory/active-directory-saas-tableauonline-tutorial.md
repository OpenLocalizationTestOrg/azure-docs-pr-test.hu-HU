---
title: "Oktatóanyag: Azure Active Directory-integráció a Tableau Online |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Tableau Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="5729c-103">Oktatóanyag: Azure Active Directory-integráció a Tableau Online</span><span class="sxs-lookup"><span data-stu-id="5729c-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="5729c-104">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálható a Tableau Online az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5729c-104">In this tutorial, you learn how to integrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5729c-105">Tableau Online integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5729c-105">Integrating Tableau Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5729c-106">Szabályozhatja, aki hozzáfér a Tableau Online Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5729c-106">You can control in Azure AD who has access to Tableau Online</span></span>
- <span data-ttu-id="5729c-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Tableau online (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="5729c-107">You can enable your users to automatically get signed-on to Tableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5729c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5729c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5729c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5729c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5729c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5729c-110">Prerequisites</span></span>

<span data-ttu-id="5729c-111">Az Azure AD-integráció konfigurálása a Tableau Online, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5729c-111">To configure Azure AD integration with Tableau Online, you need the following items:</span></span>

- <span data-ttu-id="5729c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5729c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5729c-113">A Tableau Online egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5729c-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5729c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5729c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5729c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5729c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5729c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5729c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5729c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5729c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5729c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5729c-118">Scenario description</span></span>
<span data-ttu-id="5729c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5729c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5729c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5729c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5729c-121">A gyűjteményből Tableau Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5729c-121">Adding Tableau Online from the gallery</span></span>
2. <span data-ttu-id="5729c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5729c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-the-gallery"></a><span data-ttu-id="5729c-123">A gyűjteményből Tableau Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5729c-123">Adding Tableau Online from the gallery</span></span>
<span data-ttu-id="5729c-124">Az Azure AD integrálása a Tableau Online konfigurálásához kell hozzáadnia Tableau Online a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5729c-124">To configure the integration of Tableau Online into Azure AD, you need to add Tableau Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5729c-125">**Adja hozzá a Tableau Online a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5729c-125">**To add Tableau Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5729c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5729c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5729c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5729c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5729c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5729c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5729c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="5729c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5729c-133">Írja be a keresőmezőbe, **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="5729c-133">In the search box, type **Tableau Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="5729c-135">Az eredmények panelen válassza ki a **Tableau Online**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5729c-135">In the results panel, select **Tableau Online**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5729c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5729c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5729c-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Tableau Online "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="5729c-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5729c-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó a Tableau Online Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5729c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Online is to a user in Azure AD.</span></span> <span data-ttu-id="5729c-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Tableau Online közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5729c-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Online needs to be established.</span></span>

<span data-ttu-id="5729c-141">A Tableau Online, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5729c-141">In Tableau Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5729c-142">Az Azure AD egyszeri bejelentkezést a Tableau Online tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5729c-142">To configure and test Azure AD single sign-on with Tableau Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5729c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5729c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5729c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5729c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5729c-145">**[A Tableau Online tesztfelhasználó létrehozása](#creating-a-tableau-online-test-user)**  - való Britta Simon egy megfelelője a Tableau Online, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5729c-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - to have a counterpart of Britta Simon in Tableau Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5729c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5729c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5729c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5729c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5729c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5729c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5729c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Tableau Online alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5729c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="5729c-150">**Az Azure AD egyszeri bejelentkezést a Tableau Online megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5729c-150">**To configure Azure AD single sign-on with Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="5729c-151">Az Azure portálon a a **Tableau Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5729c-151">In the Azure portal, on the **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5729c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5729c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="5729c-155">Az a **Tableau Online tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5729c-155">On the **Tableau Online Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="5729c-157">a.</span><span class="sxs-lookup"><span data-stu-id="5729c-157">a.</span></span> <span data-ttu-id="5729c-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="5729c-158">In the **Sign-on URL** textbox, type the URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="5729c-159">b.</span><span class="sxs-lookup"><span data-stu-id="5729c-159">b.</span></span> <span data-ttu-id="5729c-160">Az a **azonosító** szövegmező, írja be az URL-cím:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="5729c-160">In the **Identifier** textbox, type the URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="5729c-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5729c-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="5729c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5729c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5729c-165">Egy másik böngészőablakban bejelentkezés a Tableau Online alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5729c-165">In a different browser window, sign-on to your Tableau Online application.</span></span> <span data-ttu-id="5729c-166">Ugrás a **beállítások** , majd **hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="5729c-166">Go to **Settings** and then **Authentication**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="5729c-168">Ahhoz, hogy SAML, a **hitelesítési típusok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5729c-168">To enable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="5729c-169">Ellenőrizze a **egyszeri bejelentkezéshez a SAML** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5729c-169">Check the **Single sign-on with SAML** checkbox.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="5729c-171">Görgessen lefelé, amíg **importálási metaadatait tartalmazó fájl a Tableau Online** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5729c-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="5729c-172">Kattintson a Tallózás gombra, és importálja a metaadat-fájlt már letöltötte az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5729c-172">Click Browse and import the metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="5729c-173">Kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="5729c-173">Then, click **Apply**.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="5729c-175">Az a **helyességi feltételek egyeznek** szakaszban, helyezze be a megfelelő identitásszolgáltató helyességi feltétel nevét **e-mail cím**, **Utónév**, és **Vezetéknév**.</span><span class="sxs-lookup"><span data-stu-id="5729c-175">In the **Match assertions** section, insert the corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="5729c-176">Ezt az információt lekérni az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5729c-176">To get this information from Azure AD:</span></span> 
  
    <span data-ttu-id="5729c-177">a.</span><span class="sxs-lookup"><span data-stu-id="5729c-177">a.</span></span> <span data-ttu-id="5729c-178">Az Azure portálon, nyissa meg a **Tableau Online** alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="5729c-178">In the Azure portal, go on the **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="5729c-179">b.</span><span class="sxs-lookup"><span data-stu-id="5729c-179">b.</span></span> <span data-ttu-id="5729c-180">Attribútumok területen válassza ki a **"megtekintése és szerkesztése a más felhasználói attribútumok"** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5729c-180">In the attributes section, Select the **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="5729c-182">c.</span><span class="sxs-lookup"><span data-stu-id="5729c-182">c.</span></span> <span data-ttu-id="5729c-183">Másolja a névtér értéke attribútumait: givenname, e-mailek és a Vezetéknév, az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="5729c-183">Copy the namespace value for these attributes: givenname, email and surname by using the following steps:</span></span>

   ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="5729c-185">d.</span><span class="sxs-lookup"><span data-stu-id="5729c-185">d.</span></span> <span data-ttu-id="5729c-186">Kattintson a **user.givenname** érték</span><span class="sxs-lookup"><span data-stu-id="5729c-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="5729c-187">e.</span><span class="sxs-lookup"><span data-stu-id="5729c-187">e.</span></span> <span data-ttu-id="5729c-188">Másolja az értéket a **névtér** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5729c-188">Copy the value from the **namespace** textbox.</span></span>

   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="5729c-190">f.</span><span class="sxs-lookup"><span data-stu-id="5729c-190">f.</span></span> <span data-ttu-id="5729c-191">Másolja a namesapce értékek az e-mailek és a Vezetéknév kövesse a fenti lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5729c-191">To copy the namesapce values for the email and surname follow the preceding steps.</span></span>

    <span data-ttu-id="5729c-192">g.</span><span class="sxs-lookup"><span data-stu-id="5729c-192">g.</span></span> <span data-ttu-id="5729c-193">Váltson a Tableau Online alkalmazást, majd állítsa be a **Tableau Online attribútumok** szakasz az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5729c-193">Switch to the Tableau Online application, then set the **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="5729c-194">E-mailek: **mail** vagy **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="5729c-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="5729c-195">Utónév: **givenname**</span><span class="sxs-lookup"><span data-stu-id="5729c-195">First name: **givenname**</span></span>
     * <span data-ttu-id="5729c-196">Vezetéknév: **Vezetéknév**</span><span class="sxs-lookup"><span data-stu-id="5729c-196">Last name: **surname**</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="5729c-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="5729c-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5729c-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="5729c-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5729c-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5729c-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5729c-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5729c-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="5729c-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="5729c-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5729c-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5729c-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5729c-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5729c-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5729c-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5729c-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5729c-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="5729c-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5729c-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5729c-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5729c-213">a.</span><span class="sxs-lookup"><span data-stu-id="5729c-213">a.</span></span> <span data-ttu-id="5729c-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5729c-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5729c-215">b.</span><span class="sxs-lookup"><span data-stu-id="5729c-215">b.</span></span> <span data-ttu-id="5729c-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5729c-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5729c-217">c.</span><span class="sxs-lookup"><span data-stu-id="5729c-217">c.</span></span> <span data-ttu-id="5729c-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5729c-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5729c-219">d.</span><span class="sxs-lookup"><span data-stu-id="5729c-219">d.</span></span> <span data-ttu-id="5729c-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5729c-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="5729c-221">A Tableau Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5729c-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="5729c-222">Ebben a szakaszban a Tableau Online Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5729c-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="5729c-223">A **Tableau Online**, kattintson a **beállítások** , majd **hitelesítési** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5729c-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="5729c-224">Görgessen le a **felhasználók** szakasz.</span><span class="sxs-lookup"><span data-stu-id="5729c-224">Scroll down to **Select Users** section.</span></span> <span data-ttu-id="5729c-225">Kattintson a **felhasználók hozzáadása az** , majd **adja meg az E-mail címek**.</span><span class="sxs-lookup"><span data-stu-id="5729c-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="5729c-227">Válassza ki **adja hozzá a felhasználók az egyszeri bejelentkezés (SSO) hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="5729c-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="5729c-228">Az a **E-mail címet adjon meg** szövegmező hozzáadásabritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="5729c-228">In the **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="5729c-230">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5729c-230">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5729c-231">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5729c-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5729c-232">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó Tableau online-hoz való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="5729c-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5729c-234">**Britta Simon hozzárendelése Tableau Online, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5729c-234">**To assign Britta Simon to Tableau Online, perform the following steps:**</span></span>

1. <span data-ttu-id="5729c-235">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5729c-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5729c-237">Az alkalmazások listában válassza ki a **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="5729c-237">In the applications list, select **Tableau Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="5729c-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5729c-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5729c-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5729c-241">Click **Add** button.</span></span> <span data-ttu-id="5729c-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5729c-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5729c-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5729c-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5729c-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5729c-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5729c-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5729c-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5729c-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5729c-247">Testing single sign-on</span></span>

<span data-ttu-id="5729c-248">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5729c-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5729c-249">Ha a hozzáférési panelen a Tableau Online csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Tableau Online alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5729c-249">When you click the Tableau Online tile in the Access Panel, you should get automatically signed-on to your Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5729c-250">További források</span><span class="sxs-lookup"><span data-stu-id="5729c-250">Additional resources</span></span>

* [<span data-ttu-id="5729c-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5729c-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5729c-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5729c-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

