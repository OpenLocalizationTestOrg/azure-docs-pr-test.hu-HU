---
title: "Oktatóanyag: Azure Active Directoryval integrált Intacct |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Intacct között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c203b192b9da0d280cbd7f6c123219242ee4a3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="848f6-103">Oktatóanyag: Azure Active Directoryval integrált Intacct</span><span class="sxs-lookup"><span data-stu-id="848f6-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="848f6-104">Ebben az oktatóanyagban elsajátíthatja Intacct integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="848f6-104">In this tutorial, you learn how to integrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="848f6-105">Intacct integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="848f6-105">Integrating Intacct with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="848f6-106">Megadhatja a Intacct hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="848f6-106">You can control in Azure AD who has access to Intacct</span></span>
- <span data-ttu-id="848f6-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Intacct (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="848f6-107">You can enable your users to automatically get signed-on to Intacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="848f6-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="848f6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="848f6-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="848f6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="848f6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="848f6-110">Prerequisites</span></span>

<span data-ttu-id="848f6-111">Konfigurálása az Azure AD-integrációs Intacct, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="848f6-111">To configure Azure AD integration with Intacct, you need the following items:</span></span>

- <span data-ttu-id="848f6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="848f6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="848f6-113">Egy Intacct egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="848f6-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="848f6-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="848f6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="848f6-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="848f6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="848f6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="848f6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="848f6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="848f6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="848f6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="848f6-118">Scenario description</span></span>
<span data-ttu-id="848f6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="848f6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="848f6-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="848f6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="848f6-121">A gyűjteményből Intacct hozzáadása</span><span class="sxs-lookup"><span data-stu-id="848f6-121">Adding Intacct from the gallery</span></span>
2. <span data-ttu-id="848f6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="848f6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-the-gallery"></a><span data-ttu-id="848f6-123">A gyűjteményből Intacct hozzáadása</span><span class="sxs-lookup"><span data-stu-id="848f6-123">Adding Intacct from the gallery</span></span>
<span data-ttu-id="848f6-124">Az Azure AD integrálása a Intacct konfigurálásához kell hozzáadnia Intacct a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="848f6-124">To configure the integration of Intacct into Azure AD, you need to add Intacct from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="848f6-125">**A gyűjteményből Intacct hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="848f6-125">**To add Intacct from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="848f6-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="848f6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="848f6-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="848f6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="848f6-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="848f6-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="848f6-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="848f6-133">Írja be a keresőmezőbe, **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="848f6-133">In the search box, type **Intacct**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="848f6-135">Az eredmények panelen válassza ki a **Intacct**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="848f6-135">In the results panel, select **Intacct**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="848f6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="848f6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="848f6-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Intacct.</span><span class="sxs-lookup"><span data-stu-id="848f6-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="848f6-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Intacct a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="848f6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intacct is to a user in Azure AD.</span></span> <span data-ttu-id="848f6-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Intacct közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="848f6-140">In other words, a link relationship between an Azure AD user and the related user in Intacct needs to be established.</span></span>

