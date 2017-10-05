---
title: "Oktatóanyag: Azure Active Directoryval integrált BetterWorks |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BetterWorks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: d6a5b167c0befbd0fe2c65bdd16abc35ed0a659c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="12a57-103">Oktatóanyag: Azure Active Directoryval integrált BetterWorks</span><span class="sxs-lookup"><span data-stu-id="12a57-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="12a57-104">Ebben az oktatóanyagban elsajátíthatja BetterWorks integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="12a57-104">In this tutorial, you learn how to integrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="12a57-105">BetterWorks integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="12a57-105">Integrating BetterWorks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="12a57-106">Megadhatja a BetterWorks hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="12a57-106">You can control in Azure AD who has access to BetterWorks</span></span>
- <span data-ttu-id="12a57-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BetterWorks (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="12a57-107">You can enable your users to automatically get signed-on to BetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="12a57-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="12a57-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="12a57-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="12a57-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12a57-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="12a57-110">Prerequisites</span></span>

<span data-ttu-id="12a57-111">Konfigurálása az Azure AD-integrációs BetterWorks, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="12a57-111">To configure Azure AD integration with BetterWorks, you need the following items:</span></span>

- <span data-ttu-id="12a57-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="12a57-112">An Azure AD subscription</span></span>
- <span data-ttu-id="12a57-113">Egy BetterWorks egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="12a57-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="12a57-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="12a57-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="12a57-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="12a57-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="12a57-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="12a57-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="12a57-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="12a57-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="12a57-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="12a57-118">Scenario description</span></span>
<span data-ttu-id="12a57-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="12a57-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="12a57-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="12a57-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="12a57-121">A gyűjteményből BetterWorks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12a57-121">Adding BetterWorks from the gallery</span></span>
2. <span data-ttu-id="12a57-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12a57-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-the-gallery"></a><span data-ttu-id="12a57-123">A gyűjteményből BetterWorks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="12a57-123">Adding BetterWorks from the gallery</span></span>
<span data-ttu-id="12a57-124">Az Azure AD integrálása a BetterWorks konfigurálásához kell hozzáadnia BetterWorks a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="12a57-124">To configure the integration of BetterWorks into Azure AD, you need to add BetterWorks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="12a57-125">**A gyűjteményből BetterWorks hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12a57-125">**To add BetterWorks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="12a57-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12a57-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="12a57-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="12a57-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="12a57-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12a57-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="12a57-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="12a57-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="12a57-133">Írja be a keresőmezőbe, **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="12a57-133">In the search box, type **BetterWorks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="12a57-135">Az eredmények panelen válassza ki a **BetterWorks**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12a57-135">In the results panel, select **BetterWorks**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="12a57-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12a57-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="12a57-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BetterWorks</span><span class="sxs-lookup"><span data-stu-id="12a57-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="12a57-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BetterWorks a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="12a57-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BetterWorks is to a user in Azure AD.</span></span> <span data-ttu-id="12a57-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BetterWorks közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="12a57-140">In other words, a link relationship between an Azure AD user and the related user in BetterWorks needs to be established.</span></span>

