---
title: "Oktatóanyag: Azure Active Directoryval integrált Projectplace |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Projectplace között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: bb9dd10c887cb0e42e544066d9b0dcfa554e10ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="4a792-103">Oktatóanyag: Azure Active Directoryval integrált Projectplace</span><span class="sxs-lookup"><span data-stu-id="4a792-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="4a792-104">Ebben az oktatóanyagban elsajátíthatja Projectplace integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4a792-104">In this tutorial, you learn how to integrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a792-105">Projectplace integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="4a792-105">Integrating Projectplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4a792-106">Megadhatja a Projectplace hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4a792-106">You can control in Azure AD who has access to Projectplace</span></span>
- <span data-ttu-id="4a792-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Projectplace (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4a792-107">You can enable your users to automatically get signed-on to Projectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4a792-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4a792-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4a792-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a792-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a792-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4a792-110">Prerequisites</span></span>

<span data-ttu-id="4a792-111">Az Azure AD-integráció konfigurálása a Projectplace, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4a792-111">To configure Azure AD integration with Projectplace, you need the following items:</span></span>

- <span data-ttu-id="4a792-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4a792-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a792-113">A Projectplace egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4a792-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a792-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a792-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a792-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="4a792-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a792-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a792-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a792-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a792-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a792-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4a792-118">Scenario description</span></span>
<span data-ttu-id="4a792-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4a792-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a792-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4a792-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a792-121">A gyűjteményből Projectplace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4a792-121">Adding Projectplace from the gallery</span></span>
2. <span data-ttu-id="4a792-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a792-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-the-gallery"></a><span data-ttu-id="4a792-123">A gyűjteményből Projectplace hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4a792-123">Adding Projectplace from the gallery</span></span>
<span data-ttu-id="4a792-124">Az Azure AD integrálása a Projectplace konfigurálásához kell hozzáadnia Projectplace a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="4a792-124">To configure the integration of Projectplace into Azure AD, you need to add Projectplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4a792-125">**Adja hozzá a Projectplace a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a792-125">**To add Projectplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4a792-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a792-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4a792-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4a792-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4a792-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a792-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4a792-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="4a792-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4a792-133">Írja be a keresőmezőbe, **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="4a792-133">In the search box, type **Projectplace**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="4a792-135">Az eredmények panelen válassza ki a **Projectplace**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4a792-135">In the results panel, select **Projectplace**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4a792-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4a792-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4a792-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Projectplace.</span><span class="sxs-lookup"><span data-stu-id="4a792-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4a792-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Projectplace tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4a792-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Projectplace is to a user in Azure AD.</span></span> <span data-ttu-id="4a792-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Projectplace közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4a792-140">In other words, a link relationship between an Azure AD user and the related user in Projectplace needs to be established.</span></span>

<span data-ttu-id="4a792-141">A Projectplace, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4a792-141">In Projectplace, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4a792-142">Az Azure AD egyszeri bejelentkezést a Projectplace tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="4a792-142">To configure and test Azure AD single sign-on with Projectplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4a792-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="4a792-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4a792-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="4a792-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a792-145">**[A Projectplace tesztfelhasználó létrehozása](#creating-a-projectplace-test-user)**  - való Britta Simon egy megfelelője a Projectplace, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4a792-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - to have a counterpart of Britta Simon in Projectplace that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4a792-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4a792-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a792-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4a792-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4a792-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a792-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4a792-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Projectplace-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4a792-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="4a792-150">**Az Azure AD egyszeri bejelentkezést a Projectplace megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a792-150">**To configure Azure AD single sign-on with Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="4a792-151">Az Azure portálon a a **Projectplace** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4a792-151">In the Azure portal, on the **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4a792-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4a792-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="4a792-155">Az a **Projectplace-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a792-155">On the **Projectplace Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="4a792-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="4a792-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4a792-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="4a792-158">This value is not real.</span></span> <span data-ttu-id="4a792-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4a792-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="4a792-160">Ügyfél [Projectplace ügyfél-támogatási csoport](https://success.planview.com/Projectplace/Support) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="4a792-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) to get this value.</span></span> 
 
