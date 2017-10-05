---
title: "Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és LockPath Keylight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e64a966f24411818abc4cc4ab29a428b5577d012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="d96c1-103">Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="d96c1-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="d96c1-104">Ebben az oktatóanyagban elsajátíthatja LockPath Keylight integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d96c1-104">In this tutorial, you learn how to integrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d96c1-105">LockPath Keylight integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d96c1-105">Integrating LockPath Keylight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d96c1-106">Megadhatja a LockPath Keylight hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d96c1-106">You can control in Azure AD who has access to LockPath Keylight</span></span>
- <span data-ttu-id="d96c1-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a LockPath Keylight (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="d96c1-107">You can enable your users to automatically get signed-on to LockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d96c1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d96c1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d96c1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d96c1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d96c1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d96c1-110">Prerequisites</span></span>

<span data-ttu-id="d96c1-111">Konfigurálása az Azure AD-integrációs LockPath Keylight, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d96c1-111">To configure Azure AD integration with LockPath Keylight, you need the following items:</span></span>

- <span data-ttu-id="d96c1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d96c1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d96c1-113">Egy LockPath Keylight egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d96c1-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d96c1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d96c1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d96c1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d96c1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d96c1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d96c1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d96c1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d96c1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d96c1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d96c1-118">Scenario description</span></span>
<span data-ttu-id="d96c1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d96c1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d96c1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d96c1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d96c1-121">A gyűjteményből LockPath Keylight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d96c1-121">Adding LockPath Keylight from the gallery</span></span>
2. <span data-ttu-id="d96c1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d96c1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-the-gallery"></a><span data-ttu-id="d96c1-123">A gyűjteményből LockPath Keylight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d96c1-123">Adding LockPath Keylight from the gallery</span></span>
<span data-ttu-id="d96c1-124">Az Azure AD integrálása a LockPath Keylight konfigurálásához kell hozzáadnia LockPath Keylight a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d96c1-124">To configure the integration of LockPath Keylight into Azure AD, you need to add LockPath Keylight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d96c1-125">**A gyűjteményből LockPath Keylight hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d96c1-125">**To add LockPath Keylight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d96c1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d96c1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d96c1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d96c1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d96c1-133">Írja be a keresőmezőbe, **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-133">In the search box, type **LockPath Keylight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="d96c1-135">Az eredmények panelen válassza ki a **LockPath Keylight**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d96c1-135">In the results panel, select **LockPath Keylight**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d96c1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d96c1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d96c1-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a LockPath Keylight "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="d96c1-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d96c1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LockPath Keylight a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d96c1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LockPath Keylight is to a user in Azure AD.</span></span> <span data-ttu-id="d96c1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LockPath Keylight közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d96c1-140">In other words, a link relationship between an Azure AD user and the related user in LockPath Keylight needs to be established.</span></span>

