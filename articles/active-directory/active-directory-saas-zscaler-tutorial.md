---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Zscaler között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 73c81691b68ee820e1d905a17b4f2ab6b6ceb5fd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="408f8-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler</span><span class="sxs-lookup"><span data-stu-id="408f8-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="408f8-104">Ebben az oktatóanyagban elsajátíthatja Zscaler integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="408f8-104">In this tutorial, you learn how to integrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="408f8-105">Zscaler integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="408f8-105">Integrating Zscaler with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="408f8-106">Megadhatja a Zscaler hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="408f8-106">You can control in Azure AD who has access to Zscaler</span></span>
- <span data-ttu-id="408f8-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Zscaler (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="408f8-107">You can enable your users to automatically get signed-on to Zscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="408f8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="408f8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="408f8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="408f8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="408f8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="408f8-110">Prerequisites</span></span>

<span data-ttu-id="408f8-111">Konfigurálása az Azure AD-integrációs Zscaler, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="408f8-111">To configure Azure AD integration with Zscaler, you need the following items:</span></span>

- <span data-ttu-id="408f8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="408f8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="408f8-113">Egy Zscaler egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="408f8-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="408f8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="408f8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="408f8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="408f8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="408f8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="408f8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="408f8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="408f8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="408f8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="408f8-118">Scenario description</span></span>
<span data-ttu-id="408f8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="408f8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="408f8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="408f8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="408f8-121">A gyűjteményből Zscaler hozzáadása</span><span class="sxs-lookup"><span data-stu-id="408f8-121">Adding Zscaler from the gallery</span></span>
2. <span data-ttu-id="408f8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="408f8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-the-gallery"></a><span data-ttu-id="408f8-123">A gyűjteményből Zscaler hozzáadása</span><span class="sxs-lookup"><span data-stu-id="408f8-123">Adding Zscaler from the gallery</span></span>
<span data-ttu-id="408f8-124">Az Azure AD integrálása a Zscaler konfigurálásához kell hozzáadnia Zscaler a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="408f8-124">To configure the integration of Zscaler into Azure AD, you need to add Zscaler from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="408f8-125">**A gyűjteményből Zscaler hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="408f8-125">**To add Zscaler from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="408f8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="408f8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="408f8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="408f8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="408f8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="408f8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="408f8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="408f8-133">Írja be a keresőmezőbe, **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="408f8-133">In the search box, type **Zscaler**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="408f8-135">Az eredmények panelen válassza ki a **Zscaler**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="408f8-135">In the results panel, select **Zscaler**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="408f8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="408f8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="408f8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zscaler.</span><span class="sxs-lookup"><span data-stu-id="408f8-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="408f8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Zscaler a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="408f8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler is to a user in Azure AD.</span></span> <span data-ttu-id="408f8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Zscaler közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="408f8-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler needs to be established.</span></span>

