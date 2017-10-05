---
title: "Oktatóanyag: Azure Active Directoryval integrált InsideView |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és InsideView között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: f2b0a1d4bc44f8d0cd57c61e2d78950cb6a99854
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a><span data-ttu-id="c1abd-103">Oktatóanyag: Azure Active Directoryval integrált InsideView</span><span class="sxs-lookup"><span data-stu-id="c1abd-103">Tutorial: Azure Active Directory integration with InsideView</span></span>

<span data-ttu-id="c1abd-104">Ebben az oktatóanyagban elsajátíthatja InsideView integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1abd-104">In this tutorial, you learn how to integrate InsideView with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1abd-105">InsideView integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c1abd-105">Integrating InsideView with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c1abd-106">Megadhatja a InsideView hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c1abd-106">You can control in Azure AD who has access to InsideView</span></span>
- <span data-ttu-id="c1abd-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett InsideView (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c1abd-107">You can enable your users to automatically get signed-on to InsideView (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c1abd-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c1abd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c1abd-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1abd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1abd-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1abd-110">Prerequisites</span></span>

<span data-ttu-id="c1abd-111">Konfigurálása az Azure AD-integrációs InsideView, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c1abd-111">To configure Azure AD integration with InsideView, you need the following items:</span></span>

- <span data-ttu-id="c1abd-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c1abd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1abd-113">Egy InsideView egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c1abd-113">A InsideView single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1abd-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c1abd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1abd-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c1abd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1abd-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c1abd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1abd-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1abd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1abd-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c1abd-118">Scenario description</span></span>
<span data-ttu-id="c1abd-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c1abd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1abd-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c1abd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1abd-121">A gyűjteményből InsideView hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1abd-121">Adding InsideView from the gallery</span></span>
2. <span data-ttu-id="c1abd-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c1abd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insideview-from-the-gallery"></a><span data-ttu-id="c1abd-123">A gyűjteményből InsideView hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c1abd-123">Adding InsideView from the gallery</span></span>
<span data-ttu-id="c1abd-124">InsideView az Azure ad integrálása konfigurálásához szüksége InsideView hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="c1abd-124">To configure the integration of InsideView in to Azure AD, you need to add InsideView from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c1abd-125">**A gyűjteményből InsideView hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1abd-125">**To add InsideView from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c1abd-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c1abd-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c1abd-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c1abd-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c1abd-133">Írja be a keresőmezőbe, **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-133">In the search box, type **InsideView**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. <span data-ttu-id="c1abd-135">Az eredmények panelen válassza ki a **InsideView**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c1abd-135">In the results panel, select **InsideView**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c1abd-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c1abd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c1abd-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján InsideView</span><span class="sxs-lookup"><span data-stu-id="c1abd-138">In this section, you configure and test Azure AD single sign-on with InsideView based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c1abd-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó InsideView a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c1abd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in InsideView is to a user in Azure AD.</span></span> <span data-ttu-id="c1abd-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a InsideView közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c1abd-140">In other words, a link relationship between an Azure AD user and the related user in InsideView needs to be established.</span></span>

<span data-ttu-id="c1abd-141">InsideView, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c1abd-141">In InsideView, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c1abd-142">Az Azure AD egyszeri bejelentkezést a InsideView tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c1abd-142">To configure and test Azure AD single sign-on with InsideView, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c1abd-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c1abd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c1abd-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c1abd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1abd-145">**[InsideView tesztfelhasználó létrehozása](#creating-a-insideview-test-user)**  - való Britta Simon valami InsideView, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c1abd-145">**[Creating a InsideView test user](#creating-a-insideview-test-user)** - to have a counterpart of Britta Simon in InsideView that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1abd-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c1abd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1abd-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c1abd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c1abd-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1abd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c1abd-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az InsideView alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c1abd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your InsideView application.</span></span>

<span data-ttu-id="c1abd-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés InsideView, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1abd-150">**To configure Azure AD single sign-on with InsideView, perform the following steps:**</span></span>

1. <span data-ttu-id="c1abd-151">Az Azure portálon a a **InsideView** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-151">In the Azure portal, on the **InsideView** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c1abd-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c1abd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. <span data-ttu-id="c1abd-155">Az a **InsideView tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c1abd-155">On the **InsideView Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    <span data-ttu-id="c1abd-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://my.insideview.com/iv/<STS Name>/login.iv`</span><span class="sxs-lookup"><span data-stu-id="c1abd-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://my.insideview.com/iv/<STS Name>/login.iv`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c1abd-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c1abd-158">This value is not real.</span></span> <span data-ttu-id="c1abd-159">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c1abd-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="c1abd-160">Ügyfél [InsideView támogatási csoport ](mailto:support@insideview.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c1abd-160">Contact [InsideView support team ](mailto:support@insideview.com) to get this value.</span></span>
 
4. <span data-ttu-id="c1abd-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c1abd-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. <span data-ttu-id="c1abd-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1abd-165">A a **InsideView konfigurációs** kattintson **konfigurálása InsideView** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c1abd-165">On the **InsideView Configuration** section, click **Configure InsideView** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c1abd-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c1abd-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. <span data-ttu-id="c1abd-168">Egy másik webes böngészőablakban jelentkezzen be a InsideView vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c1abd-168">In a different web browser window, log in to your InsideView company site as an administrator.</span></span>

8. <span data-ttu-id="c1abd-169">A felső eszköztáron kattintson **Admin**, **SingleSignOn beállítások**, és kattintson a **hozzáadása SAML**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-169">In the toolbar on the top, click **Admin**, **SingleSignOn Settings**, and then click **Add SAML**.</span></span>
   
   <span data-ttu-id="c1abd-170">![SAML-alapú egyszeri bejelentkezési beállítások](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML-alapú egyszeri bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="c1abd-170">![SAML Single Sign On Settings](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML Single Sign On Settings")</span></span>

9. <span data-ttu-id="c1abd-171">Az a **hozzáadása egy új SAML** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c1abd-171">In the **Add a New SAML** section, perform the following steps:</span></span>

    <span data-ttu-id="c1abd-172">![Adja hozzá egy új SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "egy új SAML hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="c1abd-172">![Add a New SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "Add a New SAML")</span></span>
   
    <span data-ttu-id="c1abd-173">a.</span><span class="sxs-lookup"><span data-stu-id="c1abd-173">a.</span></span> <span data-ttu-id="c1abd-174">Az a **STS neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="c1abd-174">In the **STS Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="c1abd-175">b.</span><span class="sxs-lookup"><span data-stu-id="c1abd-175">b.</span></span> <span data-ttu-id="c1abd-176">A **SamlP/WS-Fed kéretlen végpont** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="c1abd-176">In **SamlP/WS-Fed Unsolicited EndPoint** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="c1abd-177">c.</span><span class="sxs-lookup"><span data-stu-id="c1abd-177">c.</span></span> <span data-ttu-id="c1abd-178">A base-64 kódolású tanúsítványt, amely az Azure-portálról letöltött, nyissa meg a tartalmának másolása a vágólapra és illessze be azt a **STS tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="c1abd-178">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **STS Certificate** textbox.</span></span>

    <span data-ttu-id="c1abd-179">d.</span><span class="sxs-lookup"><span data-stu-id="c1abd-179">d.</span></span> <span data-ttu-id="c1abd-180">Az a **Crm felhasználói azonosító leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="c1abd-180">In the **Crm User Id Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
        
    <span data-ttu-id="c1abd-181">e.</span><span class="sxs-lookup"><span data-stu-id="c1abd-181">e.</span></span> <span data-ttu-id="c1abd-182">Az a **Crm E-mail leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="c1abd-182">In the **Crm Email Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="c1abd-183">f.</span><span class="sxs-lookup"><span data-stu-id="c1abd-183">f.</span></span> <span data-ttu-id="c1abd-184">Az a **Crm első neve Mapping** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="c1abd-184">In the **Crm First Name Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="c1abd-185">g.</span><span class="sxs-lookup"><span data-stu-id="c1abd-185">g.</span></span> <span data-ttu-id="c1abd-186">Az a **Crm Vezetéknév leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="c1abd-186">In the **Crm lastName Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>  

    <span data-ttu-id="c1abd-187">h.</span><span class="sxs-lookup"><span data-stu-id="c1abd-187">h.</span></span> <span data-ttu-id="c1abd-188">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c1abd-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c1abd-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c1abd-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c1abd-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c1abd-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1abd-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c1abd-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1abd-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="c1abd-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c1abd-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c1abd-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1abd-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c1abd-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c1abd-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c1abd-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c1abd-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c1abd-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c1abd-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c1abd-204">a.</span><span class="sxs-lookup"><span data-stu-id="c1abd-204">a.</span></span> <span data-ttu-id="c1abd-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1abd-206">b.</span><span class="sxs-lookup"><span data-stu-id="c1abd-206">b.</span></span> <span data-ttu-id="c1abd-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c1abd-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c1abd-208">c.</span><span class="sxs-lookup"><span data-stu-id="c1abd-208">c.</span></span> <span data-ttu-id="c1abd-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c1abd-210">d.</span><span class="sxs-lookup"><span data-stu-id="c1abd-210">d.</span></span> <span data-ttu-id="c1abd-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-211">Click **Create**.</span></span>
 
### <a name="creating-a-insideview-test-user"></a><span data-ttu-id="c1abd-212">InsideView tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1abd-212">Creating a InsideView test user</span></span>

<span data-ttu-id="c1abd-213">Ahhoz, hogy az Azure AD-felhasználók InsideView bejelentkezni, akkor ki kell építenie a InsideView.</span><span class="sxs-lookup"><span data-stu-id="c1abd-213">To enable Azure AD users to log in to InsideView, they must be provisioned in to InsideView.</span></span> <span data-ttu-id="c1abd-214">InsideView, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c1abd-214">In the case of InsideView, provisioning is a manual task.</span></span>

<span data-ttu-id="c1abd-215">Ahhoz, hogy a felhasználók vagy InsideView létrehozott contacts, lépjen kapcsolatba [InsideView támogatási csoport](mailto:support@insideview.com).</span><span class="sxs-lookup"><span data-stu-id="c1abd-215">To get users or contacts created in InsideView, Contact [InsideView support team](mailto:support@insideview.com).</span></span>

>[!NOTE]
><span data-ttu-id="c1abd-216">Bármely más InsideView felhasználói fiók létrehozása eszközök vagy InsideView kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="c1abd-216">You can use any other InsideView user account creation tools or APIs provided by InsideView to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c1abd-217">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c1abd-217">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c1abd-218">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés InsideView Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c1abd-218">In this section, you enable Britta Simon to use Azure single sign-on by granting access to InsideView.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c1abd-220">**Britta Simon hozzárendelése InsideView, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c1abd-220">**To assign Britta Simon to InsideView, perform the following steps:**</span></span>

1. <span data-ttu-id="c1abd-221">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-221">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c1abd-223">Az alkalmazások listában válassza ki a **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-223">In the applications list, select **InsideView**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. <span data-ttu-id="c1abd-225">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c1abd-225">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c1abd-227">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1abd-227">Click **Add** button.</span></span> <span data-ttu-id="c1abd-228">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1abd-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c1abd-230">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c1abd-230">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c1abd-231">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1abd-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1abd-232">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c1abd-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c1abd-233">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c1abd-233">Testing single sign-on</span></span>

<span data-ttu-id="c1abd-234">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c1abd-234">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c1abd-235">Ha a hozzáférési panelen InsideView csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az InsideView alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c1abd-235">When you click the InsideView tile in the Access Panel, you should get automatically signed-on to your InsideView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1abd-236">További források</span><span class="sxs-lookup"><span data-stu-id="c1abd-236">Additional resources</span></span>

* [<span data-ttu-id="c1abd-237">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c1abd-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1abd-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c1abd-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

