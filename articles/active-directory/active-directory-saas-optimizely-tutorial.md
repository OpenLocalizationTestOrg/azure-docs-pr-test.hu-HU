---
title: "Oktatóanyag: Azure Active Directoryval integrált Optimizely |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Optimizely között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 4d6f6da6bace09fbd6ab105530a1162653675c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a><span data-ttu-id="90328-103">Oktatóanyag: Azure Active Directoryval integrált Optimizely</span><span class="sxs-lookup"><span data-stu-id="90328-103">Tutorial: Azure Active Directory integration with Optimizely</span></span>

<span data-ttu-id="90328-104">Ebben az oktatóanyagban elsajátíthatja Optimizely integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="90328-104">In this tutorial, you learn how to integrate Optimizely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="90328-105">Optimizely integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="90328-105">Integrating Optimizely with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="90328-106">Megadhatja a Optimizely hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="90328-106">You can control in Azure AD who has access to Optimizely</span></span>
- <span data-ttu-id="90328-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Optimizely (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="90328-107">You can enable your users to automatically get signed-on to Optimizely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="90328-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="90328-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="90328-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90328-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90328-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="90328-110">Prerequisites</span></span>

<span data-ttu-id="90328-111">Konfigurálása az Azure AD-integrációs Optimizely, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="90328-111">To configure Azure AD integration with Optimizely, you need the following items:</span></span>

- <span data-ttu-id="90328-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="90328-112">An Azure AD subscription</span></span>
- <span data-ttu-id="90328-113">Egy Optimizely egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="90328-113">An Optimizely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="90328-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="90328-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="90328-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="90328-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="90328-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="90328-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="90328-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90328-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="90328-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="90328-118">Scenario description</span></span>
<span data-ttu-id="90328-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="90328-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="90328-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="90328-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90328-121">A gyűjteményből Optimizely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90328-121">Adding Optimizely from the gallery</span></span>
2. <span data-ttu-id="90328-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="90328-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-optimizely-from-the-gallery"></a><span data-ttu-id="90328-123">A gyűjteményből Optimizely hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90328-123">Adding Optimizely from the gallery</span></span>
<span data-ttu-id="90328-124">Az Azure AD integrálása a Optimizely konfigurálásához kell hozzáadnia Optimizely a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="90328-124">To configure the integration of Optimizely into Azure AD, you need to add Optimizely from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="90328-125">**A gyűjteményből Optimizely hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="90328-125">**To add Optimizely from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="90328-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="90328-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="90328-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="90328-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="90328-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="90328-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="90328-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="90328-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="90328-133">Írja be a keresőmezőbe, **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="90328-133">In the search box, type **Optimizely**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. <span data-ttu-id="90328-135">Az eredmények panelen válassza ki a **Optimizely**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90328-135">In the results panel, select **Optimizely**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="90328-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="90328-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="90328-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Optimizely</span><span class="sxs-lookup"><span data-stu-id="90328-138">In this section, you configure and test Azure AD single sign-on with Optimizely based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="90328-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Optimizely a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="90328-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Optimizely is to a user in Azure AD.</span></span> <span data-ttu-id="90328-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Optimizely közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="90328-140">In other words, a link relationship between an Azure AD user and the related user in Optimizely needs to be established.</span></span>

<span data-ttu-id="90328-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Optimizely a.</span><span class="sxs-lookup"><span data-stu-id="90328-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Optimizely.</span></span>

<span data-ttu-id="90328-142">Az Azure AD egyszeri bejelentkezést a Optimizely tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="90328-142">To configure and test Azure AD single sign-on with Optimizely, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="90328-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="90328-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="90328-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="90328-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90328-145">**[Egy Optimizely tesztfelhasználó létrehozása](#creating-an-optimizely-test-user)**  - való Britta Simon valami Optimizely, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="90328-145">**[Creating an Optimizely test user](#creating-an-optimizely-test-user)** - to have a counterpart of Britta Simon in Optimizely that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="90328-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="90328-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90328-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="90328-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="90328-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90328-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="90328-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Optimizely alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="90328-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Optimizely application.</span></span>

