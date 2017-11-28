---
title: "Oktatóanyag: Azure Active Directoryval integrált Wikispaces |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Wikispaces között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d01543955bdf6a274571f67eafdff5f637863d5c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a><span data-ttu-id="992a3-103">Oktatóanyag: Azure Active Directoryval integrált Wikispaces</span><span class="sxs-lookup"><span data-stu-id="992a3-103">Tutorial: Azure Active Directory integration with Wikispaces</span></span>

<span data-ttu-id="992a3-104">Ebben az oktatóanyagban elsajátíthatja Wikispaces integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="992a3-104">In this tutorial, you learn how to integrate Wikispaces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="992a3-105">Wikispaces integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="992a3-105">Integrating Wikispaces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="992a3-106">Megadhatja a Wikispaces hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="992a3-106">You can control in Azure AD who has access to Wikispaces</span></span>
- <span data-ttu-id="992a3-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Wikispaces (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="992a3-107">You can enable your users to automatically get signed-on to Wikispaces (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="992a3-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="992a3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="992a3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="992a3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="992a3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="992a3-110">Prerequisites</span></span>

<span data-ttu-id="992a3-111">Konfigurálása az Azure AD-integrációs Wikispaces, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="992a3-111">To configure Azure AD integration with Wikispaces, you need the following items:</span></span>

- <span data-ttu-id="992a3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="992a3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="992a3-113">Egy Wikispaces egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="992a3-113">A Wikispaces single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="992a3-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="992a3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="992a3-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="992a3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="992a3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="992a3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="992a3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="992a3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="992a3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="992a3-118">Scenario description</span></span>
<span data-ttu-id="992a3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="992a3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="992a3-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="992a3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="992a3-121">A gyűjteményből Wikispaces hozzáadása</span><span class="sxs-lookup"><span data-stu-id="992a3-121">Adding Wikispaces from the gallery</span></span>
2. <span data-ttu-id="992a3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="992a3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wikispaces-from-the-gallery"></a><span data-ttu-id="992a3-123">A gyűjteményből Wikispaces hozzáadása</span><span class="sxs-lookup"><span data-stu-id="992a3-123">Adding Wikispaces from the gallery</span></span>
<span data-ttu-id="992a3-124">Az Azure AD integrálása a Wikispaces konfigurálásához kell hozzáadnia Wikispaces a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="992a3-124">To configure the integration of Wikispaces into Azure AD, you need to add Wikispaces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="992a3-125">**A gyűjteményből Wikispaces hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="992a3-125">**To add Wikispaces from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="992a3-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="992a3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="992a3-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="992a3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="992a3-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="992a3-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="992a3-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="992a3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="992a3-133">Írja be a keresőmezőbe, **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="992a3-133">In the search box, type **Wikispaces**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_search.png)

5. <span data-ttu-id="992a3-135">Az eredmények panelen válassza ki a **Wikispaces**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="992a3-135">In the results panel, select **Wikispaces**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="992a3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="992a3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="992a3-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="992a3-138">In this section, you configure and test Azure AD single sign-on with Wikispaces based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="992a3-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Wikispaces a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="992a3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Wikispaces is to a user in Azure AD.</span></span> <span data-ttu-id="992a3-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Wikispaces közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="992a3-140">In other words, a link relationship between an Azure AD user and the related user in Wikispaces needs to be established.</span></span>

