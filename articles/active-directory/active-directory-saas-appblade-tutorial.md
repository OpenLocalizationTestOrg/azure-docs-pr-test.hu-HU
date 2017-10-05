---
title: "Oktatóanyag: Azure Active Directoryval integrált AppBlade |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és AppBlade között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 7820a70b34b6d25ba81b17c472159d08904335d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="0dec5-103">Oktatóanyag: Azure Active Directoryval integrált AppBlade</span><span class="sxs-lookup"><span data-stu-id="0dec5-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="0dec5-104">Ebben az oktatóanyagban elsajátíthatja AppBlade integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0dec5-104">In this tutorial, you learn how to integrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0dec5-105">AppBlade integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0dec5-105">Integrating AppBlade with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0dec5-106">Megadhatja a AppBlade hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0dec5-106">You can control in Azure AD who has access to AppBlade</span></span>
- <span data-ttu-id="0dec5-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett AppBlade (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0dec5-107">You can enable your users to automatically get signed-on to AppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0dec5-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0dec5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0dec5-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0dec5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0dec5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0dec5-110">Prerequisites</span></span>

<span data-ttu-id="0dec5-111">Konfigurálása az Azure AD-integrációs AppBlade, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0dec5-111">To configure Azure AD integration with AppBlade, you need the following items:</span></span>

- <span data-ttu-id="0dec5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0dec5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0dec5-113">Egy AppBlade egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0dec5-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0dec5-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0dec5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0dec5-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0dec5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0dec5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0dec5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0dec5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0dec5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0dec5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0dec5-118">Scenario description</span></span>
<span data-ttu-id="0dec5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0dec5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0dec5-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0dec5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0dec5-121">A gyűjteményből AppBlade hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0dec5-121">Adding AppBlade from the gallery</span></span>
2. <span data-ttu-id="0dec5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0dec5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-the-gallery"></a><span data-ttu-id="0dec5-123">A gyűjteményből AppBlade hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0dec5-123">Adding AppBlade from the gallery</span></span>
<span data-ttu-id="0dec5-124">Az Azure AD integrálása a AppBlade konfigurálásához kell hozzáadnia AppBlade a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0dec5-124">To configure the integration of AppBlade into Azure AD, you need to add AppBlade from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0dec5-125">**A gyűjteményből AppBlade hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0dec5-125">**To add AppBlade from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0dec5-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0dec5-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0dec5-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0dec5-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0dec5-133">Írja be a keresőmezőbe, **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-133">In the search box, type **AppBlade**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="0dec5-135">Az eredmények panelen válassza ki a **AppBlade**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0dec5-135">In the results panel, select **AppBlade**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0dec5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0dec5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0dec5-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AppBlade</span><span class="sxs-lookup"><span data-stu-id="0dec5-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0dec5-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó AppBlade a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0dec5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppBlade is to a user in Azure AD.</span></span> <span data-ttu-id="0dec5-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a AppBlade közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0dec5-140">In other words, a link relationship between an Azure AD user and the related user in AppBlade needs to be established.</span></span>

