---
title: "Oktatóanyag: Azure Active Directoryval integrált ADP eTime |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ADP eTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 31fed307a32e629d00aab7cc9d5167ee16d83936
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="c40d9-103">Oktatóanyag: Azure Active Directoryval integrált ADP eTime</span><span class="sxs-lookup"><span data-stu-id="c40d9-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="c40d9-104">Ebben az oktatóanyagban elsajátíthatja ADP eTime integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c40d9-104">In this tutorial, you learn how to integrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c40d9-105">ADP eTime integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c40d9-105">Integrating ADP eTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c40d9-106">Megadhatja a ADP eTime hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c40d9-106">You can control in Azure AD who has access to ADP eTime</span></span>
- <span data-ttu-id="c40d9-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a ADP eTime (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c40d9-107">You can enable your users to automatically get signed-on to ADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c40d9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c40d9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c40d9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c40d9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c40d9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c40d9-110">Prerequisites</span></span>

<span data-ttu-id="c40d9-111">Az Azure AD-integrációs ADP eTime konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="c40d9-111">To configure Azure AD integration with ADP eTime, you need the following items:</span></span>

- <span data-ttu-id="c40d9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c40d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c40d9-113">Egy ADP eTime egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c40d9-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c40d9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c40d9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c40d9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c40d9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c40d9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c40d9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c40d9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c40d9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c40d9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c40d9-118">Scenario description</span></span>
<span data-ttu-id="c40d9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c40d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c40d9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c40d9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c40d9-121">A gyűjteményből ADP eTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c40d9-121">Adding ADP eTime from the gallery</span></span>
2. <span data-ttu-id="c40d9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c40d9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-the-gallery"></a><span data-ttu-id="c40d9-123">A gyűjteményből ADP eTime hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c40d9-123">Adding ADP eTime from the gallery</span></span>
<span data-ttu-id="c40d9-124">Az Azure AD integrálása a ADP eTime konfigurálásához kell hozzáadnia ADP eTime a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c40d9-124">To configure the integration of ADP eTime into Azure AD, you need to add ADP eTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c40d9-125">**A gyűjteményből ADP eTime hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c40d9-125">**To add ADP eTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c40d9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c40d9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c40d9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c40d9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c40d9-133">Írja be a keresőmezőbe, **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-133">In the search box, type **ADP eTime**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="c40d9-135">Az eredmények panelen válassza ki a **ADP eTime**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c40d9-135">In the results panel, select **ADP eTime**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c40d9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c40d9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c40d9-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az úgynevezett "Britta Simon." tesztfelhasználó alapján ADP eTime</span><span class="sxs-lookup"><span data-stu-id="c40d9-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c40d9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ADP eTime a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c40d9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ADP eTime is to a user in Azure AD.</span></span> <span data-ttu-id="c40d9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ADP eTime közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c40d9-140">In other words, a link relationship between an Azure AD user and the related user in ADP eTime needs to be established.</span></span>

<span data-ttu-id="c40d9-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** ADP eTime a.</span><span class="sxs-lookup"><span data-stu-id="c40d9-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ADP eTime.</span></span>

