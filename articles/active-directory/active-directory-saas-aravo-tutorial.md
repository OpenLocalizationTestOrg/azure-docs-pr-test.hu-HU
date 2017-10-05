---
title: "Oktatóanyag: Azure Active Directoryval integrált Aravo |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Aravo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 224939d8-2c9c-4561-968d-62722f5ab5ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2b6da25a22463619180f635954660e6efeef62ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aravo"></a><span data-ttu-id="1a8a2-103">Oktatóanyag: Azure Active Directoryval integrált Aravo</span><span class="sxs-lookup"><span data-stu-id="1a8a2-103">Tutorial: Azure Active Directory integration with Aravo</span></span>

<span data-ttu-id="1a8a2-104">Ebben az oktatóanyagban elsajátíthatja Aravo integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1a8a2-104">In this tutorial, you learn how to integrate Aravo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a8a2-105">Aravo integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-105">Integrating Aravo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1a8a2-106">Megadhatja a Aravo hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1a8a2-106">You can control in Azure AD who has access to Aravo</span></span>
- <span data-ttu-id="1a8a2-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Aravo (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1a8a2-107">You can enable your users to automatically get signed-on to Aravo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1a8a2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1a8a2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1a8a2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a8a2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a8a2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1a8a2-110">Prerequisites</span></span>

<span data-ttu-id="1a8a2-111">Konfigurálása az Azure AD-integrációs Aravo, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-111">To configure Azure AD integration with Aravo, you need the following items:</span></span>

- <span data-ttu-id="1a8a2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1a8a2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a8a2-113">Egy Aravo egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1a8a2-113">An Aravo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a8a2-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a8a2-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a8a2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a8a2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a8a2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a8a2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-118">Scenario description</span></span>
<span data-ttu-id="1a8a2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a8a2-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a8a2-121">A gyűjteményből Aravo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-121">Adding Aravo from the gallery</span></span>
2. <span data-ttu-id="1a8a2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a8a2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aravo-from-the-gallery"></a><span data-ttu-id="1a8a2-123">A gyűjteményből Aravo hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-123">Adding Aravo from the gallery</span></span>
<span data-ttu-id="1a8a2-124">Az Azure AD integrálása a Aravo konfigurálásához kell hozzáadnia Aravo a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-124">To configure the integration of Aravo into Azure AD, you need to add Aravo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1a8a2-125">**A gyűjteményből Aravo hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a8a2-125">**To add Aravo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1a8a2-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1a8a2-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1a8a2-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1a8a2-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1a8a2-133">Írja be a keresőmezőbe, **Aravo**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-133">In the search box, type **Aravo**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_search.png)

5. <span data-ttu-id="1a8a2-135">Az eredmények panelen válassza ki a **Aravo**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-135">In the results panel, select **Aravo**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1a8a2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1a8a2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1a8a2-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Aravo</span><span class="sxs-lookup"><span data-stu-id="1a8a2-138">In this section, you configure and test Azure AD single sign-on with Aravo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1a8a2-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Aravo a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Aravo is to a user in Azure AD.</span></span> <span data-ttu-id="1a8a2-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Aravo közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-140">In other words, a link relationship between an Azure AD user and the related user in Aravo needs to be established.</span></span>

