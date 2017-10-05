---
title: "Oktatóanyag: Azure Active Directoryval integrált hitelesítés |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Certify között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0b36e020-175a-4534-b341-85260739f889
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bbb2357d17535de438555a0b1f8256b134c8a40e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-certify"></a><span data-ttu-id="4707f-103">Oktatóanyag: Azure Active Directoryval integrált hitelesítés</span><span class="sxs-lookup"><span data-stu-id="4707f-103">Tutorial: Azure Active Directory integration with Certify</span></span>

<span data-ttu-id="4707f-104">Ebben az oktatóanyagban elsajátíthatja Certify integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4707f-104">In this tutorial, you learn how to integrate Certify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4707f-105">Certify integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="4707f-105">Integrating Certify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4707f-106">Megadhatja a Certify hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4707f-106">You can control in Azure AD who has access to Certify</span></span>
- <span data-ttu-id="4707f-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a hitelesítés (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4707f-107">You can enable your users to automatically get signed-on to Certify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4707f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4707f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4707f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4707f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4707f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4707f-110">Prerequisites</span></span>

<span data-ttu-id="4707f-111">Az Azure AD-integráció konfigurálása a Certify, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4707f-111">To configure Azure AD integration with Certify, you need the following items:</span></span>

- <span data-ttu-id="4707f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4707f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4707f-113">A hitelesítés egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="4707f-113">A Certify single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4707f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="4707f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4707f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="4707f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4707f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4707f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4707f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4707f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4707f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4707f-118">Scenario description</span></span>
<span data-ttu-id="4707f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4707f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4707f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4707f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4707f-121">Hitelesítés hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4707f-121">Adding Certify from the gallery</span></span>
2. <span data-ttu-id="4707f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4707f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-certify-from-the-gallery"></a><span data-ttu-id="4707f-123">Hitelesítés hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4707f-123">Adding Certify from the gallery</span></span>
<span data-ttu-id="4707f-124">Az Azure AD integrálása a Certify konfigurálásához kell hozzáadnia Certify a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="4707f-124">To configure the integration of Certify into Azure AD, you need to add Certify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4707f-125">**Hitelesítés hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4707f-125">**To add Certify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4707f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4707f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4707f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4707f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4707f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4707f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4707f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="4707f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4707f-133">Írja be a keresőmezőbe, **Certify**.</span><span class="sxs-lookup"><span data-stu-id="4707f-133">In the search box, type **Certify**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/tutorial_certify_search.png)

5. <span data-ttu-id="4707f-135">Az eredmények panelen válassza ki a **Certify**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4707f-135">In the results panel, select **Certify**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/tutorial_certify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4707f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4707f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4707f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="4707f-138">In this section, you configure and test Azure AD single sign-on with Certify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4707f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a partner felhasználónak, a Certify a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="4707f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Certify is to a user in Azure AD.</span></span> <span data-ttu-id="4707f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Certify közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4707f-140">In other words, a link relationship between an Azure AD user and the related user in Certify needs to be established.</span></span>

<span data-ttu-id="4707f-141">A Certify, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4707f-141">In Certify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4707f-142">Az Azure AD egyszeri bejelentkezés, hitelesítés tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="4707f-142">To configure and test Azure AD single sign-on with Certify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4707f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="4707f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4707f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="4707f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4707f-145">**[Certify tesztfelhasználó létrehozása](#creating-a-certify-test-user)**  - való Britta Simon egy megfelelője a Certify, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4707f-145">**[Creating a Certify test user](#creating-a-certify-test-user)** - to have a counterpart of Britta Simon in Certify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4707f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4707f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4707f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="4707f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4707f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4707f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4707f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Certify alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4707f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Certify application.</span></span>

<span data-ttu-id="4707f-150">**Hitelesítés az Azure AD az egyszeri bejelentkezés konfigurálása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4707f-150">**To configure Azure AD single sign-on with Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="4707f-151">Az Azure portálon a a **Certify** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4707f-151">In the Azure portal, on the **Certify** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4707f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4707f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_samlbase.png)

3. <span data-ttu-id="4707f-155">Az a **tanúsítására tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4707f-155">On the **Certify Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_url.png)

    <span data-ttu-id="4707f-157">Az a **azonosító** szövegmező, írja be az URL-cím:`https://www.certify.com`</span><span class="sxs-lookup"><span data-stu-id="4707f-157">In the **Identifier** textbox, type the URL: `https://www.certify.com`</span></span>