<span data-ttu-id="408f8-141">Zscaler, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="408f8-141">In Zscaler, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="408f8-142">Az Azure AD egyszeri bejelentkezést a Zscaler tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="408f8-142">To configure and test Azure AD single sign-on with Zscaler, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="408f8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="408f8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="408f8-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  – a Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="408f8-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="408f8-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="408f8-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="408f8-146">**[Zscaler tesztfelhasználó létrehozása](#creating-a-zscaler-test-user)**  - való Britta Simon valami Zscaler, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="408f8-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - to have a counterpart of Britta Simon in Zscaler that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="408f8-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="408f8-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="408f8-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="408f8-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="408f8-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="408f8-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="408f8-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="408f8-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="408f8-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Zscaler, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="408f8-151">**To configure Azure AD single sign-on with Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="408f8-152">Az Azure portálon a a **Zscaler** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="408f8-152">In the Azure portal, on the **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="408f8-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="408f8-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="408f8-156">Az a **Zscaler tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="408f8-156">On the **Zscaler Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="408f8-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="408f8-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="408f8-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="408f8-159">This value is not real.</span></span> <span data-ttu-id="408f8-160">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="408f8-160">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="408f8-161">Ügyfél [Zscaler ügyfél-támogatási csoport](https://www.zscaler.com/company/contact) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="408f8-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) to get this value.</span></span> 

4. <span data-ttu-id="408f8-162">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="408f8-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="408f8-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="408f8-166">A a **Zscaler konfigurációs** kattintson **konfigurálása Zscaler** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="408f8-166">On the **Zscaler Configuration** section, click **Configure Zscaler** to open **Configure sign-on** window.</span></span> <span data-ttu-id="408f8-167">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="408f8-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="408f8-169">Egy másik webes böngészőablakban jelentkezzen be a ZScaler vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="408f8-169">In a different web browser window, log in to your ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="408f8-170">Kattintson a felső menüben **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="408f8-170">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="408f8-171">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="408f8-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="408f8-172">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="408f8-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="408f8-173">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="408f8-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="408f8-174">Az a **hitelesítési beállítások kiválasztása a szervezet** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="408f8-174">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="408f8-175">![Hitelesítési](./media/active-directory-saas-zscaler-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="408f8-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="408f8-176">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-176">a.</span></span> <span data-ttu-id="408f8-177">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="408f8-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="408f8-178">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-178">b.</span></span> <span data-ttu-id="408f8-179">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="408f8-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="408f8-180">Az a **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson **kész**</span><span class="sxs-lookup"><span data-stu-id="408f8-180">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="408f8-181">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="408f8-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="408f8-182">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-182">a.</span></span> <span data-ttu-id="408f8-183">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **, amelyhez a hitelesítéshez a felhasználóknak legyenek elküldve az SAML-portál URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="408f8-183">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="408f8-184">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-184">b.</span></span> <span data-ttu-id="408f8-185">Az a **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="408f8-185">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="408f8-186">c.</span><span class="sxs-lookup"><span data-stu-id="408f8-186">c.</span></span> <span data-ttu-id="408f8-187">A letöltött tanúsítvány feltöltése, kattintson a **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="408f8-187">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="408f8-188">d.</span><span class="sxs-lookup"><span data-stu-id="408f8-188">d.</span></span> <span data-ttu-id="408f8-189">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="408f8-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="408f8-190">Az a **felhasználói hitelesítés beállítása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="408f8-190">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="408f8-191">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="408f8-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="408f8-192">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-192">a.</span></span> <span data-ttu-id="408f8-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-193">Click **Save**.</span></span>

    <span data-ttu-id="408f8-194">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-194">b.</span></span> <span data-ttu-id="408f8-195">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="408f8-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="408f8-196">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="408f8-196">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="408f8-197">A Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="408f8-197">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="408f8-198">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="408f8-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="408f8-199">Válassza ki **Internetbeállítások** a a **eszközök** menü megnyitása a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-199">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="408f8-200">![Internetbeállítások](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="408f8-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="408f8-201">Kattintson a **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="408f8-201">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="408f8-202">![Kapcsolatok](./media/active-directory-saas-zscaler-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="408f8-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="408f8-203">Kattintson a **LAN-beállítások** megnyitásához a **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-203">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="408f8-204">A Proxy server területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="408f8-204">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="408f8-205">![Proxykiszolgáló](./media/active-directory-saas-zscaler-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="408f8-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="408f8-206">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-206">a.</span></span> <span data-ttu-id="408f8-207">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="408f8-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="408f8-208">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-208">b.</span></span> <span data-ttu-id="408f8-209">Írja be a címet szövegmező **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="408f8-209">In the Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="408f8-210">c.</span><span class="sxs-lookup"><span data-stu-id="408f8-210">c.</span></span> <span data-ttu-id="408f8-211">Írja be a Port szövegmező **80**.</span><span class="sxs-lookup"><span data-stu-id="408f8-211">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="408f8-212">d.</span><span class="sxs-lookup"><span data-stu-id="408f8-212">d.</span></span> <span data-ttu-id="408f8-213">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="408f8-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="408f8-214">e.</span><span class="sxs-lookup"><span data-stu-id="408f8-214">e.</span></span> <span data-ttu-id="408f8-215">Kattintson a **OK** bezárásához a **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-215">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="408f8-216">Kattintson a **OK** bezárásához a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-216">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="408f8-217">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="408f8-217">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="408f8-218">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="408f8-218">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="408f8-219">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="408f8-219">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="408f8-220">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="408f8-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="408f8-221">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="408f8-221">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="408f8-223">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="408f8-223">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="408f8-224">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="408f8-224">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="408f8-226">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="408f8-226">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="408f8-228">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="408f8-228">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="408f8-230">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="408f8-230">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="408f8-232">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-232">a.</span></span> <span data-ttu-id="408f8-233">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="408f8-233">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="408f8-234">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-234">b.</span></span> <span data-ttu-id="408f8-235">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="408f8-235">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="408f8-236">c.</span><span class="sxs-lookup"><span data-stu-id="408f8-236">c.</span></span> <span data-ttu-id="408f8-237">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="408f8-237">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="408f8-238">d.</span><span class="sxs-lookup"><span data-stu-id="408f8-238">d.</span></span> <span data-ttu-id="408f8-239">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="408f8-240">Zscaler tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="408f8-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="408f8-241">Ahhoz, hogy az Azure AD-felhasználók ZScaler bejelentkezni, akkor ki kell építenie ZScaler.</span><span class="sxs-lookup"><span data-stu-id="408f8-241">To enable Azure AD users to log in to ZScaler, they must be provisioned to ZScaler.</span></span>  
<span data-ttu-id="408f8-242">ZScaler, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="408f8-242">In the case of ZScaler, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="408f8-243">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="408f8-243">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="408f8-244">Jelentkezzen be a **Zscaler** bérlő.</span><span class="sxs-lookup"><span data-stu-id="408f8-244">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="408f8-245">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="408f8-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="408f8-246">![Felügyeleti](./media/active-directory-saas-zscaler-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="408f8-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="408f8-247">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="408f8-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="408f8-248">![Adja hozzá](./media/active-directory-saas-zscaler-tutorial/ic781036.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="408f8-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="408f8-249">Az a **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="408f8-249">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="408f8-250">![Adja hozzá](./media/active-directory-saas-zscaler-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="408f8-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="408f8-251">A felhasználó hozzáadása a szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="408f8-251">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="408f8-252">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="408f8-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="408f8-253">a.</span><span class="sxs-lookup"><span data-stu-id="408f8-253">a.</span></span> <span data-ttu-id="408f8-254">Típus a **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és a **részleg** egy érvényes AAD-fióknevet, amelyet kiépítését.</span><span class="sxs-lookup"><span data-stu-id="408f8-254">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="408f8-255">b.</span><span class="sxs-lookup"><span data-stu-id="408f8-255">b.</span></span> <span data-ttu-id="408f8-256">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="408f8-257">Bármely más ZScaler felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz ZScaler által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="408f8-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="408f8-258">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="408f8-258">Assigning the Azure AD test user</span></span>

<span data-ttu-id="408f8-259">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Zscaler Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="408f8-259">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="408f8-261">**Britta Simon hozzárendelése Zscaler, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="408f8-261">**To assign Britta Simon to Zscaler, perform the following steps:**</span></span>

1. <span data-ttu-id="408f8-262">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="408f8-262">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="408f8-264">Az alkalmazások listában válassza ki a **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="408f8-264">In the applications list, select **Zscaler**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="408f8-266">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="408f8-266">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="408f8-268">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="408f8-268">Click **Add** button.</span></span> <span data-ttu-id="408f8-269">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="408f8-271">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="408f8-271">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="408f8-272">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="408f8-273">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="408f8-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="408f8-274">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="408f8-274">Testing single sign-on</span></span>

<span data-ttu-id="408f8-275">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="408f8-275">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="408f8-276">Ha a hozzáférési panelen Zscaler csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Zscaler alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="408f8-276">When you click the Zscaler tile in the Access Panel, you should get automatically signed-on to your Zscaler application.</span></span>
<span data-ttu-id="408f8-277">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="408f8-277">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="408f8-278">További források</span><span class="sxs-lookup"><span data-stu-id="408f8-278">Additional resources</span></span>

* [<span data-ttu-id="408f8-279">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="408f8-279">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="408f8-280">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="408f8-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

