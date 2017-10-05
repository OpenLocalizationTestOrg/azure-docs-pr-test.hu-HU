---
title: "Oktatóanyag: Azure Active Directoryval integrált Antik további |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a további Antik között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: c2b7638e99fa46ff41a7f2202bdf0e7a2b017c19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a><span data-ttu-id="ac1c8-103">Oktatóanyag: Azure Active Directoryval integrált Antik tudnivalók</span><span class="sxs-lookup"><span data-stu-id="ac1c8-103">Tutorial: Azure Active Directory integration with Blackboard Learn</span></span>

<span data-ttu-id="ac1c8-104">Ebben az oktatóanyagban elsajátíthatja Antik további integrálása Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ac1c8-104">In this tutorial, you learn how to integrate Blackboard Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ac1c8-105">Antik további integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-105">Integrating Blackboard Learn with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ac1c8-106">Szabályozhatja, aki hozzáfér Antik további Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ac1c8-106">You can control in Azure AD who has access to Blackboard Learn</span></span>
- <span data-ttu-id="ac1c8-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Antik további (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="ac1c8-107">You can enable your users to automatically get signed-on to Blackboard Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ac1c8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ac1c8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ac1c8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ac1c8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac1c8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ac1c8-110">Prerequisites</span></span>

<span data-ttu-id="ac1c8-111">Konfigurálása az Azure AD-integrációs Antik ismerje meg, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-111">To configure Azure AD integration with Blackboard Learn, you need the following items:</span></span>

- <span data-ttu-id="ac1c8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ac1c8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ac1c8-113">Egy Antik ismerje meg az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ac1c8-113">A Blackboard Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ac1c8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ac1c8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ac1c8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ac1c8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ac1c8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ac1c8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ac1c8-118">Scenario description</span></span>
<span data-ttu-id="ac1c8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ac1c8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ac1c8-121">Ismerje meg, antik hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ac1c8-121">Adding Blackboard Learn from the gallery</span></span>
2. <span data-ttu-id="ac1c8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ac1c8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn-from-the-gallery"></a><span data-ttu-id="ac1c8-123">Ismerje meg, antik hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ac1c8-123">Adding Blackboard Learn from the gallery</span></span>
<span data-ttu-id="ac1c8-124">Az Azure AD integrálása a Antik további konfigurálásához kell hozzáadnia Antik ismerje meg a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-124">To configure the integration of Blackboard Learn into Azure AD, you need to add Blackboard Learn from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ac1c8-125">**Adja hozzá a Antik ismerje meg a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-125">**To add Blackboard Learn from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ac1c8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ac1c8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ac1c8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ac1c8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ac1c8-133">Írja be a keresőmezőbe, **Antik további**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-133">In the search box, type **Blackboard Learn**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. <span data-ttu-id="ac1c8-135">Az eredmények panelen válassza ki a **Antik további**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-135">In the results panel, select **Blackboard Learn**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ac1c8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ac1c8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ac1c8-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Antik megtudhatja, hogy a "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="ac1c8-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ac1c8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Antik ismerje meg a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Blackboard Learn is to a user in Azure AD.</span></span> <span data-ttu-id="ac1c8-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Antik további közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-140">In other words, a link relationship between an Azure AD user and the related user in Blackboard Learn needs to be established.</span></span>

<span data-ttu-id="ac1c8-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Antik ismerje meg a.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Blackboard Learn.</span></span>

