---
title: "Oktatóanyag: Azure Active Directoryval integrált SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: fa6242cf7f9559ca394ffde2e5e734cb935b03dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="1e205-103">Oktatóanyag: Azure Active Directory-integráció rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelése</span><span class="sxs-lookup"><span data-stu-id="1e205-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="1e205-104">Ebben az oktatóanyagban elsajátíthatja SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1e205-104">In this tutorial, you learn how to integrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1e205-105">SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1e205-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1e205-106">Megadhatja a SensoScientific vezeték nélküli hőmérséklet figyelési rendszerhez hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1e205-106">You can control in Azure AD who has access to SensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="1e205-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SensoScientific vezeték nélküli hőmérséklet (egyszeri bejelentkezés) figyelési rendszerhez</span><span class="sxs-lookup"><span data-stu-id="1e205-107">You can enable your users to automatically get signed-on to SensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1e205-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1e205-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1e205-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1e205-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e205-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e205-110">Prerequisites</span></span>

<span data-ttu-id="1e205-111">Rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelése az Azure AD-integrációs konfigurálásához a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1e205-111">To configure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need the following items:</span></span>

- <span data-ttu-id="1e205-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1e205-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1e205-113">Egy SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1e205-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1e205-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1e205-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1e205-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1e205-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1e205-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1e205-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1e205-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1e205-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1e205-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1e205-118">Scenario description</span></span>
<span data-ttu-id="1e205-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1e205-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1e205-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1e205-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1e205-121">A gyűjteményből SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1e205-121">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
2. <span data-ttu-id="1e205-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1e205-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a><span data-ttu-id="1e205-123">A gyűjteményből SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1e205-123">Adding SensoScientific Wireless Temperature Monitoring System from the gallery</span></span>
<span data-ttu-id="1e205-124">Az Azure AD integrálása SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer konfigurálásához kell hozzáadnia SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1e205-124">To configure the integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need to add SensoScientific Wireless Temperature Monitoring System from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1e205-125">**A gyűjteményből SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e205-125">**To add SensoScientific Wireless Temperature Monitoring System from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1e205-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e205-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1e205-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1e205-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1e205-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1e205-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1e205-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1e205-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1e205-133">Írja be a keresőmezőbe, **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**.</span><span class="sxs-lookup"><span data-stu-id="1e205-133">In the search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="1e205-135">Az eredmények panelen válassza ki a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1e205-135">In the results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1e205-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1e205-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1e205-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelés "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="1e205-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1e205-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben egy felhasználói Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1e205-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SensoScientific Wireless Temperature Monitoring System is to a user in Azure AD.</span></span> <span data-ttu-id="1e205-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1e205-140">In other words, a link relationship between an Azure AD user and the related user in SensoScientific Wireless Temperature Monitoring System needs to be established.</span></span>

