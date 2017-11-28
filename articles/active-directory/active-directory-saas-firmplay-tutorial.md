---
title: "Oktatóanyag: Azure Active Directoryval integrált FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és FirmPlay - a személyzeti osztályon dolgozó tanácsadáson között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 3cddd5b9508159089bf344dbb3882d462799747c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a><span data-ttu-id="230c4-103">Oktatóanyag: Azure Active Directoryval integrált FirmPlay - a személyzeti osztályon dolgozó tanácsadáson</span><span class="sxs-lookup"><span data-stu-id="230c4-103">Tutorial: Azure Active Directory integration with FirmPlay - Employee Advocacy for Recruiting</span></span>

<span data-ttu-id="230c4-104">Ebben az oktatóanyagban elsajátíthatja FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure Active Directoryval (Azure AD) integrálása.</span><span class="sxs-lookup"><span data-stu-id="230c4-104">In this tutorial, you learn how to integrate FirmPlay - Employee Advocacy for Recruiting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="230c4-105">FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="230c4-105">Integrating FirmPlay - Employee Advocacy for Recruiting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="230c4-106">Szabályozhatja, aki hozzáférhet FirmPlay - alkalmazott tanácsadáson személyzeti osztályon az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="230c4-106">You can control in Azure AD who has access to FirmPlay - Employee Advocacy for Recruiting</span></span>
- <span data-ttu-id="230c4-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="230c4-107">You can enable your users to automatically get signed-on to FirmPlay - Employee Advocacy for Recruiting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="230c4-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="230c4-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="230c4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="230c4-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="230c4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="230c4-110">Prerequisites</span></span>

<span data-ttu-id="230c4-111">FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD-integrációs konfigurálni kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="230c4-111">To configure Azure AD integration with FirmPlay - Employee Advocacy for Recruiting, you need the following items:</span></span>

- <span data-ttu-id="230c4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="230c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="230c4-113">Egy FirmPlay - alkalmazott tanácsadáson felvételi egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="230c4-113">A FirmPlay - Employee Advocacy for Recruiting single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="230c4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="230c4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="230c4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="230c4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="230c4-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="230c4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="230c4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="230c4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="230c4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="230c4-118">Scenario description</span></span>
<span data-ttu-id="230c4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="230c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="230c4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="230c4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="230c4-121">Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="230c4-121">Adding FirmPlay - Employee Advocacy for Recruiting from the gallery</span></span>
2. <span data-ttu-id="230c4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="230c4-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a><span data-ttu-id="230c4-123">Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="230c4-123">Adding FirmPlay - Employee Advocacy for Recruiting from the gallery</span></span>
<span data-ttu-id="230c4-124">A FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD-integráció konfigurálása kell hozzáadnia a FirmPlay - alkalmazott tanácsadáson a kezelt SaaS-alkalmazások listájára a gyűjteményből személyzeti osztályon.</span><span class="sxs-lookup"><span data-stu-id="230c4-124">To configure the integration of FirmPlay - Employee Advocacy for Recruiting into Azure AD, you need to add FirmPlay - Employee Advocacy for Recruiting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="230c4-125">**Adja hozzá a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="230c4-125">**To add FirmPlay - Employee Advocacy for Recruiting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="230c4-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="230c4-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="230c4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="230c4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="230c4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="230c4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="230c4-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="230c4-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="230c4-133">Írja be a keresőmezőbe, **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.</span><span class="sxs-lookup"><span data-stu-id="230c4-133">In the search box, type **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. <span data-ttu-id="230c4-135">Az eredmények panelen válassza ki a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="230c4-135">In the results panel, select **FirmPlay - Employee Advocacy for Recruiting**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="230c4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="230c4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="230c4-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="230c4-138">In this section, you configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="230c4-139">Az egyszeri bejelentkezés használatához az Azure AD meg kell tudni, hogy milyen a párjukhoz felhasználó FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="230c4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FirmPlay - Employee Advocacy for Recruiting is to a user in Azure AD.</span></span> <span data-ttu-id="230c4-140">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználó a FirmPlay - alkalmazott tanácsadáson felvételi kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="230c4-140">In other words, a link relationship between an Azure AD user and the related user in FirmPlay - Employee Advocacy for Recruiting needs to be established.</span></span>

