---
title: "Oktatóanyag: Azure Active Directoryval integrált WORKS MOBILE |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és WORKS MOBILE között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 139a1968a59424eae278de3e7fa227ad340a1eb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a><span data-ttu-id="939a6-103">Oktatóanyag: Azure Active Directoryval integrált WORKS MOBILE</span><span class="sxs-lookup"><span data-stu-id="939a6-103">Tutorial: Azure Active Directory integration with WORKS MOBILE</span></span>

<span data-ttu-id="939a6-104">Ebben az oktatóanyagban elsajátíthatja WORKS MOBILE integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="939a6-104">In this tutorial, you learn how to integrate WORKS MOBILE with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="939a6-105">WORKS MOBILE integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="939a6-105">Integrating WORKS MOBILE with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="939a6-106">Megadhatja a WORKS MOBILE hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="939a6-106">You can control in Azure AD who has access to WORKS MOBILE</span></span>
- <span data-ttu-id="939a6-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett WORKS Mobile (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="939a6-107">You can enable your users to automatically get signed-on to WORKS MOBILE (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="939a6-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="939a6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="939a6-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="939a6-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="939a6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="939a6-110">Prerequisites</span></span>

<span data-ttu-id="939a6-111">Konfigurálása az Azure AD-integrációs WORKS MOBILE, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="939a6-111">To configure Azure AD integration with WORKS MOBILE, you need the following items:</span></span>

- <span data-ttu-id="939a6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="939a6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="939a6-113">Egy WORKS MOBILE egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="939a6-113">A WORKS MOBILE single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="939a6-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="939a6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="939a6-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="939a6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="939a6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="939a6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="939a6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="939a6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="939a6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="939a6-118">Scenario description</span></span>
<span data-ttu-id="939a6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="939a6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="939a6-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="939a6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="939a6-121">A gyűjteményből WORKS MOBILE hozzáadása</span><span class="sxs-lookup"><span data-stu-id="939a6-121">Adding WORKS MOBILE from the gallery</span></span>
2. <span data-ttu-id="939a6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="939a6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-works-mobile-from-the-gallery"></a><span data-ttu-id="939a6-123">A gyűjteményből WORKS MOBILE hozzáadása</span><span class="sxs-lookup"><span data-stu-id="939a6-123">Adding WORKS MOBILE from the gallery</span></span>
<span data-ttu-id="939a6-124">Az Azure AD integrálása a WORKS MOBILE konfigurálásához kell hozzáadnia WORKS MOBILE a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="939a6-124">To configure the integration of WORKS MOBILE into Azure AD, you need to add WORKS MOBILE from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="939a6-125">**A gyűjteményből WORKS MOBILE hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="939a6-125">**To add WORKS MOBILE from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="939a6-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="939a6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="939a6-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="939a6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="939a6-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="939a6-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="939a6-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="939a6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="939a6-133">Írja be a keresőmezőbe, **WORKS MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="939a6-133">In the search box, type **WORKS MOBILE**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. <span data-ttu-id="939a6-135">Az eredmények panelen válassza ki a **WORKS MOBILE**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="939a6-135">In the results panel, select **WORKS MOBILE**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="939a6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="939a6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="939a6-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést és a "Britta Simon." nevű tesztfelhasználó alapján WORKS MOBILE</span><span class="sxs-lookup"><span data-stu-id="939a6-138">In this section, you configure and test Azure AD single sign-on with WORKS MOBILE based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="939a6-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó WORKS MOBILE a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="939a6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in WORKS MOBILE is to a user in Azure AD.</span></span> <span data-ttu-id="939a6-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a WORKS MOBILE közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="939a6-140">In other words, a link relationship between an Azure AD user and the related user in WORKS MOBILE needs to be established.</span></span>

<span data-ttu-id="939a6-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a WORKS MOBILE.</span><span class="sxs-lookup"><span data-stu-id="939a6-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in WORKS MOBILE.</span></span>

