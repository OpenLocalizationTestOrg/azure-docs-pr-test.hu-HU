---
title: "Oktatóanyag: Azure Active Directory integrálása vxMaintain |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és vxMaintain között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: ad87534af448356b8cc80d8ddd278bfb8a9165e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="bb2d4-103">Oktatóanyag: Azure Active Directory integrálása vxMaintain</span><span class="sxs-lookup"><span data-stu-id="bb2d4-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="bb2d4-104">Ebben az oktatóanyagban elsajátíthatja vxMaintain integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-104">In this tutorial, you learn how to integrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bb2d4-105">Ez az integráció kínál számos fontos előnnyel jár.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-105">This integration provides several important benefits.</span></span> <span data-ttu-id="bb2d4-106">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-106">You can:</span></span>

- <span data-ttu-id="bb2d4-107">Ellenőrzés vxMaintain hozzáféréssel rendelkező Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-107">Control in Azure AD who has access to vxMaintain.</span></span>
- <span data-ttu-id="bb2d4-108">Engedélyezze a felhasználóknak automatikusan jelentkezzen be az egyszeri bejelentkezés (SSO) vxMaintain az Azure AD-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-108">Enable your users to automatically sign in to vxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="bb2d4-109">A fiók egyetlen központi helyen kezelheti: az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="bb2d4-110">Az Azure AD SaaS alkalmazásintegráció kapcsolatos további információkért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-110">To learn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb2d4-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bb2d4-111">Prerequisites</span></span>

<span data-ttu-id="bb2d4-112">Konfigurálása az Azure AD-integrációs vxMaintain, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-112">To configure Azure AD integration with vxMaintain, you need the following items:</span></span>

- <span data-ttu-id="bb2d4-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bb2d4-113">An Azure AD subscription</span></span>
- <span data-ttu-id="bb2d4-114">Egy vxMaintain előfizetés SSO engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bb2d4-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bb2d4-115">Ebben az oktatóanyagban tesztelésekor a lépéseket, azt javasoljuk, hogy nem használ egy éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-115">When you test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="bb2d4-116">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="bb2d4-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bb2d4-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bb2d4-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-119">Scenario description</span></span>
<span data-ttu-id="bb2d4-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="bb2d4-121">Ez az oktatóanyag ismerteti a forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-121">The scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="bb2d4-122">A gyűjteményből vxMaintain hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-122">Adding vxMaintain from the gallery</span></span>
* <span data-ttu-id="bb2d4-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bb2d4-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-the-gallery"></a><span data-ttu-id="bb2d4-124">Adja hozzá a vxMaintain a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="bb2d4-124">Add vxMaintain from the gallery</span></span>
<span data-ttu-id="bb2d4-125">Az Azure AD integrálása vxMaintain konfigurálásához kell hozzáadnia vxMaintain a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-125">To configure the integration of vxMaintain with Azure AD, you need to add vxMaintain from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bb2d4-126">A gyűjteményből vxMaintain hozzáadásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-126">To add vxMaintain from the gallery, do the following:</span></span>

