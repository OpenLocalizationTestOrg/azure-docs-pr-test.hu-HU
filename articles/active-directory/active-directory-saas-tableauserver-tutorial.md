---
title: "Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Tableau kiszolgáló között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 6b35609d88fbbf649e15863901d521886db2a4d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="9ca2b-103">Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="9ca2b-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="9ca2b-104">Ebben az oktatóanyagban elsajátíthatja Tableau kiszolgáló integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-104">In this tutorial, you learn how to integrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ca2b-105">Tableau kiszolgáló integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-105">Integrating Tableau Server with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9ca2b-106">Megadhatja a Tableau-kiszolgálóhoz hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9ca2b-106">You can control in Azure AD who has access to Tableau Server</span></span>
- <span data-ttu-id="9ca2b-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Tableau kiszolgálóra (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9ca2b-107">You can enable your users to automatically get signed-on to Tableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ca2b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9ca2b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9ca2b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ca2b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ca2b-110">Prerequisites</span></span>

<span data-ttu-id="9ca2b-111">Az Azure AD-integráció konfigurálása a Tableau kiszolgálóval, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-111">To configure Azure AD integration with Tableau Server, you need the following items:</span></span>

- <span data-ttu-id="9ca2b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9ca2b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ca2b-113">A Tableau Server egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9ca2b-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ca2b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ca2b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ca2b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ca2b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ca2b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9ca2b-118">Scenario description</span></span>
<span data-ttu-id="9ca2b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ca2b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ca2b-121">Tableau kiszolgáló hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9ca2b-121">Adding Tableau Server from the gallery</span></span>
2. <span data-ttu-id="9ca2b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ca2b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-the-gallery"></a><span data-ttu-id="9ca2b-123">Tableau kiszolgáló hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9ca2b-123">Adding Tableau Server from the gallery</span></span>
<span data-ttu-id="9ca2b-124">Az Azure AD integrálása a Tableau kiszolgáló konfigurálásához szüksége Tableau kiszolgáló hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-124">To configure the integration of Tableau Server into Azure AD, you need to add Tableau Server from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9ca2b-125">**Tableau kiszolgáló hozzáadása a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ca2b-125">**To add Tableau Server from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9ca2b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9ca2b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9ca2b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9ca2b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9ca2b-133">Írja be a keresőmezőbe, **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-133">In the search box, type **Tableau Server**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="9ca2b-135">Az eredmények panelen válassza ki a **Tableau Server**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-135">In the results panel, select **Tableau Server**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9ca2b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ca2b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9ca2b-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Tableau kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="9ca2b-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9ca2b-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Tableau Server Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Server is to a user in Azure AD.</span></span> <span data-ttu-id="9ca2b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Tableau Server közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Server needs to be established.</span></span>

