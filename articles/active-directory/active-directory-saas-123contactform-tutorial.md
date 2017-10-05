---
title: "Oktatóanyag: Azure Active Directoryval integrált 123ContactForm |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és 123ContactForm között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3a99f0841c3e0d973168991f5dbee40e54c1d054
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a><span data-ttu-id="aaf85-103">Oktatóanyag: Azure Active Directoryval integrált 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="aaf85-103">Tutorial: Azure Active Directory integration with 123ContactForm</span></span>

<span data-ttu-id="aaf85-104">Ebben az oktatóanyagban elsajátíthatja 123ContactForm integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="aaf85-104">In this tutorial, you learn how to integrate 123ContactForm with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aaf85-105">123ContactForm integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="aaf85-105">Integrating 123ContactForm with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="aaf85-106">Megadhatja a 123ContactForm hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="aaf85-106">You can control in Azure AD who has access to 123ContactForm</span></span>
- <span data-ttu-id="aaf85-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a 123ContactForm (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="aaf85-107">You can enable your users to automatically get signed-on to 123ContactForm (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aaf85-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="aaf85-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="aaf85-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aaf85-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aaf85-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aaf85-110">Prerequisites</span></span>

<span data-ttu-id="aaf85-111">Konfigurálása az Azure AD-integrációs 123ContactForm, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="aaf85-111">To configure Azure AD integration with 123ContactForm, you need the following items:</span></span>

- <span data-ttu-id="aaf85-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="aaf85-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aaf85-113">Egy 123ContactForm egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="aaf85-113">A 123ContactForm single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aaf85-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="aaf85-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aaf85-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="aaf85-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aaf85-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="aaf85-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aaf85-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aaf85-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aaf85-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="aaf85-118">Scenario description</span></span>
<span data-ttu-id="aaf85-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="aaf85-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aaf85-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="aaf85-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aaf85-121">A gyűjteményből 123ContactForm hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aaf85-121">Adding 123ContactForm from the gallery</span></span>
2. <span data-ttu-id="aaf85-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aaf85-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-123contactform-from-the-gallery"></a><span data-ttu-id="aaf85-123">A gyűjteményből 123ContactForm hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aaf85-123">Adding 123ContactForm from the gallery</span></span>
<span data-ttu-id="aaf85-124">Az Azure AD integrálása a 123ContactForm konfigurálásához kell hozzáadnia 123ContactForm a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="aaf85-124">To configure the integration of 123ContactForm into Azure AD, you need to add 123ContactForm from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="aaf85-125">**A gyűjteményből 123ContactForm hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="aaf85-125">**To add 123ContactForm from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="aaf85-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aaf85-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="aaf85-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="aaf85-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="aaf85-133">Írja be a keresőmezőbe, **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-133">In the search box, type **123ContactForm**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. <span data-ttu-id="aaf85-135">Az eredmények panelen válassza ki a **123ContactForm**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aaf85-135">In the results panel, select **123ContactForm**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aaf85-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="aaf85-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aaf85-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="aaf85-138">In this section, you configure and test Azure AD single sign-on with 123ContactForm based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="aaf85-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó 123ContactForm a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="aaf85-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 123ContactForm is to a user in Azure AD.</span></span> <span data-ttu-id="aaf85-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a 123ContactForm közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="aaf85-140">In other words, a link relationship between an Azure AD user and the related user in 123ContactForm needs to be established.</span></span>

<span data-ttu-id="aaf85-141">123ContactForm, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="aaf85-141">In 123ContactForm, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="aaf85-142">Az Azure AD egyszeri bejelentkezést a 123ContactForm tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="aaf85-142">To configure and test Azure AD single sign-on with 123ContactForm, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="aaf85-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="aaf85-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="aaf85-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="aaf85-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aaf85-145">**[123ContactForm tesztfelhasználó létrehozása](#creating-a-123contactform-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó 123ContactForm rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="aaf85-145">**[Creating a 123ContactForm test user](#creating-a-123contactform-test-user)** - to have a counterpart of Britta Simon in 123ContactForm that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="aaf85-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="aaf85-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aaf85-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="aaf85-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aaf85-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aaf85-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aaf85-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az 123ContactForm alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="aaf85-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 123ContactForm application.</span></span>

