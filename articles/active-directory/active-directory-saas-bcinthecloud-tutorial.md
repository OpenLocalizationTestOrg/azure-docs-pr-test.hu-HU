---
title: "Oktatóanyag: A felhőben BC Azure Active Directoryval integrált |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BC között a felhőben."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: ebc95d600eca1027331cd92cfe481d0c3ee833a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-the-cloud"></a><span data-ttu-id="e90ad-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező BC a felhőben</span><span class="sxs-lookup"><span data-stu-id="e90ad-103">Tutorial: Azure Active Directory integration with BC in the Cloud</span></span>

<span data-ttu-id="e90ad-104">Ebben az oktatóanyagban elsajátíthatja BC integrálása az Azure Active Directory (Azure AD) a felhőben.</span><span class="sxs-lookup"><span data-stu-id="e90ad-104">In this tutorial, you learn how to integrate BC in the Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e90ad-105">BC a felhőben az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e90ad-105">Integrating BC in the Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e90ad-106">Szabályozhatja, aki hozzáfér a felhőben BC Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e90ad-106">You can control in Azure AD who has access to BC in the Cloud</span></span>
- <span data-ttu-id="e90ad-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a BC a felhőben (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e90ad-107">You can enable your users to automatically get signed-on to BC in the Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e90ad-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e90ad-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e90ad-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e90ad-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e90ad-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e90ad-110">Prerequisites</span></span>

<span data-ttu-id="e90ad-111">Az Azure AD rendszerrel történő integráció konfigurálása BC a felhőben, szüksége van a következőkre:</span><span class="sxs-lookup"><span data-stu-id="e90ad-111">To configure Azure AD integration with BC in the Cloud, you need the following items:</span></span>

- <span data-ttu-id="e90ad-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e90ad-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e90ad-113">A felhő egyszeri bejelentkezés engedélyezve van az előfizetésben a egy BC</span><span class="sxs-lookup"><span data-stu-id="e90ad-113">A BC in the Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e90ad-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e90ad-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e90ad-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e90ad-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e90ad-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e90ad-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e90ad-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e90ad-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e90ad-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e90ad-118">Scenario description</span></span>
<span data-ttu-id="e90ad-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e90ad-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e90ad-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e90ad-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e90ad-121">A felhőben a gyűjteményből BC hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e90ad-121">Adding BC in the Cloud from the gallery</span></span>
2. <span data-ttu-id="e90ad-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e90ad-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bc-in-the-cloud-from-the-gallery"></a><span data-ttu-id="e90ad-123">A felhőben a gyűjteményből BC hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e90ad-123">Adding BC in the Cloud from the gallery</span></span>
<span data-ttu-id="e90ad-124">BC integrálása az Azure AD-be a felhőben konfigurálásához kell hozzáadnia BC a felhőben a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e90ad-124">To configure the integration of BC in the Cloud into Azure AD, you need to add BC in the Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e90ad-125">**A felhőben a gyűjteményből BC hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e90ad-125">**To add BC in the Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e90ad-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e90ad-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e90ad-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e90ad-131">Új alkalmazás hozzáadásához kattintson **Hozzáadás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-131">To add new application, click **Add** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e90ad-133">Írja be a keresőmezőbe, **a felhőben BC**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-133">In the search box, type **BC in the Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

5. <span data-ttu-id="e90ad-135">Az eredmények panelen válassza ki a **a felhőben BC**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e90ad-135">In the results panel, select **BC in the Cloud**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e90ad-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e90ad-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e90ad-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a BC "Britta Simon." nevű tesztfelhasználó alapján a felhőben</span><span class="sxs-lookup"><span data-stu-id="e90ad-138">In this section, you configure and test Azure AD single sign-on with BC in the Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e90ad-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BC a felhőben a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e90ad-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BC in the Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="e90ad-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a BC a felhőben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e90ad-140">In other words, a link relationship between an Azure AD user and the related user in BC in the Cloud needs to be established.</span></span>

