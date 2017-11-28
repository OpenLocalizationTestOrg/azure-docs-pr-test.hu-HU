---
title: "Oktatóanyag: Azure Active Directory-integrációval és a csendes-óceáni időrendben |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a csendes-óceáni időrendben között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: fda06c340430d19bea035a2cab2f318fe8a5998c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a><span data-ttu-id="abe7a-103">Oktatóanyag: Azure Active Directory-integrációval és a csendes-óceáni időrendben</span><span class="sxs-lookup"><span data-stu-id="abe7a-103">Tutorial: Azure Active Directory integration with Pacific Timesheet</span></span>

<span data-ttu-id="abe7a-104">Ebben az oktatóanyagban elsajátíthatja a csendes-óceáni időrendben integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="abe7a-104">In this tutorial, you learn how to integrate Pacific Timesheet with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="abe7a-105">Csendes-óceáni időrendben integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="abe7a-105">Integrating Pacific Timesheet with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="abe7a-106">Megadhatja a csendes-óceáni időrendben hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="abe7a-106">You can control in Azure AD who has access to Pacific Timesheet</span></span>
- <span data-ttu-id="abe7a-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett csendes-óceáni időrendbe (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="abe7a-107">You can enable your users to automatically get signed-on to Pacific Timesheet (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="abe7a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="abe7a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="abe7a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="abe7a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abe7a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="abe7a-110">Prerequisites</span></span>

<span data-ttu-id="abe7a-111">Az Azure AD-integráció konfigurálása a csendes-óceáni munkaidő-nyilvántartási, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="abe7a-111">To configure Azure AD integration with Pacific Timesheet, you need the following items:</span></span>

- <span data-ttu-id="abe7a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="abe7a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="abe7a-113">A csendes-óceáni időrendben egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="abe7a-113">A Pacific Timesheet single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="abe7a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="abe7a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="abe7a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="abe7a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="abe7a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="abe7a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="abe7a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abe7a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="abe7a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="abe7a-118">Scenario description</span></span>
<span data-ttu-id="abe7a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="abe7a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="abe7a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="abe7a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="abe7a-121">Csendes-óceáni időrendben hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="abe7a-121">Adding Pacific Timesheet from the gallery</span></span>
2. <span data-ttu-id="abe7a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="abe7a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pacific-timesheet-from-the-gallery"></a><span data-ttu-id="abe7a-123">Csendes-óceáni időrendben hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="abe7a-123">Adding Pacific Timesheet from the gallery</span></span>
<span data-ttu-id="abe7a-124">Az Azure AD integrálása a csendes-óceáni időrendben konfigurálásához kell hozzáadnia csendes-óceáni időrendben a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="abe7a-124">To configure the integration of Pacific Timesheet into Azure AD, you need to add Pacific Timesheet from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="abe7a-125">**Adja hozzá a csendes-óceáni időrendben a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="abe7a-125">**To add Pacific Timesheet from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="abe7a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="abe7a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="abe7a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="abe7a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="abe7a-133">Írja be a keresőmezőbe, **csendes-óceáni időrendben**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-133">In the search box, type **Pacific Timesheet**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

5. <span data-ttu-id="abe7a-135">Az eredmények panelen válassza ki a **csendes-óceáni időrendben**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="abe7a-135">In the results panel, select **Pacific Timesheet**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="abe7a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="abe7a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="abe7a-138">Ebben a szakaszban konfigurálása, és a csendes-óceáni munkaidő-nyilvántartási "Britta Simon" nevű tesztfelhasználó alapján az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="abe7a-138">In this section, you configure and test Azure AD single sign-on with Pacific Timesheet based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="abe7a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a csendes-óceáni időrendben megfelelőjére felhasználó a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="abe7a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pacific Timesheet is to a user in Azure AD.</span></span> <span data-ttu-id="abe7a-140">Ez azt jelenti az Azure AD-felhasználó és a csendes-óceáni időrendben a kapcsolódó felhasználó közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="abe7a-140">In other words, a link relationship between an Azure AD user and the related user in Pacific Timesheet needs to be established.</span></span>