<span data-ttu-id="ac1c8-142">Az Azure AD egyszeri bejelentkezést a Antik megtudhatja, tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-142">To configure and test Azure AD single sign-on with Blackboard Learn, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ac1c8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ac1c8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ac1c8-145">**[Ismerje meg, antik tesztfelhasználó létrehozása](#creating-a-blackboard-learn-test-user)**  - való egy megfelelője a Britta Simon Antik ismerje meg, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-145">**[Creating a Blackboard Learn test user](#creating-a-blackboard-learn-test-user)** - to have a counterpart of Britta Simon in Blackboard Learn that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ac1c8-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ac1c8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ac1c8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac1c8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ac1c8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez Antik ismerje meg az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Blackboard Learn application.</span></span>

<span data-ttu-id="ac1c8-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása Antik ismerje meg, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-150">**To configure Azure AD single sign-on with Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="ac1c8-151">Az Azure portálon a a **Antik további** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-151">In the Azure portal, on the **Blackboard Learn** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ac1c8-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. <span data-ttu-id="ac1c8-155">Az a **Antik megtudhatja, tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-155">On the **Blackboard Learn Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    <span data-ttu-id="ac1c8-157">a.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-157">a.</span></span> <span data-ttu-id="ac1c8-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.blackboard.com/`</span><span class="sxs-lookup"><span data-stu-id="ac1c8-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/`</span></span>

    <span data-ttu-id="ac1c8-159">b.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-159">b.</span></span> <span data-ttu-id="ac1c8-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span><span class="sxs-lookup"><span data-stu-id="ac1c8-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="ac1c8-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-161">These values are not real.</span></span> <span data-ttu-id="ac1c8-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ac1c8-163">Ügyfél [Antik további ügyfél-támogatási csoport](https://www.blackboard.com/support/index.aspx) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-163">Contact [Blackboard Learn Client support team](https://www.blackboard.com/support/index.aspx) to get these values.</span></span> 

4. <span data-ttu-id="ac1c8-164">Antik információk az alkalmazás a SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-164">Blackboard Learn application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="ac1c8-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-165">Configure the following claims for this application.</span></span> <span data-ttu-id="ac1c8-166">Ezek az attribútumok értékének kezelheti a **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-166">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span>
 <span data-ttu-id="ac1c8-167">Az alábbi képernyőfelvételen a rá vonatkozó példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-167">The following screenshot shows an example about it.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="ac1c8-169">A a **felhasználói attribútumok** lap **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútumok, az ábrán látható módon, és hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-169">In the **User Attributes** section on **Single sign-on** dialog, configure SAML token attributes as shown in the image and perform the following steps.</span></span> <span data-ttu-id="ac1c8-170">Hozzárendelt igazolnia a Userprincipalname Itt egyedi felhasználói attribútumként, de a megfelelő értékre, amely egyértelműen azonosítja a szervezet felhasználói és antik további felhasználónév mezője, amely leképezhető is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-170">We have mapped the Userprincipalname as the unique user attribute here but you can map it to the appropriate value, which uniquely distinguishes the user in the organization and that maps to Blackboard Learn username field.</span></span>
           
    | <span data-ttu-id="ac1c8-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="ac1c8-171">Attribute Name</span></span> | <span data-ttu-id="ac1c8-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="ac1c8-172">Attribute Value</span></span> |   
    | ---------------| ----------------|
    | <span data-ttu-id="ac1c8-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span><span class="sxs-lookup"><span data-stu-id="ac1c8-173">urn:oid:1.3.6.1.4.1.5923.1.1.1.6</span></span> |<span data-ttu-id="ac1c8-174">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="ac1c8-174">user.userprincipalname</span></span> |

    <span data-ttu-id="ac1c8-175">a.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-175">a.</span></span> <span data-ttu-id="ac1c8-176">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="ac1c8-179">b.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-179">b.</span></span> <span data-ttu-id="ac1c8-180">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="ac1c8-181">c.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-181">c.</span></span> <span data-ttu-id="ac1c8-182">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="ac1c8-183">d.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-183">d.</span></span> <span data-ttu-id="ac1c8-184">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-184">Click **Ok**.</span></span>

4. <span data-ttu-id="ac1c8-185">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. <span data-ttu-id="ac1c8-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="ac1c8-189">A a **Antik további konfigurációs** kattintson **Antik további konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-189">On the **Blackboard Learn Configuration** section, click **Configure Blackboard Learn** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ac1c8-190">Másolás a **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-190">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. <span data-ttu-id="ac1c8-192">Egyszeri bejelentkezés konfigurálása **Antik további** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** és **SAML Entitásazonosító** való [Antik további támogatási](https://www.blackboard.com/support/index.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac1c8-192">To configure single sign-on on **Blackboard Learn** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID** to [Blackboard Learn support](https://www.blackboard.com/support/index.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="ac1c8-193">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ac1c8-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ac1c8-194">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ac1c8-195">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ac1c8-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ac1c8-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac1c8-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="ac1c8-197">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ac1c8-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ac1c8-200">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ac1c8-202">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ac1c8-204">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ac1c8-206">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ac1c8-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ac1c8-208">a.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-208">a.</span></span> <span data-ttu-id="ac1c8-209">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ac1c8-210">b.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-210">b.</span></span> <span data-ttu-id="ac1c8-211">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-211">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="ac1c8-212">c.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-212">c.</span></span> <span data-ttu-id="ac1c8-213">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ac1c8-214">d.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-214">d.</span></span> <span data-ttu-id="ac1c8-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-215">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn-test-user"></a><span data-ttu-id="ac1c8-216">Ismerje meg, antik tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac1c8-216">Creating a Blackboard Learn test user</span></span>
<span data-ttu-id="ac1c8-217">Ebben a szakaszban Britta Simon nevű Antik ismerje meg a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-217">In this section, you create a user called Britta Simon in Blackboard Learn.</span></span> 

<span data-ttu-id="ac1c8-218">Antik további információ az alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-218">Blackboard Learn application support just in time user provisioning.</span></span> <span data-ttu-id="ac1c8-219">Győződjön meg arról, hogy konfigurálta a jogcímeket a szakaszban leírt módon  **[az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on)**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-219">Make sure that you have configured the claims as described in the section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**</span></span>
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ac1c8-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ac1c8-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ac1c8-221">Ebben a szakaszban engedélyezze Britta Simon nyújtó Antik további Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Blackboard Learn.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ac1c8-223">**Britta Simon hozzárendelése Antik ismerje meg, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ac1c8-223">**To assign Britta Simon to Blackboard Learn, perform the following steps:**</span></span>

1. <span data-ttu-id="ac1c8-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ac1c8-226">Az alkalmazások listában válassza ki a **Antik további**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-226">In the applications list, select **Blackboard Learn**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. <span data-ttu-id="ac1c8-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ac1c8-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-230">Click **Add** button.</span></span> <span data-ttu-id="ac1c8-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ac1c8-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ac1c8-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ac1c8-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ac1c8-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ac1c8-236">Testing single sign-on</span></span>

<span data-ttu-id="ac1c8-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ac1c8-238">Ha a hozzáférési Panel Antik további mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az antik további alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ac1c8-238">When you click the Blackboard Learn tile in the Access Panel, you should get automatically signed-on to your Blackboard Learn application.</span></span> <span data-ttu-id="ac1c8-239">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ac1c8-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ac1c8-240">További források</span><span class="sxs-lookup"><span data-stu-id="ac1c8-240">Additional resources</span></span>

* [<span data-ttu-id="ac1c8-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ac1c8-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ac1c8-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ac1c8-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png

