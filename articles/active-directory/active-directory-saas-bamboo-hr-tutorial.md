---
title: "Oktatóanyag: Azure Active Directoryval integrált BambooHR |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BambooHR között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: 06bf91b0e598fd3d8e644378efdb753611ee1ebc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="5b30c-103">Oktatóanyag: Azure Active Directoryval integrált BambooHR</span><span class="sxs-lookup"><span data-stu-id="5b30c-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="5b30c-104">Ebben az oktatóanyagban elsajátíthatja BambooHR integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5b30c-104">In this tutorial, you learn how to integrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5b30c-105">BambooHR integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5b30c-105">Integrating BambooHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5b30c-106">Megadhatja a BambooHR hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5b30c-106">You can control in Azure AD who has access to BambooHR</span></span>
- <span data-ttu-id="5b30c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BambooHR (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5b30c-107">You can enable your users to automatically get signed-on to BambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5b30c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5b30c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5b30c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5b30c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b30c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5b30c-110">Prerequisites</span></span>

<span data-ttu-id="5b30c-111">Konfigurálása az Azure AD-integrációs BambooHR, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5b30c-111">To configure Azure AD integration with BambooHR, you need the following items:</span></span>

- <span data-ttu-id="5b30c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5b30c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5b30c-113">Egy BambooHR egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="5b30c-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5b30c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5b30c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5b30c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5b30c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5b30c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5b30c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5b30c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5b30c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5b30c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5b30c-118">Scenario description</span></span>
<span data-ttu-id="5b30c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5b30c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5b30c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5b30c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5b30c-121">A gyűjteményből BambooHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5b30c-121">Adding BambooHR from the gallery</span></span>
2. <span data-ttu-id="5b30c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5b30c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-the-gallery"></a><span data-ttu-id="5b30c-123">A gyűjteményből BambooHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5b30c-123">Adding BambooHR from the gallery</span></span>
<span data-ttu-id="5b30c-124">Az Azure AD integrálása a BambooHR konfigurálásához kell hozzáadnia BambooHR a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5b30c-124">To configure the integration of BambooHR into Azure AD, you need to add BambooHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5b30c-125">**A gyűjteményből BambooHR hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5b30c-125">**To add BambooHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5b30c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5b30c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5b30c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5b30c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5b30c-133">Írja be a keresőmezőbe, **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-133">In the search box, type **BambooHR**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="5b30c-135">Az eredmények panelen válassza ki a **BambooHR**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5b30c-135">In the results panel, select **BambooHR**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5b30c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5b30c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5b30c-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BambooHR</span><span class="sxs-lookup"><span data-stu-id="5b30c-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5b30c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BambooHR a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="5b30c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BambooHR is to a user in Azure AD.</span></span> <span data-ttu-id="5b30c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BambooHR közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5b30c-140">In other words, a link relationship between an Azure AD user and the related user in BambooHR needs to be established.</span></span>

<span data-ttu-id="5b30c-141">BambooHR, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="5b30c-141">In BambooHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5b30c-142">Az Azure AD egyszeri bejelentkezést a BambooHR tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5b30c-142">To configure and test Azure AD single sign-on with BambooHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5b30c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5b30c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5b30c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5b30c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5b30c-145">**[BambooHR tesztfelhasználó létrehozása](#creating-a-bamboohr-test-user)**  - való Britta Simon valami BambooHR, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="5b30c-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - to have a counterpart of Britta Simon in BambooHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5b30c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5b30c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5b30c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5b30c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5b30c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5b30c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5b30c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BambooHR alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5b30c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="5b30c-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BambooHR, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5b30c-150">**To configure Azure AD single sign-on with BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="5b30c-151">Az Azure portálon a a **BambooHR** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-151">In the Azure portal, on the **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5b30c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5b30c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="5b30c-155">Az a **BambooHR tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5b30c-155">On the **BambooHR Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="5b30c-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="5b30c-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="5b30c-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="5b30c-158">This value is not real.</span></span> <span data-ttu-id="5b30c-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="5b30c-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="5b30c-160">Ügyfél [BambooHR ügyfél-támogatási csoport](https://www.bamboohr.com/contact.php) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="5b30c-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) to get this value.</span></span> 
 
