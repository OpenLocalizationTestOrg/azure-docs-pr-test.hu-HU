---
title: "Oktatóanyag: Azure Active Directoryval integrált híd |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és híd között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dbb6499-80c1-4d00-a0b4-e0ad5522cf0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d2b7fd6f73f1b782cfe6337d13f9f22f9c70d389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bridge"></a><span data-ttu-id="d28e5-103">Oktatóanyag: Azure Active Directoryval integrált híd</span><span class="sxs-lookup"><span data-stu-id="d28e5-103">Tutorial: Azure Active Directory integration with Bridge</span></span>

<span data-ttu-id="d28e5-104">Ebben az oktatóanyagban elsajátíthatja híd integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d28e5-104">In this tutorial, you learn how to integrate Bridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d28e5-105">A Helykapcsolathíd integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d28e5-105">Integrating Bridge with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d28e5-106">Szabályozhatja az Azure AD, aki hozzáfér híd</span><span class="sxs-lookup"><span data-stu-id="d28e5-106">You can control in Azure AD who has access to Bridge</span></span>
- <span data-ttu-id="d28e5-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett híd (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d28e5-107">You can enable your users to automatically get signed-on to Bridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d28e5-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d28e5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d28e5-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d28e5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d28e5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d28e5-110">Prerequisites</span></span>

<span data-ttu-id="d28e5-111">Az Azure AD-integráció konfigurálása a híd, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d28e5-111">To configure Azure AD integration with Bridge, you need the following items:</span></span>

- <span data-ttu-id="d28e5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d28e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d28e5-113">Egy híd egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d28e5-113">A Bridge single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d28e5-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d28e5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d28e5-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d28e5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d28e5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d28e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d28e5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d28e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d28e5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d28e5-118">Scenario description</span></span>
<span data-ttu-id="d28e5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d28e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d28e5-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d28e5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d28e5-121">A Helykapcsolathíd hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d28e5-121">Adding Bridge from the gallery</span></span>
2. <span data-ttu-id="d28e5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d28e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bridge-from-the-gallery"></a><span data-ttu-id="d28e5-123">A Helykapcsolathíd hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d28e5-123">Adding Bridge from the gallery</span></span>
<span data-ttu-id="d28e5-124">Az Azure AD integrálása a híd konfigurálásához kell hozzáadnia híd a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d28e5-124">To configure the integration of Bridge into Azure AD, you need to add Bridge from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d28e5-125">**A gyűjteményből híd hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d28e5-125">**To add Bridge from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d28e5-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d28e5-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d28e5-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d28e5-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d28e5-133">Írja be a keresőmezőbe, **híd**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-133">In the search box, type **Bridge**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_search.png)

5. <span data-ttu-id="d28e5-135">Az eredmények panelen válassza ki a **híd**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d28e5-135">In the results panel, select **Bridge**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d28e5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d28e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d28e5-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a "Britta Simon." nevű tesztfelhasználó alapján híd</span><span class="sxs-lookup"><span data-stu-id="d28e5-138">In this section, you configure and test Azure AD single sign-on with Bridge based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d28e5-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Bridge a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d28e5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bridge is to a user in Azure AD.</span></span> <span data-ttu-id="d28e5-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Bridge közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d28e5-140">In other words, a link relationship between an Azure AD user and the related user in Bridge needs to be established.</span></span>

