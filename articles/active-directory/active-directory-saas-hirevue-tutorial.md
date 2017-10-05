---
title: "Oktatóanyag: Azure Active Directoryval integrált HireVue |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és HireVue között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aadfc342-14db-4d74-a83d-f0c76f0cf63c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 682d88d3010f5781948a9adde0e1351471608a5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a><span data-ttu-id="70c93-103">Oktatóanyag: Azure Active Directoryval integrált HireVue</span><span class="sxs-lookup"><span data-stu-id="70c93-103">Tutorial: Azure Active Directory integration with HireVue</span></span>

<span data-ttu-id="70c93-104">Ebben az oktatóanyagban elsajátíthatja HireVue integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="70c93-104">In this tutorial, you learn how to integrate HireVue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70c93-105">HireVue integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="70c93-105">Integrating HireVue with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="70c93-106">Megadhatja a HireVue hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="70c93-106">You can control in Azure AD who has access to HireVue</span></span>
- <span data-ttu-id="70c93-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett HireVue (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="70c93-107">You can enable your users to automatically get signed-on to HireVue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70c93-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="70c93-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="70c93-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70c93-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70c93-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="70c93-110">Prerequisites</span></span>

<span data-ttu-id="70c93-111">Konfigurálása az Azure AD-integrációs HireVue, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="70c93-111">To configure Azure AD integration with HireVue, you need the following items:</span></span>

- <span data-ttu-id="70c93-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="70c93-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70c93-113">Egy HireVue egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="70c93-113">A HireVue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70c93-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="70c93-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70c93-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="70c93-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70c93-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="70c93-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70c93-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70c93-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70c93-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="70c93-118">Scenario description</span></span>
<span data-ttu-id="70c93-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="70c93-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70c93-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="70c93-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70c93-121">A gyűjteményből HireVue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="70c93-121">Adding HireVue from the gallery</span></span>
2. <span data-ttu-id="70c93-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="70c93-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hirevue-from-the-gallery"></a><span data-ttu-id="70c93-123">A gyűjteményből HireVue hozzáadása</span><span class="sxs-lookup"><span data-stu-id="70c93-123">Adding HireVue from the gallery</span></span>
<span data-ttu-id="70c93-124">Az Azure AD integrálása a HireVue konfigurálásához kell hozzáadnia HireVue a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="70c93-124">To configure the integration of HireVue into Azure AD, you need to add HireVue from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="70c93-125">**A gyűjteményből HireVue hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="70c93-125">**To add HireVue from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="70c93-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="70c93-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70c93-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="70c93-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="70c93-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="70c93-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="70c93-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="70c93-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="70c93-133">Írja be a keresőmezőbe, **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="70c93-133">In the search box, type **HireVue**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_search.png)

5. <span data-ttu-id="70c93-135">Az eredmények panelen válassza ki a **HireVue**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="70c93-135">In the results panel, select **HireVue**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70c93-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="70c93-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70c93-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján HireVue.</span><span class="sxs-lookup"><span data-stu-id="70c93-138">In this section, you configure and test Azure AD single sign-on with HireVue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="70c93-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó HireVue a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="70c93-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HireVue is to a user in Azure AD.</span></span> <span data-ttu-id="70c93-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a HireVue közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="70c93-140">In other words, a link relationship between an Azure AD user and the related user in HireVue needs to be established.</span></span>

<span data-ttu-id="70c93-141">HireVue, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="70c93-141">In HireVue, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="70c93-142">Az Azure AD egyszeri bejelentkezést a HireVue tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="70c93-142">To configure and test Azure AD single sign-on with HireVue, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="70c93-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="70c93-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="70c93-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="70c93-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70c93-145">**[HireVue tesztfelhasználó létrehozása](#creating-a-hirevue-test-user)**  - való Britta Simon valami HireVue, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="70c93-145">**[Creating a HireVue test user](#creating-a-hirevue-test-user)** - to have a counterpart of Britta Simon in HireVue that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="70c93-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="70c93-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70c93-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="70c93-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70c93-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="70c93-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70c93-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az HireVue alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="70c93-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HireVue application.</span></span>

<span data-ttu-id="70c93-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés HireVue, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="70c93-150">**To configure Azure AD single sign-on with HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="70c93-151">Az Azure portálon a a **HireVue** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="70c93-151">In the Azure portal, on the **HireVue** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="70c93-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="70c93-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_samlbase.png)

