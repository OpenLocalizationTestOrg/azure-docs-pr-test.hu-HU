---
title: "Oktatóanyag: Azure Active Directoryval integrált az igény szerinti Proofpoint |} Microsoft Docs"
description: "Megtudhatja, hogyan igény szerinti konfigurálása egyszeri bejelentkezés Azure Active Directory és Proofpoint között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b4c8d8c187fc865a905016f04a41843894249f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a><span data-ttu-id="23940-103">Oktatóanyag: Azure Active Directoryval integrált Proofpoint igény szerint</span><span class="sxs-lookup"><span data-stu-id="23940-103">Tutorial: Azure Active Directory integration with Proofpoint on Demand</span></span>

<span data-ttu-id="23940-104">Ebben az oktatóanyagban elsajátíthatja az igény szerinti Proofpoint integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="23940-104">In this tutorial, you learn how to integrate Proofpoint on Demand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="23940-105">Az igény szerinti Proofpoint integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="23940-105">Integrating Proofpoint on Demand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="23940-106">Szabályozhatja, aki hozzáfér az igény szerinti Proofpoint Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="23940-106">You can control in Azure AD who has access to Proofpoint on Demand</span></span>
- <span data-ttu-id="23940-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Proofpoint az igény szerinti (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="23940-107">You can enable your users to automatically get signed-on to Proofpoint on Demand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="23940-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="23940-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="23940-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="23940-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23940-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23940-110">Prerequisites</span></span>

<span data-ttu-id="23940-111">Az Azure AD rendszerrel történő integráció konfigurálása az igény szerinti Proofpoint, szüksége van a következőkre:</span><span class="sxs-lookup"><span data-stu-id="23940-111">To configure Azure AD integration with Proofpoint on Demand, you need the following items:</span></span>

- <span data-ttu-id="23940-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="23940-112">An Azure AD subscription</span></span>
- <span data-ttu-id="23940-113">Egy igény szerinti egyszeri bejelentkezés engedélyezve van az előfizetésben lévő Proofpoint</span><span class="sxs-lookup"><span data-stu-id="23940-113">A Proofpoint on Demand single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="23940-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="23940-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="23940-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="23940-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="23940-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="23940-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="23940-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23940-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="23940-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="23940-118">Scenario description</span></span>
<span data-ttu-id="23940-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="23940-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="23940-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="23940-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="23940-121">Proofpoint igény szerinti hozzáadásával a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="23940-121">Adding Proofpoint on Demand from the gallery</span></span>
2. <span data-ttu-id="23940-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="23940-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a><span data-ttu-id="23940-123">Proofpoint igény szerinti hozzáadásával a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="23940-123">Adding Proofpoint on Demand from the gallery</span></span>
<span data-ttu-id="23940-124">Proofpoint integrálása igény szerint az Azure AD-be történő konfigurálásához szüksége a katalógusban az igény szerinti Proofpoint hozzáadása a kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="23940-124">To configure the integration of Proofpoint on Demand into Azure AD, you need to add Proofpoint on Demand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="23940-125">**Adja hozzá az igény szerinti Proofpoint a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="23940-125">**To add Proofpoint on Demand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="23940-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="23940-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="23940-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="23940-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="23940-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="23940-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="23940-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="23940-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="23940-133">Írja be a keresőmezőbe, **Proofpoint az igény szerinti**.</span><span class="sxs-lookup"><span data-stu-id="23940-133">In the search box, type **Proofpoint on Demand**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. <span data-ttu-id="23940-135">Az eredmények panelen válassza ki a **az igény szerinti Proofpoint**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="23940-135">In the results panel, select **Proofpoint on Demand**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="23940-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="23940-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="23940-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Proofpoint az igény szerinti "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="23940-138">In this section, you configure and test Azure AD single sign-on with Proofpoint on Demand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="23940-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Proofpoint az igény szerinti megfelelője felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="23940-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Proofpoint on Demand is to a user in Azure AD.</span></span> <span data-ttu-id="23940-140">Ez azt jelenti az igény szerinti Proofpoint kapcsolódó felhasználói, valamint az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="23940-140">In other words, a link relationship between an Azure AD user and the related user in Proofpoint on Demand needs to be established.</span></span>

<span data-ttu-id="23940-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Proofpoint igény szerint a.</span><span class="sxs-lookup"><span data-stu-id="23940-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Proofpoint on Demand.</span></span>

