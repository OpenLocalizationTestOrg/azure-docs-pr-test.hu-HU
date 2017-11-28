---
title: "Oktatóanyag: Azure Active Directoryval integrált ADP Globalview |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ADP Globalview között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: e9a5e65c484dfb98d1a7bc63d55f6ef92039554b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a><span data-ttu-id="2c464-103">Oktatóanyag: Azure Active Directoryval integrált ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="2c464-103">Tutorial: Azure Active Directory integration with ADP Globalview</span></span>

<span data-ttu-id="2c464-104">Ebben az oktatóanyagban elsajátíthatja ADP Globalview integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c464-104">In this tutorial, you learn how to integrate ADP Globalview with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c464-105">ADP Globalview integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2c464-105">Integrating ADP Globalview with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2c464-106">Megadhatja a ADP Globalview hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2c464-106">You can control in Azure AD who has access to ADP Globalview</span></span>
- <span data-ttu-id="2c464-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a ADP Globalview (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="2c464-107">You can enable your users to automatically get signed-on to ADP Globalview (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c464-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2c464-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2c464-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c464-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c464-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2c464-110">Prerequisites</span></span>

<span data-ttu-id="2c464-111">Az Azure AD-integrációs ADP Globalview konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="2c464-111">To configure Azure AD integration with ADP Globalview, you need the following items:</span></span>

- <span data-ttu-id="2c464-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2c464-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c464-113">Egy ADP Globalview egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2c464-113">An ADP Globalview single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2c464-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="2c464-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2c464-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="2c464-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c464-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2c464-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2c464-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c464-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c464-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2c464-118">Scenario description</span></span>
<span data-ttu-id="2c464-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2c464-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2c464-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2c464-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c464-121">A gyűjteményből ADP Globalview hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2c464-121">Adding ADP Globalview from the gallery</span></span>
2. <span data-ttu-id="2c464-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2c464-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-globalview-from-the-gallery"></a><span data-ttu-id="2c464-123">A gyűjteményből ADP Globalview hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2c464-123">Adding ADP Globalview from the gallery</span></span>
<span data-ttu-id="2c464-124">Az Azure AD integrálása a ADP Globalview konfigurálásához kell hozzáadnia ADP Globalview a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="2c464-124">To configure the integration of ADP Globalview into Azure AD, you need to add ADP Globalview from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2c464-125">**A gyűjteményből ADP Globalview hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2c464-125">**To add ADP Globalview from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2c464-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2c464-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2c464-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2c464-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2c464-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2c464-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2c464-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="2c464-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2c464-133">Írja be a keresőmezőbe, **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="2c464-133">In the search box, type **ADP Globalview**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. <span data-ttu-id="2c464-135">Az eredmények panelen válassza ki a **ADP Globalview**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2c464-135">In the results panel, select **ADP Globalview**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c464-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2c464-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c464-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a ADP Globalview "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="2c464-138">In this section, you configure and test Azure AD single sign-on with ADP Globalview based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2c464-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ADP Globalview a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="2c464-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ADP Globalview is to a user in Azure AD.</span></span> <span data-ttu-id="2c464-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ADP Globalview közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2c464-140">In other words, a link relationship between an Azure AD user and the related user in ADP Globalview needs to be established.</span></span>

<span data-ttu-id="2c464-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** ADP Globalview a.</span><span class="sxs-lookup"><span data-stu-id="2c464-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ADP Globalview.</span></span>

<span data-ttu-id="2c464-142">Az Azure AD egyszeri bejelentkezést a ADP Globalview tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="2c464-142">To configure and test Azure AD single sign-on with ADP Globalview, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2c464-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="2c464-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2c464-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="2c464-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c464-145">**[Egy ADP Globalview tesztfelhasználó létrehozása](#creating-an-adp-globalview-test-user)**  - való egy megfelelője a Britta Simon ADP Globalview, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="2c464-145">**[Creating an ADP Globalview test user](#creating-an-adp-globalview-test-user)** - to have a counterpart of Britta Simon in ADP Globalview that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2c464-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2c464-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c464-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2c464-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c464-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c464-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c464-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ADP Globalview alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c464-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ADP Globalview application.</span></span>

