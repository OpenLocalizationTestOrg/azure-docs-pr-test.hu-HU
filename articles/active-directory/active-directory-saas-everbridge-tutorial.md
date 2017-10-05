---
title: "Oktatóanyag: Azure Active Directoryval integrált EverBridge |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és EverBridge között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: f5a97fc8df978dd55a73ae53516a82f884c14bec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="6fbc9-103">Oktatóanyag: Azure Active Directoryval integrált EverBridge</span><span class="sxs-lookup"><span data-stu-id="6fbc9-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="6fbc9-104">Ebben az oktatóanyagban elsajátíthatja EverBridge integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6fbc9-104">In this tutorial, you learn how to integrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6fbc9-105">EverBridge integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-105">Integrating EverBridge with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6fbc9-106">Megadhatja a EverBridge hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6fbc9-106">You can control in Azure AD who has access to EverBridge</span></span>
- <span data-ttu-id="6fbc9-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett EverBridge (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6fbc9-107">You can enable your users to automatically get signed-on to EverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6fbc9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6fbc9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6fbc9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6fbc9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6fbc9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6fbc9-110">Prerequisites</span></span>

<span data-ttu-id="6fbc9-111">Konfigurálása az Azure AD-integrációs EverBridge, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-111">To configure Azure AD integration with EverBridge, you need the following items:</span></span>

- <span data-ttu-id="6fbc9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6fbc9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6fbc9-113">Egy EverBridge egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6fbc9-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6fbc9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6fbc9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6fbc9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6fbc9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6fbc9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6fbc9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-118">Scenario description</span></span>
<span data-ttu-id="6fbc9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6fbc9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6fbc9-121">A gyűjteményből EverBridge hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-121">Adding EverBridge from the gallery</span></span>
2. <span data-ttu-id="6fbc9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6fbc9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-the-gallery"></a><span data-ttu-id="6fbc9-123">A gyűjteményből EverBridge hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-123">Adding EverBridge from the gallery</span></span>
<span data-ttu-id="6fbc9-124">Az Azure AD integrálása a EverBridge konfigurálásához kell hozzáadnia EverBridge a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-124">To configure the integration of EverBridge into Azure AD, you need to add EverBridge from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6fbc9-125">**A gyűjteményből EverBridge hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-125">**To add EverBridge from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6fbc9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6fbc9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6fbc9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6fbc9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6fbc9-133">Írja be a keresőmezőbe, **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-133">In the search box, type **EverBridge**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="6fbc9-135">Az eredmények panelen válassza ki a **EverBridge**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-135">In the results panel, select **EverBridge**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6fbc9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6fbc9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6fbc9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján EverBridge.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6fbc9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó EverBridge a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EverBridge is to a user in Azure AD.</span></span> <span data-ttu-id="6fbc9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a EverBridge közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-140">In other words, a link relationship between an Azure AD user and the related user in EverBridge needs to be established.</span></span>

