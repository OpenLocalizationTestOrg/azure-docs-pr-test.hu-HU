---
title: "Oktatóanyag: Azure Active Directoryval integrált Hightail |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Hightail között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: ba55f9b62d274aa3eb91723c62b53f54de0891b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="1a1db-103">Oktatóanyag: Azure Active Directoryval integrált Hightail</span><span class="sxs-lookup"><span data-stu-id="1a1db-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="1a1db-104">Ebben az oktatóanyagban elsajátíthatja Hightail integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1a1db-104">In this tutorial, you learn how to integrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a1db-105">Hightail integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1a1db-105">Integrating Hightail with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1a1db-106">Megadhatja a Hightail hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1a1db-106">You can control in Azure AD who has access to Hightail</span></span>
- <span data-ttu-id="1a1db-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Hightail (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1a1db-107">You can enable your users to automatically get signed-on to Hightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1a1db-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1a1db-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1a1db-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a1db-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a1db-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1a1db-110">Prerequisites</span></span>

<span data-ttu-id="1a1db-111">Konfigurálása az Azure AD-integrációs Hightail, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1a1db-111">To configure Azure AD integration with Hightail, you need the following items:</span></span>

- <span data-ttu-id="1a1db-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1a1db-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a1db-113">Egy Hightail egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1a1db-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a1db-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1a1db-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a1db-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1a1db-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a1db-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1a1db-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a1db-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a1db-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a1db-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1a1db-118">Scenario description</span></span>
<span data-ttu-id="1a1db-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1a1db-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a1db-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1a1db-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a1db-121">A gyűjteményből Hightail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a1db-121">Adding Hightail from the gallery</span></span>
2. <span data-ttu-id="1a1db-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a1db-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-the-gallery"></a><span data-ttu-id="1a1db-123">A gyűjteményből Hightail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a1db-123">Adding Hightail from the gallery</span></span>
<span data-ttu-id="1a1db-124">Az Azure AD integrálása a Hightail konfigurálásához kell hozzáadnia Hightail a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1a1db-124">To configure the integration of Hightail into Azure AD, you need to add Hightail from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1a1db-125">**A gyűjteményből Hightail hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a1db-125">**To add Hightail from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1a1db-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1a1db-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1a1db-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1a1db-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1a1db-133">Írja be a keresőmezőbe, **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-133">In the search box, type **Hightail**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="1a1db-135">Az eredmények panelen válassza ki a **Hightail**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a1db-135">In the results panel, select **Hightail**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1a1db-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a1db-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1a1db-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Hightail.</span><span class="sxs-lookup"><span data-stu-id="1a1db-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1a1db-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Hightail a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1a1db-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Hightail is to a user in Azure AD.</span></span> <span data-ttu-id="1a1db-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Hightail közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1a1db-140">In other words, a link relationship between an Azure AD user and the related user in Hightail needs to be established.</span></span>