<span data-ttu-id="1a8a2-141">Aravo, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-141">In Aravo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1a8a2-142">Az Azure AD egyszeri bejelentkezést a Aravo tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-142">To configure and test Azure AD single sign-on with Aravo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1a8a2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1a8a2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a8a2-145">**[Egy Aravo tesztfelhasználó létrehozása](#creating-an-aravo-test-user)**  - való Britta Simon valami Aravo, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-145">**[Creating an Aravo test user](#creating-an-aravo-test-user)** - to have a counterpart of Britta Simon in Aravo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1a8a2-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a8a2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1a8a2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1a8a2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Aravo alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Aravo application.</span></span>

<span data-ttu-id="1a8a2-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Aravo, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a8a2-150">**To configure Azure AD single sign-on with Aravo, perform the following steps:**</span></span>

1. <span data-ttu-id="1a8a2-151">Az Azure portálon a a **Aravo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-151">In the Azure portal, on the **Aravo** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1a8a2-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_samlbase.png)

3. <span data-ttu-id="1a8a2-155">Az a **Aravo tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-155">On the **Aravo Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_url.png)

    <span data-ttu-id="1a8a2-157">a.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-157">a.</span></span> <span data-ttu-id="1a8a2-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.aravo.com`</span><span class="sxs-lookup"><span data-stu-id="1a8a2-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.aravo.com`</span></span>

    <span data-ttu-id="1a8a2-159">b.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-159">b.</span></span> <span data-ttu-id="1a8a2-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.aravo.com/aems/login.do`</span><span class="sxs-lookup"><span data-stu-id="1a8a2-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.aravo.com/aems/login.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1a8a2-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-161">These values are not real.</span></span> <span data-ttu-id="1a8a2-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="1a8a2-163">Ügyfél [Aravo támogatási csoport](http://www.aravo.com/about-us/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-163">Contact [Aravo support team](http://www.aravo.com/about-us/contact/) to get these values.</span></span>
 
4. <span data-ttu-id="1a8a2-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_certificate.png) 

5. <span data-ttu-id="1a8a2-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1a8a2-168">A a **Aravo konfigurációs** kattintson **konfigurálása Aravo** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-168">On the **Aravo Configuration** section, click **Configure Aravo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1a8a2-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1a8a2-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_configure.png) 

7. <span data-ttu-id="1a8a2-171">Egyszeri bejelentkezés konfigurálása **Aravo** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Aravo támogatási csoport](http://www.aravo.com/about-us/contact/).</span><span class="sxs-lookup"><span data-stu-id="1a8a2-171">To configure single sign-on on **Aravo** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Aravo support team](http://www.aravo.com/about-us/contact/).</span></span> 


> [!TIP]
> <span data-ttu-id="1a8a2-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1a8a2-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1a8a2-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1a8a2-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a8a2-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1a8a2-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="1a8a2-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1a8a2-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a8a2-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1a8a2-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1a8a2-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1a8a2-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1a8a2-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1a8a2-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-aravo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1a8a2-187">a.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-187">a.</span></span> <span data-ttu-id="1a8a2-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a8a2-189">b.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-189">b.</span></span> <span data-ttu-id="1a8a2-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1a8a2-191">c.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-191">c.</span></span> <span data-ttu-id="1a8a2-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1a8a2-193">d.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-193">d.</span></span> <span data-ttu-id="1a8a2-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-194">Click **Create**.</span></span>
 
### <a name="creating-an-aravo-test-user"></a><span data-ttu-id="1a8a2-195">Egy Aravo tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-195">Creating an Aravo test user</span></span>

<span data-ttu-id="1a8a2-196">Ez a szakasz célja Aravo Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-196">The objective of this section is to create a user called Britta Simon in Aravo.</span></span> <span data-ttu-id="1a8a2-197">Együttműködve [Aravo támogatási csoport](http://www.aravo.com/about-us/contact/) a felhasználók hozzáadása a Aravo fiók.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-197">Work with [Aravo support team](http://www.aravo.com/about-us/contact/) to add the users in the Aravo account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1a8a2-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1a8a2-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1a8a2-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Aravo Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Aravo.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1a8a2-201">**Britta Simon hozzárendelése Aravo, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1a8a2-201">**To assign Britta Simon to Aravo, perform the following steps:**</span></span>

1. <span data-ttu-id="1a8a2-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1a8a2-204">Az alkalmazások listában válassza ki a **Aravo**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-204">In the applications list, select **Aravo**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_app.png) 

3. <span data-ttu-id="1a8a2-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1a8a2-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-208">Click **Add** button.</span></span> <span data-ttu-id="1a8a2-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1a8a2-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1a8a2-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a8a2-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1a8a2-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a8a2-214">Testing single sign-on</span></span>

<span data-ttu-id="1a8a2-215">Ez a szakasz célja a Microsoft Azure AD az egyszeri bejelentkezés konfiguráció teszteléséhez a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-215">The objective of this section is to test your Microsoft Azure AD Single Sign-On configuration using the Access Panel.</span></span>

<span data-ttu-id="1a8a2-216">Ha a hozzáférési panelen Aravo csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Aravo alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1a8a2-216">When you click the Aravo tile in the Access Panel, you should get automatically signed-on to your Aravo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a8a2-217">További források</span><span class="sxs-lookup"><span data-stu-id="1a8a2-217">Additional resources</span></span>

* [<span data-ttu-id="1a8a2-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1a8a2-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a8a2-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1a8a2-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_203.png

