---
title: "Oktatóanyag: Azure Active Directoryval integrált Skilljar |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Skilljar között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: d5238a0471b6ae4b367ca5c1ed5e1f273a0c476b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skilljar"></a><span data-ttu-id="ff07c-103">Oktatóanyag: Azure Active Directoryval integrált Skilljar</span><span class="sxs-lookup"><span data-stu-id="ff07c-103">Tutorial: Azure Active Directory integration with Skilljar</span></span>

<span data-ttu-id="ff07c-104">Ebben az oktatóanyagban elsajátíthatja Skilljar integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff07c-104">In this tutorial, you learn how to integrate Skilljar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff07c-105">Skilljar integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ff07c-105">Integrating Skilljar with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ff07c-106">Megadhatja a Skilljar hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ff07c-106">You can control in Azure AD who has access to Skilljar</span></span>
- <span data-ttu-id="ff07c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Skilljar (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ff07c-107">You can enable your users to automatically get signed-on to Skilljar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff07c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ff07c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ff07c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff07c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff07c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff07c-110">Prerequisites</span></span>

<span data-ttu-id="ff07c-111">Konfigurálása az Azure AD-integrációs Skilljar, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ff07c-111">To configure Azure AD integration with Skilljar, you need the following items:</span></span>

- <span data-ttu-id="ff07c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ff07c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff07c-113">Egy Skilljar egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="ff07c-113">A Skilljar single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff07c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ff07c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff07c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ff07c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff07c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff07c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff07c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff07c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff07c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ff07c-118">Scenario description</span></span>
<span data-ttu-id="ff07c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ff07c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff07c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ff07c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff07c-121">A gyűjteményből Skilljar hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ff07c-121">Adding Skilljar from the gallery</span></span>
2. <span data-ttu-id="ff07c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ff07c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skilljar-from-the-gallery"></a><span data-ttu-id="ff07c-123">A gyűjteményből Skilljar hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ff07c-123">Adding Skilljar from the gallery</span></span>
<span data-ttu-id="ff07c-124">Az Azure AD integrálása a Skilljar konfigurálásához kell hozzáadnia Skilljar a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ff07c-124">To configure the integration of Skilljar into Azure AD, you need to add Skilljar from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ff07c-125">**A gyűjteményből Skilljar hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ff07c-125">**To add Skilljar from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ff07c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff07c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ff07c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ff07c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ff07c-133">Írja be a keresőmezőbe, **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-133">In the search box, type **Skilljar**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_search.png)

5. <span data-ttu-id="ff07c-135">Az eredmények panelen válassza ki a **Skilljar**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ff07c-135">In the results panel, select **Skilljar**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff07c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ff07c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff07c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Skilljar.</span><span class="sxs-lookup"><span data-stu-id="ff07c-138">In this section, you configure and test Azure AD single sign-on with Skilljar based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff07c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Skilljar a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ff07c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Skilljar is to a user in Azure AD.</span></span> <span data-ttu-id="ff07c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Skilljar közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ff07c-140">In other words, a link relationship between an Azure AD user and the related user in Skilljar needs to be established.</span></span>