<span data-ttu-id="aaf85-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés 123ContactForm, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="aaf85-150">**To configure Azure AD single sign-on with 123ContactForm, perform the following steps:**</span></span>

1. <span data-ttu-id="aaf85-151">Az Azure portálon a a **123ContactForm** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-151">In the Azure portal, on the **123ContactForm** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="aaf85-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="aaf85-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. <span data-ttu-id="aaf85-155">Az a **123ContactForm tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aaf85-155">On the **123ContactForm Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url1.png)

    <span data-ttu-id="aaf85-157">a.</span><span class="sxs-lookup"><span data-stu-id="aaf85-157">a.</span></span> <span data-ttu-id="aaf85-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span><span class="sxs-lookup"><span data-stu-id="aaf85-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span></span>

    <span data-ttu-id="aaf85-159">b.</span><span class="sxs-lookup"><span data-stu-id="aaf85-159">b.</span></span> <span data-ttu-id="aaf85-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span><span class="sxs-lookup"><span data-stu-id="aaf85-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span></span>

4. <span data-ttu-id="aaf85-161">Ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aaf85-161">If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url2.png)

    <span data-ttu-id="aaf85-163">a.</span><span class="sxs-lookup"><span data-stu-id="aaf85-163">a.</span></span> <span data-ttu-id="aaf85-164">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="aaf85-164">Click the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="aaf85-165">b.</span><span class="sxs-lookup"><span data-stu-id="aaf85-165">b.</span></span> <span data-ttu-id="aaf85-166">Az a **URL-cím bejelentkezési** szövegmező, adja meg az URL-címet:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span><span class="sxs-lookup"><span data-stu-id="aaf85-166">In the **Sign On URL** textbox, type a URL as: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aaf85-167">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="aaf85-167">These values are not real.</span></span> <span data-ttu-id="aaf85-168">Ezek a tényleges URL-címek és az oktatóanyag későbbi részében ismertetett azonosító értékét kell.</span><span class="sxs-lookup"><span data-stu-id="aaf85-168">You'll need to update these value from actual URLs and Identifier which is explained later in the tutorial.</span></span>
    
