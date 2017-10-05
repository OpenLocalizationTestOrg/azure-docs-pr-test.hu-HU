---
title: "Oktatóanyag: Azure Active Directoryval integrált Boomi |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Boomi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="58d5a-103">Oktatóanyag: Azure Active Directoryval integrált Boomi</span><span class="sxs-lookup"><span data-stu-id="58d5a-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="58d5a-104">Ebben az oktatóanyagban elsajátíthatja Boomi integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="58d5a-104">In this tutorial, you learn how to integrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="58d5a-105">Boomi integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="58d5a-105">Integrating Boomi with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="58d5a-106">Megadhatja a Boomi hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="58d5a-106">You can control in Azure AD who has access to Boomi</span></span>
- <span data-ttu-id="58d5a-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Boomi (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="58d5a-107">You can enable your users to automatically get signed-on to Boomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="58d5a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="58d5a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="58d5a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="58d5a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58d5a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="58d5a-110">Prerequisites</span></span>

<span data-ttu-id="58d5a-111">Konfigurálása az Azure AD-integrációs Boomi, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="58d5a-111">To configure Azure AD integration with Boomi, you need the following items:</span></span>

- <span data-ttu-id="58d5a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="58d5a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="58d5a-113">Egy Boomi egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="58d5a-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="58d5a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="58d5a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="58d5a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="58d5a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="58d5a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="58d5a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="58d5a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="58d5a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="58d5a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="58d5a-118">Scenario description</span></span>
<span data-ttu-id="58d5a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="58d5a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="58d5a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="58d5a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="58d5a-121">A gyűjteményből Boomi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="58d5a-121">Adding Boomi from the gallery</span></span>
2. <span data-ttu-id="58d5a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="58d5a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-the-gallery"></a><span data-ttu-id="58d5a-123">A gyűjteményből Boomi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="58d5a-123">Adding Boomi from the gallery</span></span>
<span data-ttu-id="58d5a-124">Az Azure AD integrálása a Boomi konfigurálásához kell hozzáadnia Boomi a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="58d5a-124">To configure the integration of Boomi into Azure AD, you need to add Boomi from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="58d5a-125">**A gyűjteményből Boomi hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="58d5a-125">**To add Boomi from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="58d5a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="58d5a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="58d5a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="58d5a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="58d5a-133">Írja be a keresőmezőbe, **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-133">In the search box, type **Boomi**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="58d5a-135">Az eredmények panelen válassza ki a **Boomi**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="58d5a-135">In the results panel, select **Boomi**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="58d5a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="58d5a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="58d5a-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Boomi.</span><span class="sxs-lookup"><span data-stu-id="58d5a-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="58d5a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Boomi a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="58d5a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Boomi is to a user in Azure AD.</span></span> <span data-ttu-id="58d5a-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Boomi közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="58d5a-140">In other words, a link relationship between an Azure AD user and the related user in Boomi needs to be established.</span></span>

