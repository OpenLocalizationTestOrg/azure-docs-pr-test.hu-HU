---
title: "Oktatóanyag: Azure Active Directoryval integrált eDigitalResearch |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és eDigitalResearch között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c6b66ea0-16ba-45b4-b550-e81c56262b1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: f877a1dd844c40c913f3121e5288952653c312cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-edigitalresearch"></a><span data-ttu-id="d9a61-103">Oktatóanyag: Azure Active Directoryval integrált eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="d9a61-103">Tutorial: Azure Active Directory integration with eDigitalResearch</span></span>

<span data-ttu-id="d9a61-104">Ebben az oktatóanyagban elsajátíthatja eDigitalResearch integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d9a61-104">In this tutorial, you learn how to integrate eDigitalResearch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9a61-105">EDigitalResearch integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d9a61-105">Integrating eDigitalResearch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d9a61-106">Az Azure AD, aki hozzáfér eDigitalResearch szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="d9a61-106">You can control in Azure AD who has access to eDigitalResearch.</span></span>
- <span data-ttu-id="d9a61-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett eDigitalResearch (egyszeri bejelentkezés) a saját Azure AD-fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="d9a61-107">You can enable your users to automatically get signed-on to eDigitalResearch (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d9a61-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d9a61-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d9a61-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9a61-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9a61-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9a61-110">Prerequisites</span></span>

<span data-ttu-id="d9a61-111">Konfigurálása az Azure AD-integrációs eDigitalResearch, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d9a61-111">To configure Azure AD integration with eDigitalResearch, you need the following items:</span></span>

