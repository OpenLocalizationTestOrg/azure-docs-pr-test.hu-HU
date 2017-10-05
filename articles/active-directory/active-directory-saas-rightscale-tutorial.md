---
title: "Oktatóanyag: Azure Active Directoryval integrált Rightscale |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Rightscale között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 222c4414a9f736a3589b4cdd0ed934696f6c31ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a><span data-ttu-id="47cd8-103">Oktatóanyag: Azure Active Directoryval integrált Rightscale</span><span class="sxs-lookup"><span data-stu-id="47cd8-103">Tutorial: Azure Active Directory integration with Rightscale</span></span>

<span data-ttu-id="47cd8-104">Ebben az oktatóanyagban elsajátíthatja Rightscale integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="47cd8-104">In this tutorial, you learn how to integrate Rightscale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="47cd8-105">Rightscale integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="47cd8-105">Integrating Rightscale with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="47cd8-106">Megadhatja a Rightscale hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="47cd8-106">You can control in Azure AD who has access to Rightscale</span></span>
- <span data-ttu-id="47cd8-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Rightscale (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="47cd8-107">You can enable your users to automatically get signed-on to Rightscale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="47cd8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="47cd8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="47cd8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="47cd8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47cd8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="47cd8-110">Prerequisites</span></span>

<span data-ttu-id="47cd8-111">Konfigurálása az Azure AD-integrációs Rightscale, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="47cd8-111">To configure Azure AD integration with Rightscale, you need the following items:</span></span>

- <span data-ttu-id="47cd8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="47cd8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="47cd8-113">Egy Rightscale egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="47cd8-113">A Rightscale single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="47cd8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="47cd8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="47cd8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="47cd8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="47cd8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="47cd8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="47cd8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="47cd8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="47cd8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="47cd8-118">Scenario description</span></span>
<span data-ttu-id="47cd8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="47cd8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="47cd8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="47cd8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="47cd8-121">A gyűjteményből Rightscale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="47cd8-121">Adding Rightscale from the gallery</span></span>
2. <span data-ttu-id="47cd8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="47cd8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightscale-from-the-gallery"></a><span data-ttu-id="47cd8-123">A gyűjteményből Rightscale hozzáadása</span><span class="sxs-lookup"><span data-stu-id="47cd8-123">Adding Rightscale from the gallery</span></span>
<span data-ttu-id="47cd8-124">Az Azure AD integrálása a Rightscale konfigurálásához kell hozzáadnia Rightscale a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="47cd8-124">To configure the integration of Rightscale into Azure AD, you need to add Rightscale from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="47cd8-125">**A gyűjteményből Rightscale hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47cd8-125">**To add Rightscale from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="47cd8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="47cd8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="47cd8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="47cd8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="47cd8-133">Írja be a keresőmezőbe, **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-133">In the search box, type **Rightscale**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. <span data-ttu-id="47cd8-135">Az eredmények panelen válassza ki a **Rightscale**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="47cd8-135">In the results panel, select **Rightscale**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="47cd8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="47cd8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="47cd8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Rightscale.</span><span class="sxs-lookup"><span data-stu-id="47cd8-138">In this section, you configure and test Azure AD single sign-on with Rightscale based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="47cd8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Rightscale a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="47cd8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Rightscale is to a user in Azure AD.</span></span> <span data-ttu-id="47cd8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Rightscale közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="47cd8-140">In other words, a link relationship between an Azure AD user and the related user in Rightscale needs to be established.</span></span>