5. <span data-ttu-id="aaf85-169">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="aaf85-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. <span data-ttu-id="aaf85-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="aaf85-173">Egyszeri bejelentkezés konfigurálása **123ContactForm** oldalán, keresse fel [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aaf85-173">To configure single sign-on on **123ContactForm** side, go to [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) and perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    <span data-ttu-id="aaf85-175">a.</span><span class="sxs-lookup"><span data-stu-id="aaf85-175">a.</span></span> <span data-ttu-id="aaf85-176">Az a **E-mail** szövegmező, írja be az e-mailt, a felhasználó Egytényezős</span><span class="sxs-lookup"><span data-stu-id="aaf85-176">In the **Email** textbox, type the email of the user i.e</span></span> <span data-ttu-id="aaf85-177">**BrittaSimon@Contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-177">**BrittaSimon@Contoso.com**.</span></span>

    <span data-ttu-id="aaf85-178">b.</span><span class="sxs-lookup"><span data-stu-id="aaf85-178">b.</span></span> <span data-ttu-id="aaf85-179">Kattintson a **feltöltése** , és keresse meg a metaadatok XML-fájl, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="aaf85-179">Click **Upload** and browse the Metadata XML file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="aaf85-180">c.</span><span class="sxs-lookup"><span data-stu-id="aaf85-180">c.</span></span> <span data-ttu-id="aaf85-181">Kattintson a **SUBMIT űrlap**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-181">Click **SUBMIT FORM**.</span></span>

8. <span data-ttu-id="aaf85-182">Az a **Microsoft Azure AD egyszeri bejelentkezés - alkalmazások beállításainak konfigurálása** hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aaf85-182">On the **Microsoft Azure AD - Single sign-on - Configure App Settings** perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url3.png)

    <span data-ttu-id="aaf85-184">a.</span><span class="sxs-lookup"><span data-stu-id="aaf85-184">a.</span></span> <span data-ttu-id="aaf85-185">Ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, másolása a **azonosítója** érték, a példány, és illessze be **azonosítója** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="aaf85-185">If you wish to configure the application in **IDP initiated mode**, copy the **IDENTIFIER** value for your instance and paste it in **Identifier** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="aaf85-186">b.</span><span class="sxs-lookup"><span data-stu-id="aaf85-186">b.</span></span> <span data-ttu-id="aaf85-187">Ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, másolása a **válasz URL-CÍMEN** érték, a példány, és illessze be **válasz URL-CÍMEN** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="aaf85-187">If you wish to configure the application in **IDP initiated mode**, copy the **REPLY URL** value for your instance and paste it in **Reply URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="aaf85-188">c.</span><span class="sxs-lookup"><span data-stu-id="aaf85-188">c.</span></span> <span data-ttu-id="aaf85-189">Ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, másolása a **SIGN-ON URL** érték, a példány, és illessze be **URL-cím bejelentkezési** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="aaf85-189">If you wish to configure the application in **SP initiated mode**, copy the **SIGN ON URL** value for your instance and paste it in **Sign On URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="aaf85-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="aaf85-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="aaf85-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="aaf85-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="aaf85-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aaf85-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aaf85-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaf85-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="aaf85-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="aaf85-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="aaf85-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="aaf85-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="aaf85-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aaf85-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aaf85-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="aaf85-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aaf85-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="aaf85-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aaf85-205">a.</span><span class="sxs-lookup"><span data-stu-id="aaf85-205">a.</span></span> <span data-ttu-id="aaf85-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aaf85-207">b.</span><span class="sxs-lookup"><span data-stu-id="aaf85-207">b.</span></span> <span data-ttu-id="aaf85-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aaf85-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aaf85-209">c.</span><span class="sxs-lookup"><span data-stu-id="aaf85-209">c.</span></span> <span data-ttu-id="aaf85-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="aaf85-211">d.</span><span class="sxs-lookup"><span data-stu-id="aaf85-211">d.</span></span> <span data-ttu-id="aaf85-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-212">Click **Create**.</span></span>
 
### <a name="creating-a-123contactform-test-user"></a><span data-ttu-id="aaf85-213">123ContactForm tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="aaf85-213">Creating a 123ContactForm test user</span></span>

<span data-ttu-id="aaf85-214">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="aaf85-214">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="aaf85-215">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="aaf85-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="aaf85-216">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó 123ContactForm való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="aaf85-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 123ContactForm.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="aaf85-218">**Britta Simon hozzárendelése 123ContactForm, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="aaf85-218">**To assign Britta Simon to 123ContactForm, perform the following steps:**</span></span>

1. <span data-ttu-id="aaf85-219">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="aaf85-221">Az alkalmazások listában válassza ki a **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-221">In the applications list, select **123ContactForm**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. <span data-ttu-id="aaf85-223">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="aaf85-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="aaf85-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="aaf85-225">Click **Add** button.</span></span> <span data-ttu-id="aaf85-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aaf85-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="aaf85-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="aaf85-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="aaf85-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aaf85-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aaf85-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aaf85-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aaf85-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="aaf85-231">Testing single sign-on</span></span>

<span data-ttu-id="aaf85-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="aaf85-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="aaf85-233">Ha a hozzáférési panelen 123ContactForm csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az 123ContactForm alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="aaf85-233">When you click the 123ContactForm tile in the Access Panel, you should get automatically signed-on to your 123ContactForm application.</span></span>
<span data-ttu-id="aaf85-234">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aaf85-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aaf85-235">További források</span><span class="sxs-lookup"><span data-stu-id="aaf85-235">Additional resources</span></span>

* [<span data-ttu-id="aaf85-236">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="aaf85-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aaf85-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="aaf85-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

