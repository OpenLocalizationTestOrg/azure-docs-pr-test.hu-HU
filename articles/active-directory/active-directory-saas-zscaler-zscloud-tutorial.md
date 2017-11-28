---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler ZSCloud |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Zscaler ZSCloud között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 2b6eb113e5725260bc04f50e9218939bf28b1ff0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="549ae-103">Oktatóanyag: Azure Active Directoryval integrált Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="549ae-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="549ae-104">Ebben az oktatóanyagban elsajátíthatja Zscaler ZSCloud integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="549ae-104">In this tutorial, you learn how to integrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="549ae-105">Zscaler ZSCloud integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="549ae-105">Integrating Zscaler ZSCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="549ae-106">Megadhatja a Zscaler ZSCloud hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="549ae-106">You can control in Azure AD who has access to Zscaler ZSCloud</span></span>
- <span data-ttu-id="549ae-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Zscaler ZSCloud (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="549ae-107">You can enable your users to automatically get signed-on to Zscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="549ae-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="549ae-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="549ae-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="549ae-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="549ae-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="549ae-110">Prerequisites</span></span>

<span data-ttu-id="549ae-111">Konfigurálása az Azure AD-integrációs Zscaler ZSCloud, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="549ae-111">To configure Azure AD integration with Zscaler ZSCloud, you need the following items:</span></span>

- <span data-ttu-id="549ae-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="549ae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="549ae-113">Egy Zscaler ZSCloud egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="549ae-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="549ae-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="549ae-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="549ae-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="549ae-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="549ae-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="549ae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="549ae-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="549ae-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="549ae-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="549ae-118">Scenario description</span></span>
<span data-ttu-id="549ae-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="549ae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="549ae-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="549ae-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="549ae-121">A gyűjteményből Zscaler ZSCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="549ae-121">Adding Zscaler ZSCloud from the gallery</span></span>
2. <span data-ttu-id="549ae-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="549ae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-the-gallery"></a><span data-ttu-id="549ae-123">A gyűjteményből Zscaler ZSCloud hozzáadása</span><span class="sxs-lookup"><span data-stu-id="549ae-123">Adding Zscaler ZSCloud from the gallery</span></span>
<span data-ttu-id="549ae-124">Az Azure AD integrálása a Zscaler ZSCloud konfigurálásához kell hozzáadnia Zscaler ZSCloud a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="549ae-124">To configure the integration of Zscaler ZSCloud into Azure AD, you need to add Zscaler ZSCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="549ae-125">**A gyűjteményből Zscaler ZSCloud hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="549ae-125">**To add Zscaler ZSCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="549ae-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="549ae-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="549ae-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="549ae-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="549ae-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="549ae-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="549ae-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="549ae-133">Írja be a keresőmezőbe, **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="549ae-133">In the search box, type **Zscaler ZSCloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="549ae-135">Az eredmények panelen válassza ki a **Zscaler ZSCloud**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="549ae-135">In the results panel, select **Zscaler ZSCloud**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="549ae-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="549ae-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="549ae-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Zscaler ZSCloud "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="549ae-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="549ae-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Zscaler ZSCloud a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="549ae-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler ZSCloud is to a user in Azure AD.</span></span> <span data-ttu-id="549ae-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Zscaler ZSCloud közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="549ae-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler ZSCloud needs to be established.</span></span>

<span data-ttu-id="549ae-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Zscaler ZSCloud a.</span><span class="sxs-lookup"><span data-stu-id="549ae-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="549ae-142">Az Azure AD egyszeri bejelentkezést a Zscaler ZSCloud tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="549ae-142">To configure and test Azure AD single sign-on with Zscaler ZSCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="549ae-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="549ae-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="549ae-144">**[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  – a Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="549ae-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="549ae-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="549ae-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="549ae-146">**[Zscaler ZSCloud tesztfelhasználó létrehozása](#creating-a-zscaler-zscloud-test-user)**  - való egy megfelelője a Britta Simon Zscaler ZSCloud, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="549ae-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - to have a counterpart of Britta Simon in Zscaler ZSCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="549ae-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="549ae-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="549ae-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="549ae-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="549ae-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="549ae-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="549ae-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler ZSCloud alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="549ae-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="549ae-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Zscaler ZSCloud, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="549ae-151">**To configure Azure AD single sign-on with Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="549ae-152">Az Azure portálon a a **Zscaler ZSCloud** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="549ae-152">In the Azure portal, on the **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="549ae-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="549ae-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="549ae-156">Az a **Zscaler ZSCloud tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="549ae-156">On the **Zscaler ZSCloud Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="549ae-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés az ZScaler ZSCloud alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="549ae-158">In the **Sign-on URL** textbox, type the URL used by your users to sign-on to your ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="549ae-159">Ez az érték a tényleges bejelentkezési URL-címet frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="549ae-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="549ae-160">Ügyfél [Zscaler ZSCloud ügyfél-támogatási csoport](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="549ae-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) to get this value.</span></span> 
 
