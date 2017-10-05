---
title: "Oktatóanyag: Azure Active Directoryval integrált Allocadia |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Allocadia között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: 8e97c365383ecdb72cc1cd449b522b75875fc1db
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a><span data-ttu-id="e5146-103">Oktatóanyag: Azure Active Directoryval integrált Allocadia</span><span class="sxs-lookup"><span data-stu-id="e5146-103">Tutorial: Azure Active Directory integration with Allocadia</span></span>

<span data-ttu-id="e5146-104">Ebben az oktatóanyagban elsajátíthatja Allocadia integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5146-104">In this tutorial, you learn how to integrate Allocadia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5146-105">Allocadia integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e5146-105">Integrating Allocadia with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e5146-106">Megadhatja a Allocadia hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e5146-106">You can control in Azure AD who has access to Allocadia</span></span>
- <span data-ttu-id="e5146-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Allocadia (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e5146-107">You can enable your users to automatically get signed-on to Allocadia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5146-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5146-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e5146-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5146-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5146-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5146-110">Prerequisites</span></span>

<span data-ttu-id="e5146-111">Konfigurálása az Azure AD-integrációs Allocadia, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="e5146-111">To configure Azure AD integration with Allocadia, you need the following items:</span></span>

- <span data-ttu-id="e5146-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e5146-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5146-113">Egy Allocadia egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e5146-113">An Allocadia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5146-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e5146-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5146-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e5146-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5146-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e5146-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5146-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5146-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5146-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e5146-118">Scenario description</span></span>
<span data-ttu-id="e5146-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e5146-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5146-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e5146-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5146-121">A gyűjteményből Allocadia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5146-121">Adding Allocadia from the gallery</span></span>
2. <span data-ttu-id="e5146-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5146-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-allocadia-from-the-gallery"></a><span data-ttu-id="e5146-123">A gyűjteményből Allocadia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5146-123">Adding Allocadia from the gallery</span></span>
<span data-ttu-id="e5146-124">Az Azure AD integrálása a Allocadia konfigurálásához kell hozzáadnia Allocadia a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e5146-124">To configure the integration of Allocadia into Azure AD, you need to add Allocadia from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e5146-125">**A gyűjteményből Allocadia hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e5146-125">**To add Allocadia from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e5146-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5146-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5146-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e5146-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e5146-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5146-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e5146-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e5146-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e5146-133">Írja be a keresőmezőbe, **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="e5146-133">In the search box, type **Allocadia**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_search.png)

5. <span data-ttu-id="e5146-135">Az eredmények panelen válassza ki a **Allocadia**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e5146-135">In the results panel, select **Allocadia**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e5146-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e5146-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e5146-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Allocadia</span><span class="sxs-lookup"><span data-stu-id="e5146-138">In this section, you configure and test Azure AD single sign-on with Allocadia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e5146-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Allocadia a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e5146-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Allocadia is to a user in Azure AD.</span></span> <span data-ttu-id="e5146-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Allocadia közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e5146-140">In other words, a link relationship between an Azure AD user and the related user in Allocadia needs to be established.</span></span>

<span data-ttu-id="e5146-141">Allocadia, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e5146-141">In Allocadia, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e5146-142">Az Azure AD egyszeri bejelentkezést a Allocadia tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e5146-142">To configure and test Azure AD single sign-on with Allocadia, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e5146-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e5146-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e5146-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e5146-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5146-145">**[Egy Allocadia tesztfelhasználó létrehozása](#creating-an-allocadia-test-user)**  - való Britta Simon valami Allocadia, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e5146-145">**[Creating an Allocadia test user](#creating-an-allocadia-test-user)** - to have a counterpart of Britta Simon in Allocadia that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5146-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e5146-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5146-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e5146-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e5146-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5146-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e5146-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Allocadia alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e5146-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Allocadia application.</span></span>

<span data-ttu-id="e5146-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Allocadia, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e5146-150">**To configure Azure AD single sign-on with Allocadia, perform the following steps:**</span></span>

1. <span data-ttu-id="e5146-151">Az Azure portálon a a **Allocadia** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e5146-151">In the Azure portal, on the **Allocadia** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e5146-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e5146-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. <span data-ttu-id="e5146-155">Az a **Allocadia tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e5146-155">On the **Allocadia Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_url.png)

    <span data-ttu-id="e5146-157">a.</span><span class="sxs-lookup"><span data-stu-id="e5146-157">a.</span></span> <span data-ttu-id="e5146-158">Az a **azonosító** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="e5146-158">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
       
     <span data-ttu-id="e5146-159">A tesztkörnyezet-`https://na2standby.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="e5146-159">For test environment -  `https://na2standby.allocadia.com`</span></span>
    
     <span data-ttu-id="e5146-160">Az éles környezetben-`https://na2.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="e5146-160">For production environment - `https://na2.allocadia.com`</span></span>

    <span data-ttu-id="e5146-161">b.</span><span class="sxs-lookup"><span data-stu-id="e5146-161">b.</span></span> <span data-ttu-id="e5146-162">Az a **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="e5146-162">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    
     <span data-ttu-id="e5146-163">A tesztkörnyezet-`https://na2standby.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="e5146-163">For test environment - `https://na2standby.allocadia.com/allocadia/saml/SSO`</span></span>
    
     <span data-ttu-id="e5146-164">Az éles környezetben-`https://na2.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="e5146-164">For production environment - `https://na2.allocadia.com/allocadia/saml/SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5146-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e5146-165">These values are not real.</span></span> <span data-ttu-id="e5146-166">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="e5146-166">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="e5146-167">Ügyfél [Allocadia támogatási csoport](mailTo:support@allocadia.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e5146-167">Contact [Allocadia support team](mailTo:support@allocadia.com) to get these values.</span></span>

