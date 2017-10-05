---
title: "Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az Adobe bejelentkezési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b413772de1af1fbb128d29b81e5831cfd6a39ab4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="4c490-103">Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="4c490-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="4c490-104">Ebben az oktatóanyagban elsajátíthatja Adobe bejelentkezési integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c490-104">In this tutorial, you learn how to integrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4c490-105">Az Adobe bejelentkezési integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="4c490-105">Integrating Adobe Sign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4c490-106">Szabályozhatja, aki hozzáfér az Adobe bejelentkezési Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c490-106">You can control in Azure AD who has access to Adobe Sign</span></span>
- <span data-ttu-id="4c490-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Adobe bejelentkezéshez (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="4c490-107">You can enable your users to automatically get signed-on to Adobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c490-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4c490-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4c490-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c490-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c490-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c490-110">Prerequisites</span></span>

<span data-ttu-id="4c490-111">Az Azure AD-integráció konfigurálása Adobe bejelentkezési, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4c490-111">To configure Azure AD integration with Adobe Sign, you need the following items:</span></span>

- <span data-ttu-id="4c490-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c490-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4c490-113">Az Adobe bejelentkezési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4c490-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4c490-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c490-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4c490-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="4c490-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c490-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c490-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4c490-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c490-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c490-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c490-118">Scenario description</span></span>
<span data-ttu-id="4c490-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c490-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4c490-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c490-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c490-121">Az Adobe bejelentkezési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c490-121">Adding Adobe Sign from the gallery</span></span>
2. <span data-ttu-id="4c490-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c490-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-the-gallery"></a><span data-ttu-id="4c490-123">Az Adobe bejelentkezési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c490-123">Adding Adobe Sign from the gallery</span></span>
<span data-ttu-id="4c490-124">Az Azure AD integrálása a Adobe bejelentkezési konfigurálásához kell hozzáadnia az Adobe bejelentkezési a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="4c490-124">To configure the integration of Adobe Sign into Azure AD, you need to add Adobe Sign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4c490-125">**Az Adobe hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c490-125">**To add Adobe Sign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4c490-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c490-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c490-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c490-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4c490-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c490-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c490-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="4c490-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c490-133">Írja be a keresőmezőbe, **Adobe bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="4c490-133">In the search box, type **Adobe Sign**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="4c490-135">Az eredmények panelen válassza ki a **Adobe bejelentkezési**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4c490-135">In the results panel, select **Adobe Sign**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4c490-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4c490-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4c490-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Adobe bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="4c490-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4c490-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Adobe bejelentkezési a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4c490-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adobe Sign is to a user in Azure AD.</span></span> <span data-ttu-id="4c490-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Adobe bejelentkezési közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4c490-140">In other words, a link relationship between an Azure AD user and the related user in Adobe Sign needs to be established.</span></span>

<span data-ttu-id="4c490-141">Az Adobe bejelentkezési, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4c490-141">In Adobe Sign, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4c490-142">Az Azure AD az egyszeri bejelentkezés Adobe bejelentkezési tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="4c490-142">To configure and test Azure AD single sign-on with Adobe Sign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4c490-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="4c490-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4c490-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="4c490-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c490-145">**[Az Adobe bejelentkezési tesztfelhasználó létrehozása](#creating-an-adobe-sign-test-user)**  - való egy megfelelője a Britta Simon Adobe bejelentkezési, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4c490-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - to have a counterpart of Britta Simon in Adobe Sign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4c490-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4c490-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c490-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4c490-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4c490-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c490-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4c490-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az Adobe bejelentkezési alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4c490-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="4c490-150">**Az Azure AD az egyszeri bejelentkezés Adobe bejelentkezési megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c490-150">**To configure Azure AD single sign-on with Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="4c490-151">Az Azure portálon a a **Adobe bejelentkezési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c490-151">In the Azure portal, on the **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c490-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4c490-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="4c490-155">Az a **Adobe bejelentkezési tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4c490-155">On the **Adobe Sign Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="4c490-157">a.</span><span class="sxs-lookup"><span data-stu-id="4c490-157">a.</span></span> <span data-ttu-id="4c490-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="4c490-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="4c490-159">b.</span><span class="sxs-lookup"><span data-stu-id="4c490-159">b.</span></span> <span data-ttu-id="4c490-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="4c490-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4c490-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4c490-161">These values are not real.</span></span> <span data-ttu-id="4c490-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4c490-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4c490-163">Ügyfél [Adobe bejelentkezési ügyfél-támogatási csoport](https://helpx.adobe.com/in/contact/support.html) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4c490-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) to get these values.</span></span> 
 