<span data-ttu-id="d96c1-141">LockPath Keylight, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d96c1-141">In LockPath Keylight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d96c1-142">Az Azure AD egyszeri bejelentkezést a LockPath Keylight tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d96c1-142">To configure and test Azure AD single sign-on with LockPath Keylight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d96c1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d96c1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d96c1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d96c1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d96c1-145">**[LockPath Keylight tesztfelhasználó létrehozása](#creating-a-lockpath-keylight-test-user)**  - való egy megfelelője a Britta Simon LockPath Keylight, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d96c1-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - to have a counterpart of Britta Simon in LockPath Keylight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d96c1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d96c1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d96c1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d96c1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d96c1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d96c1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d96c1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az LockPath Keylight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d96c1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="d96c1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés LockPath Keylight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d96c1-150">**To configure Azure AD single sign-on with LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="d96c1-151">Az Azure portálon a a **LockPath Keylight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-151">In the Azure portal, on the **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d96c1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d96c1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="d96c1-155">A a **LockPath Keylight tartomány és az URL-címek** területen az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d96c1-155">On the **LockPath Keylight Domain and URLs** section, perform the following steps::</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="d96c1-157">a.</span><span class="sxs-lookup"><span data-stu-id="d96c1-157">a.</span></span> <span data-ttu-id="d96c1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="d96c1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="d96c1-159">b.</span><span class="sxs-lookup"><span data-stu-id="d96c1-159">b.</span></span> <span data-ttu-id="d96c1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="d96c1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="d96c1-161">c.</span><span class="sxs-lookup"><span data-stu-id="d96c1-161">c.</span></span> <span data-ttu-id="d96c1-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="d96c1-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="d96c1-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d96c1-163">These values are not real.</span></span> <span data-ttu-id="d96c1-164">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="d96c1-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="d96c1-165">Ügyfél [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d96c1-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) to get these values.</span></span> 

4. <span data-ttu-id="d96c1-166">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d96c1-166">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="d96c1-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d96c1-170">A a **LockPath Keylight konfigurációs** kattintson **konfigurálása LockPath Keylight** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d96c1-170">On the **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d96c1-171">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d96c1-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="d96c1-173">Ahhoz, hogy a LockPath Keylight SSO, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d96c1-173">To enable SSO in LockPath Keylight, perform the following steps:</span></span>
   
    <span data-ttu-id="d96c1-174">a.</span><span class="sxs-lookup"><span data-stu-id="d96c1-174">a.</span></span> <span data-ttu-id="d96c1-175">Bejelentkezés LockPath Keylight fiókjába rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d96c1-175">Sign-on to your LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="d96c1-176">b.</span><span class="sxs-lookup"><span data-stu-id="d96c1-176">b.</span></span> <span data-ttu-id="d96c1-177">Kattintson a felső menüben **személy**, és válassza ki **Keylight telepítő**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-177">In the menu on the top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="d96c1-179">c.</span><span class="sxs-lookup"><span data-stu-id="d96c1-179">c.</span></span> <span data-ttu-id="d96c1-180">Kattintson a bal oldali fastruktúrában, **SAML**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-180">In the treeview on the left, click **SAML**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="d96c1-182">d.</span><span class="sxs-lookup"><span data-stu-id="d96c1-182">d.</span></span> <span data-ttu-id="d96c1-183">Az a **SAML beállítások** párbeszédpanel, kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-183">On the **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="d96c1-185">Az a **SAML beállításainak szerkesztése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d96c1-185">On the **Edit SAML Settings** dialog page, perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="d96c1-187">a.</span><span class="sxs-lookup"><span data-stu-id="d96c1-187">a.</span></span> <span data-ttu-id="d96c1-188">Állítsa be **SAML-alapú hitelesítés** való **aktív**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-188">Set **SAML authentication** to **Active**.</span></span>

    <span data-ttu-id="d96c1-189">b.</span><span class="sxs-lookup"><span data-stu-id="d96c1-189">b.</span></span> <span data-ttu-id="d96c1-190">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** érték, amely az Azure-portálról másolta a **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d96c1-190">Paste the **SAML Single Sign-On Service URL** value which you have copied from the Azure portal into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="d96c1-191">c.</span><span class="sxs-lookup"><span data-stu-id="d96c1-191">c.</span></span> <span data-ttu-id="d96c1-192">Beillesztés a **egyetlen Sign-Out URL-címe** érték, amely az Azure-portálról másolta a **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d96c1-192">Paste the **Single Sign-Out Service URL** value which you have copied from the Azure portal into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="d96c1-193">d.</span><span class="sxs-lookup"><span data-stu-id="d96c1-193">d.</span></span> <span data-ttu-id="d96c1-194">Kattintson a **Choose File** válassza ki a letöltött LockPath Keylight tanúsítványt, és kattintson **nyitott** töltse fel a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d96c1-194">Click **Choose File** to select your downloaded LockPath Keylight certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="d96c1-195">e.</span><span class="sxs-lookup"><span data-stu-id="d96c1-195">e.</span></span> <span data-ttu-id="d96c1-196">Állítsa be **SAML felhasználóazonosító hely** való **NameIdentifier elem a tulajdonos utasítás**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-196">Set **SAML User Id location** to **NameIdentifier element of the subject statement**.</span></span>
    
    <span data-ttu-id="d96c1-197">f.</span><span class="sxs-lookup"><span data-stu-id="d96c1-197">f.</span></span> <span data-ttu-id="d96c1-198">Adja meg a **Keylight szolgáltató** használatával a következő mintát: **https://&lt;#companyname&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-198">Provide the **Keylight Service Provider** using the following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="d96c1-199">g.</span><span class="sxs-lookup"><span data-stu-id="d96c1-199">g.</span></span> <span data-ttu-id="d96c1-200">Állítsa be **automatikus-kiépítési felhasználók** való **aktív**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-200">Set **Auto-provision users** to **Active**.</span></span>

    <span data-ttu-id="d96c1-201">h.</span><span class="sxs-lookup"><span data-stu-id="d96c1-201">h.</span></span> <span data-ttu-id="d96c1-202">Állítsa be **automatikus-kiépítési fióktípus** való **teljes felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-202">Set **Auto-provision account type** to **Full User**.</span></span>

    <span data-ttu-id="d96c1-203">i.</span><span class="sxs-lookup"><span data-stu-id="d96c1-203">i.</span></span> <span data-ttu-id="d96c1-204">Állítsa be **automatikus-kiépítési biztonsági szerepkör**, jelölje be **SAML az általános jogú felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="d96c1-205">j.</span><span class="sxs-lookup"><span data-stu-id="d96c1-205">j.</span></span> <span data-ttu-id="d96c1-206">Állítsa be **automatikus-kiépítési biztonsági config**, jelölje be **normál felhasználói konfigurációban**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="d96c1-207">k.</span><span class="sxs-lookup"><span data-stu-id="d96c1-207">k.</span></span> <span data-ttu-id="d96c1-208">Az a **E-mail attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="d96c1-208">In the **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="d96c1-209">l.</span><span class="sxs-lookup"><span data-stu-id="d96c1-209">l.</span></span> <span data-ttu-id="d96c1-210">Az a **Keresztnév attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="d96c1-210">In the **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="d96c1-211">m.</span><span class="sxs-lookup"><span data-stu-id="d96c1-211">m.</span></span> <span data-ttu-id="d96c1-212">Az a **utolsó name attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="d96c1-212">In the **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="d96c1-213">n.</span><span class="sxs-lookup"><span data-stu-id="d96c1-213">n.</span></span> <span data-ttu-id="d96c1-214">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d96c1-215">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d96c1-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d96c1-216">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d96c1-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d96c1-217">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d96c1-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d96c1-218">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d96c1-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="d96c1-219">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d96c1-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d96c1-221">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d96c1-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d96c1-222">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-222">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d96c1-224">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-224">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d96c1-226">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d96c1-226">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d96c1-228">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d96c1-228">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d96c1-230">a.</span><span class="sxs-lookup"><span data-stu-id="d96c1-230">a.</span></span> <span data-ttu-id="d96c1-231">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-231">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d96c1-232">b.</span><span class="sxs-lookup"><span data-stu-id="d96c1-232">b.</span></span> <span data-ttu-id="d96c1-233">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d96c1-233">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d96c1-234">c.</span><span class="sxs-lookup"><span data-stu-id="d96c1-234">c.</span></span> <span data-ttu-id="d96c1-235">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-235">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d96c1-236">d.</span><span class="sxs-lookup"><span data-stu-id="d96c1-236">d.</span></span> <span data-ttu-id="d96c1-237">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="d96c1-238">LockPath Keylight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d96c1-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="d96c1-239">Ebben a szakaszban egy LockPath Keylight Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d96c1-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="d96c1-240">LockPath Keylight támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d96c1-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="d96c1-241">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="d96c1-241">There is no action item for you in this section.</span></span> <span data-ttu-id="d96c1-242">Új felhasználó jön létre, amikor LockPath Keylight elérését, ha a felhasználó még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="d96c1-242">A new user is created when accessing LockPath Keylight if the user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="d96c1-243">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="d96c1-243">If you need to create a user manually, you need to contact the [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d96c1-244">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d96c1-244">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d96c1-245">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LockPath Keylight Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d96c1-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LockPath Keylight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d96c1-247">**Britta Simon hozzárendelése LockPath Keylight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d96c1-247">**To assign Britta Simon to LockPath Keylight, perform the following steps:**</span></span>

1. <span data-ttu-id="d96c1-248">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d96c1-250">Az alkalmazások listában válassza ki a **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-250">In the applications list, select **LockPath Keylight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="d96c1-252">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d96c1-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d96c1-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d96c1-254">Click **Add** button.</span></span> <span data-ttu-id="d96c1-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d96c1-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d96c1-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d96c1-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d96c1-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d96c1-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d96c1-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d96c1-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d96c1-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d96c1-260">Testing single sign-on</span></span>

<span data-ttu-id="d96c1-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d96c1-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d96c1-262">Ha a hozzáférési panelen LockPath Keylight csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az LockPath Keylight alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d96c1-262">When you click the LockPath Keylight tile in the Access Panel, you should get automatically signed-on to your LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d96c1-263">További források</span><span class="sxs-lookup"><span data-stu-id="d96c1-263">Additional resources</span></span>

* [<span data-ttu-id="d96c1-264">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d96c1-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d96c1-265">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d96c1-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

