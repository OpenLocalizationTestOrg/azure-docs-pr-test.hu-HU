---
title: "Oktatóanyag: Azure Active Directoryval integrált hitelesítésszolgáltató PPM |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a hitelesítésszolgáltató PPM között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 4ca9268c26f681fcc96955b6161fe4a119b2dcf4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="0a1a4-103">Oktatóanyag: Azure Active Directoryval integrált hitelesítésszolgáltató PPM</span><span class="sxs-lookup"><span data-stu-id="0a1a4-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="0a1a4-104">Ebben az oktatóanyagban elsajátíthatja hitelesítésszolgáltató PPM integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0a1a4-104">In this tutorial, you learn how to integrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a1a4-105">Hitelesítésszolgáltató PPM integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-105">Integrating CA PPM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0a1a4-106">Szabályozhatja, aki hozzáfér a hitelesítésszolgáltató PPM Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0a1a4-106">You can control in Azure AD who has access to CA PPM</span></span>
- <span data-ttu-id="0a1a4-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a hitelesítésszolgáltató PPM (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="0a1a4-107">You can enable your users to automatically get signed-on to CA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a1a4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0a1a4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0a1a4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a1a4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a1a4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a1a4-110">Prerequisites</span></span>

<span data-ttu-id="0a1a4-111">Az Azure AD-integráció konfigurálása a hitelesítésszolgáltató PPM, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-111">To configure Azure AD integration with CA PPM, you need the following items:</span></span>

- <span data-ttu-id="0a1a4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0a1a4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0a1a4-113">A hitelesítésszolgáltató PPM egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0a1a4-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a1a4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a1a4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a1a4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a1a4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a1a4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a1a4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0a1a4-118">Scenario description</span></span>
<span data-ttu-id="0a1a4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a1a4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a1a4-121">Hitelesítésszolgáltató PPM hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0a1a4-121">Adding CA PPM from the gallery</span></span>
2. <span data-ttu-id="0a1a4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a1a4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-the-gallery"></a><span data-ttu-id="0a1a4-123">Hitelesítésszolgáltató PPM hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0a1a4-123">Adding CA PPM from the gallery</span></span>
<span data-ttu-id="0a1a4-124">Az Azure AD integrálása a hitelesítésszolgáltató PPM konfigurálásához kell hozzáadnia hitelesítésszolgáltató PPM a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-124">To configure the integration of CA PPM into Azure AD, you need to add CA PPM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0a1a4-125">**Adja hozzá a hitelesítésszolgáltató PPM a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a1a4-125">**To add CA PPM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0a1a4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a1a4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0a1a4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0a1a4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0a1a4-133">Írja be a keresőmezőbe, **hitelesítésszolgáltató PPM**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-133">In the search box, type **CA PPM**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="0a1a4-135">Az eredmények panelen válassza ki a **hitelesítésszolgáltató PPM**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-135">In the results panel, select **CA PPM**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a1a4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a1a4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a1a4-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a hitelesítésszolgáltató PPM "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="0a1a4-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0a1a4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a hitelesítésszolgáltató PPM tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CA PPM is to a user in Azure AD.</span></span> <span data-ttu-id="0a1a4-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a CA PPM közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-140">In other words, a link relationship between an Azure AD user and the related user in CA PPM needs to be established.</span></span>

