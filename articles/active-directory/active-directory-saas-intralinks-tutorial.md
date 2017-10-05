---
title: "Oktatóanyag: Azure Active Directoryval integrált Intralinks |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Intralinks között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ee7fd5b88ac806104002ffb41af11bab4fd1b2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="1da0c-103">Oktatóanyag: Azure Active Directoryval integrált Intralinks</span><span class="sxs-lookup"><span data-stu-id="1da0c-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="1da0c-104">Ebben az oktatóanyagban elsajátíthatja Intralinks integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1da0c-104">In this tutorial, you learn how to integrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1da0c-105">Intralinks integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1da0c-105">Integrating Intralinks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1da0c-106">Megadhatja a Intralinks hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1da0c-106">You can control in Azure AD who has access to Intralinks</span></span>
- <span data-ttu-id="1da0c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Intralinks (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1da0c-107">You can enable your users to automatically get signed-on to Intralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1da0c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1da0c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1da0c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1da0c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1da0c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1da0c-110">Prerequisites</span></span>

<span data-ttu-id="1da0c-111">Konfigurálása az Azure AD-integrációs Intralinks, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1da0c-111">To configure Azure AD integration with Intralinks, you need the following items:</span></span>

- <span data-ttu-id="1da0c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1da0c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1da0c-113">Egy Intralinks egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="1da0c-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1da0c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1da0c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1da0c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1da0c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1da0c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1da0c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1da0c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1da0c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1da0c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1da0c-118">Scenario description</span></span>
<span data-ttu-id="1da0c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1da0c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1da0c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1da0c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1da0c-121">A gyűjteményből Intralinks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1da0c-121">Adding Intralinks from the gallery</span></span>
2. <span data-ttu-id="1da0c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1da0c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-the-gallery"></a><span data-ttu-id="1da0c-123">A gyűjteményből Intralinks hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1da0c-123">Adding Intralinks from the gallery</span></span>
<span data-ttu-id="1da0c-124">Az Azure AD integrálása a Intralinks konfigurálásához kell hozzáadnia Intralinks a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1da0c-124">To configure the integration of Intralinks into Azure AD, you need to add Intralinks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1da0c-125">**A gyűjteményből Intralinks hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1da0c-125">**To add Intralinks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1da0c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1da0c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1da0c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1da0c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1da0c-133">Írja be a keresőmezőbe, **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-133">In the search box, type **Intralinks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="1da0c-135">Az eredmények panelen válassza ki a **Intralinks**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1da0c-135">In the results panel, select **Intralinks**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1da0c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1da0c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1da0c-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Intralinks.</span><span class="sxs-lookup"><span data-stu-id="1da0c-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1da0c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Intralinks a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1da0c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intralinks is to a user in Azure AD.</span></span> <span data-ttu-id="1da0c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Intralinks közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1da0c-140">In other words, a link relationship between an Azure AD user and the related user in Intralinks needs to be established.</span></span>

<span data-ttu-id="1da0c-141">Intralinks, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1da0c-141">In Intralinks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1da0c-142">Az Azure AD egyszeri bejelentkezést a Intralinks tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1da0c-142">To configure and test Azure AD single sign-on with Intralinks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1da0c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1da0c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1da0c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1da0c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1da0c-145">**[Egy Intralinks tesztfelhasználó létrehozása](#creating-an-intralinks-test-user)**  - való Britta Simon valami Intralinks, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1da0c-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - to have a counterpart of Britta Simon in Intralinks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1da0c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1da0c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1da0c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1da0c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1da0c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1da0c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1da0c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Intralinks alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1da0c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="1da0c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Intralinks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1da0c-150">**To configure Azure AD single sign-on with Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="1da0c-151">Az Azure portálon a a **Intralinks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-151">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1da0c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1da0c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="1da0c-155">Az a **Intralinks tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1da0c-155">On the **Intralinks Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="1da0c-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="1da0c-157">In the **Sign-on URL** textbox, type a URL using the following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1da0c-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="1da0c-158">This value is not real.</span></span> <span data-ttu-id="1da0c-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1da0c-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="1da0c-160">Ügyfél [Intralinks ügyfél-támogatási csoport](https://www.intralinks.com/contact-1) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="1da0c-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) to get this value.</span></span> 
 
4. <span data-ttu-id="1da0c-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1da0c-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="1da0c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1da0c-165">Egyszeri bejelentkezés konfigurálása **Intralinks** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** [Intralinks támogatási csoport](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="1da0c-165">To configure single sign-on on **Intralinks** side, you need to send the downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="1da0c-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="1da0c-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="1da0c-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1da0c-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1da0c-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1da0c-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1da0c-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1da0c-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1da0c-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1da0c-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="1da0c-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1da0c-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1da0c-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1da0c-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1da0c-174">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1da0c-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1da0c-178">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1da0c-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1da0c-180">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1da0c-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1da0c-182">a.</span><span class="sxs-lookup"><span data-stu-id="1da0c-182">a.</span></span> <span data-ttu-id="1da0c-183">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1da0c-184">b.</span><span class="sxs-lookup"><span data-stu-id="1da0c-184">b.</span></span> <span data-ttu-id="1da0c-185">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1da0c-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1da0c-186">c.</span><span class="sxs-lookup"><span data-stu-id="1da0c-186">c.</span></span> <span data-ttu-id="1da0c-187">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1da0c-188">d.</span><span class="sxs-lookup"><span data-stu-id="1da0c-188">d.</span></span> <span data-ttu-id="1da0c-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="1da0c-190">Egy Intralinks tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1da0c-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="1da0c-191">Ebben a szakaszban egy Intralinks Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1da0c-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="1da0c-192">Adjon együttműködve [Intralinks támogatási csoport](https://www.intralinks.com/contact-1) a felhasználók hozzáadása a Intralinks platform.</span><span class="sxs-lookup"><span data-stu-id="1da0c-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) to add the users in the Intralinks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1da0c-193">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1da0c-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1da0c-194">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Intralinks Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1da0c-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intralinks.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1da0c-196">**Britta Simon hozzárendelése Intralinks, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1da0c-196">**To assign Britta Simon to Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="1da0c-197">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1da0c-199">Az alkalmazások listában válassza ki a **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-199">In the applications list, select **Intralinks**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="1da0c-201">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1da0c-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-203">Click **Add** button.</span></span> <span data-ttu-id="1da0c-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1da0c-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1da0c-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1da0c-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1da0c-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1da0c-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1da0c-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1da0c-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="1da0c-209">Intralinks keresztül vagy a Elite alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1da0c-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="1da0c-210">Intralinks összes alkalmazáshoz más Intralinks üzlet Nexus alkalmazás kivételével azonos egyszeri bejelentkezési identitás platformot használ.</span><span class="sxs-lookup"><span data-stu-id="1da0c-210">Intralinks uses the same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="1da0c-211">Ezért ha egyetlen más Intralinks alkalmazáshoz használni kívánt majd először létre kell egyszeri bejelentkezés konfigurálása egy elsődleges Intralinks alkalmazáshoz a fent leírt eljárás segítségével.</span><span class="sxs-lookup"><span data-stu-id="1da0c-211">So if you plan to use any other Intralinks application then first you have to configure SSO for one Primary Intralinks application using the procedure described above.</span></span>

