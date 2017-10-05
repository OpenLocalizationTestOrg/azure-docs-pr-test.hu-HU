---
title: "Oktatóanyag: Azure Active Directoryval integrált CloudPassage |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és CloudPassage között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 094740e20570665e975dec1a591989e411f90c16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="353a4-103">Oktatóanyag: Azure Active Directoryval integrált CloudPassage</span><span class="sxs-lookup"><span data-stu-id="353a4-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="353a4-104">Ebben az oktatóanyagban elsajátíthatja CloudPassage integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="353a4-104">In this tutorial, you learn how to integrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="353a4-105">CloudPassage integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="353a4-105">Integrating CloudPassage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="353a4-106">Megadhatja a CloudPassage hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="353a4-106">You can control in Azure AD who has access to CloudPassage</span></span>
- <span data-ttu-id="353a4-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett CloudPassage (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="353a4-107">You can enable your users to automatically get signed-on to CloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="353a4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="353a4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="353a4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="353a4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="353a4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="353a4-110">Prerequisites</span></span>

<span data-ttu-id="353a4-111">Konfigurálása az Azure AD-integrációs CloudPassage, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="353a4-111">To configure Azure AD integration with CloudPassage, you need the following items:</span></span>

- <span data-ttu-id="353a4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="353a4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="353a4-113">Egy CloudPassage egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="353a4-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="353a4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="353a4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="353a4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="353a4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="353a4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="353a4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="353a4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="353a4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="353a4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="353a4-118">Scenario description</span></span>
<span data-ttu-id="353a4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="353a4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="353a4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="353a4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="353a4-121">A gyűjteményből CloudPassage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="353a4-121">Adding CloudPassage from the gallery</span></span>
2. <span data-ttu-id="353a4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="353a4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-the-gallery"></a><span data-ttu-id="353a4-123">A gyűjteményből CloudPassage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="353a4-123">Adding CloudPassage from the gallery</span></span>
<span data-ttu-id="353a4-124">Az Azure AD integrálása a CloudPassage konfigurálásához kell hozzáadnia CloudPassage a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="353a4-124">To configure the integration of CloudPassage into Azure AD, you need to add CloudPassage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="353a4-125">**A gyűjteményből CloudPassage hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="353a4-125">**To add CloudPassage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="353a4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="353a4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="353a4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="353a4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="353a4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="353a4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="353a4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="353a4-133">Írja be a keresőmezőbe, **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="353a4-133">In the search box, type **CloudPassage**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="353a4-135">Az eredmények panelen válassza ki a **CloudPassage**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="353a4-135">In the results panel, select **CloudPassage**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="353a4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="353a4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="353a4-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján CloudPassage</span><span class="sxs-lookup"><span data-stu-id="353a4-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="353a4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó CloudPassage a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="353a4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CloudPassage is to a user in Azure AD.</span></span> <span data-ttu-id="353a4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a CloudPassage közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="353a4-140">In other words, a link relationship between an Azure AD user and the related user in CloudPassage needs to be established.</span></span>