<span data-ttu-id="0a1a4-141">A hitelesítésszolgáltató PPM, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-141">In CA PPM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0a1a4-142">Az Azure AD egyszeri bejelentkezést a hitelesítésszolgáltató PPM tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-142">To configure and test Azure AD single sign-on with CA PPM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0a1a4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0a1a4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a1a4-145">**[Hitelesítésszolgáltató PPM tesztfelhasználó létrehozása](#creating-a-ca-ppm-test-user)**  - való Britta Simon egy megfelelője a hitelesítésszolgáltató PPM, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - to have a counterpart of Britta Simon in CA PPM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a1a4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a1a4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a1a4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a1a4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a1a4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a hitelesítésszolgáltató PPM alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="0a1a4-150">**A hitelesítésszolgáltató PPM konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a1a4-150">**To configure Azure AD single sign-on with CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="0a1a4-151">Az Azure portálon a a **hitelesítésszolgáltató PPM** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-151">In the Azure portal, on the **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0a1a4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="0a1a4-155">Az a **hitelesítésszolgáltató PPM tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-155">On the **CA PPM Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="0a1a4-157">a.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-157">a.</span></span> <span data-ttu-id="0a1a4-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="0a1a4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="0a1a4-159">b.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-159">b.</span></span> <span data-ttu-id="0a1a4-160">Az a **válasz URL-CÍMEN** szövegmezőhöz típusú, mint:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="0a1a4-160">In the **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0a1a4-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-161">This value is not real.</span></span> <span data-ttu-id="0a1a4-162">Frissítse ezt az értéket a tényleges azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="0a1a4-163">Ügyfél [hitelesítésszolgáltató PPM támogatási csoport](mailto:catechnicalsupport@ca.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) to get this value.</span></span>
 
4. <span data-ttu-id="0a1a4-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="0a1a4-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a1a4-168">Az a **hitelesítésszolgáltató PPM konfigurációjának** területen kattintson **konfigurálása hitelesítésszolgáltató PPM** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-168">On the **CA PPM Configuration** section, click **Configure CA PPM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0a1a4-169">Másolás a **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0a1a4-169">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="0a1a4-171">Egyszeri bejelentkezés konfigurálása **hitelesítésszolgáltató PPM** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **SAML Entitásazonosító** való [hitelesítésszolgáltató PPM támogatási csoport](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="0a1a4-171">To configure single sign-on on **CA PPM** side, you need to send the downloaded **Certificate(Base64)** and **SAML Entity ID** to [CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="0a1a4-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0a1a4-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0a1a4-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0a1a4-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a1a4-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a1a4-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a1a4-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a1a4-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0a1a4-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a1a4-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0a1a4-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0a1a4-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a1a4-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a1a4-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0a1a4-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a1a4-187">a.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-187">a.</span></span> <span data-ttu-id="0a1a4-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a1a4-189">b.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-189">b.</span></span> <span data-ttu-id="0a1a4-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a1a4-191">c.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-191">c.</span></span> <span data-ttu-id="0a1a4-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0a1a4-193">d.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-193">d.</span></span> <span data-ttu-id="0a1a4-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="0a1a4-195">Hitelesítésszolgáltató PPM tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a1a4-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="0a1a4-196">Ebben a szakaszban egy hitelesítésszolgáltató PPM Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="0a1a4-197">Együttműködve [hitelesítésszolgáltató PPM támogatási csoport](mailto:catechnicalsupport@ca.com) a felhasználók hozzáadása a CA PPM platform.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) to add the users in the CA PPM platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0a1a4-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0a1a4-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0a1a4-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés hitelesítésszolgáltató PPM Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CA PPM.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0a1a4-201">**Hitelesítésszolgáltató PPM Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a1a4-201">**To assign Britta Simon to CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="0a1a4-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0a1a4-204">Az alkalmazások listában válassza ki a **hitelesítésszolgáltató PPM**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-204">In the applications list, select **CA PPM**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="0a1a4-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0a1a4-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-208">Click **Add** button.</span></span> <span data-ttu-id="0a1a4-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0a1a4-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0a1a4-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a1a4-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a1a4-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0a1a4-214">Testing single sign-on</span></span>

<span data-ttu-id="0a1a4-215">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-215">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="0a1a4-216">Ha a hitelesítésszolgáltató PPM csempe a hozzáférési panelen gombra kattint, meg kell beolvasása automatikusan bejelentkezett a hitelesítésszolgáltató PPM alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="0a1a4-216">When you click the CA PPM tile in the Access Panel, you should get automatically signed-on to your CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a1a4-217">További források</span><span class="sxs-lookup"><span data-stu-id="0a1a4-217">Additional resources</span></span>

* [<span data-ttu-id="0a1a4-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0a1a4-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a1a4-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0a1a4-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

