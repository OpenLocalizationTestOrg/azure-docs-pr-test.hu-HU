---
title: "Oktatóanyag: Azure Active Directoryval integrált Oneteam |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Oneteam között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2e94916c-64ae-4e1a-a8b5-bc6ef7d28c29
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: c4381ca3166bd75bda1179b9a67b2224ba58ae68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oneteam"></a><span data-ttu-id="fbc4e-103">Oktatóanyag: Azure Active Directoryval integrált Oneteam</span><span class="sxs-lookup"><span data-stu-id="fbc4e-103">Tutorial: Azure Active Directory integration with Oneteam</span></span>

<span data-ttu-id="fbc4e-104">Ebben az oktatóanyagban elsajátíthatja Oneteam integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fbc4e-104">In this tutorial, you learn how to integrate Oneteam with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fbc4e-105">Oneteam integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-105">Integrating Oneteam with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fbc4e-106">Megadhatja a Oneteam hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fbc4e-106">You can control in Azure AD who has access to Oneteam</span></span>
- <span data-ttu-id="fbc4e-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Oneteam (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="fbc4e-107">You can enable your users to automatically get signed-on to Oneteam (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fbc4e-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fbc4e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fbc4e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fbc4e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbc4e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbc4e-110">Prerequisites</span></span>

<span data-ttu-id="fbc4e-111">Konfigurálása az Azure AD-integrációs Oneteam, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-111">To configure Azure AD integration with Oneteam, you need the following items:</span></span>

- <span data-ttu-id="fbc4e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fbc4e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fbc4e-113">Egy Oneteam egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fbc4e-113">A Oneteam single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fbc4e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fbc4e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fbc4e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fbc4e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbc4e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fbc4e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-118">Scenario description</span></span>
<span data-ttu-id="fbc4e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fbc4e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fbc4e-121">A gyűjteményből Oneteam hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-121">Adding Oneteam from the gallery</span></span>
2. <span data-ttu-id="fbc4e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fbc4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oneteam-from-the-gallery"></a><span data-ttu-id="fbc4e-123">A gyűjteményből Oneteam hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-123">Adding Oneteam from the gallery</span></span>
<span data-ttu-id="fbc4e-124">Az Azure AD integrálása a Oneteam konfigurálásához kell hozzáadnia Oneteam a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-124">To configure the integration of Oneteam into Azure AD, you need to add Oneteam from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fbc4e-125">**A gyűjteményből Oneteam hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbc4e-125">**To add Oneteam from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fbc4e-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fbc4e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fbc4e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fbc4e-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fbc4e-133">Írja be a keresőmezőbe, **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-133">In the search box, type **Oneteam**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_search.png)

5. <span data-ttu-id="fbc4e-135">Az eredmények panelen válassza ki a **Oneteam**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-135">In the results panel, select **Oneteam**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fbc4e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fbc4e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fbc4e-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Oneteam.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-138">In this section, you configure and test Azure AD single sign-on with Oneteam based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fbc4e-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Oneteam a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Oneteam is to a user in Azure AD.</span></span> <span data-ttu-id="fbc4e-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Oneteam közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-140">In other words, a link relationship between an Azure AD user and the related user in Oneteam needs to be established.</span></span>

