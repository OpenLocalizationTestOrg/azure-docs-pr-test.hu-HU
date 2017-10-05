---
title: "Oktatóanyag: Azure Active Directoryval integrált O.C. Péter - AppreciateHub |} Microsoft Docs"
description: "Egyszeri bejelentkezés Azure Active Directory és O.C. konfigurálása Péter - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 9af12372b30d9ee1575e46be3b4144fc3b73ec69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a><span data-ttu-id="17ffd-105">Oktatóanyag: Azure Active Directoryval integrált O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-105">Tutorial: Azure Active Directory integration with O.C.</span></span> <span data-ttu-id="17ffd-106">Péter - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="17ffd-106">Tanner - AppreciateHub</span></span>

<span data-ttu-id="17ffd-107">Ebben az oktatóanyagban elsajátíthatja, hogyan integrálhatja O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-107">In this tutorial, you learn how to integrate O.C.</span></span> <span data-ttu-id="17ffd-108">Péter - AppreciateHub az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="17ffd-108">Tanner - AppreciateHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="17ffd-109">O.C. integrálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-109">Integrating O.C.</span></span> <span data-ttu-id="17ffd-110">Péter - AppreciateHub az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="17ffd-110">Tanner - AppreciateHub with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="17ffd-111">Megadhatja a O.C. hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="17ffd-111">You can control in Azure AD who has access to O.C.</span></span> <span data-ttu-id="17ffd-112">Péter - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="17ffd-112">Tanner - AppreciateHub</span></span>
- <span data-ttu-id="17ffd-113">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-113">You can enable your users to automatically get signed-on to O.C.</span></span> <span data-ttu-id="17ffd-114">Péter - AppreciateHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="17ffd-114">Tanner - AppreciateHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="17ffd-115">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="17ffd-115">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="17ffd-116">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="17ffd-116">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17ffd-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="17ffd-117">Prerequisites</span></span>

<span data-ttu-id="17ffd-118">Az Azure AD-integrációs O.C. konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-118">To configure Azure AD integration with O.C.</span></span> <span data-ttu-id="17ffd-119">Péter - AppreciateHub, van szüksége a következőkre:</span><span class="sxs-lookup"><span data-stu-id="17ffd-119">Tanner - AppreciateHub, you need the following items:</span></span>

- <span data-ttu-id="17ffd-120">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="17ffd-120">An Azure AD subscription</span></span>
- <span data-ttu-id="17ffd-121">EGY O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-121">A O.C.</span></span> <span data-ttu-id="17ffd-122">Péter - AppreciateHub egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="17ffd-122">Tanner - AppreciateHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="17ffd-123">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="17ffd-123">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="17ffd-124">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="17ffd-124">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="17ffd-125">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="17ffd-125">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="17ffd-126">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="17ffd-126">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="17ffd-127">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="17ffd-127">Scenario description</span></span>
<span data-ttu-id="17ffd-128">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="17ffd-128">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="17ffd-129">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="17ffd-129">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="17ffd-130">O.C. hozzáadása</span><span class="sxs-lookup"><span data-stu-id="17ffd-130">Adding O.C.</span></span> <span data-ttu-id="17ffd-131">Péter - AppreciateHub a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="17ffd-131">Tanner - AppreciateHub from the gallery</span></span>
2. <span data-ttu-id="17ffd-132">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="17ffd-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a><span data-ttu-id="17ffd-133">O.C. hozzáadása</span><span class="sxs-lookup"><span data-stu-id="17ffd-133">Adding O.C.</span></span> <span data-ttu-id="17ffd-134">Péter - AppreciateHub a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="17ffd-134">Tanner - AppreciateHub from the gallery</span></span>
<span data-ttu-id="17ffd-135">O.C. integrációjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-135">To configure the integration of O.C.</span></span> <span data-ttu-id="17ffd-136">Péter - AppreciateHub az Azure AD-be, hozzá kell adnia O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-136">Tanner - AppreciateHub into Azure AD, you need to add O.C.</span></span> <span data-ttu-id="17ffd-137">Péter - AppreciateHub a gyűjteményből, a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="17ffd-137">Tanner - AppreciateHub from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="17ffd-138">**O.C. hozzáadása Péter - AppreciateHub a gyűjteményből, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="17ffd-138">**To add O.C. Tanner - AppreciateHub from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="17ffd-139">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-139">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="17ffd-141">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-141">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="17ffd-142">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-142">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="17ffd-144">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-144">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="17ffd-146">Írja be a keresőmezőbe, **O.C. Péter - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-146">In the search box, type **O.C. Tanner - AppreciateHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. <span data-ttu-id="17ffd-148">Az eredmények panelen válassza ki a **O.C. Péter - AppreciateHub**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="17ffd-148">In the results panel, select **O.C. Tanner - AppreciateHub**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="17ffd-150">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="17ffd-150">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="17ffd-151">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-151">In this section, you configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="17ffd-152">Péter - AppreciateHub "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="17ffd-152">Tanner - AppreciateHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="17ffd-153">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, milyen a párjukhoz felhasználó O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-153">For single sign-on to work, Azure AD needs to know what the counterpart user in O.C.</span></span> <span data-ttu-id="17ffd-154">Péter - AppreciateHub van egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="17ffd-154">Tanner - AppreciateHub is to a user in Azure AD.</span></span> <span data-ttu-id="17ffd-155">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a O.C. közötti kapcsolat kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="17ffd-155">In other words, a link relationship between an Azure AD user and the related user in O.C.</span></span> <span data-ttu-id="17ffd-156">Péter - AppreciateHub kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="17ffd-156">Tanner - AppreciateHub needs to be established.</span></span>

