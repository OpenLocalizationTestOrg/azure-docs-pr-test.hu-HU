---
title: "Oktatóanyag: Azure Active Directoryval integrált 360 Online |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és 360 Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 629c7db04b0f9c880da6dfa8eac7fe14ecd8a215
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a><span data-ttu-id="d33c9-103">Oktatóanyag: Azure Active Directoryval integrált 360 Online</span><span class="sxs-lookup"><span data-stu-id="d33c9-103">Tutorial: Azure Active Directory integration with 360 Online</span></span>

<span data-ttu-id="d33c9-104">Ebben az oktatóanyagban elsajátíthatja 360 Online integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d33c9-104">In this tutorial, you learn how to integrate 360 Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d33c9-105">360 Online integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d33c9-105">Integrating 360 Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d33c9-106">Szabályozhatja, aki hozzáfér a 360 Online Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d33c9-106">You can control in Azure AD who has access to 360 Online</span></span>
- <span data-ttu-id="d33c9-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az Azure AD-fiókok (egyszeri bejelentkezés) 360 online</span><span class="sxs-lookup"><span data-stu-id="d33c9-107">You can enable your users to automatically get signed-on to 360 Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d33c9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d33c9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d33c9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d33c9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d33c9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d33c9-110">Prerequisites</span></span>

<span data-ttu-id="d33c9-111">360 Online az Azure AD-integrációs konfigurálásához a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d33c9-111">To configure Azure AD integration with 360 Online, you need the following items:</span></span>

- <span data-ttu-id="d33c9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d33c9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d33c9-113">A 360 Online egyszeri bejelentkezést előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d33c9-113">A 360 Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d33c9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d33c9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d33c9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d33c9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d33c9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d33c9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d33c9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d33c9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d33c9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d33c9-118">Scenario description</span></span>
<span data-ttu-id="d33c9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d33c9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d33c9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d33c9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d33c9-121">360 Online hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d33c9-121">Adding 360 Online from the gallery</span></span>
2. <span data-ttu-id="d33c9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d33c9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-360-online-from-the-gallery"></a><span data-ttu-id="d33c9-123">360 Online hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d33c9-123">Adding 360 Online from the gallery</span></span>
<span data-ttu-id="d33c9-124">Az Azure AD integrálása a 360 Online konfigurálásához kell hozzáadnia 360 Online a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d33c9-124">To configure the integration of 360 Online into Azure AD, you need to add 360 Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d33c9-125">**360 Online hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d33c9-125">**To add 360 Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d33c9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d33c9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d33c9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d33c9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d33c9-133">Írja be a keresőmezőbe, **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-133">In the search box, type **360 Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/tutorial_360online_search.png)

5. <span data-ttu-id="d33c9-135">Az eredmények panelen válassza ki a **360 Online**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d33c9-135">In the results panel, select **360 Online**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d33c9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d33c9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d33c9-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az úgynevezett "Britta Simon." tesztfelhasználó alapján 360 Online</span><span class="sxs-lookup"><span data-stu-id="d33c9-138">In this section, you configure and test Azure AD single sign-on with 360 Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d33c9-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó 360 Online Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d33c9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 360 Online is to a user in Azure AD.</span></span> <span data-ttu-id="d33c9-140">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó a 360 hivatkozás kapcsolatának Online kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d33c9-140">In other words, a link relationship between an Azure AD user and the related user in 360 Online needs to be established.</span></span>

<span data-ttu-id="d33c9-141">360 Online, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d33c9-141">In 360 Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d33c9-142">Az Azure AD egyszeri bejelentkezést a 360 Online tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d33c9-142">To configure and test Azure AD single sign-on with 360 Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d33c9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d33c9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d33c9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d33c9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d33c9-145">**[360 Online tesztfelhasználó létrehozása](#creating-a-360-online-test-user)**  - való egy megfelelője a Britta Simon 360 Online, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d33c9-145">**[Creating a 360 Online test user](#creating-a-360-online-test-user)** - to have a counterpart of Britta Simon in 360 Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d33c9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d33c9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d33c9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d33c9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d33c9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d33c9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d33c9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az 360 Online alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d33c9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 360 Online application.</span></span>

<span data-ttu-id="d33c9-150">**360 Online az Azure AD az egyszeri bejelentkezés konfigurálása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d33c9-150">**To configure Azure AD single sign-on with 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="d33c9-151">Az Azure portálon a a **360 Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-151">In the Azure portal, on the **360 Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d33c9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d33c9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_samlbase.png)

