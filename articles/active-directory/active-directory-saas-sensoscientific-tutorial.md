---
title: "Oktatóanyag: Azure Active Directoryval integrált SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer között."
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
ms.openlocfilehash: 4eabf7fc6457c217fd5c0c2539ab88c8110055e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a><span data-ttu-id="e8fc6-103">Oktatóanyag: Azure Active Directory-integráció rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelése</span><span class="sxs-lookup"><span data-stu-id="e8fc6-103">Tutorial: Azure Active Directory integration with SensoScientific Wireless Temperature Monitoring System</span></span>

<span data-ttu-id="e8fc6-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e8fc6-104">In this tutorial, you learn how toointegrate SensoScientific Wireless Temperature Monitoring System with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e8fc6-105">SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-105">Integrating SensoScientific Wireless Temperature Monitoring System with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e8fc6-106">Megadhatja a hozzáférés tooSensoScientific vezeték nélküli hőmérséklet-figyelő rendszer rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e8fc6-106">You can control in Azure AD who has access tooSensoScientific Wireless Temperature Monitoring System</span></span>
- <span data-ttu-id="e8fc6-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSensoScientific vezeték nélküli hőmérséklet-figyelő rendszer (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="e8fc6-107">You can enable your users tooautomatically get signed-on tooSensoScientific Wireless Temperature Monitoring System (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e8fc6-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e8fc6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e8fc6-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e8fc6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8fc6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e8fc6-110">Prerequisites</span></span>

<span data-ttu-id="e8fc6-111">tooconfigure SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-111">tooconfigure Azure AD integration with SensoScientific Wireless Temperature Monitoring System, you need hello following items:</span></span>

- <span data-ttu-id="e8fc6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e8fc6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e8fc6-113">Egy SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="e8fc6-113">A SensoScientific Wireless Temperature Monitoring System single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e8fc6-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e8fc6-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e8fc6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e8fc6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e8fc6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e8fc6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-118">Scenario description</span></span>
<span data-ttu-id="e8fc6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e8fc6-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e8fc6-121">Hello gyűjteményből SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-121">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
2. <span data-ttu-id="e8fc6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e8fc6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-hello-gallery"></a><span data-ttu-id="e8fc6-123">Hello gyűjteményből SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-123">Adding SensoScientific Wireless Temperature Monitoring System from hello gallery</span></span>
<span data-ttu-id="e8fc6-124">tooconfigure hello integrációs SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer, az Azure AD-be, meg kell tooadd SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-124">tooconfigure hello integration of SensoScientific Wireless Temperature Monitoring System into Azure AD, you need tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e8fc6-125">**tooadd SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e8fc6-125">**tooadd SensoScientific Wireless Temperature Monitoring System from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8fc6-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e8fc6-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e8fc6-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e8fc6-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e8fc6-133">Hello keresési mezőbe, írja be a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-133">In hello search box, type **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

5. <span data-ttu-id="e8fc6-135">Hello eredmények panelen, jelölje ki a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-135">In hello results panel, select **SensoScientific Wireless Temperature Monitoring System**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e8fc6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e8fc6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e8fc6-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelés "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="e8fc6-138">In this section, you configure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e8fc6-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SensoScientific Wireless Temperature Monitoring System is tooa user in Azure AD.</span></span> <span data-ttu-id="e8fc6-140">Ez azt jelenti hello kapcsolódó felhasználói SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-140">In other words, a link relationship between an Azure AD user and hello related user in SensoScientific Wireless Temperature Monitoring System needs toobe established.</span></span>

<span data-ttu-id="e8fc6-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SensoScientific Wireless Temperature Monitoring System.</span></span>

