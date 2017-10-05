---
title: "Oktatóanyag: Azure Active Directoryval integrált Abintegro |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Abintegro között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 99287e1f-4189-494a-97c8-e1c03d047fd3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: a2a3c1a7a338ee1cb35dd08176ad3bb5f3cdc319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-abintegro"></a><span data-ttu-id="3f2e8-103">Oktatóanyag: Azure Active Directoryval integrált Abintegro</span><span class="sxs-lookup"><span data-stu-id="3f2e8-103">Tutorial: Azure Active Directory integration with Abintegro</span></span>

<span data-ttu-id="3f2e8-104">Ebben az oktatóanyagban elsajátíthatja Abintegro integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-104">In this tutorial, you learn how to integrate Abintegro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f2e8-105">Abintegro integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-105">Integrating Abintegro with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f2e8-106">Megadhatja a Abintegro hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3f2e8-106">You can control in Azure AD who has access to Abintegro</span></span>
- <span data-ttu-id="3f2e8-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Abintegro (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3f2e8-107">You can enable your users to automatically get signed-on to Abintegro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f2e8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3f2e8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f2e8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f2e8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f2e8-110">Prerequisites</span></span>

<span data-ttu-id="3f2e8-111">Konfigurálása az Azure AD-integrációs Abintegro, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-111">To configure Azure AD integration with Abintegro, you need the following items:</span></span>

- <span data-ttu-id="3f2e8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3f2e8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f2e8-113">Egy Abintegro egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3f2e8-113">An Abintegro single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f2e8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f2e8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f2e8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f2e8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f2e8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-118">Scenario description</span></span>
<span data-ttu-id="3f2e8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f2e8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f2e8-121">A gyűjteményből Abintegro hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-121">Adding Abintegro from the gallery</span></span>
2. <span data-ttu-id="3f2e8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f2e8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-abintegro-from-the-gallery"></a><span data-ttu-id="3f2e8-123">A gyűjteményből Abintegro hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-123">Adding Abintegro from the gallery</span></span>
<span data-ttu-id="3f2e8-124">Az Azure AD integrálása a Abintegro konfigurálásához kell hozzáadnia Abintegro a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-124">To configure the integration of Abintegro into Azure AD, you need to add Abintegro from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f2e8-125">**A gyűjteményből Abintegro hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2e8-125">**To add Abintegro from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2e8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f2e8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f2e8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3f2e8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3f2e8-133">Írja be a keresőmezőbe, **Abintegro**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-133">In the search box, type **Abintegro**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_search.png)

5. <span data-ttu-id="3f2e8-135">Az eredmények panelen válassza ki a **Abintegro**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-135">In the results panel, select **Abintegro**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f2e8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f2e8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f2e8-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Abintegro</span><span class="sxs-lookup"><span data-stu-id="3f2e8-138">In this section, you configure and test Azure AD single sign-on with Abintegro based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3f2e8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Abintegro a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Abintegro is to a user in Azure AD.</span></span> <span data-ttu-id="3f2e8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Abintegro közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-140">In other words, a link relationship between an Azure AD user and the related user in Abintegro needs to be established.</span></span>

<span data-ttu-id="3f2e8-141">Abintegro, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-141">In Abintegro, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f2e8-142">Az Azure AD egyszeri bejelentkezést a Abintegro tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-142">To configure and test Azure AD single sign-on with Abintegro, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f2e8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f2e8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f2e8-145">**[Egy Abintegro tesztfelhasználó létrehozása](#creating-an-abintegro-test-user)**  - való Britta Simon valami Abintegro, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-145">**[Creating an Abintegro test user](#creating-an-abintegro-test-user)** - to have a counterpart of Britta Simon in Abintegro that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f2e8-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f2e8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f2e8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f2e8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Abintegro alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Abintegro application.</span></span>

<span data-ttu-id="3f2e8-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Abintegro, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2e8-150">**To configure Azure AD single sign-on with Abintegro, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2e8-151">Az Azure portálon a a **Abintegro** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-151">In the Azure portal, on the **Abintegro** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3f2e8-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_samlbase.png)

