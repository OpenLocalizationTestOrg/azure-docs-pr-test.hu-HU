---
title: "Oktatóanyag: Azure Active Directoryval integrált Cerner központi |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Cerner központi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 77b5fb94cdfa5722081198aabc59fbf86229c2b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="45855-103">Oktatóanyag: Azure Active Directoryval integrált Cerner központi</span><span class="sxs-lookup"><span data-stu-id="45855-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="45855-104">Ebben az oktatóanyagban elsajátíthatja Cerner központi integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="45855-104">In this tutorial, you learn how to integrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="45855-105">Cerner központi integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="45855-105">Integrating Cerner Central with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="45855-106">Megadhatja a Cerner központi hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="45855-106">You can control in Azure AD who has access to Cerner Central</span></span>
- <span data-ttu-id="45855-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Cerner központi (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="45855-107">You can enable your users to automatically get signed-on to Cerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="45855-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="45855-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="45855-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="45855-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45855-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45855-110">Prerequisites</span></span>

<span data-ttu-id="45855-111">Konfigurálása az Azure AD-integrációs Cerner központi, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="45855-111">To configure Azure AD integration with Cerner Central, you need the following items:</span></span>

- <span data-ttu-id="45855-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="45855-112">An Azure AD subscription</span></span>
- <span data-ttu-id="45855-113">Egy jóváhagyott Cerner központi rendszerfiók</span><span class="sxs-lookup"><span data-stu-id="45855-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="45855-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="45855-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="45855-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="45855-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="45855-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="45855-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="45855-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="45855-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="45855-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="45855-118">Scenario description</span></span>
<span data-ttu-id="45855-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="45855-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="45855-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="45855-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="45855-121">A gyűjteményből Cerner központi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="45855-121">Adding Cerner Central from the gallery</span></span>
2. <span data-ttu-id="45855-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="45855-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-the-gallery"></a><span data-ttu-id="45855-123">A gyűjteményből Cerner központi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="45855-123">Adding Cerner Central from the gallery</span></span>
<span data-ttu-id="45855-124">Az Azure AD integrálása a Cerner központi konfigurálásához kell hozzáadnia Cerner központi a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="45855-124">To configure the integration of Cerner Central into Azure AD, you need to add Cerner Central from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="45855-125">**A gyűjteményből Cerner központi hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45855-125">**To add Cerner Central from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="45855-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45855-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="45855-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="45855-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="45855-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45855-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="45855-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel felett.</span><span class="sxs-lookup"><span data-stu-id="45855-131">To add new application, click **New application** button on top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="45855-133">Írja be a keresőmezőbe, **Cerner központi**.</span><span class="sxs-lookup"><span data-stu-id="45855-133">In the search box, type **Cerner Central**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="45855-135">Az eredmények panelen válassza ki a **Cerner központi**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="45855-135">In the results panel, select **Cerner Central**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="45855-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="45855-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="45855-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a Cerner központi "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="45855-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="45855-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Cerner központi a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="45855-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cerner Central is to a user in Azure AD.</span></span> <span data-ttu-id="45855-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Cerner központi közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="45855-140">In other words, a link relationship between an Azure AD user and the related user in Cerner Central needs to be established.</span></span>