<span data-ttu-id="1a1db-141">Hightail, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1a1db-141">In Hightail, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1a1db-142">Az Azure AD egyszeri bejelentkezést a Hightail tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1a1db-142">To configure and test Azure AD single sign-on with Hightail, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1a1db-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1a1db-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1a1db-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1a1db-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a1db-145">**[Hightail tesztfelhasználó létrehozása](#creating-a-hightail-test-user)**  - való Britta Simon valami Hightail, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1a1db-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - to have a counterpart of Britta Simon in Hightail that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1a1db-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1a1db-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a1db-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1a1db-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1a1db-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a1db-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1a1db-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Hightail alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1a1db-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="1a1db-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Hightail, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a1db-150">**To configure Azure AD single sign-on with Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="1a1db-151">Az Azure portálon a a **Hightail** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-151">In the Azure portal, on the **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1a1db-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1a1db-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="1a1db-155">Az a **Hightail tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1a1db-155">On the **Hightail Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="1a1db-157">Az a **válasz URL-CÍMEN** szövegmező, írja be az URL-címet:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="1a1db-157">In the **Reply URL** textbox, type the URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1a1db-158">Az előző érték nincs valós értékének.</span><span class="sxs-lookup"><span data-stu-id="1a1db-158">The preceding value is not real value.</span></span> <span data-ttu-id="1a1db-159">Az érték a tényleges válasz URL-címet, az oktatóanyag későbbi részében ismertetett frissíti.</span><span class="sxs-lookup"><span data-stu-id="1a1db-159">You will update the value with the actual Reply URL, which is explained later in the tutorial.</span></span>
 
4. <span data-ttu-id="1a1db-160">Az a **Hightail tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1a1db-160">On the **Hightail Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="1a1db-162">a.</span><span class="sxs-lookup"><span data-stu-id="1a1db-162">a.</span></span> <span data-ttu-id="1a1db-163">Kattintson a **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-163">Click the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="1a1db-164">b.</span><span class="sxs-lookup"><span data-stu-id="1a1db-164">b.</span></span> <span data-ttu-id="1a1db-165">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="1a1db-165">In the **Sign On URL** textbox, type the URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="1a1db-166">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1a1db-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="1a1db-168">Hightail alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="1a1db-168">Hightail application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="1a1db-169">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1a1db-169">Please configure the following claims for this application.</span></span> <span data-ttu-id="1a1db-170">Ezek az attribútumok értékének kezelheti a **"Atrribute"** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="1a1db-170">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="1a1db-171">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="1a1db-171">The following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="1a1db-173">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1a1db-173">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="1a1db-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="1a1db-174">Attribute Name</span></span> | <span data-ttu-id="1a1db-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="1a1db-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="1a1db-176">Utónév</span><span class="sxs-lookup"><span data-stu-id="1a1db-176">FirstName</span></span> | <span data-ttu-id="1a1db-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="1a1db-177">user.givenname</span></span> |
    | <span data-ttu-id="1a1db-178">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="1a1db-178">LastName</span></span> | <span data-ttu-id="1a1db-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="1a1db-179">user.surname</span></span> |
    | <span data-ttu-id="1a1db-180">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="1a1db-180">Email</span></span> | <span data-ttu-id="1a1db-181">User.mail</span><span class="sxs-lookup"><span data-stu-id="1a1db-181">user.mail</span></span> |    
    | <span data-ttu-id="1a1db-182">UserIdentity</span><span class="sxs-lookup"><span data-stu-id="1a1db-182">UserIdentity</span></span> | <span data-ttu-id="1a1db-183">User.mail</span><span class="sxs-lookup"><span data-stu-id="1a1db-183">user.mail</span></span> |
    
    <span data-ttu-id="1a1db-184">a.</span><span class="sxs-lookup"><span data-stu-id="1a1db-184">a.</span></span> <span data-ttu-id="1a1db-185">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a1db-185">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="1a1db-188">b.</span><span class="sxs-lookup"><span data-stu-id="1a1db-188">b.</span></span> <span data-ttu-id="1a1db-189">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="1a1db-189">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="1a1db-190">c.</span><span class="sxs-lookup"><span data-stu-id="1a1db-190">c.</span></span> <span data-ttu-id="1a1db-191">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="1a1db-191">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="1a1db-192">d.</span><span class="sxs-lookup"><span data-stu-id="1a1db-192">d.</span></span> <span data-ttu-id="1a1db-193">Hagyja a **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="1a1db-193">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="1a1db-194">e.</span><span class="sxs-lookup"><span data-stu-id="1a1db-194">e.</span></span> <span data-ttu-id="1a1db-195">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-195">Click **Ok**.</span></span>

7. <span data-ttu-id="1a1db-196">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-196">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="1a1db-198">A a **Hightail konfigurációs** kattintson **konfigurálása Hightail** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1a1db-198">On the **Hightail Configuration** section, click **Configure Hightail** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1a1db-199">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1a1db-199">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="1a1db-201">Az egyszeri bejelentkezés konfigurálása a következő Hightail app, előtt kérjük, az e-mail tartomány Hightail vonja össze az, hogy minden a felhasználóknak, akik ebben a tartományban az egyszeri bejelentkezés funkciót használhatja a fehér lista.</span><span class="sxs-lookup"><span data-stu-id="1a1db-201">Before configuring the Single Sign On at Hightail app, please white list your email domain with Hightail team so that all the users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="1a1db-202">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, meg kell bejelentkezés a Hightail bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1a1db-202">To get SSO configured for your application, you need to sign-on to your Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="1a1db-203">a.</span><span class="sxs-lookup"><span data-stu-id="1a1db-203">a.</span></span> <span data-ttu-id="1a1db-204">A felső menüben kattintson a **fiók** lapot, és válasszon **SAML konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-204">In the menu on the top, click the **Account** tab and select **Configure SAML**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="1a1db-206">b.</span><span class="sxs-lookup"><span data-stu-id="1a1db-206">b.</span></span> <span data-ttu-id="1a1db-207">A jelölőnégyzet bejelölésével **SAML-alapú hitelesítés engedélyezéséhez**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-207">Select the checkbox of **Enable SAML Authentication**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="1a1db-209">c.</span><span class="sxs-lookup"><span data-stu-id="1a1db-209">c.</span></span> <span data-ttu-id="1a1db-210">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról letöltött, a tartalmának másolása a vágólapra és illessze be azt a **SAML jogkivonat-aláíró tanúsítványa** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="1a1db-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Token Signing Certificate** textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="1a1db-212">d.</span><span class="sxs-lookup"><span data-stu-id="1a1db-212">d.</span></span> <span data-ttu-id="1a1db-213">Az a **SAML hatóság (identitásszolgáltató)** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** átmásolja az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1a1db-213">In the **SAML Authority (Identity Provider)** textbox, paste the value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="1a1db-215">e.</span><span class="sxs-lookup"><span data-stu-id="1a1db-215">e.</span></span> <span data-ttu-id="1a1db-216">Ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód** válasszon **"Identitásszolgáltató (IdP) kezdeményezett bejelentkezés"**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-216">If you wish to configure the application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="1a1db-217">Ha **Szolgáltató kezdeményezett mód** válasszon **"Szolgáltatókkal (SP) kezdeményezett bejelentkezés"**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="1a1db-219">f.</span><span class="sxs-lookup"><span data-stu-id="1a1db-219">f.</span></span> <span data-ttu-id="1a1db-220">A SAML-alapú ügyfél URL-címe, a példány másolja és illessze be azt a **válasz URL-CÍMEN** textbox **Hightail tartomány és az URL-címek** Azure-portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1a1db-220">Copy the SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="1a1db-221">g.</span><span class="sxs-lookup"><span data-stu-id="1a1db-221">g.</span></span> <span data-ttu-id="1a1db-222">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1a1db-223">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1a1db-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1a1db-224">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1a1db-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1a1db-225">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a1db-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1a1db-226">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a1db-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="1a1db-227">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1a1db-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1a1db-229">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a1db-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1a1db-230">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1a1db-232">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-232">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1a1db-234">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1a1db-234">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1a1db-236">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1a1db-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1a1db-238">a.</span><span class="sxs-lookup"><span data-stu-id="1a1db-238">a.</span></span> <span data-ttu-id="1a1db-239">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a1db-240">b.</span><span class="sxs-lookup"><span data-stu-id="1a1db-240">b.</span></span> <span data-ttu-id="1a1db-241">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1a1db-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1a1db-242">c.</span><span class="sxs-lookup"><span data-stu-id="1a1db-242">c.</span></span> <span data-ttu-id="1a1db-243">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1a1db-244">d.</span><span class="sxs-lookup"><span data-stu-id="1a1db-244">d.</span></span> <span data-ttu-id="1a1db-245">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="1a1db-246">Hightail tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a1db-246">Creating a Hightail test user</span></span>

<span data-ttu-id="1a1db-247">Ez a szakasz célja Hightail Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1a1db-247">The objective of this section is to create a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="1a1db-248">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="1a1db-248">There is no action item for you in this section.</span></span> <span data-ttu-id="1a1db-249">Támogatja az egyéni igények alapján just-in-time felhasználólétesítés hightail.</span><span class="sxs-lookup"><span data-stu-id="1a1db-249">Hightail supports just-in-time user provisioning based on the custom claims.</span></span> <span data-ttu-id="1a1db-250">Ha konfigurálta az egyéni igények, ahogy az a szakasz  **[az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on)**  újabb verziók esetén a felhasználók automatikusan létrejön az alkalmazás még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="1a1db-250">If you have configured the custom claims as shown in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in the application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="1a1db-251">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Hightail támogatási csoport](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="1a1db-251">If you need to create a user manually, you need to contact the [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1a1db-252">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1a1db-252">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1a1db-253">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Hightail Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1a1db-253">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Hightail.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1a1db-255">**Britta Simon hozzárendelése Hightail, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a1db-255">**To assign Britta Simon to Hightail, perform the following steps:**</span></span>

1. <span data-ttu-id="1a1db-256">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-256">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1a1db-258">Az alkalmazások listában válassza ki a **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-258">In the applications list, select **Hightail**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="1a1db-260">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1a1db-260">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1a1db-262">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a1db-262">Click **Add** button.</span></span> <span data-ttu-id="1a1db-263">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a1db-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1a1db-265">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1a1db-265">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1a1db-266">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a1db-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a1db-267">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a1db-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1a1db-268">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a1db-268">Testing single sign-on</span></span>

<span data-ttu-id="1a1db-269">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="1a1db-269">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1a1db-270">Ha a hozzáférési panelen Hightail csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Hightail alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1a1db-270">When you click the Hightail tile in the Access Panel, you should get automatically signed-on to your Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1a1db-271">További források</span><span class="sxs-lookup"><span data-stu-id="1a1db-271">Additional resources</span></span>

* [<span data-ttu-id="1a1db-272">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1a1db-272">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a1db-273">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1a1db-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

