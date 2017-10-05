---
title: "Oktatóanyag: Azure Active Directoryval integrált Heroku |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Heroku között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: d30605e4757b484f327a784b73f939b62ef59373
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a><span data-ttu-id="a58b9-103">Oktatóanyag: Azure Active Directoryval integrált Heroku</span><span class="sxs-lookup"><span data-stu-id="a58b9-103">Tutorial: Azure Active Directory integration with Heroku</span></span>

<span data-ttu-id="a58b9-104">Ebben az oktatóanyagban elsajátíthatja Heroku integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a58b9-104">In this tutorial, you learn how to integrate Heroku with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a58b9-105">Heroku integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a58b9-105">Integrating Heroku with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a58b9-106">Megadhatja a Heroku hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a58b9-106">You can control in Azure AD who has access to Heroku</span></span>
- <span data-ttu-id="a58b9-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Heroku (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a58b9-107">You can enable your users to automatically get signed-on to Heroku (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a58b9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a58b9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a58b9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a58b9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a58b9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a58b9-110">Prerequisites</span></span>

<span data-ttu-id="a58b9-111">Konfigurálása az Azure AD-integrációs Heroku, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a58b9-111">To configure Azure AD integration with Heroku, you need the following items:</span></span>

- <span data-ttu-id="a58b9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a58b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a58b9-113">Egy Heroku egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a58b9-113">A Heroku single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a58b9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a58b9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a58b9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a58b9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a58b9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a58b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a58b9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a58b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a58b9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a58b9-118">Scenario description</span></span>
<span data-ttu-id="a58b9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a58b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a58b9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a58b9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a58b9-121">A gyűjteményből Heroku hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a58b9-121">Adding Heroku from the gallery</span></span>
2. <span data-ttu-id="a58b9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a58b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-heroku-from-the-gallery"></a><span data-ttu-id="a58b9-123">A gyűjteményből Heroku hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a58b9-123">Adding Heroku from the gallery</span></span>
<span data-ttu-id="a58b9-124">Az Azure AD integrálása a Heroku konfigurálásához kell hozzáadnia Heroku a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a58b9-124">To configure the integration of Heroku into Azure AD, you need to add Heroku from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a58b9-125">**A gyűjteményből Heroku hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a58b9-125">**To add Heroku from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a58b9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a58b9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a58b9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a58b9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a58b9-133">Írja be a keresőmezőbe, **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-133">In the search box, type **Heroku**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_search.png)

5. <span data-ttu-id="a58b9-135">Az eredmények panelen válassza ki a **Heroku**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a58b9-135">In the results panel, select **Heroku**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a58b9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a58b9-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="a58b9-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Heroku</span><span class="sxs-lookup"><span data-stu-id="a58b9-138">In this section, you configure and test Azure AD single sign-on with Heroku based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a58b9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Heroku a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a58b9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Heroku is to a user in Azure AD.</span></span> <span data-ttu-id="a58b9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Heroku közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a58b9-140">In other words, a link relationship between an Azure AD user and the related user in Heroku needs to be established.</span></span>

<span data-ttu-id="a58b9-141">Heroku, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a58b9-141">In Heroku, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a58b9-142">Az Azure AD egyszeri bejelentkezést a Heroku tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a58b9-142">To configure and test Azure AD single sign-on with Heroku, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a58b9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a58b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a58b9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a58b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a58b9-145">**[Heroku tesztfelhasználó létrehozása](#creating-a-heroku-test-user)**  - való Britta Simon valami Heroku, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a58b9-145">**[Creating a Heroku test user](#creating-a-heroku-test-user)** - to have a counterpart of Britta Simon in Heroku that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a58b9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a58b9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a58b9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a58b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a58b9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a58b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a58b9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Heroku alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a58b9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Heroku application.</span></span>

<span data-ttu-id="a58b9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Heroku, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a58b9-150">**To configure Azure AD single sign-on with Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="a58b9-151">Az Azure portálon a a **Heroku** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-151">In the Azure portal, on the **Heroku** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a58b9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a58b9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_samlbase.png)

3. <span data-ttu-id="a58b9-155">Az a **Heroku tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a58b9-155">On the **Heroku Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_url.png)

    <span data-ttu-id="a58b9-157">a.</span><span class="sxs-lookup"><span data-stu-id="a58b9-157">a.</span></span> <span data-ttu-id="a58b9-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="a58b9-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>    
    `https://sso.heroku.com/saml/<company-name>/init`

    <span data-ttu-id="a58b9-159">b.</span><span class="sxs-lookup"><span data-stu-id="a58b9-159">b.</span></span> <span data-ttu-id="a58b9-160">Az a **azonosító URL-t** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="a58b9-160">In the **Identifier URL** textbox, type a URL using the following pattern:</span></span>            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    ><span data-ttu-id="a58b9-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a58b9-161">These values are not real.</span></span> <span data-ttu-id="a58b9-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a58b9-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a58b9-163">Ezek az értékek beolvasása Heroku csoport, amely a cikk későbbi szakaszaiban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="a58b9-163">You get these values from Heroku team, which is described in later sections of this article.</span></span> 
        
4. <span data-ttu-id="a58b9-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a58b9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_certificate.png) 

5. <span data-ttu-id="a58b9-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a58b9-168">Ahhoz, hogy a SSO Heroku, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a58b9-168">To enable SSO in Heroku, perform the following steps:</span></span>
   
    <span data-ttu-id="a58b9-169">a.</span><span class="sxs-lookup"><span data-stu-id="a58b9-169">a.</span></span> <span data-ttu-id="a58b9-170">Jelentkezzen be rendszergazdaként a Heroku fiókjába.</span><span class="sxs-lookup"><span data-stu-id="a58b9-170">Log in to the Heroku account as an administrator.</span></span>

    <span data-ttu-id="a58b9-171">b.</span><span class="sxs-lookup"><span data-stu-id="a58b9-171">b.</span></span> <span data-ttu-id="a58b9-172">Kattintson a **Beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="a58b9-172">Click the **Settings** tab.</span></span>

    <span data-ttu-id="a58b9-173">c.</span><span class="sxs-lookup"><span data-stu-id="a58b9-173">c.</span></span> <span data-ttu-id="a58b9-174">Az a **egyszeri bejelentkezési oldalon**, kattintson a **metaadatok feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-174">On the **Single Sign On Page**, click **Upload Metadata**.</span></span>

    <span data-ttu-id="a58b9-175">d.</span><span class="sxs-lookup"><span data-stu-id="a58b9-175">d.</span></span> <span data-ttu-id="a58b9-176">Töltse fel az Azure-portálról letöltött metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="a58b9-176">Upload the metadata file, which you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="a58b9-177">e.</span><span class="sxs-lookup"><span data-stu-id="a58b9-177">e.</span></span> <span data-ttu-id="a58b9-178">Ha a telepítés sikeres volt, a rendszergazdák számára jelenik meg a megerősítő párbeszédpanelen, és a végfelhasználók számára a Egyszeri bejelentkezési URL-címe jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a58b9-178">When the setup is successful, administrators see a confirmation dialog and the URL of the SSO Login for end users is displayed.</span></span> 

    <span data-ttu-id="a58b9-179">f.</span><span class="sxs-lookup"><span data-stu-id="a58b9-179">f.</span></span> <span data-ttu-id="a58b9-180">Másolás a **Heroku bejelentkezési URL-címet** és **Heroku Entitásazonosító** értéket, majd térjen vissza **Heroku tartomány és az URL-címek** Azure-portálon szakaszt, és illessze be ezeket az értékeket be a **bejelentkezési URL-cím** és **azonosítója** szövegmezők kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="a58b9-180">Copy the **Heroku Login URL** and **Heroku Entity ID** values and go back to **Heroku Domain and URLs** section in Azure portal and paste these values into the **Sign-On Url** and **Identifier** textboxes respectively.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_52.png) 
    
8. <span data-ttu-id="a58b9-182">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-182">Click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="a58b9-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a58b9-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a58b9-184">Ez az alkalmazás a hozzáadása után a **Active Directory vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a58b9-184">After adding this app from the **Active Directory Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a58b9-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a58b9-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a58b9-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a58b9-186">Creating an Azure AD test user</span></span>

<span data-ttu-id="a58b9-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a58b9-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a58b9-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a58b9-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a58b9-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a58b9-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a58b9-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a58b9-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a58b9-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a58b9-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-heroku-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a58b9-198">a.</span><span class="sxs-lookup"><span data-stu-id="a58b9-198">a.</span></span> <span data-ttu-id="a58b9-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a58b9-200">b.</span><span class="sxs-lookup"><span data-stu-id="a58b9-200">b.</span></span> <span data-ttu-id="a58b9-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a58b9-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a58b9-202">c.</span><span class="sxs-lookup"><span data-stu-id="a58b9-202">c.</span></span> <span data-ttu-id="a58b9-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a58b9-204">d.</span><span class="sxs-lookup"><span data-stu-id="a58b9-204">d.</span></span> <span data-ttu-id="a58b9-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-205">Click **Create**.</span></span>
 
### <a name="creating-a-heroku-test-user"></a><span data-ttu-id="a58b9-206">Heroku tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a58b9-206">Creating a Heroku test user</span></span>

<span data-ttu-id="a58b9-207">Ebben a szakaszban egy Heroku Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a58b9-207">In this section, you create a user called Britta Simon in Heroku.</span></span> <span data-ttu-id="a58b9-208">Heroku támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a58b9-208">Heroku supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="a58b9-209">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="a58b9-209">There is no action item for you in this section.</span></span> <span data-ttu-id="a58b9-210">Új felhasználó jön létre, amikor Heroku elérését, ha a felhasználó még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="a58b9-210">A new user is created when accessing Heroku if the user doesn't exist yet.</span></span> <span data-ttu-id="a58b9-211">Miután a fiók ki van építve, a végfelhasználó ellenőrző e-mailt kap, és nyugtázási hivatkozásra kattintva meg kell.</span><span class="sxs-lookup"><span data-stu-id="a58b9-211">After the account is provisioned, the end user receives a verification email and needs to click the acknowledgement link.</span></span>

>[!NOTE]
><span data-ttu-id="a58b9-212">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Heroku ügyfél-támogatási csoport](https://www.heroku.com/support).</span><span class="sxs-lookup"><span data-stu-id="a58b9-212">If you need to create a user manually, you need to contact the [Heroku Client support team](https://www.heroku.com/support).</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a58b9-213">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a58b9-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a58b9-214">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Heroku Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a58b9-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Heroku.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a58b9-216">**Britta Simon hozzárendelése Heroku, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a58b9-216">**To assign Britta Simon to Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="a58b9-217">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a58b9-219">Az alkalmazások listában válassza ki a **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-219">In the applications list, select **Heroku**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_app.png) 

3. <span data-ttu-id="a58b9-221">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a58b9-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a58b9-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a58b9-223">Click **Add** button.</span></span> <span data-ttu-id="a58b9-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a58b9-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a58b9-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a58b9-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a58b9-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a58b9-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a58b9-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a58b9-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a58b9-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a58b9-229">Testing single sign-on</span></span>

<span data-ttu-id="a58b9-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a58b9-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a58b9-231">Ha a hozzáférési panelen Heroku csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Heroku alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="a58b9-231">When you click the Heroku tile in the Access Panel, you should get automatically signed-on to your Heroku application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a58b9-232">További források</span><span class="sxs-lookup"><span data-stu-id="a58b9-232">Additional resources</span></span>

* [<span data-ttu-id="a58b9-233">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a58b9-233">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a58b9-234">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a58b9-234">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_203.png
