---
title: "Oktatóanyag: Azure Active Directoryval integrált Huddle |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Huddle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 59d4019545d39ec76bf401696338140f430630c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a><span data-ttu-id="e37f4-103">Oktatóanyag: Azure Active Directoryval integrált Huddle</span><span class="sxs-lookup"><span data-stu-id="e37f4-103">Tutorial: Azure Active Directory integration with Huddle</span></span>

<span data-ttu-id="e37f4-104">Ebben az oktatóanyagban elsajátíthatja Huddle integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e37f4-104">In this tutorial, you learn how to integrate Huddle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e37f4-105">Huddle integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e37f4-105">Integrating Huddle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e37f4-106">Megadhatja a Huddle hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e37f4-106">You can control in Azure AD who has access to Huddle</span></span>
- <span data-ttu-id="e37f4-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Huddle (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e37f4-107">You can enable your users to automatically get signed-on to Huddle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e37f4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e37f4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e37f4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e37f4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e37f4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e37f4-110">Prerequisites</span></span>

<span data-ttu-id="e37f4-111">Konfigurálása az Azure AD-integrációs Huddle, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="e37f4-111">To configure Azure AD integration with Huddle, you need the following items:</span></span>

- <span data-ttu-id="e37f4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e37f4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e37f4-113">Egy Huddle egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e37f4-113">A Huddle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e37f4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e37f4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e37f4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e37f4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e37f4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e37f4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e37f4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e37f4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e37f4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e37f4-118">Scenario description</span></span>

<span data-ttu-id="e37f4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e37f4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e37f4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e37f4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e37f4-121">A gyűjteményből Huddle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e37f4-121">Adding Huddle from the gallery</span></span>
2. <span data-ttu-id="e37f4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e37f4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-huddle-from-the-gallery"></a><span data-ttu-id="e37f4-123">A gyűjteményből Huddle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e37f4-123">Adding Huddle from the gallery</span></span>
<span data-ttu-id="e37f4-124">Az Azure AD-be Huddle integrálása konfigurálásához kell hozzáadnia Huddle a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e37f4-124">To configure the integration of Huddle into Azure AD, you need to add Huddle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e37f4-125">**A gyűjteményből Huddle hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e37f4-125">**To add Huddle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e37f4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e37f4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e37f4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e37f4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e37f4-133">Írja be a keresőmezőbe, **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-133">In the search box, type **Huddle**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. <span data-ttu-id="e37f4-135">Az eredmények panelen válassza ki a **Huddle**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e37f4-135">In the results panel, select **Huddle**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e37f4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e37f4-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="e37f4-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Huddle</span><span class="sxs-lookup"><span data-stu-id="e37f4-138">In this section, you configure and test Azure AD single sign-on with Huddle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e37f4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Huddle a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e37f4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Huddle is to a user in Azure AD.</span></span> <span data-ttu-id="e37f4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Huddle közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e37f4-140">In other words, a link relationship between an Azure AD user and the related user in Huddle needs to be established.</span></span>

<span data-ttu-id="e37f4-141">Huddle, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e37f4-141">In Huddle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e37f4-142">Az Azure AD egyszeri bejelentkezést a Huddle tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e37f4-142">To configure and test Azure AD single sign-on with Huddle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e37f4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e37f4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>

2. <span data-ttu-id="e37f4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e37f4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>

