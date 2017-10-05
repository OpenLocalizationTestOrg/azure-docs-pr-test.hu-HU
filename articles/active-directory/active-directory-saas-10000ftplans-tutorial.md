---
title: "Oktatóanyag: Azure Active Directory-integráció a 10 000 ft tervek |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és 10 000 ft tervek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: c81a6adb3abad58af149bbef6f5dff736c142f51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="dc7e2-103">Oktatóanyag: Azure Active Directory-integráció a 10 000 ft tervek</span><span class="sxs-lookup"><span data-stu-id="dc7e2-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="dc7e2-104">Ebben az oktatóanyagban elsajátíthatja 10 000 ft tervek integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dc7e2-104">In this tutorial, you learn how to integrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dc7e2-105">10 000 ft tervek integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-105">Integrating 10,000ft Plans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dc7e2-106">Megadhatja a 10 000 ft tervek hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dc7e2-106">You can control in Azure AD who has access to 10,000ft Plans</span></span>
- <span data-ttu-id="dc7e2-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett 10 000 ft tervekbe (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="dc7e2-107">You can enable your users to automatically get signed-on to 10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dc7e2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dc7e2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dc7e2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dc7e2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc7e2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dc7e2-110">Prerequisites</span></span>

<span data-ttu-id="dc7e2-111">Az Azure AD-integráció konfigurálása a 10 000 ft tervek, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-111">To configure Azure AD integration with 10,000ft Plans, you need the following items:</span></span>

- <span data-ttu-id="dc7e2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dc7e2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dc7e2-113">A 10 000 ft tervek egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="dc7e2-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dc7e2-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dc7e2-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dc7e2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dc7e2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverziós ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dc7e2-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dc7e2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dc7e2-118">Scenario description</span></span>
<span data-ttu-id="dc7e2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dc7e2-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dc7e2-121">10 000 ft hozzáadása tervek a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="dc7e2-121">Adding 10,000ft Plans from the gallery</span></span>
2. <span data-ttu-id="dc7e2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dc7e2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-the-gallery"></a><span data-ttu-id="dc7e2-123">10 000 ft hozzáadása tervek a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="dc7e2-123">Adding 10,000ft Plans from the gallery</span></span>
<span data-ttu-id="dc7e2-124">Az Azure AD integrálása a 10 000 ft tervek konfigurálásához kell hozzáadnia 10 000 ft tervek a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-124">To configure the integration of 10,000ft Plans into Azure AD, you need to add 10,000ft Plans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dc7e2-125">**10 000 ft csomagok hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dc7e2-125">**To add 10,000ft Plans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dc7e2-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dc7e2-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dc7e2-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="dc7e2-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="dc7e2-133">Írja be a keresőmezőbe, **10 000 ft tervek**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-133">In the search box, type **10,000ft Plans**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="dc7e2-135">Az eredmények panelen válassza ki a **10 000 ft tervek**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-135">In the results panel, select **10,000ft Plans**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dc7e2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dc7e2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dc7e2-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a 10 000 ft "Britta Simon." nevű tesztfelhasználó alapján tervek</span><span class="sxs-lookup"><span data-stu-id="dc7e2-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dc7e2-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó 10 000 ft tervek a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 10,000ft Plans is to a user in Azure AD.</span></span> <span data-ttu-id="dc7e2-140">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó a 10 000 ft hivatkozás kapcsolatának tervek kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-140">In other words, a link relationship between an Azure AD user and the related user in 10,000ft Plans needs to be established.</span></span>