<span data-ttu-id="d28e5-141">Bridge, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d28e5-141">In Bridge, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d28e5-142">Az Azure AD egyszeri bejelentkezést a híd tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d28e5-142">To configure and test Azure AD single sign-on with Bridge, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d28e5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d28e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d28e5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d28e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d28e5-145">**[A Helykapcsolathíd tesztfelhasználó létrehozása](#creating-a-bridge-test-user)**  - való egy megfelelője a Britta Simon híd, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d28e5-145">**[Creating a Bridge test user](#creating-a-bridge-test-user)** - to have a counterpart of Britta Simon in Bridge that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d28e5-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d28e5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d28e5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d28e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d28e5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d28e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d28e5-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az híd alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d28e5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bridge application.</span></span>

<span data-ttu-id="d28e5-150">**A híd konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d28e5-150">**To configure Azure AD single sign-on with Bridge, perform the following steps:**</span></span>

1. <span data-ttu-id="d28e5-151">Az Azure portálon a a **híd** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-151">In the Azure portal, on the **Bridge** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d28e5-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d28e5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_samlbase.png)

3. <span data-ttu-id="d28e5-155">Az a **híd tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d28e5-155">On the **Bridge Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_url.png)

    <span data-ttu-id="d28e5-157">a.</span><span class="sxs-lookup"><span data-stu-id="d28e5-157">a.</span></span> <span data-ttu-id="d28e5-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d28e5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.bridgeapp.com`</span></span>

    <span data-ttu-id="d28e5-159">b.</span><span class="sxs-lookup"><span data-stu-id="d28e5-159">b.</span></span> <span data-ttu-id="d28e5-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="d28e5-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.bridgeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d28e5-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d28e5-161">These values are not real.</span></span> <span data-ttu-id="d28e5-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d28e5-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d28e5-163">Ügyfél [híd ügyfél-támogatási csoport](https://community.bridgeapp.com/community/help) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d28e5-163">Contact [Bridge Client support team](https://community.bridgeapp.com/community/help) to get these values.</span></span> 
 
4. <span data-ttu-id="d28e5-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d28e5-164">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_certificate.png) 

5. <span data-ttu-id="d28e5-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d28e5-168">A a **híd konfigurációs** kattintson **konfigurálása híd** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d28e5-168">On the **Bridge Configuration** section, click **Configure Bridge** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d28e5-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d28e5-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_configure.png) 

7. <span data-ttu-id="d28e5-171">Egyszeri bejelentkezés konfigurálása **híd** oldalon kell küldeniük a letöltött **Certificate(Raw)** és **SAML Entitásazonosító, SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím** való [híd támogatási csoport](https://community.bridgeapp.com/community/help).</span><span class="sxs-lookup"><span data-stu-id="d28e5-171">To configure single sign-on on **Bridge** side, you need to send the downloaded **Certificate(Raw)** and **SAML Entity ID, SAML Single Sign-On Service URL, and Sign-Out URL** to [Bridge support team](https://community.bridgeapp.com/community/help).</span></span> 

> [!TIP]
> <span data-ttu-id="d28e5-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d28e5-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d28e5-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d28e5-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d28e5-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d28e5-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d28e5-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d28e5-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="d28e5-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d28e5-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d28e5-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d28e5-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d28e5-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d28e5-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d28e5-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d28e5-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d28e5-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d28e5-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d28e5-187">a.</span><span class="sxs-lookup"><span data-stu-id="d28e5-187">a.</span></span> <span data-ttu-id="d28e5-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d28e5-189">b.</span><span class="sxs-lookup"><span data-stu-id="d28e5-189">b.</span></span> <span data-ttu-id="d28e5-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d28e5-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d28e5-191">c.</span><span class="sxs-lookup"><span data-stu-id="d28e5-191">c.</span></span> <span data-ttu-id="d28e5-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d28e5-193">d.</span><span class="sxs-lookup"><span data-stu-id="d28e5-193">d.</span></span> <span data-ttu-id="d28e5-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-194">Click **Create**.</span></span>
 
### <a name="creating-a-bridge-test-user"></a><span data-ttu-id="d28e5-195">A Helykapcsolathíd tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d28e5-195">Creating a Bridge test user</span></span>

<span data-ttu-id="d28e5-196">Ebben a szakaszban egy híd Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d28e5-196">In this section, you create a user called Britta Simon in Bridge.</span></span> <span data-ttu-id="d28e5-197">Együttműködve [híd ügyfél-támogatási csoport](https://community.bridgeapp.com/community/help) a platform felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d28e5-197">Work with [Bridge Client support team](https://community.bridgeapp.com/community/help) to create a user in the platform.</span></span> <span data-ttu-id="d28e5-198">A támogatási jegy a Bridge is növelheti <a href="https://community.bridgeapp.com/community/help">Itt</a> a felhasználók hozzáadása a híd platform.</span><span class="sxs-lookup"><span data-stu-id="d28e5-198">You can raise the support ticket with Bridge from <a href="https://community.bridgeapp.com/community/help">here</a> to add the users in the Bridge platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d28e5-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d28e5-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d28e5-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés híd Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d28e5-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Bridge.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d28e5-202">**Britta Simon hozzárendelése híd, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d28e5-202">**To assign Britta Simon to Bridge, perform the following steps:**</span></span>

1. <span data-ttu-id="d28e5-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d28e5-205">Az alkalmazások listában válassza ki a **híd**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-205">In the applications list, select **Bridge**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_app.png) 

3. <span data-ttu-id="d28e5-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d28e5-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d28e5-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d28e5-209">Click **Add** button.</span></span> <span data-ttu-id="d28e5-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d28e5-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d28e5-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d28e5-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d28e5-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d28e5-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d28e5-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d28e5-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d28e5-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d28e5-215">Testing single sign-on</span></span>

<span data-ttu-id="d28e5-216">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d28e5-216">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="d28e5-217">Ha a hozzáférési panelen híd csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az híd alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d28e5-217">When you click the Bridge tile in the Access Panel, you should get automatically signed-on to your Bridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d28e5-218">További források</span><span class="sxs-lookup"><span data-stu-id="d28e5-218">Additional resources</span></span>

* [<span data-ttu-id="d28e5-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d28e5-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d28e5-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d28e5-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_203.png