<span data-ttu-id="23940-142">Az Azure AD egyszeri bejelentkezést az igény szerinti Proofpoint tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="23940-142">To configure and test Azure AD single sign-on with Proofpoint on Demand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="23940-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="23940-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="23940-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="23940-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="23940-145">**[Igény szerinti tesztfelhasználó egy Proofpoint létrehozását](#creating-a-proofpoint-on-demand-test-user)**  - való egy megfelelője a Britta Simon Proofpoint igény szerint, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="23940-145">**[Creating a Proofpoint on Demand test user](#creating-a-proofpoint-on-demand-test-user)** - to have a counterpart of Britta Simon in Proofpoint on Demand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="23940-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="23940-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="23940-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="23940-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="23940-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23940-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="23940-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az igény szerinti alkalmazástól Proofpoint egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="23940-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Proofpoint on Demand application.</span></span>

<span data-ttu-id="23940-150">**Az igény szerinti Proofpoint konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="23940-150">**To configure Azure AD single sign-on with Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="23940-151">Az Azure portálon a a **az igény szerinti Proofpoint** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="23940-151">In the Azure portal, on the **Proofpoint on Demand** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="23940-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="23940-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
  
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. <span data-ttu-id="23940-155">Az a **Proofpoint igény szerinti tartomány és az URL-címekről** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="23940-155">On the **Proofpoint on Demand Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    <span data-ttu-id="23940-157">a.In a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<hostname>.pphosted.com/ppssamlsp_hostname`</span><span class="sxs-lookup"><span data-stu-id="23940-157">a.In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp_hostname`</span></span>

    <span data-ttu-id="23940-158">b.</span><span class="sxs-lookup"><span data-stu-id="23940-158">b.</span></span> <span data-ttu-id="23940-159">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<hostname>.pphosted.com/ppssamlsp`</span><span class="sxs-lookup"><span data-stu-id="23940-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp`</span></span>

    <span data-ttu-id="23940-160">c.</span><span class="sxs-lookup"><span data-stu-id="23940-160">c.</span></span>  <span data-ttu-id="23940-161">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span><span class="sxs-lookup"><span data-stu-id="23940-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="23940-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="23940-162">These values are not the real.</span></span> <span data-ttu-id="23940-163">Ezek az értékek frissíti a tényleges azonosítója, válasz és bejelentkezési URL-címe.</span><span class="sxs-lookup"><span data-stu-id="23940-163">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="23940-164">Ügyfél [Proofpoint az igény szerinti ügyfél-támogatási csoport](https://www.proofpoint.com/us/support-services) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="23940-164">Contact [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to get these values.</span></span> 

4. <span data-ttu-id="23940-165">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="23940-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. <span data-ttu-id="23940-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="23940-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="23940-169">A a **igény szerinti konfigurációban Proofpoint** kattintson **Proofpoint konfigurálása az igény szerinti** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="23940-169">On the **Proofpoint on Demand Configuration** section, click **Configure Proofpoint on Demand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="23940-170">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="23940-170">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. <span data-ttu-id="23940-172">Egyszeri bejelentkezés konfigurálása **az igény szerinti Proofpoint** oldalon kell küldeniük a letöltött **Certificate(Base64)**,**SAML Entitásazonosító**, és **SAML egyetlen Bejelentkezési URL-címe** való [Proofpoint az igény szerinti ügyfél-támogatási csoport](https://www.proofpoint.com/us/support-services).</span><span class="sxs-lookup"><span data-stu-id="23940-172">To configure single sign-on on **Proofpoint on Demand** side, you need to send the downloaded **Certificate(Base64)**,**SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services).</span></span>

> [!TIP]
> <span data-ttu-id="23940-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="23940-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="23940-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="23940-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="23940-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="23940-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="23940-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="23940-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="23940-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="23940-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="23940-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="23940-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="23940-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="23940-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="23940-182">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="23940-182">These values are not the real.</span></span> <span data-ttu-id="23940-183">A tényleges frissíteni ezeket az értékeket</span><span class="sxs-lookup"><span data-stu-id="23940-183">Update these values with the actual</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="23940-185">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="23940-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="23940-187">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="23940-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="23940-189">a.</span><span class="sxs-lookup"><span data-stu-id="23940-189">a.</span></span> <span data-ttu-id="23940-190">Az a **neve** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="23940-190">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="23940-191">b.</span><span class="sxs-lookup"><span data-stu-id="23940-191">b.</span></span> <span data-ttu-id="23940-192">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23940-192">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="23940-193">c.</span><span class="sxs-lookup"><span data-stu-id="23940-193">c.</span></span> <span data-ttu-id="23940-194">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="23940-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="23940-195">d.</span><span class="sxs-lookup"><span data-stu-id="23940-195">d.</span></span> <span data-ttu-id="23940-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="23940-196">Click **Create**.</span></span>
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a><span data-ttu-id="23940-197">Egy Proofpoint igény szerinti tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="23940-197">Creating a Proofpoint on Demand test user</span></span>

<span data-ttu-id="23940-198">Ebben a szakaszban az igény szerinti Proofpoint Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="23940-198">In this section, you create a user called Britta Simon in Proofpoint on Demand.</span></span> <span data-ttu-id="23940-199">Együttműködve [Proofpoint az igény szerinti ügyfél-támogatási csoport](https://www.proofpoint.com/us/support-services) felhasználót is hozzáadhat a Proofpoint igény szerint a platformon.</span><span class="sxs-lookup"><span data-stu-id="23940-199">Work with [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to add users in the Proofpoint on Demand platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="23940-200">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="23940-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="23940-201">Ebben a szakaszban Britta Simon hozzáférést biztosít az igény szerinti Proofpoint által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="23940-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Proofpoint on Demand.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="23940-203">**Az igény szerinti Proofpoint Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="23940-203">**To assign Britta Simon to Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="23940-204">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="23940-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="23940-206">Az alkalmazások listában válassza ki a **Proofpoint az igény szerinti**.</span><span class="sxs-lookup"><span data-stu-id="23940-206">In the applications list, select **Proofpoint on Demand**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. <span data-ttu-id="23940-208">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="23940-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="23940-210">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="23940-210">Click **Add** button.</span></span> <span data-ttu-id="23940-211">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23940-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="23940-213">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="23940-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="23940-214">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23940-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="23940-215">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="23940-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="23940-216">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="23940-216">Testing single sign-on</span></span>

<span data-ttu-id="23940-217">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="23940-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="23940-218">Ha a **Proofpoint az igény szerinti** csempét, hogy kell automatikusan megtörténik igény szerinti alkalmazásra a Proofpoint a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="23940-218">When you click the **Proofpoint on Demand** tile on the Access Panel, you should be automatically signed on to your Proofpoint on Demand application.</span></span>
<span data-ttu-id="23940-219">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="23940-219">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>  

## <a name="additional-resources"></a><span data-ttu-id="23940-220">További források</span><span class="sxs-lookup"><span data-stu-id="23940-220">Additional resources</span></span>

* [<span data-ttu-id="23940-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="23940-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="23940-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="23940-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

