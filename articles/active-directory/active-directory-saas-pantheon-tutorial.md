---
title: "Oktatóanyag: Azure Active Directoryval integrált Pantheon |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Pantheon között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3f4ac1db2ee83d9f9fcb375d0fb7c40ad21c4688
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a><span data-ttu-id="652d9-103">Oktatóanyag: Azure Active Directoryval integrált Pantheon</span><span class="sxs-lookup"><span data-stu-id="652d9-103">Tutorial: Azure Active Directory integration with Pantheon</span></span>

<span data-ttu-id="652d9-104">Ebben az oktatóanyagban elsajátíthatja Pantheon integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="652d9-104">In this tutorial, you learn how to integrate Pantheon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="652d9-105">Pantheon integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="652d9-105">Integrating Pantheon with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="652d9-106">Megadhatja a Pantheon hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="652d9-106">You can control in Azure AD who has access to Pantheon</span></span>
- <span data-ttu-id="652d9-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Pantheon (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="652d9-107">You can enable your users to automatically get signed-on to Pantheon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="652d9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="652d9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="652d9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="652d9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="652d9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="652d9-110">Prerequisites</span></span>

<span data-ttu-id="652d9-111">Konfigurálása az Azure AD-integrációs Pantheon, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="652d9-111">To configure Azure AD integration with Pantheon, you need the following items:</span></span>

- <span data-ttu-id="652d9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="652d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="652d9-113">Egy Pantheon egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="652d9-113">A Pantheon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="652d9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="652d9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="652d9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="652d9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="652d9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="652d9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="652d9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="652d9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="652d9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="652d9-118">Scenario description</span></span>
<span data-ttu-id="652d9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="652d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="652d9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="652d9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="652d9-121">A gyűjteményből Pantheon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="652d9-121">Adding Pantheon from the gallery</span></span>
2. <span data-ttu-id="652d9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="652d9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pantheon-from-the-gallery"></a><span data-ttu-id="652d9-123">A gyűjteményből Pantheon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="652d9-123">Adding Pantheon from the gallery</span></span>
<span data-ttu-id="652d9-124">Az Azure AD integrálása a Pantheon konfigurálásához kell hozzáadnia Pantheon a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="652d9-124">To configure the integration of Pantheon into Azure AD, you need to add Pantheon from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="652d9-125">**A gyűjteményből Pantheon hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="652d9-125">**To add Pantheon from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="652d9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="652d9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="652d9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="652d9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="652d9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="652d9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="652d9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="652d9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="652d9-133">Írja be a keresőmezőbe, **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="652d9-133">In the search box, type **Pantheon**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_search.png)

5. <span data-ttu-id="652d9-135">Az eredmények panelen válassza ki a **Pantheon**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="652d9-135">In the results panel, select **Pantheon**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="652d9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="652d9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="652d9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pantheon.</span><span class="sxs-lookup"><span data-stu-id="652d9-138">In this section, you configure and test Azure AD single sign-on with Pantheon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="652d9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Pantheon a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="652d9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pantheon is to a user in Azure AD.</span></span> <span data-ttu-id="652d9-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Pantheon közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="652d9-140">In other words, a link relationship between an Azure AD user and the related user in Pantheon needs to be established.</span></span>

