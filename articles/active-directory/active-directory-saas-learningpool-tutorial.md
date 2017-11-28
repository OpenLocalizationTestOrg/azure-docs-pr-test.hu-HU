---
title: "Oktatóanyag: Azure Active Directoryval integrált Learningpool Act |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a törvény Learningpool között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 51e8695f-31e1-4d09-8eb3-13241999d99f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 932f5f12c75299e532d3fa2c31f1805a7df30158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learningpool-act"></a><span data-ttu-id="ee6f7-103">Oktatóanyag: Azure Active Directoryval integrált Learningpool Act</span><span class="sxs-lookup"><span data-stu-id="ee6f7-103">Tutorial: Azure Active Directory integration with Learningpool Act</span></span>

<span data-ttu-id="ee6f7-104">Ebben az oktatóanyagban elsajátíthatja Learningpool Act integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-104">In this tutorial, you learn how to integrate Learningpool Act with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ee6f7-105">Learningpool Act integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-105">Integrating Learningpool Act with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ee6f7-106">Megadhatja a Learningpool Act hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ee6f7-106">You can control in Azure AD who has access to Learningpool Act</span></span>
- <span data-ttu-id="ee6f7-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Learningpool el (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="ee6f7-107">You can enable your users to automatically get signed-on to Learningpool Act (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ee6f7-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ee6f7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ee6f7-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee6f7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ee6f7-110">Prerequisites</span></span>

<span data-ttu-id="ee6f7-111">Az Azure AD-integrációs Learningpool Act konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-111">To configure Azure AD integration with Learningpool Act, you need the following items:</span></span>

- <span data-ttu-id="ee6f7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ee6f7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ee6f7-113">Egy Learningpool Act egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ee6f7-113">A Learningpool Act single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ee6f7-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ee6f7-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ee6f7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ee6f7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ee6f7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-118">Scenario description</span></span>
<span data-ttu-id="ee6f7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ee6f7-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ee6f7-121">A gyűjteményből Learningpool Act hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-121">Adding Learningpool Act from the gallery</span></span>
2. <span data-ttu-id="ee6f7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ee6f7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learningpool-act-from-the-gallery"></a><span data-ttu-id="ee6f7-123">A gyűjteményből Learningpool Act hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-123">Adding Learningpool Act from the gallery</span></span>
<span data-ttu-id="ee6f7-124">Az Azure AD integrálása a Learningpool Act konfigurálásához kell hozzáadnia Learningpool Act a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-124">To configure the integration of Learningpool Act into Azure AD, you need to add Learningpool Act from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ee6f7-125">**A gyűjteményből Learningpool Act hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ee6f7-125">**To add Learningpool Act from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ee6f7-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ee6f7-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ee6f7-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ee6f7-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ee6f7-133">Írja be a keresőmezőbe, **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-133">In the search box, type **Learningpool Act**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_search.png)

5. <span data-ttu-id="ee6f7-135">Az eredmények panelen válassza ki a **Learningpool Act**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-135">In the results panel, select **Learningpool Act**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ee6f7-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ee6f7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ee6f7-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Learningpool Act-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-138">In this section, you configure and test Azure AD single sign-on with Learningpool Act based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ee6f7-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Learningpool Act a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learningpool Act is to a user in Azure AD.</span></span> <span data-ttu-id="ee6f7-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Learningpool Act közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-140">In other words, a link relationship between an Azure AD user and the related user in Learningpool Act needs to be established.</span></span>