<span data-ttu-id="12a57-141">BetterWorks, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="12a57-141">In BetterWorks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="12a57-142">Az Azure AD egyszeri bejelentkezést a BetterWorks tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="12a57-142">To configure and test Azure AD single sign-on with BetterWorks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="12a57-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="12a57-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="12a57-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="12a57-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="12a57-145">**[BetterWorks tesztfelhasználó létrehozása](#creating-a-betterworks-test-user)**  - való Britta Simon valami BetterWorks, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="12a57-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - to have a counterpart of Britta Simon in BetterWorks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="12a57-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12a57-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="12a57-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="12a57-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="12a57-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12a57-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="12a57-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BetterWorks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="12a57-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="12a57-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BetterWorks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12a57-150">**To configure Azure AD single sign-on with BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="12a57-151">Az Azure portálon a a **BetterWorks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="12a57-151">In the Azure portal, on the **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="12a57-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12a57-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="12a57-155">Az a **BetterWorks tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**:</span><span class="sxs-lookup"><span data-stu-id="12a57-155">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="12a57-157">a.</span><span class="sxs-lookup"><span data-stu-id="12a57-157">a.</span></span> <span data-ttu-id="12a57-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="12a57-158">In the **Identifier** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="12a57-159">b.</span><span class="sxs-lookup"><span data-stu-id="12a57-159">b.</span></span> <span data-ttu-id="12a57-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="12a57-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="12a57-161">Az a **BetterWorks tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="12a57-161">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="12a57-163">a.</span><span class="sxs-lookup"><span data-stu-id="12a57-163">a.</span></span> <span data-ttu-id="12a57-164">Kattintson a **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="12a57-164">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="12a57-165">b.</span><span class="sxs-lookup"><span data-stu-id="12a57-165">b.</span></span> <span data-ttu-id="12a57-166">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="12a57-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="12a57-167">Ezek a valódi értékek nem.</span><span class="sxs-lookup"><span data-stu-id="12a57-167">These are not real values.</span></span> <span data-ttu-id="12a57-168">Frissítheti ezeket az értékeket a válasz URL-CÍMEN, a azonosítója és a tényleges bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="12a57-168">Update these values with the Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="12a57-169">Ügyfél [BetterWorks támogatási csoport](mailto:support@betterworks.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="12a57-169">Contact [BetterWorks support team](mailto:support@betterworks.com) to get these values.</span></span>
 
4. <span data-ttu-id="12a57-170">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="12a57-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="12a57-172">BetterWorks alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="12a57-172">BetterWorks application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="12a57-173">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="12a57-173">Configure the following claims for this application.</span></span> <span data-ttu-id="12a57-174">Ezek az attribútumok értékének kezelheti a "**attribútum**" fülre az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="12a57-174">You can manage the values of these attributes from the "**Attribute**" tab of the application.</span></span> <span data-ttu-id="12a57-175">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="12a57-175">The following screenshot shows an example for this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="12a57-177">Az a **SAML-jogkivonat attribútumok** párbeszédpanel, az alábbi táblázatban szereplő minden egyes sorára hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="12a57-177">On the **SAML token attributes** dialog, for each row shown in the table below, perform the following steps:</span></span>
 
   | <span data-ttu-id="12a57-178">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="12a57-178">Attribute Name</span></span> | <span data-ttu-id="12a57-179">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="12a57-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="12a57-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="12a57-180">saml_token</span></span>     | <span data-ttu-id="12a57-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="12a57-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="12a57-182">a.</span><span class="sxs-lookup"><span data-stu-id="12a57-182">a.</span></span> <span data-ttu-id="12a57-183">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12a57-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="12a57-186">b.</span><span class="sxs-lookup"><span data-stu-id="12a57-186">b.</span></span> <span data-ttu-id="12a57-187">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="12a57-187">In the **Name** textbox, type the attribute name shown for that row.</span></span> 

   <span data-ttu-id="12a57-188">c.</span><span class="sxs-lookup"><span data-stu-id="12a57-188">c.</span></span> <span data-ttu-id="12a57-189">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="12a57-189">From the **Value** list, type the attribute value shown for that row.</span></span>
    
   <span data-ttu-id="12a57-190">d.</span><span class="sxs-lookup"><span data-stu-id="12a57-190">d.</span></span> <span data-ttu-id="12a57-191">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="12a57-191">Click **Ok**.</span></span>

7. <span data-ttu-id="12a57-192">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12a57-192">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="12a57-194">Egyszeri bejelentkezés konfigurálása **BetterWorks** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [BetterWorks támogatási csoport](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="12a57-194">To configure single sign-on on **BetterWorks** side, you need to send the downloaded **Metadata XML** to [BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="12a57-195">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="12a57-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="12a57-196">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="12a57-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="12a57-197">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="12a57-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="12a57-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12a57-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="12a57-199">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="12a57-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="12a57-201">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12a57-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="12a57-202">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12a57-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="12a57-204">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="12a57-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="12a57-206">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="12a57-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="12a57-208">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="12a57-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="12a57-210">a.</span><span class="sxs-lookup"><span data-stu-id="12a57-210">a.</span></span> <span data-ttu-id="12a57-211">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="12a57-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="12a57-212">b.</span><span class="sxs-lookup"><span data-stu-id="12a57-212">b.</span></span> <span data-ttu-id="12a57-213">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="12a57-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="12a57-214">c.</span><span class="sxs-lookup"><span data-stu-id="12a57-214">c.</span></span> <span data-ttu-id="12a57-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="12a57-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="12a57-216">d.</span><span class="sxs-lookup"><span data-stu-id="12a57-216">d.</span></span> <span data-ttu-id="12a57-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="12a57-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="12a57-218">BetterWorks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12a57-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="12a57-219">Ebben a szakaszban egy BetterWorks Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="12a57-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="12a57-220">Együttműködve [BetterWorks támogatási csoport](mailto:support@betterworks.com) a felhasználók hozzáadása a BetterWorks platform.</span><span class="sxs-lookup"><span data-stu-id="12a57-220">Work with [BetterWorks support team](mailto:support@betterworks.com) to add the users in the BetterWorks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="12a57-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12a57-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="12a57-222">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BetterWorks Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="12a57-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BetterWorks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="12a57-224">**Britta Simon hozzárendelése BetterWorks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12a57-224">**To assign Britta Simon to BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="12a57-225">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12a57-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="12a57-227">Az alkalmazások listában válassza ki a **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="12a57-227">In the applications list, select **BetterWorks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="12a57-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="12a57-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="12a57-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="12a57-231">Click **Add** button.</span></span> <span data-ttu-id="12a57-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12a57-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="12a57-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="12a57-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="12a57-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12a57-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="12a57-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12a57-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="12a57-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="12a57-237">Testing single sign-on</span></span>

<span data-ttu-id="12a57-238">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="12a57-238">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="12a57-239">Ha a hozzáférési panelen BetterWorks csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az BetterWorks alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="12a57-239">When you click the BetterWorks tile in the Access Panel, you should get automatically signed-on to your BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12a57-240">További források</span><span class="sxs-lookup"><span data-stu-id="12a57-240">Additional resources</span></span>

* [<span data-ttu-id="12a57-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="12a57-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12a57-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="12a57-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