<span data-ttu-id="58d5a-141">Boomi, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="58d5a-141">In Boomi, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="58d5a-142">Az Azure AD egyszeri bejelentkezést a Boomi tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="58d5a-142">To configure and test Azure AD single sign-on with Boomi, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="58d5a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="58d5a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="58d5a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="58d5a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="58d5a-145">**[Boomi tesztfelhasználó létrehozása](#creating-a-boomi-test-user)**  - való Britta Simon valami Boomi, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="58d5a-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - to have a counterpart of Britta Simon in Boomi that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="58d5a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="58d5a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="58d5a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="58d5a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="58d5a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58d5a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="58d5a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Boomi alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="58d5a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="58d5a-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Boomi, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="58d5a-150">**To configure Azure AD single sign-on with Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="58d5a-151">Az Azure portálon a a **Boomi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-151">In the Azure portal, on the **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="58d5a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="58d5a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="58d5a-155">Az a **Boomi tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="58d5a-155">On the **Boomi Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="58d5a-157">a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-157">a.</span></span> <span data-ttu-id="58d5a-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="58d5a-158">In the **Identifier** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="58d5a-159">b.</span><span class="sxs-lookup"><span data-stu-id="58d5a-159">b.</span></span> <span data-ttu-id="58d5a-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="58d5a-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="58d5a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="58d5a-161">These values are not real.</span></span> <span data-ttu-id="58d5a-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="58d5a-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="58d5a-163">Ügyfél [Boomi támogatási csoport](https://boomi.com/company/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="58d5a-163">Contact [Boomi support team](https://boomi.com/company/contact/) to get these values.</span></span>

4. <span data-ttu-id="58d5a-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="58d5a-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="58d5a-166">Boomi alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="58d5a-166">Boomi application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="58d5a-167">Állítsa be a következő jogcímeket ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="58d5a-167">Please configure the following claims for this application.</span></span> <span data-ttu-id="58d5a-168">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="58d5a-168">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="58d5a-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-169">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="58d5a-171">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanel, az alábbi táblázatban szereplő minden egyes sorára hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="58d5a-171">In the **User Attributes** section on the **Single sign-on** dialog, for each row shown in the table below, perform the following steps:</span></span>

    | <span data-ttu-id="58d5a-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="58d5a-172">Attribute Name</span></span> | <span data-ttu-id="58d5a-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="58d5a-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="58d5a-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="58d5a-174">FEDERATION_ID</span></span> | <span data-ttu-id="58d5a-175">User.mail</span><span class="sxs-lookup"><span data-stu-id="58d5a-175">user.mail</span></span> |
    
    <span data-ttu-id="58d5a-176">a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-176">a.</span></span> <span data-ttu-id="58d5a-177">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="58d5a-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="58d5a-180">b.</span><span class="sxs-lookup"><span data-stu-id="58d5a-180">b.</span></span> <span data-ttu-id="58d5a-181">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="58d5a-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="58d5a-182">c.</span><span class="sxs-lookup"><span data-stu-id="58d5a-182">c.</span></span> <span data-ttu-id="58d5a-183">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="58d5a-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="58d5a-184">d.</span><span class="sxs-lookup"><span data-stu-id="58d5a-184">d.</span></span> <span data-ttu-id="58d5a-185">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-185">Click **Ok**.</span></span>

6. <span data-ttu-id="58d5a-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-186">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="58d5a-188">A a **Boomi konfigurációs** kattintson **konfigurálása Boomi** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="58d5a-188">On the **Boomi Configuration** section, click **Configure Boomi** to open **Configure sign-on** window.</span></span> <span data-ttu-id="58d5a-189">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="58d5a-189">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="58d5a-191">Egy másik webes böngészőablakban jelentkezzen be a Boomi vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="58d5a-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="58d5a-192">Navigáljon a **vállalatnév** , és navigáljon **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-192">Navigate to **Company Name** and go to **Set up**.</span></span>

10. <span data-ttu-id="58d5a-193">Kattintson a **egyszeri bejelentkezési beállítások** lapra, és hajtsa végre a következő lépések.</span><span class="sxs-lookup"><span data-stu-id="58d5a-193">Click the **SSO Options** tab and perform below steps.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="58d5a-195">a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-195">a.</span></span> <span data-ttu-id="58d5a-196">Ellenőrizze **engedélyezése SAML-alapú egyszeri bejelentkezést** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="58d5a-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="58d5a-197">b.</span><span class="sxs-lookup"><span data-stu-id="58d5a-197">b.</span></span> <span data-ttu-id="58d5a-198">Kattintson a **importálási** az Azure AD-be a letöltött tanúsítvány feltöltése **szolgáltató Identitástanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-198">Click **Import** to upload the downloaded certificate from Azure AD to **Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="58d5a-199">c.</span><span class="sxs-lookup"><span data-stu-id="58d5a-199">c.</span></span> <span data-ttu-id="58d5a-200">Az a **Identity Provider bejelentkezési URL-cím** szövegmező, írja be az értéket a **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="58d5a-200">In the **Identity Provider Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="58d5a-201">d.</span><span class="sxs-lookup"><span data-stu-id="58d5a-201">d.</span></span> <span data-ttu-id="58d5a-202">Mint **összevonási azonosító hely**, jelölje be **összevonási azonosító FEDERATION_ID attribútum elem van** választógombot.</span><span class="sxs-lookup"><span data-stu-id="58d5a-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="58d5a-203">e.</span><span class="sxs-lookup"><span data-stu-id="58d5a-203">e.</span></span> <span data-ttu-id="58d5a-204">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="58d5a-205">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="58d5a-205">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="58d5a-206">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="58d5a-206">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="58d5a-207">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="58d5a-207">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="58d5a-208">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="58d5a-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="58d5a-209">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="58d5a-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="58d5a-211">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="58d5a-211">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="58d5a-212">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-212">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="58d5a-214">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-214">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="58d5a-216">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="58d5a-216">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="58d5a-218">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="58d5a-218">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="58d5a-220">a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-220">a.</span></span> <span data-ttu-id="58d5a-221">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-221">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="58d5a-222">b.</span><span class="sxs-lookup"><span data-stu-id="58d5a-222">b.</span></span> <span data-ttu-id="58d5a-223">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="58d5a-223">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="58d5a-224">c.</span><span class="sxs-lookup"><span data-stu-id="58d5a-224">c.</span></span> <span data-ttu-id="58d5a-225">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-225">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="58d5a-226">d.</span><span class="sxs-lookup"><span data-stu-id="58d5a-226">d.</span></span> <span data-ttu-id="58d5a-227">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="58d5a-228">Boomi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="58d5a-228">Creating a Boomi test user</span></span>

<span data-ttu-id="58d5a-229">Ahhoz, hogy az Azure AD-felhasználók Boomi bejelentkezni, akkor ki kell építenie a Boomi.</span><span class="sxs-lookup"><span data-stu-id="58d5a-229">In order to enable Azure AD users to log in to Boomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="58d5a-230">Boomi, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="58d5a-230">In the case of Boomi, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="58d5a-231">Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="58d5a-231">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="58d5a-232">Jelentkezzen be rendszergazdaként a Boomi vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="58d5a-232">Log in to your Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="58d5a-233">A bejelentkezés után navigáljon **felhasználókezelés** , és navigáljon **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-233">After logging in, navigate to **User Management** and go to **Users**.</span></span>

    <span data-ttu-id="58d5a-234">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="58d5a-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="58d5a-235">Kattintson a  **+**  ikon és a **Add/karbantartása felhasználói szerepkörök** párbeszédpanel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="58d5a-235">Click **+**  icon and the **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="58d5a-236">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="58d5a-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="58d5a-237">![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="58d5a-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="58d5a-238">a.</span><span class="sxs-lookup"><span data-stu-id="58d5a-238">a.</span></span> <span data-ttu-id="58d5a-239">Az a **felhasználó e-mail címe** szövegmezőben, az e-mailt a felhasználó típusát, például BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="58d5a-239">In the **User e-mail address** textbox, type the email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="58d5a-240">b.</span><span class="sxs-lookup"><span data-stu-id="58d5a-240">b.</span></span> <span data-ttu-id="58d5a-241">Az a **Keresztnév** szövegmezőhöz felhasználói Britta például az első nevét.</span><span class="sxs-lookup"><span data-stu-id="58d5a-241">In the **First name** textbox, type the First name of user like Britta.</span></span>

    <span data-ttu-id="58d5a-242">c.</span><span class="sxs-lookup"><span data-stu-id="58d5a-242">c.</span></span> <span data-ttu-id="58d5a-243">Az a **Vezetéknév** szövegmező, írja be például Simon felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="58d5a-243">In the **Last name** textbox, type the Last name of user like Simon.</span></span>
    
    <span data-ttu-id="58d5a-244">d.</span><span class="sxs-lookup"><span data-stu-id="58d5a-244">d.</span></span> <span data-ttu-id="58d5a-245">Adja meg a felhasználó **összevonási azonosító**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-245">Enter the user's **Federation ID**.</span></span> <span data-ttu-id="58d5a-246">Minden felhasználónak rendelkeznie kell egy összevonási azonosító, amely egyedileg azonosítja a felhasználó a fiókon belül.</span><span class="sxs-lookup"><span data-stu-id="58d5a-246">Each user must have a Federation ID that uniquely identifies the user within the account.</span></span>
    
    <span data-ttu-id="58d5a-247">e.</span><span class="sxs-lookup"><span data-stu-id="58d5a-247">e.</span></span> <span data-ttu-id="58d5a-248">Rendelje hozzá a **általános jogú felhasználó** a felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="58d5a-248">Assign the **Standard User** role to the user.</span></span> <span data-ttu-id="58d5a-249">Nem rendelhető hozzá, a rendszergazda szerepkör mert, amely ad neki normál légkör hozzáférés, valamint az egyszeri bejelentkezéses hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="58d5a-249">Do not assign the Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="58d5a-250">f.</span><span class="sxs-lookup"><span data-stu-id="58d5a-250">f.</span></span> <span data-ttu-id="58d5a-251">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="58d5a-252">A felhasználó nem kap meg az üdvözlő e-mailben értesítést tartalmazó egy jelszót, amely használható a AtomSphere fiók bejelentkezni, mert az identitásszolgáltató kezeli a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="58d5a-252">The user will not receive a welcome notification email containing a password that can be used to log in to the AtomSphere account because his password is managed through the identity provider.</span></span> <span data-ttu-id="58d5a-253">Bármely más Boomi felhasználói fiók létrehozása eszközt, vagy rendelkezés AAD felhasználói fiókokhoz Boomi által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="58d5a-253">You may use any other Boomi user account creation tools or APIs provided by Boomi to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="58d5a-254">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="58d5a-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="58d5a-255">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Boomi Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="58d5a-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Boomi.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="58d5a-257">**Britta Simon hozzárendelése Boomi, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="58d5a-257">**To assign Britta Simon to Boomi, perform the following steps:**</span></span>

1. <span data-ttu-id="58d5a-258">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="58d5a-260">Az alkalmazások listában válassza ki a **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-260">In the applications list, select **Boomi**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="58d5a-262">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="58d5a-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="58d5a-264">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="58d5a-264">Click **Add** button.</span></span> <span data-ttu-id="58d5a-265">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="58d5a-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="58d5a-267">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="58d5a-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="58d5a-268">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="58d5a-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="58d5a-269">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="58d5a-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="58d5a-270">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="58d5a-270">Testing single sign-on</span></span>

<span data-ttu-id="58d5a-271">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="58d5a-271">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="58d5a-272">Ha a hozzáférési panelen Boomi csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Boomi alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="58d5a-272">When you click the Boomi tile in the Access Panel, you should get automatically signed-on to your Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58d5a-273">További források</span><span class="sxs-lookup"><span data-stu-id="58d5a-273">Additional resources</span></span>

* [<span data-ttu-id="58d5a-274">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="58d5a-274">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="58d5a-275">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="58d5a-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

