---
title: "Oktatóanyag: Azure Active Directoryval integrált MobileXpense |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és MobileXpense között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e649fc4e-3e15-4948-b977-00bfe9f7db13
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: jeedes
ms.openlocfilehash: 030a1fc9f36d6fcfa607552d85ce232e36eaa64b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mobilexpense"></a><span data-ttu-id="4be4c-103">Oktatóanyag: Azure Active Directoryval integrált MobileXpense</span><span class="sxs-lookup"><span data-stu-id="4be4c-103">Tutorial: Azure Active Directory integration with MobileXpense</span></span>

<span data-ttu-id="4be4c-104">Ebben az oktatóanyagban elsajátíthatja MobileXpense integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4be4c-104">In this tutorial, you learn how to integrate MobileXpense with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4be4c-105">MobileXpense integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="4be4c-105">Integrating MobileXpense with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4be4c-106">Megadhatja a MobileXpense hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4be4c-106">You can control in Azure AD who has access to MobileXpense</span></span>
- <span data-ttu-id="4be4c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett MobileXpense (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4be4c-107">You can enable your users to automatically get signed-on to MobileXpense (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4be4c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4be4c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4be4c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4be4c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4be4c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4be4c-110">Prerequisites</span></span>

<span data-ttu-id="4be4c-111">Konfigurálása az Azure AD-integrációs MobileXpense, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4be4c-111">To configure Azure AD integration with MobileXpense, you need the following items:</span></span>

- <span data-ttu-id="4be4c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4be4c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4be4c-113">Egy MobileXpense egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="4be4c-113">A MobileXpense single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4be4c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4be4c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4be4c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="4be4c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4be4c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4be4c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4be4c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4be4c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4be4c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4be4c-118">Scenario description</span></span>
<span data-ttu-id="4be4c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4be4c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4be4c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4be4c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4be4c-121">A gyűjteményből MobileXpense hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4be4c-121">Adding MobileXpense from the gallery</span></span>
2. <span data-ttu-id="4be4c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4be4c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mobilexpense-from-the-gallery"></a><span data-ttu-id="4be4c-123">A gyűjteményből MobileXpense hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4be4c-123">Adding MobileXpense from the gallery</span></span>
<span data-ttu-id="4be4c-124">Az Azure AD integrálása a MobileXpense konfigurálásához kell hozzáadnia MobileXpense a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="4be4c-124">To configure the integration of MobileXpense into Azure AD, you need to add MobileXpense from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4be4c-125">**A gyűjteményből MobileXpense hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4be4c-125">**To add MobileXpense from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4be4c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4be4c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4be4c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4be4c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4be4c-133">Írja be a keresőmezőbe, **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-133">In the search box, type **MobileXpense**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_search.png)

5. <span data-ttu-id="4be4c-135">Az eredmények panelen válassza ki a **MobileXpense**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4be4c-135">In the results panel, select **MobileXpense**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4be4c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4be4c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4be4c-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján MobileXpense</span><span class="sxs-lookup"><span data-stu-id="4be4c-138">In this section, you configure and test Azure AD single sign-on with MobileXpense based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4be4c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó MobileXpense a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4be4c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MobileXpense is to a user in Azure AD.</span></span> <span data-ttu-id="4be4c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a MobileXpense közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4be4c-140">In other words, a link relationship between an Azure AD user and the related user in MobileXpense needs to be established.</span></span>

<span data-ttu-id="4be4c-141">MobileXpense, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4be4c-141">In MobileXpense, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4be4c-142">Az Azure AD egyszeri bejelentkezést a MobileXpense tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="4be4c-142">To configure and test Azure AD single sign-on with MobileXpense, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4be4c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="4be4c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4be4c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="4be4c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4be4c-145">**[MobileXpense tesztfelhasználó létrehozása](#creating-a-mobilexpense-test-user)**  - való Britta Simon valami MobileXpense, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4be4c-145">**[Creating a MobileXpense test user](#creating-a-mobilexpense-test-user)** - to have a counterpart of Britta Simon in MobileXpense that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4be4c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4be4c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4be4c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4be4c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4be4c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4be4c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4be4c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az MobileXpense alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4be4c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MobileXpense application.</span></span>

<span data-ttu-id="4be4c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés MobileXpense, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4be4c-150">**To configure Azure AD single sign-on with MobileXpense, perform the following steps:**</span></span>

1. <span data-ttu-id="4be4c-151">Az Azure portálon a a **MobileXpense** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-151">In the Azure portal, on the **MobileXpense** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4be4c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4be4c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_samlbase.png)

3. <span data-ttu-id="4be4c-155">Az a **MobileXpense tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="4be4c-155">On the **MobileXpense Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url11.png)

    <span data-ttu-id="4be4c-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="4be4c-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span></span>