<span data-ttu-id="90328-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Optimizely, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="90328-150">**To configure Azure AD single sign-on with Optimizely, perform the following steps:**</span></span>

1. <span data-ttu-id="90328-151">Az Azure portálon a a **Optimizely** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="90328-151">In the Azure portal, on the **Optimizely** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="90328-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="90328-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. <span data-ttu-id="90328-155">Az a **Optimizely tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="90328-155">On the **Optimizely Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    <span data-ttu-id="90328-157">a.</span><span class="sxs-lookup"><span data-stu-id="90328-157">a.</span></span> <span data-ttu-id="90328-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.optimizely.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="90328-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.optimizely.net/<instance name>`</span></span>

    <span data-ttu-id="90328-159">b.</span><span class="sxs-lookup"><span data-stu-id="90328-159">b.</span></span> <span data-ttu-id="90328-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`urn:auth0:optimizely:contoso`</span><span class="sxs-lookup"><span data-stu-id="90328-160">In the **Identifier** textbox, type a URL using the following pattern:  `urn:auth0:optimizely:contoso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="90328-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="90328-161">These values are not the real.</span></span> <span data-ttu-id="90328-162">A tényleges bejelentkezési URL-cím és azonosítója, amelynek az ismertetése, az oktatóanyag későbbi részében fogja frissítse az értéket.</span><span class="sxs-lookup"><span data-stu-id="90328-162">You will update the value with the actual Sign-on URL and Identifier, which is explained later in the tutorial.</span></span> 

4. <span data-ttu-id="90328-163">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="90328-163">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. <span data-ttu-id="90328-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="90328-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="90328-167">A a **Optimizely konfigurációs** kattintson **konfigurálása Optimizely** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="90328-167">On the **Optimizely Configuration** section, click **Configure Optimizely** to open **Configure sign-on** window.</span></span> <span data-ttu-id="90328-168">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="90328-168">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. <span data-ttu-id="90328-170">Egyszeri bejelentkezés konfigurálása **Optimizely** oldalán található, lépjen kapcsolatba a Optimizely fiókkezelővel, és adja meg a letöltött **tanúsítvány (Base64)**, és **SAML-alapú egyszeri bejelentkezési URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="90328-170">To configure single sign-on on **Optimizely** side, contact your Optimizely Account Manager and provide the downloaded **Certificate (Base64)**, and **SAML Single Sign-On Service URL**.</span></span> 

8. <span data-ttu-id="90328-171">Az e-maileket válaszul Optimizely tesz lehetővé a bejelentkezési URL-cím (a Szolgáltató által kezdeményezett SSO) és a azonosítóértékek (Service Provider entitás azonosítója).</span><span class="sxs-lookup"><span data-stu-id="90328-171">In response to your email, Optimizely provides you with the Sign On URL (SP-initiated SSO) and the Identifier (Service Provider Entity ID) values.</span></span>

    <span data-ttu-id="90328-172">a.</span><span class="sxs-lookup"><span data-stu-id="90328-172">a.</span></span> <span data-ttu-id="90328-173">Másolás a **Szolgáltató által kezdeményezett egyszeri bejelentkezési URL-cím** Optimizely, és illessze be a megadott a **URL-cím bejelentkezési** textbox **Optimizely tartomány és az URL-címek** szakaszt, Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="90328-173">Copy the **SP-initiated SSO URL** provided by Optimizely, and paste into the **Sign On URL** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

    <span data-ttu-id="90328-174">b.</span><span class="sxs-lookup"><span data-stu-id="90328-174">b.</span></span> <span data-ttu-id="90328-175">Másolás a **Service Provider Entitásazonosító** Optimizely, és illessze be a megadott a **azonosító** textbox **Optimizely tartomány és az URL-címek** szakaszt, Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="90328-175">Copy the **Service Provider Entity ID** provided by Optimizely, and paste into the **Identifier** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

9. <span data-ttu-id="90328-176">Egy másik böngészőablakban bejelentkezés az Optimizely alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="90328-176">In a different browser window, sign-on to your Optimizely application.</span></span>

