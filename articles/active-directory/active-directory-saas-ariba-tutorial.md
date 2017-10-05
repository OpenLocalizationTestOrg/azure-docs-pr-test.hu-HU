---
title: "Oktatóanyag: Azure Active Directoryval integrált Ariba |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Ariba között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 45a8364c-55d1-4dc7-b079-9eb2a701842d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 214367847055ba38ee03a28d0afdcc58f68333cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ariba"></a><span data-ttu-id="d7c49-103">Oktatóanyag: Azure Active Directoryval integrált Ariba</span><span class="sxs-lookup"><span data-stu-id="d7c49-103">Tutorial: Azure Active Directory integration with Ariba</span></span>

<span data-ttu-id="d7c49-104">Ebben az oktatóanyagban elsajátíthatja Ariba integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d7c49-104">In this tutorial, you learn how to integrate Ariba with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d7c49-105">Ariba integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d7c49-105">Integrating Ariba with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d7c49-106">Megadhatja a Ariba hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d7c49-106">You can control in Azure AD who has access to Ariba</span></span>
- <span data-ttu-id="d7c49-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Ariba (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d7c49-107">You can enable your users to automatically get signed-on to Ariba (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d7c49-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d7c49-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d7c49-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d7c49-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7c49-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d7c49-110">Prerequisites</span></span>

<span data-ttu-id="d7c49-111">Konfigurálása az Azure AD-integrációs Ariba, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d7c49-111">To configure Azure AD integration with Ariba, you need the following items:</span></span>

- <span data-ttu-id="d7c49-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d7c49-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d7c49-113">Egy Ariba egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="d7c49-113">An Ariba single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d7c49-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d7c49-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d7c49-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d7c49-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d7c49-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7c49-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d7c49-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d7c49-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d7c49-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d7c49-118">Scenario description</span></span>
<span data-ttu-id="d7c49-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d7c49-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d7c49-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d7c49-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d7c49-121">A gyűjteményből Ariba hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7c49-121">Adding Ariba from the gallery</span></span>
2. <span data-ttu-id="d7c49-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d7c49-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ariba-from-the-gallery"></a><span data-ttu-id="d7c49-123">A gyűjteményből Ariba hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7c49-123">Adding Ariba from the gallery</span></span>
<span data-ttu-id="d7c49-124">Az Azure AD integrálása a Ariba konfigurálásához kell hozzáadnia Ariba a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d7c49-124">To configure the integration of Ariba into Azure AD, you need to add Ariba from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d7c49-125">**A gyűjteményből Ariba hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7c49-125">**To add Ariba from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d7c49-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d7c49-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d7c49-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d7c49-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d7c49-133">Írja be a keresőmezőbe, **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-133">In the search box, type **Ariba**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_search.png)

5. <span data-ttu-id="d7c49-135">Az eredmények panelen válassza ki a **Ariba**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7c49-135">In the results panel, select **Ariba**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d7c49-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d7c49-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d7c49-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Ariba</span><span class="sxs-lookup"><span data-stu-id="d7c49-138">In this section, you configure and test Azure AD single sign-on with Ariba based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d7c49-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Ariba a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d7c49-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Ariba is to a user in Azure AD.</span></span> <span data-ttu-id="d7c49-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Ariba közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d7c49-140">In other words, a link relationship between an Azure AD user and the related user in Ariba needs to be established.</span></span>

<span data-ttu-id="d7c49-141">Ariba, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d7c49-141">In Ariba, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d7c49-142">Az Azure AD egyszeri bejelentkezést a Ariba tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d7c49-142">To configure and test Azure AD single sign-on with Ariba, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d7c49-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d7c49-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d7c49-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d7c49-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d7c49-145">**[Egy Ariba tesztfelhasználó létrehozása](#creating-an-ariba-test-user)**  - való Britta Simon valami Ariba, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d7c49-145">**[Creating an Ariba test user](#creating-an-ariba-test-user)** - to have a counterpart of Britta Simon in Ariba that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d7c49-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d7c49-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d7c49-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d7c49-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d7c49-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7c49-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d7c49-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Ariba alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d7c49-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Ariba application.</span></span>

<span data-ttu-id="d7c49-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Ariba, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7c49-150">**To configure Azure AD single sign-on with Ariba, perform the following steps:**</span></span>

1. <span data-ttu-id="d7c49-151">Az Azure portálon a a **Ariba** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-151">In the Azure portal, on the **Ariba** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d7c49-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d7c49-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_samlbase.png)

3. <span data-ttu-id="d7c49-155">Az a **Ariba tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d7c49-155">On the **Ariba Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_url.png)

    <span data-ttu-id="d7c49-157">a.</span><span class="sxs-lookup"><span data-stu-id="d7c49-157">a.</span></span> <span data-ttu-id="d7c49-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-cím: `https://<subdomain>.sourcing.ariba.com` vagy`https://<subdomain>.supplier.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="d7c49-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sourcing.ariba.com` or `https://<subdomain>.supplier.ariba.com`</span></span>

    <span data-ttu-id="d7c49-159">b.</span><span class="sxs-lookup"><span data-stu-id="d7c49-159">b.</span></span> <span data-ttu-id="d7c49-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`http://<subdomain>.procurement-2.ariba.com`</span><span class="sxs-lookup"><span data-stu-id="d7c49-160">In the **Identifier** textbox, type a URL using the following pattern: `http://<subdomain>.procurement-2.ariba.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d7c49-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d7c49-161">These values are not real.</span></span> <span data-ttu-id="d7c49-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d7c49-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d7c49-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="d7c49-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="d7c49-164">Lépjen kapcsolatba a terméktámogatással Ariba ügyfél következő **1-866-218-2155** beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d7c49-164">Contact Ariba Client support team at **1-866-218-2155** to get these values.</span></span> 
 

4. <span data-ttu-id="d7c49-165">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7c49-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate  file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_certificate.png) 

5. <span data-ttu-id="d7c49-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ariba-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d7c49-169">Az alkalmazáshoz konfigurált SSO beszerzéséhez hívja Ariba támogatási csoport **1-866-218-2155** és azok lesz segítséget nyújt további való adja meg a letöltött **tanúsítvány (Base64)** fájlt.</span><span class="sxs-lookup"><span data-stu-id="d7c49-169">To get SSO configured for your application, call Ariba support team on **1-866-218-2155** and they'll assist you further on how to provide them the downloaded **Certificate (Base64)** file.</span></span>  
 
> [!TIP]
> <span data-ttu-id="d7c49-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d7c49-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d7c49-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d7c49-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d7c49-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d7c49-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d7c49-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7c49-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="d7c49-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d7c49-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d7c49-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7c49-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d7c49-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d7c49-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d7c49-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d7c49-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d7c49-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d7c49-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ariba-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d7c49-185">a.</span><span class="sxs-lookup"><span data-stu-id="d7c49-185">a.</span></span> <span data-ttu-id="d7c49-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d7c49-187">b.</span><span class="sxs-lookup"><span data-stu-id="d7c49-187">b.</span></span> <span data-ttu-id="d7c49-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d7c49-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d7c49-189">c.</span><span class="sxs-lookup"><span data-stu-id="d7c49-189">c.</span></span> <span data-ttu-id="d7c49-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d7c49-191">d.</span><span class="sxs-lookup"><span data-stu-id="d7c49-191">d.</span></span> <span data-ttu-id="d7c49-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-192">Click **Create**.</span></span>
 
### <a name="creating-an-ariba-test-user"></a><span data-ttu-id="d7c49-193">Egy Ariba tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7c49-193">Creating an Ariba test user</span></span>

<span data-ttu-id="d7c49-194">Ez a szakasz célja Ariba Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d7c49-194">The objective of this section is to create a user called Britta Simon in Ariba.</span></span> <span data-ttu-id="d7c49-195">Ariba támogatási csapatának együttműködve **1-866-218-2155** a felhasználók hozzáadása a Ariba rendszerben.</span><span class="sxs-lookup"><span data-stu-id="d7c49-195">Work with Ariba support team at **1-866-218-2155** to add the users in the Ariba system.</span></span> 

 >[!NOTE]
 ><span data-ttu-id="d7c49-196">Hozza létre a felhasználó manuálisan kell, ha szeretné-e a Ariba támogatási csoportjához, **1-866-218-2155**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-196">If you need to create a user manually, you need to contact the Ariba support team at **1-866-218-2155**.</span></span>
 >  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d7c49-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d7c49-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d7c49-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Ariba Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d7c49-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ariba.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d7c49-200">**Britta Simon hozzárendelése Ariba, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d7c49-200">**To assign Britta Simon to Ariba, perform the following steps:**</span></span>

1. <span data-ttu-id="d7c49-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d7c49-203">Az alkalmazások listában válassza ki a **Ariba**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-203">In the applications list, select **Ariba**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ariba-tutorial/tutorial_ariba_app.png) 

3. <span data-ttu-id="d7c49-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d7c49-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d7c49-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d7c49-207">Click **Add** button.</span></span> <span data-ttu-id="d7c49-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7c49-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d7c49-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d7c49-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d7c49-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7c49-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d7c49-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d7c49-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d7c49-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d7c49-213">Testing single sign-on</span></span>
<span data-ttu-id="d7c49-214">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d7c49-214">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="d7c49-215">Ha a hozzáférési panelen Ariba csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Ariba alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d7c49-215">When you click the Ariba tile in the Access Panel, you should get automatically signed-on to your Ariba application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d7c49-216">További források</span><span class="sxs-lookup"><span data-stu-id="d7c49-216">Additional resources</span></span>

* [<span data-ttu-id="d7c49-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d7c49-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d7c49-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d7c49-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ariba-tutorial/tutorial_general_203.png