1. <span data-ttu-id="bb2d4-127">Az a [Azure-portálon](https://portal.azure.com), a bal oldali panelen válassza ki a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-127">In the [Azure portal](https://portal.azure.com), in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="bb2d4-129">Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![A "Vállalati alkalmazások" ablak][2]
    
3. <span data-ttu-id="bb2d4-131">Hozzáadhat egy alkalmazást, az a **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-131">To add an application, in the **All applications** dialog box, select **New application**.</span></span>

    ![Az "új alkalmazás" gomb][3]

4. <span data-ttu-id="bb2d4-133">Írja be a keresőmezőbe, **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-133">In the search box, type **vxMaintain**.</span></span>

    ![Az "Egyszeri bejelentkezés mód" legördülő lista](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="bb2d4-135">Az eredmények listájában válassza **vxMaintain**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-135">In the results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![A vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bb2d4-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="bb2d4-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO vxMaintain "Britta Simon." nevű tesztfelhasználó alapján használatával</span><span class="sxs-lookup"><span data-stu-id="bb2d4-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bb2d4-139">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a vxMaintain megfelelője az Azure AD-felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-139">For SSO to work, Azure AD needs to know the vxMaintain counterpart to the Azure AD user.</span></span> <span data-ttu-id="bb2d4-140">Ez azt jelenti, hogy az Azure AD-felhasználó és a megfelelő vxMaintain felhasználó közötti kapcsolat kapcsolatot kell létesítenie.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-140">That is, you must establish a link relationship between the Azure AD user and the corresponding vxMaintain user.</span></span>

<span data-ttu-id="bb2d4-141">A hivatkozás kapcsolatot létesíteni, rendelje hozzá a vxMaintain **felhasználónév** érték, mint az Azure AD **felhasználónév** érték.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-141">To establish the link relationship, assign the vxMaintain **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="bb2d4-142">Konfigurálása és tesztelése az Azure AD SSO vxMaintain használatával végezze el a következő építőelemeit.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-142">To configure and test Azure AD SSO by using vxMaintain, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="bb2d4-143">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="bb2d4-144">Ebben a szakaszban is engedélyezheti Azure AD egyszeri bejelentkezés az Azure portálon és egyszeri bejelentkezés konfigurálása az vxMaintain alkalmazásban a következő módon:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-144">In this section, you can both enable Azure AD SSO in the Azure portal and configure SSO in your vxMaintain application by doing the following:</span></span>

1. <span data-ttu-id="bb2d4-145">Az Azure portálon a a **vxMaintain** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-145">In the Azure portal, on the **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    ![Az "Egyszeri bejelentkezés" parancs][4]

2. <span data-ttu-id="bb2d4-147">Is engedélyezhető az egyszeri bejelentkezés, a **egyszeri bejelentkezés mód** legördülő listában válassza **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-147">To enable SSO, in the **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![A "SAML-alapú bejelentkezés" parancs](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="bb2d4-149">A **vxMaintain tartomány és az URL-címek**, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-149">Under **vxMaintain Domain and URLs**, do the following:</span></span>

    ![A tartomány és az URL-címek szakasz vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="bb2d4-151">a.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-151">a.</span></span> <span data-ttu-id="bb2d4-152">Az a **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő szintaxist:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="bb2d4-152">In the **Identifier** box, type a URL that has the following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="bb2d4-153">b.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-153">b.</span></span> <span data-ttu-id="bb2d4-154">Az a **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő szintaxist:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="bb2d4-154">In the **Reply URL** box, type a URL that has the following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bb2d4-155">Az előző értékei nem valódi.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-155">The preceding values are not real.</span></span> <span data-ttu-id="bb2d4-156">A tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-156">Update them with the actual identifier and reply URL.</span></span> <span data-ttu-id="bb2d4-157">Szerezze be az értékeket, lépjen kapcsolatba a [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-157">To obtain the values, contact the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="bb2d4-158">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, majd mentse a metaadat-fájlt a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file to your computer.</span></span>

    ![A "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="bb2d4-160">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-160">Select **Save**.</span></span>

    ![A Mentés gombra](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bb2d4-162">Konfigurálása **vxMaintain** SSO, küldjön a letöltött **metaadatainak XML-kódja** fájlt a [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-162">To configure **vxMaintain** SSO, send the downloaded **Metadata XML** file to the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="bb2d4-163">Állít be az alkalmazást, mert egy előző utasításait tömör verziója elolvashatja a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-163">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="bb2d4-164">Miután hozzáadta az alkalmazásból a **Active Directory** > **vállalati alkalmazások** szakaszban jelölje be a **egyszeri bejelentkezés** lapot, és hozzáférhet a beágyazott dokumentáció a **konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-164">After you add the app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab, and then access the embedded documentation from the **Configuration** section.</span></span> 
>
><span data-ttu-id="bb2d4-165">A beágyazott dokumentáció szolgáltatással kapcsolatos további tudnivalókért lásd: [egyszeri bejelentkezéshez a vállalati alkalmazásokat kezeléséhez](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-165">To learn more about the embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bb2d4-166">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="bb2d4-166">Create an Azure AD test user</span></span>
<span data-ttu-id="bb2d4-167">Ez a szakasz az alábbi lépésekkel hoz létre tesztfelhasználó Britta Simon az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-167">In this section, you create test user Britta Simon in the Azure portal by doing the following:</span></span>

![Az Azure AD-teszt felhasználó][100]

1. <span data-ttu-id="bb2d4-169">Az a **Azure-portálon**, a bal oldali panelen válassza ki a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-169">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Az "Azure Active Directory" gomb](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bb2d4-171">Megjeleníti azoknak a felhasználóknak, keresse fel **felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-171">To display a list of users, go to **Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="bb2d4-172">![A "Minden felhasználó" hivatkozásra](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="bb2d4-172">![The "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="bb2d4-173">A **minden felhasználó** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-173">The **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="bb2d4-174">Lehetőségre a **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-174">To open the **User** dialog box, select **Add**.</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bb2d4-176">Az a **felhasználói** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-176">In the **User** dialog box, do the following:</span></span>
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bb2d4-178">a.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-178">a.</span></span> <span data-ttu-id="bb2d4-179">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-179">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bb2d4-180">b.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-180">b.</span></span> <span data-ttu-id="bb2d4-181">Az a **felhasználónév** mezőbe írja be a tesztfelhasználó Britta Simon e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-181">In the **User name** box, type the email address of test user Britta Simon.</span></span>

    <span data-ttu-id="bb2d4-182">c.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-182">c.</span></span> <span data-ttu-id="bb2d4-183">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel az értéket, amelyet hozták létre a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-183">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="bb2d4-184">d.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-184">d.</span></span> <span data-ttu-id="bb2d4-185">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="bb2d4-186">VxMaintain tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="bb2d4-187">Ebben a szakaszban Britta Simon tesztfelhasználó vxMaintain hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="bb2d4-188">Felhasználók hozzáadása a vxMaintain platform, dolgozni a [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-188">To add users in the vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="bb2d4-189">Mielőtt használná az egyszeri Bejelentkezést, hozzon létre, és a felhasználók aktiválása.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-189">Before you use SSO, create and activate the users.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="bb2d4-190">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="bb2d4-190">Assign the Azure AD test user</span></span>

<span data-ttu-id="bb2d4-191">Ebben a szakaszban Azure SSO vxMaintain való hozzáférés biztosítása által használandó Britta Simon tesztfelhasználó engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-191">In this section, you enable test user Britta Simon to use Azure SSO by granting access to vxMaintain.</span></span> <span data-ttu-id="bb2d4-192">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bb2d4-192">To do so, do the following:</span></span>

![A megjelenítendő név listában a tesztfelhasználó számára][200] 

1. <span data-ttu-id="bb2d4-194">Az Azure portálon **alkalmazások** megtekintéséhez lépjen **Directory** Nézet > **vállalati alkalmazások** > **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-194">In the Azure portal **Applications** view, go to **Directory** view > **Enterprise applications** > **All applications**.</span></span>

    ![Az "Összes alkalmazás" hivatkozásra][201] 

2. <span data-ttu-id="bb2d4-196">Az a **alkalmazások** listáról válassza ki **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-196">In the **Applications** list, select **vxMaintain**.</span></span>

    ![A vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="bb2d4-198">A bal oldali panelen válassza ki a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-198">In the left pane, select **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. <span data-ttu-id="bb2d4-200">Válassza ki **Hozzáadás** , majd a a **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-200">Select **Add** and then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][203]

5. <span data-ttu-id="bb2d4-202">A a **felhasználók és csoportok** párbeszédpanel a **felhasználók** listáról válassza ki **Britta Simon**, majd válassza ki a **válassza** gomb.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-202">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**, and then select the **Select** button.</span></span>

7. <span data-ttu-id="bb2d4-203">Az a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-203">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="bb2d4-204">Az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bb2d4-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="bb2d4-205">Ebben a szakaszban az Azure AD SSO konfigurációját a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-205">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="bb2d4-206">Válassza a **vxMaintain** csempe a hozzáférési panelen kell jelentkezhet be a vxMaintain alkalmazás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="bb2d4-206">Selecting the **vxMaintain** tile in the Access Panel should sign you in to your vxMaintain application automatically.</span></span>

<span data-ttu-id="bb2d4-207">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bb2d4-207">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb2d4-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb2d4-208">Next steps</span></span>

* [<span data-ttu-id="bb2d4-209">SaaS-alkalmazások integrálása az Azure Active Directoryval kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="bb2d4-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bb2d4-210">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bb2d4-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