<span data-ttu-id="17ffd-157">A O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-157">In O.C.</span></span> <span data-ttu-id="17ffd-158">Péter - AppreciateHub, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="17ffd-158">Tanner - AppreciateHub, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="17ffd-159">Az Azure AD egyszeri bejelentkezést a O.C. tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-159">To configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="17ffd-160">Péter - AppreciateHub, végre kell hajtania a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="17ffd-160">Tanner - AppreciateHub, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="17ffd-161">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="17ffd-161">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="17ffd-162">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="17ffd-162">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="17ffd-163">**[Egy O.C. létrehozása Péter - AppreciateHub tesztfelhasználó](#creating-a-oc-tanner---appreciatehub-test-user)**  - való egy megfelelője a Britta Simon O.C.</span><span class="sxs-lookup"><span data-stu-id="17ffd-163">**[Creating a O.C. Tanner - AppreciateHub test user](#creating-a-oc-tanner---appreciatehub-test-user)** - to have a counterpart of Britta Simon in O.C.</span></span> <span data-ttu-id="17ffd-164">Péter - AppreciateHub, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="17ffd-164">Tanner - AppreciateHub that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="17ffd-165">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="17ffd-165">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="17ffd-166">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="17ffd-166">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="17ffd-167">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-167">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="17ffd-168">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a O.C. az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17ffd-168">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your O.C.</span></span> <span data-ttu-id="17ffd-169">Péter - AppreciateHub alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17ffd-169">Tanner - AppreciateHub application.</span></span>

<span data-ttu-id="17ffd-170">**Az Azure AD az egyszeri bejelentkezés O.C. konfigurálása Péter - AppreciateHub, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="17ffd-170">**To configure Azure AD single sign-on with O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

1. <span data-ttu-id="17ffd-171">Az Azure portálon a a **O.C. Péter - AppreciateHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-171">In the Azure portal, on the **O.C. Tanner - AppreciateHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="17ffd-173">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="17ffd-173">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. <span data-ttu-id="17ffd-175">Az a **O.C. Péter - AppreciateHub tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="17ffd-175">On the **O.C. Tanner - AppreciateHub Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    <span data-ttu-id="17ffd-177">a.</span><span class="sxs-lookup"><span data-stu-id="17ffd-177">a.</span></span> <span data-ttu-id="17ffd-178">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span><span class="sxs-lookup"><span data-stu-id="17ffd-178">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="17ffd-179">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="17ffd-179">This value is not real.</span></span> <span data-ttu-id="17ffd-180">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="17ffd-180">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="17ffd-181">Ügyfél [O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="17ffd-181">Contact [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) to get this value.</span></span>

    <span data-ttu-id="17ffd-182">b.</span><span class="sxs-lookup"><span data-stu-id="17ffd-182">b.</span></span> <span data-ttu-id="17ffd-183">Nyissa meg a metaadat-fájlt a következő hivatkozás használatával: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span><span class="sxs-lookup"><span data-stu-id="17ffd-183">Open the metadata file using the following link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span></span>
   
    <span data-ttu-id="17ffd-184">c.</span><span class="sxs-lookup"><span data-stu-id="17ffd-184">c.</span></span> <span data-ttu-id="17ffd-185">Keresse meg a **md:AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="17ffd-185">Locate the **md:AssertionConsumerService** node.</span></span> 
   
    <span data-ttu-id="17ffd-186">d.</span><span class="sxs-lookup"><span data-stu-id="17ffd-186">d.</span></span> <span data-ttu-id="17ffd-187">Másolja a értékének a **hely** attribútum.</span><span class="sxs-lookup"><span data-stu-id="17ffd-187">Copy the value of the **Location** attribute.</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][12]
   
    <span data-ttu-id="17ffd-189">e.</span><span class="sxs-lookup"><span data-stu-id="17ffd-189">e.</span></span> <span data-ttu-id="17ffd-190">Az a **URL-cím bejelentkezési** szövegmezőhöz túli az érték az előző lépésben szerzett be.</span><span class="sxs-lookup"><span data-stu-id="17ffd-190">In the **Sign On URL** textbox, past the value you have obtained in the previous step.</span></span>