<span data-ttu-id="2c464-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés ADP Globalview, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2c464-150">**To configure Azure AD single sign-on with ADP Globalview, perform the following steps:**</span></span>

1. <span data-ttu-id="2c464-151">Az Azure portálon a a **ADP Globalview** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2c464-151">In the Azure portal, on the **ADP Globalview** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2c464-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2c464-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. <span data-ttu-id="2c464-155">Az a **ADP Globalview tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2c464-155">On the **ADP Globalview Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     <span data-ttu-id="2c464-157">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-cím: `https://<subdomain>.globalview.adp.com/federate` vagy`https://<subdomain>.globalview.adp.com/federate2`</span><span class="sxs-lookup"><span data-stu-id="2c464-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.globalview.adp.com/federate` or `https://<subdomain>.globalview.adp.com/federate2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2c464-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="2c464-158">The value is not real.</span></span> <span data-ttu-id="2c464-159">Frissítse az értéket a tényleges azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="2c464-159">Update the value with the actual Identifier.</span></span> <span data-ttu-id="2c464-160">Ügyfél [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="2c464-160">Contact [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) to get the value.</span></span>
 
4. <span data-ttu-id="2c464-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2c464-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. <span data-ttu-id="2c464-163">A ADP GlobalView alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="2c464-163">The ADP GlobalView application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> 

6. <span data-ttu-id="2c464-164">Az alábbi képernyőfelvételen egy példát mutat be, amíg.</span><span class="sxs-lookup"><span data-stu-id="2c464-164">The following screenshot shows an example for it.</span></span> <span data-ttu-id="2c464-165">A jogcím neve mindig lehet **"PersonImmutableID"** és az érték, amelynek jelenleg van leképezve ExtensionAttribute2, amely tartalmazza az EmployeeID felhasználó.</span><span class="sxs-lookup"><span data-stu-id="2c464-165">The claim names always be **"PersonImmutableID"** and the value of which we have mapped to ExtensionAttribute2, which contains the EmployeeID of the user.</span></span> <span data-ttu-id="2c464-166">Itt az EmployeeID állítva az Azure AD ADP GlobalView történik, de is képezi le egy másik értéket is alapján az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="2c464-166">Here the user mapping from Azure AD to ADP GlobalView is done on the EmployeeID but you can map it to a different value also based on your application settings.</span></span> <span data-ttu-id="2c464-167">Használhatja a ADP GlobalView csapatától először használja a helyes azonosító egy olyan felhasználó, és képezze le az értéket a **"PersonImmutableID"** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="2c464-167">You can work with the ADP GlobalView team first to use the correct identifier of a user and map that value with the **"PersonImmutableID"** claim.</span></span> <span data-ttu-id="2c464-168">Az e-mailek és a UserID jogcím a képen látható módon is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="2c464-168">You can also map the Email and UserID claim as shown in the picture.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. <span data-ttu-id="2c464-170">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2c464-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="2c464-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="2c464-171">Attribute Name</span></span> | <span data-ttu-id="2c464-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="2c464-172">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="2c464-173">personalimmutableid</span><span class="sxs-lookup"><span data-stu-id="2c464-173">personalimmutableid</span></span> | <span data-ttu-id="2c464-174">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="2c464-174">user.extensionattribute2</span></span> |
    | <span data-ttu-id="2c464-175">E-mailek</span><span class="sxs-lookup"><span data-stu-id="2c464-175">email</span></span>               | <span data-ttu-id="2c464-176">User.mail</span><span class="sxs-lookup"><span data-stu-id="2c464-176">user.mail</span></span> |
    | <span data-ttu-id="2c464-177">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="2c464-177">userid</span></span>              | <span data-ttu-id="2c464-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2c464-178">user.userprincipalname</span></span>|
    
    <span data-ttu-id="2c464-179">a.</span><span class="sxs-lookup"><span data-stu-id="2c464-179">a.</span></span> <span data-ttu-id="2c464-180">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2c464-180">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="2c464-183">b.</span><span class="sxs-lookup"><span data-stu-id="2c464-183">b.</span></span> <span data-ttu-id="2c464-184">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="2c464-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="2c464-185">c.</span><span class="sxs-lookup"><span data-stu-id="2c464-185">c.</span></span> <span data-ttu-id="2c464-186">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="2c464-186">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2c464-187">d.</span><span class="sxs-lookup"><span data-stu-id="2c464-187">d.</span></span> <span data-ttu-id="2c464-188">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c464-188">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2c464-189">A SAML-előfeltétel konfigurálása előtt kapcsolatba kell lépnie a [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) és az egyedi azonosító attribútum értékének kérhetnek a bérlő.</span><span class="sxs-lookup"><span data-stu-id="2c464-189">Before you can configure the SAML assertion, you need to contact your [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="2c464-190">Ezt az értéket az egyéni jogcímleírásokat, az alkalmazás konfigurálása van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2c464-190">You need this value to configure the custom claim for your application.</span></span> 