4. <span data-ttu-id="4a792-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4a792-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="4a792-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a792-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="4a792-165">Egyszeri bejelentkezés konfigurálása **Projectplace** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Projectplace támogatási csoport](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="4a792-165">To configure single sign-on on **Projectplace** side, you need to send the downloaded **Metadata XML** to [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="4a792-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="4a792-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="4a792-167">Egyszeri bejelentkezés konfiguráció által végrehajtandó tartalmaz a [Projectplace támogatási csoport](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="4a792-167">The single sign-on configuration has to be performed by the [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="4a792-168">Értesítést kap, amint a konfigurálása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="4a792-168">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="4a792-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="4a792-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4a792-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="4a792-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4a792-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a792-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4a792-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a792-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="4a792-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="4a792-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4a792-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a792-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4a792-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4a792-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a792-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4a792-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a792-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="4a792-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a792-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4a792-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4a792-184">a.</span><span class="sxs-lookup"><span data-stu-id="4a792-184">a.</span></span> <span data-ttu-id="4a792-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a792-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a792-186">b.</span><span class="sxs-lookup"><span data-stu-id="4a792-186">b.</span></span> <span data-ttu-id="4a792-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4a792-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4a792-188">c.</span><span class="sxs-lookup"><span data-stu-id="4a792-188">c.</span></span> <span data-ttu-id="4a792-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4a792-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4a792-190">d.</span><span class="sxs-lookup"><span data-stu-id="4a792-190">d.</span></span> <span data-ttu-id="4a792-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4a792-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="4a792-192">A Projectplace tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a792-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="4a792-193">Ahhoz, hogy az Azure AD-felhasználók Projectplace bejelentkezni, akkor ki kell építenie a Projectplace.</span><span class="sxs-lookup"><span data-stu-id="4a792-193">In order to enable Azure AD users to log into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="4a792-194">Projectplace, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="4a792-194">In the case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="4a792-195">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a792-195">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="4a792-196">Jelentkezzen be a **Projectplace** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4a792-196">Log in to your **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="4a792-197">Ugrás a **személyek**, és kattintson a **tagok**.</span><span class="sxs-lookup"><span data-stu-id="4a792-197">Go to **People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="4a792-198">![Személyek](./media/active-directory-saas-projectplace-tutorial/ic790228.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="4a792-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="4a792-199">Kattintson a **felvenni abban az esetben**.</span><span class="sxs-lookup"><span data-stu-id="4a792-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="4a792-200">![Tagok hozzáadása](./media/active-directory-saas-projectplace-tutorial/ic790232.png "tagok hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="4a792-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="4a792-201">Az a **tag hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4a792-201">In the **Add Member** section, perform the following steps:</span></span>
   
    <span data-ttu-id="4a792-202">![Új tagok](./media/active-directory-saas-projectplace-tutorial/ic790233.png "új tagok számára")</span><span class="sxs-lookup"><span data-stu-id="4a792-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="4a792-203">a.</span><span class="sxs-lookup"><span data-stu-id="4a792-203">a.</span></span> <span data-ttu-id="4a792-204">Az a **új tagok** szövegmező, írja be egy érvényes AAD-fiókba, azokat a kapcsolódó szövegmezők rendelkezés kívánt e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="4a792-204">In the **New Members** textbox, type the email address of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="4a792-205">b.</span><span class="sxs-lookup"><span data-stu-id="4a792-205">b.</span></span> <span data-ttu-id="4a792-206">Kattintson a **küldése**.</span><span class="sxs-lookup"><span data-stu-id="4a792-206">Click **Send**.</span></span>

   <span data-ttu-id="4a792-207">Az Azure Active Directory fióktulajdonos küld egy e-mailt hivatkozással erősítse meg a fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="4a792-207">An email including a link to confirm the account before it becomes active is sent to the Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="4a792-208">Bármely más Projectplace felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz Projectplace által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="4a792-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4a792-209">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4a792-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4a792-210">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Projectplace Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="4a792-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Projectplace.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4a792-212">**Britta Simon hozzárendelése Projectplace, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4a792-212">**To assign Britta Simon to Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="4a792-213">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4a792-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4a792-215">Az alkalmazások listában válassza ki a **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="4a792-215">In the applications list, select **Projectplace**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="4a792-217">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4a792-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4a792-219">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4a792-219">Click **Add** button.</span></span> <span data-ttu-id="4a792-220">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a792-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4a792-222">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4a792-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4a792-223">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a792-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a792-224">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4a792-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4a792-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4a792-225">Testing single sign-on</span></span>

<span data-ttu-id="4a792-226">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="4a792-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4a792-227">Ha a hozzáférési panelen a Projectplace csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Projectplace-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="4a792-227">When you click the Projectplace tile in the Access Panel, you should get automatically signed-on to your Projectplace application.</span></span>
<span data-ttu-id="4a792-228">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4a792-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a792-229">További források</span><span class="sxs-lookup"><span data-stu-id="4a792-229">Additional resources</span></span>

* [<span data-ttu-id="4a792-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="4a792-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a792-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4a792-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