4. <span data-ttu-id="17ffd-191">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="17ffd-191">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. <span data-ttu-id="17ffd-193">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-193">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="17ffd-195">Egyszeri bejelentkezés konfigurálása **O.C. Péter - AppreciateHub** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com).</span><span class="sxs-lookup"><span data-stu-id="17ffd-195">To configure single sign-on on **O.C. Tanner - AppreciateHub** side, you need to send the downloaded **Metadata XML** to [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com).</span></span>

> [!TIP]
> <span data-ttu-id="17ffd-196">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="17ffd-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="17ffd-197">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="17ffd-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="17ffd-198">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="17ffd-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="17ffd-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="17ffd-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="17ffd-200">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="17ffd-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="17ffd-202">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="17ffd-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="17ffd-203">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="17ffd-205">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="17ffd-207">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="17ffd-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="17ffd-209">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="17ffd-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="17ffd-211">a.</span><span class="sxs-lookup"><span data-stu-id="17ffd-211">a.</span></span> <span data-ttu-id="17ffd-212">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="17ffd-213">b.</span><span class="sxs-lookup"><span data-stu-id="17ffd-213">b.</span></span> <span data-ttu-id="17ffd-214">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="17ffd-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="17ffd-215">c.</span><span class="sxs-lookup"><span data-stu-id="17ffd-215">c.</span></span> <span data-ttu-id="17ffd-216">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="17ffd-217">d.</span><span class="sxs-lookup"><span data-stu-id="17ffd-217">d.</span></span> <span data-ttu-id="17ffd-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-218">Click **Create**.</span></span>
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a><span data-ttu-id="17ffd-219">Egy O.C. létrehozása</span><span class="sxs-lookup"><span data-stu-id="17ffd-219">Creating a O.C.</span></span> <span data-ttu-id="17ffd-220">Péter - AppreciateHub tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="17ffd-220">Tanner - AppreciateHub test user</span></span>

<span data-ttu-id="17ffd-221">Ez a szakasz célja Britta Simon meghívta O.C. felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="17ffd-221">The objective of this section is to create a user called Britta Simon in O.C.</span></span> <span data-ttu-id="17ffd-222">Péter - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="17ffd-222">Tanner - AppreciateHub.</span></span>

<span data-ttu-id="17ffd-223">**Felhasználó létrehozásához O.C. Britta Simon meghívta Péter - AppreciateHub, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="17ffd-223">**To create a user called Britta Simon in O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

<span data-ttu-id="17ffd-224">Kérje meg a [O.C. Péter - AppreciateHub támogatási csoport](mailto:sso@octanner.com) , amely rendelkezik nameID attribútum értéke Britta Simon felhasználó neve megegyezik az Azure AD-felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="17ffd-224">Ask your [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) to create a user that has as nameID attribute the same value as the user name of Britta Simon in Azure AD.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="17ffd-225">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="17ffd-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="17ffd-226">Ebben a szakaszban Britta Simon által biztosított hozzáférés O.C. használandó Azure egyszeri bejelentkezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="17ffd-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to O.C.</span></span> <span data-ttu-id="17ffd-227">Péter - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="17ffd-227">Tanner - AppreciateHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="17ffd-229">**O.C. Britta Simon hozzárendelése Péter - AppreciateHub, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="17ffd-229">**To assign Britta Simon to O.C. Tanner - AppreciateHub, perform the following steps:**</span></span>

1. <span data-ttu-id="17ffd-230">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-230">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="17ffd-232">Az alkalmazások listában válassza ki a **O.C. Péter - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-232">In the applications list, select **O.C. Tanner - AppreciateHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. <span data-ttu-id="17ffd-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="17ffd-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="17ffd-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="17ffd-236">Click **Add** button.</span></span> <span data-ttu-id="17ffd-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="17ffd-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="17ffd-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="17ffd-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="17ffd-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="17ffd-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="17ffd-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="17ffd-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="17ffd-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="17ffd-242">Testing single sign-on</span></span>

<span data-ttu-id="17ffd-243">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="17ffd-243">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="17ffd-244">Ha a O.C. kattint.</span><span class="sxs-lookup"><span data-stu-id="17ffd-244">When you click the O.C.</span></span> <span data-ttu-id="17ffd-245">Péter - AppreciateHub csempe a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett a O.C. számára</span><span class="sxs-lookup"><span data-stu-id="17ffd-245">Tanner - AppreciateHub tile in the Access Panel, you should get automatically signed-on to your O.C.</span></span> <span data-ttu-id="17ffd-246">Péter - AppreciateHub alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17ffd-246">Tanner - AppreciateHub application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17ffd-247">További források</span><span class="sxs-lookup"><span data-stu-id="17ffd-247">Additional resources</span></span>

* [<span data-ttu-id="17ffd-248">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="17ffd-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="17ffd-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="17ffd-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