<span data-ttu-id="47cd8-141">Rightscale, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="47cd8-141">In Rightscale, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="47cd8-142">Az Azure AD egyszeri bejelentkezést a Rightscale tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="47cd8-142">To configure and test Azure AD single sign-on with Rightscale, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="47cd8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="47cd8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="47cd8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="47cd8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="47cd8-145">**[Rightscale tesztfelhasználó létrehozása](#creating-a-rightscale-test-user)**  - való Britta Simon valami Rightscale, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="47cd8-145">**[Creating a Rightscale test user](#creating-a-rightscale-test-user)** - to have a counterpart of Britta Simon in Rightscale that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="47cd8-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="47cd8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="47cd8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="47cd8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="47cd8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="47cd8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="47cd8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Rightscale alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="47cd8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Rightscale application.</span></span>

<span data-ttu-id="47cd8-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Rightscale, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47cd8-150">**To configure Azure AD single sign-on with Rightscale, perform the following steps:**</span></span>

1. <span data-ttu-id="47cd8-151">Az Azure portálon a a **Rightscale** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-151">In the Azure portal, on the **Rightscale** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="47cd8-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="47cd8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. <span data-ttu-id="47cd8-155">Az a **Rightscale tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód** nincs bármely lépések végrehajtásához, mert az alkalmazás már előre integrált Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="47cd8-155">On the **Rightscale Domain and URLs** section, if you wish to configure the application in **IDP initiated mode** you do not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. <span data-ttu-id="47cd8-157">Az a **Rightscale tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="47cd8-157">On the **Rightscale Domain and URLs** section, if you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    <span data-ttu-id="47cd8-159">a.</span><span class="sxs-lookup"><span data-stu-id="47cd8-159">a.</span></span> <span data-ttu-id="47cd8-160">Kattintson a **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-160">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="47cd8-161">b.</span><span class="sxs-lookup"><span data-stu-id="47cd8-161">b.</span></span> <span data-ttu-id="47cd8-162">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-cím:`https://login.rightscale.com/`</span><span class="sxs-lookup"><span data-stu-id="47cd8-162">In the **Sign On URL** textbox, type the URL: `https://login.rightscale.com/`</span></span>

5. <span data-ttu-id="47cd8-163">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="47cd8-163">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. <span data-ttu-id="47cd8-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="47cd8-167">A a **Rightscale konfigurációs** kattintson **konfigurálása Rightscale** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="47cd8-167">On the **Rightscale Configuration** section, click **Configure Rightscale** to open **Configure sign-on** window.</span></span> <span data-ttu-id="47cd8-168">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="47cd8-168">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="47cd8-169">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="47cd8-169">![Configure Single Sign-On](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span></span>
8. <span data-ttu-id="47cd8-170">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, meg kell bejelentkezés a RightScale bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="47cd8-170">To get SSO configured for your application, you need to sign-on to your RightScale tenant as an administrator.</span></span>

    <span data-ttu-id="47cd8-171">a.</span><span class="sxs-lookup"><span data-stu-id="47cd8-171">a.</span></span> <span data-ttu-id="47cd8-172">A felső menüben kattintson a **beállítások** lapot, és válasszon **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-172">In the menu on the top, click the **Settings** tab and select **Single Sign-On**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    <span data-ttu-id="47cd8-174">b.</span><span class="sxs-lookup"><span data-stu-id="47cd8-174">b.</span></span> <span data-ttu-id="47cd8-175">Kattintson a "**új**" gombra kattintva adja hozzá **a SAML-alapú identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-175">Click the "**new**" button to add **Your SAML Identity Providers**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    <span data-ttu-id="47cd8-177">c.</span><span class="sxs-lookup"><span data-stu-id="47cd8-177">c.</span></span> <span data-ttu-id="47cd8-178">A szövegmezőben **megjelenített név**, adjon meg a cég nevét.</span><span class="sxs-lookup"><span data-stu-id="47cd8-178">In the textbox of **Display Name**, input your company name.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    <span data-ttu-id="47cd8-180">d.</span><span class="sxs-lookup"><span data-stu-id="47cd8-180">d.</span></span> <span data-ttu-id="47cd8-181">Válassza ki **engedélyezése RightScale által kezdeményezett egyszeri Bejelentkezést használó felderítés mutató** bemeneti és a **tartománynév** a a szövegmező alatt.</span><span class="sxs-lookup"><span data-stu-id="47cd8-181">Select **Allow RightScale-initiated SSO using a discovery hint** and input your **domain name** in the below textbox.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    <span data-ttu-id="47cd8-183">e.</span><span class="sxs-lookup"><span data-stu-id="47cd8-183">e.</span></span> <span data-ttu-id="47cd8-184">Illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** , amely az Azure portálról másolta **SAML SSO végpont** a RightScale.</span><span class="sxs-lookup"><span data-stu-id="47cd8-184">Paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal into **SAML SSO Endpoint** in RightScale.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    <span data-ttu-id="47cd8-186">f.</span><span class="sxs-lookup"><span data-stu-id="47cd8-186">f.</span></span> <span data-ttu-id="47cd8-187">Illessze be az értékét **SAML Entitásazonosító** , amely az Azure portálról másolta **SAML entityid beállítást** a RightScale.</span><span class="sxs-lookup"><span data-stu-id="47cd8-187">Paste the value of **SAML Entity ID** which you have copied from Azure portal into **SAML EntityID** in RightScale.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    <span data-ttu-id="47cd8-189">g.</span><span class="sxs-lookup"><span data-stu-id="47cd8-189">g.</span></span> <span data-ttu-id="47cd8-190">Kattintson a **böngésző** töltse fel a tanúsítványt, amely az Azure-portálról letöltött gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-190">Click **Browser** button to upload the certificate which you downloaded from Azure portal.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    <span data-ttu-id="47cd8-192">h.</span><span class="sxs-lookup"><span data-stu-id="47cd8-192">h.</span></span> <span data-ttu-id="47cd8-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-193">Click **Save**.</span></span>
<CE>
> [!TIP]
> <span data-ttu-id="47cd8-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="47cd8-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="47cd8-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="47cd8-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="47cd8-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="47cd8-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="47cd8-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="47cd8-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="47cd8-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="47cd8-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="47cd8-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47cd8-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="47cd8-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="47cd8-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="47cd8-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="47cd8-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="47cd8-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="47cd8-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="47cd8-209">a.</span><span class="sxs-lookup"><span data-stu-id="47cd8-209">a.</span></span> <span data-ttu-id="47cd8-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="47cd8-211">b.</span><span class="sxs-lookup"><span data-stu-id="47cd8-211">b.</span></span> <span data-ttu-id="47cd8-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="47cd8-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="47cd8-213">c.</span><span class="sxs-lookup"><span data-stu-id="47cd8-213">c.</span></span> <span data-ttu-id="47cd8-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="47cd8-215">d.</span><span class="sxs-lookup"><span data-stu-id="47cd8-215">d.</span></span> <span data-ttu-id="47cd8-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-216">Click **Create**.</span></span>
 
### <a name="creating-a-rightscale-test-user"></a><span data-ttu-id="47cd8-217">Rightscale tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="47cd8-217">Creating a Rightscale test user</span></span>

<span data-ttu-id="47cd8-218">Ebben a szakaszban egy RightScale Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="47cd8-218">In this section, you create a user called Britta Simon in RightScale.</span></span> <span data-ttu-id="47cd8-219">Együttműködve [Rightscale ügyfél-támogatási csoport](mailto:support@rightscale.com) a felhasználók hozzáadása a RightScale platform.</span><span class="sxs-lookup"><span data-stu-id="47cd8-219">Work with [Rightscale Client support team](mailto:support@rightscale.com) to add the users in the RightScale platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="47cd8-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="47cd8-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="47cd8-221">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Rightscale Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="47cd8-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Rightscale.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="47cd8-223">**Britta Simon hozzárendelése Rightscale, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="47cd8-223">**To assign Britta Simon to Rightscale, perform the following steps:**</span></span>

1. <span data-ttu-id="47cd8-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="47cd8-226">Az alkalmazások listában válassza ki a **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-226">In the applications list, select **Rightscale**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. <span data-ttu-id="47cd8-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="47cd8-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="47cd8-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="47cd8-230">Click **Add** button.</span></span> <span data-ttu-id="47cd8-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47cd8-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="47cd8-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="47cd8-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="47cd8-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47cd8-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="47cd8-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="47cd8-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="47cd8-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="47cd8-236">Testing single sign-on</span></span>

<span data-ttu-id="47cd8-237">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="47cd8-237">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="47cd8-238">Ha a hozzáférési panelen RightScale csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az RightScale alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="47cd8-238">When you click the RightScale tile in the Access Panel, you should get automatically signed-on to your RightScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="47cd8-239">További források</span><span class="sxs-lookup"><span data-stu-id="47cd8-239">Additional resources</span></span>

* [<span data-ttu-id="47cd8-240">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="47cd8-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="47cd8-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="47cd8-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