4. <span data-ttu-id="e5146-168">Allocadia alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="e5146-168">Allocadia application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="e5146-169">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e5146-169">Configure the following claims for this application.</span></span> <span data-ttu-id="e5146-170">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="e5146-170">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="e5146-171">Az alábbi képernyőfelvételen látható egy példa ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="e5146-171">The following screenshot shows an example for this configuration.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. <span data-ttu-id="e5146-173">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e5146-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="e5146-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="e5146-174">Attribute Name</span></span> | <span data-ttu-id="e5146-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="e5146-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="e5146-176">Utónév</span><span class="sxs-lookup"><span data-stu-id="e5146-176">firstname</span></span> | <span data-ttu-id="e5146-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="e5146-177">user.givenname</span></span> |
    | <span data-ttu-id="e5146-178">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="e5146-178">lastname</span></span> | <span data-ttu-id="e5146-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="e5146-179">user.surname</span></span> |
    | <span data-ttu-id="e5146-180">E-mailek</span><span class="sxs-lookup"><span data-stu-id="e5146-180">email</span></span> | <span data-ttu-id="e5146-181">User.mail</span><span class="sxs-lookup"><span data-stu-id="e5146-181">user.mail</span></span> |
    
    <span data-ttu-id="e5146-182">a.</span><span class="sxs-lookup"><span data-stu-id="e5146-182">a.</span></span> <span data-ttu-id="e5146-183">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5146-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="e5146-185">b.</span><span class="sxs-lookup"><span data-stu-id="e5146-185">b.</span></span> <span data-ttu-id="e5146-186">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="e5146-186">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="e5146-188">c.</span><span class="sxs-lookup"><span data-stu-id="e5146-188">c.</span></span> <span data-ttu-id="e5146-189">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="e5146-189">From the **Value** list, type the attribute value shown for that row.</span></span>
 
    <span data-ttu-id="e5146-190">d.</span><span class="sxs-lookup"><span data-stu-id="e5146-190">d.</span></span> <span data-ttu-id="e5146-191">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5146-191">Click **Ok**.</span></span>



6. <span data-ttu-id="e5146-192">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e5146-192">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. <span data-ttu-id="e5146-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5146-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="e5146-196">Egyszeri bejelentkezés konfigurálása **Allocadia** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Allocadia támogatási csoport](mailTo:support@allocadia.com).</span><span class="sxs-lookup"><span data-stu-id="e5146-196">To configure single sign-on on **Allocadia** side, you need to send the downloaded **Metadata XML** to [Allocadia support team](mailTo:support@allocadia.com).</span></span> <span data-ttu-id="e5146-197">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="e5146-197">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e5146-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e5146-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e5146-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e5146-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e5146-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5146-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e5146-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5146-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="e5146-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e5146-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e5146-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e5146-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e5146-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e5146-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5146-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e5146-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5146-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e5146-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5146-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e5146-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5146-213">a.</span><span class="sxs-lookup"><span data-stu-id="e5146-213">a.</span></span> <span data-ttu-id="e5146-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5146-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5146-215">b.</span><span class="sxs-lookup"><span data-stu-id="e5146-215">b.</span></span> <span data-ttu-id="e5146-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5146-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5146-217">c.</span><span class="sxs-lookup"><span data-stu-id="e5146-217">c.</span></span> <span data-ttu-id="e5146-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e5146-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e5146-219">d.</span><span class="sxs-lookup"><span data-stu-id="e5146-219">d.</span></span> <span data-ttu-id="e5146-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5146-220">Click **Create**.</span></span>
 
### <a name="creating-an-allocadia-test-user"></a><span data-ttu-id="e5146-221">Egy Allocadia tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5146-221">Creating an Allocadia test user</span></span>

<span data-ttu-id="e5146-222">Alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="e5146-222">Application supports Just in time user provisioning.</span></span> <span data-ttu-id="e5146-223">A hitelesítés után felhasználók automatikusan jönnek létre az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e5146-223">After authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e5146-224">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e5146-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e5146-225">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Allocadia Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="e5146-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Allocadia.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e5146-227">**Britta Simon hozzárendelése Allocadia, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e5146-227">**To assign Britta Simon to Allocadia, perform the following steps:**</span></span>

1. <span data-ttu-id="e5146-228">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e5146-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e5146-230">Az alkalmazások listában válassza ki a **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="e5146-230">In the applications list, select **Allocadia**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_app.png) 

3. <span data-ttu-id="e5146-232">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e5146-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e5146-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5146-234">Click **Add** button.</span></span> <span data-ttu-id="e5146-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5146-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e5146-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e5146-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e5146-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5146-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5146-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e5146-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e5146-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e5146-240">Testing single sign-on</span></span>

<span data-ttu-id="e5146-241">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e5146-241">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e5146-242">Ha a hozzáférési panelen Allocadia csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Allocadia alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="e5146-242">When you click the Allocadia tile in the Access Panel, you should get automatically signed-on to your Allocadia application.</span></span>
<span data-ttu-id="e5146-243">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="e5146-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5146-244">További források</span><span class="sxs-lookup"><span data-stu-id="e5146-244">Additional resources</span></span>

* [<span data-ttu-id="e5146-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e5146-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5146-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e5146-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png

