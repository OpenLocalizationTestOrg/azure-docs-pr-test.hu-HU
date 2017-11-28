---
title: "Oktatóanyag: Azure Active Directory-integráció ellenszolgáltatás átjáróval |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ellenszolgáltatás átjáró között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34336386-998a-4d47-ab55-721d97708e5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: b1a468caa22159ad603dbec1ef530e7e0e24f96d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-reward-gateway"></a><span data-ttu-id="889df-103">Oktatóanyag: Azure Active Directory-integráció ellenszolgáltatás átjáróval</span><span class="sxs-lookup"><span data-stu-id="889df-103">Tutorial: Azure Active Directory integration with Reward Gateway</span></span>

<span data-ttu-id="889df-104">Ebben az oktatóanyagban elsajátíthatja ellenszolgáltatás átjáró integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="889df-104">In this tutorial, you learn how to integrate Reward Gateway with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="889df-105">Ellenszolgáltatás átjáró integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="889df-105">Integrating Reward Gateway with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="889df-106">Szabályozhatja az Azure AD, aki hozzáfér ellenszolgáltatás átjáró</span><span class="sxs-lookup"><span data-stu-id="889df-106">You can control in Azure AD who has access to Reward Gateway</span></span>
- <span data-ttu-id="889df-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett ellenszolgáltatás átjáró (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="889df-107">You can enable your users to automatically get signed-on to Reward Gateway (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="889df-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="889df-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="889df-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="889df-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="889df-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="889df-110">Prerequisites</span></span>

<span data-ttu-id="889df-111">Konfigurálja az Azure AD-integrációs ellenszolgáltatás átjáró, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="889df-111">To configure Azure AD integration with Reward Gateway, you need the following items:</span></span>

- <span data-ttu-id="889df-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="889df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="889df-113">Egy ellenszolgáltatás átjáró egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="889df-113">A Reward Gateway single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="889df-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="889df-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="889df-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="889df-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="889df-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="889df-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="889df-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="889df-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="889df-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="889df-118">Scenario description</span></span>
<span data-ttu-id="889df-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="889df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="889df-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="889df-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="889df-121">Ellenszolgáltatás átjáró hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="889df-121">Adding Reward Gateway from the gallery</span></span>
2. <span data-ttu-id="889df-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="889df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-reward-gateway-from-the-gallery"></a><span data-ttu-id="889df-123">Ellenszolgáltatás átjáró hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="889df-123">Adding Reward Gateway from the gallery</span></span>
<span data-ttu-id="889df-124">Az Azure AD integrálása a ellenszolgáltatás átjáró konfigurálásához szüksége ellenszolgáltatás átjáró hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="889df-124">To configure the integration of Reward Gateway into Azure AD, you need to add Reward Gateway from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="889df-125">**Ellenszolgáltatás átjáró hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="889df-125">**To add Reward Gateway from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="889df-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="889df-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="889df-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="889df-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="889df-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="889df-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="889df-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="889df-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="889df-133">Írja be a keresőmezőbe, **ellenszolgáltatás átjáró**.</span><span class="sxs-lookup"><span data-stu-id="889df-133">In the search box, type **Reward Gateway**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_search.png)

5. <span data-ttu-id="889df-135">Az eredmények panelen válassza ki a **ellenszolgáltatás átjáró**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="889df-135">In the results panel, select **Reward Gateway**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="889df-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="889df-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="889df-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ellenszolgáltatás átjáró.</span><span class="sxs-lookup"><span data-stu-id="889df-138">In this section, you configure and test Azure AD single sign-on with Reward Gateway based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="889df-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó az ellenszolgáltatás átjáró a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="889df-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Reward Gateway is to a user in Azure AD.</span></span> <span data-ttu-id="889df-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ellenszolgáltatás átjáró közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="889df-140">In other words, a link relationship between an Azure AD user and the related user in Reward Gateway needs to be established.</span></span>

<span data-ttu-id="889df-141">Az átjáró ellenszolgáltatás, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="889df-141">In Reward Gateway, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="889df-142">Az Azure AD az egyszeri bejelentkezés ellenszolgáltatás átjáróval tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="889df-142">To configure and test Azure AD single sign-on with Reward Gateway, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="889df-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="889df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="889df-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="889df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="889df-145">**[Ellenszolgáltatás átjáró tesztfelhasználó létrehozása](#creating-a-reward-gateway-test-user)**  - való egy megfelelője a Britta Simon ellenszolgáltatás átjáró, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="889df-145">**[Creating a Reward Gateway test user](#creating-a-reward-gateway-test-user)** - to have a counterpart of Britta Simon in Reward Gateway that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="889df-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="889df-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="889df-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="889df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="889df-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="889df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="889df-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ellenszolgáltatás átjáró alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="889df-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Reward Gateway application.</span></span>

<span data-ttu-id="889df-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés ellenszolgáltatás átjáró, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="889df-150">**To configure Azure AD single sign-on with Reward Gateway, perform the following steps:**</span></span>

1. <span data-ttu-id="889df-151">Az Azure portálon a a **ellenszolgáltatás átjáró** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="889df-151">In the Azure portal, on the **Reward Gateway** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="889df-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="889df-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_samlbase.png)