<span data-ttu-id="652d9-141">Pantheon, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="652d9-141">In Pantheon, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="652d9-142">Az Azure AD egyszeri bejelentkezést a Pantheon tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="652d9-142">To configure and test Azure AD single sign-on with Pantheon, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="652d9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="652d9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="652d9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="652d9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="652d9-145">**[Pantheon tesztfelhasználó létrehozása](#creating-a-pantheon-test-user)**  - való Britta Simon valami Pantheon, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="652d9-145">**[Creating a Pantheon test user](#creating-a-pantheon-test-user)** - to have a counterpart of Britta Simon in Pantheon that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="652d9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="652d9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="652d9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="652d9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="652d9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="652d9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="652d9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Pantheon alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="652d9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pantheon application.</span></span>

<span data-ttu-id="652d9-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Pantheon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="652d9-150">**To configure Azure AD single sign-on with Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="652d9-151">Az Azure portálon a a **Pantheon** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="652d9-151">In the Azure portal, on the **Pantheon** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="652d9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="652d9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. <span data-ttu-id="652d9-155">Az a **Pantheon tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="652d9-155">On the **Pantheon Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_url.png)

    <span data-ttu-id="652d9-157">a.</span><span class="sxs-lookup"><span data-stu-id="652d9-157">a.</span></span> <span data-ttu-id="652d9-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`urn:auth0:pantheon:<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="652d9-158">In the **Identifier** textbox, type a URL using the following pattern: `urn:auth0:pantheon:<orgname>-SSO`</span></span>

    <span data-ttu-id="652d9-159">b.</span><span class="sxs-lookup"><span data-stu-id="652d9-159">b.</span></span> <span data-ttu-id="652d9-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span><span class="sxs-lookup"><span data-stu-id="652d9-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="652d9-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="652d9-161">These values are not real.</span></span> <span data-ttu-id="652d9-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="652d9-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="652d9-163">Ügyfél [Pantheon támogatási csoport](https://pantheon.io/docs/getting-support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="652d9-163">Contact [Pantheon support team](https://pantheon.io/docs/getting-support/) to get these values.</span></span>

4. <span data-ttu-id="652d9-164">Pantheon alkalmazás adott formátum, ami megköveteli, hogy a felhasználó e-mail címe UserIdentifier attribútum értékének beállítása a SAML-előfeltétel vár.</span><span class="sxs-lookup"><span data-stu-id="652d9-164">Pantheon application expects the SAML assertion in specific format, which requires you to set the UserIdentifier attribute value with the user’s email address.</span></span> <span data-ttu-id="652d9-165">Alapértelmezés szerint az Azure AD használja a UserPrincipalName UserIdentifier attribútum.</span><span class="sxs-lookup"><span data-stu-id="652d9-165">By default Azure AD uses the UserPrincipalName for UserIdentifier attribute.</span></span> <span data-ttu-id="652d9-166">Azonban sikeres integrációs érték a felhasználó e-mail címének egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="652d9-166">But for successful integration you need to adjust this value to match with user’s email address.</span></span> <span data-ttu-id="652d9-167">Az integráció csak a helyes leképezés végrehajtása után fog működni.</span><span class="sxs-lookup"><span data-stu-id="652d9-167">The integration will only work after doing the correct mapping.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_attribute.png)  


5. <span data-ttu-id="652d9-169">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="652d9-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_certificate.png)

6. <span data-ttu-id="652d9-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="652d9-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="652d9-173">A a **Pantheon konfigurációs** kattintson **konfigurálása Pantheon** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="652d9-173">On the **Pantheon Configuration** section, click **Configure Pantheon** to open **Configure sign-on** window.</span></span> <span data-ttu-id="652d9-174">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="652d9-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_configure.png) 