<span data-ttu-id="ff07c-141">Skilljar, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="ff07c-141">In Skilljar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ff07c-142">Az Azure AD egyszeri bejelentkezést a Skilljar tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ff07c-142">To configure and test Azure AD single sign-on with Skilljar, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ff07c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ff07c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ff07c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ff07c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff07c-145">**[Skilljar tesztfelhasználó létrehozása](#creating-a-skilljar-test-user)**  - való Britta Simon valami Skilljar, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ff07c-145">**[Creating a Skilljar test user](#creating-a-skilljar-test-user)** - to have a counterpart of Britta Simon in Skilljar that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff07c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ff07c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff07c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ff07c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff07c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ff07c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff07c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Skilljar alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ff07c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Skilljar application.</span></span>

<span data-ttu-id="ff07c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Skilljar, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ff07c-150">**To configure Azure AD single sign-on with Skilljar, perform the following steps:**</span></span>

1. <span data-ttu-id="ff07c-151">Az Azure portálon a a **Skilljar** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-151">In the Azure portal, on the **Skilljar** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ff07c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ff07c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_samlbase.png)

3. <span data-ttu-id="ff07c-155">Az a **Skilljar tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ff07c-155">On the **Skilljar Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_url.png)

    <span data-ttu-id="ff07c-157">a.</span><span class="sxs-lookup"><span data-stu-id="ff07c-157">a.</span></span> <span data-ttu-id="ff07c-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="ff07c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`</span></span>

    <span data-ttu-id="ff07c-159">b.</span><span class="sxs-lookup"><span data-stu-id="ff07c-159">b.</span></span> <span data-ttu-id="ff07c-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="ff07c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff07c-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ff07c-161">These values are not real.</span></span> <span data-ttu-id="ff07c-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ff07c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ff07c-163">Ügyfél [Skilljar ügyfél-támogatási csoport](http://support.skilljar.com/hc/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ff07c-163">Contact [Skilljar Client support team](http://support.skilljar.com/hc/) to get these values.</span></span> 
 
4. <span data-ttu-id="ff07c-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ff07c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_certificate.png) 

5. <span data-ttu-id="ff07c-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skilljar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ff07c-168">Egyszeri bejelentkezés konfigurálása **Skilljar** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja**, és **név azonosító formátuma érték - urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: e-mail cím** való [Skilljar támogatási csoport](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="ff07c-168">To configure single sign-on on **Skilljar** side, you need to send the downloaded **Metadata XML**, and **Name Identifier Format Value - urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** to [Skilljar support team](http://support.skilljar.com/hc/).</span></span> <span data-ttu-id="ff07c-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="ff07c-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ff07c-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ff07c-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ff07c-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ff07c-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ff07c-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff07c-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff07c-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff07c-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff07c-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ff07c-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ff07c-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ff07c-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ff07c-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff07c-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff07c-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ff07c-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff07c-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ff07c-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skilljar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff07c-185">a.</span><span class="sxs-lookup"><span data-stu-id="ff07c-185">a.</span></span> <span data-ttu-id="ff07c-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff07c-187">b.</span><span class="sxs-lookup"><span data-stu-id="ff07c-187">b.</span></span> <span data-ttu-id="ff07c-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff07c-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff07c-189">c.</span><span class="sxs-lookup"><span data-stu-id="ff07c-189">c.</span></span> <span data-ttu-id="ff07c-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ff07c-191">d.</span><span class="sxs-lookup"><span data-stu-id="ff07c-191">d.</span></span> <span data-ttu-id="ff07c-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-192">Click **Create**.</span></span>
 
### <a name="creating-a-skilljar-test-user"></a><span data-ttu-id="ff07c-193">Skilljar tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ff07c-193">Creating a Skilljar test user</span></span>

<span data-ttu-id="ff07c-194">Ez a szakasz célja Skilljar Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ff07c-194">The objective of this section is to create a user called Britta Simon in Skilljar.</span></span> <span data-ttu-id="ff07c-195">Skilljar támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="ff07c-195">Skilljar supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="ff07c-196">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="ff07c-196">There is no action item for you in this section.</span></span> <span data-ttu-id="ff07c-197">Új felhasználó jön létre az Skilljar elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="ff07c-197">A new user is created during an attempt to access Skilljar if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="ff07c-198">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Skilljar támogatási csoport](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="ff07c-198">If you need to create a user manually, you need to contact the [Skilljar support team](http://support.skilljar.com/hc/).</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ff07c-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ff07c-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ff07c-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Skilljar Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ff07c-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Skilljar.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ff07c-202">**Britta Simon hozzárendelése Skilljar, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ff07c-202">**To assign Britta Simon to Skilljar, perform the following steps:**</span></span>

1. <span data-ttu-id="ff07c-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ff07c-205">Az alkalmazások listában válassza ki a **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-205">In the applications list, select **Skilljar**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_app.png) 

3. <span data-ttu-id="ff07c-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ff07c-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ff07c-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ff07c-209">Click **Add** button.</span></span> <span data-ttu-id="ff07c-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ff07c-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ff07c-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ff07c-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ff07c-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ff07c-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff07c-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ff07c-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff07c-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ff07c-215">Testing single sign-on</span></span>

<span data-ttu-id="ff07c-216">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ff07c-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="ff07c-217">Ha a hozzáférési panelen Skilljar csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Skilljar alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="ff07c-217">When you click the Skilljar tile in the Access Panel, you should get automatically signed-on to your Skilljar application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff07c-218">További források</span><span class="sxs-lookup"><span data-stu-id="ff07c-218">Additional resources</span></span>

* [<span data-ttu-id="ff07c-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ff07c-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff07c-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ff07c-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_203.png