<span data-ttu-id="dc7e2-141">A 10 000 ft csomagokban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-141">In 10,000ft Plans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dc7e2-142">Az Azure AD egyszeri bejelentkezést a 10 000 ft tervek tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-142">To configure and test Azure AD single sign-on with 10,000ft Plans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dc7e2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dc7e2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dc7e2-145">**[10 000 ft létrehozása tervek tesztfelhasználó](#creating-a-10000ft-plans-test-user)**  - kell rendelkeznie egy Britta Simon megfelelője a 10 000 ft terveket, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - to have a counterpart of Britta Simon in 10,000ft Plans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dc7e2-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dc7e2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dc7e2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc7e2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dc7e2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és 10 000 ft tervek alkalmazásában egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="dc7e2-150">**A 10 000 ft tervek konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dc7e2-150">**To configure Azure AD single sign-on with 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="dc7e2-151">Az Azure portálon a a **10 000 ft tervek** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-151">In the Azure portal, on the **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="dc7e2-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="dc7e2-155">Az a **10 000 ft tervek tartományi és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-155">On the **10,000ft Plans Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="dc7e2-157">a.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-157">a.</span></span> <span data-ttu-id="dc7e2-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="dc7e2-158">In the **Sign-on URL** textbox, type the URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="dc7e2-159">b.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-159">b.</span></span> <span data-ttu-id="dc7e2-160">Az a **azonosító** szövegmező, írja be az URL-cím:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="dc7e2-160">In the **Identifier** textbox, type the URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dc7e2-161">A következő **azonosító** eltérő, ha egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-161">The value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="dc7e2-162">Ügyfél [10 000 ft tervek támogatási csoport](https://www.10000ft.com/plans/support) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) to get this value.</span></span> 
 
4. <span data-ttu-id="dc7e2-163">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-163">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="dc7e2-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dc7e2-167">A a **10 000 ft-csomagok konfigurációs** területen kattintson **konfigurálása 10 000 ft tervek** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-167">On the **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dc7e2-168">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="dc7e2-168">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="dc7e2-170">Egyszeri bejelentkezés konfigurálása **10 000 ft tervek** oldalon kell küldeniük a letöltött **Certificate(Raw), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** való [10 000 ft Tervek támogatási csoport](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="dc7e2-170">To configure single sign-on on **10,000ft Plans** side, you need to send the downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="dc7e2-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dc7e2-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dc7e2-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dc7e2-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dc7e2-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dc7e2-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc7e2-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="dc7e2-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="dc7e2-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dc7e2-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dc7e2-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dc7e2-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dc7e2-182">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dc7e2-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dc7e2-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dc7e2-186">a.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-186">a.</span></span> <span data-ttu-id="dc7e2-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dc7e2-188">b.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-188">b.</span></span> <span data-ttu-id="dc7e2-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dc7e2-190">c.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-190">c.</span></span> <span data-ttu-id="dc7e2-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dc7e2-192">d.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-192">d.</span></span> <span data-ttu-id="dc7e2-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="dc7e2-194">A 10 000 ft létrehozása tervek teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="dc7e2-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="dc7e2-195">Ez a szakasz célkitűzése 10 000 ft tervek Britta Simon nevű felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-195">The objective of this section is to create a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="dc7e2-196">10 000 ft tervek támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="dc7e2-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-197">There is no action item for you in this section.</span></span> <span data-ttu-id="dc7e2-198">Új felhasználó jön létre az 10 000 ft tervek elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-198">A new user is created during an attempt to access 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="dc7e2-199">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [10 000 ft tervek támogatási csoport](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="dc7e2-199">If you need to create a user manually, you need to contact the [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dc7e2-200">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dc7e2-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dc7e2-201">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés 10 000 ft tervek Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 10,000ft Plans.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="dc7e2-203">**10 000 ft tervek Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dc7e2-203">**To assign Britta Simon to 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="dc7e2-204">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dc7e2-206">Az alkalmazások listában válassza ki a **10 000 ft tervek**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-206">In the applications list, select **10,000ft Plans**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="dc7e2-208">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="dc7e2-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-210">Click **Add** button.</span></span> <span data-ttu-id="dc7e2-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="dc7e2-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dc7e2-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dc7e2-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dc7e2-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dc7e2-216">Testing single sign-on</span></span>

<span data-ttu-id="dc7e2-217">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="dc7e2-218">Ha a hozzáférési Panel 10 000 ft tervek mozaik gombra kattint, meg kell beolvasása automatikusan bejelentkezett az 10 000 ft tervek alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="dc7e2-218">When you click the 10,000ft Plans tile in the Access Panel, you should get automatically signed-on to your 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="dc7e2-219">További források</span><span class="sxs-lookup"><span data-stu-id="dc7e2-219">Additional resources</span></span>

* [<span data-ttu-id="dc7e2-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dc7e2-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dc7e2-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dc7e2-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