<span data-ttu-id="ee6f7-141">Learningpool Act, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-141">In Learningpool Act, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ee6f7-142">Az Azure AD egyszeri bejelentkezést a Learningpool Act tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-142">To configure and test Azure AD single sign-on with Learningpool Act, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ee6f7-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ee6f7-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ee6f7-145">**[Learningpool Act tesztfelhasználó létrehozása](#creating-a-learningpool-act-test-user)**  - való egy megfelelője a Britta Simon Learningpool Act, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-145">**[Creating a Learningpool Act test user](#creating-a-learningpool-act-test-user)** - to have a counterpart of Britta Simon in Learningpool Act that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ee6f7-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ee6f7-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ee6f7-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ee6f7-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Learningpool Act-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learningpool Act application.</span></span>

<span data-ttu-id="ee6f7-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Learningpool Act, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ee6f7-150">**To configure Azure AD single sign-on with Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="ee6f7-151">Az Azure portálon a a **Learningpool Act** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-151">In the Azure portal, on the **Learningpool Act** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ee6f7-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_samlbase.png)

3. <span data-ttu-id="ee6f7-155">Az a **Learningpool Act tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-155">On the **Learningpool Act Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_url.png)

    <span data-ttu-id="ee6f7-157">a.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-157">a.</span></span> <span data-ttu-id="ee6f7-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span><span class="sxs-lookup"><span data-stu-id="ee6f7-158">In the **Sign-on URL** textbox, type the URL: `https://parliament.preview.Learningpool.com/auth/shibboleth/index.php`</span></span>

    <span data-ttu-id="ee6f7-159">b.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-159">b.</span></span> <span data-ttu-id="ee6f7-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.Learningpool.com/shibboleth` |
    | `https://<subdomain>.preview.Learningpool.com/shibboleth` |

    > [!NOTE] 
    > <span data-ttu-id="ee6f7-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-161">These values are not real.</span></span> <span data-ttu-id="ee6f7-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ee6f7-163">Ügyfél [Learningpool Act ügyfél-támogatási csoport](https://www.Learningpool.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-163">Contact [Learningpool Act Client support team](https://www.Learningpool.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="ee6f7-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_certificate.png) 

5. <span data-ttu-id="ee6f7-166">Learningpool Act alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-166">Learningpool Act application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="ee6f7-167">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="ee6f7-168">Ezek az attribútumok értékének kezelheti a **"Atrribute"** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-168">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="ee6f7-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-169">The following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_attribute.png) 

6. <span data-ttu-id="ee6f7-171">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="ee6f7-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="ee6f7-172">Attribute Name</span></span> | <span data-ttu-id="ee6f7-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="ee6f7-173">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="ee6f7-174">urn: oid:1.2.840.113556.1.4.221</span><span class="sxs-lookup"><span data-stu-id="ee6f7-174">urn:oid:1.2.840.113556.1.4.221</span></span> | <span data-ttu-id="ee6f7-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="ee6f7-175">user.userprincipalname</span></span> |
    | <span data-ttu-id="ee6f7-176">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="ee6f7-176">urn:oid:2.5.4.42</span></span> | <span data-ttu-id="ee6f7-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="ee6f7-177">user.givenname</span></span> |
    | <span data-ttu-id="ee6f7-178">urn: oid:0.9.2342.19200300.100.1.3</span><span class="sxs-lookup"><span data-stu-id="ee6f7-178">urn:oid:0.9.2342.19200300.100.1.3</span></span> | <span data-ttu-id="ee6f7-179">User.mail</span><span class="sxs-lookup"><span data-stu-id="ee6f7-179">user.mail</span></span> |    
    | <span data-ttu-id="ee6f7-180">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="ee6f7-180">urn:oid:2.5.4.4</span></span> | <span data-ttu-id="ee6f7-181">User.surname</span><span class="sxs-lookup"><span data-stu-id="ee6f7-181">user.surname</span></span> |
    
    <span data-ttu-id="ee6f7-182">a.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-182">a.</span></span> <span data-ttu-id="ee6f7-183">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="ee6f7-186">b.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-186">b.</span></span> <span data-ttu-id="ee6f7-187">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-187">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="ee6f7-188">c.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-188">c.</span></span> <span data-ttu-id="ee6f7-189">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-189">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="ee6f7-190">d.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-190">d.</span></span> <span data-ttu-id="ee6f7-191">Hagyja a **Namespace** üres.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-191">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="ee6f7-192">e.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-192">e.</span></span> <span data-ttu-id="ee6f7-193">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-193">Click **Ok**.</span></span>

7. <span data-ttu-id="ee6f7-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="ee6f7-196">Egyszeri bejelentkezés konfigurálása **Learningpool Act** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Learningpool Act támogatási csoport](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-196">To configure single sign-on on **Learningpool Act** side, you need to send the downloaded **Metadata XML** to [Learningpool Act support team](https://www.Learningpool.com/support).</span></span> <span data-ttu-id="ee6f7-197">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-197">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ee6f7-198">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ee6f7-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ee6f7-199">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ee6f7-200">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ee6f7-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ee6f7-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="ee6f7-202">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ee6f7-204">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ee6f7-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ee6f7-205">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ee6f7-207">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ee6f7-209">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ee6f7-211">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ee6f7-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Learningpool-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ee6f7-213">a.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-213">a.</span></span> <span data-ttu-id="ee6f7-214">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ee6f7-215">b.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-215">b.</span></span> <span data-ttu-id="ee6f7-216">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ee6f7-217">c.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-217">c.</span></span> <span data-ttu-id="ee6f7-218">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ee6f7-219">d.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-219">d.</span></span> <span data-ttu-id="ee6f7-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-220">Click **Create**.</span></span>
 
### <a name="creating-a-learningpool-act-test-user"></a><span data-ttu-id="ee6f7-221">Learningpool Act tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-221">Creating a Learningpool Act test user</span></span>

<span data-ttu-id="ee6f7-222">Ahhoz, hogy az Azure AD-felhasználók Learningpool Act bejelentkezni, akkor ki kell építenie Learningpool Act be.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-222">To enable Azure AD users to log in to Learningpool Act, they must be provisioned into Learningpool Act.</span></span>

<span data-ttu-id="ee6f7-223">Nincs művelet elem konfigurálhatók a felhasználók átadásához Learningpool el.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-223">There is no action item for you to configure user provisioning to Learningpool Act.</span></span>  
<span data-ttu-id="ee6f7-224">A felhasználóknak kell létrehozni a [Learningpool Act támogatási csoport](https://www.Learningpool.com/support).</span><span class="sxs-lookup"><span data-stu-id="ee6f7-224">Users need to be created by your [Learningpool Act support team](https://www.Learningpool.com/support).</span></span>

>[!NOTE]
><span data-ttu-id="ee6f7-225">Bármely más Learningpool Act felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz Learningpool Act által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-225">You can use any other Learningpool Act user account creation tools or APIs provided by Learningpool Act to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ee6f7-226">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ee6f7-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ee6f7-227">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Learningpool Act Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-227">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learningpool Act.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ee6f7-229">**Britta Simon hozzárendelése Learningpool Act, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ee6f7-229">**To assign Britta Simon to Learningpool Act, perform the following steps:**</span></span>

1. <span data-ttu-id="ee6f7-230">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ee6f7-232">Az alkalmazások listában válassza ki a **Learningpool Act**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-232">In the applications list, select **Learningpool Act**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Learningpool-tutorial/tutorial_Learningpoolact_app.png) 

3. <span data-ttu-id="ee6f7-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ee6f7-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-236">Click **Add** button.</span></span> <span data-ttu-id="ee6f7-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ee6f7-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ee6f7-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ee6f7-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ee6f7-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ee6f7-242">Testing single sign-on</span></span>

<span data-ttu-id="ee6f7-243">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ee6f7-244">Ha a hozzáférési panelen Learningpool Act csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Learningpool Act alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ee6f7-244">When you click the Learningpool Act tile in the Access Panel, you should get automatically signed-on to your Learningpool Act application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee6f7-245">További források</span><span class="sxs-lookup"><span data-stu-id="ee6f7-245">Additional resources</span></span>

* [<span data-ttu-id="ee6f7-246">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ee6f7-246">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ee6f7-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ee6f7-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Learningpool-tutorial/tutorial_general_203.png