3. <span data-ttu-id="70c93-155">Az a **HireVue tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="70c93-155">On the **HireVue Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_url.png)

    <span data-ttu-id="70c93-157">a.</span><span class="sxs-lookup"><span data-stu-id="70c93-157">a.</span></span> <span data-ttu-id="70c93-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="70c93-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>

    | <span data-ttu-id="70c93-159">Környezet</span><span class="sxs-lookup"><span data-stu-id="70c93-159">Environment</span></span> | <span data-ttu-id="70c93-160">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="70c93-160">URL</span></span> |
    |-------------|---|
    | <span data-ttu-id="70c93-161">Éles</span><span class="sxs-lookup"><span data-stu-id="70c93-161">Production</span></span> | `https://<companyname>.hirevue.com` |
    | <span data-ttu-id="70c93-162">Átmeneti</span><span class="sxs-lookup"><span data-stu-id="70c93-162">Staging</span></span>    | `https://<companyname>.stghv.com` |
    
    <span data-ttu-id="70c93-163">b.</span><span class="sxs-lookup"><span data-stu-id="70c93-163">b.</span></span> <span data-ttu-id="70c93-164">Az a **azonosító** szövegmező, adja meg az URL-címet:</span><span class="sxs-lookup"><span data-stu-id="70c93-164">In the **Identifier** textbox, type a URL as:</span></span>
    
    | <span data-ttu-id="70c93-165">Környezet</span><span class="sxs-lookup"><span data-stu-id="70c93-165">Environment</span></span> | <span data-ttu-id="70c93-166">URN</span><span class="sxs-lookup"><span data-stu-id="70c93-166">URN</span></span> |
    |-------------|-----|
    | <span data-ttu-id="70c93-167">Éles</span><span class="sxs-lookup"><span data-stu-id="70c93-167">Production</span></span> |`urn:federation:hirevue.com:saml:sp:prod` |
    | <span data-ttu-id="70c93-168">Átmeneti</span><span class="sxs-lookup"><span data-stu-id="70c93-168">Staging</span></span>    | `urn:federation:hirevue.com:saml:sp:staging`|
    
    > [!NOTE] 
    > <span data-ttu-id="70c93-169">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="70c93-169">These values are not real.</span></span> <span data-ttu-id="70c93-170">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="70c93-170">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="70c93-171">Ügyfél [HireVue ügyfél-támogatási csoport](mailto:samlsupport@hirevue.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="70c93-171">Contact [HireVue Client support team](mailto:samlsupport@hirevue.com) to get these values.</span></span> 
 
4. <span data-ttu-id="70c93-172">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="70c93-172">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_certificate.png) 

5. <span data-ttu-id="70c93-174">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="70c93-174">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70c93-176">A a **HireVue konfigurációs** kattintson **konfigurálása HireVue** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="70c93-176">On the **HireVue Configuration** section, click **Configure HireVue** to open **Configure sign-on** window.</span></span> <span data-ttu-id="70c93-177">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="70c93-177">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_configure.png) 

7. <span data-ttu-id="70c93-179">Egyszeri bejelentkezés konfigurálása **HireVue** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**való [HireVue támogatási csoport](mailto:samlsupport@hirevue.com).</span><span class="sxs-lookup"><span data-stu-id="70c93-179">To configure single sign-on on **HireVue** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [HireVue support team](mailto:samlsupport@hirevue.com).</span></span> <span data-ttu-id="70c93-180">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="70c93-180">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="70c93-181">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="70c93-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="70c93-182">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="70c93-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="70c93-183">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70c93-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70c93-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="70c93-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="70c93-185">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="70c93-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="70c93-187">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="70c93-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="70c93-188">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="70c93-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70c93-190">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="70c93-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70c93-192">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="70c93-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70c93-194">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="70c93-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70c93-196">a.</span><span class="sxs-lookup"><span data-stu-id="70c93-196">a.</span></span> <span data-ttu-id="70c93-197">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70c93-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70c93-198">b.</span><span class="sxs-lookup"><span data-stu-id="70c93-198">b.</span></span> <span data-ttu-id="70c93-199">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="70c93-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="70c93-200">c.</span><span class="sxs-lookup"><span data-stu-id="70c93-200">c.</span></span> <span data-ttu-id="70c93-201">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="70c93-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="70c93-202">d.</span><span class="sxs-lookup"><span data-stu-id="70c93-202">d.</span></span> <span data-ttu-id="70c93-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="70c93-203">Click **Create**.</span></span>
 
### <a name="creating-a-hirevue-test-user"></a><span data-ttu-id="70c93-204">HireVue tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="70c93-204">Creating a HireVue test user</span></span>

<span data-ttu-id="70c93-205">Ebben a szakaszban egy HireVue Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="70c93-205">In this section, you create a user called Britta Simon in HireVue.</span></span> <span data-ttu-id="70c93-206">Adjon együttműködve [HireVue támogatási csoport](mailto:samlsupport@hirevue.com) a felhasználók hozzáadása a HireVue platform.</span><span class="sxs-lookup"><span data-stu-id="70c93-206">Please work with [HireVue support team](mailto:samlsupport@hirevue.com) to add the users in the HireVue platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="70c93-207">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="70c93-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="70c93-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés HireVue Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="70c93-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HireVue.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="70c93-210">**Britta Simon hozzárendelése HireVue, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="70c93-210">**To assign Britta Simon to HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="70c93-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="70c93-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="70c93-213">Az alkalmazások listában válassza ki a **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="70c93-213">In the applications list, select **HireVue**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_app.png) 

3. <span data-ttu-id="70c93-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="70c93-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="70c93-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="70c93-217">Click **Add** button.</span></span> <span data-ttu-id="70c93-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="70c93-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="70c93-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="70c93-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="70c93-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="70c93-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70c93-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="70c93-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70c93-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="70c93-223">Testing single sign-on</span></span>

<span data-ttu-id="70c93-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="70c93-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="70c93-225">Ha a hozzáférési panelen HireVue csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az HireVue alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="70c93-225">When you click the HireVue tile in the Access Panel, you should get automatically signed-on to your HireVue application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70c93-226">További források</span><span class="sxs-lookup"><span data-stu-id="70c93-226">Additional resources</span></span>

* [<span data-ttu-id="70c93-227">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="70c93-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70c93-228">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="70c93-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png