4. <span data-ttu-id="5b30c-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5b30c-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="5b30c-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5b30c-165">A a **BambooHR konfigurációs** kattintson **konfigurálása BambooHR** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="5b30c-165">On the **BambooHR Configuration** section, click **Configure BambooHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5b30c-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="5b30c-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="5b30c-168">Egy másik webes böngészőablakban jelentkezzen be a BambooHR vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5b30c-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="5b30c-169">A kezdőlapon hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5b30c-169">On the homepage, perform the following steps:</span></span>
   
    <span data-ttu-id="5b30c-170">![Egyszeri bejelentkezés](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="5b30c-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="5b30c-171">a.</span><span class="sxs-lookup"><span data-stu-id="5b30c-171">a.</span></span> <span data-ttu-id="5b30c-172">Kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="5b30c-173">b.</span><span class="sxs-lookup"><span data-stu-id="5b30c-173">b.</span></span> <span data-ttu-id="5b30c-174">Kattintson a bal oldali alkalmazások menüben **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-174">In the apps menu on the left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="5b30c-175">c.</span><span class="sxs-lookup"><span data-stu-id="5b30c-175">c.</span></span> <span data-ttu-id="5b30c-176">Kattintson a **SAML-alapú egyszeri bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="5b30c-177">Az a **SAML-alapú egyszeri bejelentkezést** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="5b30c-177">In the **SAML Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="5b30c-178">![SAML-alapú egyszeri bejelentkezést](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML-alapú egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="5b30c-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="5b30c-179">a.</span><span class="sxs-lookup"><span data-stu-id="5b30c-179">a.</span></span> <span data-ttu-id="5b30c-180">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** be értéket a **Egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="5b30c-180">Paste the **SAML Single Sign-On Service URL** value into the **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="5b30c-181">b.</span><span class="sxs-lookup"><span data-stu-id="5b30c-181">b.</span></span> <span data-ttu-id="5b30c-182">Nyissa meg a Jegyzettömbben az Azure portálról letöltött base-64 kódolású tanúsítvány, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="5b30c-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="5b30c-183">c.</span><span class="sxs-lookup"><span data-stu-id="5b30c-183">c.</span></span> <span data-ttu-id="5b30c-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="5b30c-185">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="5b30c-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5b30c-186">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="5b30c-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5b30c-187">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5b30c-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5b30c-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b30c-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="5b30c-189">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="5b30c-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5b30c-191">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5b30c-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5b30c-192">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5b30c-194">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5b30c-196">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="5b30c-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5b30c-198">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5b30c-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5b30c-200">a.</span><span class="sxs-lookup"><span data-stu-id="5b30c-200">a.</span></span> <span data-ttu-id="5b30c-201">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5b30c-202">b.</span><span class="sxs-lookup"><span data-stu-id="5b30c-202">b.</span></span> <span data-ttu-id="5b30c-203">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5b30c-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5b30c-204">c.</span><span class="sxs-lookup"><span data-stu-id="5b30c-204">c.</span></span> <span data-ttu-id="5b30c-205">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5b30c-206">d.</span><span class="sxs-lookup"><span data-stu-id="5b30c-206">d.</span></span> <span data-ttu-id="5b30c-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="5b30c-208">BambooHR tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b30c-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="5b30c-209">Ahhoz, hogy az Azure AD-felhasználók BambooHR bejelentkezni, akkor ki kell építenie a BambooHR.</span><span class="sxs-lookup"><span data-stu-id="5b30c-209">To enable Azure AD users to log in to BambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="5b30c-210">BambooHR, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5b30c-210">In the case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="5b30c-211">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5b30c-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="5b30c-212">Jelentkezzen be a **BambooHR** hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5b30c-212">Log in to your **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="5b30c-213">A felső eszköztáron kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-213">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="5b30c-214">![Beállítás](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "beállítás")</span><span class="sxs-lookup"><span data-stu-id="5b30c-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="5b30c-215">Kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-215">Click **Overview**.</span></span>

4. <span data-ttu-id="5b30c-216">A bal oldali ablaktáblában lépjen **biztonsági \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-216">In the left navigation pane, go to **Security \> Users**.</span></span>

5. <span data-ttu-id="5b30c-217">Írja be a felhasználónevet, jelszót és egy érvényes szeretné azokat a kapcsolódó szövegmezők rendelkezés AAD-fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="5b30c-217">Type the user name, password, and email address of a valid AAD account you want to provision into the related textboxes.</span></span>

6. <span data-ttu-id="5b30c-218">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="5b30c-219">Bármely más BambooHR felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz BambooHR által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="5b30c-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5b30c-220">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5b30c-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5b30c-221">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BambooHR Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="5b30c-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BambooHR.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5b30c-223">**Britta Simon hozzárendelése BambooHR, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5b30c-223">**To assign Britta Simon to BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="5b30c-224">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5b30c-226">Az alkalmazások listában válassza ki a **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-226">In the applications list, select **BambooHR**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="5b30c-228">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5b30c-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5b30c-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5b30c-230">Click **Add** button.</span></span> <span data-ttu-id="5b30c-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5b30c-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5b30c-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5b30c-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5b30c-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5b30c-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5b30c-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5b30c-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5b30c-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5b30c-236">Testing single sign-on</span></span>

<span data-ttu-id="5b30c-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5b30c-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5b30c-238">Ha a hozzáférési panelen BambooHR csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az BambooHR alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5b30c-238">When you click the BambooHR tile in the Access Panel, you should get automatically signed-on to your BambooHR application.</span></span>
<span data-ttu-id="5b30c-239">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5b30c-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5b30c-240">További források</span><span class="sxs-lookup"><span data-stu-id="5b30c-240">Additional resources</span></span>

* [<span data-ttu-id="5b30c-241">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5b30c-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5b30c-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5b30c-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

