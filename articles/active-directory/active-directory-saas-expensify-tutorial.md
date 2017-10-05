---
title: "Oktatóanyag: Azure Active Directoryval integrált Expensify |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Expensify között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: jeedes
ms.openlocfilehash: e45576fd92706881121469ccd82150b3d48059cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a><span data-ttu-id="3f89c-103">Oktatóanyag: Azure Active Directoryval integrált Expensify</span><span class="sxs-lookup"><span data-stu-id="3f89c-103">Tutorial: Azure Active Directory integration with Expensify</span></span>

<span data-ttu-id="3f89c-104">Ebben az oktatóanyagban elsajátíthatja Expensify integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f89c-104">In this tutorial, you learn how to integrate Expensify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f89c-105">Expensify integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3f89c-105">Integrating Expensify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f89c-106">Megadhatja a Expensify hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3f89c-106">You can control in Azure AD who has access to Expensify</span></span>
- <span data-ttu-id="3f89c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Expensify (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3f89c-107">You can enable your users to automatically get signed-on to Expensify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f89c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3f89c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f89c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f89c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f89c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f89c-110">Prerequisites</span></span>

<span data-ttu-id="3f89c-111">Konfigurálása az Azure AD-integrációs Expensify, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3f89c-111">To configure Azure AD integration with Expensify, you need the following items:</span></span>

- <span data-ttu-id="3f89c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3f89c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f89c-113">Egy Expensify egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3f89c-113">An Expensify single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f89c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3f89c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f89c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3f89c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f89c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3f89c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f89c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f89c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f89c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3f89c-118">Scenario description</span></span>
<span data-ttu-id="3f89c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3f89c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f89c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3f89c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f89c-121">A gyűjteményből Expensify hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f89c-121">Adding Expensify from the gallery</span></span>
2. <span data-ttu-id="3f89c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f89c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-expensify-from-the-gallery"></a><span data-ttu-id="3f89c-123">A gyűjteményből Expensify hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f89c-123">Adding Expensify from the gallery</span></span>
<span data-ttu-id="3f89c-124">Az Azure AD integrálása a Expensify konfigurálásához kell hozzáadnia Expensify a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3f89c-124">To configure the integration of Expensify into Azure AD, you need to add Expensify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f89c-125">**A gyűjteményből Expensify hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f89c-125">**To add Expensify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f89c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f89c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f89c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3f89c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3f89c-133">Írja be a keresőmezőbe, **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-133">In the search box, type **Expensify**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_search.png)

5. <span data-ttu-id="3f89c-135">Az eredmények panelen válassza ki a **Expensify**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3f89c-135">In the results panel, select **Expensify**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f89c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f89c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f89c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Expensify.</span><span class="sxs-lookup"><span data-stu-id="3f89c-138">In this section, you configure and test Azure AD single sign-on with Expensify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f89c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Expensify a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3f89c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Expensify is to a user in Azure AD.</span></span> <span data-ttu-id="3f89c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Expensify közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3f89c-140">In other words, a link relationship between an Azure AD user and the related user in Expensify needs to be established.</span></span>