<span data-ttu-id="939a6-142">Az Azure AD egyszeri bejelentkezést és a WORKS MOBILE tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="939a6-142">To configure and test Azure AD single sign-on with WORKS MOBILE, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="939a6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="939a6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="939a6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="939a6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="939a6-145">**[WORKS MOBILE tesztfelhasználó létrehozása](#creating-a-works-mobile-test-user)**  - való egy megfelelője a Britta Simon WORKS MOBILESZKÖZ, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="939a6-145">**[Creating a WORKS MOBILE test user](#creating-a-works-mobile-test-user)** - to have a counterpart of Britta Simon in WORKS MOBILE that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="939a6-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="939a6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="939a6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="939a6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="939a6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="939a6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="939a6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az WORKS MOBILE alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="939a6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your WORKS MOBILE application.</span></span>

<span data-ttu-id="939a6-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása WORKS MOBILE, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="939a6-150">**To configure Azure AD single sign-on with WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="939a6-151">Az Azure portálon a a **WORKS MOBILE** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="939a6-151">In the Azure portal, on the **WORKS MOBILE** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="939a6-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="939a6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. <span data-ttu-id="939a6-155">Az a **WORKS MOBILE tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="939a6-155">On the **WORKS MOBILE Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    <span data-ttu-id="939a6-157">a.</span><span class="sxs-lookup"><span data-stu-id="939a6-157">a.</span></span> <span data-ttu-id="939a6-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.worksmobile.com/jp/myservice`</span><span class="sxs-lookup"><span data-stu-id="939a6-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.worksmobile.com/jp/myservice`</span></span>

    <span data-ttu-id="939a6-159">b.</span><span class="sxs-lookup"><span data-stu-id="939a6-159">b.</span></span> <span data-ttu-id="939a6-160">Az a **azonosító** szövegmező, írja be az értéket, mint`worksmobile.com`</span><span class="sxs-lookup"><span data-stu-id="939a6-160">In the **Identifier** textbox, type the value as `worksmobile.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="939a6-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="939a6-161">This value is not real.</span></span> <span data-ttu-id="939a6-162">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="939a6-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="939a6-163">Ügyfél [WORKS MOBILESZKÖZ ügyfél-támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="939a6-163">Contact [WORKS MOBILE Client support team](mailto:dl_ssoinfo@worksmobile.com) to get this value.</span></span> 
 
4. <span data-ttu-id="939a6-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="939a6-164">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. <span data-ttu-id="939a6-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="939a6-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="939a6-168">A a **WORKS MOBILE konfigurációs** kattintson **WORKS MOBILALKALMAZÁS konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="939a6-168">On the **WORKS MOBILE Configuration** section, click **Configure WORKS MOBILE** to open **Configure sign-on** window.</span></span> <span data-ttu-id="939a6-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="939a6-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. <span data-ttu-id="939a6-171">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [WORKS MOBILE támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) és adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="939a6-171">To get SSO configured for your application, contact [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) and provide them with the following information:</span></span> 

    <span data-ttu-id="939a6-172">• A letöltött **tanúsítványfájl**</span><span class="sxs-lookup"><span data-stu-id="939a6-172">• The downloaded **Certificate file**</span></span>

    <span data-ttu-id="939a6-173">• A **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-címe**</span><span class="sxs-lookup"><span data-stu-id="939a6-173">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="939a6-174">• A **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="939a6-174">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="939a6-175">• A **kijelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="939a6-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="939a6-176">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="939a6-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="939a6-177">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="939a6-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="939a6-178">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="939a6-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="939a6-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="939a6-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="939a6-180">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="939a6-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="939a6-182">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="939a6-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="939a6-183">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="939a6-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="939a6-185">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="939a6-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="939a6-187">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="939a6-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="939a6-189">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="939a6-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="939a6-191">a.</span><span class="sxs-lookup"><span data-stu-id="939a6-191">a.</span></span> <span data-ttu-id="939a6-192">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="939a6-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="939a6-193">b.</span><span class="sxs-lookup"><span data-stu-id="939a6-193">b.</span></span> <span data-ttu-id="939a6-194">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="939a6-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="939a6-195">c.</span><span class="sxs-lookup"><span data-stu-id="939a6-195">c.</span></span> <span data-ttu-id="939a6-196">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="939a6-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="939a6-197">d.</span><span class="sxs-lookup"><span data-stu-id="939a6-197">d.</span></span> <span data-ttu-id="939a6-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="939a6-198">Click **Create**.</span></span>
 
### <a name="creating-a-works-mobile-test-user"></a><span data-ttu-id="939a6-199">WORKS MOBILE tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="939a6-199">Creating a WORKS MOBILE test user</span></span>

 <span data-ttu-id="939a6-200">Ebben a szakaszban egy felhasználó Britta Simon meghívta WORKS MOBILE hoz létre.</span><span class="sxs-lookup"><span data-stu-id="939a6-200">In this section, you create a user called Britta Simon in WORKS MOBILE.</span></span> <span data-ttu-id="939a6-201">Adjon együttműködve [WORKS MOBILE támogatási csoport](mailto:dl_ssoinfo@worksmobile.com) a felhasználók hozzáadása az a WORKS MOBILESZKÖZ-platformokon.</span><span class="sxs-lookup"><span data-stu-id="939a6-201">Please work with [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) to add the users in the WORKS MOBILE platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="939a6-202">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="939a6-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="939a6-203">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés WORKS MOBILE Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="939a6-203">In this section, you enable Britta Simon to use Azure single sign-on by granting access to WORKS MOBILE.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="939a6-205">**Britta Simon hozzárendelése WORKS MOBILE, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="939a6-205">**To assign Britta Simon to WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="939a6-206">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="939a6-206">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="939a6-208">Az alkalmazások listában válassza ki a **WORKS MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="939a6-208">In the applications list, select **WORKS MOBILE**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. <span data-ttu-id="939a6-210">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="939a6-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="939a6-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="939a6-212">Click **Add** button.</span></span> <span data-ttu-id="939a6-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="939a6-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="939a6-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="939a6-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="939a6-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="939a6-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="939a6-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="939a6-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="939a6-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="939a6-218">Testing single sign-on</span></span>

<span data-ttu-id="939a6-219">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="939a6-219">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="939a6-220">Ha a hozzáférési panelen WORKS MOBILE csempére kattint, meg kell beolvasása automatikusan bejelentkezett az WORKS MOBILE-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="939a6-220">When you click the WORKS MOBILE tile in the Access Panel, you should get automatically signed-on to your WORKS MOBILE application.</span></span>
<span data-ttu-id="939a6-221">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="939a6-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="939a6-222">További források</span><span class="sxs-lookup"><span data-stu-id="939a6-222">Additional resources</span></span>

* [<span data-ttu-id="939a6-223">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="939a6-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="939a6-224">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="939a6-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