4. <span data-ttu-id="4707f-158">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4707f-158">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_certificate.png) 

5. <span data-ttu-id="4707f-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4707f-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4707f-162">A a **tanúsítása Configuration** kattintson **konfigurálása tanúsítására** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="4707f-162">On the **Certify Configuration** section, click **Configure Certify** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4707f-163">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="4707f-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_configure.png) 

7. <span data-ttu-id="4707f-165">Egyszeri bejelentkezés konfigurálása **Certify** oldalon kell küldeniük a letöltött **Certificate(Raw)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a [Certify támogatási csoport](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="4707f-165">To configure single sign-on on **Certify** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Certify support team](mailto:support@certify.com).</span></span> <span data-ttu-id="4707f-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="4707f-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="4707f-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="4707f-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4707f-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="4707f-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4707f-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4707f-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4707f-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4707f-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="4707f-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="4707f-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4707f-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4707f-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4707f-174">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4707f-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4707f-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4707f-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4707f-178">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="4707f-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4707f-180">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="4707f-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-certify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4707f-182">a.</span><span class="sxs-lookup"><span data-stu-id="4707f-182">a.</span></span> <span data-ttu-id="4707f-183">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4707f-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4707f-184">b.</span><span class="sxs-lookup"><span data-stu-id="4707f-184">b.</span></span> <span data-ttu-id="4707f-185">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4707f-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4707f-186">c.</span><span class="sxs-lookup"><span data-stu-id="4707f-186">c.</span></span> <span data-ttu-id="4707f-187">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4707f-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4707f-188">d.</span><span class="sxs-lookup"><span data-stu-id="4707f-188">d.</span></span> <span data-ttu-id="4707f-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4707f-189">Click **Create**.</span></span>
 
### <a name="creating-a-certify-test-user"></a><span data-ttu-id="4707f-190">Certify tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4707f-190">Creating a Certify test user</span></span>

<span data-ttu-id="4707f-191">Ez a szakasz célja Certify Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4707f-191">The objective of this section is to create a user called Britta Simon in Certify.</span></span> <span data-ttu-id="4707f-192">Igazolnia támogatja közvetlenül az időponthoz kötött kiépítés, alapértelmezett beállítás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4707f-192">Certify supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="4707f-193">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="4707f-193">There is no action item for you in this section.</span></span> <span data-ttu-id="4707f-194">Új felhasználó jön létre az Ha még nem létezik a Certify elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="4707f-194">A new user will be created during an attempt to access Certify if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="4707f-195">Ha hozzon létre manuálisan egy felhasználó van szüksége, forduljon a kell a [Certify támogatási csoport](mailto:support@certify.com).</span><span class="sxs-lookup"><span data-stu-id="4707f-195">If you need to create an user manually, you need to contact the [Certify support team](mailto:support@certify.com).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4707f-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4707f-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4707f-197">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Certify Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="4707f-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Certify.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4707f-199">**Britta Simon hozzárendelése Certify, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="4707f-199">**To assign Britta Simon to Certify, perform the following steps:**</span></span>

1. <span data-ttu-id="4707f-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4707f-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4707f-202">Az alkalmazások listában válassza ki a **Certify**.</span><span class="sxs-lookup"><span data-stu-id="4707f-202">In the applications list, select **Certify**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-certify-tutorial/tutorial_certify_app.png) 

3. <span data-ttu-id="4707f-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4707f-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4707f-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4707f-206">Click **Add** button.</span></span> <span data-ttu-id="4707f-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4707f-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4707f-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4707f-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4707f-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4707f-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4707f-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4707f-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4707f-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4707f-212">Testing single sign-on</span></span>

<span data-ttu-id="4707f-213">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="4707f-213">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="4707f-214">Ha a hozzáférési panelen a Certify csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Certify alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="4707f-214">When you click the Certify tile in the Access Panel, you should get automatically signed-on to your Certify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4707f-215">További források</span><span class="sxs-lookup"><span data-stu-id="4707f-215">Additional resources</span></span>

* [<span data-ttu-id="4707f-216">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="4707f-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4707f-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4707f-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-certify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-certify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-certify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-certify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-certify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-certify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-certify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-certify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-certify-tutorial/tutorial_general_203.png