4. <span data-ttu-id="549ae-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="549ae-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="549ae-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="549ae-165">A a **Zscaler ZSCloud konfigurációs** kattintson **konfigurálása Zscaler ZSCloud** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="549ae-165">On the **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="549ae-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="549ae-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="549ae-168">Egy másik webes böngészőablakban jelentkezzen be a ZScaler ZSCloud vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="549ae-168">In a different web browser window, log in to your ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="549ae-169">Kattintson a felső menüben **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="549ae-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="549ae-170">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="549ae-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="549ae-171">A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="549ae-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="549ae-172">![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")</span><span class="sxs-lookup"><span data-stu-id="549ae-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="549ae-173">Az a **hitelesítési beállítások kiválasztása a szervezet** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="549ae-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="549ae-174">![Hitelesítési](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="549ae-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="549ae-175">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-175">a.</span></span> <span data-ttu-id="549ae-176">Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.</span><span class="sxs-lookup"><span data-stu-id="549ae-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="549ae-177">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-177">b.</span></span> <span data-ttu-id="549ae-178">Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="549ae-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="549ae-179">Az a **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépéseket, és kattintson **kész**</span><span class="sxs-lookup"><span data-stu-id="549ae-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="549ae-180">![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="549ae-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="549ae-181">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-181">a.</span></span> <span data-ttu-id="549ae-182">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** be értéket a **, amelyhez a hitelesítéshez a felhasználóknak legyenek elküldve az SAML-portál URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="549ae-182">Paste the **SAML Single Sign-On Service URL** value into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="549ae-183">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-183">b.</span></span> <span data-ttu-id="549ae-184">Az a **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.</span><span class="sxs-lookup"><span data-stu-id="549ae-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="549ae-185">c.</span><span class="sxs-lookup"><span data-stu-id="549ae-185">c.</span></span> <span data-ttu-id="549ae-186">A letöltött tanúsítvány feltöltése, kattintson a **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="549ae-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="549ae-187">d.</span><span class="sxs-lookup"><span data-stu-id="549ae-187">d.</span></span> <span data-ttu-id="549ae-188">Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="549ae-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="549ae-189">Az a **felhasználói hitelesítés beállítása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="549ae-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="549ae-190">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="549ae-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="549ae-191">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-191">a.</span></span> <span data-ttu-id="549ae-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-192">Click **Save**.</span></span>

    <span data-ttu-id="549ae-193">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-193">b.</span></span> <span data-ttu-id="549ae-194">Kattintson a **aktiválásához**.</span><span class="sxs-lookup"><span data-stu-id="549ae-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="549ae-195">Proxybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="549ae-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="549ae-196">A Proxybeállítások konfigurálása az Internet Explorerben</span><span class="sxs-lookup"><span data-stu-id="549ae-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="549ae-197">Start **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="549ae-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="549ae-198">Válassza ki **Internetbeállítások** a a **eszközök** menü megnyitása a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="549ae-199">![Internetbeállítások](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internetbeállítások")</span><span class="sxs-lookup"><span data-stu-id="549ae-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="549ae-200">Kattintson a **kapcsolatok** fülre.</span><span class="sxs-lookup"><span data-stu-id="549ae-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="549ae-201">![Kapcsolatok](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "kapcsolatok")</span><span class="sxs-lookup"><span data-stu-id="549ae-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="549ae-202">Kattintson a **LAN-beállítások** megnyitásához a **LAN-beállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="549ae-203">A Proxy server területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="549ae-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="549ae-204">![Proxykiszolgáló](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxykiszolgáló")</span><span class="sxs-lookup"><span data-stu-id="549ae-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="549ae-205">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-205">a.</span></span> <span data-ttu-id="549ae-206">Válassza ki **proxykiszolgálót használni a helyi hálózaton**.</span><span class="sxs-lookup"><span data-stu-id="549ae-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="549ae-207">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-207">b.</span></span> <span data-ttu-id="549ae-208">Írja be a címet szövegmező **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="549ae-208">In the Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="549ae-209">c.</span><span class="sxs-lookup"><span data-stu-id="549ae-209">c.</span></span> <span data-ttu-id="549ae-210">Írja be a Port szövegmező **80**.</span><span class="sxs-lookup"><span data-stu-id="549ae-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="549ae-211">d.</span><span class="sxs-lookup"><span data-stu-id="549ae-211">d.</span></span> <span data-ttu-id="549ae-212">Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.</span><span class="sxs-lookup"><span data-stu-id="549ae-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="549ae-213">e.</span><span class="sxs-lookup"><span data-stu-id="549ae-213">e.</span></span> <span data-ttu-id="549ae-214">Kattintson a **OK** bezárásához a **helyi hálózati (LAN) beállításai** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="549ae-215">Kattintson a **OK** bezárásához a **Internetbeállítások** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-215">Click **OK** to close the **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="549ae-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="549ae-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="549ae-217">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="549ae-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="549ae-219">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="549ae-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="549ae-220">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="549ae-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="549ae-222">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="549ae-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="549ae-224">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="549ae-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="549ae-226">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="549ae-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="549ae-228">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-228">a.</span></span> <span data-ttu-id="549ae-229">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="549ae-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="549ae-230">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-230">b.</span></span> <span data-ttu-id="549ae-231">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="549ae-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="549ae-232">c.</span><span class="sxs-lookup"><span data-stu-id="549ae-232">c.</span></span> <span data-ttu-id="549ae-233">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="549ae-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="549ae-234">d.</span><span class="sxs-lookup"><span data-stu-id="549ae-234">d.</span></span> <span data-ttu-id="549ae-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="549ae-236">Zscaler ZSCloud tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="549ae-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="549ae-237">Ahhoz, hogy az Azure AD-felhasználók ZScaler ZSCloud bejelentkezni, akkor ki kell építenie ZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="549ae-237">To enable Azure AD users to log in to ZScaler ZSCloud, they must be provisioned to ZScaler ZSCloud.</span></span>  
<span data-ttu-id="549ae-238">ZScaler ZSCloud, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="549ae-238">In the case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="549ae-239">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="549ae-239">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="549ae-240">Jelentkezzen be a **Zscaler** bérlő.</span><span class="sxs-lookup"><span data-stu-id="549ae-240">Log in to your **Zscaler** tenant.</span></span>

2. <span data-ttu-id="549ae-241">Kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="549ae-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="549ae-242">![Felügyeleti](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "felügyeleti")</span><span class="sxs-lookup"><span data-stu-id="549ae-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="549ae-243">Kattintson a **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="549ae-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="549ae-244">![Adja hozzá](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="549ae-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="549ae-245">Az a **felhasználók** lapra, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="549ae-245">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="549ae-246">![Adja hozzá](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="549ae-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="549ae-247">A felhasználó hozzáadása a szakaszban a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="549ae-247">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="549ae-248">![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="549ae-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="549ae-249">a.</span><span class="sxs-lookup"><span data-stu-id="549ae-249">a.</span></span> <span data-ttu-id="549ae-250">Típus a **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és a **részleg** egy érvényes AAD-fióknevet, amelyet kiépítését.</span><span class="sxs-lookup"><span data-stu-id="549ae-250">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="549ae-251">b.</span><span class="sxs-lookup"><span data-stu-id="549ae-251">b.</span></span> <span data-ttu-id="549ae-252">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="549ae-253">Bármely más ZScaler ZSCloud felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz ZScaler ZSCloud által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="549ae-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="549ae-254">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="549ae-254">Assigning the Azure AD test user</span></span>

<span data-ttu-id="549ae-255">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Zscaler ZSCloud Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="549ae-255">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler ZSCloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="549ae-257">**Britta Simon hozzárendelése Zscaler ZSCloud, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="549ae-257">**To assign Britta Simon to Zscaler ZSCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="549ae-258">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="549ae-258">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="549ae-260">Az alkalmazások listában válassza ki a **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="549ae-260">In the applications list, select **Zscaler ZSCloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="549ae-262">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="549ae-262">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="549ae-264">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="549ae-264">Click **Add** button.</span></span> <span data-ttu-id="549ae-265">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="549ae-267">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="549ae-267">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="549ae-268">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="549ae-269">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="549ae-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="549ae-270">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="549ae-270">Testing single sign-on</span></span>

<span data-ttu-id="549ae-271">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="549ae-271">If you want to test your single sign-on settings, open the Access Panel.</span></span>

<span data-ttu-id="549ae-272">Ha a hozzáférési panelen Zscaler ZSCloud csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Zscaler ZSCloud alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="549ae-272">When you click the Zscaler ZSCloud tile in the Access Panel, you should get automatically signed-on to your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="549ae-273">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="549ae-273">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="549ae-274">További források</span><span class="sxs-lookup"><span data-stu-id="549ae-274">Additional resources</span></span>

* [<span data-ttu-id="549ae-275">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="549ae-275">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="549ae-276">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="549ae-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