<span data-ttu-id="0dec5-141">AppBlade, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0dec5-141">In AppBlade, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0dec5-142">Az Azure AD egyszeri bejelentkezést a AppBlade tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0dec5-142">To configure and test Azure AD single sign-on with AppBlade, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0dec5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0dec5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0dec5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0dec5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0dec5-145">**[Egy AppBlade tesztfelhasználó létrehozása](#creating-an-appblade-test-user)**  - való Britta Simon valami AppBlade, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0dec5-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - to have a counterpart of Britta Simon in AppBlade that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0dec5-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0dec5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0dec5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0dec5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0dec5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0dec5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0dec5-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az AppBlade alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0dec5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="0dec5-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés AppBlade, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0dec5-150">**To configure Azure AD single sign-on with AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="0dec5-151">Az Azure portálon a a **AppBlade** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-151">In the Azure portal, on the **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0dec5-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0dec5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="0dec5-155">Az a **AppBlade tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0dec5-155">On the **AppBlade Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="0dec5-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="0dec5-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0dec5-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="0dec5-158">This value is not real.</span></span> <span data-ttu-id="0dec5-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="0dec5-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="0dec5-160">Ügyfél [AppBlade ügyfél-támogatási csoport](mailto:support@appblade.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="0dec5-160">Contact [AppBlade Client support team](mailto:support@appblade.com) to get the value.</span></span> 
 
4. <span data-ttu-id="0dec5-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0dec5-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="0dec5-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0dec5-165">Egyszeri bejelentkezés konfigurálása **AppBlade** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [AppBlade támogatási csoport](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="0dec5-165">To configure single sign-on on **AppBlade** side, you need to send the downloaded **Metadata XML** to [AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="0dec5-166">Is, kérje meg őket, konfigurálhatja a **SSO kiállítójának URL-címe** , `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="0dec5-166">Also, please ask them to configure the **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="0dec5-167">Ezt a beállítást az egyszeri bejelentkezés működéséhez szükség.</span><span class="sxs-lookup"><span data-stu-id="0dec5-167">This setting is required for single sign-on to work.</span></span>


> [!TIP]
> <span data-ttu-id="0dec5-168">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0dec5-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0dec5-169">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0dec5-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0dec5-170">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0dec5-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0dec5-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0dec5-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="0dec5-172">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0dec5-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0dec5-174">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0dec5-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0dec5-175">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0dec5-177">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0dec5-179">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0dec5-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0dec5-181">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0dec5-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0dec5-183">a.</span><span class="sxs-lookup"><span data-stu-id="0dec5-183">a.</span></span> <span data-ttu-id="0dec5-184">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0dec5-185">b.</span><span class="sxs-lookup"><span data-stu-id="0dec5-185">b.</span></span> <span data-ttu-id="0dec5-186">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0dec5-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0dec5-187">c.</span><span class="sxs-lookup"><span data-stu-id="0dec5-187">c.</span></span> <span data-ttu-id="0dec5-188">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0dec5-189">d.</span><span class="sxs-lookup"><span data-stu-id="0dec5-189">d.</span></span> <span data-ttu-id="0dec5-190">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="0dec5-191">Egy AppBlade tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0dec5-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="0dec5-192">Ez a szakasz célja AppBlade Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0dec5-192">The objective of this section is to create a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="0dec5-193">AppBlade támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="0dec5-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="0dec5-194">**Győződjön meg arról, hogy a tartománynév beállítása a AppBlade a felhasználók átadása. Követően, hogy csak a közvetlenül az időponthoz kötött felhasználók átadásához működik.**</span><span class="sxs-lookup"><span data-stu-id="0dec5-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only the just-in-time user provisioning works.**</span></span>

<span data-ttu-id="0dec5-195">Ha a felhasználó egy végződő AppBlade állította be a fiókot, a tartomány, akkor a felhasználó automatikusan csatlakoznak a fiók tagja a jogosultsági szint megadja az e-mail címet, amely az egyik "Basic" (alapszintű felhasználó csak telepíthető alkalmazások), a "Csoport tagja" (olyan felhasználó, aki projektek kezelését és feltöltését alkalmazás új verziója is) vagy a "Rendszergazda" (a fiók teljes rendszergazdai jogosultságokkal).</span><span class="sxs-lookup"><span data-stu-id="0dec5-195">If the user has an email address ending with the domain configured by AppBlade for your account, then the user will automatically join the account as a member with the permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges to the account).</span></span> <span data-ttu-id="0dec5-196">Ehhez általában egy válassza ki a Basic, és előreléphet a felhasználók manuálisan egy rendszergazdai bejelentkezés (AppBlade kell előre konfigurálhatja az vagy egy e-mail-alapú rendszergazdai bejelentkezés vagy lépteti elő a felhasználó bejelentkezése után az ügyfél nevében) keresztül.</span><span class="sxs-lookup"><span data-stu-id="0dec5-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs to configure either an email-based admin login in advance or promote a user on behalf of the customer after login).</span></span>

<span data-ttu-id="0dec5-197">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="0dec5-197">There is no action item for you in this section.</span></span> <span data-ttu-id="0dec5-198">Új felhasználó jön létre az AppBlade elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="0dec5-198">A new user is created during an attempt to access AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="0dec5-199">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [AppBlade támogatási csoport](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="0dec5-199">If you need to create a user manually, you need to contact the [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0dec5-200">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0dec5-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0dec5-201">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés AppBlade Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0dec5-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppBlade.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0dec5-203">**Britta Simon hozzárendelése AppBlade, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0dec5-203">**To assign Britta Simon to AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="0dec5-204">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0dec5-206">Az alkalmazások listában válassza ki a **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-206">In the applications list, select **AppBlade**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="0dec5-208">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0dec5-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0dec5-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0dec5-210">Click **Add** button.</span></span> <span data-ttu-id="0dec5-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0dec5-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0dec5-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0dec5-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0dec5-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0dec5-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0dec5-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0dec5-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0dec5-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0dec5-216">Testing single sign-on</span></span>

<span data-ttu-id="0dec5-217">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="0dec5-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="0dec5-218">Ha a hozzáférési panelen AppBlade csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az AppBlade alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="0dec5-218">When you click the AppBlade tile in the Access Panel, you should get automatically signed-on to your AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0dec5-219">További források</span><span class="sxs-lookup"><span data-stu-id="0dec5-219">Additional resources</span></span>

* [<span data-ttu-id="0dec5-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0dec5-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0dec5-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0dec5-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

