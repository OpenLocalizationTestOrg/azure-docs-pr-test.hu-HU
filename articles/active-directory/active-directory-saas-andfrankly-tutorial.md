---
title: "Oktatóanyag: Azure Active Directoryval integrált & frankly |} Microsoft Docs"
description: "Egyszeri bejelentkezés Azure Active Directory közötti konfigurálásával és & frankly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d702060-1b89-4e9d-9f01-ede4f1171c73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ea18a9f9bff258337a3de6d7703b4c548efa37df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-frankly"></a><span data-ttu-id="d8e14-103">Oktatóanyag: Azure Active Directoryval integrált & frankly</span><span class="sxs-lookup"><span data-stu-id="d8e14-103">Tutorial: Azure Active Directory integration with &frankly</span></span>

<span data-ttu-id="d8e14-104">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálhatja & frankly és az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d8e14-104">In this tutorial, you learn how to integrate &frankly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8e14-105">Integrálása & frankly az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d8e14-105">Integrating &frankly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d8e14-106">Szabályozhatja, aki hozzáféréssel rendelkezik a & frankly Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d8e14-106">You can control in Azure AD who has access to &frankly</span></span>
- <span data-ttu-id="d8e14-107">Engedélyezheti a felhasználók automatikusan lekérni bejelentkezett & frankly (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d8e14-107">You can enable your users to automatically get signed-on to &frankly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8e14-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d8e14-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d8e14-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8e14-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8e14-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d8e14-110">Prerequisites</span></span>

<span data-ttu-id="d8e14-111">Az Azure AD integrálása & frankly, akkor kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="d8e14-111">To configure Azure AD integration with &frankly, you need the following items:</span></span>

- <span data-ttu-id="d8e14-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d8e14-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8e14-113">A & frankly egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d8e14-113">A &frankly single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8e14-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d8e14-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8e14-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d8e14-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8e14-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d8e14-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8e14-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8e14-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8e14-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d8e14-118">Scenario description</span></span>
<span data-ttu-id="d8e14-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d8e14-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8e14-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d8e14-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8e14-121">Hozzáadás & frankly a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d8e14-121">Adding &frankly from the gallery</span></span>
2. <span data-ttu-id="d8e14-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d8e14-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-frankly-from-the-gallery"></a><span data-ttu-id="d8e14-123">Hozzáadás & frankly a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d8e14-123">Adding &frankly from the gallery</span></span>
<span data-ttu-id="d8e14-124">Konfigurálja az integrációt a & frankly az Azure AD-be, meg kell adni & frankly a felügyelt SaaS-alkalmazások listájára a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="d8e14-124">To configure the integration of &frankly into Azure AD, you need to add &frankly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d8e14-125">**Vegye fel & frankly a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d8e14-125">**To add &frankly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d8e14-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8e14-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d8e14-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d8e14-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d8e14-133">Írja be a keresőmezőbe, **& frankly**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-133">In the search box, type **&frankly**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_search.png)

5. <span data-ttu-id="d8e14-135">Az eredmények panelen válassza ki a **& frankly**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d8e14-135">In the results panel, select **&frankly**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8e14-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d8e14-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8e14-138">Ebben a szakaszban, konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a & "Britta Simon." nevű tesztfelhasználó frankly alapján</span><span class="sxs-lookup"><span data-stu-id="d8e14-138">In this section, you configure and test Azure AD single sign-on with &frankly based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d8e14-139">Az egyszeri bejelentkezés működéséhez az Azure AD milyen a partner felhasználónak tudnia kell, és frankly egy felhasználó számára az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8e14-139">For single sign-on to work, Azure AD needs to know what the counterpart user in &frankly is to a user in Azure AD.</span></span> <span data-ttu-id="d8e14-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó közötti kapcsolat kapcsolatot frankly & k kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d8e14-140">In other words, a link relationship between an Azure AD user and the related user in &frankly needs to be established.</span></span>

<span data-ttu-id="d8e14-141">A & frankly, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d8e14-141">In &frankly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d8e14-142">Az Azure AD tesztelése és konfigurálása egyszeri bejelentkezéshez az & frankly, akkor kell végezze el a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="d8e14-142">To configure and test Azure AD single sign-on with &frankly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d8e14-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d8e14-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d8e14-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d8e14-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8e14-145">**[Létrehozása egy & frankly tesztfelhasználó](#creating-a-frankly-test-user)**  – egy partner Britta Simon, hogy a & frankly vagyis csatolt a felhasználó az Azure AD-megjelenítésre.</span><span class="sxs-lookup"><span data-stu-id="d8e14-145">**[Creating a &frankly test user](#creating-a-frankly-test-user)** - to have a counterpart of Britta Simon in &frankly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8e14-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d8e14-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8e14-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d8e14-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8e14-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d8e14-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8e14-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az a & frankly alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d8e14-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your &frankly application.</span></span>

