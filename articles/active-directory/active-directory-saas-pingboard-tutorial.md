---
title: "Oktatóanyag: Azure Active Directoryval integrált Pingboard |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Pingboard között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 0a916b1f9ef32d8124aa11284d2115bb4fc0bbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="efba3-103">Oktatóanyag: Azure Active Directoryval integrált Pingboard</span><span class="sxs-lookup"><span data-stu-id="efba3-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="efba3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Pingboard az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="efba3-104">In this tutorial, you learn how toointegrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="efba3-105">Pingboard integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="efba3-105">Integrating Pingboard with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="efba3-106">Megadhatja a hozzáférés tooPingboard rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="efba3-106">You can control in Azure AD who has access tooPingboard</span></span>
- <span data-ttu-id="efba3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPingboard (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="efba3-107">You can enable your users tooautomatically get signed-on tooPingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="efba3-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="efba3-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="efba3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="efba3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efba3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="efba3-110">Prerequisites</span></span>

<span data-ttu-id="efba3-111">az Azure AD integrálása Pingboard tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="efba3-111">tooconfigure Azure AD integration with Pingboard, you need hello following items:</span></span>

- <span data-ttu-id="efba3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="efba3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="efba3-113">Egy Pingboard egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="efba3-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="efba3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="efba3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="efba3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="efba3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="efba3-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="efba3-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="efba3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="efba3-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="efba3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="efba3-118">Scenario description</span></span>
<span data-ttu-id="efba3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="efba3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="efba3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="efba3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="efba3-121">Hello gyűjteményből Pingboard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efba3-121">Adding Pingboard from hello gallery</span></span>
2. <span data-ttu-id="efba3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="efba3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-hello-gallery"></a><span data-ttu-id="efba3-123">Hello gyűjteményből Pingboard hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efba3-123">Adding Pingboard from hello gallery</span></span>
<span data-ttu-id="efba3-124">tooconfigure hello integrációja Pingboard az Azure AD-be, meg kell tooadd Pingboard hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="efba3-124">tooconfigure hello integration of Pingboard into Azure AD, you need tooadd Pingboard from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="efba3-125">**tooadd Pingboard hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="efba3-125">**tooadd Pingboard from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="efba3-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="efba3-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="efba3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="efba3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="efba3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="efba3-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="efba3-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="efba3-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="efba3-133">Hello keresési mezőbe, írja be a **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="efba3-133">In hello search box, type **Pingboard**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="efba3-135">A hello eredmények panelen válassza ki a **Pingboard**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="efba3-135">In hello results panel, select **Pingboard**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="efba3-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="efba3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="efba3-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pingboard.</span><span class="sxs-lookup"><span data-stu-id="efba3-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="efba3-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Pingboard tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="efba3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pingboard is tooa user in Azure AD.</span></span> <span data-ttu-id="efba3-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Pingboard közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="efba3-140">In other words, a link relationship between an Azure AD user and hello related user in Pingboard needs toobe established.</span></span>

<span data-ttu-id="efba3-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Pingboard.</span><span class="sxs-lookup"><span data-stu-id="efba3-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Pingboard.</span></span>

