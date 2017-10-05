---
title: "Oktatóanyag: Azure Active Directoryval integrált RolePoint |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és RolePoint között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: fcde562484f4401e9f936614b9978f839f4aa290
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a><span data-ttu-id="5a7ed-103">Oktatóanyag: Azure Active Directoryval integrált RolePoint</span><span class="sxs-lookup"><span data-stu-id="5a7ed-103">Tutorial: Azure Active Directory integration with RolePoint</span></span>

<span data-ttu-id="5a7ed-104">Ebben az oktatóanyagban elsajátíthatja RolePoint integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a7ed-104">In this tutorial, you learn how to integrate RolePoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5a7ed-105">RolePoint integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-105">Integrating RolePoint with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5a7ed-106">Megadhatja a RolePoint hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5a7ed-106">You can control in Azure AD who has access to RolePoint</span></span>
- <span data-ttu-id="5a7ed-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett RolePoint (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5a7ed-107">You can enable your users to automatically get signed-on to RolePoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5a7ed-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5a7ed-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5a7ed-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5a7ed-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a7ed-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5a7ed-110">Prerequisites</span></span>

<span data-ttu-id="5a7ed-111">Konfigurálása az Azure AD-integrációs RolePoint, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-111">To configure Azure AD integration with RolePoint, you need the following items:</span></span>

- <span data-ttu-id="5a7ed-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5a7ed-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5a7ed-113">Egy RolePoint egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="5a7ed-113">A RolePoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5a7ed-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5a7ed-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5a7ed-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5a7ed-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5a7ed-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5a7ed-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-118">Scenario description</span></span>
<span data-ttu-id="5a7ed-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5a7ed-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5a7ed-121">A gyűjteményből RolePoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-121">Adding RolePoint from the gallery</span></span>
2. <span data-ttu-id="5a7ed-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5a7ed-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rolepoint-from-the-gallery"></a><span data-ttu-id="5a7ed-123">A gyűjteményből RolePoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-123">Adding RolePoint from the gallery</span></span>
<span data-ttu-id="5a7ed-124">Az Azure AD integrálása a RolePoint konfigurálásához kell hozzáadnia RolePoint a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-124">To configure the integration of RolePoint into Azure AD, you need to add RolePoint from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5a7ed-125">**A gyűjteményből RolePoint hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a7ed-125">**To add RolePoint from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5a7ed-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5a7ed-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5a7ed-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5a7ed-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5a7ed-133">Írja be a keresőmezőbe, **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-133">In the search box, type **RolePoint**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_search.png)

5. <span data-ttu-id="5a7ed-135">Az eredmények panelen válassza ki a **RolePoint**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-135">In the results panel, select **RolePoint**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5a7ed-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5a7ed-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5a7ed-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján RolePoint</span><span class="sxs-lookup"><span data-stu-id="5a7ed-138">In this section, you configure and test Azure AD single sign-on with RolePoint based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5a7ed-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó RolePoint a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-139">For single sign-on to work, Azure AD needs to know what the counterpart user in RolePoint is to a user in Azure AD.</span></span> <span data-ttu-id="5a7ed-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a RolePoint közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-140">In other words, a link relationship between an Azure AD user and the related user in RolePoint needs to be established.</span></span>

<span data-ttu-id="5a7ed-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** RolePoint a.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in RolePoint.</span></span>