<span data-ttu-id="1da0c-212">Ezután kövesse az alábbi eljárás egy másik Intralinks alkalmazás hozzáadása az Ön bérlőjében, amelyek kihasználhatják az egyszeri bejelentkezés az elsődleges alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1da0c-212">After that you can follow the below procedure to add another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="1da0c-213">Ez a funkció csak az Azure AD Premium Termékváltozat használó ügyfelek számára elérhető és nem érhető el az ingyenes vagy alapszintű Termékváltozat ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="1da0c-213">This feature is available only to Azure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="1da0c-214">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-214">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="1da0c-216">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-216">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1da0c-217">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-217">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1da0c-219">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-219">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1da0c-221">Írja be a keresőmezőbe, **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-221">In the search box, type **Intralinks**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="1da0c-223">A **alkalmazás Intralinks hozzáadása** hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1da0c-223">On **Intralinks Add app** perform the following steps:</span></span>

    ![Intralinks keresztül vagy a Elite alkalmazás hozzáadása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="1da0c-225">a.</span><span class="sxs-lookup"><span data-stu-id="1da0c-225">a.</span></span> <span data-ttu-id="1da0c-226">A **neve** szövegmező, írja be például az alkalmazás megfelelő neve **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-226">In **Name** textbox, enter appropriate name of the application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="1da0c-227">b.</span><span class="sxs-lookup"><span data-stu-id="1da0c-227">b.</span></span> <span data-ttu-id="1da0c-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="1da0c-229">Az Azure portálon a a **Intralinks** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-229">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

7. <span data-ttu-id="1da0c-231">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **társított bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-231">On the **Single sign-on** dialog, select **Mode** as **Linked Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="1da0c-233">Beolvasása a SP által kezdeményezett egyszeri bejelentkezési URL-CÍMÉT a [Intralinks team](https://www.intralinks.com/contact-1) más Intralinks alkalmazásához, és adja meg a **konfigurálása bejelentkezési URL-cím** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="1da0c-233">Get the the SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for the other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="1da0c-235">Az URL-cím bejelentkezési szövegmezőben írja be való bejelentkezéshez az Intralinks alkalmazáshoz a következő minta használatával a felhasználók által használt URL-cím:</span><span class="sxs-lookup"><span data-stu-id="1da0c-235">In the Sign On URL textbox, type the URL used by your users to sign-on to your Intralinks application using the following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="1da0c-236">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1da0c-236">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="1da0c-238">Rendelje hozzá az alkalmazás a felhasználók vagy csoportok, ahogy az a szakasz  **[hozzárendelése az Azure AD-teszt felhasználó](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="1da0c-238">Assign the application to user or groups as shown in the section **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="1da0c-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1da0c-239">Testing single sign-on</span></span>

<span data-ttu-id="1da0c-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="1da0c-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1da0c-241">Ha a hozzáférési panelen Intralinks csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Intralinks alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1da0c-241">When you click the Intralinks tile in the Access Panel, you should get automatically signed-on to your Intralinks application.</span></span>
<span data-ttu-id="1da0c-242">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1da0c-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1da0c-243">További források</span><span class="sxs-lookup"><span data-stu-id="1da0c-243">Additional resources</span></span>

* [<span data-ttu-id="1da0c-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1da0c-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1da0c-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1da0c-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

