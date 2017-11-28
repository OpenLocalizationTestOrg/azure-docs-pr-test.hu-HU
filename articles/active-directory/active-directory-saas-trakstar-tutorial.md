---
title: "Oktatóanyag: Azure Active Directoryval integrált Trakstar |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Trakstar között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411cb8c3-95c6-4138-acf2-ffc7f663e89a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 757429aa187e6536489b6636a0a11d122c7f9378
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trakstar"></a><span data-ttu-id="2b51f-103">Oktatóanyag: Azure Active Directoryval integrált Trakstar</span><span class="sxs-lookup"><span data-stu-id="2b51f-103">Tutorial: Azure Active Directory integration with Trakstar</span></span>

<span data-ttu-id="2b51f-104">Ebben az oktatóanyagban elsajátíthatja Trakstar integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2b51f-104">In this tutorial, you learn how to integrate Trakstar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2b51f-105">Trakstar integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2b51f-105">Integrating Trakstar with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2b51f-106">Megadhatja a Trakstar hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2b51f-106">You can control in Azure AD who has access to Trakstar</span></span>
- <span data-ttu-id="2b51f-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Trakstar (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2b51f-107">You can enable your users to automatically get signed-on to Trakstar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2b51f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2b51f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2b51f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2b51f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b51f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b51f-110">Prerequisites</span></span>

<span data-ttu-id="2b51f-111">Konfigurálása az Azure AD-integrációs Trakstar, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="2b51f-111">To configure Azure AD integration with Trakstar, you need the following items:</span></span>

- <span data-ttu-id="2b51f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2b51f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2b51f-113">Egy Trakstar egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2b51f-113">A Trakstar single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2b51f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="2b51f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2b51f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="2b51f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2b51f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b51f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2b51f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2b51f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2b51f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2b51f-118">Scenario description</span></span>
<span data-ttu-id="2b51f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2b51f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2b51f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2b51f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2b51f-121">A gyűjteményből Trakstar hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b51f-121">Adding Trakstar from the gallery</span></span>
2. <span data-ttu-id="2b51f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2b51f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trakstar-from-the-gallery"></a><span data-ttu-id="2b51f-123">A gyűjteményből Trakstar hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b51f-123">Adding Trakstar from the gallery</span></span>
<span data-ttu-id="2b51f-124">Az Azure AD integrálása a Trakstar konfigurálásához kell hozzáadnia Trakstar a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="2b51f-124">To configure the integration of Trakstar into Azure AD, you need to add Trakstar from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2b51f-125">**A gyűjteményből Trakstar hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2b51f-125">**To add Trakstar from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2b51f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2b51f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2b51f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2b51f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2b51f-133">Írja be a keresőmezőbe, **Trakstar**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-133">In the search box, type **Trakstar**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_search.png)

5. <span data-ttu-id="2b51f-135">Az eredmények panelen válassza ki a **Trakstar**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2b51f-135">In the results panel, select **Trakstar**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2b51f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2b51f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2b51f-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Trakstar</span><span class="sxs-lookup"><span data-stu-id="2b51f-138">In this section, you configure and test Azure AD single sign-on with Trakstar based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2b51f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Trakstar a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="2b51f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Trakstar is to a user in Azure AD.</span></span> <span data-ttu-id="2b51f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Trakstar közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2b51f-140">In other words, a link relationship between an Azure AD user and the related user in Trakstar needs to be established.</span></span>

