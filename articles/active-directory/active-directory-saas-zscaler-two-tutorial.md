---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler két |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Zscaler két között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 38c9da0a6599bb66c452fdb8a8911338601155f9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a><span data-ttu-id="3f64f-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler két</span><span class="sxs-lookup"><span data-stu-id="3f64f-103">Tutorial: Azure Active Directory integration with Zscaler Two</span></span>

<span data-ttu-id="3f64f-104">Ebben az oktatóanyagban elsajátíthatja Zscaler két integrálása Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f64f-104">In this tutorial, you learn how to integrate Zscaler Two with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f64f-105">Zscaler két integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3f64f-105">Integrating Zscaler Two with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f64f-106">Szabályozhatja, aki hozzáfér Zscaler két Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3f64f-106">You can control in Azure AD who has access to Zscaler Two</span></span>
- <span data-ttu-id="3f64f-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Zscaler két (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="3f64f-107">You can enable your users to automatically get signed-on to Zscaler Two (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f64f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3f64f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f64f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f64f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f64f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f64f-110">Prerequisites</span></span>

<span data-ttu-id="3f64f-111">Konfigurálása az Azure AD-integrációs Zscaler két, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3f64f-111">To configure Azure AD integration with Zscaler Two, you need the following items:</span></span>

- <span data-ttu-id="3f64f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3f64f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f64f-113">Egy Zscaler két egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3f64f-113">A Zscaler Two single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f64f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3f64f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f64f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3f64f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f64f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3f64f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f64f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f64f-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f64f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3f64f-118">Scenario description</span></span>
<span data-ttu-id="3f64f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3f64f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f64f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3f64f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f64f-121">A gyűjteményből Zscaler két hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f64f-121">Adding Zscaler Two from the gallery</span></span>
2. <span data-ttu-id="3f64f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f64f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-two-from-the-gallery"></a><span data-ttu-id="3f64f-123">A gyűjteményből Zscaler két hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f64f-123">Adding Zscaler Two from the gallery</span></span>
<span data-ttu-id="3f64f-124">Az Azure AD integrálása a Zscaler két konfigurálásához kell hozzáadnia Zscaler két a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3f64f-124">To configure the integration of Zscaler Two into Azure AD, you need to add Zscaler Two from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f64f-125">**Adja hozzá a Zscaler két a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f64f-125">**To add Zscaler Two from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f64f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f64f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f64f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3f64f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3f64f-133">Írja be a keresőmezőbe, **Zscaler két**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-133">In the search box, type **Zscaler Two**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_search.png)

5. <span data-ttu-id="3f64f-135">Az eredmények panelen válassza ki a **Zscaler két**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3f64f-135">In the results panel, select **Zscaler Two**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f64f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f64f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f64f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Zscaler két "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="3f64f-138">In this section, you configure and test Azure AD single sign-on with Zscaler Two based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f64f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Zscaler két a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3f64f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler Two is to a user in Azure AD.</span></span> <span data-ttu-id="3f64f-140">Ez azt jelenti Zscaler két kapcsolódó felhasználói, valamint az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3f64f-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler Two needs to be established.</span></span>

