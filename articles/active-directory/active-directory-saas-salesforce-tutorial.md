---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a><span data-ttu-id="d5761-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce</span><span class="sxs-lookup"><span data-stu-id="d5761-103">Tutorial: Azure Active Directory integration with Salesforce</span></span>

<span data-ttu-id="d5761-104">Ebben az oktatóanyagban elsajátíthatja Salesforce integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d5761-104">In this tutorial, you learn how to integrate Salesforce with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5761-105">Salesforce integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d5761-105">Integrating Salesforce with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5761-106">Megadhatja a Salesforce hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d5761-106">You can control in Azure AD who has access to Salesforce</span></span>
- <span data-ttu-id="d5761-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Salesforce (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d5761-107">You can enable your users to automatically get signed-on to Salesforce (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5761-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d5761-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5761-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5761-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5761-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d5761-110">Prerequisites</span></span>

<span data-ttu-id="d5761-111">Az Azure AD-integráció konfigurálása a Salesforce, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d5761-111">To configure Azure AD integration with Salesforce, you need the following items:</span></span>

- <span data-ttu-id="d5761-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d5761-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5761-113">A Salesforce egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d5761-113">A Salesforce single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d5761-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d5761-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d5761-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d5761-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5761-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d5761-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5761-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5761-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d5761-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d5761-118">Scenario description</span></span>
<span data-ttu-id="d5761-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d5761-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5761-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d5761-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5761-121">Salesforce hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d5761-121">Adding Salesforce from the gallery</span></span>
2. <span data-ttu-id="d5761-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d5761-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-from-the-gallery"></a><span data-ttu-id="d5761-123">Salesforce hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d5761-123">Adding Salesforce from the gallery</span></span>
<span data-ttu-id="d5761-124">Az Azure AD integrálása a Salesforce konfigurálásához kell hozzáadnia Salesforce a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d5761-124">To configure the integration of Salesforce into Azure AD, you need to add Salesforce from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5761-125">**Vegye fel a Salesforce a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5761-125">**To add Salesforce from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5761-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d5761-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5761-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d5761-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5761-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d5761-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d5761-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d5761-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d5761-133">Írja be a keresőmezőbe, **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="d5761-133">In the search box, type **Salesforce**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. <span data-ttu-id="d5761-135">Az eredmények panelen válassza ki a **Salesforce**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d5761-135">In the results panel, select **Salesforce**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d5761-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d5761-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d5761-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Salesforce "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="d5761-138">In this section, you configure and test Azure AD single sign-on with Salesforce based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d5761-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó a Salesforce-ban a felhasználók az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d5761-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce is to a user in Azure AD.</span></span> <span data-ttu-id="d5761-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Salesforce-ban közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d5761-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce needs to be established.</span></span>

<span data-ttu-id="d5761-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="d5761-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce.</span></span>

<span data-ttu-id="d5761-142">Az Azure AD egyszeri bejelentkezést a Salesforce tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d5761-142">To configure and test Azure AD single sign-on with Salesforce, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5761-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d5761-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5761-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d5761-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5761-145">**[A Salesforce tesztfelhasználó létrehozása](#creating-a-salesforce-test-user)**  - való Britta Simon egy megfelelője a Salesforce, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d5761-145">**[Creating a Salesforce test user](#creating-a-salesforce-test-user)** - to have a counterpart of Britta Simon in Salesforce that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5761-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d5761-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5761-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d5761-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d5761-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5761-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d5761-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Salesforce alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d5761-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce application.</span></span>

<span data-ttu-id="d5761-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Salesforce, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5761-150">**To configure Azure AD single sign-on with Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="d5761-151">Az Azure portálon a a **Salesforce** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d5761-151">In the Azure portal, on the **Salesforce** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d5761-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d5761-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. <span data-ttu-id="d5761-155">Az a **Salesforce-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d5761-155">On the **Salesforce Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    <span data-ttu-id="d5761-157">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:</span><span class="sxs-lookup"><span data-stu-id="d5761-157">In the **Sign-on URL** textbox, type the value using the following pattern:</span></span> 
   * <span data-ttu-id="d5761-158">Vállalati fiók:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="d5761-158">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
   * <span data-ttu-id="d5761-159">Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="d5761-159">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5761-160">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="d5761-160">These values are not the real.</span></span> <span data-ttu-id="d5761-161">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d5761-161">Update these values with the actual Sign-on URL.</span></span> <span data-ttu-id="d5761-162">Ügyfél [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d5761-162">Contact [Salesforce Client support team](https://help.salesforce.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="d5761-163">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d5761-163">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. <span data-ttu-id="d5761-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5761-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d5761-167">A a **Salesforce konfigurációs** kattintson **konfigurálása Salesforce** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d5761-167">On the **Salesforce Configuration** section, click **Configure Salesforce** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d5761-168">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d5761-168">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    <span data-ttu-id="d5761-169">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="d5761-169">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span></span>
7.  <span data-ttu-id="d5761-170">Új lap megnyitása a böngésző és a Salesforce-rendszergazdai fiókjával való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="d5761-170">Open a new tab in your browser and log in to your Salesforce administrator account.</span></span>

8.  <span data-ttu-id="d5761-171">Az a **rendszergazda** navigációs ablaktábláján kattintson **biztonsági vezérlők** a kapcsolódó szakasz kibontásához.</span><span class="sxs-lookup"><span data-stu-id="d5761-171">Under the **Administrator** navigation pane, click **Security Controls** to expand the related section.</span></span> <span data-ttu-id="d5761-172">Kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="d5761-172">Then click **Single Sign-On Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  <span data-ttu-id="d5761-174">Az a **egyszeri bejelentkezési beállítások** lapján kattintson a **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5761-174">On the **Single Sign-On Settings** page, click the **Edit** button.</span></span>
    <span data-ttu-id="d5761-175">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span><span class="sxs-lookup"><span data-stu-id="d5761-175">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span></span>

      > [!NOTE]
      > <span data-ttu-id="d5761-176">Ha nem lehet engedélyezni az egyszeri bejelentkezés beállítása a Salesforce-fiókhoz, esetleg forduljon [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support).</span><span class="sxs-lookup"><span data-stu-id="d5761-176">If you are unable to enable Single Sign-On settings for your Salesforce account, you may need to contact [Salesforce Client support team](https://help.salesforce.com/support).</span></span> 

10. <span data-ttu-id="d5761-177">Válassza ki **SAML engedélyezett**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d5761-177">Select **SAML Enabled**, and then click **Save**.</span></span>

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. <span data-ttu-id="d5761-179">A SAML-alapú egyszeri bejelentkezés beállítások konfigurálásához kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="d5761-179">To configure your SAML single sign-on settings, click **New**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. <span data-ttu-id="d5761-181">Az a **SAML-alapú egyszeri bejelentkezési beállítás szerkesztése** lapon, a következő konfigurációkat:</span><span class="sxs-lookup"><span data-stu-id="d5761-181">On the **SAML Single Sign-On Setting Edit** page, make the following configurations:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    <span data-ttu-id="d5761-183">a.</span><span class="sxs-lookup"><span data-stu-id="d5761-183">a.</span></span> <span data-ttu-id="d5761-184">Az a **neve** mezőben adjon meg egy felhasználóbarát nevet ehhez a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d5761-184">For the **Name** field, type in a friendly name for this configuration.</span></span> <span data-ttu-id="d5761-185">Értéket biztosító **neve** automatikusan feltölti a **API-név** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d5761-185">Providing a value for **Name** automatically populate the **API Name** textbox.</span></span>

    <span data-ttu-id="d5761-186">b.</span><span class="sxs-lookup"><span data-stu-id="d5761-186">b.</span></span> <span data-ttu-id="d5761-187">Beillesztés **SMAL Entitásazonosító** be értéket a **kibocsátó** mezőjét a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="d5761-187">Paste **SMAL Entity ID** value into the **Issuer** field in Salesforce.</span></span>

    <span data-ttu-id="d5761-188">c.</span><span class="sxs-lookup"><span data-stu-id="d5761-188">c.</span></span> <span data-ttu-id="d5761-189">Az a **entitásazonosító szövegmező**, írja be a Salesforce-tartománynév, a következő minta használatával:</span><span class="sxs-lookup"><span data-stu-id="d5761-189">In the **Entity Id textbox**, type your Salesforce domain name using the following pattern:</span></span>
      
      * <span data-ttu-id="d5761-190">Vállalati fiók:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="d5761-190">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
      * <span data-ttu-id="d5761-191">Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="d5761-191">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>
      
    <span data-ttu-id="d5761-192">d.</span><span class="sxs-lookup"><span data-stu-id="d5761-192">d.</span></span> <span data-ttu-id="d5761-193">Kattintson **Tallózás** vagy **Choose File** megnyitásához a **válassza ki a feltöltendő fájlt** párbeszédpanelen válassza ki a Salesforce-tanúsítványt, és kattintson **nyissa meg a**töltse fel a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d5761-193">Click **Browse** or **Choose File** to open the **Choose File to Upload** dialog, select your Salesforce certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="d5761-194">e.</span><span class="sxs-lookup"><span data-stu-id="d5761-194">e.</span></span> <span data-ttu-id="d5761-195">A **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz salesforce.com felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="d5761-195">For **SAML Identity Type**, select **Assertion contains User's salesforce.com username**.</span></span>

    <span data-ttu-id="d5761-196">f.</span><span class="sxs-lookup"><span data-stu-id="d5761-196">f.</span></span> <span data-ttu-id="d5761-197">A **SAML-alapú identitás hely**, jelölje be **a tulajdonos utasítás NameIdentifier elemében van identitás**</span><span class="sxs-lookup"><span data-stu-id="d5761-197">For **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**</span></span>

    <span data-ttu-id="d5761-198">g.</span><span class="sxs-lookup"><span data-stu-id="d5761-198">g.</span></span> <span data-ttu-id="d5761-199">Beillesztés **egyszeri bejelentkezési URL-címe** azokat a **Identity Provider bejelentkezési URL-cím** mezőjét a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="d5761-199">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** field in Salesforce.</span></span>
    
    <span data-ttu-id="d5761-200">h.</span><span class="sxs-lookup"><span data-stu-id="d5761-200">h.</span></span> <span data-ttu-id="d5761-201">A **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP-átirányítás**.</span><span class="sxs-lookup"><span data-stu-id="d5761-201">For **Service Provider Initiated Request Binding**, select **HTTP Redirect**.</span></span>
    
    <span data-ttu-id="d5761-202">i.</span><span class="sxs-lookup"><span data-stu-id="d5761-202">i.</span></span> <span data-ttu-id="d5761-203">Végezetül kattintson **mentése** az SAML-alapú egyszeri bejelentkezés beállítások alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="d5761-203">Finally, click **Save** to apply your SAML single sign-on settings.</span></span>

13. <span data-ttu-id="d5761-204">A Salesforce bal oldali navigációs ablaktábláján kattintson **tartományok** bontsa ki a kapcsolódó csomópontot, majd **saját tartomány**.</span><span class="sxs-lookup"><span data-stu-id="d5761-204">On the left navigation pane in Salesforce, click **Domain Management** to expand the related section, and then click **My Domain**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. <span data-ttu-id="d5761-206">Görgessen le a **hitelesítési** szakaszt, és kattintson a **szerkesztése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5761-206">Scroll down to the **Authentication Configuration** section, and click the **Edit** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. <span data-ttu-id="d5761-208">Az a **hitelesítési szolgáltatás** szakaszt, válassza ki a SAML SSO konfigurációs rövid nevét, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d5761-208">In the **Authentication Service** section, select the friendly name of your SAML SSO configuration, and then click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > <span data-ttu-id="d5761-210">Ha egynél több hitelesítési szolgáltatás be van jelölve, a rendszer kéri a felhasználóktól mely hitelesítési szolgáltatás, például a bejelentkezéshez egyszeri bejelentkezést a Salesforce-környezet elindítása során használandó kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="d5761-210">If more than one authentication service is selected, users are prompted to select which authentication service they like to sign in with while initiating single sign-on to your Salesforce environment.</span></span> <span data-ttu-id="d5761-211">Ha nem szeretné, hogy megtörténjen-e, majd meg kell **hagyja bejelölve minden hitelesítési szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="d5761-211">If you don’t want it to happen, then you should **leave all other authentication services unchecked**.</span></span>
<CE>    
> [!TIP]
> <span data-ttu-id="d5761-212">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d5761-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d5761-213">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d5761-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d5761-214">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d5761-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d5761-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5761-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="d5761-216">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d5761-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d5761-218">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5761-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5761-219">A a bal oldali navigációs panelen a **Azure-portálon**, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d5761-219">On the left navigation pane in the **Azure portal**, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5761-221">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d5761-221">To display the list of users, Go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5761-223">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5761-223">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5761-225">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d5761-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5761-227">a.</span><span class="sxs-lookup"><span data-stu-id="d5761-227">a.</span></span> <span data-ttu-id="d5761-228">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5761-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5761-229">b.</span><span class="sxs-lookup"><span data-stu-id="d5761-229">b.</span></span> <span data-ttu-id="d5761-230">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5761-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5761-231">c.</span><span class="sxs-lookup"><span data-stu-id="d5761-231">c.</span></span> <span data-ttu-id="d5761-232">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d5761-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5761-233">d.</span><span class="sxs-lookup"><span data-stu-id="d5761-233">d.</span></span> <span data-ttu-id="d5761-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d5761-234">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-test-user"></a><span data-ttu-id="d5761-235">A Salesforce tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5761-235">Creating a Salesforce test user</span></span>

<span data-ttu-id="d5761-236">Ebben a szakaszban egy Britta Simon nevű felhasználó létrehozása a Salesforce-ban.</span><span class="sxs-lookup"><span data-stu-id="d5761-236">In this section, a user called Britta Simon is created in Salesforce.</span></span> <span data-ttu-id="d5761-237">Salesforce támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d5761-237">Salesforce supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="d5761-238">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="d5761-238">There is no action item for you in this section.</span></span> <span data-ttu-id="d5761-239">Ha a felhasználó nem létezik a Salesforce-ban, egy új létrejön a Salesforce elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="d5761-239">If a user doesn't already exist in Salesforce, a new one is created when you attempt to access Salesforce.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d5761-240">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d5761-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d5761-241">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Salesforce Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d5761-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d5761-243">**Britta Simon hozzárendelése Salesforce, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d5761-243">**To assign Britta Simon to Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="d5761-244">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d5761-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d5761-246">Az alkalmazások listában válassza ki a **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="d5761-246">In the applications list, select **Salesforce**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. <span data-ttu-id="d5761-248">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d5761-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d5761-250">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d5761-250">Click **Add** button.</span></span> <span data-ttu-id="d5761-251">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5761-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d5761-253">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d5761-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5761-254">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5761-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5761-255">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d5761-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d5761-256">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d5761-256">Testing single sign-on</span></span>

<span data-ttu-id="d5761-257">Az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), majd jelentkezzen be a fiókot, és kattintson a **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="d5761-257">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), then sign into the test account, and click **Salesforce**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5761-258">További források</span><span class="sxs-lookup"><span data-stu-id="d5761-258">Additional resources</span></span>

* [<span data-ttu-id="d5761-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d5761-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5761-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d5761-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d5761-261">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5761-261">Configure User Provisioning</span></span>](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