4. <span data-ttu-id="4be4c-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="4be4c-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url22.png)

<span data-ttu-id="4be4c-160">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-cím::`https://<sub domain>.mobilexpense.com/<customername>`</span><span class="sxs-lookup"><span data-stu-id="4be4c-160">In the **Sign-on URL** textbox, type a URL using the following pattern:: `https://<sub domain>.mobilexpense.com/<customername>`</span></span>

> [!NOTE] 
> <span data-ttu-id="4be4c-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="4be4c-161">These values are not real.</span></span> <span data-ttu-id="4be4c-162">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="4be4c-162">Update these values with the actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="4be4c-163">Ügyfél [MobileXpense ügyfél-támogatási csoport](http://www.mobilexpense.net/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4be4c-163">Contact [MobileXpense Client support team](http://www.mobilexpense.net/contact) to get these values.</span></span> 

5. <span data-ttu-id="4be4c-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4be4c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_certificate.png) 

6. <span data-ttu-id="4be4c-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="4be4c-168">Egyszeri bejelentkezés konfigurálása **MobileXpense** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [MobileXpense támogatási csoport](http://www.mobilexpense.net/contact).</span><span class="sxs-lookup"><span data-stu-id="4be4c-168">To configure single sign-on on **MobileXpense** side, you need to send the downloaded **Metadata XML** to [MobileXpense support team](http://www.mobilexpense.net/contact).</span></span>

> [!TIP]
> <span data-ttu-id="4be4c-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="4be4c-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4be4c-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="4be4c-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4be4c-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4be4c-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4be4c-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4be4c-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="4be4c-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="4be4c-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4be4c-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4be4c-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4be4c-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4be4c-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4be4c-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="4be4c-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4be4c-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4be4c-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4be4c-184">a.</span><span class="sxs-lookup"><span data-stu-id="4be4c-184">a.</span></span> <span data-ttu-id="4be4c-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4be4c-186">b.</span><span class="sxs-lookup"><span data-stu-id="4be4c-186">b.</span></span> <span data-ttu-id="4be4c-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4be4c-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4be4c-188">c.</span><span class="sxs-lookup"><span data-stu-id="4be4c-188">c.</span></span> <span data-ttu-id="4be4c-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4be4c-190">d.</span><span class="sxs-lookup"><span data-stu-id="4be4c-190">d.</span></span> <span data-ttu-id="4be4c-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-191">Click **Create**.</span></span>
 
### <a name="creating-a-mobilexpense-test-user"></a><span data-ttu-id="4be4c-192">MobileXpense tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4be4c-192">Creating a MobileXpense test user</span></span>

<span data-ttu-id="4be4c-193">Ebben a szakaszban egy MobileXpense Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4be4c-193">In this section, you create a user called Britta Simon in MobileXpense.</span></span> <span data-ttu-id="4be4c-194">a [MobileXpense támogatási csoport](http://www.mobilexpense.net/contact) a felhasználók hozzáadása a MobileXpense platform.</span><span class="sxs-lookup"><span data-stu-id="4be4c-194">work with [MobileXpense support team](http://www.mobilexpense.net/contact) to add the users in the MobileXpense platform.</span></span> <span data-ttu-id="4be4c-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="4be4c-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4be4c-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4be4c-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4be4c-197">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés MobileXpense Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="4be4c-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MobileXpense.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4be4c-199">**Britta Simon hozzárendelése MobileXpense, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4be4c-199">**To assign Britta Simon to MobileXpense, perform the following steps:**</span></span>

1. <span data-ttu-id="4be4c-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4be4c-202">Az alkalmazások listában válassza ki a **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-202">In the applications list, select **MobileXpense**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_app.png) 

3. <span data-ttu-id="4be4c-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4be4c-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4be4c-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4be4c-206">Click **Add** button.</span></span> <span data-ttu-id="4be4c-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4be4c-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4be4c-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4be4c-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4be4c-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4be4c-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4be4c-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4be4c-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4be4c-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4be4c-212">Testing single sign-on</span></span>

<span data-ttu-id="4be4c-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="4be4c-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4be4c-214">Ha a hozzáférési panelen MobileXpense csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az MobileXpense alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="4be4c-214">When you click the MobileXpense tile in the Access Panel, you should get automatically signed-on to your MobileXpense application.</span></span>
<span data-ttu-id="4be4c-215">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4be4c-215">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4be4c-216">További források</span><span class="sxs-lookup"><span data-stu-id="4be4c-216">Additional resources</span></span>

* [<span data-ttu-id="4be4c-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="4be4c-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4be4c-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4be4c-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_203.png

