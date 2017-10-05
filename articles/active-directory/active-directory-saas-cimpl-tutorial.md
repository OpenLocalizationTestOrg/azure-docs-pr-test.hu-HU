---
title: "Oktatóanyag: Azure Active Directoryval integrált Cimpl |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Cimpl között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58ee5481-ae40-4e4a-a3c9-86343851fc9a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 24a0c89966c83e1b32367d4519ead98d76f5ac6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cimpl"></a><span data-ttu-id="af8cb-103">Oktatóanyag: Azure Active Directoryval integrált Cimpl</span><span class="sxs-lookup"><span data-stu-id="af8cb-103">Tutorial: Azure Active Directory integration with Cimpl</span></span>

<span data-ttu-id="af8cb-104">Ebben az oktatóanyagban elsajátíthatja Cimpl integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="af8cb-104">In this tutorial, you learn how to integrate Cimpl with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="af8cb-105">Cimpl integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="af8cb-105">Integrating Cimpl with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="af8cb-106">Megadhatja a Cimpl hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="af8cb-106">You can control in Azure AD who has access to Cimpl</span></span>
- <span data-ttu-id="af8cb-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Cimpl (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="af8cb-107">You can enable your users to automatically get signed-on to Cimpl (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="af8cb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="af8cb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="af8cb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="af8cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af8cb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="af8cb-110">Prerequisites</span></span>

<span data-ttu-id="af8cb-111">Konfigurálása az Azure AD-integrációs Cimpl, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="af8cb-111">To configure Azure AD integration with Cimpl, you need the following items:</span></span>

- <span data-ttu-id="af8cb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="af8cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="af8cb-113">Egy Cimpl egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="af8cb-113">A Cimpl single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="af8cb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="af8cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="af8cb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="af8cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="af8cb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="af8cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="af8cb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="af8cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="af8cb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="af8cb-118">Scenario description</span></span>
<span data-ttu-id="af8cb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="af8cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="af8cb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="af8cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="af8cb-121">A gyűjteményből Cimpl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af8cb-121">Adding Cimpl from the gallery</span></span>
2. <span data-ttu-id="af8cb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="af8cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cimpl-from-the-gallery"></a><span data-ttu-id="af8cb-123">A gyűjteményből Cimpl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af8cb-123">Adding Cimpl from the gallery</span></span>
<span data-ttu-id="af8cb-124">Az Azure AD integrálása a Cimpl konfigurálásához kell hozzáadnia Cimpl a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="af8cb-124">To configure the integration of Cimpl into Azure AD, you need to add Cimpl from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="af8cb-125">**A gyűjteményből Cimpl hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="af8cb-125">**To add Cimpl from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="af8cb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="af8cb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="af8cb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="af8cb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="af8cb-133">Írja be a keresőmezőbe, **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-133">In the search box, type **Cimpl**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_search.png)

5. <span data-ttu-id="af8cb-135">Az eredmények panelen válassza ki a **Cimpl**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="af8cb-135">In the results panel, select **Cimpl**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="af8cb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="af8cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="af8cb-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Cimpl.</span><span class="sxs-lookup"><span data-stu-id="af8cb-138">In this section, you configure and test Azure AD single sign-on with Cimpl based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="af8cb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Cimpl a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="af8cb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cimpl is to a user in Azure AD.</span></span> <span data-ttu-id="af8cb-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Cimpl közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="af8cb-140">In other words, a link relationship between an Azure AD user and the related user in Cimpl needs to be established.</span></span>

<span data-ttu-id="af8cb-141">Cimpl, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="af8cb-141">In Cimpl, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="af8cb-142">Az Azure AD egyszeri bejelentkezést a Cimpl tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="af8cb-142">To configure and test Azure AD single sign-on with Cimpl, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="af8cb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="af8cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="af8cb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="af8cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="af8cb-145">**[Cimpl tesztfelhasználó létrehozása](#creating-a-cimpl-test-user)**  - való Britta Simon valami Cimpl, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="af8cb-145">**[Creating a Cimpl test user](#creating-a-cimpl-test-user)** - to have a counterpart of Britta Simon in Cimpl that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="af8cb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="af8cb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="af8cb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="af8cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="af8cb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af8cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="af8cb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Cimpl alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="af8cb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cimpl application.</span></span>

<span data-ttu-id="af8cb-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Cimpl, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="af8cb-150">**To configure Azure AD single sign-on with Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="af8cb-151">Az Azure portálon a a **Cimpl** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-151">In the Azure portal, on the **Cimpl** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="af8cb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="af8cb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_samlbase.png)

