---
title: "Oktatóanyag: Azure Active Directoryval integrált Alcumus Info Exchange |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Alcumus adatai az Exchange között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 1f67682111de0bea1b18fd97d739492661ebbfd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a><span data-ttu-id="7dc89-103">Oktatóanyag: Azure Active Directoryval integrált Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="7dc89-103">Tutorial: Azure Active Directory integration with Alcumus Info Exchange</span></span>

<span data-ttu-id="7dc89-104">Ebben az oktatóanyagban elsajátíthatja Alcumus Info Exchange integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7dc89-104">In this tutorial, you learn how to integrate Alcumus Info Exchange with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7dc89-105">Alcumus Info Exchange integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7dc89-105">Integrating Alcumus Info Exchange with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7dc89-106">Megadhatja a Alcumus Info Exchange hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7dc89-106">You can control in Azure AD who has access to Alcumus Info Exchange</span></span>
- <span data-ttu-id="7dc89-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Alcumus Info Exchange (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="7dc89-107">You can enable your users to automatically get signed-on to Alcumus Info Exchange (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7dc89-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7dc89-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7dc89-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7dc89-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dc89-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7dc89-110">Prerequisites</span></span>

<span data-ttu-id="7dc89-111">Konfigurálhatja az Azure AD-integrációs Alcumus adatait, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="7dc89-111">To configure Azure AD integration with Alcumus Info Exchange, you need the following items:</span></span>

- <span data-ttu-id="7dc89-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7dc89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7dc89-113">Egy Alcumus Info Exchange egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="7dc89-113">An Alcumus Info Exchange single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7dc89-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="7dc89-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7dc89-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="7dc89-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7dc89-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7dc89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7dc89-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7dc89-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7dc89-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7dc89-118">Scenario description</span></span>
<span data-ttu-id="7dc89-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7dc89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7dc89-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7dc89-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7dc89-121">A gyűjteményből Alcumus Info Exchange hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7dc89-121">Adding Alcumus Info Exchange from the gallery</span></span>
2. <span data-ttu-id="7dc89-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7dc89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-alcumus-info-exchange-from-the-gallery"></a><span data-ttu-id="7dc89-123">A gyűjteményből Alcumus Info Exchange hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7dc89-123">Adding Alcumus Info Exchange from the gallery</span></span>
<span data-ttu-id="7dc89-124">Az Azure AD integrálása a Alcumus Info Exchange konfigurálásához kell hozzáadnia Alcumus Info Exchange a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="7dc89-124">To configure the integration of Alcumus Info Exchange into Azure AD, you need to add Alcumus Info Exchange from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7dc89-125">**A gyűjteményből Alcumus Info Exchange hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7dc89-125">**To add Alcumus Info Exchange from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7dc89-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7dc89-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7dc89-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7dc89-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7dc89-133">Írja be a keresőmezőbe, **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-133">In the search box, type **Alcumus Info Exchange**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. <span data-ttu-id="7dc89-135">Az eredmények panelen válassza ki a **Alcumus Info Exchange**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7dc89-135">In the results panel, select **Alcumus Info Exchange**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7dc89-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7dc89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7dc89-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az Alcumus Info Exchange "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="7dc89-138">In this section, you configure and test Azure AD single sign-on with Alcumus Info Exchange based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7dc89-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Alcumus Info Exchange Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7dc89-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Alcumus Info Exchange is to a user in Azure AD.</span></span> <span data-ttu-id="7dc89-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Alcumus Info Exchange közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7dc89-140">In other words, a link relationship between an Azure AD user and the related user in Alcumus Info Exchange needs to be established.</span></span>