<span data-ttu-id="e90ad-141">A felhőben BC, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e90ad-141">In BC in the Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e90ad-142">A felhőben az Azure AD egyszeri bejelentkezést a BC tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e90ad-142">To configure and test Azure AD single sign-on with BC in the Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e90ad-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e90ad-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e90ad-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e90ad-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e90ad-145">**[A felhő tesztfelhasználó létrehozása egy BC](#creating-a-bc-in-the-cloud-test-user)**  - való egy megfelelője a Britta Simon BC a felhőben, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e90ad-145">**[Creating a BC in the Cloud test user](#creating-a-bc-in-the-cloud-test-user)** - to have a counterpart of Britta Simon in BC in the Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e90ad-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e90ad-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e90ad-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e90ad-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e90ad-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e90ad-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e90ad-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a felhő alkalmazásban a BC egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e90ad-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BC in the Cloud application.</span></span>

<span data-ttu-id="e90ad-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BC a felhőben, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e90ad-150">**To configure Azure AD single sign-on with BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e90ad-151">Az Azure portálon a a **a felhőben BC** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-151">In the Azure portal, on the **BC in the Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e90ad-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e90ad-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

3. <span data-ttu-id="e90ad-155">Az a **felhő tartományban és URL-címek BC** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e90ad-155">On the **BC in the Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    <span data-ttu-id="e90ad-157">a.</span><span class="sxs-lookup"><span data-stu-id="e90ad-157">a.</span></span> <span data-ttu-id="e90ad-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span><span class="sxs-lookup"><span data-stu-id="e90ad-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span></span>

    <span data-ttu-id="e90ad-159">b.</span><span class="sxs-lookup"><span data-stu-id="e90ad-159">b.</span></span> <span data-ttu-id="e90ad-160">Az a **azonosító** szövegmező, adja meg az URL-címet:`https://app.bcinthecloud.com`</span><span class="sxs-lookup"><span data-stu-id="e90ad-160">In the **Identifier** textbox, type a URL as: `https://app.bcinthecloud.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e90ad-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e90ad-161">This value is not real.</span></span> <span data-ttu-id="e90ad-162">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e90ad-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="e90ad-163">Ügyfél [BC a felhőalapú ügyfél-támogatási csoport](https://www.bcinthecloud.com/supportcenter/) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="e90ad-163">Contact [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to get this value.</span></span> 
 
4. <span data-ttu-id="e90ad-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e90ad-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

5. <span data-ttu-id="e90ad-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e90ad-168">Egyszeri bejelentkezés konfigurálása **a felhőben BC** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [a felhőben BC támogatási csoport](https://www.bcinthecloud.com/supportcenter/).</span><span class="sxs-lookup"><span data-stu-id="e90ad-168">To configure single sign-on on **BC in the Cloud** side, you need to send the downloaded **Metadata XML** to [BC in the Cloud support team](https://www.bcinthecloud.com/supportcenter/).</span></span>

> [!TIP]
> <span data-ttu-id="e90ad-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e90ad-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e90ad-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e90ad-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e90ad-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e90ad-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e90ad-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e90ad-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="e90ad-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e90ad-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e90ad-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e90ad-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e90ad-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e90ad-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e90ad-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e90ad-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e90ad-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e90ad-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e90ad-184">a.</span><span class="sxs-lookup"><span data-stu-id="e90ad-184">a.</span></span> <span data-ttu-id="e90ad-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e90ad-186">b.</span><span class="sxs-lookup"><span data-stu-id="e90ad-186">b.</span></span> <span data-ttu-id="e90ad-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e90ad-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e90ad-188">c.</span><span class="sxs-lookup"><span data-stu-id="e90ad-188">c.</span></span> <span data-ttu-id="e90ad-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e90ad-190">d.</span><span class="sxs-lookup"><span data-stu-id="e90ad-190">d.</span></span> <span data-ttu-id="e90ad-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-191">Click **Create**.</span></span>
 
### <a name="creating-a-bc-in-the-cloud-test-user"></a><span data-ttu-id="e90ad-192">Egy BC felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e90ad-192">Creating a BC in the Cloud test user</span></span>

<span data-ttu-id="e90ad-193">Ebben a szakaszban hoz létre a felhasználó BC Britta Simon meghívta a felhőben.</span><span class="sxs-lookup"><span data-stu-id="e90ad-193">In this section, you create a user called Britta Simon in BC in the Cloud.</span></span> <span data-ttu-id="e90ad-194">Együttműködve [BC a felhőalapú ügyfél-támogatási csoport](https://www.bcinthecloud.com/supportcenter/) felhasználót is hozzáadhat a felhő alkalmazása BC.</span><span class="sxs-lookup"><span data-stu-id="e90ad-194">Work with [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to add the users in the BC in the Cloud application.</span></span> <span data-ttu-id="e90ad-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="e90ad-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e90ad-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e90ad-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e90ad-197">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés a felhőben BC Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="e90ad-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BC in the Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e90ad-199">**Britta Simon hozzárendelése BC a felhőben, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e90ad-199">**To assign Britta Simon to BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e90ad-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e90ad-202">Az alkalmazások listában válassza ki a **a felhőben BC**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-202">In the applications list, select **BC in the Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

3. <span data-ttu-id="e90ad-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e90ad-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e90ad-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e90ad-206">Click **Add** button.</span></span> <span data-ttu-id="e90ad-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e90ad-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e90ad-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e90ad-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e90ad-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e90ad-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e90ad-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e90ad-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e90ad-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e90ad-212">Testing single sign-on</span></span>

<span data-ttu-id="e90ad-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e90ad-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

 <span data-ttu-id="e90ad-214">Ha a hozzáférési panelen a felhő csempén BC gombra kattint, akkor kell beolvasása automatikusan bejelentkezett a BC a felhőalapú alkalmazásokban való.</span><span class="sxs-lookup"><span data-stu-id="e90ad-214">When you click the BC in the Cloud tile in the Access Panel, you should get automatically signed-on to your BC in the Cloud application.</span></span> <span data-ttu-id="e90ad-215">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e90ad-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e90ad-216">További források</span><span class="sxs-lookup"><span data-stu-id="e90ad-216">Additional resources</span></span>

* [<span data-ttu-id="e90ad-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e90ad-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e90ad-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e90ad-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_203.png