<span data-ttu-id="e8fc6-142">tooconfigure és rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelése az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-142">tooconfigure and test Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e8fc6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e8fc6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e8fc6-145">**[SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer tesztfelhasználó létrehozása](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  -toohave Britta Simon SensoScientific vezeték nélküli hőmérséklet figyelési rendszerben, amely egy megfelelője a kapcsolódó toohello az Azure AD-ábrázolása a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-145">**[Creating a SensoScientific Wireless Temperature Monitoring System test user](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)** - toohave a counterpart of Britta Simon in SensoScientific Wireless Temperature Monitoring System that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e8fc6-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e8fc6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e8fc6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e8fc6-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a SensoScientific vezeték nélküli hőmérséklet figyelési Rendszeralkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SensoScientific Wireless Temperature Monitoring System application.</span></span>

<span data-ttu-id="e8fc6-150">**rendszerrel SensoScientific vezeték nélküli hőmérséklet figyelés, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e8fc6-150">**tooconfigure Azure AD single sign-on with SensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8fc6-151">Az Azure portál, a hello hello **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-151">In hello Azure portal, on hello **SensoScientific Wireless Temperature Monitoring System** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e8fc6-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

3. <span data-ttu-id="e8fc6-155">A hello **SensoScientific vezeték nélküli hőmérséklet figyelési rendszer tartomány és az URL-címek** szakaszban, nincs szükség tooperform hello alkalmazásként egyik lépéshez sem már előre integrálva van az Azure-ral:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-155">On hello **SensoScientific Wireless Temperature Monitoring System Domain and URLs** section, no need tooperform any steps as hello app is already pre-integrated with Azure:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

4. <span data-ttu-id="e8fc6-157">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

5. <span data-ttu-id="e8fc6-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e8fc6-161">A hello **SensoScientific vezeték nélküli hőmérséklet figyelési rendszerkonfiguráció** kattintson **SensoScientific vezeték nélküli hőmérséklet figyelési rendszer konfigurálása** tooopen  **Bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-161">On hello **SensoScientific Wireless Temperature Monitoring System Configuration** section, click **Configure SensoScientific Wireless Temperature Monitoring System** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e8fc6-162">Másolás hello **Sign-Out URL, SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e8fc6-162">Copy hello **Sign-Out URL, SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

7. <span data-ttu-id="e8fc6-164">Bejelentkezés tooyour SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-164">Sign on tooyour SensoScientific Wireless Temperature Monitoring System application as an administrator.</span></span>

8. <span data-ttu-id="e8fc6-165">Hello felül hello navigációs ablakában kattintson **konfigurációs** és goto **konfigurálása** alatt **egyszeri bejelentkezés** tooopen hello egyetlen bejelentkezést a beállítások.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-165">In hello navigation menu on hello top, click **Configuration** and goto **Configure** under **Single Sign On** tooopen hello Single Sign On Settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

9. <span data-ttu-id="e8fc6-167">A **egyetlen bejelentkezést a beállítások** űrlap hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-167">In **Single Sign On Settings** form perform hello following steps:</span></span>
 
    <span data-ttu-id="e8fc6-168">a.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-168">a.</span></span> <span data-ttu-id="e8fc6-169">Válassza ki **Kibocsátónév** , az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-169">Select **Issuer Name** as Azure AD.</span></span>
    
    <span data-ttu-id="e8fc6-170">b.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-170">b.</span></span> <span data-ttu-id="e8fc6-171">Beillesztés hello **SAML Entitásazonosító** amely kibocsátó URL-cím szövegmezőbe az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-171">Paste hello **SAML Entity ID** which you have copied from Azure portal into Issuer URL textbox.</span></span>
    
    <span data-ttu-id="e8fc6-172">c.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-172">c.</span></span> <span data-ttu-id="e8fc6-173">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** , amely az egyszeri bejelentkezési URL-címe szövegmező másolta az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-173">Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal into Single Sign-On Service URL textbox.</span></span>

    <span data-ttu-id="e8fc6-174">d.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-174">d.</span></span> <span data-ttu-id="e8fc6-175">Beillesztés hello **Sign-Out URL-cím** , amely egyetlen Sign-Out szolgáltatás URL-cím szövegmezőbe az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-175">Paste hello **Sign-Out URL** which you have copied from Azure portal into Single Sign-Out Service URL textbox.</span></span>

    <span data-ttu-id="e8fc6-176">e.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-176">e.</span></span> <span data-ttu-id="e8fc6-177">Keresse meg az Azure-portálról letöltött, és töltse fel itt hello tartalmazó tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-177">Browse hello certificate which you have downloaded from Azure portal and upload here.</span></span>
    
    <span data-ttu-id="e8fc6-178">f.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-178">f.</span></span> <span data-ttu-id="e8fc6-179">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-179">Click **Save**.</span></span>
  
> [!TIP]
> <span data-ttu-id="e8fc6-180">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="e8fc6-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e8fc6-181">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e8fc6-182">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e8fc6-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e8fc6-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="e8fc6-184">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e8fc6-186">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e8fc6-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8fc6-187">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e8fc6-189">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e8fc6-191">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e8fc6-193">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e8fc6-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sensoscientific-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e8fc6-195">a.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-195">a.</span></span> <span data-ttu-id="e8fc6-196">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e8fc6-197">b.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-197">b.</span></span> <span data-ttu-id="e8fc6-198">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e8fc6-199">c.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-199">c.</span></span> <span data-ttu-id="e8fc6-200">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e8fc6-201">d.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-201">d.</span></span> <span data-ttu-id="e8fc6-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-202">Click **Create**.</span></span>
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a><span data-ttu-id="e8fc6-203">SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8fc6-203">Creating a SensoScientific Wireless Temperature Monitoring System test user</span></span>

<span data-ttu-id="e8fc6-204">az Azure AD tooenable felhasználók toolog a vezeték nélküli hőmérséklet-figyelő rendszer tooSensoScientific, azok ki kell építenie SensoScientific vezeték nélküli hőmérséklet figyelési rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-204">tooenable Azure AD users toolog in tooSensoScientific Wireless Temperature Monitoring System, they must be provisioned into SensoScientific Wireless Temperature Monitoring System.</span></span> <span data-ttu-id="e8fc6-205">Együttműködve [SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer támogatási csoport](https://www.sensoscientific.com/contact-us/) felhasználót is hozzáadhat hello hello SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer platform.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-205">Work with [SensoScientific Wireless Temperature Monitoring System support team](https://www.sensoscientific.com/contact-us/) to add hello users in hello SensoScientific Wireless Temperature Monitoring System platform.</span></span> <span data-ttu-id="e8fc6-206">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-206">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e8fc6-207">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e8fc6-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e8fc6-208">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSensoScientific vezeték nélküli hőmérséklet-figyelő rendszer megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSensoScientific Wireless Temperature Monitoring System.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e8fc6-210">**tooassign Britta Simon tooSensoScientific vezeték nélküli hőmérséklet-figyelő rendszer hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e8fc6-210">**tooassign Britta Simon tooSensoScientific Wireless Temperature Monitoring System, perform hello following steps:**</span></span>

1. <span data-ttu-id="e8fc6-211">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e8fc6-213">Hello alkalmazások listában válassza ki a **SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-213">In hello applications list, select **SensoScientific Wireless Temperature Monitoring System**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

3. <span data-ttu-id="e8fc6-215">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e8fc6-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-217">Click **Add** button.</span></span> <span data-ttu-id="e8fc6-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e8fc6-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e8fc6-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e8fc6-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e8fc6-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e8fc6-223">Testing single sign-on</span></span>

<span data-ttu-id="e8fc6-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="e8fc6-225">Hello SensoScientific vezeték nélküli hőmérséklet-figyelő rendszer csempén kattintson a hozzáférési Panel hello, automatikusan bejelentkezett tooyour SensoScientific vezeték nélküli hőmérséklet figyelési rendszeralkalmazás lesz.</span><span class="sxs-lookup"><span data-stu-id="e8fc6-225">Click hello SensoScientific Wireless Temperature Monitoring System tile in hello Access Panel, you will be automatically signed-on tooyour SensoScientific Wireless Temperature Monitoring System application.</span></span> <span data-ttu-id="e8fc6-226">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="e8fc6-226">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8fc6-227">További források</span><span class="sxs-lookup"><span data-stu-id="e8fc6-227">Additional resources</span></span>

* [<span data-ttu-id="e8fc6-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e8fc6-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e8fc6-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e8fc6-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