<span data-ttu-id="353a4-141">CloudPassage, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="353a4-141">In CloudPassage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="353a4-142">Az Azure AD egyszeri bejelentkezést a CloudPassage tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="353a4-142">To configure and test Azure AD single sign-on with CloudPassage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="353a4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="353a4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="353a4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="353a4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="353a4-145">**[CloudPassage tesztfelhasználó létrehozása](#creating-a-cloudpassage-test-user)**  - való Britta Simon valami CloudPassage, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="353a4-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - to have a counterpart of Britta Simon in CloudPassage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="353a4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="353a4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="353a4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="353a4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="353a4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="353a4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="353a4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az CloudPassage alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="353a4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="353a4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés CloudPassage, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="353a4-150">**To configure Azure AD single sign-on with CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="353a4-151">Az Azure portálon a a **CloudPassage** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="353a4-151">In the Azure portal, on the **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="353a4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="353a4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="353a4-155">Az a **CloudPassage tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="353a4-155">On the **CloudPassage Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="353a4-157">a.</span><span class="sxs-lookup"><span data-stu-id="353a4-157">a.</span></span> <span data-ttu-id="353a4-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="353a4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="353a4-159">b.</span><span class="sxs-lookup"><span data-stu-id="353a4-159">b.</span></span> <span data-ttu-id="353a4-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-cím: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="353a4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="353a4-161">Kaphat a érték ehhez az attribútumhoz kattintva **egyszeri bejelentkezés beállítása dokumentáció** a a **egyszeri bejelentkezési beállítások** a CloudPassage portál szakaszban.</span><span class="sxs-lookup"><span data-stu-id="353a4-161">You can get your value for this attribute by clicking **SSO Setup documentation** in the **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="353a4-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="353a4-163">These values are not real.</span></span> <span data-ttu-id="353a4-164">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN, és a bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="353a4-164">Update these values with the actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="353a4-165">Ügyfél [CloudPassage ügyfél-támogatási csoport](https://www.cloudpassage.com/company/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="353a4-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="353a4-166">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="353a4-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="353a4-168">A CloudPassage alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="353a4-168">Your CloudPassage application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="353a4-169">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="353a4-169">The following screenshot shows an example for this.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="353a4-171">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="353a4-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>

    | <span data-ttu-id="353a4-172">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="353a4-172">Attribute Name</span></span> | <span data-ttu-id="353a4-173">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="353a4-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="353a4-174">Utónév</span><span class="sxs-lookup"><span data-stu-id="353a4-174">firstname</span></span> |<span data-ttu-id="353a4-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="353a4-175">user.givenname</span></span> |
    | <span data-ttu-id="353a4-176">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="353a4-176">lastname</span></span> |<span data-ttu-id="353a4-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="353a4-177">user.surname</span></span> |
    | <span data-ttu-id="353a4-178">E-mailek</span><span class="sxs-lookup"><span data-stu-id="353a4-178">email</span></span> |<span data-ttu-id="353a4-179">User.mail</span><span class="sxs-lookup"><span data-stu-id="353a4-179">user.mail</span></span> |
    
    <span data-ttu-id="353a4-180">a.</span><span class="sxs-lookup"><span data-stu-id="353a4-180">a.</span></span> <span data-ttu-id="353a4-181">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="353a4-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="353a4-184">b.</span><span class="sxs-lookup"><span data-stu-id="353a4-184">b.</span></span> <span data-ttu-id="353a4-185">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="353a4-185">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="353a4-186">c.</span><span class="sxs-lookup"><span data-stu-id="353a4-186">c.</span></span> <span data-ttu-id="353a4-187">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="353a4-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="353a4-188">d.</span><span class="sxs-lookup"><span data-stu-id="353a4-188">d.</span></span> <span data-ttu-id="353a4-189">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-189">Click **Ok**.</span></span>

7. <span data-ttu-id="353a4-190">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-190">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="353a4-192">A a **CloudPassage konfigurációs** kattintson **konfigurálása CloudPassage** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="353a4-192">On the **CloudPassage Configuration** section, click **Configure CloudPassage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="353a4-193">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="353a4-193">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="353a4-195">Egy másik böngészőablakban bejelentkezés webhelyekhez CloudPassage vállalati rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="353a4-195">In a different browser window, sign-on to your CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="353a4-196">Kattintson a felső menüben **beállítások**, és kattintson a **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="353a4-196">In the menu on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][12]

11. <span data-ttu-id="353a4-198">Kattintson a **hitelesítési beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="353a4-198">Click the **Authentication Settings** tab.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][13]

12. <span data-ttu-id="353a4-200">Az a **egyszeri bejelentkezési beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="353a4-200">In the **Single Sign-on Settings** section, perform the following steps:</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása][14]

    <span data-ttu-id="353a4-202">a.</span><span class="sxs-lookup"><span data-stu-id="353a4-202">a.</span></span> <span data-ttu-id="353a4-203">Válassza ki **engedélyezése egyetlen sign-on(SSO) (egyszeri bejelentkezés beállítása dokumentáció)** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="353a4-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="353a4-204">b.</span><span class="sxs-lookup"><span data-stu-id="353a4-204">b.</span></span> <span data-ttu-id="353a4-205">Beillesztés **SAML Entitásazonosító** azokat a **SAML kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="353a4-205">Paste **SAML Entity ID** into the **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="353a4-206">c.</span><span class="sxs-lookup"><span data-stu-id="353a4-206">c.</span></span> <span data-ttu-id="353a4-207">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** azokat a **SAML végponti URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="353a4-207">Paste **SAML Single Sign-On Service URL** into the **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="353a4-208">d.</span><span class="sxs-lookup"><span data-stu-id="353a4-208">d.</span></span> <span data-ttu-id="353a4-209">Beillesztés **Sign-Out URL-cím** azokat a **kijelentkezési kezdőlapja** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="353a4-209">Paste **Sign-Out URL** into the **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="353a4-210">e.</span><span class="sxs-lookup"><span data-stu-id="353a4-210">e.</span></span> <span data-ttu-id="353a4-211">A letöltött tanúsítvány megnyitása a Jegyzettömbben, a letöltött tanúsítvány tartalmának másolása a vágólapra és illessze be azt a **x 509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="353a4-211">Open your downloaded certificate in notepad, copy the content of downloaded certificate into your clipboard, and then paste it into the **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="353a4-212">f.</span><span class="sxs-lookup"><span data-stu-id="353a4-212">f.</span></span> <span data-ttu-id="353a4-213">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="353a4-214">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="353a4-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="353a4-215">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="353a4-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="353a4-216">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="353a4-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="353a4-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="353a4-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="353a4-218">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="353a4-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="353a4-220">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="353a4-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="353a4-221">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="353a4-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="353a4-223">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="353a4-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="353a4-225">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="353a4-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="353a4-227">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="353a4-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="353a4-229">a.</span><span class="sxs-lookup"><span data-stu-id="353a4-229">a.</span></span> <span data-ttu-id="353a4-230">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="353a4-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="353a4-231">b.</span><span class="sxs-lookup"><span data-stu-id="353a4-231">b.</span></span> <span data-ttu-id="353a4-232">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="353a4-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="353a4-233">c.</span><span class="sxs-lookup"><span data-stu-id="353a4-233">c.</span></span> <span data-ttu-id="353a4-234">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="353a4-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="353a4-235">d.</span><span class="sxs-lookup"><span data-stu-id="353a4-235">d.</span></span> <span data-ttu-id="353a4-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="353a4-237">CloudPassage tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="353a4-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="353a4-238">Ez a szakasz célja CloudPassage Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="353a4-238">The objective of this section is to create a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="353a4-239">**A felhasználó Britta Simon meghívta CloudPassage létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="353a4-239">**To create a user called Britta Simon in CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="353a4-240">Bejelentkezés a **CloudPassage** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="353a4-240">Sign-on to your **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="353a4-241">A felső eszköztáron kattintson **beállítások**, és kattintson a **helyfelügyelet**.</span><span class="sxs-lookup"><span data-stu-id="353a4-241">In the toolbar on the top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][22] 