<span data-ttu-id="9ca2b-141">A Tableau kiszolgáló rendelje értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-141">In Tableau Server, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9ca2b-142">Az Azure AD az egyszeri bejelentkezés Tableau kiszolgálóval tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-142">To configure and test Azure AD single sign-on with Tableau Server, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9ca2b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9ca2b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ca2b-145">**[A Tableau Server tesztfelhasználó létrehozása](#creating-a-tableau-server-test-user)**  - való Britta Simon egy megfelelője a Tableau kiszolgáló, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - to have a counterpart of Britta Simon in Tableau Server that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ca2b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ca2b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9ca2b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ca2b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9ca2b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Tableau kiszolgálóalkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="9ca2b-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Tableau kiszolgálóval, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ca2b-150">**To configure Azure AD single sign-on with Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="9ca2b-151">Az Azure portálon a a **Tableau Server** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-151">In the Azure portal, on the **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9ca2b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="9ca2b-155">Az a **Tableau kiszolgáló tartományával és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-155">On the **Tableau Server Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="9ca2b-157">a.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-157">a.</span></span> <span data-ttu-id="9ca2b-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="9ca2b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="9ca2b-159">b.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-159">b.</span></span> <span data-ttu-id="9ca2b-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="9ca2b-160">In the **Identifier** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="9ca2b-161">c.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-161">c.</span></span> <span data-ttu-id="9ca2b-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="9ca2b-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="9ca2b-163">A fenti értékek nem valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-163">The preceding values are not real values.</span></span> <span data-ttu-id="9ca2b-164">Később akkor módosítsa a tényleges URL-cím és a Tableau kiszolgáló konfigurációs lapon azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-164">Later, you update the values with the actual URL and identifier from the Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="9ca2b-165">Tableau kiszolgálóalkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-165">Tableau Server application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="9ca2b-166">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-166">Configure the following claims for this application.</span></span> <span data-ttu-id="9ca2b-167">Ezek az attribútumok értékének kezelheti a **"Felhasználói attribútumok"** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-167">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="9ca2b-168">Az alábbi képernyőfelvételen látható egy példa az azonos.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-168">The following screenshot shows an example for the same.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="9ca2b-170">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="9ca2b-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="9ca2b-171">Attribute Name</span></span> | <span data-ttu-id="9ca2b-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="9ca2b-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="9ca2b-173">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="9ca2b-173">username</span></span> | <span data-ttu-id="9ca2b-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="9ca2b-174">*user.displayname*</span></span> |

    <span data-ttu-id="9ca2b-175">a.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-175">a.</span></span> <span data-ttu-id="9ca2b-176">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="9ca2b-179">b.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-179">b.</span></span> <span data-ttu-id="9ca2b-180">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="9ca2b-181">c.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-181">c.</span></span> <span data-ttu-id="9ca2b-182">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="9ca2b-183">d.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-183">d.</span></span> <span data-ttu-id="9ca2b-184">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="9ca2b-184">Click **Ok**</span></span>


6. <span data-ttu-id="9ca2b-185">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="9ca2b-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-187">Click **Save** button.</span></span>

    <span data-ttu-id="9ca2b-188">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="9ca2b-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="9ca2b-189">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, meg kell bejelentkezés a Tableau Server bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-189">To get SSO configured for your application, you need to sign-on to your Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="9ca2b-190">a.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-190">a.</span></span> <span data-ttu-id="9ca2b-191">A Tableau kiszolgálókonfiguráció lapon kattintson a **SAML** fülre.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-191">In the Tableau Server configuration, click the **SAML** tab.</span></span>
  
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="9ca2b-193">b.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-193">b.</span></span> <span data-ttu-id="9ca2b-194">A jelölőnégyzet bejelölésével **az egyszeri bejelentkezéshez használható SAML**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-194">Select the checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="9ca2b-195">c.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-195">c.</span></span> <span data-ttu-id="9ca2b-196">Tableau kiszolgálói válasz URL-cím – az URL-címet, amely a Tableau Server felhasználók hozzáférhetnek, például a http://tableau_server.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-196">Tableau Server return URL—The URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="9ca2b-197">Http://localhost használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="9ca2b-198">Egy URL-címet a záró perjelet (például http://tableau_server/) használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="9ca2b-199">Másolás **Tableau kiszolgálói válasz URL-cím** és illessze be az Azure AD **URL-cím bejelentkezési** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-199">Copy **Tableau Server return URL** and paste it to Azure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="9ca2b-200">d.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-200">d.</span></span> <span data-ttu-id="9ca2b-201">SAML Entitásazonosító – az entitás azonosítója egyedileg azonosítja a kiállító terjesztési hely a Tableau Server telepítése.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-201">SAML entity ID—The entity ID uniquely identifies your Tableau Server installation to the IdP.</span></span> <span data-ttu-id="9ca2b-202">Adhatja meg a Tableau URL-címe újra ide, ha szeretné, de nem rendelkezik a Tableau kiszolgáló URL-címét.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-202">You can enter your Tableau Server URL again here, if you like, but it does not have to be your Tableau Server URL.</span></span> <span data-ttu-id="9ca2b-203">Másolás **SAML Entitásazonosító** és illessze be az Azure AD **azonosító** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-203">Copy **SAML entity ID** and paste it to Azure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="9ca2b-204">e.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-204">e.</span></span> <span data-ttu-id="9ca2b-205">Kattintson a **metaadat-fájl exportálása** , majd nyissa meg a szöveg szerkesztő alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-205">Click the **Export Metadata File** and open it in the text editor application.</span></span> <span data-ttu-id="9ca2b-206">Keresse meg a helyességi feltétel fogyasztói szolgáltatás URL-cím elé Http Post és Index 0, és másolja az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy the URL.</span></span> <span data-ttu-id="9ca2b-207">Most illessze be az Azure AD **válasz URL-CÍMEN** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-207">Now paste it to Azure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="9ca2b-208">f.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-208">f.</span></span> <span data-ttu-id="9ca2b-209">Keresse meg az összevonási metaadatok fájlt az Azure portálról letöltött, majd töltse fel azt a **SAML Idp metaadatfájl**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in the **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="9ca2b-210">g.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-210">g.</span></span> <span data-ttu-id="9ca2b-211">Kattintson a **OK** gomb a Tableau kiszolgálókonfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-211">Click the **OK** button in the Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="9ca2b-212">Ügyfél töltse fel a tanúsítványt a Tableau kiszolgáló SAML SSO konfigurációban kell, és figyelmen beolvasása az SSO folyamatában.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-212">Customer have to upload any certificate in the Tableau Server SAML SSO configuration and it will get ignored in the SSO flow.</span></span>
    ><span data-ttu-id="9ca2b-213">Ha később kell súgó a SAML konfigurálása a Tableau kiszolgálón, majd tekintse meg a cikk [SAML konfigurálása](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-213">If you need help configuring SAML on Tableau Server then please refer to this article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="9ca2b-214">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9ca2b-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9ca2b-215">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9ca2b-216">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ca2b-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9ca2b-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ca2b-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="9ca2b-218">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9ca2b-220">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ca2b-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9ca2b-221">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9ca2b-223">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ca2b-225">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ca2b-227">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9ca2b-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ca2b-229">a.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-229">a.</span></span> <span data-ttu-id="9ca2b-230">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ca2b-231">b.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-231">b.</span></span> <span data-ttu-id="9ca2b-232">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ca2b-233">c.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-233">c.</span></span> <span data-ttu-id="9ca2b-234">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9ca2b-235">d.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-235">d.</span></span> <span data-ttu-id="9ca2b-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="9ca2b-237">A Tableau Server tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ca2b-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="9ca2b-238">Ez a szakasz célja a Tableau Server Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-238">The objective of this section is to create a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="9ca2b-239">Kell kiépíteni a Tableau kiszolgáló összes felhasználóját.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-239">You need to provision all the users in the Tableau server.</span></span> 

<span data-ttu-id="9ca2b-240">A felhasználó a felhasználónév meg kell felelnie a állított be az Azure AD egyéni attribútum értéke **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-240">That username of the user should match the value which you have configured in the Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="9ca2b-241">A megfelelő leképezéssel az integráció kell működnie [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-241">With the correct mapping the integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="9ca2b-242">Hozza létre a felhasználó manuálisan kell, ha a rendszergazdától Tableau a szervezet szeretné.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-242">If you need to create a user manually, you need to contact the Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9ca2b-243">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9ca2b-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9ca2b-244">Ebben a szakaszban Britta Simon hozzáférést biztosít a Tableau kiszolgáló által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Server.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9ca2b-246">**Britta Simon hozzárendelése Tableau Server, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="9ca2b-246">**To assign Britta Simon to Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="9ca2b-247">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9ca2b-249">Az alkalmazások listában válassza ki a **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-249">In the applications list, select **Tableau Server**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="9ca2b-251">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9ca2b-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-253">Click **Add** button.</span></span> <span data-ttu-id="9ca2b-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9ca2b-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9ca2b-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ca2b-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9ca2b-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9ca2b-259">Testing single sign-on</span></span>

<span data-ttu-id="9ca2b-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9ca2b-261">Ha a hozzáférési Panel Tableau Server mozaik gombra kattint, meg kell beolvasása automatikusan bejelentkezett a Tableau Server alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9ca2b-261">When you click the Tableau Server tile in the Access Panel, you should get automatically signed-on to your Tableau Server application.</span></span>
<span data-ttu-id="9ca2b-262">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="9ca2b-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9ca2b-263">További források</span><span class="sxs-lookup"><span data-stu-id="9ca2b-263">Additional resources</span></span>

* [<span data-ttu-id="9ca2b-264">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9ca2b-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ca2b-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9ca2b-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

