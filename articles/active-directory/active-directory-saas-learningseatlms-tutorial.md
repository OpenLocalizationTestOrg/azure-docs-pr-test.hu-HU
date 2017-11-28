---
title: "Oktatóanyag: Azure Active Directoryval integrált tanulási ülések LMS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a tanulási ülések LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: 877e0288fdd1f590acf064c204aff0741539b112
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a><span data-ttu-id="08b38-103">Oktatóanyag: Azure Active Directoryval integrált tanulási ülések LMS</span><span class="sxs-lookup"><span data-stu-id="08b38-103">Tutorial: Azure Active Directory integration with Learning Seat LMS</span></span>

<span data-ttu-id="08b38-104">Ebben az oktatóanyagban elsajátíthatja tanulási ülések LMS integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="08b38-104">In this tutorial, you learn how to integrate Learning Seat LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="08b38-105">Learning ülések LMS integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="08b38-105">Integrating Learning Seat LMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="08b38-106">Megadhatja a tanulási ülések LMS hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="08b38-106">You can control in Azure AD who has access to Learning Seat LMS</span></span>
- <span data-ttu-id="08b38-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a tanulási ülések LMS (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="08b38-107">You can enable your users to automatically get signed-on to Learning Seat LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="08b38-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="08b38-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="08b38-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="08b38-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="08b38-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="08b38-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08b38-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="08b38-111">Prerequisites</span></span>

<span data-ttu-id="08b38-112">Learning ülések LMS konfigurálása az Azure AD-integrációs, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="08b38-112">To configure Azure AD integration with Learning Seat LMS, you need the following items:</span></span>

- <span data-ttu-id="08b38-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="08b38-113">An Azure AD subscription</span></span>
- <span data-ttu-id="08b38-114">A Learning ülések LMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="08b38-114">A Learning Seat LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="08b38-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="08b38-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="08b38-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="08b38-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="08b38-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="08b38-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="08b38-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="08b38-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="08b38-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="08b38-119">Scenario description</span></span>
<span data-ttu-id="08b38-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="08b38-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="08b38-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="08b38-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="08b38-122">Learning ülések LMS hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="08b38-122">Adding Learning Seat LMS from the gallery</span></span>
2. <span data-ttu-id="08b38-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="08b38-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-seat-lms-from-the-gallery"></a><span data-ttu-id="08b38-124">Learning ülések LMS hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="08b38-124">Adding Learning Seat LMS from the gallery</span></span>
<span data-ttu-id="08b38-125">Az Azure AD integrálása a tanulási ülések LMS konfigurálásához kell hozzáadnia tanulási ülések LMS a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="08b38-125">To configure the integration of Learning Seat LMS into Azure AD, you need to add Learning Seat LMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="08b38-126">**Adja hozzá a tanulási ülések LMS a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="08b38-126">**To add Learning Seat LMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="08b38-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="08b38-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="08b38-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="08b38-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="08b38-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="08b38-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="08b38-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="08b38-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="08b38-134">Írja be a keresőmezőbe, **tanulási ülések LMS**.</span><span class="sxs-lookup"><span data-stu-id="08b38-134">In the search box, type **Learning Seat LMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_search.png)

5. <span data-ttu-id="08b38-136">Az eredmények panelen válassza ki a **tanulási ülések LMS**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="08b38-136">In the results panel, select **Learning Seat LMS**, and then click **Add** button to add the application.</span></span>


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="08b38-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="08b38-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="08b38-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a tanulási ülések LMS "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="08b38-138">In this section, you configure and test Azure AD single sign-on with Learning Seat LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="08b38-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a tanulási ülések LMS tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="08b38-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learning Seat LMS is to a user in Azure AD.</span></span> <span data-ttu-id="08b38-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a tanulási ülések LMS közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="08b38-140">In other words, a link relationship between an Azure AD user and the related user in Learning Seat LMS needs to be established.</span></span>

<span data-ttu-id="08b38-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a tanulási ülések LMS.</span><span class="sxs-lookup"><span data-stu-id="08b38-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Learning Seat LMS.</span></span>

<span data-ttu-id="08b38-142">Az Azure AD egyszeri bejelentkezést a tanulási ülések LMS tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="08b38-142">To configure and test Azure AD single sign-on with Learning Seat LMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="08b38-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="08b38-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="08b38-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="08b38-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="08b38-145">**[Learning ülések LMS tesztfelhasználó létrehozása](#creating-a-learnconnect-test-user)**  - való egy megfelelője a Britta Simon tanulási ülések LMS, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="08b38-145">**[Creating a Learning Seat LMS test user](#creating-a-learnconnect-test-user)** - to have a counterpart of Britta Simon in Learning Seat LMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="08b38-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="08b38-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="08b38-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="08b38-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="08b38-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="08b38-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="08b38-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a tanulási ülések LMS alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="08b38-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learning Seat LMS application.</span></span>

<span data-ttu-id="08b38-150">**Learning ülések LMS konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="08b38-150">**To configure Azure AD single sign-on with Learning Seat LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="08b38-151">Az Azure portálon a a **tanulási ülések LMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="08b38-151">In the Azure portal, on the **Learning Seat LMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="08b38-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="08b38-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_samlbase.png)