3. <span data-ttu-id="af8cb-155">Az a **Cimpl tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="af8cb-155">On the **Cimpl Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_url.png)

    <span data-ttu-id="af8cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="af8cb-157">a.</span></span> <span data-ttu-id="af8cb-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="af8cb-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    <span data-ttu-id="af8cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="af8cb-159">b.</span></span> <span data-ttu-id="af8cb-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="af8cb-160">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="af8cb-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="af8cb-161">These values are not real.</span></span> <span data-ttu-id="af8cb-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="af8cb-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="af8cb-163">Lépjen kapcsolatba a Cimpl csapatának **+1 866-982-8250** beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="af8cb-163">Contact Cimpl team at **+1 866-982-8250** to get these values.</span></span> 
 
4. <span data-ttu-id="af8cb-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="af8cb-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_certificate.png) 

5. <span data-ttu-id="af8cb-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="af8cb-168">A a **Cimpl konfigurációs** kattintson **konfigurálása Cimpl** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="af8cb-168">On the **Cimpl Configuration** section, click **Configure Cimpl** to open **Configure sign-on** window.</span></span> <span data-ttu-id="af8cb-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="af8cb-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_configure.png) 

7. <span data-ttu-id="af8cb-171">Egyszeri bejelentkezés konfigurálása **Cimpl** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** Cimpl számára támogatja a következő **+1 866-982-8250**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-171">To configure single sign-on on **Cimpl** side, you need to send the downloaded **Certificate (Base64)**, **SAML Entity ID, and SAML Single Sign-On Service URL** to Cimpl support at **+1 866-982-8250**.</span></span>


> [!TIP]
> <span data-ttu-id="af8cb-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="af8cb-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="af8cb-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="af8cb-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="af8cb-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="af8cb-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="af8cb-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="af8cb-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="af8cb-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="af8cb-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="af8cb-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="af8cb-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="af8cb-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="af8cb-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="af8cb-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="af8cb-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="af8cb-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="af8cb-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cimpl-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="af8cb-187">a.</span><span class="sxs-lookup"><span data-stu-id="af8cb-187">a.</span></span> <span data-ttu-id="af8cb-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="af8cb-189">b.</span><span class="sxs-lookup"><span data-stu-id="af8cb-189">b.</span></span> <span data-ttu-id="af8cb-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="af8cb-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="af8cb-191">c.</span><span class="sxs-lookup"><span data-stu-id="af8cb-191">c.</span></span> <span data-ttu-id="af8cb-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="af8cb-193">d.</span><span class="sxs-lookup"><span data-stu-id="af8cb-193">d.</span></span> <span data-ttu-id="af8cb-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-194">Click **Create**.</span></span>
 
### <a name="creating-a-cimpl-test-user"></a><span data-ttu-id="af8cb-195">Cimpl tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="af8cb-195">Creating a Cimpl test user</span></span>

<span data-ttu-id="af8cb-196">Ez a szakasz célja Cimpl Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="af8cb-196">The objective of this section is to create a user called Britta Simon in Cimpl.</span></span> <span data-ttu-id="af8cb-197">Cimpl támogatását együttműködve **+1 866-982-8250** a felhasználók hozzáadása a Cimpl fiók.</span><span class="sxs-lookup"><span data-stu-id="af8cb-197">Work with Cimpl support at **+1 866-982-8250** to add the users in the Cimpl account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="af8cb-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="af8cb-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="af8cb-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Cimpl Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="af8cb-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cimpl.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="af8cb-201">**Britta Simon hozzárendelése Cimpl, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="af8cb-201">**To assign Britta Simon to Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="af8cb-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="af8cb-204">Az alkalmazások listában válassza ki a **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-204">In the applications list, select **Cimpl**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_app.png) 

3. <span data-ttu-id="af8cb-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="af8cb-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="af8cb-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="af8cb-208">Click **Add** button.</span></span> <span data-ttu-id="af8cb-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="af8cb-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="af8cb-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="af8cb-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="af8cb-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="af8cb-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="af8cb-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="af8cb-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="af8cb-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="af8cb-214">Testing single sign-on</span></span>

<span data-ttu-id="af8cb-215">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="af8cb-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  <span data-ttu-id="af8cb-216">Ha a hozzáférési panelen Cimpl csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Cimpl alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="af8cb-216">When you click the Cimpl tile in the Access Panel, you should get automatically signed-on to your Cimpl application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="af8cb-217">További források</span><span class="sxs-lookup"><span data-stu-id="af8cb-217">Additional resources</span></span>

* [<span data-ttu-id="af8cb-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="af8cb-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="af8cb-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="af8cb-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_203.png