3. <span data-ttu-id="353a4-243">Kattintson a **felhasználók** fülre, majd **új felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="353a4-243">Click the **Users** tab, and then click **Add New User**.</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][23]

4. <span data-ttu-id="353a4-245">Az a **új felhasználó hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="353a4-245">In the **Add New User** section, perform the following steps:</span></span> 
   
   ![CloudPassage tesztfelhasználó létrehozása][24]
    
    <span data-ttu-id="353a4-247">a.</span><span class="sxs-lookup"><span data-stu-id="353a4-247">a.</span></span> <span data-ttu-id="353a4-248">Az a **Keresztnév** szövegmező, írja be a Britta.</span><span class="sxs-lookup"><span data-stu-id="353a4-248">In the **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="353a4-249">b.</span><span class="sxs-lookup"><span data-stu-id="353a4-249">b.</span></span> <span data-ttu-id="353a4-250">Az a **Vezetéknév** szövegmezőhöz Simon írja be.</span><span class="sxs-lookup"><span data-stu-id="353a4-250">In the **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="353a4-251">c.</span><span class="sxs-lookup"><span data-stu-id="353a4-251">c.</span></span> <span data-ttu-id="353a4-252">A a **felhasználónév** szövegmezőhöz a **E-mail** szövegmező és a **írja be újra E-mail** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="353a4-252">In the **Username** textbox, the **Email** textbox and the **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="353a4-253">d.</span><span class="sxs-lookup"><span data-stu-id="353a4-253">d.</span></span> <span data-ttu-id="353a4-254">Mint **hozzáférési típus**, jelölje be **Halo Portal hozzáférés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="353a4-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="353a4-255">e.</span><span class="sxs-lookup"><span data-stu-id="353a4-255">e.</span></span> <span data-ttu-id="353a4-256">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="353a4-256">Click **Add**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="353a4-257">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="353a4-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="353a4-258">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés CloudPassage Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="353a4-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CloudPassage.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="353a4-260">**Britta Simon hozzárendelése CloudPassage, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="353a4-260">**To assign Britta Simon to CloudPassage, perform the following steps:**</span></span>

1. <span data-ttu-id="353a4-261">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="353a4-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="353a4-263">Az alkalmazások listában válassza ki a **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="353a4-263">In the applications list, select **CloudPassage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="353a4-265">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="353a4-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="353a4-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="353a4-267">Click **Add** button.</span></span> <span data-ttu-id="353a4-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="353a4-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="353a4-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="353a4-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="353a4-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="353a4-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="353a4-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="353a4-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="353a4-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="353a4-273">Testing single sign-on</span></span>

<span data-ttu-id="353a4-274">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="353a4-274">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="353a4-275">Ha a hozzáférési panelen CloudPassage csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az CloudPassage alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="353a4-275">When you click the CloudPassage tile in the Access Panel, you should get automatically signed-on to your CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="353a4-276">További források</span><span class="sxs-lookup"><span data-stu-id="353a4-276">Additional resources</span></span>

* [<span data-ttu-id="353a4-277">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="353a4-277">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="353a4-278">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="353a4-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