<span data-ttu-id="45855-141">Az Azure AD egyszeri bejelentkezést a Cerner központi tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="45855-141">To configure and test Azure AD single sign-on with Cerner Central, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="45855-142">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="45855-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="45855-143">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="45855-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="45855-144">**[Cerner központi tesztfelhasználó létrehozása](#creating-a-cerner-central-test-user)**  - való egy megfelelője a Britta Simon Cerner központi, amely csatolva van a felhasználó az Azure AD ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="45855-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - to have a counterpart of Britta Simon in Cerner Central that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="45855-145">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="45855-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="45855-146">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="45855-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="45855-147">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="45855-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="45855-148">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Cerner központi alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="45855-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="45855-149">**Az Azure AD az egyszeri bejelentkezés konfigurálása Cerner központi, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="45855-149">**To configure Azure AD single sign-on with Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="45855-150">Az Azure portálon a a **Cerner központi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="45855-150">In the Azure portal, on the **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="45855-152">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="45855-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="45855-154">Az a **Cerner központi tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="45855-154">On the **Cerner Central Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="45855-156">a.</span><span class="sxs-lookup"><span data-stu-id="45855-156">a.</span></span> <span data-ttu-id="45855-157">Az a **azonosító** szövegmező, írja be az értéket a következő minták használatával:</span><span class="sxs-lookup"><span data-stu-id="45855-157">In the **Identifier** textbox, type the value using the following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="45855-158">b.</span><span class="sxs-lookup"><span data-stu-id="45855-158">b.</span></span> <span data-ttu-id="45855-159">Az a **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő minták használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="45855-159">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="45855-160">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="45855-160">These values are not the real.</span></span> <span data-ttu-id="45855-161">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="45855-161">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="45855-162">Ügyfél [Cerner központi támogatási csoport](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="45855-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) to get these values.</span></span>
 
4. <span data-ttu-id="45855-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="45855-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="45855-165">Létrehozásához a **metaadatok** URL-címe, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="45855-165">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="45855-166">a.</span><span class="sxs-lookup"><span data-stu-id="45855-166">a.</span></span> <span data-ttu-id="45855-167">Kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="45855-167">Click **App registrations**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="45855-169">b.</span><span class="sxs-lookup"><span data-stu-id="45855-169">b.</span></span> <span data-ttu-id="45855-170">Kattintson a **végpontok** megnyitásához **végpontok** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="45855-170">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="45855-172">c.</span><span class="sxs-lookup"><span data-stu-id="45855-172">c.</span></span> <span data-ttu-id="45855-173">Kattintson a Másolás gombra másolása **ÖSSZEVONÁSI METAADAT-dokumentum** URL-címet, és illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="45855-173">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="45855-175">d.</span><span class="sxs-lookup"><span data-stu-id="45855-175">d.</span></span> <span data-ttu-id="45855-176">Most lépjen a tulajdonságlapján **Cerner központi** , és másolja a **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.</span><span class="sxs-lookup"><span data-stu-id="45855-176">Now go to the property page of **Cerner Central** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="45855-178">e.</span><span class="sxs-lookup"><span data-stu-id="45855-178">e.</span></span> <span data-ttu-id="45855-179">Készítése a **metaadatainak URL-CÍMÉT** a következő minta használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="45855-179">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="45855-180">Egyszeri bejelentkezés konfigurálása **Cerner központi** oldalon kell küldeniük a **metaadatainak URL-CÍMÉT** való [Cerner központi támogatási](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="45855-180">To configure single sign-on on **Cerner Central** side, you need to send the **Metadata URL** to [Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="45855-181">Az egyszeri bejelentkezés az integráció végrehajtásához alkalmazás oldalon konfigurálják.</span><span class="sxs-lookup"><span data-stu-id="45855-181">They configure the SSO on application side to complete the integration.</span></span>

> [!TIP]
> <span data-ttu-id="45855-182">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="45855-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="45855-183">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="45855-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="45855-184">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="45855-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="45855-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="45855-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="45855-186">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="45855-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span> 

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="45855-188">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45855-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="45855-189">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="45855-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="45855-191">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="45855-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="45855-193">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="45855-193">To open the **User** dialog, click **Add**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="45855-195">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="45855-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="45855-197">a.</span><span class="sxs-lookup"><span data-stu-id="45855-197">a.</span></span> <span data-ttu-id="45855-198">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="45855-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="45855-199">b.</span><span class="sxs-lookup"><span data-stu-id="45855-199">b.</span></span> <span data-ttu-id="45855-200">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="45855-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="45855-201">c.</span><span class="sxs-lookup"><span data-stu-id="45855-201">c.</span></span> <span data-ttu-id="45855-202">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="45855-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="45855-203">d.</span><span class="sxs-lookup"><span data-stu-id="45855-203">d.</span></span> <span data-ttu-id="45855-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="45855-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="45855-205">Cerner központi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="45855-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="45855-206">**Cerner központi** alkalmazás lehetővé teszi, hogy a hitelesítés bármely összevont identitás-szolgáltatójából.</span><span class="sxs-lookup"><span data-stu-id="45855-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="45855-207">Ha a felhasználó képes-e jelentkezni az alkalmazás kezdőlapját, amelyek összevont, és nincs szükségük a manuális létesítési.</span><span class="sxs-lookup"><span data-stu-id="45855-207">If a user is able to log in to the application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="45855-208">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="45855-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="45855-209">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Cerner központi Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="45855-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cerner Central.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="45855-211">**Britta Simon hozzárendelése Cerner központi, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="45855-211">**To assign Britta Simon to Cerner Central, perform the following steps:**</span></span>

1. <span data-ttu-id="45855-212">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="45855-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="45855-214">Az alkalmazások listában válassza ki a **Cerner központi**.</span><span class="sxs-lookup"><span data-stu-id="45855-214">In the applications list, select **Cerner Central**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="45855-216">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="45855-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="45855-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="45855-218">Click **Add** button.</span></span> <span data-ttu-id="45855-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45855-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="45855-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="45855-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="45855-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45855-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="45855-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45855-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="45855-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="45855-224">Testing single sign-on</span></span>

<span data-ttu-id="45855-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="45855-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="45855-226">Ha a hozzáférési panelen Cerner központi csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Cerner központi alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="45855-226">When you click the Cerner Central tile in the Access Panel, you should get automatically signed-on to your Cerner Central application.</span></span> <span data-ttu-id="45855-227">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="45855-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="45855-228">További források</span><span class="sxs-lookup"><span data-stu-id="45855-228">Additional resources</span></span>

* [<span data-ttu-id="45855-229">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="45855-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="45855-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="45855-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

