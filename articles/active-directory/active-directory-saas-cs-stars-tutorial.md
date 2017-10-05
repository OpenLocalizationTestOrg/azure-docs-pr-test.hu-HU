---
title: "Oktatóanyag: Azure Active Directoryval integrált CS csillag |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory- és CS csillag között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: acc9160f86e58c7af4779a8bab5627dc5c5ad721
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a><span data-ttu-id="8608a-103">Oktatóanyag: Azure Active Directoryval integrált CS csillag</span><span class="sxs-lookup"><span data-stu-id="8608a-103">Tutorial: Azure Active Directory integration with CS Stars</span></span>

<span data-ttu-id="8608a-104">Ebben az oktatóanyagban elsajátíthatja CS csillag integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8608a-104">In this tutorial, you learn how to integrate CS Stars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8608a-105">CS csillag integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8608a-105">Integrating CS Stars with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8608a-106">Szabályozhatja az Azure AD CS csillag hozzáféréssel rendelkező</span><span class="sxs-lookup"><span data-stu-id="8608a-106">You can control in Azure AD who has access to CS Stars</span></span>
- <span data-ttu-id="8608a-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a CS csillag (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="8608a-107">You can enable your users to automatically get signed-on to CS Stars (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8608a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8608a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8608a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8608a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8608a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8608a-110">Prerequisites</span></span>

<span data-ttu-id="8608a-111">Konfigurálása az Azure AD-integrációs CS csillag, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8608a-111">To configure Azure AD integration with CS Stars, you need the following items:</span></span>

- <span data-ttu-id="8608a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8608a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8608a-113">A Tanúsítványszolgáltatások csillag egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="8608a-113">A CS Stars single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8608a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8608a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8608a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8608a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8608a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8608a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8608a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8608a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8608a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8608a-118">Scenario description</span></span>
<span data-ttu-id="8608a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8608a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8608a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8608a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8608a-121">CS csillag hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8608a-121">Adding CS Stars from the gallery</span></span>
2. <span data-ttu-id="8608a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8608a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cs-stars-from-the-gallery"></a><span data-ttu-id="8608a-123">CS csillag hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8608a-123">Adding CS Stars from the gallery</span></span>
<span data-ttu-id="8608a-124">Az Azure AD CS csillag integrálása konfigurálásához kell hozzáadnia CS csillag a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8608a-124">To configure the integration of CS Stars into Azure AD, you need to add CS Stars from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8608a-125">**A gyűjteményből CS csillag hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8608a-125">**To add CS Stars from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8608a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8608a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8608a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8608a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8608a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8608a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8608a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8608a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8608a-133">Írja be a keresőmezőbe, **CS csillag**.</span><span class="sxs-lookup"><span data-stu-id="8608a-133">In the search box, type **CS Stars**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_search.png)

5. <span data-ttu-id="8608a-135">Az eredmények panelen válassza ki a **CS csillag**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8608a-135">In the results panel, select **CS Stars**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8608a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8608a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8608a-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a CS csillag "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="8608a-138">In this section, you configure and test Azure AD single sign-on with CS Stars based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8608a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a CS csillag tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8608a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CS Stars is to a user in Azure AD.</span></span> <span data-ttu-id="8608a-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a CS csillag közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8608a-140">In other words, a link relationship between an Azure AD user and the related user in CS Stars needs to be established.</span></span>

