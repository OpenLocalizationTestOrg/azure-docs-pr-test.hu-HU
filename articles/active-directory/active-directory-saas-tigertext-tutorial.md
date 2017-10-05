---
title: "Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TigerText biztonságos Messenger között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: e101e5fc84b032b66dd0636bab8bff128791f77c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a><span data-ttu-id="c2adb-103">Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger</span><span class="sxs-lookup"><span data-stu-id="c2adb-103">Tutorial: Azure Active Directory integration with TigerText Secure Messenger</span></span>

<span data-ttu-id="c2adb-104">Ebben az oktatóanyagban elsajátíthatja TigerText biztonságos Messenger integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c2adb-104">In this tutorial, you learn how to integrate TigerText Secure Messenger with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2adb-105">TigerText biztonságos Messenger integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c2adb-105">Integrating TigerText Secure Messenger with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c2adb-106">Megadhatja a TigerText biztonságos Messenger hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c2adb-106">You can control in Azure AD who has access to TigerText Secure Messenger</span></span>
- <span data-ttu-id="c2adb-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett TigerText biztonságos Messenger (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="c2adb-107">You can enable your users to automatically get signed-on to TigerText Secure Messenger (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c2adb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c2adb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c2adb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2adb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2adb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2adb-110">Prerequisites</span></span>

<span data-ttu-id="c2adb-111">Az Azure AD-integrációs konfigurálásához TigerText biztonságos Messenger, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c2adb-111">To configure Azure AD integration with TigerText Secure Messenger, you need the following items:</span></span>

- <span data-ttu-id="c2adb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c2adb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2adb-113">Egy TigerText biztonságos Messenger egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c2adb-113">A TigerText Secure Messenger single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2adb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c2adb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2adb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c2adb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2adb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c2adb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2adb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2adb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2adb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c2adb-118">Scenario description</span></span>
<span data-ttu-id="c2adb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c2adb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2adb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c2adb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2adb-121">Adja hozzá a TigerText biztonságos Messenger a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c2adb-121">Add TigerText Secure Messenger from the gallery</span></span>
2. <span data-ttu-id="c2adb-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c2adb-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a><span data-ttu-id="c2adb-123">Adja hozzá a TigerText biztonságos Messenger a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="c2adb-123">Add TigerText Secure Messenger from the gallery</span></span>
<span data-ttu-id="c2adb-124">Az Azure AD integrálása a TigerText biztonságos Messenger konfigurálásához kell hozzáadnia TigerText biztonságos Messenger a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c2adb-124">To configure the integration of TigerText Secure Messenger into Azure AD, you need to add TigerText Secure Messenger from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c2adb-125">**A gyűjteményből TigerText biztonságos Messenger hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2adb-125">**To add TigerText Secure Messenger from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c2adb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c2adb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c2adb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c2adb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c2adb-133">Írja be a keresőmezőbe, **TigerText biztonságos Messenger**, jelölje be **TigerText biztonságos Messenger** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2adb-133">In the search box, type **TigerText Secure Messenger**, select **TigerText Secure Messenger** from result panel then click **Add** button to add the application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c2adb-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c2adb-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c2adb-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="c2adb-136">In this section, you configure and test Azure AD single sign-on with TigerText Secure Messenger based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2adb-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó TigerText biztonságos Messenger a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c2adb-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TigerText Secure Messenger is to a user in Azure AD.</span></span> <span data-ttu-id="c2adb-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a TigerText biztonságos Messenger közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c2adb-138">In other words, a link relationship between an Azure AD user and the related user in TigerText Secure Messenger needs to be established.</span></span>