8. <span data-ttu-id="2c464-191">A a **ADP Globalview konfigurációs** kattintson **konfigurálása ADP Globalview** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2c464-191">On the **ADP Globalview Configuration** section, click **Configure ADP Globalview** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2c464-192">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2c464-192">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. <span data-ttu-id="2c464-194">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c464-194">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="2c464-196">Egyszeri bejelentkezés konfigurálása **ADP Globalview** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c464-196">To configure single sign-on on **ADP Globalview** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="2c464-197">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="2c464-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2c464-198">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="2c464-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2c464-199">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2c464-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c464-200">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c464-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c464-201">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="2c464-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2c464-203">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2c464-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2c464-204">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2c464-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="2c464-206">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2c464-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c464-208">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="2c464-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c464-210">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="2c464-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2c464-212">a.</span><span class="sxs-lookup"><span data-stu-id="2c464-212">a.</span></span> <span data-ttu-id="2c464-213">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c464-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c464-214">b.</span><span class="sxs-lookup"><span data-stu-id="2c464-214">b.</span></span> <span data-ttu-id="2c464-215">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c464-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2c464-216">c.</span><span class="sxs-lookup"><span data-stu-id="2c464-216">c.</span></span> <span data-ttu-id="2c464-217">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2c464-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2c464-218">d.</span><span class="sxs-lookup"><span data-stu-id="2c464-218">d.</span></span> <span data-ttu-id="2c464-219">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2c464-219">Click **Create**.</span></span>
 
### <a name="creating-an-adp-globalview-test-user"></a><span data-ttu-id="2c464-220">Egy ADP Globalview tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c464-220">Creating an ADP Globalview test user</span></span>

<span data-ttu-id="2c464-221">Ez a szakasz célja ADP GlobalView Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2c464-221">The objective of this section is to create a user called Britta Simon in ADP GlobalView.</span></span> <span data-ttu-id="2c464-222">Együttműködve [ADP Globalview támogatási](https://www.adp.com/contact-us/overview.aspx) a felhasználók hozzáadása a ADP GlobalView fiók.</span><span class="sxs-lookup"><span data-stu-id="2c464-222">Work with [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) to add the users in the ADP GlobalView account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2c464-223">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2c464-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2c464-224">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ADP Globalview Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="2c464-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ADP Globalview.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2c464-226">**Britta Simon hozzárendelése ADP Globalview, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2c464-226">**To assign Britta Simon to ADP Globalview, perform the following steps:**</span></span>

1. <span data-ttu-id="2c464-227">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2c464-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2c464-229">Az alkalmazások listában válassza ki a **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="2c464-229">In the applications list, select **ADP Globalview**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. <span data-ttu-id="2c464-231">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2c464-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2c464-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c464-233">Click **Add** button.</span></span> <span data-ttu-id="2c464-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2c464-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2c464-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2c464-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2c464-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2c464-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c464-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2c464-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2c464-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2c464-239">Testing single sign-on</span></span>

<span data-ttu-id="2c464-240">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2c464-240">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="2c464-241">Ha a hozzáférési panelen ADP GlobalView csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ADP GlobalView alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="2c464-241">When you click the ADP GlobalView tile in the Access Panel, you should get automatically signed-on to your ADP GlobalView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c464-242">További források</span><span class="sxs-lookup"><span data-stu-id="2c464-242">Additional resources</span></span>

* [<span data-ttu-id="2c464-243">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="2c464-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c464-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2c464-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

