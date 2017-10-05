---
title: "Oktatóanyag: Azure Active Directoryval integrált Veritas vállalati Vault.cloud SSO |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Veritas vállalati Vault.cloud SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1f8c7fd97021f320a23bc78466a7b61f2d7e513e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a><span data-ttu-id="c7840-103">Oktatóanyag: Azure Active Directoryval integrált Veritas vállalati Vault.cloud egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c7840-103">Tutorial: Azure Active Directory integration with Veritas Enterprise Vault.cloud SSO</span></span>

<span data-ttu-id="c7840-104">Ebben az oktatóanyagban elsajátíthatja Veritas vállalati Vault.cloud SSO integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c7840-104">In this tutorial, you learn how to integrate Veritas Enterprise Vault.cloud SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c7840-105">Veritas vállalati Vault.cloud SSO integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c7840-105">Integrating Veritas Enterprise Vault.cloud SSO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c7840-106">Megadhatja a Veritas vállalati Vault.cloud SSO hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c7840-106">You can control in Azure AD who has access to Veritas Enterprise Vault.cloud SSO</span></span>
- <span data-ttu-id="c7840-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Veritas vállalati Vault.cloud SSO (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="c7840-107">You can enable your users to automatically get signed-on to Veritas Enterprise Vault.cloud SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c7840-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c7840-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c7840-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c7840-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7840-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c7840-110">Prerequisites</span></span>

<span data-ttu-id="c7840-111">Az Azure AD-integrációs konfigurálásához Veritas vállalati Vault.cloud egyszeri bejelentkezési modellel, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c7840-111">To configure Azure AD integration with Veritas Enterprise Vault.cloud SSO, you need the following items:</span></span>

- <span data-ttu-id="c7840-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c7840-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c7840-113">Egy Veritas vállalati Vault.cloud SSO egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c7840-113">A Veritas Enterprise Vault.cloud SSO single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c7840-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c7840-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c7840-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c7840-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c7840-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c7840-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c7840-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7840-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c7840-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c7840-118">Scenario description</span></span>
<span data-ttu-id="c7840-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c7840-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c7840-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c7840-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c7840-121">A gyűjteményből Veritas vállalati Vault.cloud SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c7840-121">Adding Veritas Enterprise Vault.cloud SSO from the gallery</span></span>
2. <span data-ttu-id="c7840-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c7840-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a><span data-ttu-id="c7840-123">A gyűjteményből Veritas vállalati Vault.cloud SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c7840-123">Adding Veritas Enterprise Vault.cloud SSO from the gallery</span></span>
<span data-ttu-id="c7840-124">Az Azure AD integrálása a Veritas vállalati Vault.cloud SSO konfigurálásához kell hozzáadnia Veritas vállalati Vault.cloud egyszeri Bejelentkezést a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c7840-124">To configure the integration of Veritas Enterprise Vault.cloud SSO into Azure AD, you need to add Veritas Enterprise Vault.cloud SSO from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c7840-125">**A gyűjteményből Veritas vállalati Vault.cloud SSO hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c7840-125">**To add Veritas Enterprise Vault.cloud SSO from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c7840-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c7840-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c7840-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c7840-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c7840-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c7840-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c7840-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c7840-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c7840-133">Írja be a keresőmezőbe, **Veritas vállalati Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="c7840-133">In the search box, type **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. <span data-ttu-id="c7840-135">Az eredmények panelen válassza ki a **Veritas vállalati Vault.cloud SSO**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c7840-135">In the results panel, select **Veritas Enterprise Vault.cloud SSO**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c7840-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c7840-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c7840-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Veritas vállalati Vault.cloud SSO "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="c7840-138">In this section, you configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c7840-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Veritas vállalati Vault.cloud egyszeri Bejelentkezést a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c7840-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Veritas Enterprise Vault.cloud SSO is to a user in Azure AD.</span></span> <span data-ttu-id="c7840-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Veritas vállalati Vault.cloud SSO közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c7840-140">In other words, a link relationship between an Azure AD user and the related user in Veritas Enterprise Vault.cloud SSO needs to be established.</span></span>