10. <span data-ttu-id="90328-177">Kattintson, akkor a fiók neve felső sarokban, majd **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="90328-177">Click you account name in the top right corner and then **Account Settings**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. <span data-ttu-id="90328-179">A fiók lapon jelölje be a jelölőnégyzetet **SSO engedélyezése** az egyszeri bejelentkezés a a **áttekintése** szakasz.</span><span class="sxs-lookup"><span data-stu-id="90328-179">In the Account tab, check the box **Enable SSO** under Single Sign On in the **Overview** section.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. <span data-ttu-id="90328-181">Kattintson a **mentése**</span><span class="sxs-lookup"><span data-stu-id="90328-181">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="90328-182">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="90328-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="90328-183">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="90328-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="90328-184">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="90328-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="90328-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="90328-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="90328-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="90328-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="90328-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="90328-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="90328-189">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="90328-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="90328-191">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="90328-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="90328-193">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="90328-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="90328-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="90328-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="90328-197">a.</span><span class="sxs-lookup"><span data-stu-id="90328-197">a.</span></span> <span data-ttu-id="90328-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90328-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="90328-199">b.</span><span class="sxs-lookup"><span data-stu-id="90328-199">b.</span></span> <span data-ttu-id="90328-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90328-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="90328-201">c.</span><span class="sxs-lookup"><span data-stu-id="90328-201">c.</span></span> <span data-ttu-id="90328-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="90328-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="90328-203">d.</span><span class="sxs-lookup"><span data-stu-id="90328-203">d.</span></span> <span data-ttu-id="90328-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="90328-204">Click **Create**.</span></span>
 
### <a name="creating-an-optimizely-test-user"></a><span data-ttu-id="90328-205">Egy Optimizely tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="90328-205">Creating an Optimizely test user</span></span>

<span data-ttu-id="90328-206">Ebben a szakaszban egy Optimizely Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="90328-206">In this section, you create a user called Britta Simon in Optimizely.</span></span>

1. <span data-ttu-id="90328-207">Válassza ki a kezdőlapon **közreműködők** fülre.</span><span class="sxs-lookup"><span data-stu-id="90328-207">On the home page, select **Collaborators** tab.</span></span>

2. <span data-ttu-id="90328-208">Kattintson az új közreműködő hozzáadása a projekthez, **új közreműködő**.</span><span class="sxs-lookup"><span data-stu-id="90328-208">To add new collaborator to the project, click **New Collaborator**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. <span data-ttu-id="90328-210">Adja meg az e-mail címet, és szerepkört rendel hozzá.</span><span class="sxs-lookup"><span data-stu-id="90328-210">Fill in the email address and assign them a role.</span></span> <span data-ttu-id="90328-211">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="90328-211">Click **Invite**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. <span data-ttu-id="90328-213">Az e-mailt a meghívás kapnak.</span><span class="sxs-lookup"><span data-stu-id="90328-213">They receive an email invite.</span></span> <span data-ttu-id="90328-214">Rendelkeznek Optimizely jelentkezzenek be az e-mail címet használja.</span><span class="sxs-lookup"><span data-stu-id="90328-214">Using the email address, they have to log in to Optimizely.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="90328-215">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="90328-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="90328-216">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Optimizely Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="90328-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Optimizely.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="90328-218">**Britta Simon hozzárendelése Optimizely, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="90328-218">**To assign Britta Simon to Optimizely, perform the following steps:**</span></span>

1. <span data-ttu-id="90328-219">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="90328-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="90328-221">Az alkalmazások listában válassza ki a **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="90328-221">In the applications list, select **Optimizely**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. <span data-ttu-id="90328-223">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="90328-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="90328-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="90328-225">Click **Add** button.</span></span> <span data-ttu-id="90328-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90328-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="90328-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="90328-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="90328-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90328-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="90328-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="90328-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="90328-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="90328-231">Testing single sign-on</span></span>

<span data-ttu-id="90328-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="90328-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="90328-233">Ha a hozzáférési panelen Optimizely csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Optimizely alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="90328-233">When you click the Optimizely tile in the Access Panel, you should get automatically signed-on to your Optimizely application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="90328-234">További források</span><span class="sxs-lookup"><span data-stu-id="90328-234">Additional resources</span></span>

* [<span data-ttu-id="90328-235">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="90328-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90328-236">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="90328-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

