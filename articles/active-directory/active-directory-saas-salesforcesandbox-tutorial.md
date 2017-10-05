---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Salesforce védőfal között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="750c6-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal</span><span class="sxs-lookup"><span data-stu-id="750c6-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="750c6-104">Ebben az oktatóanyagban elsajátíthatja Salesforce védőfal integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="750c6-104">In this tutorial, you learn how to integrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="750c6-105">Salesforce védőfal integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="750c6-105">Integrating Salesforce Sandbox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="750c6-106">Megadhatja a Salesforce védőfal hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="750c6-106">You can control in Azure AD who has access to Salesforce Sandbox</span></span>
- <span data-ttu-id="750c6-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Salesforce védőfal (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="750c6-107">You can enable your users to automatically get signed-on to Salesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="750c6-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="750c6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="750c6-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="750c6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="750c6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="750c6-110">Prerequisites</span></span>

<span data-ttu-id="750c6-111">Az Azure AD-integráció konfigurálása a Salesforce védőfal, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="750c6-111">To configure Azure AD integration with Salesforce Sandbox, you need the following items:</span></span>

- <span data-ttu-id="750c6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="750c6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="750c6-113">A Salesforce védőfal egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="750c6-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="750c6-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="750c6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="750c6-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="750c6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="750c6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="750c6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="750c6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="750c6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="750c6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="750c6-118">Scenario description</span></span>
<span data-ttu-id="750c6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="750c6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="750c6-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="750c6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="750c6-121">Salesforce védőfal hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="750c6-121">Adding Salesforce Sandbox from the gallery</span></span>
2. <span data-ttu-id="750c6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="750c6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-the-gallery"></a><span data-ttu-id="750c6-123">Salesforce védőfal hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="750c6-123">Adding Salesforce Sandbox from the gallery</span></span>
<span data-ttu-id="750c6-124">Az Azure AD integrálása a Salesforce védőfal konfigurálásához kell hozzáadnia Salesforce védőfal a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="750c6-124">To configure the integration of Salesforce Sandbox into Azure AD, you need to add Salesforce Sandbox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="750c6-125">**Vegye fel a Salesforce védőfal a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="750c6-125">**To add Salesforce Sandbox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="750c6-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="750c6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="750c6-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="750c6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="750c6-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="750c6-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="750c6-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="750c6-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="750c6-133">Írja be a keresőmezőbe, **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="750c6-133">In the search box, type **Salesforce Sandbox**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="750c6-135">Az eredmények panelen válassza ki a **Salesforce védőfal**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="750c6-135">In the results panel, select **Salesforce Sandbox**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="750c6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="750c6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="750c6-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Salesforce védőfal "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="750c6-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="750c6-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Salesforce védőfal tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="750c6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce Sandbox is to a user in Azure AD.</span></span> <span data-ttu-id="750c6-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Salesforce védőfal közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="750c6-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce Sandbox needs to be established.</span></span>

