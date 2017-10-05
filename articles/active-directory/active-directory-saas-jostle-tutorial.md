---
title: "Oktatóanyag: Azure Active Directoryval integrált Jostle |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Jostle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0ca8aca1446a38643ce9f6751b6fe9cae1eaa5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a><span data-ttu-id="3d581-103">Oktatóanyag: Azure Active Directoryval integrált Jostle</span><span class="sxs-lookup"><span data-stu-id="3d581-103">Tutorial: Azure Active Directory integration with Jostle</span></span>

<span data-ttu-id="3d581-104">Ebben az oktatóanyagban elsajátíthatja Jostle integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3d581-104">In this tutorial, you learn how to integrate Jostle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d581-105">Jostle integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3d581-105">Integrating Jostle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3d581-106">Megadhatja a Jostle hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3d581-106">You can control in Azure AD who has access to Jostle</span></span>
- <span data-ttu-id="3d581-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Jostle (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3d581-107">You can enable your users to automatically get signed-on to Jostle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3d581-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3d581-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3d581-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d581-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d581-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d581-110">Prerequisites</span></span>

<span data-ttu-id="3d581-111">Konfigurálása az Azure AD-integrációs Jostle, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3d581-111">To configure Azure AD integration with Jostle, you need the following items:</span></span>

- <span data-ttu-id="3d581-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3d581-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d581-113">Egy Jostle egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3d581-113">A Jostle single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d581-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3d581-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d581-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3d581-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d581-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3d581-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d581-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d581-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d581-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3d581-118">Scenario description</span></span>
<span data-ttu-id="3d581-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3d581-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d581-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3d581-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d581-121">A gyűjteményből Jostle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d581-121">Adding Jostle from the gallery</span></span>
2. <span data-ttu-id="3d581-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d581-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jostle-from-the-gallery"></a><span data-ttu-id="3d581-123">A gyűjteményből Jostle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d581-123">Adding Jostle from the gallery</span></span>
<span data-ttu-id="3d581-124">Az Azure AD integrálása a Jostle konfigurálásához kell hozzáadnia Jostle a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3d581-124">To configure the integration of Jostle into Azure AD, you need to add Jostle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3d581-125">**A gyűjteményből Jostle hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d581-125">**To add Jostle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3d581-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d581-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3d581-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3d581-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3d581-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d581-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3d581-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3d581-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3d581-133">Írja be a keresőmezőbe, **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="3d581-133">In the search box, type **Jostle**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_search.png)

5. <span data-ttu-id="3d581-135">Az eredmények panelen válassza ki a **Jostle**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3d581-135">In the results panel, select **Jostle**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3d581-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3d581-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3d581-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jostle</span><span class="sxs-lookup"><span data-stu-id="3d581-138">In this section, you configure and test Azure AD single sign-on with Jostle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3d581-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Jostle a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3d581-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jostle is to a user in Azure AD.</span></span> <span data-ttu-id="3d581-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Jostle közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3d581-140">In other words, a link relationship between an Azure AD user and the related user in Jostle needs to be established.</span></span>