4. <span data-ttu-id="4c490-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c490-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="4c490-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c490-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4c490-168">A a **Adobe bejelentkezési konfigurációs** kattintson **konfigurálása Adobe bejelentkezési** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4c490-168">On the **Adobe Sign Configuration** section, click **Configure Adobe Sign** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4c490-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4c490-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="4c490-171">Egy másik webes böngészőablakban jelentkezzen be a Adobe bejelentkezési vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c490-171">In a different web browser window, log in to your Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="4c490-172">A felső menüben kattintson a **fiók**, és a bal oldali navigációs ablakában kattintson az **SAML beállítások** alatt **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="4c490-172">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="4c490-173">![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="4c490-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="4c490-174">A SAML-beállítások területen a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4c490-174">In the SAML Settings section, perform the following steps:</span></span>
   
   <span data-ttu-id="4c490-175">![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="4c490-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="4c490-176">a.</span><span class="sxs-lookup"><span data-stu-id="4c490-176">a.</span></span> <span data-ttu-id="4c490-177">Mint **SAML mód**, jelölje be **SAML kötelező**.</span><span class="sxs-lookup"><span data-stu-id="4c490-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="4c490-178">b.</span><span class="sxs-lookup"><span data-stu-id="4c490-178">b.</span></span> <span data-ttu-id="4c490-179">Válassza ki **EchoSign Fiókrendszergazdák engedélyezése a EchoSign hitelesítő adataival bejelentkezni**.</span><span class="sxs-lookup"><span data-stu-id="4c490-179">Select **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="4c490-180">c.</span><span class="sxs-lookup"><span data-stu-id="4c490-180">c.</span></span> <span data-ttu-id="4c490-181">Mint **felhasználó létrehozása**, jelölje be **keresztül SAML hitelesített felhasználók automatikus hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4c490-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="4c490-182">Helyezze át, amely, a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="4c490-182">Move on, performing the following steps:</span></span>

       <span data-ttu-id="4c490-183">![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-beállítások")</span><span class="sxs-lookup"><span data-stu-id="4c490-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="4c490-184">a.</span><span class="sxs-lookup"><span data-stu-id="4c490-184">a.</span></span> <span data-ttu-id="4c490-185">Beillesztés **SAML Entitásazonosító**, amely az Azure portálról másolta a **IdP Entitásazonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c490-185">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="4c490-186">b.</span><span class="sxs-lookup"><span data-stu-id="4c490-186">b.</span></span> <span data-ttu-id="4c490-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure portálról másolta a **IdP bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c490-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into the **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="4c490-188">c.</span><span class="sxs-lookup"><span data-stu-id="4c490-188">c.</span></span> <span data-ttu-id="4c490-189">Beillesztés **Sign-Out URL-cím**, amely az Azure portálról másolta a **IdP kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="4c490-189">Paste **Sign-Out URL**, which you have copied from Azure portal into the **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="4c490-190">d.</span><span class="sxs-lookup"><span data-stu-id="4c490-190">d.</span></span> <span data-ttu-id="4c490-191">Nyissa meg a letöltött **Certificate(Base64)** fájlt a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **IdP tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="4c490-191">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Certificate** textbox</span></span>

    <span data-ttu-id="4c490-192">e.</span><span class="sxs-lookup"><span data-stu-id="4c490-192">e.</span></span> <span data-ttu-id="4c490-193">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="4c490-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="4c490-194">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="4c490-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4c490-195">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="4c490-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4c490-196">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4c490-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4c490-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c490-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="4c490-198">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="4c490-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c490-200">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c490-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4c490-201">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c490-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c490-203">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c490-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c490-205">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="4c490-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c490-207">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4c490-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4c490-209">a.</span><span class="sxs-lookup"><span data-stu-id="4c490-209">a.</span></span> <span data-ttu-id="4c490-210">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c490-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4c490-211">b.</span><span class="sxs-lookup"><span data-stu-id="4c490-211">b.</span></span> <span data-ttu-id="4c490-212">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c490-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4c490-213">c.</span><span class="sxs-lookup"><span data-stu-id="4c490-213">c.</span></span> <span data-ttu-id="4c490-214">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c490-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4c490-215">d.</span><span class="sxs-lookup"><span data-stu-id="4c490-215">d.</span></span> <span data-ttu-id="4c490-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c490-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="4c490-217">Az Adobe bejelentkezési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c490-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="4c490-218">Ahhoz, hogy az Azure AD-felhasználók Adobe bejelentkezési bejelentkezni, akkor ki kell építenie az Adobe jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="4c490-218">To enable Azure AD users to log in to Adobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="4c490-219">Adobe bejelentkezési, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="4c490-219">In the case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="4c490-220">Bármely más Adobe bejelentkezési felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Adobe bejelentkezési által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="4c490-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign to provision AAD user accounts.</span></span> 

<span data-ttu-id="4c490-221">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c490-221">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="4c490-222">Jelentkezzen be a **Adobe bejelentkezési** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4c490-222">Log in to your **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="4c490-223">A felső menüben kattintson **fiók**, és a bal oldali navigációs ablakában kattintson az **felhasználók és csoportok**, majd kattintson a **hozzon létre egy új felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="4c490-223">In the menu on the top, click **Account**, and then, in the navigation pane on the left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="4c490-224">![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "fiók")</span><span class="sxs-lookup"><span data-stu-id="4c490-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="4c490-225">Az a **új felhasználó létrehozása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4c490-225">In the **Create New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="4c490-226">![Hozzon létre felhasználói](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="4c490-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="4c490-227">a.</span><span class="sxs-lookup"><span data-stu-id="4c490-227">a.</span></span> <span data-ttu-id="4c490-228">Típus a **E-mail cím**, **Utónév**, és **Vezetéknév** szeretné azokat a kapcsolódó szövegmezők rendelkezés érvényes AAD-fiók.</span><span class="sxs-lookup"><span data-stu-id="4c490-228">Type the **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="4c490-229">b.</span><span class="sxs-lookup"><span data-stu-id="4c490-229">b.</span></span> <span data-ttu-id="4c490-230">Kattintson a **létrehozza a felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4c490-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="4c490-231">Az Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmaz egy hivatkozást a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="4c490-231">The Azure Active Directory account holder receives an email that includes a link to confirm the account before it becomes active.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4c490-232">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4c490-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4c490-233">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Adobe bejelentkezési Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="4c490-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adobe Sign.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c490-235">**Az Adobe bejelentkezési Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4c490-235">**To assign Britta Simon to Adobe Sign, perform the following steps:**</span></span>

1. <span data-ttu-id="4c490-236">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c490-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c490-238">Az alkalmazások listában válassza ki a **Adobe bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="4c490-238">In the applications list, select **Adobe Sign**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="4c490-240">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c490-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c490-242">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c490-242">Click **Add** button.</span></span> <span data-ttu-id="4c490-243">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c490-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c490-245">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c490-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4c490-246">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c490-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c490-247">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c490-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4c490-248">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c490-248">Testing single sign-on</span></span>

<span data-ttu-id="4c490-249">Ha a hozzáférési panelen Adobe bejelentkezési csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Adobe bejelentkezési alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4c490-249">When you click the Adobe Sign tile in the Access Panel, you should get automatically signed-on to your Adobe Sign application.</span></span>
<span data-ttu-id="4c490-250">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4c490-250">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4c490-251">További források</span><span class="sxs-lookup"><span data-stu-id="4c490-251">Additional resources</span></span>

* [<span data-ttu-id="4c490-252">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="4c490-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c490-253">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c490-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

