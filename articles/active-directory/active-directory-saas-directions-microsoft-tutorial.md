---
title: "Oktatóanyag: Azure Active Directory-integráció, amelyen Microsoft |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálja az egyszeri bejelentkezés Azure Active Directory és az utasításokat a Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f9c068c71eb00a4c779c91c8ee0f0dc9d6ba85ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="12b2c-103">Oktatóanyag: Azure Active Directory-integráció, amelyen Microsoft</span><span class="sxs-lookup"><span data-stu-id="12b2c-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="12b2c-104">Ebben az oktatóanyagban elsajátíthatja a Microsoft irányban integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="12b2c-104">In this tutorial, you learn how to integrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="12b2c-105">A Microsoft irányban integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="12b2c-105">Integrating Directions on Microsoft with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="12b2c-106">Szabályozhatja, aki hozzáfér utasításait a Microsoft Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="12b2c-106">You can control in Azure AD who has access to Directions on Microsoft</span></span>
- <span data-ttu-id="12b2c-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett irányú (egyszeri bejelentkezés) a Microsoft számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="12b2c-107">You can enable your users to automatically get signed-on to Directions on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="12b2c-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="12b2c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="12b2c-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="12b2c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12b2c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="12b2c-110">Prerequisites</span></span>

<span data-ttu-id="12b2c-111">Amelyen a Microsoft Azure AD-integrációs konfigurálásához a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="12b2c-111">To configure Azure AD integration with Directions on Microsoft, you need the following items:</span></span>

- <span data-ttu-id="12b2c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="12b2c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="12b2c-113">A Microsoft egyszeri bejelentkezéshez egy irányban engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="12b2c-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="12b2c-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="12b2c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="12b2c-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="12b2c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="12b2c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="12b2c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="12b2c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="12b2c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="12b2c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="12b2c-118">Scenario description</span></span>
<span data-ttu-id="12b2c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="12b2c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="12b2c-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="12b2c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="12b2c-121">Microsoft irányban hozzáadásával a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="12b2c-121">Adding Directions on Microsoft from the gallery</span></span>
2. <span data-ttu-id="12b2c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12b2c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-the-gallery"></a><span data-ttu-id="12b2c-123">Microsoft irányban hozzáadásával a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="12b2c-123">Adding Directions on Microsoft from the gallery</span></span>
<span data-ttu-id="12b2c-124">Az integráció konfigurálásához az utasításokat a Microsoft az Azure AD-be kell hozzáadnia utasításait a Microsoft a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="12b2c-124">To configure the integration of Directions on Microsoft into Azure AD, you need to add Directions on Microsoft from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="12b2c-125">**Adja hozzá a Microsoft utasításait a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12b2c-125">**To add Directions on Microsoft from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="12b2c-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="12b2c-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="12b2c-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="12b2c-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="12b2c-133">Írja be a keresőmezőbe, **Microsoft irányú**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-133">In the search box, type **Directions on Microsoft**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="12b2c-135">Az eredmények panelen válassza ki a **Microsoft irányú**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="12b2c-135">In the results panel, select **Directions on Microsoft**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="12b2c-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="12b2c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="12b2c-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést, amelyen Microsoft "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="12b2c-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="12b2c-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Microsoft irányú megfelelőjére felhasználó a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="12b2c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Directions on Microsoft is to a user in Azure AD.</span></span> <span data-ttu-id="12b2c-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Microsoft irányban közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="12b2c-140">In other words, a link relationship between an Azure AD user and the related user in Directions on Microsoft needs to be established.</span></span>

