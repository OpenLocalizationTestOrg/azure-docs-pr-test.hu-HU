---
title: "Oktatóanyag: Azure Active Directory-integráció adaptív Suite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és adaptív Suite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 5d7ba2f4c7d814e3aaa1bf804ddc5030380ccb2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="474a3-103">Oktatóanyag: Azure Active Directory-integráció adaptív csomaggal</span><span class="sxs-lookup"><span data-stu-id="474a3-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="474a3-104">Ebben az oktatóanyagban elsajátíthatja adaptív Suite integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="474a3-104">In this tutorial, you learn how to integrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="474a3-105">Adaptív Suite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="474a3-105">Integrating Adaptive Suite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="474a3-106">Megadhatja a adaptív Suite hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="474a3-106">You can control in Azure AD who has access to Adaptive Suite</span></span>
- <span data-ttu-id="474a3-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az adaptív Suite (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="474a3-107">You can enable your users to automatically get signed-on to Adaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="474a3-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="474a3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="474a3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="474a3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="474a3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="474a3-110">Prerequisites</span></span>

<span data-ttu-id="474a3-111">Az Azure AD-integráció konfigurálása adaptív csomaggal, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="474a3-111">To configure Azure AD integration with Adaptive Suite, you need the following items:</span></span>

- <span data-ttu-id="474a3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="474a3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="474a3-113">Egy adaptív Suite egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="474a3-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="474a3-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="474a3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="474a3-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="474a3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="474a3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="474a3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="474a3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="474a3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="474a3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="474a3-118">Scenario description</span></span>
<span data-ttu-id="474a3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="474a3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="474a3-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="474a3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="474a3-121">Adaptív csomag hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="474a3-121">Adding Adaptive Suite from the gallery</span></span>
2. <span data-ttu-id="474a3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="474a3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-the-gallery"></a><span data-ttu-id="474a3-123">Adaptív csomag hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="474a3-123">Adding Adaptive Suite from the gallery</span></span>
<span data-ttu-id="474a3-124">Az Azure AD integrálása a adaptív Suite konfigurálásához kell hozzáadnia adaptív Suite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="474a3-124">To configure the integration of Adaptive Suite into Azure AD, you need to add Adaptive Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="474a3-125">**A gyűjteményből adaptív Suite hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="474a3-125">**To add Adaptive Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="474a3-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="474a3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="474a3-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="474a3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="474a3-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="474a3-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="474a3-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="474a3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="474a3-133">Írja be a keresőmezőbe, **adaptív Suite**.</span><span class="sxs-lookup"><span data-stu-id="474a3-133">In the search box, type **Adaptive Suite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="474a3-135">Az eredmények panelen válassza ki a **adaptív Suite**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="474a3-135">In the results panel, select **Adaptive Suite**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="474a3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="474a3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="474a3-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján adaptív csomaggal</span><span class="sxs-lookup"><span data-stu-id="474a3-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="474a3-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó adaptív csomagban található a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="474a3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adaptive Suite is to a user in Azure AD.</span></span> <span data-ttu-id="474a3-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó adaptív csomagban található hivatkozás kapcsolatának kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="474a3-140">In other words, a link relationship between an Azure AD user and the related user in Adaptive Suite needs to be established.</span></span>