3. <span data-ttu-id="3f2e8-155">Az a **Abintegro tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-155">On the **Abintegro Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_url.png)

    <span data-ttu-id="3f2e8-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`</span><span class="sxs-lookup"><span data-stu-id="3f2e8-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f2e8-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-158">This value is not real.</span></span> <span data-ttu-id="3f2e8-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3f2e8-160">Ügyfél [Abintegro ügyfél-támogatási csoport](mailto:support@abintegro.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-160">Contact [Abintegro Client support team](mailto:support@abintegro.com) to get this value.</span></span> 
 
4. <span data-ttu-id="3f2e8-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_certificate.png) 

5. <span data-ttu-id="3f2e8-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-abintegro-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f2e8-165">Egyszeri bejelentkezés konfigurálása **Abintegro** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Abintegro támogatási csoport](mailto:support@abintegro.com).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-165">To configure single sign-on on **Abintegro** side, you need to send the downloaded **Metadata XML** to [Abintegro support team](mailto:support@abintegro.com).</span></span> <span data-ttu-id="3f2e8-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3f2e8-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3f2e8-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f2e8-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f2e8-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f2e8-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f2e8-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f2e8-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3f2e8-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2e8-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2e8-174">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f2e8-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f2e8-178">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f2e8-180">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f2e8-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-abintegro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f2e8-182">a.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-182">a.</span></span> <span data-ttu-id="3f2e8-183">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f2e8-184">b.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-184">b.</span></span> <span data-ttu-id="3f2e8-185">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f2e8-186">c.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-186">c.</span></span> <span data-ttu-id="3f2e8-187">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f2e8-188">d.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-188">d.</span></span> <span data-ttu-id="3f2e8-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-189">Click **Create**.</span></span>
 
### <a name="creating-an-abintegro-test-user"></a><span data-ttu-id="3f2e8-190">Egy Abintegro tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-190">Creating an Abintegro test user</span></span>

<span data-ttu-id="3f2e8-191">Nincs művelet elem ahhoz, hogy a felhasználó Abintegro történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-191">There is no action item for you to configure user provisioning to Abintegro.</span></span> <span data-ttu-id="3f2e8-192">Ha egy hozzárendelt felhasználó megpróbál bejelentkezni, a hozzáférési panelen Abintegro, Abintegro ellenőrzi, hogy a felhasználó létezik-e.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-192">When an assigned user tries to log into Abintegro using the access panel, Abintegro checks whether the user exists.</span></span>
  
<span data-ttu-id="3f2e8-193">Nincs még nincs felhasználói fiók érhető el, ha a Abintegro automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-193">If there is no user account available yet, it is automatically created by Abintegro.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f2e8-194">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3f2e8-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f2e8-195">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Abintegro Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Abintegro.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3f2e8-197">**Britta Simon hozzárendelése Abintegro, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2e8-197">**To assign Britta Simon to Abintegro, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2e8-198">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3f2e8-200">Az alkalmazások listában válassza ki a **Abintegro**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-200">In the applications list, select **Abintegro**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-abintegro-tutorial/tutorial_abintegro_app.png) 

3. <span data-ttu-id="3f2e8-202">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3f2e8-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-204">Click **Add** button.</span></span> <span data-ttu-id="3f2e8-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3f2e8-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f2e8-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f2e8-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f2e8-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3f2e8-210">Testing single sign-on</span></span>

<span data-ttu-id="3f2e8-211">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3f2e8-212">Ha a hozzáférési panelen Abintegro csempére kattint, Abintegro alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="3f2e8-212">When you click the Abintegro tile in the Access Panel, you should get login page of Abintegro application.</span></span>
<span data-ttu-id="3f2e8-213">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f2e8-213">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3f2e8-214">További források</span><span class="sxs-lookup"><span data-stu-id="3f2e8-214">Additional resources</span></span>

* [<span data-ttu-id="3f2e8-215">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3f2e8-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f2e8-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3f2e8-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-abintegro-tutorial/tutorial_general_203.png