<span data-ttu-id="848f6-141">Intacct, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="848f6-141">In Intacct, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="848f6-142">Az Azure AD egyszeri bejelentkezést a Intacct tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="848f6-142">To configure and test Azure AD single sign-on with Intacct, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="848f6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="848f6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="848f6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="848f6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="848f6-145">**[Egy Intacct tesztfelhasználó létrehozása](#creating-an-intacct-test-user)**  - való Britta Simon valami Intacct, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="848f6-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - to have a counterpart of Britta Simon in Intacct that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="848f6-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="848f6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="848f6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="848f6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="848f6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="848f6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="848f6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Intacct alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="848f6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="848f6-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Intacct, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="848f6-150">**To configure Azure AD single sign-on with Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="848f6-151">Az Azure portálon a a **Intacct** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="848f6-151">In the Azure portal, on the **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="848f6-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="848f6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="848f6-155">Az a **Intacct tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="848f6-155">On the **Intacct Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="848f6-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="848f6-157">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="848f6-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="848f6-158">This value is not real.</span></span> <span data-ttu-id="848f6-159">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="848f6-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="848f6-160">Ügyfél [Intacct támogatási csoport](https://us.intacct.com/support) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="848f6-160">Contact [Intacct support team](https://us.intacct.com/support) to get this value.</span></span>

4. <span data-ttu-id="848f6-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="848f6-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="848f6-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="848f6-165">A a **Intacct konfigurációs** kattintson **konfigurálása Intacct** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="848f6-165">On the **Intacct Configuration** section, click **Configure Intacct** to open **Configure sign-on** window.</span></span> <span data-ttu-id="848f6-166">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="848f6-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="848f6-168">Egy másik webes böngészőablakban jelentkezzen be a Intacct vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="848f6-168">In a different web browser window, sign in to your Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="848f6-169">Kattintson a **vállalati** fülre, majd **vállalati adatok**.</span><span class="sxs-lookup"><span data-stu-id="848f6-169">Click the **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="848f6-170">![Vállalati](./media/active-directory-saas-intacct-tutorial/ic790037.png "vállalati")</span><span class="sxs-lookup"><span data-stu-id="848f6-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="848f6-171">Kattintson a **biztonsági** fülre, majd **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="848f6-171">Click the **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="848f6-172">![Biztonsági](./media/active-directory-saas-intacct-tutorial/ic790038.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="848f6-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="848f6-173">Az a **egyszeri bejelentkezés (SSO)** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="848f6-173">In the **Single sign on (SSO)** section, perform the following steps:</span></span>

    <span data-ttu-id="848f6-174">![Egyszeri bejelentkezés](./media/active-directory-saas-intacct-tutorial/ic790039.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="848f6-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="848f6-175">a.</span><span class="sxs-lookup"><span data-stu-id="848f6-175">a.</span></span> <span data-ttu-id="848f6-176">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="848f6-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="848f6-177">b.</span><span class="sxs-lookup"><span data-stu-id="848f6-177">b.</span></span> <span data-ttu-id="848f6-178">Mint **identitás szolgáltatótípus**, jelölje be **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="848f6-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="848f6-179">c.</span><span class="sxs-lookup"><span data-stu-id="848f6-179">c.</span></span> <span data-ttu-id="848f6-180">A **kiállítójának URL-címe** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="848f6-180">In **Issuer URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="848f6-181">d.</span><span class="sxs-lookup"><span data-stu-id="848f6-181">d.</span></span> <span data-ttu-id="848f6-182">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="848f6-182">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="848f6-183">e.</span><span class="sxs-lookup"><span data-stu-id="848f6-183">e.</span></span> <span data-ttu-id="848f6-184">Nyissa meg a **base-64** kódolású Jegyzettömb-tanúsítványt, a tartalmának másolása a vágólapra és illessze be azt a **tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="848f6-184">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** box.</span></span>
   
    <span data-ttu-id="848f6-185">f.</span><span class="sxs-lookup"><span data-stu-id="848f6-185">f.</span></span> <span data-ttu-id="848f6-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="848f6-187">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="848f6-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="848f6-188">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="848f6-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="848f6-189">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="848f6-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="848f6-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="848f6-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="848f6-191">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="848f6-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="848f6-193">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="848f6-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="848f6-194">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="848f6-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="848f6-196">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="848f6-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="848f6-198">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="848f6-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="848f6-200">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="848f6-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="848f6-202">a.</span><span class="sxs-lookup"><span data-stu-id="848f6-202">a.</span></span> <span data-ttu-id="848f6-203">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="848f6-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="848f6-204">b.</span><span class="sxs-lookup"><span data-stu-id="848f6-204">b.</span></span> <span data-ttu-id="848f6-205">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="848f6-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="848f6-206">c.</span><span class="sxs-lookup"><span data-stu-id="848f6-206">c.</span></span> <span data-ttu-id="848f6-207">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="848f6-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="848f6-208">d.</span><span class="sxs-lookup"><span data-stu-id="848f6-208">d.</span></span> <span data-ttu-id="848f6-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="848f6-210">Egy Intacct tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="848f6-210">Creating an Intacct test user</span></span>

<span data-ttu-id="848f6-211">Beállítása az Azure Active Directory-felhasználók, így Intacct való bejelentkezéshez, akkor ki kell építenie Intacct be.</span><span class="sxs-lookup"><span data-stu-id="848f6-211">To set up Azure AD users so they can sign in to Intacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="848f6-212">Intacct a kiépítés kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="848f6-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="848f6-213">**Felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="848f6-213">**To provision user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="848f6-214">Jelentkezzen be a **Intacct** bérlő.</span><span class="sxs-lookup"><span data-stu-id="848f6-214">Sign in to your **Intacct** tenant.</span></span>

2. <span data-ttu-id="848f6-215">Kattintson a **vállalati** fülre, majd **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="848f6-215">Click the **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="848f6-216">![Felhasználók](./media/active-directory-saas-intacct-tutorial/ic790041.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="848f6-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="848f6-217">Kattintson a **Hozzáadás** fülre.</span><span class="sxs-lookup"><span data-stu-id="848f6-217">Click the **Add** tab.</span></span>

    <span data-ttu-id="848f6-218">![Adja hozzá](./media/active-directory-saas-intacct-tutorial/ic790042.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="848f6-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="848f6-219">Az a **felhasználói adatok** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="848f6-219">In the **User Information** section, perform the following steps:</span></span>

    <span data-ttu-id="848f6-220">![Felhasználói adatok](./media/active-directory-saas-intacct-tutorial/ic790043.png "felhasználói adatok")</span><span class="sxs-lookup"><span data-stu-id="848f6-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="848f6-221">a.</span><span class="sxs-lookup"><span data-stu-id="848f6-221">a.</span></span> <span data-ttu-id="848f6-222">Adja meg a **felhasználói azonosító**, a **Vezetéknév**, **Utónév**, a **E-mail cím**, a **cím**, és a **Phone** kiépítése a kívánt Azure AD-fiókot, a **felhasználói adatok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="848f6-222">Enter the **User ID**, the **Last name**, **First name**, the **Email address**, the **Title**, and the **Phone** of an Azure AD account that you want to provision into the **User Information** section.</span></span>

    <span data-ttu-id="848f6-223">b.</span><span class="sxs-lookup"><span data-stu-id="848f6-223">b.</span></span> <span data-ttu-id="848f6-224">Válassza ki a **rendszergazda jogosultságokkal** a kiépítés kívánt Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="848f6-224">Select the **Admin privileges** of an Azure AD account that you want to provision.</span></span>
   
    <span data-ttu-id="848f6-225">c.</span><span class="sxs-lookup"><span data-stu-id="848f6-225">c.</span></span> <span data-ttu-id="848f6-226">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-226">Click **Save**.</span></span> <span data-ttu-id="848f6-227">Az Azure AD fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="848f6-227">The Azure AD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="848f6-228">Azure AD-felhasználói fiókok létrehozásához használhatja más Intacct felhasználói fiók létrehozása eszközök vagy Intacct által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="848f6-228">To provision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="848f6-229">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="848f6-229">Assigning the Azure AD test user</span></span>

<span data-ttu-id="848f6-230">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Intacct Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="848f6-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intacct.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="848f6-232">**Britta Simon hozzárendelése Intacct, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="848f6-232">**To assign Britta Simon to Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="848f6-233">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="848f6-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="848f6-235">Az alkalmazások listában válassza ki a **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="848f6-235">In the applications list, select **Intacct**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="848f6-237">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="848f6-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="848f6-239">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="848f6-239">Click **Add** button.</span></span> <span data-ttu-id="848f6-240">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="848f6-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="848f6-242">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="848f6-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="848f6-243">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="848f6-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="848f6-244">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="848f6-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="848f6-245">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="848f6-245">Testing single sign-on</span></span>

<span data-ttu-id="848f6-246">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="848f6-246">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="848f6-247">Ha a hozzáférési panelen Intacct csempére kattint, be kell automatikusan jelentkeznie az Intacct alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="848f6-247">When you click the Intacct tile in the Access Panel, you should be automatically signed in to your Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="848f6-248">További források</span><span class="sxs-lookup"><span data-stu-id="848f6-248">Additional resources</span></span>

* [<span data-ttu-id="848f6-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="848f6-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="848f6-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="848f6-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