<span data-ttu-id="5a7ed-142">Az Azure AD egyszeri bejelentkezést a RolePoint tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-142">To configure and test Azure AD single sign-on with RolePoint, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5a7ed-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5a7ed-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5a7ed-145">**[RolePoint tesztfelhasználó létrehozása](#creating-a-rolepoint-test-user)**  - való Britta Simon valami RolePoint, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-145">**[Creating a RolePoint test user](#creating-a-rolepoint-test-user)** - to have a counterpart of Britta Simon in RolePoint that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5a7ed-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5a7ed-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5a7ed-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5a7ed-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az RolePoint alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RolePoint application.</span></span>

<span data-ttu-id="5a7ed-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés RolePoint, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a7ed-150">**To configure Azure AD single sign-on with RolePoint, perform the following steps:**</span></span>

1. <span data-ttu-id="5a7ed-151">Az Azure portálon a a **RolePoint** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-151">In the Azure portal, on the **RolePoint** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5a7ed-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_samlbase.png)

3. <span data-ttu-id="5a7ed-155">Az a **RolePoint tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-155">On the **RolePoint Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_url.png)

    <span data-ttu-id="5a7ed-157">a.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-157">a.</span></span> <span data-ttu-id="5a7ed-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.rolepoint.com/login`</span><span class="sxs-lookup"><span data-stu-id="5a7ed-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.rolepoint.com/login`</span></span>
    
    <span data-ttu-id="5a7ed-159">b.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-159">b.</span></span> <span data-ttu-id="5a7ed-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.rolepoint.com/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="5a7ed-160">In the **Identifier** textbox, type a URL using the following pattern: `https://app.rolepoint.com/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5a7ed-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-161">These values are not the real.</span></span> <span data-ttu-id="5a7ed-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="5a7ed-163">Itt javasoljuk, hogy a karakterlánc egyedi azonosító használata a Identifier.Contact [RolePoint támogatási csoport](mailto:info@rolepoint.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-163">Here we suggest you to use the unique value of string in the Identifier.Contact [RolePoint support team](mailto:info@rolepoint.com) to get the value.</span></span> 
 
4. <span data-ttu-id="5a7ed-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_certificate.png) 

5. <span data-ttu-id="5a7ed-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_general_400.png)


6. <span data-ttu-id="5a7ed-168">Egyszeri bejelentkezés konfigurálása **RolePoint** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [RolePoint támogatási csoport](mailto:info@rolepoint.com).</span><span class="sxs-lookup"><span data-stu-id="5a7ed-168">To configure single sign-on on **RolePoint** side, you need to send the downloaded **Metadata XML** to [RolePoint support team](mailto:info@rolepoint.com).</span></span>

> [!TIP]
> <span data-ttu-id="5a7ed-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="5a7ed-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5a7ed-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5a7ed-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5a7ed-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5a7ed-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="5a7ed-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5a7ed-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a7ed-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5a7ed-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5a7ed-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5a7ed-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5a7ed-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5a7ed-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5a7ed-184">a.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-184">a.</span></span> <span data-ttu-id="5a7ed-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5a7ed-186">b.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-186">b.</span></span> <span data-ttu-id="5a7ed-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5a7ed-188">c.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-188">c.</span></span> <span data-ttu-id="5a7ed-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5a7ed-190">d.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-190">d.</span></span> <span data-ttu-id="5a7ed-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-191">Click **Create**.</span></span>
 
### <a name="creating-a-rolepoint-test-user"></a><span data-ttu-id="5a7ed-192">RolePoint tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-192">Creating a RolePoint test user</span></span>

<span data-ttu-id="5a7ed-193">Ebben a szakaszban egy RolePoint Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-193">In this section, you create a user called Britta Simon in RolePoint.</span></span> <span data-ttu-id="5a7ed-194">Együttműködve [RolePoint támogatási csoport](mailto:info@rolepoint.com) a felhasználók hozzáadása a RolePoint platform.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-194">Work with [RolePoint support team](mailto:info@rolepoint.com) to add the users in the RolePoint platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5a7ed-195">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5a7ed-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5a7ed-196">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés RolePoint Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RolePoint.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5a7ed-198">**Britta Simon hozzárendelése RolePoint, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a7ed-198">**To assign Britta Simon to RolePoint, perform the following steps:**</span></span>

1. <span data-ttu-id="5a7ed-199">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5a7ed-201">Az alkalmazások listában válassza ki a **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-201">In the applications list, select **RolePoint**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_app.png) 

3. <span data-ttu-id="5a7ed-203">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5a7ed-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-205">Click **Add** button.</span></span> <span data-ttu-id="5a7ed-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5a7ed-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5a7ed-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5a7ed-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5a7ed-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5a7ed-211">Testing single sign-on</span></span>

<span data-ttu-id="5a7ed-212">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5a7ed-213">Ha a hozzáférési panelen RolePoint csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az RolePoint alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5a7ed-213">When you click the RolePoint tile in the Access Panel, you should get automatically signed-on to your RolePoint application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5a7ed-214">További források</span><span class="sxs-lookup"><span data-stu-id="5a7ed-214">Additional resources</span></span>

* [<span data-ttu-id="5a7ed-215">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5a7ed-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5a7ed-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5a7ed-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_203.png