<span data-ttu-id="750c6-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Salesforce védőfal.</span><span class="sxs-lookup"><span data-stu-id="750c6-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="750c6-142">Az Azure AD egyszeri bejelentkezést a Salesforce védőfal tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="750c6-142">To configure and test Azure AD single sign-on with Salesforce Sandbox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="750c6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="750c6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="750c6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="750c6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="750c6-145">**[A Salesforce védőfal tesztfelhasználó létrehozása](#creating-a-salesforce-sandbox-test-user)**  - való Britta Simon egy megfelelője a Salesforce védőfal, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="750c6-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - to have a counterpart of Britta Simon in Salesforce Sandbox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="750c6-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="750c6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="750c6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="750c6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="750c6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="750c6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="750c6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Salesforce védőfal alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="750c6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="750c6-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Salesforce védőfal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="750c6-150">**To configure Azure AD single sign-on with Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="750c6-151">Az Azure portálon a a **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="750c6-151">In the Azure portal, on the **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="750c6-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="750c6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="750c6-155">Az a **Salesforce védőfal tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="750c6-155">On the **Salesforce Sandbox Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="750c6-157">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="750c6-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="750c6-158">Ez az érték nem a valós.</span><span class="sxs-lookup"><span data-stu-id="750c6-158">This value is not the real.</span></span> <span data-ttu-id="750c6-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet. Ügyfél [Salesforce védőfal ügyfél-támogatási csoport](https://help.salesforce.com/support) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="750c6-159">Update this value with the actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) to get this value.</span></span>


4. <span data-ttu-id="750c6-160">Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, majd konfigurálnia kell a **azonosító** ugyanazt az értéket, hogy a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="750c6-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure the **Identifier** to have the same value as the **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="750c6-161">A **azonosító** mező található ellenőrzésével a **megjelenítése speciális beállítások** jelölőnégyzetet a **alkalmazás URL-cím konfigurálása** lap a párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="750c6-161">The **Identifier** field can be found by checking the **Show advanced settings** checkbox on the **Configure App URL** page of the dialog</span></span> 


5. <span data-ttu-id="750c6-162">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="750c6-162">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="750c6-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="750c6-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="750c6-166">A a **Salesforce védőfal konfigurációs** kattintson **konfigurálása Salesforce védőfal** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="750c6-166">On the **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="750c6-167">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="750c6-167">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="750c6-168">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="750c6-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="750c6-169">Új lap megnyitása a böngészőben, és jelentkezzen be a Salesforce védőfal rendszergazdai fiókjával.</span><span class="sxs-lookup"><span data-stu-id="750c6-169">Open a new tab in your browser and log in to your Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="750c6-170">Kattintson a felső menüben **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="750c6-170">In the menu on the top, click **Setup**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="750c6-172">A bal oldali navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="750c6-172">In the navigation pane on the left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="750c6-174">Egyszeri bejelentkezés beállítások csoportjában hajtsa végre a következő lépéseket: ![konfigurálása egyszeri bejelentkezéshez](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="750c6-174">On the Single Sign-On Settings section, perform the following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="750c6-175">a.</span><span class="sxs-lookup"><span data-stu-id="750c6-175">a.</span></span>  <span data-ttu-id="750c6-176">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="750c6-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="750c6-177">b.</span><span class="sxs-lookup"><span data-stu-id="750c6-177">b.</span></span>  <span data-ttu-id="750c6-178">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="750c6-178">Click **New**.</span></span>

12. <span data-ttu-id="750c6-179">A SAML egyszeri bejelentkezés beállítások szakaszban hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="750c6-179">On the SAML Single Sign-On Settings section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="750c6-181">a.In a szövegmezőben adja meg a konfiguráció nevét (pl.: *SPSSOWAAD\_teszt*).</span><span class="sxs-lookup"><span data-stu-id="750c6-181">a.In the Name textbox, type the name of the configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="750c6-182">b.</span><span class="sxs-lookup"><span data-stu-id="750c6-182">b.</span></span> <span data-ttu-id="750c6-183">Beillesztés **SMAL Entitásazonosító** be értéket a **kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="750c6-183">Paste **SMAL Entity ID** value into the **Issuer** textbox.</span></span>

    <span data-ttu-id="750c6-184">c.</span><span class="sxs-lookup"><span data-stu-id="750c6-184">c.</span></span> <span data-ttu-id="750c6-185">Az a **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** Salesforce védőfal elsősorban a könyvtárhoz hozzáadandó esetén.</span><span class="sxs-lookup"><span data-stu-id="750c6-185">In the **Entity Id** textbox, type **https://test.salesforce.com** if it is the first Salesforce Sandbox instance that you are adding to your directory.</span></span> <span data-ttu-id="750c6-186">Ha már van egy példánya Salesforce védőfal, majd a a **Entitásazonosító** írja be a **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="750c6-186">If you have already added an instance of Salesforce Sandbox, then for the **Entity ID** type in the **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="750c6-187">d.</span><span class="sxs-lookup"><span data-stu-id="750c6-187">d.</span></span> <span data-ttu-id="750c6-188">Kattintson a **Tallózás** a letöltött tanúsítvány feltöltése.</span><span class="sxs-lookup"><span data-stu-id="750c6-188">Click **Browse** to upload the downloaded certificate.</span></span>  

    <span data-ttu-id="750c6-189">e.</span><span class="sxs-lookup"><span data-stu-id="750c6-189">e.</span></span> <span data-ttu-id="750c6-190">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz az összevonási azonosító felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="750c6-190">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
 
    <span data-ttu-id="750c6-191">f.</span><span class="sxs-lookup"><span data-stu-id="750c6-191">f.</span></span> <span data-ttu-id="750c6-192">Mint **SAML-alapú identitás hely**, jelölje be **identitás a tulajdonos utasítás NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="750c6-192">As **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>

    <span data-ttu-id="750c6-193">g.</span><span class="sxs-lookup"><span data-stu-id="750c6-193">g.</span></span> <span data-ttu-id="750c6-194">Beillesztés **egyszeri bejelentkezési URL-címe** azokat a **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="750c6-194">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="750c6-195">h.</span><span class="sxs-lookup"><span data-stu-id="750c6-195">h.</span></span> <span data-ttu-id="750c6-196">SFDC nem támogatja az SAML jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="750c6-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="750c6-197">A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" be azt a **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="750c6-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="750c6-198">i.</span><span class="sxs-lookup"><span data-stu-id="750c6-198">i.</span></span> <span data-ttu-id="750c6-199">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="750c6-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="750c6-200">j.</span><span class="sxs-lookup"><span data-stu-id="750c6-200">j.</span></span> <span data-ttu-id="750c6-201">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="750c6-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="750c6-202">A tartomány</span><span class="sxs-lookup"><span data-stu-id="750c6-202">Enable your domain</span></span>
<span data-ttu-id="750c6-203">Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="750c6-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="750c6-204">További információkért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="750c6-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="750c6-205">**Ahhoz, hogy a tartomány, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="750c6-205">**To enable your domain, perform the following steps:**</span></span>

1. <span data-ttu-id="750c6-206">A bal oldali navigációs ablaktáblán kattintson **tartományok**, és kattintson a **saját tartomány.**</span><span class="sxs-lookup"><span data-stu-id="750c6-206">In the left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="750c6-208">Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="750c6-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="750c6-209">Az a **bejelentkezési oldal beállításainak** területen kattintson **szerkesztése**, majd, mint **hitelesítési szolgáltatás**, válassza ki a nevét a SAML egyszeri bejelentkezés beállítása a fenti szakaszban leírt, és végül kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="750c6-209">In the **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select the name of the SAML Single Sign-On Setting from the previous section, and finally click **Save**.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="750c6-211">Amint egy tartományhoz, konfigurálva van, a felhasználók használjon bejelentkezni a Salesforce védőfal tartomány URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="750c6-211">As soon as you have a domain configured, your users should use the domain URL to login to the Salesforce sandbox.</span></span>  

<span data-ttu-id="750c6-212">Ahhoz, hogy az URL-cím értékét, kattintson az előző szakaszban létrehozott SSO profilra.</span><span class="sxs-lookup"><span data-stu-id="750c6-212">To get the value of the URL, click the SSO profile you have created in the previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="750c6-213">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="750c6-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="750c6-214">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="750c6-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="750c6-215">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="750c6-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="750c6-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="750c6-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="750c6-217">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="750c6-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="750c6-219">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="750c6-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="750c6-220">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="750c6-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="750c6-222">Megjelenítendő felhasználót tartalmazó listát Ugrás **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="750c6-222">to display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="750c6-224">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="750c6-224">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="750c6-226">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="750c6-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="750c6-228">a.</span><span class="sxs-lookup"><span data-stu-id="750c6-228">a.</span></span> <span data-ttu-id="750c6-229">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="750c6-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="750c6-230">b.</span><span class="sxs-lookup"><span data-stu-id="750c6-230">b.</span></span> <span data-ttu-id="750c6-231">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="750c6-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="750c6-232">c.</span><span class="sxs-lookup"><span data-stu-id="750c6-232">c.</span></span> <span data-ttu-id="750c6-233">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="750c6-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="750c6-234">d.</span><span class="sxs-lookup"><span data-stu-id="750c6-234">d.</span></span> <span data-ttu-id="750c6-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="750c6-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="750c6-236">A Salesforce védőfal tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="750c6-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="750c6-237">Ebben a szakaszban egy felhasználó Britta Simon nevű Salesforce védőfal jön létre.</span><span class="sxs-lookup"><span data-stu-id="750c6-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="750c6-238">Salesforce védőfal támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="750c6-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="750c6-239">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="750c6-239">There is no action item for you in this section.</span></span> <span data-ttu-id="750c6-240">Ha a felhasználó nem létezik a Salesforce védőfal, egy új létrejön a Salesforce védőfal elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="750c6-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt to access Salesforce Sandbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="750c6-241">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="750c6-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="750c6-242">Ebben a szakaszban Britta Simon hozzáférést biztosít a Salesforce védőfal által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="750c6-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce Sandbox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="750c6-244">**Britta Simon hozzárendelése Salesforce védőfal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="750c6-244">**To assign Britta Simon to Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="750c6-245">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="750c6-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="750c6-247">Az alkalmazások listában válassza ki a **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="750c6-247">In the applications list, select **Salesforce Sandbox**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="750c6-249">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="750c6-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="750c6-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="750c6-251">Click **Add** button.</span></span> <span data-ttu-id="750c6-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="750c6-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="750c6-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="750c6-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="750c6-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="750c6-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="750c6-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="750c6-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="750c6-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="750c6-257">Testing single sign-on</span></span>

<span data-ttu-id="750c6-258">Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="750c6-258">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="750c6-259">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="750c6-259">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="750c6-260">További források</span><span class="sxs-lookup"><span data-stu-id="750c6-260">Additional resources</span></span>

* [<span data-ttu-id="750c6-261">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="750c6-261">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="750c6-262">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="750c6-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="750c6-263">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="750c6-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