3. <span data-ttu-id="889df-155">Az a **ellenszolgáltatás Átjárótartományhoz és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="889df-155">On the **Reward Gateway Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_url.png)

    <span data-ttu-id="889df-157">a.</span><span class="sxs-lookup"><span data-stu-id="889df-157">a.</span></span> <span data-ttu-id="889df-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="889df-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.rewardgateway.com` |
    | `https://<companyname>.rewardgateway.co.uk/` |
    | `https://<companyname>.rewardgateway.co.nz/` |
    | `https://<companyname>.rewardgateway.com.au/` |

    <span data-ttu-id="889df-159">b.</span><span class="sxs-lookup"><span data-stu-id="889df-159">b.</span></span> <span data-ttu-id="889df-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="889df-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    |  `https://<companyname>.rewardgateway.com/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.uk/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.nz/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.com.au/Authentication/EndLogin?idp=<Unique Id>` |

    > [!NOTE] 
    > <span data-ttu-id="889df-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="889df-161">These values are not real.</span></span> <span data-ttu-id="889df-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="889df-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="889df-163">Ügyfél [ellenszolgáltatás átjáró támogatási csoport](mailto:clientsupport@rewardgateway.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="889df-163">Contact [Reward Gateway support team](mailto:clientsupport@rewardgateway.com) to get these values.</span></span>
 
4. <span data-ttu-id="889df-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="889df-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_certificate.png) 

5. <span data-ttu-id="889df-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="889df-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="889df-168">Egyszeri bejelentkezés konfigurálása **ellenszolgáltatás átjáró** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [ellenszolgáltatás átjáró támogatási csoport](mailto:clientsupport@rewardgateway.com).</span><span class="sxs-lookup"><span data-stu-id="889df-168">To configure single sign-on on **Reward Gateway** side, you need to send the downloaded **Metadata XML** to [Reward Gateway support team](mailto:clientsupport@rewardgateway.com).</span></span> <span data-ttu-id="889df-169">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="889df-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="889df-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="889df-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="889df-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="889df-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="889df-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="889df-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="889df-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="889df-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="889df-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="889df-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="889df-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="889df-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="889df-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="889df-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="889df-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="889df-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="889df-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="889df-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="889df-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="889df-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="889df-185">a.</span><span class="sxs-lookup"><span data-stu-id="889df-185">a.</span></span> <span data-ttu-id="889df-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="889df-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="889df-187">b.</span><span class="sxs-lookup"><span data-stu-id="889df-187">b.</span></span> <span data-ttu-id="889df-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="889df-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="889df-189">c.</span><span class="sxs-lookup"><span data-stu-id="889df-189">c.</span></span> <span data-ttu-id="889df-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="889df-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="889df-191">d.</span><span class="sxs-lookup"><span data-stu-id="889df-191">d.</span></span> <span data-ttu-id="889df-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="889df-192">Click **Create**.</span></span>
 
### <a name="creating-a-reward-gateway-test-user"></a><span data-ttu-id="889df-193">Ellenszolgáltatás átjáró tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="889df-193">Creating a Reward Gateway test user</span></span>

<span data-ttu-id="889df-194">Ebben a szakaszban az ellenszolgáltatás átjáró Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="889df-194">In this section, you create a user called Britta Simon in Reward Gateway.</span></span> <span data-ttu-id="889df-195">Ellenszolgáltatás átjáróként működik [támogatási csoport](mailto:clientsupport@rewardgateway.com) a felhasználók hozzáadása az átjáró ellenszolgáltatás platform.</span><span class="sxs-lookup"><span data-stu-id="889df-195">Work with Reward Gateway [support team](mailto:clientsupport@rewardgateway.com) to add the users in the Reward Gateway platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="889df-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="889df-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="889df-197">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés által biztosított hozzáférés ellenszolgáltatás átjáró használatára.</span><span class="sxs-lookup"><span data-stu-id="889df-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Reward Gateway.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="889df-199">**Britta Simon hozzárendelése ellenszolgáltatás átjáró, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="889df-199">**To assign Britta Simon to Reward Gateway, perform the following steps:**</span></span>

1. <span data-ttu-id="889df-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="889df-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="889df-202">Az alkalmazások listában válassza ki a **ellenszolgáltatás átjáró**.</span><span class="sxs-lookup"><span data-stu-id="889df-202">In the applications list, select **Reward Gateway**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_app.png) 

3. <span data-ttu-id="889df-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="889df-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="889df-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="889df-206">Click **Add** button.</span></span> <span data-ttu-id="889df-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="889df-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="889df-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="889df-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="889df-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="889df-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="889df-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="889df-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="889df-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="889df-212">Testing single sign-on</span></span>

<span data-ttu-id="889df-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="889df-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="889df-214">Ha a hozzáférési panelen ellenszolgáltatás átjáró csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ellenszolgáltatás átjáró alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="889df-214">When you click the Reward Gateway tile in the Access Panel, you should get automatically signed-on to your Reward Gateway application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="889df-215">További források</span><span class="sxs-lookup"><span data-stu-id="889df-215">Additional resources</span></span>

* [<span data-ttu-id="889df-216">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="889df-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="889df-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="889df-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_203.png