<span data-ttu-id="2b51f-141">Trakstar, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="2b51f-141">In Trakstar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2b51f-142">Az Azure AD egyszeri bejelentkezést a Trakstar tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="2b51f-142">To configure and test Azure AD single sign-on with Trakstar, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2b51f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="2b51f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2b51f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="2b51f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2b51f-145">**[Trakstar tesztfelhasználó létrehozása](#creating-a-trakstar-test-user)**  - való Britta Simon valami Trakstar, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="2b51f-145">**[Creating a Trakstar test user](#creating-a-trakstar-test-user)** - to have a counterpart of Britta Simon in Trakstar that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2b51f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2b51f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2b51f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2b51f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2b51f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2b51f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2b51f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Trakstar alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2b51f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trakstar application.</span></span>

<span data-ttu-id="2b51f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Trakstar, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2b51f-150">**To configure Azure AD single sign-on with Trakstar, perform the following steps:**</span></span>

1. <span data-ttu-id="2b51f-151">Az Azure portálon a a **Trakstar** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-151">In the Azure portal, on the **Trakstar** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2b51f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2b51f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_samlbase.png)

3. <span data-ttu-id="2b51f-155">Az a **Trakstar tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2b51f-155">On the **Trakstar Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_url.png)

    <span data-ttu-id="2b51f-157">a.</span><span class="sxs-lookup"><span data-stu-id="2b51f-157">a.</span></span> <span data-ttu-id="2b51f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.trakstar.com/auth/saml/callback?namespace=<NAMESPACE>`</span><span class="sxs-lookup"><span data-stu-id="2b51f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.trakstar.com/auth/saml/callback?namespace=<NAMESPACE>`</span></span>

    <span data-ttu-id="2b51f-159">b.</span><span class="sxs-lookup"><span data-stu-id="2b51f-159">b.</span></span> <span data-ttu-id="2b51f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.trakstar.com`</span><span class="sxs-lookup"><span data-stu-id="2b51f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.trakstar.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2b51f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2b51f-161">These values are not real.</span></span> <span data-ttu-id="2b51f-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2b51f-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2b51f-163">Ügyfél [Trakstar ügyfél-támogatási csoport](mailto:integrations@trakstar.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2b51f-163">Contact [Trakstar Client support team](mailto:integrations@trakstar.com) to get these values.</span></span> 
 
4. <span data-ttu-id="2b51f-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2b51f-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_certificate.png) 

5. <span data-ttu-id="2b51f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2b51f-168">A a **Trakstar konfigurációs** kattintson **konfigurálása Trakstar** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2b51f-168">On the **Trakstar Configuration** section, click **Configure Trakstar** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2b51f-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2b51f-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_configure.png) 

7. <span data-ttu-id="2b51f-171">Egyszeri bejelentkezés konfigurálása **Trakstar** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Trakstar támogatási csoport](mailto:integrations@trakstar.com).</span><span class="sxs-lookup"><span data-stu-id="2b51f-171">To configure single sign-on on **Trakstar** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Trakstar support team](mailto:integrations@trakstar.com).</span></span> 

> [!TIP]
> <span data-ttu-id="2b51f-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="2b51f-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2b51f-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="2b51f-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2b51f-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2b51f-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2b51f-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b51f-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="2b51f-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="2b51f-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2b51f-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2b51f-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2b51f-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2b51f-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2b51f-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="2b51f-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2b51f-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="2b51f-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-trakstar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2b51f-187">a.</span><span class="sxs-lookup"><span data-stu-id="2b51f-187">a.</span></span> <span data-ttu-id="2b51f-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2b51f-189">b.</span><span class="sxs-lookup"><span data-stu-id="2b51f-189">b.</span></span> <span data-ttu-id="2b51f-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2b51f-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2b51f-191">c.</span><span class="sxs-lookup"><span data-stu-id="2b51f-191">c.</span></span> <span data-ttu-id="2b51f-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2b51f-193">d.</span><span class="sxs-lookup"><span data-stu-id="2b51f-193">d.</span></span> <span data-ttu-id="2b51f-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-194">Click **Create**.</span></span>
 
### <a name="creating-a-trakstar-test-user"></a><span data-ttu-id="2b51f-195">Trakstar tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b51f-195">Creating a Trakstar test user</span></span>

<span data-ttu-id="2b51f-196">Ez a szakasz célja Trakstar Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2b51f-196">The objective of this section is to create a user called Britta Simon in Trakstar.</span></span> <span data-ttu-id="2b51f-197">Együttműködve [Trakstar támogatási csoport](mailto:integrations@trakstar.com) a felhasználók hozzáadása a Trakstar fiók.</span><span class="sxs-lookup"><span data-stu-id="2b51f-197">Work with [Trakstar support team](mailto:integrations@trakstar.com) to add the users in the Trakstar account.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2b51f-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2b51f-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2b51f-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Trakstar Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="2b51f-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trakstar.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2b51f-201">**Britta Simon hozzárendelése Trakstar, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2b51f-201">**To assign Britta Simon to Trakstar, perform the following steps:**</span></span>

1. <span data-ttu-id="2b51f-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2b51f-204">Az alkalmazások listában válassza ki a **Trakstar**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-204">In the applications list, select **Trakstar**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_app.png) 

3. <span data-ttu-id="2b51f-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2b51f-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2b51f-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b51f-208">Click **Add** button.</span></span> <span data-ttu-id="2b51f-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b51f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2b51f-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2b51f-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2b51f-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b51f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2b51f-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b51f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2b51f-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2b51f-214">Testing single sign-on</span></span>

<span data-ttu-id="2b51f-215">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="2b51f-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="2b51f-216">Ha a hozzáférési panelen Trakstar csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Trakstar alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="2b51f-216">When you click the Trakstar tile in the Access Panel, you should get automatically signed-on to your Trakstar application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2b51f-217">További források</span><span class="sxs-lookup"><span data-stu-id="2b51f-217">Additional resources</span></span>

* [<span data-ttu-id="2b51f-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="2b51f-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2b51f-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2b51f-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_203.png

