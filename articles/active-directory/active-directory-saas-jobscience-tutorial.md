---
title: "Oktatóanyag: Azure Active Directoryval integrált Jobscience |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jobscience között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a><span data-ttu-id="48011-103">Oktatóanyag: Azure Active Directoryval integrált Jobscience</span><span class="sxs-lookup"><span data-stu-id="48011-103">Tutorial: Azure Active Directory integration with Jobscience</span></span>

<span data-ttu-id="48011-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jobscience az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48011-104">In this tutorial, you learn how toointegrate Jobscience with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48011-105">Jobscience integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="48011-105">Integrating Jobscience with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="48011-106">Megadhatja a hozzáférés tooJobscience rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="48011-106">You can control in Azure AD who has access tooJobscience</span></span>
- <span data-ttu-id="48011-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJobscience (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="48011-107">You can enable your users tooautomatically get signed-on tooJobscience (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48011-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="48011-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="48011-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48011-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48011-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48011-110">Prerequisites</span></span>

<span data-ttu-id="48011-111">az Azure AD integrálása Jobscience tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="48011-111">tooconfigure Azure AD integration with Jobscience, you need hello following items:</span></span>

- <span data-ttu-id="48011-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="48011-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48011-113">Egy Jobscience egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="48011-113">A Jobscience single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48011-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="48011-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48011-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="48011-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48011-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="48011-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48011-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48011-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48011-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="48011-118">Scenario description</span></span>
<span data-ttu-id="48011-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="48011-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48011-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="48011-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48011-121">Hello gyűjteményből Jobscience hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48011-121">Adding Jobscience from hello gallery</span></span>
2. <span data-ttu-id="48011-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48011-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscience-from-hello-gallery"></a><span data-ttu-id="48011-123">Hello gyűjteményből Jobscience hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48011-123">Adding Jobscience from hello gallery</span></span>
<span data-ttu-id="48011-124">tooconfigure hello integrációja Jobscience az Azure AD-be, meg kell tooadd Jobscience hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="48011-124">tooconfigure hello integration of Jobscience into Azure AD, you need tooadd Jobscience from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="48011-125">**tooadd Jobscience hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="48011-125">**tooadd Jobscience from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="48011-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48011-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48011-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="48011-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="48011-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48011-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="48011-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="48011-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="48011-133">Hello keresési mezőbe, írja be a **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="48011-133">In hello search box, type **Jobscience**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. <span data-ttu-id="48011-135">A hello eredmények panelen válassza ki a **Jobscience**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-135">In hello results panel, select **Jobscience**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48011-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48011-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48011-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jobscience</span><span class="sxs-lookup"><span data-stu-id="48011-138">In this section, you configure and test Azure AD single sign-on with Jobscience based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48011-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jobscience tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="48011-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jobscience is tooa user in Azure AD.</span></span> <span data-ttu-id="48011-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Jobscience közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="48011-140">In other words, a link relationship between an Azure AD user and hello related user in Jobscience needs toobe established.</span></span>

<span data-ttu-id="48011-141">Jobscience, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="48011-141">In Jobscience, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="48011-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Jobscience-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="48011-142">tooconfigure and test Azure AD single sign-on with Jobscience, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="48011-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="48011-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="48011-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48011-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48011-145">**[Jobscience tesztfelhasználó létrehozása](#creating-a-jobscience-test-user)**  -toohave egy megfelelője a Britta Simon a Jobscience, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="48011-145">**[Creating a Jobscience test user](#creating-a-jobscience-test-user)** - toohave a counterpart of Britta Simon in Jobscience that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="48011-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="48011-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48011-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="48011-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48011-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48011-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48011-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jobscience alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="48011-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jobscience application.</span></span>