3. <span data-ttu-id="08b38-155">Az a **tanulási ülések LMS tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="08b38-155">On the **Learning Seat LMS Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url.png)

    <span data-ttu-id="08b38-157">a.</span><span class="sxs-lookup"><span data-stu-id="08b38-157">a.</span></span> <span data-ttu-id="08b38-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="08b38-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com`</span></span>

    <span data-ttu-id="08b38-159">b.</span><span class="sxs-lookup"><span data-stu-id="08b38-159">b.</span></span> <span data-ttu-id="08b38-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span><span class="sxs-lookup"><span data-stu-id="08b38-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span></span>

4. <span data-ttu-id="08b38-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="08b38-161">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url2.png)

    <span data-ttu-id="08b38-163">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="08b38-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="08b38-164">Ezek az értékek nem a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="08b38-164">These values are not the real values.</span></span> <span data-ttu-id="08b38-165">Ezek az értékek frissíti a tényleges azonosítója, válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="08b38-165">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="08b38-166">Ügyfél [tanulási ülések támogatási csoport](http://help.learningseatlms.com/help) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="08b38-166">Contact [Learning Seat support team](http://help.learningseatlms.com/help) to get these values.</span></span> 

5. <span data-ttu-id="08b38-167">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="08b38-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_certificate.png) 

6. <span data-ttu-id="08b38-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="08b38-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="08b38-171">Egyszeri bejelentkezés konfigurálása **tanulási ülések LMS** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [tanulási ülések támogatási csoport](http://help.learningseatlms.com/help).</span><span class="sxs-lookup"><span data-stu-id="08b38-171">To configure single sign-on on **Learning Seat LMS** side, you need to send the downloaded **Metadata XML** to [Learning Seat support team](http://help.learningseatlms.com/help).</span></span>

> [!TIP]
> <span data-ttu-id="08b38-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="08b38-172">You can now read a concise version of these instructions inside the [Azure  portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="08b38-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="08b38-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="08b38-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="08b38-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="08b38-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08b38-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="08b38-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="08b38-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="08b38-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="08b38-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="08b38-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="08b38-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="08b38-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="08b38-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="08b38-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="08b38-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="08b38-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="08b38-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="08b38-187">a.</span><span class="sxs-lookup"><span data-stu-id="08b38-187">a.</span></span> <span data-ttu-id="08b38-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="08b38-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="08b38-189">b.</span><span class="sxs-lookup"><span data-stu-id="08b38-189">b.</span></span> <span data-ttu-id="08b38-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="08b38-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="08b38-191">c.</span><span class="sxs-lookup"><span data-stu-id="08b38-191">c.</span></span> <span data-ttu-id="08b38-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="08b38-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="08b38-193">d.</span><span class="sxs-lookup"><span data-stu-id="08b38-193">d.</span></span> <span data-ttu-id="08b38-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="08b38-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-seat-lms-test-user"></a><span data-ttu-id="08b38-195">Learning ülések LMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08b38-195">Creating a Learning Seat LMS test user</span></span>

<span data-ttu-id="08b38-196">Ebben a szakaszban a tanulási ülések LMS Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="08b38-196">In this section, you create a user called Britta Simon in Learning Seat LMS.</span></span> <span data-ttu-id="08b38-197">Ügyfél [tanulási ülések támogatási csoport](http://help.learningseatlms.com/help) a felhasználók hozzáadása a tanulási ülések LMS alkalmazás a felhasználó adataival.</span><span class="sxs-lookup"><span data-stu-id="08b38-197">Contact [Learning Seat support team](http://help.learningseatlms.com/help) with all the user information to add the users in the Learning Seat LMS application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="08b38-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="08b38-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="08b38-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés tanulási ülések LMS Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="08b38-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learning Seat LMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="08b38-201">**Learning ülések LMS Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="08b38-201">**To assign Britta Simon to Learning Seat LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="08b38-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="08b38-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="08b38-204">Az alkalmazások listában válassza ki a **tanulási ülések LMS**.</span><span class="sxs-lookup"><span data-stu-id="08b38-204">In the applications list, select **Learning Seat LMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_app.png) 

3. <span data-ttu-id="08b38-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="08b38-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="08b38-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="08b38-208">Click **Add** button.</span></span> <span data-ttu-id="08b38-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08b38-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="08b38-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="08b38-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="08b38-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08b38-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="08b38-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08b38-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="08b38-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="08b38-214">Testing single sign-on</span></span>

<span data-ttu-id="08b38-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="08b38-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> 

<span data-ttu-id="08b38-216">A Learning ülések LMS csempe a hozzáférési panelen kattintson, akkor lesz lehet automatikusan bejelentkezett a tanulási ülések LMS alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="08b38-216">Click the Learning Seat LMS tile in the Access Panel, you will be automatically signed-on to your Learning Seat LMS application.</span></span> <span data-ttu-id="08b38-217">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="08b38-217">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08b38-218">További források</span><span class="sxs-lookup"><span data-stu-id="08b38-218">Additional resources</span></span>

* [<span data-ttu-id="08b38-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="08b38-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="08b38-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="08b38-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_203.png

