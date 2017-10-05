---
title: "Oktatóanyag: Azure Active Directoryval integrált Cherwell |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Cherwell között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: jeedes
ms.openlocfilehash: 27fedb9caa1ef27693b2267df285d62aab78bc24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a><span data-ttu-id="c98ec-103">Oktatóanyag: Azure Active Directoryval integrált Cherwell</span><span class="sxs-lookup"><span data-stu-id="c98ec-103">Tutorial: Azure Active Directory integration with Cherwell</span></span>

<span data-ttu-id="c98ec-104">Ebben az oktatóanyagban elsajátíthatja Cherwell integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c98ec-104">In this tutorial, you learn how to integrate Cherwell with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c98ec-105">Cherwell integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c98ec-105">Integrating Cherwell with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c98ec-106">Megadhatja a Cherwell hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c98ec-106">You can control in Azure AD who has access to Cherwell</span></span>
- <span data-ttu-id="c98ec-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Cherwell (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c98ec-107">You can enable your users to automatically get signed-on to Cherwell (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c98ec-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c98ec-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c98ec-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c98ec-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c98ec-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c98ec-110">Prerequisites</span></span>

<span data-ttu-id="c98ec-111">Konfigurálása az Azure AD-integrációs Cherwell, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c98ec-111">To configure Azure AD integration with Cherwell, you need the following items:</span></span>

- <span data-ttu-id="c98ec-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c98ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c98ec-113">Egy Cherwell egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="c98ec-113">A Cherwell single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c98ec-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c98ec-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c98ec-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c98ec-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c98ec-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c98ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c98ec-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c98ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c98ec-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c98ec-118">Scenario description</span></span>
<span data-ttu-id="c98ec-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c98ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c98ec-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c98ec-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c98ec-121">A gyűjteményből Cherwell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c98ec-121">Adding Cherwell from the gallery</span></span>
2. <span data-ttu-id="c98ec-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c98ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cherwell-from-the-gallery"></a><span data-ttu-id="c98ec-123">A gyűjteményből Cherwell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c98ec-123">Adding Cherwell from the gallery</span></span>
<span data-ttu-id="c98ec-124">Az Azure AD integrálása a Cherwell konfigurálásához kell hozzáadnia Cherwell a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c98ec-124">To configure the integration of Cherwell into Azure AD, you need to add Cherwell from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c98ec-125">**A gyűjteményből Cherwell hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c98ec-125">**To add Cherwell from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c98ec-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c98ec-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c98ec-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c98ec-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c98ec-133">Írja be a keresőmezőbe, **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-133">In the search box, type **Cherwell**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_search.png)

5. <span data-ttu-id="c98ec-135">Az eredmények panelen válassza ki a **Cherwell**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c98ec-135">In the results panel, select **Cherwell**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c98ec-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c98ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c98ec-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Cherwell</span><span class="sxs-lookup"><span data-stu-id="c98ec-138">In this section, you configure and test Azure AD single sign-on with Cherwell based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c98ec-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Cherwell a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c98ec-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cherwell is to a user in Azure AD.</span></span> <span data-ttu-id="c98ec-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Cherwell közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c98ec-140">In other words, a link relationship between an Azure AD user and the related user in Cherwell needs to be established.</span></span>

<span data-ttu-id="c98ec-141">Cherwell, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c98ec-141">In Cherwell, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c98ec-142">Az Azure AD egyszeri bejelentkezést a Cherwell tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c98ec-142">To configure and test Azure AD single sign-on with Cherwell, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c98ec-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c98ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c98ec-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c98ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c98ec-145">**[Cherwell tesztfelhasználó létrehozása](#creating-a-cherwell-test-user)**  - való Britta Simon valami Cherwell, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c98ec-145">**[Creating a Cherwell test user](#creating-a-cherwell-test-user)** - to have a counterpart of Britta Simon in Cherwell that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c98ec-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c98ec-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c98ec-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c98ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c98ec-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c98ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c98ec-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Cherwell alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c98ec-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cherwell application.</span></span>

<span data-ttu-id="c98ec-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Cherwell, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c98ec-150">**To configure Azure AD single sign-on with Cherwell, perform the following steps:**</span></span>

1. <span data-ttu-id="c98ec-151">Az Azure portálon a a **Cherwell** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-151">In the Azure portal, on the **Cherwell** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c98ec-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c98ec-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_samlbase.png)

3. <span data-ttu-id="c98ec-155">Az a **Cherwell tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c98ec-155">On the **Cherwell Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_url.png)

    <span data-ttu-id="c98ec-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.cherwellondemand.com/cherwellclient`</span><span class="sxs-lookup"><span data-stu-id="c98ec-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.cherwellondemand.com/cherwellclient`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c98ec-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c98ec-158">This value is not real.</span></span> <span data-ttu-id="c98ec-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c98ec-159">Update this value with the actual Sign-on URL.</span></span> <span data-ttu-id="c98ec-160">Ügyfél [Cherwell támogatási csoport](https://csm.cherwell.com/contact) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c98ec-160">Contact [Cherwell support team](https://csm.cherwell.com/contact) to get this value.</span></span>
 
4. <span data-ttu-id="c98ec-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c98ec-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_certificate.png) 

5. <span data-ttu-id="c98ec-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c98ec-165">A a **Cherwell konfigurációs** kattintson **konfigurálása Cherwell** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c98ec-165">On the **Cherwell Configuration** section, click **Configure Cherwell** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c98ec-166">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c98ec-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_configure.png) 

7. <span data-ttu-id="c98ec-168">Egyszeri bejelentkezés konfigurálása **Cherwell** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **SAML-alapú egyszeri bejelentkezési URL-címe**, és **SAML Entitásazonosító** való [Cherwell támogatási csoport](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="c98ec-168">To configure single sign-on on **Cherwell** side, you need to send the downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Cherwell support team](https://csm.cherwell.com/contact).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="c98ec-169">A Cherwell terméktámogató csapat rendelkezésére áll, a tényleges SSO konfigurációs elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="c98ec-169">Your Cherwell support team has to do the actual SSO configuration.</span></span> <span data-ttu-id="c98ec-170">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="c98ec-170">You will get a notification when SSO has been enabled for your subscription.</span></span>
    > 
    
> [!TIP]
> <span data-ttu-id="c98ec-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c98ec-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c98ec-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c98ec-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c98ec-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c98ec-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c98ec-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c98ec-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="c98ec-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c98ec-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c98ec-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c98ec-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c98ec-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c98ec-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c98ec-182">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c98ec-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c98ec-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c98ec-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cherwell-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c98ec-186">a.</span><span class="sxs-lookup"><span data-stu-id="c98ec-186">a.</span></span> <span data-ttu-id="c98ec-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c98ec-188">b.</span><span class="sxs-lookup"><span data-stu-id="c98ec-188">b.</span></span> <span data-ttu-id="c98ec-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c98ec-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c98ec-190">c.</span><span class="sxs-lookup"><span data-stu-id="c98ec-190">c.</span></span> <span data-ttu-id="c98ec-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c98ec-192">d.</span><span class="sxs-lookup"><span data-stu-id="c98ec-192">d.</span></span> <span data-ttu-id="c98ec-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-193">Click **Create**.</span></span>
 
### <a name="creating-a-cherwell-test-user"></a><span data-ttu-id="c98ec-194">Cherwell tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c98ec-194">Creating a Cherwell test user</span></span>

<span data-ttu-id="c98ec-195">Ahhoz, hogy az Azure AD-felhasználók Cherwell bejelentkezni, akkor ki kell építenie a Cherwell.</span><span class="sxs-lookup"><span data-stu-id="c98ec-195">To enable Azure AD users to log in to Cherwell, they must be provisioned into Cherwell.</span></span>

<span data-ttu-id="c98ec-196">Cherwell, esetén a felhasználói fiókokat kell létrehozni a [Cherwell támogatási csoport](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="c98ec-196">In the case of Cherwell, the user accounts need to be created by your [Cherwell support team](https://csm.cherwell.com/contact).</span></span>

>[!NOTE]
><span data-ttu-id="c98ec-197">Bármely más Cherwell felhasználói fiók létrehozása eszközök vagy API-k által biztosított Cherwell kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="c98ec-197">You can use any other Cherwell user account creation tools or APIs provided by Cherwell to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c98ec-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c98ec-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c98ec-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Cherwell Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c98ec-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cherwell.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c98ec-201">**Britta Simon hozzárendelése Cherwell, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c98ec-201">**To assign Britta Simon to Cherwell, perform the following steps:**</span></span>

1. <span data-ttu-id="c98ec-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c98ec-204">Az alkalmazások listában válassza ki a **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-204">In the applications list, select **Cherwell**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_app.png) 

3. <span data-ttu-id="c98ec-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c98ec-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c98ec-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c98ec-208">Click **Add** button.</span></span> <span data-ttu-id="c98ec-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c98ec-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c98ec-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c98ec-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c98ec-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c98ec-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c98ec-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c98ec-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c98ec-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c98ec-214">Testing single sign-on</span></span>

<span data-ttu-id="c98ec-215">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="c98ec-215">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="c98ec-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c98ec-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c98ec-217">További források</span><span class="sxs-lookup"><span data-stu-id="c98ec-217">Additional resources</span></span>

* [<span data-ttu-id="c98ec-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c98ec-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c98ec-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c98ec-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_203.png