<span data-ttu-id="8608a-141">CS csillag, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8608a-141">In CS Stars, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8608a-142">Az Azure AD egyszeri bejelentkezést a CS csillag tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8608a-142">To configure and test Azure AD single sign-on with CS Stars, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8608a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8608a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8608a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8608a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8608a-145">**[CS csillag tesztfelhasználó létrehozása](#creating-a-cs-stars-test-user)**  - való egy megfelelője a Britta Simon CS csillag, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8608a-145">**[Creating a CS Stars test user](#creating-a-cs-stars-test-user)** - to have a counterpart of Britta Simon in CS Stars that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8608a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8608a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8608a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8608a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8608a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8608a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8608a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az CS csillag alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8608a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CS Stars application.</span></span>

<span data-ttu-id="8608a-150">**CS csillag konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8608a-150">**To configure Azure AD single sign-on with CS Stars, perform the following steps:**</span></span>

1. <span data-ttu-id="8608a-151">Az Azure portálon a a **CS csillag** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8608a-151">In the Azure portal, on the **CS Stars** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8608a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8608a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_samlbase.png)

3. <span data-ttu-id="8608a-155">Az a **CS csillag tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8608a-155">On the **CS Stars Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_url.png)

    <span data-ttu-id="8608a-157">a.</span><span class="sxs-lookup"><span data-stu-id="8608a-157">a.</span></span> <span data-ttu-id="8608a-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="8608a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span></span>

    <span data-ttu-id="8608a-159">b.</span><span class="sxs-lookup"><span data-stu-id="8608a-159">b.</span></span> <span data-ttu-id="8608a-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.csstars.com/enterprise/`</span><span class="sxs-lookup"><span data-stu-id="8608a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.csstars.com/enterprise/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8608a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8608a-161">These values are not real.</span></span> <span data-ttu-id="8608a-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8608a-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8608a-163">Ügyfél [CS csillag ügyfél-támogatási csoport](http://www.marshclearsight.com/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8608a-163">Contact [CS Stars Client support team](http://www.marshclearsight.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="8608a-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8608a-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_certificate.png) 

5. <span data-ttu-id="8608a-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8608a-166">Click **Save** button.</span></span>

    <span data-ttu-id="8608a-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="8608a-167">![Configure Single Sign-On](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span></span>
6. <span data-ttu-id="8608a-168">Egyszeri bejelentkezés konfigurálása **CS csillag** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [CS csillag támogatási csoport](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="8608a-168">To configure single sign-on on **CS Stars** side, you need to send the downloaded **Metadata XML** to [CS Stars support team](http://www.marshclearsight.com/support/).</span></span> 
<CE>

> [!TIP]
> <span data-ttu-id="8608a-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8608a-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8608a-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8608a-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8608a-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8608a-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8608a-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8608a-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="8608a-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8608a-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8608a-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8608a-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8608a-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8608a-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8608a-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8608a-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8608a-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8608a-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8608a-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8608a-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8608a-184">a.</span><span class="sxs-lookup"><span data-stu-id="8608a-184">a.</span></span> <span data-ttu-id="8608a-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8608a-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8608a-186">b.</span><span class="sxs-lookup"><span data-stu-id="8608a-186">b.</span></span> <span data-ttu-id="8608a-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8608a-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8608a-188">c.</span><span class="sxs-lookup"><span data-stu-id="8608a-188">c.</span></span> <span data-ttu-id="8608a-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8608a-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8608a-190">d.</span><span class="sxs-lookup"><span data-stu-id="8608a-190">d.</span></span> <span data-ttu-id="8608a-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8608a-191">Click **Create**.</span></span>
 
### <a name="creating-a-cs-stars-test-user"></a><span data-ttu-id="8608a-192">CS csillag tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8608a-192">Creating a CS Stars test user</span></span>

<span data-ttu-id="8608a-193">Ez a szakasz célja CS csillag Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8608a-193">The objective of this section is to create a user called Britta Simon in CS Stars.</span></span>

<span data-ttu-id="8608a-194">A felhasználó CS csillag létrehozott beszerzéséhez kapcsolatba kell lépnie a [CS csillag támogatási csoport](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="8608a-194">To get a user created in CS Stars, you need to contact your [CS Stars support team](http://www.marshclearsight.com/support/).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8608a-195">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8608a-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8608a-196">Ebben a szakaszban engedélyezze Britta Simon legyen Azure egyszeri bejelentkezés az AD CS csillag hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="8608a-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CS Stars.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8608a-198">**A következő lépésekkel Britta Simon CS csillagok rendeléséhez:**</span><span class="sxs-lookup"><span data-stu-id="8608a-198">**To assign Britta Simon to CS Stars, perform the following steps:**</span></span>

1. <span data-ttu-id="8608a-199">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8608a-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8608a-201">Az alkalmazások listában válassza ki a **CS csillag**.</span><span class="sxs-lookup"><span data-stu-id="8608a-201">In the applications list, select **CS Stars**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_app.png) 

3. <span data-ttu-id="8608a-203">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8608a-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8608a-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8608a-205">Click **Add** button.</span></span> <span data-ttu-id="8608a-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8608a-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8608a-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8608a-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8608a-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8608a-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8608a-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8608a-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8608a-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8608a-211">Testing single sign-on</span></span>

<span data-ttu-id="8608a-212">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="8608a-212">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="8608a-213">Ha a hozzáférési panelen CS csillag csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az CS csillag alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8608a-213">When you click the CS Stars tile in the Access Panel, you should get automatically signed-on to your CS Stars application.</span></span>
 

## <a name="additional-resources"></a><span data-ttu-id="8608a-214">További források</span><span class="sxs-lookup"><span data-stu-id="8608a-214">Additional resources</span></span>

* [<span data-ttu-id="8608a-215">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8608a-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8608a-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8608a-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png