<span data-ttu-id="1e205-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben.</span><span class="sxs-lookup"><span data-stu-id="1e205-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="1e205-142">Az Azure AD az egyszeri bejelentkezés rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelési tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1e205-142">To configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1e205-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1e205-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1e205-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1e205-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1e205-145">**[SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer tesztfelhasználó létrehozása](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  - való egy megfelelője a Britta Simon SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1e205-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - to have a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1e205-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1e205-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1e205-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1e205-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1e205-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1e205-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1e205-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a SensoScientific vezeték nélküli hőmérséklet figyelési Rendszeralkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1e205-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="1e205-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés SensoScientific vezeték nélküli hőmérséklet figyelési rendszerrel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e205-150">**To configure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="1e205-151">Az Azure portálon a a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1e205-151">In the Azure portal, on the **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1e205-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1e205-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="1e205-155">Az a **SensoScientific vezeték nélküli hőmérséklet figyelési rendszer tartomány és az URL-címek** szakaszban, nem szükséges, az alkalmazás már előre integrált Azure-ral, hajtsa végre a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1e205-155">On the **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need to perform any steps as the app is already pre-integrated with Azure:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="1e205-157">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1e205-157">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="1e205-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e205-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1e205-161">A a **SensoScientific vezeték nélküli hőmérséklet figyelési rendszerkonfiguráció** kattintson **SensoScientific vezeték nélküli hőmérséklet figyelési rendszer konfigurálása** megnyitásához **konfigurálása bejelentkezés** ablak.</span><span class="sxs-lookup"><span data-stu-id="1e205-161">On the **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1e205-162">Másolás a **Sign-Out URL, SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1e205-162">Copy the **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="1e205-164">Jelentkezzen be rendszergazdaként a SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1e205-164">Sign on to your SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="1e205-165">Kattintson a navigációs menü felső **konfigurációs** és goto **konfigurálása** alatt **egyszeri bejelentkezés** az egyszeri bejelentkezési a beállításainak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1e205-165">In the navigation menu on the top, click **Configuration** and goto **Configure** under **Single Sign On** to open the Single Sign On Settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="1e205-167">A **egyetlen bejelentkezést a beállítások** űrlap hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1e205-167">In **Single Sign On Settings** form perform the following steps:</span></span>
 
    <span data-ttu-id="1e205-168">a.</span><span class="sxs-lookup"><span data-stu-id="1e205-168">a.</span></span> <span data-ttu-id="1e205-169">Válassza ki **Kibocsátónév** , az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1e205-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="1e205-170">b.</span><span class="sxs-lookup"><span data-stu-id="1e205-170">b.</span></span> <span data-ttu-id="1e205-171">Beillesztés a **SAML Entitásazonosító** amely kibocsátó URL-cím szövegmezőbe az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1e205-171">Paste the **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="1e205-172">c.</span><span class="sxs-lookup"><span data-stu-id="1e205-172">c.</span></span> <span data-ttu-id="1e205-173">Beillesztés a **SAML-alapú egyszeri bejelentkezési URL-címe** , amely az egyszeri bejelentkezési URL-címe szövegmező másolta az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="1e205-173">Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="1e205-174">d.</span><span class="sxs-lookup"><span data-stu-id="1e205-174">d.</span></span> <span data-ttu-id="1e205-175">Beillesztés a **Sign-Out URL-cím** , amely egyetlen Sign-Out szolgáltatás URL-cím szövegmezőbe az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="1e205-175">Paste the **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="1e205-176">e.</span><span class="sxs-lookup"><span data-stu-id="1e205-176">e.</span></span> <span data-ttu-id="1e205-177">Keresse meg a tanúsítványt, amely az Azure-portálról letöltött, és töltse fel itt.</span><span class="sxs-lookup"><span data-stu-id="1e205-177">Browse the certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="1e205-178">f.</span><span class="sxs-lookup"><span data-stu-id="1e205-178">f.</span></span> <span data-ttu-id="1e205-179">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e205-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="1e205-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1e205-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1e205-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1e205-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1e205-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1e205-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1e205-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e205-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="1e205-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1e205-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1e205-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e205-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1e205-187">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1e205-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1e205-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1e205-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1e205-191">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1e205-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1e205-193">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1e205-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1e205-195">a.</span><span class="sxs-lookup"><span data-stu-id="1e205-195">a.</span></span> <span data-ttu-id="1e205-196">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1e205-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1e205-197">b.</span><span class="sxs-lookup"><span data-stu-id="1e205-197">b.</span></span> <span data-ttu-id="1e205-198">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1e205-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1e205-199">c.</span><span class="sxs-lookup"><span data-stu-id="1e205-199">c.</span></span> <span data-ttu-id="1e205-200">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1e205-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1e205-201">d.</span><span class="sxs-lookup"><span data-stu-id="1e205-201">d.</span></span> <span data-ttu-id="1e205-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1e205-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="1e205-203">SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e205-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="1e205-204">Ahhoz, hogy az Azure AD-felhasználók SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer bejelentkezni, akkor ki kell építenie SensoScientific vezeték nélküli hőmérséklet figyelési rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="1e205-204">To enable Azure AD users to log in to SensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="1e205-205">Együttműködve [SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer támogatási csoport](https://www.sensoscientific.com/contact-us/) a felhasználók hozzáadása a SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer platform.</span><span class="sxs-lookup"><span data-stu-id="1e205-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add the users in the SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="1e205-206">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="1e205-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1e205-207">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1e205-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1e205-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1e205-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SensoScientific Wireless Temperature Monitoring System.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1e205-210">**Britta Simon hozzárendelése SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1e205-210">**To assign Britta Simon to SensoScientific Wireless Temperature Monitoring System, perform the following steps:**</span></span>

1. <span data-ttu-id="1e205-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1e205-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1e205-213">Az alkalmazások listában válassza ki a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**.</span><span class="sxs-lookup"><span data-stu-id="1e205-213">In the applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="1e205-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1e205-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1e205-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1e205-217">Click **Add** button.</span></span> <span data-ttu-id="1e205-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e205-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1e205-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1e205-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1e205-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e205-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1e205-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1e205-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1e205-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1e205-223">Testing single sign-on</span></span>

<span data-ttu-id="1e205-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="1e205-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="1e205-225">Kattintson a hozzáférési panelen SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer csempére, akkor lesz lehet automatikusan bejelentkezett az SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1e205-225">Click the SensoScientific Wireless Temperature Monitoring System tile in the Access Panel, you will be automatically signed-on to your SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="1e205-226">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="1e205-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e205-227">További források</span><span class="sxs-lookup"><span data-stu-id="1e205-227">Additional resources</span></span>

* [<span data-ttu-id="1e205-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1e205-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1e205-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1e205-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_203.png