<span data-ttu-id="abe7a-141">Csendes-óceáni időrendben, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="abe7a-141">In Pacific Timesheet, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="abe7a-142">Az Azure AD egyszeri bejelentkezést és a csendes-óceáni időrendben tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="abe7a-142">To configure and test Azure AD single sign-on with Pacific Timesheet, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="abe7a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="abe7a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="abe7a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="abe7a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="abe7a-145">**[Csendes-óceáni időrendben tesztfelhasználó létrehozása](#creating-a-pacific-timesheet-test-user)**  - kell rendelkeznie a Britta Simon megfelelője a csendes-óceáni időrendben, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="abe7a-145">**[Creating a Pacific Timesheet test user](#creating-a-pacific-timesheet-test-user)** - to have a counterpart of Britta Simon in Pacific Timesheet that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="abe7a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="abe7a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="abe7a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="abe7a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="abe7a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="abe7a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="abe7a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a csendes-óceáni időrendben alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="abe7a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pacific Timesheet application.</span></span>

<span data-ttu-id="abe7a-150">**Konfigurálja az Azure AD egyszeri bejelentkezést és a csendes-óceáni munkaidő-nyilvántartási, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="abe7a-150">**To configure Azure AD single sign-on with Pacific Timesheet, perform the following steps:**</span></span>

1. <span data-ttu-id="abe7a-151">Az Azure portálon a a **csendes-óceáni időrendben** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-151">In the Azure portal, on the **Pacific Timesheet** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="abe7a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="abe7a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

3. <span data-ttu-id="abe7a-155">Az a **csendes-óceáni időrendben tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="abe7a-155">On the **Pacific Timesheet Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    <span data-ttu-id="abe7a-157">a.</span><span class="sxs-lookup"><span data-stu-id="abe7a-157">a.</span></span> <span data-ttu-id="abe7a-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="abe7a-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    <span data-ttu-id="abe7a-159">b.</span><span class="sxs-lookup"><span data-stu-id="abe7a-159">b.</span></span> <span data-ttu-id="abe7a-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="abe7a-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="abe7a-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="abe7a-161">These values are not real.</span></span> <span data-ttu-id="abe7a-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="abe7a-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="abe7a-163">Ügyfél [csendes-óceáni időrendben támogatási csoport](http://www.pacifictimesheet.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="abe7a-163">Contact [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) to get these values.</span></span>
 
4. <span data-ttu-id="abe7a-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="abe7a-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

5. <span data-ttu-id="abe7a-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="abe7a-168">Az a **csendes-óceáni időrendben konfigurációs** területén kattintson **konfigurálása a csendes-óceáni munkaidő-nyilvántartási** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="abe7a-168">On the **Pacific Timesheet Configuration** section, click **Configure Pacific Timesheet** to open **Configure sign-on** window.</span></span> <span data-ttu-id="abe7a-169">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="abe7a-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

7. <span data-ttu-id="abe7a-171">Egyszeri bejelentkezés konfigurálása **csendes-óceáni időrendben** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **SAML-alapú egyszeri bejelentkezési URL-címe**, és **SAML Entitásazonosító** való [csendes-óceáni időrendben támogatási csoport](http://www.pacifictimesheet.com/support).</span><span class="sxs-lookup"><span data-stu-id="abe7a-171">To configure single sign-on on **Pacific Timesheet** side, you need to send the downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Pacific Timesheet support team](http://www.pacifictimesheet.com/support).</span></span> <span data-ttu-id="abe7a-172">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="abe7a-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="abe7a-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="abe7a-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="abe7a-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="abe7a-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="abe7a-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="abe7a-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="abe7a-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="abe7a-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="abe7a-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="abe7a-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="abe7a-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="abe7a-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="abe7a-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="abe7a-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="abe7a-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="abe7a-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="abe7a-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="abe7a-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="abe7a-188">a.</span><span class="sxs-lookup"><span data-stu-id="abe7a-188">a.</span></span> <span data-ttu-id="abe7a-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="abe7a-190">b.</span><span class="sxs-lookup"><span data-stu-id="abe7a-190">b.</span></span> <span data-ttu-id="abe7a-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="abe7a-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="abe7a-192">c.</span><span class="sxs-lookup"><span data-stu-id="abe7a-192">c.</span></span> <span data-ttu-id="abe7a-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="abe7a-194">d.</span><span class="sxs-lookup"><span data-stu-id="abe7a-194">d.</span></span> <span data-ttu-id="abe7a-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-195">Click **Create**.</span></span>
 
### <a name="creating-a-pacific-timesheet-test-user"></a><span data-ttu-id="abe7a-196">Csendes-óceáni időrendben tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="abe7a-196">Creating a Pacific Timesheet test user</span></span>

<span data-ttu-id="abe7a-197">Ebben a szakaszban a csendes-óceáni időrendben Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="abe7a-197">In this section, you create a user called Britta Simon in Pacific Timesheet.</span></span> <span data-ttu-id="abe7a-198">Együttműködve [csendes-óceáni időrendben támogatási csoport](http://www.pacifictimesheet.com/support) felhasználó létrehozása az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="abe7a-198">Work with [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) to create a user in the application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="abe7a-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="abe7a-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="abe7a-200">Ebben a szakaszban Azure egyszeri bejelentkezéshez használandó hozzáférés biztosítása a csendes-óceáni időrendbe Britta Simon engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="abe7a-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pacific Timesheet.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="abe7a-202">**Csendes-óceáni időrendbe Britta Simon hozzárendeléséhez a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="abe7a-202">**To assign Britta Simon to Pacific Timesheet, perform the following steps:**</span></span>

1. <span data-ttu-id="abe7a-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="abe7a-205">Az alkalmazások listában válassza ki a **csendes-óceáni időrendben**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-205">In the applications list, select **Pacific Timesheet**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

3. <span data-ttu-id="abe7a-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="abe7a-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="abe7a-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="abe7a-209">Click **Add** button.</span></span> <span data-ttu-id="abe7a-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abe7a-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="abe7a-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="abe7a-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="abe7a-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abe7a-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="abe7a-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="abe7a-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="abe7a-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="abe7a-215">Testing single sign-on</span></span>

<span data-ttu-id="abe7a-216">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="abe7a-216">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="abe7a-217">Ha a hozzáférési panelen a csendes-óceáni időrendben csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a csendes-óceáni időrendben alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="abe7a-217">When you click the Pacific Timesheet tile in the Access Panel, you should get automatically signed-on to your Pacific Timesheet application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abe7a-218">További források</span><span class="sxs-lookup"><span data-stu-id="abe7a-218">Additional resources</span></span>

* [<span data-ttu-id="abe7a-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="abe7a-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="abe7a-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="abe7a-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png