<span data-ttu-id="992a3-141">Wikispaces, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="992a3-141">In Wikispaces, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="992a3-142">Az Azure AD egyszeri bejelentkezést a Wikispaces tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="992a3-142">To configure and test Azure AD single sign-on with Wikispaces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="992a3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="992a3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="992a3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="992a3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="992a3-145">**[Wikispaces tesztfelhasználó létrehozása](#creating-a-wikispaces-test-user)**  - való Britta Simon valami Wikispaces, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="992a3-145">**[Creating a Wikispaces test user](#creating-a-wikispaces-test-user)** - to have a counterpart of Britta Simon in Wikispaces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="992a3-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="992a3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="992a3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="992a3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="992a3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="992a3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="992a3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Wikispaces alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="992a3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wikispaces application.</span></span>

<span data-ttu-id="992a3-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Wikispaces, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="992a3-150">**To configure Azure AD single sign-on with Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="992a3-151">Az Azure portálon a a **Wikispaces** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="992a3-151">In the Azure portal, on the **Wikispaces** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="992a3-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="992a3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. <span data-ttu-id="992a3-155">Az a **Wikispaces tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="992a3-155">On the **Wikispaces Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_url.png)

    <span data-ttu-id="992a3-157">a.</span><span class="sxs-lookup"><span data-stu-id="992a3-157">a.</span></span> <span data-ttu-id="992a3-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.wikispaces.net`</span><span class="sxs-lookup"><span data-stu-id="992a3-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.wikispaces.net`</span></span>

    <span data-ttu-id="992a3-159">b.</span><span class="sxs-lookup"><span data-stu-id="992a3-159">b.</span></span> <span data-ttu-id="992a3-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://session.wikispaces.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="992a3-160">In the **Identifier** textbox, type a URL using the following pattern: `https://session.wikispaces.net/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="992a3-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="992a3-161">These values are not real.</span></span> <span data-ttu-id="992a3-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="992a3-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="992a3-163">Ügyfél [Wikispaces ügyfél-támogatási csoport](https://www.wikispaces.com/site/help) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="992a3-163">Contact [Wikispaces Client support team](https://www.wikispaces.com/site/help) to get these values.</span></span> 

4. <span data-ttu-id="992a3-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="992a3-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. <span data-ttu-id="992a3-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="992a3-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="992a3-168">Egyszeri bejelentkezés konfigurálása **Wikispaces** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Wikispaces támogatási csoport](https://www.wikispaces.com/site/help).</span><span class="sxs-lookup"><span data-stu-id="992a3-168">To configure single sign-on on **Wikispaces** side, you need to send the downloaded **Metadata XML** to [Wikispaces support team](https://www.wikispaces.com/site/help).</span></span> <span data-ttu-id="992a3-169">Értesítést kap, amint a konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="992a3-169">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="992a3-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="992a3-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="992a3-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="992a3-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="992a3-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="992a3-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="992a3-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="992a3-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="992a3-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="992a3-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="992a3-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="992a3-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="992a3-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="992a3-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="992a3-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="992a3-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="992a3-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="992a3-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="992a3-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="992a3-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="992a3-185">a.</span><span class="sxs-lookup"><span data-stu-id="992a3-185">a.</span></span> <span data-ttu-id="992a3-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="992a3-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="992a3-187">b.</span><span class="sxs-lookup"><span data-stu-id="992a3-187">b.</span></span> <span data-ttu-id="992a3-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="992a3-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="992a3-189">c.</span><span class="sxs-lookup"><span data-stu-id="992a3-189">c.</span></span> <span data-ttu-id="992a3-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="992a3-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="992a3-191">d.</span><span class="sxs-lookup"><span data-stu-id="992a3-191">d.</span></span> <span data-ttu-id="992a3-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="992a3-192">Click **Create**.</span></span>
 
### <a name="creating-a-wikispaces-test-user"></a><span data-ttu-id="992a3-193">Wikispaces tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="992a3-193">Creating a Wikispaces test user</span></span>

<span data-ttu-id="992a3-194">Ahhoz, hogy az Azure AD-felhasználók Wikispaces bejelentkezni, akkor ki kell építenie a Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="992a3-194">In order to enable Azure AD users to log in to Wikispaces, they must be provisioned into Wikispaces.</span></span> <span data-ttu-id="992a3-195">Wikispaces, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="992a3-195">In the case of Wikispaces, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="992a3-196">A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="992a3-196">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="992a3-197">Jelentkezzen be a **Wikispaces** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="992a3-197">Log in to your **Wikispaces** company site as an administrator.</span></span>

2. <span data-ttu-id="992a3-198">Ugrás a **tagok**.</span><span class="sxs-lookup"><span data-stu-id="992a3-198">Go to **Members**.</span></span>
   
    <span data-ttu-id="992a3-199">![Tagok](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "tagok")</span><span class="sxs-lookup"><span data-stu-id="992a3-199">![Members](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "Members")</span></span>

3. <span data-ttu-id="992a3-200">Kattintson a **felkérése**.</span><span class="sxs-lookup"><span data-stu-id="992a3-200">Click the **Invite People**.</span></span>
   
    <span data-ttu-id="992a3-201">![Felkérése](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="992a3-201">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "Invite People")</span></span>

4. <span data-ttu-id="992a3-202">Az a **meghívása személyek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="992a3-202">In the **Invite People** section, perform the following steps:</span></span>
   
    <span data-ttu-id="992a3-203">![Felkérése](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "felkérése")</span><span class="sxs-lookup"><span data-stu-id="992a3-203">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "Invite People")</span></span>
   
    <span data-ttu-id="992a3-204">a.</span><span class="sxs-lookup"><span data-stu-id="992a3-204">a.</span></span> <span data-ttu-id="992a3-205">Típus a **felhasználónevek vagy E-mail cím** szeretné azokat a kapcsolódó szövegmezők rendelkezés érvényes AAD-fiók.</span><span class="sxs-lookup"><span data-stu-id="992a3-205">Type the **Usernames or Email Address** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="992a3-206">b.</span><span class="sxs-lookup"><span data-stu-id="992a3-206">b.</span></span> <span data-ttu-id="992a3-207">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="992a3-207">Click **Send**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="992a3-208">Az Azure Active Directory fióktulajdonos kap egy e-mailt hivatkozással erősítse meg a fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="992a3-208">The Azure Active Directory account holder receives an email including a link to confirm the account before it becomes active.</span></span>
    
> [!NOTE]
> <span data-ttu-id="992a3-209">Bármely más Wikispaces felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Wikispaces által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="992a3-209">You can use any other Wikispaces user account creation tools or APIs provided by Wikispaces to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="992a3-210">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="992a3-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="992a3-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Wikispaces Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="992a3-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wikispaces.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="992a3-213">**Britta Simon hozzárendelése Wikispaces, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="992a3-213">**To assign Britta Simon to Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="992a3-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="992a3-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="992a3-216">Az alkalmazások listában válassza ki a **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="992a3-216">In the applications list, select **Wikispaces**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. <span data-ttu-id="992a3-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="992a3-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="992a3-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="992a3-220">Click **Add** button.</span></span> <span data-ttu-id="992a3-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="992a3-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="992a3-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="992a3-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="992a3-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="992a3-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="992a3-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="992a3-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="992a3-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="992a3-226">Testing single sign-on</span></span>

<span data-ttu-id="992a3-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="992a3-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="992a3-228">Ha a hozzáférési panelen Wikispaces csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Wikispaces alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="992a3-228">When you click the Wikispaces tile in the Access Panel, you should get automatically signed-on to your Wikispaces application.</span></span>
<span data-ttu-id="992a3-229">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="992a3-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="992a3-230">További források</span><span class="sxs-lookup"><span data-stu-id="992a3-230">Additional resources</span></span>

* [<span data-ttu-id="992a3-231">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="992a3-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="992a3-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="992a3-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_203.png