<span data-ttu-id="6fbc9-141">EverBridge, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-141">In EverBridge, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6fbc9-142">Az Azure AD egyszeri bejelentkezést a EverBridge tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-142">To configure and test Azure AD single sign-on with EverBridge, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6fbc9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6fbc9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6fbc9-145">**[Egy EverBridge tesztfelhasználó létrehozása](#creating-an-everbridge-test-user)**  - való Britta Simon valami EverBridge, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - to have a counterpart of Britta Simon in EverBridge that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6fbc9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6fbc9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6fbc9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6fbc9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az EverBridge alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="6fbc9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés EverBridge, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-150">**To configure Azure AD single sign-on with EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="6fbc9-151">Az Azure portálon a a **EverBridge** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-151">In the Azure portal, on the **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6fbc9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="6fbc9-155">Az a **EverBridge tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-155">On the **EverBridge Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="6fbc9-157">a.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-157">a.</span></span> <span data-ttu-id="6fbc9-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="6fbc9-158">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="6fbc9-159">b.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-159">b.</span></span> <span data-ttu-id="6fbc9-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="6fbc9-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6fbc9-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-161">These values are not real.</span></span> <span data-ttu-id="6fbc9-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="6fbc9-163">Ügyfél [EverBridge támogatási csoport](mailto:support@everbridge.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-163">Contact [EverBridge support team](mailto:support@everbridge.com) to get these values.</span></span>
 
4. <span data-ttu-id="6fbc9-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="6fbc9-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6fbc9-168">A a **EverBridge konfigurációs** kattintson **konfigurálása EverBridge** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-168">On the **EverBridge Configuration** section, click **Configure EverBridge** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6fbc9-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="6fbc9-171">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, meg kell bejelentkezés a Everbridge bérlő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-171">To get SSO configured for your application, you need to sign-on to your Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="6fbc9-172">A felső menüben kattintson a **beállítások** lapot, és válasszon **egyszeri bejelentkezés** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-172">In the menu on the top, click the **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="6fbc9-174">a.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-174">a.</span></span> <span data-ttu-id="6fbc9-175">Az a **neve** szövegmezőhöz azonosítója szolgáltató neve (például: a vállalat nevét).</span><span class="sxs-lookup"><span data-stu-id="6fbc9-175">In the **Name** textbox, type the name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="6fbc9-176">b.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-176">b.</span></span> <span data-ttu-id="6fbc9-177">Az a **API-név** szövegmező, írja be az API neve.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-177">In the **API Name** textbox, type the name of API.</span></span>
   
    <span data-ttu-id="6fbc9-178">c.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-178">c.</span></span> <span data-ttu-id="6fbc9-179">Kattintson a **Choose File** feltölteni a metaadatok fájlt, amely az Azure-portálról letöltött gombra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-179">Click **Choose File** button to upload the metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="6fbc9-180">d.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-180">d.</span></span> <span data-ttu-id="6fbc9-181">Válassza a SAML-alapú identitás helye, **identitás a tulajdonos utasítás NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-181">In the SAML Identity Location, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>
   
    <span data-ttu-id="6fbc9-182">e.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-182">e.</span></span> <span data-ttu-id="6fbc9-183">Az a **Identity Provider bejelentkezési URL-cím** szövegmezőhöz illessze be az Azure AD SAML SSO URL-cím értékét.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-183">In the **Identity Provider Login URL** textbox, paste the value of SAML SSO URL from Azure AD.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="6fbc9-185">f.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-185">f.</span></span> <span data-ttu-id="6fbc9-186">Válassza ki a szolgáltató által kezdeményezett kérelem Szolgáltatáskötés, **HTTP-átirányítás**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-186">In the Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="6fbc9-187">g.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-187">g.</span></span> <span data-ttu-id="6fbc9-188">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="6fbc9-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="6fbc9-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6fbc9-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6fbc9-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6fbc9-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6fbc9-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="6fbc9-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6fbc9-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6fbc9-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6fbc9-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6fbc9-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6fbc9-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="6fbc9-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6fbc9-204">a.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-204">a.</span></span> <span data-ttu-id="6fbc9-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6fbc9-206">b.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-206">b.</span></span> <span data-ttu-id="6fbc9-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6fbc9-208">c.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-208">c.</span></span> <span data-ttu-id="6fbc9-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6fbc9-210">d.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-210">d.</span></span> <span data-ttu-id="6fbc9-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="6fbc9-212">Egy EverBridge tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="6fbc9-213">Ebben a szakaszban egy Everbridge Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="6fbc9-214">Együttműködve [EverBridge támogatási csoport](mailto:support@everbridge.com) a felhasználók hozzáadása a Everbridge platform.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-214">Work with [EverBridge support team](mailto:support@everbridge.com) to add the users in the Everbridge platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6fbc9-215">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6fbc9-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6fbc9-216">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés EverBridge Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EverBridge.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6fbc9-218">**Britta Simon hozzárendelése EverBridge, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6fbc9-218">**To assign Britta Simon to EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="6fbc9-219">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6fbc9-221">Az alkalmazások listában válassza ki a **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-221">In the applications list, select **EverBridge**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="6fbc9-223">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6fbc9-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-225">Click **Add** button.</span></span> <span data-ttu-id="6fbc9-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6fbc9-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6fbc9-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6fbc9-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6fbc9-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6fbc9-231">Testing single sign-on</span></span>

<span data-ttu-id="6fbc9-232">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-232">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6fbc9-233">Ha a hozzáférési panelen Everbridge csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Everbridge alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="6fbc9-233">When you click the Everbridge tile in the Access Panel, you should get automatically signed-on to your Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fbc9-234">További források</span><span class="sxs-lookup"><span data-stu-id="6fbc9-234">Additional resources</span></span>

* [<span data-ttu-id="6fbc9-235">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="6fbc9-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6fbc9-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6fbc9-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