<span data-ttu-id="c7840-141">Veritas vállalati Vault.cloud SSO, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c7840-141">In Veritas Enterprise Vault.cloud SSO, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c7840-142">Az Azure AD az egyszeri bejelentkezés Veritas vállalati Vault.cloud egyszeri bejelentkezési modellel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c7840-142">To configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c7840-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c7840-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c7840-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c7840-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c7840-145">**[Veritas vállalati Vault.cloud SSO tesztfelhasználó létrehozása](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  - való egy megfelelője a Britta Simon Veritas vállalati Vault.cloud egyszeri Bejelentkezést, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c7840-145">**[Creating a Veritas Enterprise Vault.cloud SSO test user](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)** - to have a counterpart of Britta Simon in Veritas Enterprise Vault.cloud SSO that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c7840-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c7840-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c7840-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c7840-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c7840-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c7840-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c7840-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Veritas vállalati Vault.cloud SSO-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c7840-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Veritas Enterprise Vault.cloud SSO application.</span></span>

<span data-ttu-id="c7840-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Veritas vállalati Vault.cloud egyszeri bejelentkezési modellel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c7840-150">**To configure Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="c7840-151">Az Azure portálon a a **Veritas vállalati Vault.cloud SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c7840-151">In the Azure portal, on the **Veritas Enterprise Vault.cloud SSO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c7840-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c7840-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. <span data-ttu-id="c7840-155">Az a **Veritas vállalati Vault.cloud SSO tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c7840-155">On the **Veritas Enterprise Vault.cloud SSO Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    <span data-ttu-id="c7840-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span><span class="sxs-lookup"><span data-stu-id="c7840-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c7840-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c7840-158">This value is not real.</span></span> <span data-ttu-id="c7840-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c7840-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="c7840-160">Ügyfél [Veritas vállalati Vault.cloud SSO ügyfél-támogatási csoport](https://www.veritas.com/support/.html) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c7840-160">Contact [Veritas Enterprise Vault.cloud SSO Client support team](https://www.veritas.com/support/.html) to get this value.</span></span> 

4. <span data-ttu-id="c7840-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c7840-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. <span data-ttu-id="c7840-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c7840-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c7840-165">A a **Veritas vállalati Vault.cloud SSO konfigurációs** kattintson **konfigurálása Veritas vállalati Vault.cloud SSO** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c7840-165">On the **Veritas Enterprise Vault.cloud SSO Configuration** section, click **Configure Veritas Enterprise Vault.cloud SSO** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c7840-166">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c7840-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. <span data-ttu-id="c7840-168">Egyszeri bejelentkezés konfigurálása **Veritas vállalati Vault.cloud SSO** oldalon kell küldeniük a letöltött **Certificate(Base64)** és **SAML-alapú egyszeri bejelentkezési URL-címe** a [Veritas vállalati Vault.cloud SSO támogatási csoport](https://www.veritas.com/support/.html).</span><span class="sxs-lookup"><span data-stu-id="c7840-168">To configure single sign-on on **Veritas Enterprise Vault.cloud SSO** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html).</span></span>

> [!TIP]
> <span data-ttu-id="c7840-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c7840-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c7840-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c7840-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c7840-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c7840-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c7840-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7840-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="c7840-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c7840-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c7840-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c7840-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c7840-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c7840-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c7840-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c7840-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c7840-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c7840-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c7840-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c7840-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c7840-184">a.</span><span class="sxs-lookup"><span data-stu-id="c7840-184">a.</span></span> <span data-ttu-id="c7840-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c7840-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c7840-186">b.</span><span class="sxs-lookup"><span data-stu-id="c7840-186">b.</span></span> <span data-ttu-id="c7840-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c7840-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c7840-188">c.</span><span class="sxs-lookup"><span data-stu-id="c7840-188">c.</span></span> <span data-ttu-id="c7840-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c7840-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c7840-190">d.</span><span class="sxs-lookup"><span data-stu-id="c7840-190">d.</span></span> <span data-ttu-id="c7840-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c7840-191">Click **Create**.</span></span>
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a><span data-ttu-id="c7840-192">Veritas vállalati Vault.cloud SSO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7840-192">Creating a Veritas Enterprise Vault.cloud SSO test user</span></span>

<span data-ttu-id="c7840-193">Ebben a szakaszban egy Britta Simon meghívta vállalati Vault.cloud egyszeri Bejelentkezéses felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7840-193">In this section, you create a user called Britta Simon in Enterprise Vault.cloud SSO.</span></span> <span data-ttu-id="c7840-194">Együttműködve [Veritas vállalati Vault.cloud SSO támogatási csoport](https://www.veritas.com/support/.html) hozzáadása a felhasználók a vállalati Vault.cloud SSO platform.</span><span class="sxs-lookup"><span data-stu-id="c7840-194">Work with [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html) to add the users in the Enterprise Vault.cloud SSO platform.</span></span> <span data-ttu-id="c7840-195">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="c7840-195">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c7840-196">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c7840-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c7840-197">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Veritas vállalati Vault.cloud SSO Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c7840-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Veritas Enterprise Vault.cloud SSO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c7840-199">**Britta Simon hozzárendelése Veritas vállalati Vault.cloud SSO, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c7840-199">**To assign Britta Simon to Veritas Enterprise Vault.cloud SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="c7840-200">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c7840-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c7840-202">Az alkalmazások listában válassza ki a **Veritas vállalati Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="c7840-202">In the applications list, select **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. <span data-ttu-id="c7840-204">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c7840-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c7840-206">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c7840-206">Click **Add** button.</span></span> <span data-ttu-id="c7840-207">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c7840-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c7840-209">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c7840-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c7840-210">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c7840-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c7840-211">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c7840-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c7840-212">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c7840-212">Testing single sign-on</span></span>

<span data-ttu-id="c7840-213">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c7840-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c7840-214">Ha a hozzáférési panelen Veritas vállalati Vault.cloud SSO csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Veritas vállalati Vault.cloud SSO alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c7840-214">When you click the Veritas Enterprise Vault.cloud SSO tile in the Access Panel, you should get automatically signed-on to your Veritas Enterprise Vault.cloud SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7840-215">További források</span><span class="sxs-lookup"><span data-stu-id="c7840-215">Additional resources</span></span>

* [<span data-ttu-id="c7840-216">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c7840-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c7840-217">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c7840-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

