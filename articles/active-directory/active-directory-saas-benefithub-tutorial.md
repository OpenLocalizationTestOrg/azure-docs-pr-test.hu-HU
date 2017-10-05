---
title: "Oktatóanyag: Azure Active Directoryval integrált BenefitHub |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BenefitHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 8df9c0d8443d6685253207ed1915c780275014fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a><span data-ttu-id="7b5cb-103">Oktatóanyag: Azure Active Directoryval integrált BenefitHub</span><span class="sxs-lookup"><span data-stu-id="7b5cb-103">Tutorial: Azure Active Directory integration with BenefitHub</span></span>

<span data-ttu-id="7b5cb-104">Ebben az oktatóanyagban elsajátíthatja BenefitHub integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7b5cb-104">In this tutorial, you learn how to integrate BenefitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7b5cb-105">BenefitHub integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-105">Integrating BenefitHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7b5cb-106">Megadhatja a BenefitHub hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7b5cb-106">You can control in Azure AD who has access to BenefitHub</span></span>
- <span data-ttu-id="7b5cb-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BenefitHub (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7b5cb-107">You can enable your users to automatically get signed-on to BenefitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7b5cb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7b5cb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7b5cb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7b5cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b5cb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7b5cb-110">Prerequisites</span></span>

<span data-ttu-id="7b5cb-111">Konfigurálása az Azure AD-integrációs BenefitHub, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-111">To configure Azure AD integration with BenefitHub, you need the following items:</span></span>

- <span data-ttu-id="7b5cb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7b5cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7b5cb-113">Egy BenefitHub egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7b5cb-113">A BenefitHub single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7b5cb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7b5cb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7b5cb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7b5cb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7b5cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7b5cb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-118">Scenario description</span></span>
<span data-ttu-id="7b5cb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7b5cb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7b5cb-121">A gyűjteményből BenefitHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-121">Adding BenefitHub from the gallery</span></span>
2. <span data-ttu-id="7b5cb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7b5cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benefithub-from-the-gallery"></a><span data-ttu-id="7b5cb-123">A gyűjteményből BenefitHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-123">Adding BenefitHub from the gallery</span></span>
<span data-ttu-id="7b5cb-124">Az Azure AD integrálása a BenefitHub konfigurálásához kell hozzáadnia BenefitHub a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-124">To configure the integration of BenefitHub into Azure AD, you need to add BenefitHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7b5cb-125">**A gyűjteményből BenefitHub hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7b5cb-125">**To add BenefitHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7b5cb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7b5cb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7b5cb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7b5cb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7b5cb-133">Írja be a keresőmezőbe, **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-133">In the search box, type **BenefitHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_search.png)

5. <span data-ttu-id="7b5cb-135">Az eredmények panelen válassza ki a **BenefitHub**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-135">In the results panel, select **BenefitHub**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7b5cb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7b5cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7b5cb-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BenefitHub</span><span class="sxs-lookup"><span data-stu-id="7b5cb-138">In this section, you configure and test Azure AD single sign-on with BenefitHub based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7b5cb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BenefitHub a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenefitHub is to a user in Azure AD.</span></span> <span data-ttu-id="7b5cb-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BenefitHub közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-140">In other words, a link relationship between an Azure AD user and the related user in BenefitHub needs to be established.</span></span>

<span data-ttu-id="7b5cb-141">BenefitHub, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-141">In BenefitHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7b5cb-142">Az Azure AD egyszeri bejelentkezést a BenefitHub tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-142">To configure and test Azure AD single sign-on with BenefitHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7b5cb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7b5cb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7b5cb-145">**[BenefitHub tesztfelhasználó létrehozása](#creating-a-benefithub-test-user)**  - való Britta Simon valami BenefitHub, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-145">**[Creating a BenefitHub test user](#creating-a-benefithub-test-user)** - to have a counterpart of Britta Simon in BenefitHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7b5cb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7b5cb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7b5cb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7b5cb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BenefitHub alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenefitHub application.</span></span>

<span data-ttu-id="7b5cb-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BenefitHub, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7b5cb-150">**To configure Azure AD single sign-on with BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="7b5cb-151">Az Azure portálon a a **BenefitHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-151">In the Azure portal, on the **BenefitHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7b5cb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. <span data-ttu-id="7b5cb-155">Az a **BenefitHub tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-155">On the **BenefitHub Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_url1.png)
  
    <span data-ttu-id="7b5cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-157">a.</span></span> <span data-ttu-id="7b5cb-158">Az a **azonosító** szövegmező, típus:`urn:benefithub:passport`</span><span class="sxs-lookup"><span data-stu-id="7b5cb-158">In the **Identifier** textbox, type: `urn:benefithub:passport`</span></span>
    
    <span data-ttu-id="7b5cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-159">b.</span></span> <span data-ttu-id="7b5cb-160">Az a **válasz URL-CÍMEN** szövegmező, típus:`https://passport.benefithub.info/saml/post/ac`</span><span class="sxs-lookup"><span data-stu-id="7b5cb-160">In the **Reply URL** textbox, type: `https://passport.benefithub.info/saml/post/ac`</span></span>

4. <span data-ttu-id="7b5cb-161">A BenefitHub alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-161">The BenefitHub application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="7b5cb-162">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-162">Configure the following claims for this application.</span></span> <span data-ttu-id="7b5cb-163">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-163">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_attribute.png)