8. <span data-ttu-id="652d9-176">Egyszeri bejelentkezés konfigurálása **Pantheon** oldalon kell küldeniük a letöltött **tanúsítvány** és **SAML-alapú egyszeri bejelentkezési URL-címe** való [Pantheon támogatási csoport](https://pantheon.io/docs/getting-support/).</span><span class="sxs-lookup"><span data-stu-id="652d9-176">To configure single sign-on on **Pantheon** side, you need to send the downloaded **Certificate** and **SAML Single Sign-On Service URL** to [Pantheon support team](https://pantheon.io/docs/getting-support/).</span></span>

     > [!Note]
     > <span data-ttu-id="652d9-177">Meg kell adnia az E-mail tartomány(ok) információkat és dátum idő, amikor engedélyezi ezt a kapcsolatot is.</span><span class="sxs-lookup"><span data-stu-id="652d9-177">You also need to provide the Email Domain(s) information and Date Time when you want to enable this connection.</span></span> <span data-ttu-id="652d9-178">További információt az található [Itt](https://pantheon.io/docs/sso-organizations/)</span><span class="sxs-lookup"><span data-stu-id="652d9-178">You can find more details about it from [here](https://pantheon.io/docs/sso-organizations/)</span></span>

> [!TIP]
> <span data-ttu-id="652d9-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="652d9-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="652d9-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="652d9-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="652d9-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="652d9-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="652d9-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="652d9-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="652d9-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="652d9-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="652d9-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="652d9-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="652d9-186">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="652d9-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="652d9-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="652d9-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="652d9-190">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="652d9-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="652d9-192">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="652d9-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pantheon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="652d9-194">a.</span><span class="sxs-lookup"><span data-stu-id="652d9-194">a.</span></span> <span data-ttu-id="652d9-195">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="652d9-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="652d9-196">b.</span><span class="sxs-lookup"><span data-stu-id="652d9-196">b.</span></span> <span data-ttu-id="652d9-197">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="652d9-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="652d9-198">c.</span><span class="sxs-lookup"><span data-stu-id="652d9-198">c.</span></span> <span data-ttu-id="652d9-199">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="652d9-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="652d9-200">d.</span><span class="sxs-lookup"><span data-stu-id="652d9-200">d.</span></span> <span data-ttu-id="652d9-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="652d9-201">Click **Create**.</span></span>
 
### <a name="creating-a-pantheon-test-user"></a><span data-ttu-id="652d9-202">Pantheon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="652d9-202">Creating a Pantheon test user</span></span>

<span data-ttu-id="652d9-203">Ebben a szakaszban egy Pantheon Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="652d9-203">In this section, you create a user called Britta Simon in Pantheon.</span></span> <span data-ttu-id="652d9-204">Kérjük, kövesse a következő lépések végrehajtásával adja hozzá a felhasználót a Pantheon.</span><span class="sxs-lookup"><span data-stu-id="652d9-204">Please follow the below steps to add the user in Pantheon.</span></span> 

>[!NOTE] 
><span data-ttu-id="652d9-205">Az egyszeri bejelentkezés felhasználói működéséhez meg kell először Pantheon hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="652d9-205">For SSO to work user needs to be created first in Pantheon.</span></span>

1. <span data-ttu-id="652d9-206">A bejelentkezési Pantheon a rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="652d9-206">Login to Pantheon with admin credentials.</span></span>

2. <span data-ttu-id="652d9-207">Navigáljon a **szervezet** irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="652d9-207">Navigate to **Organization** dashboard page.</span></span>
 
3. <span data-ttu-id="652d9-208">Kattintson a **személyek**.</span><span class="sxs-lookup"><span data-stu-id="652d9-208">Click **People**.</span></span>

4. <span data-ttu-id="652d9-209">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="652d9-209">Click **Add user**.</span></span>

5. <span data-ttu-id="652d9-210">Adja meg a felhasználó e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="652d9-210">Enter the user's email address.</span></span>

6. <span data-ttu-id="652d9-211">Válassza ki a felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="652d9-211">Choose the user's role.</span></span>

7. <span data-ttu-id="652d9-212">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="652d9-212">Click **Add user**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="652d9-213">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="652d9-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="652d9-214">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Pantheon Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="652d9-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pantheon.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="652d9-216">**Britta Simon hozzárendelése Pantheon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="652d9-216">**To assign Britta Simon to Pantheon, perform the following steps:**</span></span>

1. <span data-ttu-id="652d9-217">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="652d9-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="652d9-219">Az alkalmazások listában válassza ki a **Pantheon**.</span><span class="sxs-lookup"><span data-stu-id="652d9-219">In the applications list, select **Pantheon**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pantheon-tutorial/tutorial_pantheon_app.png) 

3. <span data-ttu-id="652d9-221">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="652d9-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="652d9-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="652d9-223">Click **Add** button.</span></span> <span data-ttu-id="652d9-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="652d9-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="652d9-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="652d9-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="652d9-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="652d9-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="652d9-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="652d9-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="652d9-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="652d9-229">Testing single sign-on</span></span>

<span data-ttu-id="652d9-230">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="652d9-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="652d9-231">Ha a hozzáférési panelen Pantheon csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Pantheon alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="652d9-231">When you click the Pantheon tile in the Access Panel, you should get automatically signed-on to your Pantheon application.</span></span>
<span data-ttu-id="652d9-232">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="652d9-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="652d9-233">További források</span><span class="sxs-lookup"><span data-stu-id="652d9-233">Additional resources</span></span>

* [<span data-ttu-id="652d9-234">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="652d9-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="652d9-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="652d9-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pantheon-tutorial/tutorial_general_203.png