<span data-ttu-id="48011-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Jobscience, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="48011-150">**tooconfigure Azure AD single sign-on with Jobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="48011-151">Az Azure portál, a hello hello **Jobscience** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="48011-151">In hello Azure portal, on hello **Jobscience** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="48011-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="48011-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. <span data-ttu-id="48011-155">A hello **Jobscience tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48011-155">On hello **Jobscience Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    <span data-ttu-id="48011-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<company name>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="48011-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `http://<company name>.my.salesforce.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="48011-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="48011-158">This value is not real.</span></span> <span data-ttu-id="48011-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="48011-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="48011-160">Hozza ki ezt az értéket [Jobscience ügyfél-támogatási csoport](https://www.jobscience.com/support) vagy hello egyszeri bejelentkezési profil létrehozandó, amely kifejtett hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="48011-160">Get this value by [Jobscience Client support team](https://www.jobscience.com/support) or from hello SSO profile you will create which is explained later in hello tutorial.</span></span> 
 
4. <span data-ttu-id="48011-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="48011-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. <span data-ttu-id="48011-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48011-165">A hello **Jobscience konfigurációs** kattintson **konfigurálása Jobscience** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="48011-165">On hello **Jobscience Configuration** section, click **Configure Jobscience** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="48011-166">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="48011-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. <span data-ttu-id="48011-168">Jelentkezzen be tooyour Jobscience vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="48011-168">Log in tooyour Jobscience company site as an administrator.</span></span>

8. <span data-ttu-id="48011-169">Nyissa meg túl**telepítő**.</span><span class="sxs-lookup"><span data-stu-id="48011-169">Go too**Setup**.</span></span>
   
   <span data-ttu-id="48011-170">![A telepítő](./media/active-directory-saas-jobscience-tutorial/IC784358.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="48011-170">![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")</span></span>

9. <span data-ttu-id="48011-171">A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap.</span><span class="sxs-lookup"><span data-stu-id="48011-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
   <span data-ttu-id="48011-172">![Saját tartomány](./media/active-directory-saas-jobscience-tutorial/ic767825.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="48011-172">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="48011-173">amely a tartomány megfelelően be van állítva tooverify győződjön meg arról, hogy "**lépés 4 telepített tooUsers**", és tekintse át a "**saját tartománybeállítások**".</span><span class="sxs-lookup"><span data-stu-id="48011-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>

    <span data-ttu-id="48011-174">![Tartományban üzembe helyezett tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "tartományban telepített tooUser")</span><span class="sxs-lookup"><span data-stu-id="48011-174">![Domain Deployed tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="48011-175">Hello Jobscience vállalati hely, kattintson a **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="48011-175">On hello Jobscience company site, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="48011-176">![Biztonsági vezérlők](./media/active-directory-saas-jobscience-tutorial/ic784364.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="48011-176">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784364.png "Security Controls")</span></span>

12. <span data-ttu-id="48011-177">A hello **egyszeri bejelentkezési beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48011-177">In hello **Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="48011-178">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-jobscience-tutorial/ic781026.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="48011-178">![Single Sign-On Settings](./media/active-directory-saas-jobscience-tutorial/ic781026.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="48011-179">a.</span><span class="sxs-lookup"><span data-stu-id="48011-179">a.</span></span> <span data-ttu-id="48011-180">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="48011-180">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="48011-181">b.</span><span class="sxs-lookup"><span data-stu-id="48011-181">b.</span></span> <span data-ttu-id="48011-182">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="48011-182">Click **New**.</span></span>

13. <span data-ttu-id="48011-183">A hello **SAML-alapú egyszeri bejelentkezési beállítás szerkesztése** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48011-183">On hello **SAML Single Sign-On Setting Edit** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="48011-184">![SAML-alapú egyszeri bejelentkezés beállítása](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML-alapú egyszeri bejelentkezés beállítása")</span><span class="sxs-lookup"><span data-stu-id="48011-184">![SAML Single Sign-On Setting](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="48011-185">a.</span><span class="sxs-lookup"><span data-stu-id="48011-185">a.</span></span> <span data-ttu-id="48011-186">A hello **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="48011-186">In hello **Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="48011-187">b.</span><span class="sxs-lookup"><span data-stu-id="48011-187">b.</span></span> <span data-ttu-id="48011-188">A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="48011-188">In **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="48011-189">c.</span><span class="sxs-lookup"><span data-stu-id="48011-189">c.</span></span> <span data-ttu-id="48011-190">A hello **entitásazonosító** szövegmezőhöz típusa`https://salesforce-jobscience.com`</span><span class="sxs-lookup"><span data-stu-id="48011-190">In hello **Entity Id** textbox, type `https://salesforce-jobscience.com`</span></span>

    <span data-ttu-id="48011-191">d.</span><span class="sxs-lookup"><span data-stu-id="48011-191">d.</span></span> <span data-ttu-id="48011-192">Kattintson a **Tallózás** tooupload az Azure AD-tanúsítványa.</span><span class="sxs-lookup"><span data-stu-id="48011-192">Click **Browse** tooupload your Azure AD certificate.</span></span>

    <span data-ttu-id="48011-193">e.</span><span class="sxs-lookup"><span data-stu-id="48011-193">e.</span></span> <span data-ttu-id="48011-194">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.</span><span class="sxs-lookup"><span data-stu-id="48011-194">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>

    <span data-ttu-id="48011-195">f.</span><span class="sxs-lookup"><span data-stu-id="48011-195">f.</span></span> <span data-ttu-id="48011-196">Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentfier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="48011-196">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>

    <span data-ttu-id="48011-197">g.</span><span class="sxs-lookup"><span data-stu-id="48011-197">g.</span></span> <span data-ttu-id="48011-198">A **Identity Provider bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="48011-198">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="48011-199">h.</span><span class="sxs-lookup"><span data-stu-id="48011-199">h.</span></span> <span data-ttu-id="48011-200">A **Identity Provider kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="48011-200">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="48011-201">i.</span><span class="sxs-lookup"><span data-stu-id="48011-201">i.</span></span> <span data-ttu-id="48011-202">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-202">Click **Save**.</span></span>