<span data-ttu-id="7dc89-141">Alcumus Info Exchange, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7dc89-141">In Alcumus Info Exchange, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7dc89-142">Az Azure AD egyszeri bejelentkezést az Alcumus Info Exchange tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="7dc89-142">To configure and test Azure AD single sign-on with Alcumus Info Exchange, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7dc89-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="7dc89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7dc89-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7dc89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7dc89-145">**[Egy Alcumus Info Exchange tesztfelhasználó létrehozása](#creating-an-alcumus-info-exchange-test-user)**  - való egy megfelelője a Britta Simon Alcumus Info Exchange, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="7dc89-145">**[Creating an Alcumus Info Exchange test user](#creating-an-alcumus-info-exchange-test-user)** - to have a counterpart of Britta Simon in Alcumus Info Exchange that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7dc89-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7dc89-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7dc89-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7dc89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7dc89-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7dc89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7dc89-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az Alcumus adatai az Exchange alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7dc89-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Alcumus Info Exchange application.</span></span>

<span data-ttu-id="7dc89-150">**Konfigurálhatja az Azure AD az egyszeri bejelentkezés Alcumus adatait, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7dc89-150">**To configure Azure AD single sign-on with Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="7dc89-151">Az Azure portálon a a **Alcumus Info Exchange** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-151">In the Azure portal, on the **Alcumus Info Exchange** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7dc89-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7dc89-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. <span data-ttu-id="7dc89-155">Az a **Alcumus Info Exchange tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="7dc89-155">On the **Alcumus Info Exchange Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    <span data-ttu-id="7dc89-157">a.</span><span class="sxs-lookup"><span data-stu-id="7dc89-157">a.</span></span> <span data-ttu-id="7dc89-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.info-exchange.com`</span><span class="sxs-lookup"><span data-stu-id="7dc89-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com`</span></span>

    <span data-ttu-id="7dc89-159">b.</span><span class="sxs-lookup"><span data-stu-id="7dc89-159">b.</span></span> <span data-ttu-id="7dc89-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.info-exchange.com/Auth/`</span><span class="sxs-lookup"><span data-stu-id="7dc89-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com/Auth/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7dc89-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7dc89-161">These values are not real.</span></span> <span data-ttu-id="7dc89-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="7dc89-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="7dc89-163">Ügyfél [Alcumus Info Exchange támogatási csoport](mailto:helpdesk@alcumusgroup.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7dc89-163">Contact [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com) to get these values.</span></span>
 
4. <span data-ttu-id="7dc89-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7dc89-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. <span data-ttu-id="7dc89-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7dc89-168">Egyszeri bejelentkezés konfigurálása **Alcumus Info Exchange** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Alcumus Info Exchange támogatási csoport](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="7dc89-168">To configure single sign-on on **Alcumus Info Exchange** side, you need to send the downloaded **Metadata XML** to [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

> [!TIP]
> <span data-ttu-id="7dc89-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="7dc89-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7dc89-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="7dc89-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7dc89-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7dc89-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7dc89-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7dc89-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="7dc89-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="7dc89-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7dc89-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7dc89-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7dc89-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7dc89-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7dc89-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="7dc89-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7dc89-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="7dc89-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7dc89-184">a.</span><span class="sxs-lookup"><span data-stu-id="7dc89-184">a.</span></span> <span data-ttu-id="7dc89-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7dc89-186">b.</span><span class="sxs-lookup"><span data-stu-id="7dc89-186">b.</span></span> <span data-ttu-id="7dc89-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7dc89-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7dc89-188">c.</span><span class="sxs-lookup"><span data-stu-id="7dc89-188">c.</span></span> <span data-ttu-id="7dc89-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7dc89-190">d.</span><span class="sxs-lookup"><span data-stu-id="7dc89-190">d.</span></span> <span data-ttu-id="7dc89-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-191">Click **Create**.</span></span>
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a><span data-ttu-id="7dc89-192">Egy Alcumus Info Exchange tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7dc89-192">Creating an Alcumus Info Exchange test user</span></span>

<span data-ttu-id="7dc89-193">Ez a szakasz célja Alcumus Info Exchange Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7dc89-193">The objective of this section is to create a user called Britta Simon in Alcumus Info Exchange.</span></span>

<span data-ttu-id="7dc89-194">Alcumus Info Exchange Britta Simon nevű felhasználót kell létrehozni, lépjen kapcsolatba a [Alcumus Info Exchange támogatási csoport](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="7dc89-194">To create a user called Britta Simon in Alcumus Info Exchange, Contact the [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7dc89-195">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7dc89-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7dc89-196">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó Alcumus adatai az Exchange-hozzáférés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7dc89-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Alcumus Info Exchange.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7dc89-198">**Britta Simon hozzárendelése Alcumus Info Exchange, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="7dc89-198">**To assign Britta Simon to Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="7dc89-199">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7dc89-201">Az alkalmazások listában válassza ki a **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-201">In the applications list, select **Alcumus Info Exchange**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. <span data-ttu-id="7dc89-203">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7dc89-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7dc89-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7dc89-205">Click **Add** button.</span></span> <span data-ttu-id="7dc89-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7dc89-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7dc89-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7dc89-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7dc89-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7dc89-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7dc89-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7dc89-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7dc89-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7dc89-211">Testing single sign-on</span></span>

<span data-ttu-id="7dc89-212">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="7dc89-212">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="7dc89-213">Ha a hozzáférési panelen Alcumus Info Exchange csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Alcumus Info Exchange alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7dc89-213">When you click the Alcumus Info Exchange tile in the Access Panel, you should get automatically signed-on to your Alcumus Info Exchange application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7dc89-214">További források</span><span class="sxs-lookup"><span data-stu-id="7dc89-214">Additional resources</span></span>

* [<span data-ttu-id="7dc89-215">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="7dc89-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7dc89-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7dc89-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png