<span data-ttu-id="230c4-141">A hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon.</span><span class="sxs-lookup"><span data-stu-id="230c4-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FirmPlay - Employee Advocacy for Recruiting.</span></span>

<span data-ttu-id="230c4-142">Az Azure AD egyszeri bejelentkezést a FirmPlay - tesztelése és konfigurálása személyzeti osztályon, az alkalmazott tanácsadáson kell végrehajtani a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="230c4-142">To configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="230c4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="230c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="230c4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="230c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="230c4-145">**[Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  - való Britta Simon valami FirmPlay: alkalmazott tanácsadáson felvételi, amely csatolva rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="230c4-145">**[Creating a FirmPlay - Employee Advocacy for Recruiting test user](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)** - to have a counterpart of Britta Simon in FirmPlay: Employee Advocacy for Recruiting that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="230c4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="230c4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="230c4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="230c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="230c4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="230c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="230c4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és a FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="230c4-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FirmPlay - Employee Advocacy for Recruiting application.</span></span>

<span data-ttu-id="230c4-150">**A következő lépésekkel FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD az egyszeri bejelentkezés konfigurálása:**</span><span class="sxs-lookup"><span data-stu-id="230c4-150">**To configure Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, perform the following steps:**</span></span>

1. <span data-ttu-id="230c4-151">Az Azure felügyeleti portálján a a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="230c4-151">In the Azure Management portal, on the **FirmPlay - Employee Advocacy for Recruiting** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="230c4-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="230c4-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. <span data-ttu-id="230c4-155">Az a **FirmPlay - tartomány felvétele és URL-címek alkalmazott tanácsadáson** részben, a a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://<your-subdomain>.firmplay.com/`</span><span class="sxs-lookup"><span data-stu-id="230c4-155">On the **FirmPlay - Employee Advocacy for Recruiting Domain and URLs** section, in the **Sign On URL** textbox, type a URL using the following pattern: `https://<your-subdomain>.firmplay.com/`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > <span data-ttu-id="230c4-157">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="230c4-157">Please note that this is not the real value.</span></span> <span data-ttu-id="230c4-158">Ezt az értéket a tényleges bejelentkezési URL-cím frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="230c4-158">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="230c4-159">Ügyfél [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="230c4-159">Contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) to get this value.</span></span> 

4. <span data-ttu-id="230c4-160">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="230c4-160">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. <span data-ttu-id="230c4-162">A a **új tanúsítvány létrehozása** párbeszédpanel, kattintson a naptár ikonra, és válasszon egy **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="230c4-162">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="230c4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="230c4-163">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="230c4-165">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="230c4-165">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. <span data-ttu-id="230c4-167">Az előugró **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="230c4-167">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="230c4-169">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="230c4-169">On the **SAML Signing Certificate** section, click **Certificate (base64)** and then save the certificate file on your computer.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. <span data-ttu-id="230c4-171">A a **FirmPlay - konfiguráció felvételi alkalmazott tanácsadáson** kattintson **FirmPlay konfigurálása - a személyzeti osztályon dolgozó tanácsadáson** megnyitásához **bejelentkezés konfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="230c4-171">On the **FirmPlay - Employee Advocacy for Recruiting Configuration** section, click **Configure FirmPlay - Employee Advocacy for Recruiting** to open **Configure sign-on** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. <span data-ttu-id="230c4-174">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) és adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="230c4-174">To get SSO configured for your application, contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) and provide them with the following:</span></span> 

    <span data-ttu-id="230c4-175">• A letöltött **tanúsítványfájl**</span><span class="sxs-lookup"><span data-stu-id="230c4-175">•  The downloaded **Certificate file**</span></span>

    <span data-ttu-id="230c4-176">• A **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-címe**</span><span class="sxs-lookup"><span data-stu-id="230c4-176">•  The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="230c4-177">• A **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="230c4-177">•  The **SAML Entity ID**</span></span>

    <span data-ttu-id="230c4-178">• A **kijelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="230c4-178">•  The **Sign-Out URL**</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="230c4-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="230c4-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="230c4-180">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="230c4-180">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="230c4-182">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="230c4-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="230c4-183">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="230c4-183">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="230c4-185">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="230c4-185">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="230c4-187">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="230c4-187">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="230c4-189">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="230c4-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="230c4-191">a.</span><span class="sxs-lookup"><span data-stu-id="230c4-191">a.</span></span> <span data-ttu-id="230c4-192">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="230c4-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="230c4-193">b.</span><span class="sxs-lookup"><span data-stu-id="230c4-193">b.</span></span> <span data-ttu-id="230c4-194">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="230c4-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="230c4-195">c.</span><span class="sxs-lookup"><span data-stu-id="230c4-195">c.</span></span> <span data-ttu-id="230c4-196">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="230c4-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="230c4-197">d.</span><span class="sxs-lookup"><span data-stu-id="230c4-197">d.</span></span> <span data-ttu-id="230c4-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="230c4-198">Click **Create**.</span></span> 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a><span data-ttu-id="230c4-199">Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="230c4-199">Creating a FirmPlay - Employee Advocacy for Recruiting test user</span></span>

<span data-ttu-id="230c4-200">Ebben a szakaszban Britta Simon meghívta FirmPlay - alkalmazott tanácsadáson személyzeti osztályon a felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="230c4-200">In this section, you create a user called Britta Simon in FirmPlay - Employee Advocacy for Recruiting.</span></span> <span data-ttu-id="230c4-201">Adjon együttműködve [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) felhasználót is hozzáadhat a FirmPlay - alkalmazott tanácsadáson személyzeti osztályon platform.</span><span class="sxs-lookup"><span data-stu-id="230c4-201">Please work with [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) to add the users in the FirmPlay - Employee Advocacy for Recruiting platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="230c4-202">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="230c4-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="230c4-203">Ebben a szakaszban engedélyezze Britta Simon által biztosított saját hozzáférés FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="230c4-203">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to FirmPlay - Employee Advocacy for Recruiting.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="230c4-205">**Britta Simon hozzárendelése FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="230c4-205">**To assign Britta Simon to FirmPlay - Employee Advocacy for Recruiting, perform the following steps:**</span></span>

1. <span data-ttu-id="230c4-206">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="230c4-206">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="230c4-208">Az alkalmazások listában válassza ki a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.</span><span class="sxs-lookup"><span data-stu-id="230c4-208">In the applications list, select **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. <span data-ttu-id="230c4-210">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="230c4-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="230c4-212">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="230c4-212">Click **Add** button.</span></span> <span data-ttu-id="230c4-213">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="230c4-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="230c4-215">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="230c4-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="230c4-216">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="230c4-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="230c4-217">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="230c4-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="230c4-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="230c4-218">Testing single sign-on</span></span>

<span data-ttu-id="230c4-219">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="230c4-219">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="230c4-220">A FirmPlay - alkalmazott tanácsadáson személyzeti osztályon csempe a hozzáférési panelen kattintva meg kell beolvasni automatikusan bejelentkezett a FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz való.</span><span class="sxs-lookup"><span data-stu-id="230c4-220">When you click the FirmPlay - Employee Advocacy for Recruiting tile in the Access Panel, you should get automatically signed-on to your FirmPlay - Employee Advocacy for Recruiting application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="230c4-221">További források</span><span class="sxs-lookup"><span data-stu-id="230c4-221">Additional resources</span></span>

* [<span data-ttu-id="230c4-222">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="230c4-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="230c4-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="230c4-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png