5. <span data-ttu-id="7b5cb-165">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az előző ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-165">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>
    
    | <span data-ttu-id="7b5cb-166">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="7b5cb-166">Attribute Name</span></span> | <span data-ttu-id="7b5cb-167">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="7b5cb-167">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="7b5cb-168">a szervezeti</span><span class="sxs-lookup"><span data-stu-id="7b5cb-168">organizationid</span></span> | <span data-ttu-id="7b5cb-169">< a szervezeti ></span><span class="sxs-lookup"><span data-stu-id="7b5cb-169">< organizationid ></span></span> |

    > [!NOTE]
    > <span data-ttu-id="7b5cb-170">Az attribútum értéke nem valódi.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-170">This attribute value is not real.</span></span> <span data-ttu-id="7b5cb-171">Frissítse a tényleges a szervezeti ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-171">Update this value with actual organizationid.</span></span> <span data-ttu-id="7b5cb-172">Ügyfél [BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs) tényleges a szervezeti eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-172">Contact [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to get the actual organizationid.</span></span>
    
    <span data-ttu-id="7b5cb-173">a.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-173">a.</span></span> <span data-ttu-id="7b5cb-174">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-174">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="7b5cb-177">b.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-177">b.</span></span> <span data-ttu-id="7b5cb-178">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-178">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="7b5cb-179">c.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-179">c.</span></span> <span data-ttu-id="7b5cb-180">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-180">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="7b5cb-181">d.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-181">d.</span></span> <span data-ttu-id="7b5cb-182">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-182">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7b5cb-183">A SAML-előfeltétel konfigurálása előtt kapcsolatba kell lépnie a [BenefitHub támogatási](https://www.benefithub.com/Home/ContactUs) és az egyedi azonosító attribútum értékének kérhetnek a bérlő.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-183">Before you can configure the SAML assertion, you need to contact your [BenefitHub support](https://www.benefithub.com/Home/ContactUs) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="7b5cb-184">Ezt az értéket az egyéni jogcímleírásokat, az alkalmazás konfigurálása van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-184">You need this value to configure the custom claim for your application.</span></span>

6. <span data-ttu-id="7b5cb-185">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. <span data-ttu-id="7b5cb-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7b5cb-189">Egyszeri bejelentkezés konfigurálása **BenefitHub** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="7b5cb-189">To configure single sign-on on **BenefitHub** side, you need to send the downloaded **Metadata XML** to [BenefitHub support team](https://www.benefithub.com/Home/ContactUs).</span></span> <span data-ttu-id="7b5cb-190">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-190">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="7b5cb-191">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7b5cb-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7b5cb-192">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7b5cb-193">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7b5cb-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7b5cb-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="7b5cb-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7b5cb-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7b5cb-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7b5cb-198">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7b5cb-200">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7b5cb-202">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7b5cb-204">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7b5cb-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benefithub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7b5cb-206">a.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-206">a.</span></span> <span data-ttu-id="7b5cb-207">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7b5cb-208">b.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-208">b.</span></span> <span data-ttu-id="7b5cb-209">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7b5cb-210">c.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-210">c.</span></span> <span data-ttu-id="7b5cb-211">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7b5cb-212">d.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-212">d.</span></span> <span data-ttu-id="7b5cb-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-213">Click **Create**.</span></span>
 
### <a name="creating-a-benefithub-test-user"></a><span data-ttu-id="7b5cb-214">BenefitHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-214">Creating a BenefitHub test user</span></span>

<span data-ttu-id="7b5cb-215">Ebben a szakaszban egy BenefitHub Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-215">In this section, you create a user called Britta Simon in BenefitHub.</span></span> <span data-ttu-id="7b5cb-216">Együttműködve [BenefitHub támogatási csoport](https://www.benefithub.com/Home/ContactUs) a felhasználók hozzáadása a BenefitHub platform.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-216">Work with [BenefitHub support team](https://www.benefithub.com/Home/ContactUs) to add the users in the BenefitHub platform.</span></span> <span data-ttu-id="7b5cb-217">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-217">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7b5cb-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7b5cb-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7b5cb-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BenefitHub Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenefitHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7b5cb-221">**Britta Simon hozzárendelése BenefitHub, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7b5cb-221">**To assign Britta Simon to BenefitHub, perform the following steps:**</span></span>

1. <span data-ttu-id="7b5cb-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7b5cb-224">Az alkalmazások listában válassza ki a **BenefitHub**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-224">In the applications list, select **BenefitHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefithub-tutorial/tutorial_benefithub_app.png) 

3. <span data-ttu-id="7b5cb-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7b5cb-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-228">Click **Add** button.</span></span> <span data-ttu-id="7b5cb-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7b5cb-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7b5cb-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7b5cb-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7b5cb-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7b5cb-234">Testing single sign-on</span></span>

<span data-ttu-id="7b5cb-235">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7b5cb-236">Ha a hozzáférési panelen BenefitHub csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az BenefitHub alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="7b5cb-236">When you click the BenefitHub tile in the Access Panel, you should get automatically signed-on to your BenefitHub application.</span></span>
<span data-ttu-id="7b5cb-237">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="7b5cb-237">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b5cb-238">További források</span><span class="sxs-lookup"><span data-stu-id="7b5cb-238">Additional resources</span></span>

* [<span data-ttu-id="7b5cb-239">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7b5cb-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7b5cb-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7b5cb-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benefithub-tutorial/tutorial_general_203.png