<span data-ttu-id="fbc4e-141">Oneteam, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-141">In Oneteam, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fbc4e-142">Az Azure AD egyszeri bejelentkezést a Oneteam tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-142">To configure and test Azure AD single sign-on with Oneteam, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fbc4e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fbc4e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fbc4e-145">**[Oneteam tesztfelhasználó létrehozása](#creating-a-oneteam-test-user)**  - való Britta Simon valami Oneteam, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-145">**[Creating a Oneteam test user](#creating-a-oneteam-test-user)** - to have a counterpart of Britta Simon in Oneteam that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fbc4e-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fbc4e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fbc4e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fbc4e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Oneteam alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Oneteam application.</span></span>

<span data-ttu-id="fbc4e-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Oneteam, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbc4e-150">**To configure Azure AD single sign-on with Oneteam, perform the following steps:**</span></span>

1. <span data-ttu-id="fbc4e-151">Az Azure portálon a a **Oneteam** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-151">In the Azure portal, on the **Oneteam** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fbc4e-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_samlbase.png)

3. <span data-ttu-id="fbc4e-155">Az a **Oneteam tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-155">On the **Oneteam Domain and URLs** section, if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url.png)

    <span data-ttu-id="fbc4e-157">a.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-157">a.</span></span> <span data-ttu-id="fbc4e-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://api.one-team.io/teams/<team name>`</span><span class="sxs-lookup"><span data-stu-id="fbc4e-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.one-team.io/teams/<team name>`</span></span>

    <span data-ttu-id="fbc4e-159">b.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-159">b.</span></span> <span data-ttu-id="fbc4e-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://api.one-team.io/teams/<team name>/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="fbc4e-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.one-team.io/teams/<team name>/auth/saml/callback`</span></span>

4. <span data-ttu-id="fbc4e-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-161">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url1.png)

    <span data-ttu-id="fbc4e-163">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<team name>.one-team.io/`</span><span class="sxs-lookup"><span data-stu-id="fbc4e-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<team name>.one-team.io/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="fbc4e-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-164">These values are not real.</span></span> <span data-ttu-id="fbc4e-165">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="fbc4e-166">Ügyfél [Oneteam ügyfél-támogatási csoport](https://support.one-team.com/hc/requests/new) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-166">Contact [Oneteam Client support team](https://support.one-team.com/hc/requests/new) to get these values.</span></span> 



5. <span data-ttu-id="fbc4e-167">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_certificate.png) 

6. <span data-ttu-id="fbc4e-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="fbc4e-171">Ahhoz, hogy az egyszeri bejelentkezés az alkalmazáshoz konfigurált, a támogatási jegy rendelkező merülhet [Oneteam támogatási csoport](https://support.one-team.com/hc/requests/new) , és adja meg a letöltött **metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-171">To get SSO configured for your application, you can raise the support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new) and provide them the downloaded **Metadata**.</span></span> 

> [!TIP]
> <span data-ttu-id="fbc4e-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fbc4e-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fbc4e-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fbc4e-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fbc4e-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fbc4e-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="fbc4e-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fbc4e-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbc4e-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fbc4e-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fbc4e-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fbc4e-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fbc4e-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fbc4e-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oneteam-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fbc4e-187">a.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-187">a.</span></span> <span data-ttu-id="fbc4e-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fbc4e-189">b.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-189">b.</span></span> <span data-ttu-id="fbc4e-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fbc4e-191">c.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-191">c.</span></span> <span data-ttu-id="fbc4e-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fbc4e-193">d.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-193">d.</span></span> <span data-ttu-id="fbc4e-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-194">Click **Create**.</span></span>
 
### <a name="creating-a-oneteam-test-user"></a><span data-ttu-id="fbc4e-195">Oneteam tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-195">Creating a Oneteam test user</span></span>

<span data-ttu-id="fbc4e-196">Ez a szakasz célja Oneteam Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-196">The objective of this section is to create a user called Britta Simon in Oneteam.</span></span> <span data-ttu-id="fbc4e-197">Oneteam támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-197">Oneteam supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="fbc4e-198">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-198">There is no action item for you in this section.</span></span> <span data-ttu-id="fbc4e-199">Ha még nem létezik egy új felhasználó jön létre az Oneteam, elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-199">A new user will be created during an attempt to access Oneteam, if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="fbc4e-200">Hozzon létre egy felhasználó manuálisan kell, ha a támogatási jegy rendelkező is növelheti [Oneteam támogatási csoport](https://support.one-team.com/hc/requests/new).</span><span class="sxs-lookup"><span data-stu-id="fbc4e-200">If you need to create an user manually, you can raise the support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fbc4e-201">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fbc4e-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fbc4e-202">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Oneteam Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Oneteam.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fbc4e-204">**Britta Simon hozzárendelése Oneteam, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbc4e-204">**To assign Britta Simon to Oneteam, perform the following steps:**</span></span>

1. <span data-ttu-id="fbc4e-205">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fbc4e-207">Az alkalmazások listában válassza ki a **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-207">In the applications list, select **Oneteam**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_app.png) 

3. <span data-ttu-id="fbc4e-209">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fbc4e-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-211">Click **Add** button.</span></span> <span data-ttu-id="fbc4e-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fbc4e-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fbc4e-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fbc4e-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fbc4e-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fbc4e-217">Testing single sign-on</span></span>

<span data-ttu-id="fbc4e-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fbc4e-219">Ha a hozzáférési panelen Oneteam csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Oneteam alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="fbc4e-219">When you click the Oneteam tile in the Access Panel, you should get automatically signed-on to your Oneteam application.</span></span>
<span data-ttu-id="fbc4e-220">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fbc4e-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fbc4e-221">További források</span><span class="sxs-lookup"><span data-stu-id="fbc4e-221">Additional resources</span></span>

* [<span data-ttu-id="fbc4e-222">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fbc4e-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fbc4e-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fbc4e-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_203.png