<span data-ttu-id="c40d9-142">Az Azure AD egyszeri bejelentkezést a ADP eTime tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c40d9-142">To configure and test Azure AD single sign-on with ADP eTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c40d9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c40d9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c40d9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c40d9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c40d9-145">**[Egy ADP eTime tesztfelhasználó létrehozása](#creating-an-adp-etime-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó ADP eTime rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c40d9-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - to have a counterpart of Britta Simon in ADP eTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c40d9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c40d9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c40d9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c40d9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c40d9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c40d9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c40d9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ADP eTime alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c40d9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="c40d9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés ADP eTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c40d9-150">**To configure Azure AD single sign-on with ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="c40d9-151">Az Azure portálon a a **ADP eTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-151">In the Azure portal, on the **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c40d9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c40d9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="c40d9-155">Az a **ADP eTime tartomány és az URL-címek** csoportjában hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="c40d9-155">On the **ADP eTime Domain and URLs** section, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="c40d9-157">a.</span><span class="sxs-lookup"><span data-stu-id="c40d9-157">a.</span></span> <span data-ttu-id="c40d9-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="c40d9-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="c40d9-159">b.</span><span class="sxs-lookup"><span data-stu-id="c40d9-159">b.</span></span> <span data-ttu-id="c40d9-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="c40d9-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="c40d9-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="c40d9-161">These values are not the real.</span></span> <span data-ttu-id="c40d9-162">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c40d9-162">Update these values with the actual Reply URL and Identifier.</span></span> <span data-ttu-id="c40d9-163">Ügyfél [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c40d9-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to get these values.</span></span>

4. <span data-ttu-id="c40d9-164">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c40d9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="c40d9-166">A ADP eTime alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="c40d9-166">The ADP eTime application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="c40d9-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="c40d9-167">The following screenshot shows an example for this.</span></span> <span data-ttu-id="c40d9-168">A jogcím neve mindig lesz **"PersonImmutableID"** és az érték, amelynek jelenleg van leképezve ExtensionAttribute2, amely tartalmazza az EmployeeID felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c40d9-168">The claim name will always be **"PersonImmutableID"** and the value of which we have mapped to ExtensionAttribute2 which contains the EmployeeID of the user.</span></span> 

    <span data-ttu-id="c40d9-169">Itt az EmployeeID a állítva az Azure AD ADP eTime fogja elvégezni, de ez leképezheti is alapján az alkalmazás beállításait egy másik értéket.</span><span class="sxs-lookup"><span data-stu-id="c40d9-169">Here the user mapping from Azure AD to ADP eTime will be done on the EmployeeID but you can map this to a different value also based on your application settings.</span></span> <span data-ttu-id="c40d9-170">Ezért kérjük, munkahelyi rendelkező [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) először használja a helyes azonosító egy olyan felhasználó, és képezze le az értéket a a **"PersonImmutableID"** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="c40d9-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first to use the correct identifier of a user and map that value with the **"PersonImmutableID"** claim.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="c40d9-172">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, az ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c40d9-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="c40d9-173">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="c40d9-173">Attribute Name</span></span> | <span data-ttu-id="c40d9-174">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="c40d9-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="c40d9-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="c40d9-175">PersonImmutableID</span></span> | <span data-ttu-id="c40d9-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="c40d9-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="c40d9-177">a.</span><span class="sxs-lookup"><span data-stu-id="c40d9-177">a.</span></span> <span data-ttu-id="c40d9-178">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c40d9-178">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c40d9-181">b.</span><span class="sxs-lookup"><span data-stu-id="c40d9-181">b.</span></span> <span data-ttu-id="c40d9-182">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="c40d9-182">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="c40d9-183">c.</span><span class="sxs-lookup"><span data-stu-id="c40d9-183">c.</span></span> <span data-ttu-id="c40d9-184">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="c40d9-184">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c40d9-185">d.</span><span class="sxs-lookup"><span data-stu-id="c40d9-185">d.</span></span> <span data-ttu-id="c40d9-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c40d9-187">A SAML-előfeltétel konfigurálása előtt kapcsolatba kell lépnie a [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) és az egyedi azonosító attribútum értékének kérhetnek a bérlő.</span><span class="sxs-lookup"><span data-stu-id="c40d9-187">Before you can configure the SAML assertion, you need to contact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="c40d9-188">Ezt az értéket az egyéni jogcímleírásokat, az alkalmazás konfigurálása van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c40d9-188">You need this value to configure the custom claim for your application.</span></span> 

7. <span data-ttu-id="c40d9-189">A a **ADP eTime konfigurációs** kattintson **konfigurálása ADP eTime** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c40d9-189">On the **ADP eTime Configuration** section, click **Configure ADP eTime** to open **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="c40d9-191">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-191">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="c40d9-193">Egyszeri bejelentkezés konfigurálása **ADP eTime** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="c40d9-193">To configure single sign-on on **ADP eTime** side, you need to send the downloaded **Metadata XML** to [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="c40d9-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c40d9-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c40d9-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c40d9-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c40d9-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c40d9-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c40d9-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c40d9-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="c40d9-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c40d9-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c40d9-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c40d9-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c40d9-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c40d9-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c40d9-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c40d9-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c40d9-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c40d9-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c40d9-209">a.</span><span class="sxs-lookup"><span data-stu-id="c40d9-209">a.</span></span> <span data-ttu-id="c40d9-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c40d9-211">b.</span><span class="sxs-lookup"><span data-stu-id="c40d9-211">b.</span></span> <span data-ttu-id="c40d9-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c40d9-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c40d9-213">c.</span><span class="sxs-lookup"><span data-stu-id="c40d9-213">c.</span></span> <span data-ttu-id="c40d9-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c40d9-215">d.</span><span class="sxs-lookup"><span data-stu-id="c40d9-215">d.</span></span> <span data-ttu-id="c40d9-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="c40d9-217">Egy ADP eTime tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c40d9-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="c40d9-218">Ez a szakasz célja ADP eTime Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c40d9-218">The objective of this section is to create a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="c40d9-219">Együttműködve [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) a felhasználók hozzáadása a ADP eTime fiók.</span><span class="sxs-lookup"><span data-stu-id="c40d9-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to add the users in the ADP eTime account.</span></span> 
   
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c40d9-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c40d9-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c40d9-221">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó ADP eTime való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="c40d9-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ADP eTime.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c40d9-223">**Britta Simon hozzárendelése ADP eTime, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c40d9-223">**To assign Britta Simon to ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="c40d9-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c40d9-226">Az alkalmazások listában válassza ki a **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-226">In the applications list, select **ADP eTime**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="c40d9-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c40d9-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c40d9-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c40d9-230">Click **Add** button.</span></span> <span data-ttu-id="c40d9-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c40d9-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c40d9-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c40d9-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c40d9-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c40d9-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c40d9-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c40d9-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c40d9-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c40d9-236">Testing single sign-on</span></span>

<span data-ttu-id="c40d9-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c40d9-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c40d9-238">Ha a hozzáférési panelen ADP eTime csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ADP eTime alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c40d9-238">When you click the ADP eTime tile in the Access Panel, you should get automatically signed-on to your ADP eTime application.</span></span>
<span data-ttu-id="c40d9-239">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c40d9-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c40d9-240">További források</span><span class="sxs-lookup"><span data-stu-id="c40d9-240">Additional resources</span></span>

* [<span data-ttu-id="c40d9-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c40d9-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c40d9-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c40d9-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