<span data-ttu-id="d8e14-150">**Konfigurálhatja az Azure AD egyetlen jelentkezhessen be & frankly, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d8e14-150">**To configure Azure AD single sign-on with &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="d8e14-151">Az Azure portálon a a **& frankly** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-151">In the Azure portal, on the **&frankly** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d8e14-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d8e14-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_samlbase.png)

3. <span data-ttu-id="d8e14-155">Az a **& frankly tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="d8e14-155">On the **&frankly Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url.png)

    <span data-ttu-id="d8e14-157">a.</span><span class="sxs-lookup"><span data-stu-id="d8e14-157">a.</span></span> <span data-ttu-id="d8e14-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d8e14-158">In the **Identifier** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span></span>

    <span data-ttu-id="d8e14-159">b.</span><span class="sxs-lookup"><span data-stu-id="d8e14-159">b.</span></span> <span data-ttu-id="d8e14-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d8e14-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span></span>

4. <span data-ttu-id="d8e14-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="d8e14-162">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="d8e14-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url1.png)

    <span data-ttu-id="d8e14-164">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d8e14-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span></span>
    > [!NOTE] 
    > <span data-ttu-id="d8e14-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d8e14-165">These values are not real.</span></span> <span data-ttu-id="d8e14-166">Frissítheti ezeket az értékeket a tényleges azonosítójú bejelentkezés és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="d8e14-166">Update these values with the actual Identifier, Sign-on, and Reply URL.</span></span> <span data-ttu-id="d8e14-167">Ügyfél [andfrankly támogatási csoport](mailto:help@andfrankly.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d8e14-167">Contact [andfrankly support team](mailto:help@andfrankly.com) to get these values.</span></span>

5. <span data-ttu-id="d8e14-168">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d8e14-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_certificate.png) 

6. <span data-ttu-id="d8e14-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d8e14-172">Egyszeri bejelentkezés konfigurálása **& frankly** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [andfrankly támogatási csoport](mailto:help@andfrankly.com).</span><span class="sxs-lookup"><span data-stu-id="d8e14-172">To configure single sign-on on **&frankly** side, you need to send the downloaded **Metadata XML** to [andfrankly support team](mailto:help@andfrankly.com).</span></span> 

> [!TIP]
> <span data-ttu-id="d8e14-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d8e14-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d8e14-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d8e14-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d8e14-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8e14-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8e14-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8e14-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8e14-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d8e14-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d8e14-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d8e14-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d8e14-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8e14-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8e14-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d8e14-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8e14-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d8e14-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8e14-188">a.</span><span class="sxs-lookup"><span data-stu-id="d8e14-188">a.</span></span> <span data-ttu-id="d8e14-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8e14-190">b.</span><span class="sxs-lookup"><span data-stu-id="d8e14-190">b.</span></span> <span data-ttu-id="d8e14-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8e14-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8e14-192">c.</span><span class="sxs-lookup"><span data-stu-id="d8e14-192">c.</span></span> <span data-ttu-id="d8e14-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d8e14-194">d.</span><span class="sxs-lookup"><span data-stu-id="d8e14-194">d.</span></span> <span data-ttu-id="d8e14-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-195">Click **Create**.</span></span>
 
### <a name="creating-a-frankly-test-user"></a><span data-ttu-id="d8e14-196">Létrehozása egy & frankly tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="d8e14-196">Creating a &frankly test user</span></span>

<span data-ttu-id="d8e14-197">Ebben a szakaszban Britta Simon nevű frankly & a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d8e14-197">In this section, you create a user called Britta Simon in &frankly.</span></span> <span data-ttu-id="d8e14-198">Együttműködve [andfrankly támogatási csoport](mailto:help@andfrankly.com) tartozó felhasználók hozzáadása a & frankly platform.</span><span class="sxs-lookup"><span data-stu-id="d8e14-198">Work with  [andfrankly support team](mailto:help@andfrankly.com) to add the users in the &frankly platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d8e14-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d8e14-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d8e14-200">Ebben a szakaszban Britta Simon hozzáférés biztosítása a & frankly által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d8e14-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to &frankly.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d8e14-202">**A & frankly Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="d8e14-202">**To assign Britta Simon to &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="d8e14-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d8e14-205">Az alkalmazások listában válassza ki a **& frankly**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-205">In the applications list, select **&frankly**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_app.png) 

3. <span data-ttu-id="d8e14-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d8e14-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d8e14-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d8e14-209">Click **Add** button.</span></span> <span data-ttu-id="d8e14-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8e14-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d8e14-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d8e14-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d8e14-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8e14-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8e14-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8e14-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8e14-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d8e14-215">Testing single sign-on</span></span>

<span data-ttu-id="d8e14-216">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d8e14-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="d8e14-217">Kattintva & frankly csempéjén a hozzáférési panelen kattintson a jobb kapja meg automatikusan bejelentkezett az a & frankly alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d8e14-217">When you click the &frankly tile in the Access Panel, you should get automatically signed-on to your &frankly application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8e14-218">További források</span><span class="sxs-lookup"><span data-stu-id="d8e14-218">Additional resources</span></span>

* [<span data-ttu-id="d8e14-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d8e14-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8e14-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d8e14-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png