<span data-ttu-id="12b2c-141">Microsoft irányú, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="12b2c-141">In Directions on Microsoft, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="12b2c-142">Az Azure AD egyszeri bejelentkezést, amelyen Microsoft tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="12b2c-142">To configure and test Azure AD single sign-on with Directions on Microsoft, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="12b2c-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="12b2c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="12b2c-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="12b2c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="12b2c-145">**[Egy irányban létrehozása a Microsoft tesztfelhasználó](#creating-a-directions-on-microsoft-test-user)**  – egy megfelelője a Britta Simon rendelkezik, amely csatolva van a felhasználó az Azure AD-ábrázolását Microsoft irányú.</span><span class="sxs-lookup"><span data-stu-id="12b2c-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - to have a counterpart of Britta Simon in Directions on Microsoft that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="12b2c-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12b2c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="12b2c-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="12b2c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="12b2c-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12b2c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="12b2c-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Microsoft alkalmazás irányú egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="12b2c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="12b2c-150">**Az Azure AD egyszeri bejelentkezést, amelyen Microsoft megadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12b2c-150">**To configure Azure AD single sign-on with Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="12b2c-151">Az Azure portálon a a **Microsoft irányú** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-151">In the Azure portal, on the **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="12b2c-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="12b2c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="12b2c-155">A a **Microsoft Domain és URL-címek irányú** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="12b2c-155">On the **Directions on Microsoft Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="12b2c-157">a.</span><span class="sxs-lookup"><span data-stu-id="12b2c-157">a.</span></span> <span data-ttu-id="12b2c-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="12b2c-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="12b2c-159">b.</span><span class="sxs-lookup"><span data-stu-id="12b2c-159">b.</span></span> <span data-ttu-id="12b2c-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="12b2c-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="12b2c-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="12b2c-161">These values are not real.</span></span> <span data-ttu-id="12b2c-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="12b2c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="12b2c-163">Ügyfél [Microsoft Client irányú támogatási csoport](mailto:service@DirectionsOnMicrosoft.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="12b2c-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) to get these values.</span></span> 
 
4. <span data-ttu-id="12b2c-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="12b2c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="12b2c-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="12b2c-168">Egyszeri bejelentkezés konfigurálása **Microsoft irányú** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [utasításait a Microsoft támogatási csoport](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="12b2c-168">To configure single sign-on on **Directions on Microsoft** side, you need to send the downloaded **Metadata XML** to [Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="12b2c-169">Ahhoz, hogy a Microsoft támogatási csapatával az összevont telephelyi tagság található utasításokat, vegye fel a vállalati adatok az e-maileket.</span><span class="sxs-lookup"><span data-stu-id="12b2c-169">To enable the Directions on Microsoft support team to locate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="12b2c-170">Egyszeri bejelentkezés a Microsoft irányú kell engedélyeznie a [Microsoft Client irányú támogatási csoport](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="12b2c-170">Single sign-on for Directions on Microsoft needs to be enabled by the [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="12b2c-171">Egy értesítést fog kapni, amikor az egyszeri bejelentkezés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="12b2c-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="12b2c-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="12b2c-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="12b2c-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="12b2c-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="12b2c-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="12b2c-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="12b2c-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12b2c-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="12b2c-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="12b2c-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="12b2c-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12b2c-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="12b2c-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="12b2c-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="12b2c-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="12b2c-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="12b2c-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="12b2c-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="12b2c-187">a.</span><span class="sxs-lookup"><span data-stu-id="12b2c-187">a.</span></span> <span data-ttu-id="12b2c-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="12b2c-189">b.</span><span class="sxs-lookup"><span data-stu-id="12b2c-189">b.</span></span> <span data-ttu-id="12b2c-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="12b2c-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="12b2c-191">c.</span><span class="sxs-lookup"><span data-stu-id="12b2c-191">c.</span></span> <span data-ttu-id="12b2c-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="12b2c-193">d.</span><span class="sxs-lookup"><span data-stu-id="12b2c-193">d.</span></span> <span data-ttu-id="12b2c-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="12b2c-195">Egy irányban Microsoft tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="12b2c-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="12b2c-196">Nincs művelet elem ahhoz, hogy a felhasználó Microsoft irányban történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="12b2c-196">There is no action item for you to configure user provisioning to Directions on Microsoft.</span></span>  

<span data-ttu-id="12b2c-197">Ha egy hozzárendelt felhasználó megpróbál bejelentkezni a Microsoft a hozzáférési panelen irányú, utasításait a Microsoft ellenőrzi, hogy létezik-e a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="12b2c-197">When an assigned user tries to log in to Directions on Microsoft using the access panel, Directions on Microsoft checks whether the user exists.</span></span> <span data-ttu-id="12b2c-198">Nincs még nincs felhasználói fiók érhető el, ha a Microsoft irányú automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="12b2c-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="12b2c-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="12b2c-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="12b2c-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés utasításait a Microsoft Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="12b2c-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Directions on Microsoft.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="12b2c-202">**Britta Simon rendel Microsoft irányú, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="12b2c-202">**To assign Britta Simon to Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="12b2c-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="12b2c-205">Az alkalmazások listában válassza ki a **Microsoft irányú**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-205">In the applications list, select **Directions on Microsoft**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="12b2c-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="12b2c-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="12b2c-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="12b2c-209">Click **Add** button.</span></span> <span data-ttu-id="12b2c-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12b2c-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="12b2c-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="12b2c-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="12b2c-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12b2c-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="12b2c-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="12b2c-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="12b2c-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="12b2c-215">Testing single sign-on</span></span>

<span data-ttu-id="12b2c-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="12b2c-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="12b2c-217">Kattintson a Microsoft csempére a hozzáférési panelen megjelenő utasításokat, meg kell beolvasása automatikusan aláírt a a Microsoft alkalmazás irányú.</span><span class="sxs-lookup"><span data-stu-id="12b2c-217">When you click the Directions on Microsoft tile in the Access Panel, you should get automatically signed-on to your Directions on Microsoft application.</span></span>

<span data-ttu-id="12b2c-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="12b2c-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="12b2c-219">További források</span><span class="sxs-lookup"><span data-stu-id="12b2c-219">Additional resources</span></span>

* [<span data-ttu-id="12b2c-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="12b2c-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12b2c-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="12b2c-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