- <span data-ttu-id="d9a61-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d9a61-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d9a61-113">Egy eDigitalResearch egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d9a61-113">A eDigitalResearch single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9a61-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d9a61-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9a61-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d9a61-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9a61-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d9a61-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d9a61-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9a61-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9a61-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d9a61-118">Scenario description</span></span>
<span data-ttu-id="d9a61-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d9a61-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9a61-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d9a61-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9a61-121">A gyűjteményből eDigitalResearch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9a61-121">Adding eDigitalResearch from the gallery</span></span>
2. <span data-ttu-id="d9a61-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d9a61-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-edigitalresearch-from-the-gallery"></a><span data-ttu-id="d9a61-123">A gyűjteményből eDigitalResearch hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9a61-123">Adding eDigitalResearch from the gallery</span></span>
<span data-ttu-id="d9a61-124">Az Azure AD integrálása a eDigitalResearch konfigurálásához kell hozzáadnia eDigitalResearch a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d9a61-124">To configure the integration of eDigitalResearch into Azure AD, you need to add eDigitalResearch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d9a61-125">**A gyűjteményből eDigitalResearch hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d9a61-125">**To add eDigitalResearch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d9a61-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="d9a61-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d9a61-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="d9a61-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="d9a61-133">Írja be a keresőmezőbe, **eDigitalResearch**, jelölje be **eDigitalResearch** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d9a61-133">In the search box, type **eDigitalResearch**, select **eDigitalResearch** from result panel then click **Add** button to add the application.</span></span>

    ![az eredménylistában eDigitalResearch](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d9a61-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d9a61-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d9a61-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="d9a61-136">In this section, you configure and test Azure AD single sign-on with eDigitalResearch based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d9a61-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó eDigitalResearch a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d9a61-137">For single sign-on to work, Azure AD needs to know what the counterpart user in eDigitalResearch is to a user in Azure AD.</span></span> <span data-ttu-id="d9a61-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a eDigitalResearch közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d9a61-138">In other words, a link relationship between an Azure AD user and the related user in eDigitalResearch needs to be established.</span></span>

<span data-ttu-id="d9a61-139">EDigitalResearch, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d9a61-139">In eDigitalResearch, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d9a61-140">Az Azure AD egyszeri bejelentkezést a eDigitalResearch tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d9a61-140">To configure and test Azure AD single sign-on with eDigitalResearch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d9a61-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d9a61-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d9a61-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d9a61-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9a61-143">**[EDigitalResearch tesztfelhasználó létrehozása](#create-a-edigitalresearch-test-user)**  - Britta Simon egy partner, a felhasználó az Azure AD ábrázolását kapcsolódó eDigitalResearch rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d9a61-143">**[Create a eDigitalResearch test user](#create-a-edigitalresearch-test-user)** - to have a counterpart of Britta Simon in eDigitalResearch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d9a61-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d9a61-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9a61-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  ellenőrzése, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d9a61-145">**[Test single sign-on](#test-single-sign-on)**  to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d9a61-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d9a61-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d9a61-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az eDigitalResearch alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d9a61-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eDigitalResearch application.</span></span>

<span data-ttu-id="d9a61-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés eDigitalResearch, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d9a61-148">**To configure Azure AD single sign-on with eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="d9a61-149">Az Azure portálon a a **eDigitalResearch** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-149">In the Azure portal, on the **eDigitalResearch** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="d9a61-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d9a61-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_samlbase.png)

3. <span data-ttu-id="d9a61-153">Az a **eDigitalResearch tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d9a61-153">On the **eDigitalResearch Domain and URLs** section, perform the following steps:</span></span>

    ![Tartomány- és URL-címek egyetlen bejelentkezési adatokat eDigitalResearch](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_url.png)

    <span data-ttu-id="d9a61-155">a.</span><span class="sxs-lookup"><span data-stu-id="d9a61-155">a.</span></span> <span data-ttu-id="d9a61-156">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company-name>.edigitalresearch.com`</span><span class="sxs-lookup"><span data-stu-id="d9a61-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com`</span></span>

    <span data-ttu-id="d9a61-157">b.</span><span class="sxs-lookup"><span data-stu-id="d9a61-157">b.</span></span> <span data-ttu-id="d9a61-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company-name>.edigitalresearch.com/login/consume`</span><span class="sxs-lookup"><span data-stu-id="d9a61-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com/login/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d9a61-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d9a61-159">These values are not real.</span></span> <span data-ttu-id="d9a61-160">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="d9a61-160">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="d9a61-161">Ügyfél [eDigitalResearch támogatási csoport](http://www.maruedr.com/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d9a61-161">Contact [eDigitalResearch support team](http://www.maruedr.com/contact) to get these values.</span></span>
 


4. <span data-ttu-id="d9a61-162">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány Base(64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d9a61-162">On the **SAML Signing Certificate** section, click **Certificate Base(64)** and then save the certificate file on your computer.</span></span>

    <span data-ttu-id="d9a61-163">!</span><span class="sxs-lookup"><span data-stu-id="d9a61-163">!</span></span>![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_certificate.png) 

5. <span data-ttu-id="d9a61-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d9a61-167">A a **eDigitalResearch konfigurációs** kattintson **eDigitalResearch konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d9a61-167">On the **eDigitalResearch Configuration** section, click **Configure eDigitalResearch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d9a61-168">Másolás a **Sign-Out URL, SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d9a61-168">Copy the **Sign-Out URL, SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurációs eDigitalResearch](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_configure.png) 

7. <span data-ttu-id="d9a61-170">Egyszeri bejelentkezés konfigurálása **eDigitalResearch** oldalon kell küldeniük a letöltött **(Base64) tanúsítványfájl**, **SAML Entitásazonosító**, és **kijelentkezési URL-cím** való [eDigitalResearch támogatási csoport](http://www.maruedr.com/contact).</span><span class="sxs-lookup"><span data-stu-id="d9a61-170">To configure single sign-on on **eDigitalResearch** side, you need to send the downloaded **Certificate (Base64) File**, **SAML Entity ID**, and **Sign-Out URL** to [eDigitalResearch support team](http://www.maruedr.com/contact).</span></span> <span data-ttu-id="d9a61-171">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d9a61-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d9a61-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d9a61-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d9a61-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d9a61-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d9a61-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9a61-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d9a61-175">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d9a61-175">Create an Azure AD test user</span></span>

<span data-ttu-id="d9a61-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d9a61-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="d9a61-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d9a61-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d9a61-179">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-179">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d9a61-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-181">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d9a61-183">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d9a61-183">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d9a61-185">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d9a61-185">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d9a61-187">a.</span><span class="sxs-lookup"><span data-stu-id="d9a61-187">a.</span></span> <span data-ttu-id="d9a61-188">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-188">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9a61-189">b.</span><span class="sxs-lookup"><span data-stu-id="d9a61-189">b.</span></span> <span data-ttu-id="d9a61-190">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9a61-190">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d9a61-191">c.</span><span class="sxs-lookup"><span data-stu-id="d9a61-191">c.</span></span> <span data-ttu-id="d9a61-192">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d9a61-192">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d9a61-193">d.</span><span class="sxs-lookup"><span data-stu-id="d9a61-193">d.</span></span> <span data-ttu-id="d9a61-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-194">Click **Create**.</span></span>
  
### <a name="create-a-edigitalresearch-test-user"></a><span data-ttu-id="d9a61-195">EDigitalResearch tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9a61-195">Create a eDigitalResearch test user</span></span>

<span data-ttu-id="d9a61-196">Ez a szakasz célja eDigitalResearch Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d9a61-196">The objective of this section is to create a user called Britta Simon in eDigitalResearch.</span></span> 

<span data-ttu-id="d9a61-197">Együttműködik az [eDigitalResearch támogatási csoport](http://www.maruedr.com/contact) megszerezni a felhasználó hozott létre.</span><span class="sxs-lookup"><span data-stu-id="d9a61-197">Work with the [eDigitalResearch support team](http://www.maruedr.com/contact) to get users created.</span></span>     
    
 > [!NOTE]
 > <span data-ttu-id="d9a61-198">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="d9a61-198">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d9a61-199">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d9a61-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="d9a61-200">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó eDigitalResearch való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="d9a61-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eDigitalResearch.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="d9a61-202">**Britta Simon hozzárendelése eDigitalResearch, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d9a61-202">**To assign Britta Simon to eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="d9a61-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d9a61-205">Az alkalmazások listában válassza ki a **eDigitalResearch**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-205">In the applications list, select **eDigitalResearch**.</span></span>

    ![Az alkalmazások listáját a eDigitalResearch hivatkozás](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_app.png)  

3. <span data-ttu-id="d9a61-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d9a61-207">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="d9a61-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d9a61-209">Click **Add** button.</span></span> <span data-ttu-id="d9a61-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9a61-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="d9a61-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d9a61-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d9a61-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9a61-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9a61-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d9a61-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d9a61-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d9a61-215">Test single sign-on</span></span>

<span data-ttu-id="d9a61-216">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d9a61-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d9a61-217">Ha a hozzáférési panelen eDigitalResearch csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az eDigitalResearch alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="d9a61-217">When you click the eDigitalResearch tile in the Access Panel, you should get automatically signed-on to your eDigitalResearch application.</span></span>
<span data-ttu-id="d9a61-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d9a61-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d9a61-219">További források</span><span class="sxs-lookup"><span data-stu-id="d9a61-219">Additional resources</span></span>

* [<span data-ttu-id="d9a61-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d9a61-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9a61-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d9a61-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png