3. <span data-ttu-id="e37f4-145">**[Huddle tesztfelhasználó létrehozása](#creating-a-huddle-test-user)**  - való Britta Simon valami Huddle, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e37f4-145">**[Creating a Huddle test user](#creating-a-huddle-test-user)** - to have a counterpart of Britta Simon in Huddle that is linked to the Azure AD representation of user.</span></span>

4. <span data-ttu-id="e37f4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e37f4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>

5. <span data-ttu-id="e37f4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e37f4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e37f4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e37f4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e37f4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Huddle alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e37f4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Huddle application.</span></span>

<span data-ttu-id="e37f4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Huddle, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e37f4-150">**To configure Azure AD single sign-on with Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="e37f4-151">Az Azure portálon a a **Huddle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-151">In the Azure portal, on the **Huddle** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e37f4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e37f4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. <span data-ttu-id="e37f4-155">Az a **Huddle tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e37f4-155">On the **Huddle Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    <span data-ttu-id="e37f4-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`http://<company name>.huddle.com`</span><span class="sxs-lookup"><span data-stu-id="e37f4-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<company name>.huddle.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e37f4-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e37f4-158">This value is not real.</span></span> <span data-ttu-id="e37f4-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e37f4-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="e37f4-160">Ügyfél [Huddle ügyfél-támogatási csoport](https://huddle.zendesk.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="e37f4-160">Contact [Huddle Client support team](https://huddle.zendesk.com) to get this value.</span></span> 

4. <span data-ttu-id="e37f4-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e37f4-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. <span data-ttu-id="e37f4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e37f4-165">A a **Huddle konfigurációs** kattintson **konfigurálása Huddle** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e37f4-165">On the **Huddle Configuration** section, click **Configure Huddle** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e37f4-166">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e37f4-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. <span data-ttu-id="e37f4-168">Az egyszeri bejelentkezés Huddle oldalon megadásához kell elküldenie a letöltött **tanúsítvány**, **SAML-alapú egyszeri bejelentkezési URL-címe**, és **SAML Entitásazonosító** való [Huddle ügyfél-támogatási csoport](https://huddle.zendesk.com).</span><span class="sxs-lookup"><span data-stu-id="e37f4-168">To configure single sign-on on Huddle side, you need to send the downloaded  **Certificate**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Huddle Client support team](https://huddle.zendesk.com).</span></span> <span data-ttu-id="e37f4-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="e37f4-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>  
   
    >[!NOTE]
    > <span data-ttu-id="e37f4-170">Egyszeri bejelentkezés kell engedélyezni lehessen az Huddle támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="e37f4-170">Single sign-on needs to be enabled by the Huddle support team.</span></span> <span data-ttu-id="e37f4-171">Értesítést kap, a konfiguráció befejezése után.</span><span class="sxs-lookup"><span data-stu-id="e37f4-171">You get a notification when the configuration has been completed.</span></span> 
    > 

> [!TIP]
> <span data-ttu-id="e37f4-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e37f4-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e37f4-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e37f4-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e37f4-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e37f4-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
   
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e37f4-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e37f4-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="e37f4-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e37f4-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e37f4-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e37f4-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e37f4-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e37f4-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e37f4-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e37f4-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e37f4-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e37f4-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e37f4-187">a.</span><span class="sxs-lookup"><span data-stu-id="e37f4-187">a.</span></span> <span data-ttu-id="e37f4-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e37f4-189">b.</span><span class="sxs-lookup"><span data-stu-id="e37f4-189">b.</span></span> <span data-ttu-id="e37f4-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e37f4-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e37f4-191">c.</span><span class="sxs-lookup"><span data-stu-id="e37f4-191">c.</span></span> <span data-ttu-id="e37f4-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e37f4-193">d.</span><span class="sxs-lookup"><span data-stu-id="e37f4-193">d.</span></span> <span data-ttu-id="e37f4-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-194">Click **Create**.</span></span>
 
### <a name="creating-a-huddle-test-user"></a><span data-ttu-id="e37f4-195">Huddle tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e37f4-195">Creating a Huddle test user</span></span>

<span data-ttu-id="e37f4-196">Ahhoz, hogy az Azure AD-felhasználók Huddle bejelentkezni, akkor ki kell építenie a Huddle.</span><span class="sxs-lookup"><span data-stu-id="e37f4-196">To enable Azure AD users to log in to Huddle, they must be provisioned into Huddle.</span></span> <span data-ttu-id="e37f4-197">Huddle, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e37f4-197">In the case of Huddle, provisioning is a manual task.</span></span>

<span data-ttu-id="e37f4-198">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e37f4-198">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="e37f4-199">Jelentkezzen be a **Huddle** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e37f4-199">Log in to your **Huddle** company site as administrator.</span></span>
2. <span data-ttu-id="e37f4-200">Kattintson a **munkaterület**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-200">Click **Workspace**.</span></span>
3. <span data-ttu-id="e37f4-201">Kattintson a **személyek \> felkérése**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-201">Click **People \> Invite People**.</span></span>
   
   <span data-ttu-id="e37f4-202">![Személyek](./media/active-directory-saas-huddle-tutorial/IC787838.png "személyek")</span><span class="sxs-lookup"><span data-stu-id="e37f4-202">![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")</span></span>

4. <span data-ttu-id="e37f4-203">Az a **hozzon létre egy új meghívó** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e37f4-203">In the **Create a new invitation** section, perform the following steps:</span></span>
   
   <span data-ttu-id="e37f4-204">![Új meghívó](./media/active-directory-saas-huddle-tutorial/IC787839.png "új meghívó")</span><span class="sxs-lookup"><span data-stu-id="e37f4-204">![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")</span></span>
   
   <span data-ttu-id="e37f4-205">a.</span><span class="sxs-lookup"><span data-stu-id="e37f4-205">a.</span></span> <span data-ttu-id="e37f4-206">Az a **válassza ki a meghívott személyek csatlakozni csoport** listáról válassza ki **team**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-206">In the **Choose a team to invite people to join** list, select **team**.</span></span>

   <span data-ttu-id="e37f4-207">b.</span><span class="sxs-lookup"><span data-stu-id="e37f4-207">b.</span></span> <span data-ttu-id="e37f4-208">Típus a **E-mail cím** egy érvényes Azure ki kívánja építeni a az AD-fiókot **szeretné a meghívott személyek e-mail címet adjon meg** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e37f4-208">Type the **Email Address** of a valid Azure AD account you want to provision in to **Enter email address for people you'd like to invite** textbox.</span></span>

   <span data-ttu-id="e37f4-209">c.</span><span class="sxs-lookup"><span data-stu-id="e37f4-209">c.</span></span> <span data-ttu-id="e37f4-210">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-210">Click **Invite**.</span></span>   
   
    >[!NOTE]
    > <span data-ttu-id="e37f4-211">Az Azure AD fióktulajdonos kap egy e-mailt hivatkozással erősítse meg a fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="e37f4-211">The Azure AD account holder will receive an email including a link to confirm the account before it becomes active.</span></span> 
    > 

>[!NOTE]
><span data-ttu-id="e37f4-212">Huddle felhasználói fiók létrehozása eszközök vagy Huddle által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e37f4-212">You can use any other Huddle user account creation tools or APIs provided by Huddle to provision Azure AD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e37f4-213">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e37f4-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e37f4-214">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Huddle Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="e37f4-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Huddle.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e37f4-216">**Britta Simon hozzárendelése Huddle, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e37f4-216">**To assign Britta Simon to Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="e37f4-217">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e37f4-219">Az alkalmazások listában válassza ki a **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-219">In the applications list, select **Huddle**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. <span data-ttu-id="e37f4-221">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e37f4-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e37f4-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e37f4-223">Click **Add** button.</span></span> <span data-ttu-id="e37f4-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e37f4-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e37f4-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e37f4-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e37f4-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e37f4-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e37f4-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e37f4-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e37f4-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e37f4-229">Testing single sign-on</span></span>

<span data-ttu-id="e37f4-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e37f4-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e37f4-231">Ha a hozzáférési panelen Huddle csempére kattint, szerezheti be automatikusan Huddle alkalmazás bejelentkezési oldalán.</span><span class="sxs-lookup"><span data-stu-id="e37f4-231">When you click the Huddle tile in the Access Panel, you should get automatically login page of Huddle application.</span></span>
<span data-ttu-id="e37f4-232">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e37f4-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e37f4-233">További források</span><span class="sxs-lookup"><span data-stu-id="e37f4-233">Additional resources</span></span>

* [<span data-ttu-id="e37f4-234">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e37f4-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e37f4-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e37f4-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