<span data-ttu-id="3f89c-141">Expensify, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3f89c-141">In Expensify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f89c-142">Az Azure AD egyszeri bejelentkezést a Expensify tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3f89c-142">To configure and test Azure AD single sign-on with Expensify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f89c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3f89c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f89c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f89c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f89c-145">**[Egy Expensify tesztfelhasználó létrehozása](#creating-an-expensify-test-user)**  - való Britta Simon valami Expensify, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3f89c-145">**[Creating an Expensify test user](#creating-an-expensify-test-user)** - to have a counterpart of Britta Simon in Expensify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f89c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f89c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f89c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3f89c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f89c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f89c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f89c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Expensify alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3f89c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Expensify application.</span></span>

<span data-ttu-id="3f89c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Expensify, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f89c-150">**To configure Azure AD single sign-on with Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="3f89c-151">Az Azure portálon a a **Expensify** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-151">In the Azure portal, on the **Expensify** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3f89c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f89c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_samlbase.png)

3. <span data-ttu-id="3f89c-155">Az a **Expensify tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f89c-155">On the **Expensify Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_url.png)

    <span data-ttu-id="3f89c-157">a.</span><span class="sxs-lookup"><span data-stu-id="3f89c-157">a.</span></span> <span data-ttu-id="3f89c-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.expensify.com/authentication/saml/login`</span><span class="sxs-lookup"><span data-stu-id="3f89c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.expensify.com/authentication/saml/login`</span></span>

    <span data-ttu-id="3f89c-159">b.</span><span class="sxs-lookup"><span data-stu-id="3f89c-159">b.</span></span> <span data-ttu-id="3f89c-160">Az a **azonosító URL-t** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.<companyname>.expensify.com/`</span><span class="sxs-lookup"><span data-stu-id="3f89c-160">In the **Identifier URL** textbox, type a URL using the following pattern: `https://www.<companyname>.expensify.com/`</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="3f89c-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3f89c-161">These values are not real.</span></span> <span data-ttu-id="3f89c-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosító URL-t.</span><span class="sxs-lookup"><span data-stu-id="3f89c-162">Update these values with the actual Sign-On URL and Identifier URL.</span></span> <span data-ttu-id="3f89c-163">Ügyfél [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3f89c-163">Contact [Expensify Client support team](mailto:help@expensify.com) to get these values.</span></span> 
 
4. <span data-ttu-id="3f89c-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f89c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_certificate.png) 

5. <span data-ttu-id="3f89c-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f89c-168">Ahhoz, hogy az SSO Expensify, először engedélyezze **tartomány vezérlő** az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3f89c-168">To enable SSO in Expensify, you first need to enable **Domain Control** in the application.</span></span> <span data-ttu-id="3f89c-169">Tartomány vezérlő engedélyezheti az alkalmazás a felsorolt lépéseket [Itt](http://help.expensify.com/domain-control).</span><span class="sxs-lookup"><span data-stu-id="3f89c-169">You can enable Domain Control in the application through the steps listed [here](http://help.expensify.com/domain-control).</span></span> <span data-ttu-id="3f89c-170">További együttműködve [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com).</span><span class="sxs-lookup"><span data-stu-id="3f89c-170">For additional support, work with [Expensify Client support team](mailto:help@expensify.com).</span></span> <span data-ttu-id="3f89c-171">Miután tartományi vezérlő engedélyezve van, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3f89c-171">Once you have Domain Control enabled, follow these steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png)
    
    <span data-ttu-id="3f89c-173">a.</span><span class="sxs-lookup"><span data-stu-id="3f89c-173">a.</span></span> <span data-ttu-id="3f89c-174">Jelentkezzen be a Expensify alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3f89c-174">Sign on to your Expensify application.</span></span>
    
    <span data-ttu-id="3f89c-175">b.</span><span class="sxs-lookup"><span data-stu-id="3f89c-175">b.</span></span> <span data-ttu-id="3f89c-176">A felső eszköztáron kattintson **Admin**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-176">In the toolbar on the top, click **Admin**.</span></span>
    
    <span data-ttu-id="3f89c-177">c.</span><span class="sxs-lookup"><span data-stu-id="3f89c-177">c.</span></span> <span data-ttu-id="3f89c-178">A bal oldali panelen kattintson a **tartomány**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-178">In the left panel, click **Domain**.</span></span>
    
    <span data-ttu-id="3f89c-179">d.</span><span class="sxs-lookup"><span data-stu-id="3f89c-179">d.</span></span> <span data-ttu-id="3f89c-180">Kattintson a ellenőrzött tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="3f89c-180">Click your verified domain name.</span></span>
    
    <span data-ttu-id="3f89c-181">e.</span><span class="sxs-lookup"><span data-stu-id="3f89c-181">e.</span></span> <span data-ttu-id="3f89c-182">A bal oldali panelen kattintson a **SAML**, majd válassza ki **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-182">In the left panel, click **SAML**, and then select **Enabled**.</span></span>
    
    <span data-ttu-id="3f89c-183">f.</span><span class="sxs-lookup"><span data-stu-id="3f89c-183">f.</span></span> <span data-ttu-id="3f89c-184">Nyissa meg a letöltött összevonási metaadatok a Jegyzettömbben az Azure AD-ből, másolja a tartalmat, és illessze be azt a **Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3f89c-184">Open the downloaded Federation Metadata from Azure AD in notepad, copy the content, and then paste it into the **Identity Provider Metadata** textbox.</span></span>

> [!TIP]
> <span data-ttu-id="3f89c-185">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3f89c-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f89c-186">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3f89c-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f89c-187">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f89c-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f89c-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f89c-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f89c-189">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3f89c-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3f89c-191">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f89c-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f89c-192">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f89c-194">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f89c-196">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3f89c-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f89c-198">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f89c-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f89c-200">a.</span><span class="sxs-lookup"><span data-stu-id="3f89c-200">a.</span></span> <span data-ttu-id="3f89c-201">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f89c-202">b.</span><span class="sxs-lookup"><span data-stu-id="3f89c-202">b.</span></span> <span data-ttu-id="3f89c-203">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f89c-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f89c-204">c.</span><span class="sxs-lookup"><span data-stu-id="3f89c-204">c.</span></span> <span data-ttu-id="3f89c-205">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f89c-206">d.</span><span class="sxs-lookup"><span data-stu-id="3f89c-206">d.</span></span> <span data-ttu-id="3f89c-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-207">Click **Create**.</span></span>
 
### <a name="creating-an-expensify-test-user"></a><span data-ttu-id="3f89c-208">Egy Expensify tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f89c-208">Creating an Expensify test user</span></span>

<span data-ttu-id="3f89c-209">Ebben a szakaszban egy Expensify Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3f89c-209">In this section, you create a user called Britta Simon in Expensify.</span></span> <span data-ttu-id="3f89c-210">Együttműködve [Expensify ügyfél-támogatási csoport](mailto:help@expensify.com) a felhasználók hozzáadása a Expensify platform.</span><span class="sxs-lookup"><span data-stu-id="3f89c-210">Work with [Expensify Client support team](mailto:help@expensify.com) to add the users in the Expensify platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f89c-211">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3f89c-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f89c-212">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Expensify Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3f89c-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Expensify.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3f89c-214">**Britta Simon hozzárendelése Expensify, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f89c-214">**To assign Britta Simon to Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="3f89c-215">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3f89c-217">Az alkalmazások listában válassza ki a **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-217">In the applications list, select **Expensify**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_app.png) 

3. <span data-ttu-id="3f89c-219">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3f89c-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3f89c-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f89c-221">Click **Add** button.</span></span> <span data-ttu-id="3f89c-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f89c-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3f89c-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3f89c-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f89c-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f89c-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f89c-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f89c-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f89c-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3f89c-227">Testing single sign-on</span></span>

<span data-ttu-id="3f89c-228">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3f89c-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="3f89c-229">Ha a hozzáférési panelen Expensify csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Expensify alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="3f89c-229">When you click the Expensify tile in the Access Panel, you should get automatically signed-on to your Expensify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f89c-230">További források</span><span class="sxs-lookup"><span data-stu-id="3f89c-230">Additional resources</span></span>

* [<span data-ttu-id="3f89c-231">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3f89c-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f89c-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3f89c-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png