<span data-ttu-id="3d581-141">Jostle, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3d581-141">In Jostle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3d581-142">Az Azure AD egyszeri bejelentkezést a Jostle tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3d581-142">To configure and test Azure AD single sign-on with Jostle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3d581-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3d581-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3d581-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3d581-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d581-145">**[Jostle tesztfelhasználó létrehozása](#creating-a-jostle-test-user)**  - való Britta Simon valami Jostle, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3d581-145">**[Creating a Jostle test user](#creating-a-jostle-test-user)** - to have a counterpart of Britta Simon in Jostle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d581-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3d581-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d581-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3d581-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3d581-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d581-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3d581-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Jostle alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3d581-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jostle application.</span></span>

<span data-ttu-id="3d581-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Jostle, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d581-150">**To configure Azure AD single sign-on with Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="3d581-151">Az Azure portálon a a **Jostle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3d581-151">In the Azure portal, on the **Jostle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3d581-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3d581-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_samlbase.png)

3. <span data-ttu-id="3d581-155">Az a **Jostle tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3d581-155">On the **Jostle Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_url.png)

    <span data-ttu-id="3d581-157">a.</span><span class="sxs-lookup"><span data-stu-id="3d581-157">a.</span></span> <span data-ttu-id="3d581-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tanent name>.jostle.us/jostle-prod/`</span><span class="sxs-lookup"><span data-stu-id="3d581-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us/jostle-prod/`</span></span>

    <span data-ttu-id="3d581-159">b.</span><span class="sxs-lookup"><span data-stu-id="3d581-159">b.</span></span> <span data-ttu-id="3d581-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tanent name>.jostle.us`</span><span class="sxs-lookup"><span data-stu-id="3d581-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3d581-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3d581-161">These values are not real.</span></span> <span data-ttu-id="3d581-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3d581-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3d581-163">Ügyfél [Jostle támogatási csoport](mailto:support@jostle.me) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3d581-163">Contact [Jostle support team](mailto:support@jostle.me) to get these values.</span></span> 
 


4. <span data-ttu-id="3d581-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3d581-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_certificate.png) 

5. <span data-ttu-id="3d581-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d581-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3d581-168">Az egyszeri bejelentkezés Jostle oldalon megadásához kell elküldenie a letöltött metaadatok XML való [Jostle támogatási csoport](mailto:support@jostle.me).</span><span class="sxs-lookup"><span data-stu-id="3d581-168">To configure single sign-on on Jostle side, you need to send the downloaded metadata XML to [Jostle support team](mailto:support@jostle.me).</span></span> <span data-ttu-id="3d581-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="3d581-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> 

> [!TIP]
> <span data-ttu-id="3d581-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3d581-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3d581-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3d581-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3d581-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d581-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3d581-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d581-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="3d581-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3d581-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3d581-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d581-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3d581-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3d581-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3d581-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3d581-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3d581-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3d581-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3d581-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3d581-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3d581-185">a.</span><span class="sxs-lookup"><span data-stu-id="3d581-185">a.</span></span> <span data-ttu-id="3d581-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d581-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d581-187">b.</span><span class="sxs-lookup"><span data-stu-id="3d581-187">b.</span></span> <span data-ttu-id="3d581-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3d581-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3d581-189">c.</span><span class="sxs-lookup"><span data-stu-id="3d581-189">c.</span></span> <span data-ttu-id="3d581-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3d581-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3d581-191">d.</span><span class="sxs-lookup"><span data-stu-id="3d581-191">d.</span></span> <span data-ttu-id="3d581-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3d581-192">Click **Create**.</span></span>
 
### <a name="creating-a-jostle-test-user"></a><span data-ttu-id="3d581-193">Jostle tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d581-193">Creating a Jostle test user</span></span>

<span data-ttu-id="3d581-194">Ebben a szakaszban egy Jostle Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3d581-194">In this section, you create a user called Britta Simon in Jostle.</span></span> <span data-ttu-id="3d581-195">Ha nem tudja Britta Simon hozzáadása a Jostle, forduljon a [Jostle támogatási csoport](mailto:support@jostle.me) adja hozzá a tesztfelhasználó számára, és engedélyezze az egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="3d581-195">If you don't know how to add Britta Simon in Jostle, please contact with [Jostle support team](mailto:support@jostle.me) to add the test user and enable SSO.</span></span>

> [!NOTE]
> <span data-ttu-id="3d581-196">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="3d581-196">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3d581-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3d581-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3d581-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Jostle Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3d581-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jostle.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3d581-200">**Britta Simon hozzárendelése Jostle, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3d581-200">**To assign Britta Simon to Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="3d581-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3d581-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3d581-203">Az alkalmazások listában válassza ki a **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="3d581-203">In the applications list, select **Jostle**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_app.png) 

3. <span data-ttu-id="3d581-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3d581-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3d581-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3d581-207">Click **Add** button.</span></span> <span data-ttu-id="3d581-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d581-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3d581-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3d581-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3d581-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d581-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d581-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3d581-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3d581-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3d581-213">Testing single sign-on</span></span>

<span data-ttu-id="3d581-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3d581-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3d581-215">Ha a hozzáférési panelen Jostle csempére kattint, szerezheti be automatikusan Jostle alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="3d581-215">When you click the Jostle tile in the Access Panel, you should get automatically login page of Jostle application.</span></span>
<span data-ttu-id="3d581-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3d581-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d581-217">További források</span><span class="sxs-lookup"><span data-stu-id="3d581-217">Additional resources</span></span>

* [<span data-ttu-id="3d581-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3d581-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d581-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3d581-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png