<span data-ttu-id="efba3-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Pingboard-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="efba3-142">tooconfigure and test Azure AD single sign-on with Pingboard, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="efba3-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="efba3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="efba3-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="efba3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="efba3-145">**[Pingboard tesztfelhasználó létrehozása](#creating-a-pingboard-test-user)**  -toohave Britta Simon Pingboard, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="efba3-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - toohave a counterpart of Britta Simon in Pingboard that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="efba3-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="efba3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="efba3-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="efba3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="efba3-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="efba3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="efba3-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Pingboard alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="efba3-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="efba3-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Pingboard, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="efba3-150">**tooconfigure Azure AD single sign-on with Pingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="efba3-151">Hello Azure felügyeleti portálon, a hello **Pingboard** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="efba3-151">In hello Azure Management portal, on hello **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="efba3-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="efba3-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="efba3-155">A hello **Pingboard tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="efba3-155">On hello **Pingboard Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="efba3-157">a.</span><span class="sxs-lookup"><span data-stu-id="efba3-157">a.</span></span> <span data-ttu-id="efba3-158">A hello **azonosító** szövegmezőhöz hello érték típusa:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="efba3-158">In hello **Identifier** textbox, type hello value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="efba3-159">b.</span><span class="sxs-lookup"><span data-stu-id="efba3-159">b.</span></span> <span data-ttu-id="efba3-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="efba3-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="efba3-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="efba3-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="efba3-162">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="efba3-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="efba3-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="efba3-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="efba3-164">Ügyfél [Pingboard ügyfél-támogatási csoport](https://support.pingboard.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="efba3-164">Contact [Pingboard Client support team](https://support.pingboard.com/) tooget these values.</span></span> 

4. <span data-ttu-id="efba3-165">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="efba3-165">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="efba3-167">a.</span><span class="sxs-lookup"><span data-stu-id="efba3-167">a.</span></span> <span data-ttu-id="efba3-168">A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="efba3-168">In hello **Sign-on URL** textbox, type hello value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="efba3-169">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="efba3-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="efba3-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="efba3-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="efba3-173">egyszeri bejelentkezés tooconfigure Pingboard oldalán, nyisson meg egy új böngészőablakot, és jelentkezzen be tooyour Pingboard fiók.</span><span class="sxs-lookup"><span data-stu-id="efba3-173">tooconfigure SSO on Pingboard side, open a new browser window and log in tooyour Pingboard Account.</span></span> <span data-ttu-id="efba3-174">A egy Pingboard admin tooset egyszeri bejelentkezési be kell lennie.</span><span class="sxs-lookup"><span data-stu-id="efba3-174">You must be a Pingboard admin tooset up single sign on.</span></span>

8. <span data-ttu-id="efba3-175">Hello felső menüben válassza ki **alkalmazások > integrációja**</span><span class="sxs-lookup"><span data-stu-id="efba3-175">From hello top menu select **Apps > Integrations**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="efba3-177">A hello **integrációja** lapján található hello **"Azure Active Directory"** csempére, majd kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="efba3-177">On hello **Integrations** page, find hello **"Azure Active Directory"** tile, and click it.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="efba3-179">A hello modális, kattintson a következő **"Beállítása"**</span><span class="sxs-lookup"><span data-stu-id="efba3-179">In hello modal that follows click **"Configure"**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="efba3-181">A következő lap hello megfigyelheti, hogy "Azure SSO-integráció engedélyezve van.".</span><span class="sxs-lookup"><span data-stu-id="efba3-181">On hello following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="efba3-182">Nyissa meg hello metaadatok XML-fájlt a Jegyzettömbben, és a Beillesztés hello a letöltött **IDP metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="efba3-182">Open hello downloaded Metadata XML file in a notepad and paste hello content in **IDP Metadata**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="efba3-184">hello fájl érvényesíti, és minden rendben, ha az egyszeri bejelentkezés ezután lesz engedélyezve</span><span class="sxs-lookup"><span data-stu-id="efba3-184">hello file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="efba3-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="efba3-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="efba3-186">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="efba3-186">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="efba3-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="efba3-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="efba3-189">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="efba3-189">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="efba3-191">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="efba3-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="efba3-193">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="efba3-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="efba3-195">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="efba3-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="efba3-197">a.</span><span class="sxs-lookup"><span data-stu-id="efba3-197">a.</span></span> <span data-ttu-id="efba3-198">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="efba3-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="efba3-199">b.</span><span class="sxs-lookup"><span data-stu-id="efba3-199">b.</span></span> <span data-ttu-id="efba3-200">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="efba3-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="efba3-201">c.</span><span class="sxs-lookup"><span data-stu-id="efba3-201">c.</span></span> <span data-ttu-id="efba3-202">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="efba3-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="efba3-203">d.</span><span class="sxs-lookup"><span data-stu-id="efba3-203">d.</span></span> <span data-ttu-id="efba3-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="efba3-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="efba3-205">Pingboard tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="efba3-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="efba3-206">A sorrend tooenable az Azure AD felhasználók toolog Pingboard be azok ki kell építenie Pingboard be.</span><span class="sxs-lookup"><span data-stu-id="efba3-206">In order tooenable Azure AD users toolog into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="efba3-207">Pingboard hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="efba3-207">In hello case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="efba3-208">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="efba3-208">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="efba3-209">Jelentkezzen be tooyour Pingboard vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="efba3-209">Log in tooyour Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="efba3-210">Kattintson a **"Alkalmazott hozzáadása"** gombra **Directory** lap.</span><span class="sxs-lookup"><span data-stu-id="efba3-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="efba3-212">A hello **"Alkalmazott hozzáadása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="efba3-212">On hello **“Add Employee”** dialog page, perform hello following steps.</span></span>

    ![Felkérése](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="efba3-214">a.</span><span class="sxs-lookup"><span data-stu-id="efba3-214">a.</span></span> <span data-ttu-id="efba3-215">A hello **teljes nevét** szövegmezőhöz hello teljes nevét Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="efba3-215">In hello **Full Name** textbox, type hello full name of Britta Simon.</span></span>

    <span data-ttu-id="efba3-216">b.</span><span class="sxs-lookup"><span data-stu-id="efba3-216">b.</span></span> <span data-ttu-id="efba3-217">A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="efba3-217">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="efba3-218">c.</span><span class="sxs-lookup"><span data-stu-id="efba3-218">c.</span></span> <span data-ttu-id="efba3-219">A hello **beosztás** szövegmezőhöz típus hello beosztással Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="efba3-219">In hello **Job Title** textbox, type hello job title of Britta Simon.</span></span>

    <span data-ttu-id="efba3-220">d.</span><span class="sxs-lookup"><span data-stu-id="efba3-220">d.</span></span> <span data-ttu-id="efba3-221">A hello **hely** legördülő menüben válassza hello Britta Simon helyét.</span><span class="sxs-lookup"><span data-stu-id="efba3-221">In hello **Location** dropdown, select hello location  of Britta Simon.</span></span>
    
    <span data-ttu-id="efba3-222">e.</span><span class="sxs-lookup"><span data-stu-id="efba3-222">e.</span></span> <span data-ttu-id="efba3-223">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="efba3-223">Click **Add**.</span></span>   

4. <span data-ttu-id="efba3-224">Egy visszaigazoló képernyő tooconfirm hello hozzáadása felhasználói fog indulni.</span><span class="sxs-lookup"><span data-stu-id="efba3-224">A confirmation screen will come up tooconfirm hello addition of user.</span></span>
    
    ![Erősítse meg](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="efba3-226">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="efba3-226">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="efba3-227">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="efba3-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="efba3-228">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooPingboard megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="efba3-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPingboard.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="efba3-230">**tooassign Britta Simon tooPingboard, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="efba3-230">**tooassign Britta Simon tooPingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="efba3-231">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="efba3-231">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="efba3-233">Hello alkalmazások listában válassza ki a **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="efba3-233">In hello applications list, select **Pingboard**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="efba3-235">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="efba3-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="efba3-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="efba3-237">Click **Add** button.</span></span> <span data-ttu-id="efba3-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="efba3-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="efba3-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="efba3-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="efba3-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="efba3-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="efba3-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="efba3-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="efba3-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="efba3-243">Testing single sign-on</span></span>

<span data-ttu-id="efba3-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="efba3-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="efba3-245">Ha a hozzáférési Panel hello hello Pingboard csempe gombra kattint, automatikusan bejelentkezett tooyour Pingboard alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="efba3-245">When you click hello Pingboard tile in hello Access Panel, you should get automatically signed-on tooyour Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efba3-246">További források</span><span class="sxs-lookup"><span data-stu-id="efba3-246">Additional resources</span></span>

* [<span data-ttu-id="efba3-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="efba3-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="efba3-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="efba3-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