<span data-ttu-id="3f64f-141">Zscaler két rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3f64f-141">In Zscaler Two, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f64f-142">Az Azure AD egyszeri bejelentkezést a Zscaler két tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3f64f-142">To configure and test Azure AD single sign-on with Zscaler Two, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f64f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3f64f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f64f-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  – a Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="3f64f-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="3f64f-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f64f-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="3f64f-146">**[Zscaler két tesztfelhasználó létrehozása](#creating-a-zscaler-two-test-user)**  - való egy megfelelője a Britta Simon Zscaler két, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3f64f-146">**[Creating a Zscaler Two test user](#creating-a-zscaler-two-test-user)** - to have a counterpart of Britta Simon in Zscaler Two that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="3f64f-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f64f-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="3f64f-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3f64f-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f64f-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f64f-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f64f-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az Zscaler két alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3f64f-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler Two application.</span></span>

<span data-ttu-id="3f64f-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Zscaler két, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f64f-151">**To configure Azure AD single sign-on with Zscaler Two, perform the following steps:**</span></span>

1. <span data-ttu-id="3f64f-152">Az Azure portálon a a **Zscaler két** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-152">In the Azure portal, on the **Zscaler Two** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3f64f-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f64f-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_samlbase.png)

3. <span data-ttu-id="3f64f-156">Az a **Zscaler két tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f64f-156">On the **Zscaler Two Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_url.png)

   <span data-ttu-id="3f64f-158">A bejelentkezési URL-cím mezőbe írja be a a felhasználók bejelentkezés az ZScaler két alkalmazás által használt URL-cím.</span><span class="sxs-lookup"><span data-stu-id="3f64f-158">In the Sign-on URL textbox, type the URL used by your users to sign-on to your ZScaler Two application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f64f-159">Ez az érték a tényleges bejelentkezési URL-címet frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="3f64f-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3f64f-160">Ügyfél [Zscaler két ügyfél-támogatási csoport](https://www.zscaler.com/company/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3f64f-160">Contact [Zscaler Two Client support team](https://www.zscaler.com/company/contact) to get these values.</span></span>

4. <span data-ttu-id="3f64f-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f64f-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_certificate.png) 

5. <span data-ttu-id="3f64f-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f64f-165">A a **Zscaler két konfigurációban** kattintson **Zscaler két konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="3f64f-165">On the **Zscaler Two Configuration** section, click **Configure Zscaler Two** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3f64f-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="3f64f-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_configure.png) 

7. <span data-ttu-id="3f64f-168">Egy másik webes böngészőablakban jelentkezzen be a ZScaler két vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3f64f-168">In a different web browser window, log in to your ZScaler Two company site as an administrator.</span></span>

8. <span data-ttu-id="3f64f-169">Kattintson a felső menüben **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="3f64f-170">![Felügyeleti](./media/active-directory-saas-zscaler-two-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="3f64f-170">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="3f64f-171">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="3f64f-172">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-two-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="3f64f-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-two-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="3f64f-173">Az a **hitelesítési beállítások kiválasztása a szervezet** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f64f-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="3f64f-174">![Hitelesítési](./media/active-directory-saas-zscaler-two-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="3f64f-174">![Authentication](./media/active-directory-saas-zscaler-two-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="3f64f-175">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-175">a.</span></span> <span data-ttu-id="3f64f-176">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="3f64f-177">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-177">b.</span></span> <span data-ttu-id="3f64f-178">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="3f64f-179">Az a **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson **kész**</span><span class="sxs-lookup"><span data-stu-id="3f64f-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="3f64f-180">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-two-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="3f64f-180">![Single Sign-On](./media/active-directory-saas-zscaler-two-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="3f64f-181">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-181">a.</span></span> <span data-ttu-id="3f64f-182">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **, amelyhez a hitelesítéshez a felhasználóknak legyenek elküldve az SAML-portál URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3f64f-182">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="3f64f-183">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-183">b.</span></span> <span data-ttu-id="3f64f-184">Az a **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="3f64f-185">c.</span><span class="sxs-lookup"><span data-stu-id="3f64f-185">c.</span></span> <span data-ttu-id="3f64f-186">A letöltött tanúsítvány feltöltése, kattintson a **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="3f64f-187">d.</span><span class="sxs-lookup"><span data-stu-id="3f64f-187">d.</span></span> <span data-ttu-id="3f64f-188">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="3f64f-189">Az a **felhasználói hitelesítés beállítása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f64f-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="3f64f-190">![Felügyeleti](./media/active-directory-saas-zscaler-two-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="3f64f-190">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="3f64f-191">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-191">a.</span></span> <span data-ttu-id="3f64f-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-192">Click **Save**.</span></span>

    <span data-ttu-id="3f64f-193">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-193">b.</span></span> <span data-ttu-id="3f64f-194">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="3f64f-195">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f64f-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="3f64f-196">A Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="3f64f-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="3f64f-197">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="3f64f-198">Válassza ki **Internetbeállítások** a a **eszközök** menü megnyitása a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="3f64f-199">![Internetbeállítások](./media/active-directory-saas-zscaler-two-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="3f64f-199">![Internet Options](./media/active-directory-saas-zscaler-two-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="3f64f-200">Kattintson a **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="3f64f-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="3f64f-201">![Kapcsolatok](./media/active-directory-saas-zscaler-two-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="3f64f-201">![Connections](./media/active-directory-saas-zscaler-two-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="3f64f-202">Kattintson a **LAN-beállítások** megnyitásához a **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="3f64f-203">A Proxy server területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f64f-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="3f64f-204">![Proxykiszolgáló](./media/active-directory-saas-zscaler-two-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="3f64f-204">![Proxy server](./media/active-directory-saas-zscaler-two-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="3f64f-205">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-205">a.</span></span> <span data-ttu-id="3f64f-206">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="3f64f-207">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-207">b.</span></span> <span data-ttu-id="3f64f-208">Írja be a címet szövegmező **gateway.zscalertwo.net**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-208">In the Address textbox, type **gateway.zscalertwo.net**.</span></span>

    <span data-ttu-id="3f64f-209">c.</span><span class="sxs-lookup"><span data-stu-id="3f64f-209">c.</span></span> <span data-ttu-id="3f64f-210">Írja be a Port szövegmező **80**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="3f64f-211">d.</span><span class="sxs-lookup"><span data-stu-id="3f64f-211">d.</span></span> <span data-ttu-id="3f64f-212">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="3f64f-213">e.</span><span class="sxs-lookup"><span data-stu-id="3f64f-213">e.</span></span> <span data-ttu-id="3f64f-214">Kattintson a **OK** bezárásához a **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="3f64f-215">Kattintson a **OK** bezárásához a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-215">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="3f64f-216">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3f64f-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f64f-217">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3f64f-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f64f-218">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f64f-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f64f-219">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f64f-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f64f-220">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3f64f-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3f64f-222">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f64f-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f64f-223">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f64f-225">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f64f-227">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3f64f-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f64f-229">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f64f-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f64f-231">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-231">a.</span></span> <span data-ttu-id="3f64f-232">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f64f-233">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-233">b.</span></span> <span data-ttu-id="3f64f-234">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f64f-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f64f-235">c.</span><span class="sxs-lookup"><span data-stu-id="3f64f-235">c.</span></span> <span data-ttu-id="3f64f-236">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f64f-237">d.</span><span class="sxs-lookup"><span data-stu-id="3f64f-237">d.</span></span> <span data-ttu-id="3f64f-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-two-test-user"></a><span data-ttu-id="3f64f-239">Zscaler két tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f64f-239">Creating a Zscaler Two test user</span></span>

<span data-ttu-id="3f64f-240">Ahhoz, hogy az Azure AD-felhasználók jelentkezzen be a ZScaler két, akkor ki kell építenie ZScaler két.</span><span class="sxs-lookup"><span data-stu-id="3f64f-240">To enable Azure AD users to log in to ZScaler Two, they must be provisioned to ZScaler Two.</span></span> <span data-ttu-id="3f64f-241">ZScaler két esetén kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="3f64f-241">In the case of ZScaler Two, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="3f64f-242">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3f64f-242">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="3f64f-243">Jelentkezzen be a **Zscaler két** bérlő.</span><span class="sxs-lookup"><span data-stu-id="3f64f-243">Log in to your **Zscaler Two** tenant.</span></span>

2. <span data-ttu-id="3f64f-244">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="3f64f-245">![Felügyeleti](./media/active-directory-saas-zscaler-two-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="3f64f-245">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="3f64f-246">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="3f64f-247">![Adja hozzá](./media/active-directory-saas-zscaler-two-tutorial/ic781036.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3f64f-247">![Add](./media/active-directory-saas-zscaler-two-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="3f64f-248">Az a **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-248">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="3f64f-249">![Adja hozzá](./media/active-directory-saas-zscaler-two-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3f64f-249">![Add](./media/active-directory-saas-zscaler-two-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="3f64f-250">A felhasználó hozzáadása a szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f64f-250">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="3f64f-251">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-two-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="3f64f-251">![Add User](./media/active-directory-saas-zscaler-two-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="3f64f-252">a.</span><span class="sxs-lookup"><span data-stu-id="3f64f-252">a.</span></span> <span data-ttu-id="3f64f-253">Típus a **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok** és a **részleg** egy érvényes Azure AD fióknevet, amelyet a rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="3f64f-253">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="3f64f-254">b.</span><span class="sxs-lookup"><span data-stu-id="3f64f-254">b.</span></span> <span data-ttu-id="3f64f-255">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="3f64f-256">ZScaler két felhasználói fiók létrehozása eszközök vagy ZScaler két által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="3f64f-256">You can use any other ZScaler Two user account creation tools or APIs provided by ZScaler Two to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f64f-257">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3f64f-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f64f-258">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Zscaler két Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3f64f-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler Two.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3f64f-260">**Britta Simon hozzárendelése Zscaler két, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f64f-260">**To assign Britta Simon to Zscaler Two, perform the following steps:**</span></span>

1. <span data-ttu-id="3f64f-261">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3f64f-263">Az alkalmazások listában válassza ki a **Zscaler két**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-263">In the applications list, select **Zscaler Two**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_app.png) 

3. <span data-ttu-id="3f64f-265">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3f64f-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3f64f-267">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f64f-267">Click **Add** button.</span></span> <span data-ttu-id="3f64f-268">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3f64f-270">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3f64f-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f64f-271">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f64f-272">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f64f-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f64f-273">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3f64f-273">Testing single sign-on</span></span>

<span data-ttu-id="3f64f-274">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3f64f-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3f64f-275">Ha a hozzáférési panelen Zscaler két csempe gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az Zscaler két alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3f64f-275">When you click the Zscaler Two tile in the Access Panel, you should get automatically signed-on to your Zscaler Two application.</span></span>
<span data-ttu-id="3f64f-276">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f64f-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f64f-277">További források</span><span class="sxs-lookup"><span data-stu-id="3f64f-277">Additional resources</span></span>

* [<span data-ttu-id="3f64f-278">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3f64f-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f64f-279">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3f64f-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_203.png