14. <span data-ttu-id="48011-203">A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap.</span><span class="sxs-lookup"><span data-stu-id="48011-203">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="48011-204">![Saját tartomány](./media/active-directory-saas-jobscience-tutorial/ic767825.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="48011-204">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

15. <span data-ttu-id="48011-205">A hello **saját tartomány** lap hello **bejelentkezési oldal vállalati arculata** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="48011-205">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="48011-206">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-jobscience-tutorial/ic767826.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="48011-206">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Login Page Branding")</span></span>

16. <span data-ttu-id="48011-207">A hello **bejelentkezési oldal vállalati arculata** lap hello **hitelesítési szolgáltatás** szakaszban, hello nevét a **SAML SSO beállítások** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="48011-207">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="48011-208">Válassza ki azt, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="48011-208">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="48011-209">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-jobscience-tutorial/ic784366.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="48011-209">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Login Page Branding")</span></span>

17. <span data-ttu-id="48011-210">tooget hello SP kezdeményezett egyszeri bejelentkezéshez a bejelentkezési URL-cím kattintson a hello **egyszeri bejelentkezés beállítások** a hello **biztonsági vezérlők** menü szakasz.</span><span class="sxs-lookup"><span data-stu-id="48011-210">tooget hello SP initiated Single Sign on Login URL click on hello **Single Sign On settings** in hello **Security Controls** menu section.</span></span>

    <span data-ttu-id="48011-211">![Biztonsági vezérlők](./media/active-directory-saas-jobscience-tutorial/ic784368.png "biztonsági vezérlőket")</span><span class="sxs-lookup"><span data-stu-id="48011-211">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784368.png "Security Controls")</span></span>
    
    <span data-ttu-id="48011-212">Kattintson a fenti hello lépésben létrehozott hello egyszeri bejelentkezési profil.</span><span class="sxs-lookup"><span data-stu-id="48011-212">Click hello SSO profile you have created in hello step above.</span></span> <span data-ttu-id="48011-213">Ezen a lapon látható hello egyszeri bejelentkezési URL-címen a vállalat (például [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span><span class="sxs-lookup"><span data-stu-id="48011-213">This page shows hello Single Sign on URL for your company (for example, [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span></span>    

> [!TIP]
> <span data-ttu-id="48011-214">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="48011-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="48011-215">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="48011-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="48011-216">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48011-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48011-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48011-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="48011-218">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="48011-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="48011-220">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="48011-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="48011-221">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48011-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48011-223">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="48011-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48011-225">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48011-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48011-227">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48011-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48011-229">a.</span><span class="sxs-lookup"><span data-stu-id="48011-229">a.</span></span> <span data-ttu-id="48011-230">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48011-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48011-231">b.</span><span class="sxs-lookup"><span data-stu-id="48011-231">b.</span></span> <span data-ttu-id="48011-232">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48011-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48011-233">c.</span><span class="sxs-lookup"><span data-stu-id="48011-233">c.</span></span> <span data-ttu-id="48011-234">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="48011-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="48011-235">d.</span><span class="sxs-lookup"><span data-stu-id="48011-235">d.</span></span> <span data-ttu-id="48011-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-236">Click **Create**.</span></span>
 
### <a name="creating-a-jobscience-test-user"></a><span data-ttu-id="48011-237">Jobscience tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48011-237">Creating a Jobscience test user</span></span>

<span data-ttu-id="48011-238">A sorrend tooenable az Azure AD felhasználók toolog a tooJobscience azok ki kell építenie Jobscience be.</span><span class="sxs-lookup"><span data-stu-id="48011-238">In order tooenable Azure AD users toolog in tooJobscience, they must be provisioned into Jobscience.</span></span> <span data-ttu-id="48011-239">Jobscience hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="48011-239">In hello case of Jobscience, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="48011-240">Bármely más Jobscience felhasználói fiók létrehozása eszközök vagy Jobscience tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="48011-240">You can use any other Jobscience user account creation tools or APIs provided by Jobscience tooprovision Azure Active Directory user accounts.</span></span>
>  
        
<span data-ttu-id="48011-241">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="48011-241">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="48011-242">Jelentkezzen be tooyour **Jobscience** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="48011-242">Log in tooyour **Jobscience** company site as administrator.</span></span>

2. <span data-ttu-id="48011-243">Nyissa meg tooSetup.</span><span class="sxs-lookup"><span data-stu-id="48011-243">Go tooSetup.</span></span>
   
   <span data-ttu-id="48011-244">![A telepítő](./media/active-directory-saas-jobscience-tutorial/ic784358.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="48011-244">![Setup](./media/active-directory-saas-jobscience-tutorial/ic784358.png "Setup")</span></span>
3. <span data-ttu-id="48011-245">Nyissa meg túl**felhasználók kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="48011-245">Go too**Manage Users \> Users**.</span></span>
   
   <span data-ttu-id="48011-246">![Felhasználók](./media/active-directory-saas-jobscience-tutorial/ic784369.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="48011-246">![Users](./media/active-directory-saas-jobscience-tutorial/ic784369.png "Users")</span></span>
4. <span data-ttu-id="48011-247">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="48011-247">Click **New User**.</span></span>
   
   <span data-ttu-id="48011-248">![Minden felhasználó](./media/active-directory-saas-jobscience-tutorial/ic784370.png "minden felhasználó")</span><span class="sxs-lookup"><span data-stu-id="48011-248">![All Users](./media/active-directory-saas-jobscience-tutorial/ic784370.png "All Users")</span></span>
5. <span data-ttu-id="48011-249">A hello **-felhasználó szerkesztése** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="48011-249">On hello **Edit User** dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="48011-250">![Felhasználó szerkesztése](./media/active-directory-saas-jobscience-tutorial/ic784371.png "felhasználó szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="48011-250">![User Edit](./media/active-directory-saas-jobscience-tutorial/ic784371.png "User Edit")</span></span>
   
   <span data-ttu-id="48011-251">a.</span><span class="sxs-lookup"><span data-stu-id="48011-251">a.</span></span> <span data-ttu-id="48011-252">A hello **Keresztnév** szövegmező, például Britta hello felhasználó utónevét adja.</span><span class="sxs-lookup"><span data-stu-id="48011-252">In hello **First Name** textbox, type a first name of hello user like Britta.</span></span>
   
   <span data-ttu-id="48011-253">b.</span><span class="sxs-lookup"><span data-stu-id="48011-253">b.</span></span> <span data-ttu-id="48011-254">A hello **Vezetéknév** szövegmező, írja be a vezetéknevet hello felhasználó például Simon.</span><span class="sxs-lookup"><span data-stu-id="48011-254">In hello **Last Name** textbox, type a last name of hello user like Simon.</span></span>
   
   <span data-ttu-id="48011-255">c.</span><span class="sxs-lookup"><span data-stu-id="48011-255">c.</span></span> <span data-ttu-id="48011-256">A hello **Alias** szövegmező, írja be például brittas hello felhasználó aliasnevet.</span><span class="sxs-lookup"><span data-stu-id="48011-256">In hello **Alias** textbox, type an alias name of hello user like brittas.</span></span>

   <span data-ttu-id="48011-257">d.</span><span class="sxs-lookup"><span data-stu-id="48011-257">d.</span></span> <span data-ttu-id="48011-258">A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="48011-258">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="48011-259">e.</span><span class="sxs-lookup"><span data-stu-id="48011-259">e.</span></span> <span data-ttu-id="48011-260">A hello **felhasználónév** szövegmező, írja be például a felhasználó a felhasználónév Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="48011-260">In hello **User Name** textbox, type a user name of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="48011-261">f.</span><span class="sxs-lookup"><span data-stu-id="48011-261">f.</span></span> <span data-ttu-id="48011-262">A hello **becenév** szövegmező, írja be például a Simon felhasználó nick nevét.</span><span class="sxs-lookup"><span data-stu-id="48011-262">In hello **Nick Name** textbox, type a nick name of user like Simon.</span></span>

   <span data-ttu-id="48011-263">g.</span><span class="sxs-lookup"><span data-stu-id="48011-263">g.</span></span> <span data-ttu-id="48011-264">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-264">Click **Save**.</span></span>

    
> [!NOTE]
> <span data-ttu-id="48011-265">hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="48011-265">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="48011-266">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="48011-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="48011-267">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJobscience megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="48011-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobscience.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="48011-269">**tooassign Britta Simon tooJobscience, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="48011-269">**tooassign Britta Simon tooJobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="48011-270">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48011-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="48011-272">Hello alkalmazások listában válassza ki a **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="48011-272">In hello applications list, select **Jobscience**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. <span data-ttu-id="48011-274">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="48011-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="48011-276">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="48011-276">Click **Add** button.</span></span> <span data-ttu-id="48011-277">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48011-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="48011-279">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="48011-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="48011-280">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48011-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48011-281">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48011-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48011-282">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="48011-282">Testing single sign-on</span></span>

<span data-ttu-id="48011-283">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="48011-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="48011-284">Ha a hozzáférési Panel hello hello Jobscience csempe gombra kattint, automatikusan bejelentkezett tooyour Jobscience alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="48011-284">When you click hello Jobscience tile in hello Access Panel, you should get automatically signed-on tooyour Jobscience application.</span></span>
<span data-ttu-id="48011-285">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="48011-285">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48011-286">További források</span><span class="sxs-lookup"><span data-stu-id="48011-286">Additional resources</span></span>

* [<span data-ttu-id="48011-287">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="48011-287">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48011-288">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="48011-288">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