<span data-ttu-id="c2adb-139">A TigerText biztonságos Messenger, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c2adb-139">In TigerText Secure Messenger, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c2adb-140">Az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c2adb-140">To configure and test Azure AD single sign-on with TigerText Secure Messenger, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c2adb-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c2adb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c2adb-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c2adb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2adb-143">**[TigerText biztonságos Messenger tesztfelhasználó létrehozása](#create-a-tigertext-secure-messenger-test-user)**  - való egy megfelelője a Britta Simon TigerText biztonságos Messenger, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c2adb-143">**[Create a TigerText Secure Messenger test user](#create-a-tigertext-secure-messenger-test-user)** - to have a counterpart of Britta Simon in TigerText Secure Messenger that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2adb-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c2adb-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2adb-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c2adb-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c2adb-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c2adb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c2adb-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az TigerText biztonságos Messenger alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c2adb-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TigerText Secure Messenger application.</span></span>

<span data-ttu-id="c2adb-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása TigerText biztonságos Messenger, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="c2adb-148">**To configure Azure AD single sign-on with TigerText Secure Messenger, perform the following steps:**</span></span>

1. <span data-ttu-id="c2adb-149">Az Azure portálon a a **TigerText biztonságos Messenger** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-149">In the Azure portal, on the **TigerText Secure Messenger** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c2adb-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c2adb-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. <span data-ttu-id="c2adb-153">Az a **TigerText biztonságos Messenger tartománya és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c2adb-153">On the **TigerText Secure Messenger Domain and URLs** section, perform the following steps:</span></span>

    ![TigerText biztonságos Messenger tartománya és URL-címek szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    <span data-ttu-id="c2adb-155">a.</span><span class="sxs-lookup"><span data-stu-id="c2adb-155">a.</span></span> <span data-ttu-id="c2adb-156">Az a **bejelentkezési URL-cím** szövegmezőhöz típus az URL-címet:`https://home.tigertext.com`</span><span class="sxs-lookup"><span data-stu-id="c2adb-156">In the **Sign-on URL** textbox, type URL as: `https://home.tigertext.com`</span></span>

    <span data-ttu-id="c2adb-157">b.</span><span class="sxs-lookup"><span data-stu-id="c2adb-157">b.</span></span> <span data-ttu-id="c2adb-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span><span class="sxs-lookup"><span data-stu-id="c2adb-158">In the **Identifier** textbox, type a URL using the following pattern: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c2adb-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c2adb-159">This value is not real.</span></span> <span data-ttu-id="c2adb-160">Frissítse ezt az értéket a tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="c2adb-160">Update this value with the actual Identifier.</span></span> <span data-ttu-id="c2adb-161">Ügyfél [TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c2adb-161">Contact [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) to get this value.</span></span> 
 
4. <span data-ttu-id="c2adb-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c2adb-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. <span data-ttu-id="c2adb-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-164">Click **Save** button.</span></span>

    ![Mentés gomb](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c2adb-166">Ahhoz, hogy az egyszeri bejelentkezés az alkalmazáshoz konfigurált, lépjen kapcsolatba [TigerText biztonságos Messenger támogatási csoport](mailTo:prosupport@tigertext.com) , és adja meg azokat a **letöltött metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-166">To get single sign-on configured for your application, contact [TigerText Secure Messenger support team](mailTo:prosupport@tigertext.com) and provide them the **Downloaded metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="c2adb-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c2adb-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c2adb-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c2adb-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c2adb-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2adb-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c2adb-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c2adb-170">Create an Azure AD test user</span></span>
<span data-ttu-id="c2adb-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c2adb-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c2adb-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c2adb-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c2adb-174">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2adb-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2adb-178">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c2adb-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2adb-180">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c2adb-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Felhasználó párbeszédpanel](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2adb-182">a.</span><span class="sxs-lookup"><span data-stu-id="c2adb-182">a.</span></span> <span data-ttu-id="c2adb-183">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2adb-184">b.</span><span class="sxs-lookup"><span data-stu-id="c2adb-184">b.</span></span> <span data-ttu-id="c2adb-185">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2adb-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2adb-186">c.</span><span class="sxs-lookup"><span data-stu-id="c2adb-186">c.</span></span> <span data-ttu-id="c2adb-187">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c2adb-188">d.</span><span class="sxs-lookup"><span data-stu-id="c2adb-188">d.</span></span> <span data-ttu-id="c2adb-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-189">Click **Create**.</span></span>
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a><span data-ttu-id="c2adb-190">TigerText biztonságos Messenger tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2adb-190">Create a TigerText Secure Messenger test user</span></span>

<span data-ttu-id="c2adb-191">Ebben a szakaszban egy TigerText Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c2adb-191">In this section, you create a user called Britta Simon in TigerText.</span></span> <span data-ttu-id="c2adb-192">Lépjen kapcsolatba [TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) a felhasználók hozzáadása a TigerText platform.</span><span class="sxs-lookup"><span data-stu-id="c2adb-192">Please reach out to [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) to add the users in the TigerText platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c2adb-193">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c2adb-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="c2adb-194">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TigerText biztonságos Messenger Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c2adb-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TigerText Secure Messenger.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c2adb-196">**Britta Simon hozzárendelése TigerText biztonságos Messenger, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="c2adb-196">**To assign Britta Simon to TigerText Secure Messenger, perform the following steps:**</span></span>

1. <span data-ttu-id="c2adb-197">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c2adb-199">Az alkalmazások listában válassza ki a **TigerText biztonságos Messenger**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-199">In the applications list, select **TigerText Secure Messenger**.</span></span>

    ![TigerText biztonságos Messenger alkalmazáslistában](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. <span data-ttu-id="c2adb-201">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c2adb-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c2adb-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2adb-203">Click **Add** button.</span></span> <span data-ttu-id="c2adb-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2adb-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c2adb-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c2adb-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c2adb-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2adb-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2adb-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c2adb-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c2adb-209">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c2adb-209">Test single sign-on</span></span>

<span data-ttu-id="c2adb-210">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c2adb-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c2adb-211">Ha a hozzáférési panelen TigerText csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az TigerText alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c2adb-211">When you click the TigerText tile in the Access Panel, you should get automatically signed-on to your TigerText application.</span></span> <span data-ttu-id="c2adb-212">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c2adb-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2adb-213">További források</span><span class="sxs-lookup"><span data-stu-id="c2adb-213">Additional resources</span></span>

* [<span data-ttu-id="c2adb-214">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c2adb-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2adb-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c2adb-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