3. <span data-ttu-id="d33c9-155">Az a **360 Online tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d33c9-155">On the **360 Online Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_url.png)

    <span data-ttu-id="d33c9-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.public360online.com`</span><span class="sxs-lookup"><span data-stu-id="d33c9-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.public360online.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d33c9-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="d33c9-158">The value is not real.</span></span> <span data-ttu-id="d33c9-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d33c9-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="d33c9-160">Ügyfél [360 Online ügyfél-támogatási csoport](mailto:360online@software-innovation.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="d33c9-160">Contact [360 Online Client support team](mailto:360online@software-innovation.com) to get the value.</span></span> 
 
4. <span data-ttu-id="d33c9-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d33c9-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_certificate.png) 

5. <span data-ttu-id="d33c9-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d33c9-165">Egyszeri bejelentkezés konfigurálása **360 Online** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [360 Online támogatási csoport](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="d33c9-165">To configure single sign-on on **360 Online** side, you need to send the downloaded **Metadata XML** to [360 Online support team](mailto:360online@software-innovation.com).</span></span> 

> [!TIP]
> <span data-ttu-id="d33c9-166">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d33c9-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d33c9-167">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d33c9-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d33c9-168">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d33c9-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d33c9-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d33c9-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="d33c9-170">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d33c9-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d33c9-172">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d33c9-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d33c9-173">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d33c9-175">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d33c9-177">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d33c9-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d33c9-179">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d33c9-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d33c9-181">a.</span><span class="sxs-lookup"><span data-stu-id="d33c9-181">a.</span></span> <span data-ttu-id="d33c9-182">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d33c9-183">b.</span><span class="sxs-lookup"><span data-stu-id="d33c9-183">b.</span></span> <span data-ttu-id="d33c9-184">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d33c9-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d33c9-185">c.</span><span class="sxs-lookup"><span data-stu-id="d33c9-185">c.</span></span> <span data-ttu-id="d33c9-186">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d33c9-187">d.</span><span class="sxs-lookup"><span data-stu-id="d33c9-187">d.</span></span> <span data-ttu-id="d33c9-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-188">Click **Create**.</span></span>
 
### <a name="creating-a-360-online-test-user"></a><span data-ttu-id="d33c9-189">360 Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d33c9-189">Creating a 360 Online test user</span></span>

<span data-ttu-id="d33c9-190">Ebben a szakaszban egy 360 Online Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d33c9-190">In this section, you create a user called Britta Simon in 360 Online.</span></span> <span data-ttu-id="d33c9-191">kapcsolatba kell lépnie [360 Online támogatási csoport](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="d33c9-191">you need to contact [360 Online support team](mailto:360online@software-innovation.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d33c9-192">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d33c9-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d33c9-193">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés 360 Online Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d33c9-193">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 360 Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d33c9-195">**A 360 Online Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="d33c9-195">**To assign Britta Simon to 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="d33c9-196">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-196">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d33c9-198">Az alkalmazások listában válassza ki a **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-198">In the applications list, select **360 Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-360online-tutorial/tutorial_360online_app.png) 

3. <span data-ttu-id="d33c9-200">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d33c9-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d33c9-202">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d33c9-202">Click **Add** button.</span></span> <span data-ttu-id="d33c9-203">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d33c9-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d33c9-205">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d33c9-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d33c9-206">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d33c9-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d33c9-207">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d33c9-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d33c9-208">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d33c9-208">Testing single sign-on</span></span>

<span data-ttu-id="d33c9-209">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="d33c9-209">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d33c9-210">A 360 Online csempe elemre a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett az 360 Online alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d33c9-210">When you click the 360 Online tile in the Access Panel, you should get automatically signed-on to your 360 Online application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d33c9-211">További források</span><span class="sxs-lookup"><span data-stu-id="d33c9-211">Additional resources</span></span>

* [<span data-ttu-id="d33c9-212">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d33c9-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d33c9-213">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d33c9-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-360online-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png