<span data-ttu-id="474a3-141">Adaptív Suite, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="474a3-141">In Adaptive Suite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="474a3-142">Az Azure AD az egyszeri bejelentkezés adaptív Suite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="474a3-142">To configure and test Azure AD single sign-on with Adaptive Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="474a3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="474a3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="474a3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="474a3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="474a3-145">**[Egy adaptív Suite tesztfelhasználó létrehozása](#creating-an-adaptive-suite-test-user)**  – egy megfelelője a Britta Simon rendelkezik, amely csatolva van a felhasználó az Azure AD-ábrázolását adaptív csomagban található.</span><span class="sxs-lookup"><span data-stu-id="474a3-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - to have a counterpart of Britta Simon in Adaptive Suite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="474a3-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="474a3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="474a3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="474a3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="474a3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="474a3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="474a3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az adaptív Suite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="474a3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="474a3-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés adaptív csomaggal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="474a3-150">**To configure Azure AD single sign-on with Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="474a3-151">Az Azure portálon a a **adaptív Suite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="474a3-151">In the Azure portal, on the **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="474a3-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="474a3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="474a3-155">Az a **adaptív Suite tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="474a3-155">On the **Adaptive Suite Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="474a3-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="474a3-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="474a3-158">Ez az érték lekérheti az adaptív Suite **SAML SSO beállítások** lap.</span><span class="sxs-lookup"><span data-stu-id="474a3-158">You can get this value from the Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="474a3-159">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="474a3-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="474a3-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="474a3-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="474a3-163">A a **adaptív Suite konfigurációs** kattintson **adaptív Suite konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="474a3-163">On the **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="474a3-164">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="474a3-164">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="474a3-166">Egy másik webes böngészőablakban jelentkezzen be a adaptív Suite vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="474a3-166">In a different web browser window, log in to your Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="474a3-167">Ugrás a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="474a3-167">Go to **Admin**.</span></span>
   
    <span data-ttu-id="474a3-168">![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="474a3-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="474a3-169">Az a **felhasználók és szerepkörök** kattintson **SAML SSO beállítások kezelése**.</span><span class="sxs-lookup"><span data-stu-id="474a3-169">In the **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="474a3-170">![SAML SSO beállításainak kezelése](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "SAML SSO beállításainak kezelése")</span><span class="sxs-lookup"><span data-stu-id="474a3-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="474a3-171">Az a **SAML SSO beállítások** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="474a3-171">On the **SAML SSO Settings** page, perform the following steps:</span></span>
   
    <span data-ttu-id="474a3-172">![SAML SSO beállítások](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-beállítások")</span><span class="sxs-lookup"><span data-stu-id="474a3-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="474a3-173">a.</span><span class="sxs-lookup"><span data-stu-id="474a3-173">a.</span></span> <span data-ttu-id="474a3-174">Az a **identitás szolgáltatónevet** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="474a3-174">In the **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="474a3-175">b.</span><span class="sxs-lookup"><span data-stu-id="474a3-175">b.</span></span> <span data-ttu-id="474a3-176">Beillesztés a **SAML Entitásazonosító** az Azure portálon átmásolja értéket a **identitásszolgáltató Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="474a3-176">Paste the **SAML Entity ID** value copied from Azure portal into the **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="474a3-177">c.</span><span class="sxs-lookup"><span data-stu-id="474a3-177">c.</span></span> <span data-ttu-id="474a3-178">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure portálon átmásolja értéket a **identitásszolgáltató egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="474a3-178">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="474a3-179">d.</span><span class="sxs-lookup"><span data-stu-id="474a3-179">d.</span></span> <span data-ttu-id="474a3-180">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure portálon átmásolja értéket a **egyéni kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="474a3-180">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="474a3-181">e.</span><span class="sxs-lookup"><span data-stu-id="474a3-181">e.</span></span> <span data-ttu-id="474a3-182">A letöltött tanúsítvány feltöltése, kattintson a **fájl kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="474a3-182">To upload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="474a3-183">f.</span><span class="sxs-lookup"><span data-stu-id="474a3-183">f.</span></span> <span data-ttu-id="474a3-184">Válassza ki a következőket:</span><span class="sxs-lookup"><span data-stu-id="474a3-184">Select the following, for:</span></span>
    * <span data-ttu-id="474a3-185">**SAML-alapú felhasználói azonosító**, jelölje be **adaptív Insights felhasználónevét**.</span><span class="sxs-lookup"><span data-stu-id="474a3-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="474a3-186">**SAML-alapú felhasználói azonosító hely**, jelölje be **felhasználóazonosító tárgyában csupa kisbetűvel NameID**.</span><span class="sxs-lookup"><span data-stu-id="474a3-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="474a3-187">**SAML NameID formátum**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="474a3-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="474a3-188">**SAML engedélyezése**, jelölje be **SAML SSO engedélyezése és a közvetlen adaptív Insights bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="474a3-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="474a3-189">g.</span><span class="sxs-lookup"><span data-stu-id="474a3-189">g.</span></span> <span data-ttu-id="474a3-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="474a3-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="474a3-191">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="474a3-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="474a3-192">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="474a3-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="474a3-193">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="474a3-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="474a3-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="474a3-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="474a3-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="474a3-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="474a3-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="474a3-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="474a3-198">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="474a3-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="474a3-200">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="474a3-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="474a3-202">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="474a3-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="474a3-204">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="474a3-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="474a3-206">a.</span><span class="sxs-lookup"><span data-stu-id="474a3-206">a.</span></span> <span data-ttu-id="474a3-207">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="474a3-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="474a3-208">b.</span><span class="sxs-lookup"><span data-stu-id="474a3-208">b.</span></span> <span data-ttu-id="474a3-209">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="474a3-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="474a3-210">c.</span><span class="sxs-lookup"><span data-stu-id="474a3-210">c.</span></span> <span data-ttu-id="474a3-211">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="474a3-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="474a3-212">d.</span><span class="sxs-lookup"><span data-stu-id="474a3-212">d.</span></span> <span data-ttu-id="474a3-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="474a3-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="474a3-214">Egy adaptív Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="474a3-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="474a3-215">Ahhoz, hogy az Azure AD-felhasználók adaptív Suite bejelentkezni, akkor ki kell építenie az adaptív Suite.</span><span class="sxs-lookup"><span data-stu-id="474a3-215">To enable Azure AD users to log in to Adaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="474a3-216">Adaptív Suite, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="474a3-216">In the case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="474a3-217">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="474a3-217">**To configure user provisioning, perform the following steps:**</span></span> 

1. <span data-ttu-id="474a3-218">Jelentkezzen be a **adaptív Suite** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="474a3-218">Log in to your **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="474a3-219">Ugrás a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="474a3-219">Go to **Admin**.</span></span>
   
   <span data-ttu-id="474a3-220">![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="474a3-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="474a3-221">Az a **felhasználók és szerepkörök** kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="474a3-221">In the **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="474a3-222">![Felhasználó hozzáadása](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="474a3-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="474a3-223">Az a **új felhasználó** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="474a3-223">In the **New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="474a3-224">![Küldje el](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "elküldése")</span><span class="sxs-lookup"><span data-stu-id="474a3-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="474a3-225">a.</span><span class="sxs-lookup"><span data-stu-id="474a3-225">a.</span></span> <span data-ttu-id="474a3-226">Típus a **neve**, **bejelentkezési**, **E-mail**, **jelszó** egy érvényes Azure Active Directory-felhasználó szeretné azokat a kapcsolódó kiépítése szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="474a3-226">Type the **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want to provision into the related textboxes.</span></span>
  
   <span data-ttu-id="474a3-227">b.</span><span class="sxs-lookup"><span data-stu-id="474a3-227">b.</span></span> <span data-ttu-id="474a3-228">Válassza ki a **szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="474a3-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="474a3-229">c.</span><span class="sxs-lookup"><span data-stu-id="474a3-229">c.</span></span> <span data-ttu-id="474a3-230">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="474a3-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="474a3-231">Bármely más adaptív Suite felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz adaptív Suite által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="474a3-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="474a3-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="474a3-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="474a3-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés adaptív Suite Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="474a3-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adaptive Suite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="474a3-235">**Adaptív Suite Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="474a3-235">**To assign Britta Simon to Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="474a3-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="474a3-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="474a3-238">Az alkalmazások listában válassza ki a **adaptív Suite**.</span><span class="sxs-lookup"><span data-stu-id="474a3-238">In the applications list, select **Adaptive Suite**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="474a3-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="474a3-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="474a3-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="474a3-242">Click **Add** button.</span></span> <span data-ttu-id="474a3-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="474a3-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="474a3-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="474a3-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="474a3-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="474a3-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="474a3-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="474a3-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="474a3-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="474a3-248">Testing single sign-on</span></span>

<span data-ttu-id="474a3-249">Ez a szakasz célja a Microsoft Azure AD az egyszeri bejelentkezés konfiguráció teszteléséhez a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="474a3-249">The objective of this section is to test your Microsoft Azure AD Single Sign-On configuration using the Access Panel.</span></span>

<span data-ttu-id="474a3-250">Ha a hozzáférési panelen adaptív Suite csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az adaptív Suite alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="474a3-250">When you click the Adaptive Suite tile in the Access Panel, you should get automatically signed-on to your Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="474a3-251">További források</span><span class="sxs-lookup"><span data-stu-id="474a3-251">Additional resources</span></span>

* [<span data-ttu-id="474a3-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="474a3-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="474a3-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="474a3-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

