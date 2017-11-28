---
title: "Oktatóanyag: Azure Active Directory integrációja a biztonságos FÁJLMEGOSZTÁSBA |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a biztonságos BIZTOSÍTÁSÁHOZ között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: f6e5b1e34893f6b8fe14e238e24086bb47d009a5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a><span data-ttu-id="f54ab-103">Oktatóanyag: Azure Active Directory integrációja a biztonságos BIZTOSÍTÁSÁHOZ</span><span class="sxs-lookup"><span data-stu-id="f54ab-103">Tutorial: Azure Active Directory integration with SECURE DELIVER</span></span>

<span data-ttu-id="f54ab-104">Ebben az oktatóanyagban elsajátíthatja az Azure Active Directoryval (Azure AD) integrálása a biztonságos BIZTOSÍTÁSÁHOZ.</span><span class="sxs-lookup"><span data-stu-id="f54ab-104">In this tutorial, you learn how to integrate SECURE DELIVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f54ab-105">BIZTONSÁGOS FÁJLMEGOSZTÁSBA integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f54ab-105">Integrating SECURE DELIVER with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f54ab-106">Megadhatja a képes biztosítani a biztonságos hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f54ab-106">You can control in Azure AD who has access to SECURE DELIVER</span></span>
- <span data-ttu-id="f54ab-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett (egyszeri bejelentkezés) biztonságos továbbítani a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f54ab-107">You can enable your users to automatically get signed-on to SECURE DELIVER (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f54ab-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f54ab-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f54ab-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f54ab-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f54ab-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f54ab-110">Prerequisites</span></span>

<span data-ttu-id="f54ab-111">Konfigurálása az Azure AD-integrációs biztonságos KÉZBESÍTI, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f54ab-111">To configure Azure AD integration with SECURE DELIVER, you need the following items:</span></span>

- <span data-ttu-id="f54ab-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f54ab-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f54ab-113">A biztonságos KÉZBESÍTI az egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f54ab-113">A SECURE DELIVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f54ab-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f54ab-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f54ab-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f54ab-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f54ab-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f54ab-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f54ab-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f54ab-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f54ab-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f54ab-118">Scenario description</span></span>
<span data-ttu-id="f54ab-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f54ab-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f54ab-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f54ab-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f54ab-121">BIZTONSÁGOS FÁJLMEGOSZTÁSBA hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f54ab-121">Adding SECURE DELIVER from the gallery</span></span>
2. <span data-ttu-id="f54ab-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f54ab-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-secure-deliver-from-the-gallery"></a><span data-ttu-id="f54ab-123">BIZTONSÁGOS FÁJLMEGOSZTÁSBA hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f54ab-123">Adding SECURE DELIVER from the gallery</span></span>
<span data-ttu-id="f54ab-124">Konfigurálhatja az Azure AD integrálása a biztonságos KÉZBESÍTI, kell hozzáadnia biztonságos BIZTOSÍTANAK a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f54ab-124">To configure the integration of SECURE DELIVER into Azure AD, you need to add SECURE DELIVER from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f54ab-125">**Adja hozzá a biztonságos BIZTOSÍTANAK a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f54ab-125">**To add SECURE DELIVER from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f54ab-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f54ab-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f54ab-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f54ab-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f54ab-133">Írja be a keresőmezőbe, **biztonságos FÁJLMEGOSZTÁSBA**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-133">In the search box, type **SECURE DELIVER**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_search.png)

5. <span data-ttu-id="f54ab-135">Az eredmények panelen válassza ki a **biztonságos FÁJLMEGOSZTÁSBA**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f54ab-135">In the results panel, select **SECURE DELIVER**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f54ab-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f54ab-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f54ab-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján biztonságos BIZTOSÍTÁSÁHOZ.</span><span class="sxs-lookup"><span data-stu-id="f54ab-138">In this section, you configure and test Azure AD single sign-on with SECURE DELIVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f54ab-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó biztonságos BIZTOSÍTANAK a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f54ab-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SECURE DELIVER is to a user in Azure AD.</span></span> <span data-ttu-id="f54ab-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a biztonságos FÁJLMEGOSZTÁSBA közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f54ab-140">In other words, a link relationship between an Azure AD user and the related user in SECURE DELIVER needs to be established.</span></span>

<span data-ttu-id="f54ab-141">BIZTONSÁGOS KÉZBESÍTI, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f54ab-141">In SECURE DELIVER, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f54ab-142">Az Azure AD egyszeri bejelentkezést a biztonságos FÁJLMEGOSZTÁSBA tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f54ab-142">To configure and test Azure AD single sign-on with SECURE DELIVER, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f54ab-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f54ab-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f54ab-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f54ab-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f54ab-145">**[BIZTONSÁGOS FÁJLMEGOSZTÁSBA tesztfelhasználó létrehozása](#creating-a-secure-deliver-test-user)**  - való egy megfelelője a Britta Simon biztonságos BIZTOSÍTANAK, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f54ab-145">**[Creating a SECURE DELIVER test user](#creating-a-secure-deliver-test-user)** - to have a counterpart of Britta Simon in SECURE DELIVER that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f54ab-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f54ab-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f54ab-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f54ab-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f54ab-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f54ab-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f54ab-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és biztonságos FÁJLMEGOSZTÁSBA alkalmazásában egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f54ab-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SECURE DELIVER application.</span></span>

<span data-ttu-id="f54ab-150">**BIZTONSÁGOS FÁJLMEGOSZTÁSBA konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f54ab-150">**To configure Azure AD single sign-on with SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="f54ab-151">Az Azure portálon a a **biztonságos FÁJLMEGOSZTÁSBA** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-151">In the Azure portal, on the **SECURE DELIVER** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f54ab-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f54ab-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

3. <span data-ttu-id="f54ab-155">Az a **biztonságos FÁJLMEGOSZTÁSBA tartománya és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f54ab-155">On the **SECURE DELIVER Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_url.png)

    <span data-ttu-id="f54ab-157">a.</span><span class="sxs-lookup"><span data-stu-id="f54ab-157">a.</span></span> <span data-ttu-id="f54ab-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span><span class="sxs-lookup"><span data-stu-id="f54ab-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span></span>

    <span data-ttu-id="f54ab-159">b.</span><span class="sxs-lookup"><span data-stu-id="f54ab-159">b.</span></span> <span data-ttu-id="f54ab-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span><span class="sxs-lookup"><span data-stu-id="f54ab-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f54ab-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f54ab-161">These values are not real.</span></span> <span data-ttu-id="f54ab-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f54ab-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f54ab-163">Ügyfél [FÁJLMEGOSZTÁSBA ügyfél biztonságos támogatási csoport](mailto:iw-sd-support@fujifilm.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f54ab-163">Contact [SECURE DELIVER Client support team](mailto:iw-sd-support@fujifilm.com) to get these values.</span></span> 
 
4. <span data-ttu-id="f54ab-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f54ab-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

5. <span data-ttu-id="f54ab-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f54ab-168">Az a **BIZTOSÍTANAK konfigurációs biztonságos** területen kattintson **konfigurálása biztonságos FÁJLMEGOSZTÁSBA** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f54ab-168">On the **SECURE DELIVER Configuration** section, click **Configure SECURE DELIVER** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f54ab-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f54ab-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_configure.png) 

7. <span data-ttu-id="f54ab-171">Egyszeri bejelentkezés konfigurálása **biztonságos FÁJLMEGOSZTÁSBA** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [támogatási csoport biztonságos FÁJLMEGOSZTÁSBA](mailto:iw-sd-support@fujifilm.com).</span><span class="sxs-lookup"><span data-stu-id="f54ab-171">To configure single sign-on on **SECURE DELIVER** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com).</span></span> <span data-ttu-id="f54ab-172">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="f54ab-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f54ab-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f54ab-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f54ab-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f54ab-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f54ab-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f54ab-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f54ab-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f54ab-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="f54ab-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f54ab-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f54ab-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f54ab-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f54ab-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f54ab-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f54ab-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f54ab-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f54ab-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f54ab-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f54ab-188">a.</span><span class="sxs-lookup"><span data-stu-id="f54ab-188">a.</span></span> <span data-ttu-id="f54ab-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f54ab-190">b.</span><span class="sxs-lookup"><span data-stu-id="f54ab-190">b.</span></span> <span data-ttu-id="f54ab-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f54ab-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f54ab-192">c.</span><span class="sxs-lookup"><span data-stu-id="f54ab-192">c.</span></span> <span data-ttu-id="f54ab-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f54ab-194">d.</span><span class="sxs-lookup"><span data-stu-id="f54ab-194">d.</span></span> <span data-ttu-id="f54ab-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-195">Click **Create**.</span></span>
 
### <a name="creating-a-secure-deliver-test-user"></a><span data-ttu-id="f54ab-196">BIZTONSÁGOS FÁJLMEGOSZTÁSBA tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f54ab-196">Creating a SECURE DELIVER test user</span></span>

<span data-ttu-id="f54ab-197">Ez a szakasz célja Britta Simon meghívta biztonságos FÁJLMEGOSZTÁSBA felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f54ab-197">The objective of this section is to create a user called Britta Simon in SECURE DELIVER.</span></span> <span data-ttu-id="f54ab-198">Együttműködve [támogatási csoport biztonságos FÁJLMEGOSZTÁSBA](mailto:iw-sd-support@fujifilm.com) a felhasználók hozzáadása a FÁJLMEGOSZTÁSBA biztonságos fiók.</span><span class="sxs-lookup"><span data-stu-id="f54ab-198">Work with [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com) to add the users in the SECURE DELIVER account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f54ab-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f54ab-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f54ab-200">Ebben a szakaszban engedélyezze Britta Simon szerint képes biztosítani a biztonságos hozzáférés biztosítása az Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f54ab-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SECURE DELIVER.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f54ab-202">**Képes biztosítani a biztonságos Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="f54ab-202">**To assign Britta Simon to SECURE DELIVER, perform the following steps:**</span></span>

1. <span data-ttu-id="f54ab-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f54ab-205">Az alkalmazások listában válassza ki a **biztonságos FÁJLMEGOSZTÁSBA**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-205">In the applications list, select **SECURE DELIVER**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_app.png) 

3. <span data-ttu-id="f54ab-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f54ab-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f54ab-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f54ab-209">Click **Add** button.</span></span> <span data-ttu-id="f54ab-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f54ab-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f54ab-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f54ab-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f54ab-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f54ab-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f54ab-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f54ab-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f54ab-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f54ab-215">Testing single sign-on</span></span>

<span data-ttu-id="f54ab-216">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f54ab-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="f54ab-217">Ha a hozzáférési Panel egy FÁJLMEGOSZTÁSBA biztonságos mozaik gombra kattint, meg kell beolvasása automatikusan bejelentkezett a biztonságos FÁJLMEGOSZTÁSBA alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="f54ab-217">When you click the SECURE DELIVER tile in the Access Panel, you should get automatically signed-on to your SECURE DELIVER application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f54ab-218">További források</span><span class="sxs-lookup"><span data-stu-id="f54ab-218">Additional resources</span></span>

* [<span data-ttu-id="f54ab-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f54ab-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f54ab-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f54ab-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png

